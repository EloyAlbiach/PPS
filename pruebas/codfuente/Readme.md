[Volver al inicio](../Readme.md)
# 1.3 Código fuente y entornos de desarrollo
## 1.3.1 Código fuente
Entendemos como código fuente al conjunto de líneas de instrucciones que, una vez compilado, será ejecutado por el procesador. Para hablar del código fuente, nos centraremos en el lenguaje **PYTHON**.
### 1.3.1.1 Comentarios
Los comentarios permite añadir información al código para que pueda ser más entendible. EStos comentarios serán ignorados por el procesador.
La forma de escribir un comentario en Python es:

```Python
#Esto es un comentario
```

### 1.3.1.2 Imprimir texto
Nos permite mostrar texto al usuario del programa para interactuar con él:

```Python
print ("Este texto será mostrado al usuario...")
```

### 1.3.1.3 Variables
Las variables permiten guardar o inicializar valores/datos durante la ejecución. En Python se construyen mediante caracteres alfanuméricos y/o la barra baja "_".

Son sensibles a las mayúsculas y minúsculas, por lo que no es lo mismo "valor" que "Valor" o "vAlor", se trataría de variables distintas.

```Python
x = 5
nombre = "Ana"

print (x) # Mostrará 5 por pantalla
print (nombre) # Mostrará Ana por pantalla
```
El 'tipo' de las variables se toma en función de su contenido, por lo que dichas variables no cesitan ser tipadas previamente:

```Python
x = 3  # x será una variable de tipo entero 
y = "3" # y será una variable de tipo string (str)
  ```
Esto implica que ciertas operaciones no se puedan realizar, veamos un ejemplo:

![Error en los tipos de las variables](_images/error_tipos.png)

La solución pasa por realizar un **CAST** sobre la variable de tipo String, de forma que la convertimos al tipo de datos que nos interesa, en este caso un entero:

![CAST a entero de una cadena](_images/cast_tipos.png)

También permite realizar código compacto, por ejemplo asignaciones múltiples en una sola línea:

``` Python
a, b, c = 1, 3, 25
# a vale 1, b vale 3 y c vale 25
```
Las variables de texto pueden ser multilínea:

``` Python
cadena = """
A Cuesta le cuesta subir la cuesta,
y en medio de la cuesta,
va y se acuesta.
"""

otra_cadena = '''
Cuando yo digo Diego, digo digo,
y cuando digo digo, digo Diego.
'''
```

Se pueden concatenar cadenas usando el operador suma (+):

![Ejemplo de concatenación de cadenas](_images/concatenar_cadenas.png)

## 1.3.2 Entornos de desarrollo




