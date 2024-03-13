[Volver al inicio](../Readme.md)
# SQLi (SQL Injection)
## Introducción.
Se trata de una vulnerabilidad que permite a un atacante **"influir"** en las consultas SQL que realiza una aplicación sobre la base de datos, de manera que se puede aprovechar para obtener información a la que por construcción, no debería tener acceso.

Ejemplo, imagina un campo "valor" en un formulario de búsqueda de productos de una aplicación web:

```https://www.webvictima.com/productos?valor=100 ```

Desde el punto de vista de la aplicación seguramente se realiza una petición parecida a:

```SELECT * FROM Productos WHERE precio < '100' ORDER BY name; ```

Así pues, si la aplicación no está protegida, se puede intentar aplicar simplemente una comilla "**'**" en el campo valor, para comprobar si la aplicación es vulnerable a SQL Injection. La consulta quedaría como:
```SELECT * FROM Productos WHERE precio < ''' ORDER BY name; ```

Una de las primeras opciones que siempre se explica en la teoría de SQLi, es la del payload " 100' OR '1'='1 ", que aplicado sobre la consulta, quedaría:

```SELECT * FROM Productos WHERE precio < '100' OR '1'='1' ORDER BY name; ```

En este caso, estamos pidiendo, por una parte, los productos con precio inferior a 100, pero por otra, estamos indicando una afirmación, por lo que el resultado es igual a TRUE y mostraría **TODOS los productos**.

Imagina ahora esta técnica pero sobre un formulario de login con la siguiente consulta:

```SELECT * FROM Usuarios WHERE usuario='$user' AND passwd='$pass'; ```

Si aplicamos una pequeña modificación del payload anterior ( admin' OR 1=1-- - ) añadiendo comentarios a la consulta SQL, tendríamos:

```SELECT * FROM Usuarios WHERE usuario='admin' OR 1=1-- - AND passwd='$pass'; ```

Al aplicar los comentarios a la parte del password, lo que estaríamos consiguiendo sería que mostrase la tabla completa de usuarios y contraseñas, además, si no se ha tenido la precaución de encriptar las contraseñas, el problema sería todavía peor.




