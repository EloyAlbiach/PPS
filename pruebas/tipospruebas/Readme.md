[Volver al inicio](../Readme.md)
# 1.5 Tipos de pruebas
Una clasificación de los tipos de pruebas podría ser la siguiente:

- Pruebas **unitarias**: Se realizan pruebas de cada parte del código por separado, por ejemplo de un módulo, una función, etc.
- Pruebas de **integración**: Una vez probadas las partes de código de forma independiente, necesitamos comprobar que al relacionarse entre ellas, el funcionamiento es el correcto. Por ejemplo, que los valores de salida de un módulo, son los adecuados para la entrada de otro.
- Pruebas de **validación**: Previo al lanzamiento en público de la aplicación, se necesita evaluar que el producto, en su conjunto, funciona adecuadamente y en base  a las especificaciones.
- Pruebas del **sistema**: Se comprueba el nivel de seguridad, se realizan pruebas de rendimiento y de resistencia frente a situaciones anormales.
- Pruebas de **aceptación**: En este punto, es el/la usuario/a quien realiza las pruebas para comprobar que se adapta a sus necesidades y que cumple las especificaciones dadas durante la contratación del servicio.

Una de las tendencias actuales en el desarrollo de Software, es el desarrollo en **espiral**, en el que se van intercalando las distintas pruebas según se va avanzando en el desarrollo completo de la aplicación, trabajando con **Prototipos** no completamente funcionales, para que el cliente pueda ver la evolución sin riesgo de sorpresa desagradable, una vez se ha desarrollado por completo la aplicación:

![Diagrama de desarrollo en espiral](_images/359px-ModeloEspiral.svg.png)

(Imagen obtenida de: https://es.wikipedia.org/wiki/Desarrollo_en_espiral)

## ¿Por qué realizar pruebas?
Una de las principales razones para realizar pruebas a un producto Software, es la de asegurar que dicho producto cumple las especificaciones para las que fue creado y que, durante su utilización, se mantiene en un estado _**robusto**_ y sin errores.

Otra razón no menos importante y relacionada con la anterior, es la de asegurar que dicho software no nos lleva a un estado que pueda ser aprovechado por un ciberdelincuente para realizar algún tipo de ataque. Hablamos desde una denegación de servicio, conocido como DoS (**D**enial **o**f **S**ervice), a por ejemplo, la obtención de una Shell con permisos de administrador debido a un ataque Buffer Overflow, por poner un par de casos.

## 1.5.1 Pruebas de caja blanca
Se trata de un tipo de pruebas _**unitarias**_. Se caracteriza por tener acceso completo al código a evaluar, se debe garantizar que:
- Se pasa por todos los caminos independientes.
- Se ejecutan todas las sentencias.
- Se toman todas las decisiones lógicas (V/F).
- Se prueban los bucles en todos sus límites.

Existen herramientas que nos permitirán conocer qué porcentaje de cobertura tienen nuestras pruebas. El éxito o fracaso depende mucho del _banco de pruebas_ utilizado para probar las aplicaciones, no solo el porcentaje de cobertura es importante, también los valores de prueba.

### Prueba del camino básico. Representación de grafos.
Se trata de un tipo de prueba adecuado para los algoritmos más sencillos, nos ayudaremos de grafos para determinar los posibles caminos que se deben evaluar. Este tipo de prueba nos permitirá calcular la **COMPLEJIDAD CICLOMÁTICA**.

Según el tipo de estructura de código, le corresponderá un tipo de grafo, se enumeran a continuación las más básicas:

- #### Secuencia
Indica la ejecución de forma ordenada de las acciones.

![Grafode secuencia](_images/secuencia.jpg)

- #### Decisión (IF-ELSE)
Si se cumple una condición, se realizan una o más acciones, en caso contrario, se realizan otras condiciones (o ninguna).

![Grafo If-Else](_images/IF.jpg)

- #### Bucle While
Se evalua primero la condición y mientras se cumpla, se realizan las acciones indicadas, una vez deja de cumplirse la condición, se sale del bucle y continua el flujo del programa

![Grafo While](_images/while.jpg)
- #### Bucle Do-While
Realiza un bucle que se repite mientras se cumpla la condición, con una característica, siempre se ejecuta al menos una acción aunque la condición no se cumpla.

![Grafo Do-While](_images/do-while.jpg)

> NOTA: En el diagrama de grafos, a los nodos que contienen una decisión se les llama **nodos predicado** y a las líneas que unen los nodos, **aristas**.

### Prueba del camino básico
Dado un algoritmo, deberemos preparar las pruebas de forma que se ejecuten todos los caminos posibles, o **caminos básicos**. En función de la cantidad de caminos posibles, el proceso de prueba será más o menos complejo. Para determinar la complejidad del algoritmo y determinar sus caminos básicos, utilizaremos la _**COMPLEJIDAD CICLOMÁTICA**_.

La complejidad ciclomática (CC) se calcula de diferentes formas:

```
CC = Número_de_aristas - número_de_nodos + 2

CC = Número_nodos_predicado + 1

CC = Número_de_zonas
```

En función de la complejidad ciclomática, determinaremos la evaluación del riesgo del algoritmo

Complejidad Ciclomática|Evaluación del riesgo del algoritmo
--|--
1 - 10|Algoritmo simple. Poco riesgo
11 - 20|Algoritmo complejidad media. Riesgo moderado
21 - 50|Algoritmo complejo. Alto riesgo
\> 50|Algoritmo no testeable. Muy alto riesgo

Para entender mejor este cálculo, se muestra un ejemplo:

Sumar los N números naturales. Dado un número natural como entrada (0, 1, 2, ....), realizar un algoritmo que sume dicho número y los naturales inferiores hasta 0. Por ejemplo, si nos pasan como entrada el número 4, tendremos **4 + 3 + 2 + 1 + 0 = 10**.

El diagrama de flujo podría ser parecido a este:

![Grafo para sumar n naturales](_images/suma_naturales.jpg)

Y la traza del programa, para un valor de 4 sería:

n | suma | contador
--|--|--
4|0|1
 "|1|2
 "|3|3
 "|6|4
 "|10|5


Realizamos el cálculo:
- Número de nodos = 6
- número de aristas = 6
- Número de nodos predicado = 1

![grafo suma n naturales numerado](_images/sumar_n_naturales_nodos_numerados.jpg)

Así pues, las fórmulas anteriores con los valores serían:

- CC = Número_de_aristas - número_de_nodos + 2 = 6 - 6 + 2 = **2**
- CC = Número_nodos_predicado + 1 = 1 + 1 = **2**
- CC = Número_de_zonas = **2**

Nos falta ver el número de zonas para ver si se cumplen todas las fórmulas...

![grafo suma n naturales zonas](_images/suma_n_naturales_zonas.jpg)

Los caminos básicos son:

- Camino 1: **1-2-3-5-6**
- Camino 2: **1-2-3-4-3-5-6**

## 1.5.2 Ejemplo de pruebas con Python y "UnitTest"

Una vez diseñado el algoritmo, debemos codificarlo y realizar pruebas de funcionamiento. En Python existen diferentes opciones, de las cuales, las más conocidas son **PyTest** y **UnitTest**.
En nuestro caso, vamos a realizar las pruebas con **UnitTest**, para ello, crearemos una clase de prueba en un fichero de pruebas, independiente del programa a probar y llamaremos a la librería mediante un "import".

> import unittest

El fichero de pruebas deberá disponer de una clase que heredará de **unittest.TestCase**. Para realizar las pruebas, utilizaremos el fichero a probar, realizando llamadas a este desde la clase de pruebas y utilizando algunas de las siguientes sentencias:

- .assertEqual(x, y): Verifica que x e y son iguales.
- .assertTrue(x): Verifica que el valor de x es True.
- .assertFalse(x): Verifica que el valor de x es False.
- .assertRaises(x): Verifica que se genera una excepción.

> Existen otras sentencias de prueba a utilizar como .assertIs(), .assertIn(), etc. (consultar la bibliografía de este capítulo).

Para entender el funcionamiento, mejor un ejemplo, para ello, vamos a realizar un algoritmo que, dados 3 números enteros, determina cual de los 3 es el **mayor**.

El diagrama de flujo (sin optimizar) podría ser este:

![Diagrama de flujo mayor de tres](_images/major_3.jpg)

En el que se observan 3 nodos **predicado** y por lo tanto, una complejidad ciclomática de 4 (3 nodos predicado + 1).

Un código en Python que refleje el diagrama de flujo sería (mayor_de_tres.py):

```Python
def devuelve_mayor(a, b, c):
    if a > b:
        if a > c:
            mayor = a
        else:
            mayor = c
    else:
        if b > c:
            mayor = b
        else:
            mayor = c
    return mayor 
```

El código para ejecutar las pruebas sería:

```Python
import unittest
from mayor_de_tres import devuelve_mayor

class TestMayorDeTres(unittest.TestCase):
    def test_mayor(self):
        self.assertEqual(devuelve_mayor(12, -3, 9), 12)

    def test_mayor2(self):
        self.assertEqual(devuelve_mayor(-12, -3, 0), 0)

    def test_mayor3(self):
        self.assertEqual(devuelve_mayor(-12, -3, -1), -1)

    def test_mayor4(self):
        self.assertIsNot(devuelve_mayor(-12, -3, -1), -12)

    def test_mayor5(self):
        self.assertEqual(devuelve_mayor(8, 15, 1), 15)

if __name__ == '__main__':
    unittest.main()
```

Si ejecutamos las pruebas en Visual Studio Code, obtenemos el siguiente resultado:

![Test mayor de tres, resultado](_images/result_test_mayor_3.png)

Imaginemos que forzamos un error en el test número 5 y en lugar de decir que esperamos un resultado de 15, decimos que esperamos un 8. Nos indicará que el resultado que esperamos (8), no coincide con el valor que devuelve la función para los datos que se pasan (15):

![Fallo de test](_images/result_test_mayor_3_fail.png)

Podemos utilizar **.assertFalse** para verificar que una comparación no va a ser cierta, o visto desde otro punto de vista, que la siguiente afirmación es falsa:

![Uso de assertFalse](_images/test_assertFalse.png)

### 1.5.2.1 Test de Excepciones

En el apartado [1.4.1.10](../codfuente/Readme.md) se introdujeron las "**Excepciones**" como una forma de controlar diferentes errores durante la ejecución de un programa. Las excepciones en este caso eran generadas por el sistema operativo en caso de dividir por cero. No obstante, el usuario puede definir sus propias excepiones tal como se muestra a continuación, utilizando "**raise**" e indicando el tipo de excepción:

![Lanzando una excepción generada por el usuario](_images/divisor_exception.png)

Si se quiere comprobar mediante tests que realmente se genera la excepción, podemos utilizar "**assertRaises**" tal como se muestra en la siguiente imagen, hay que prestar atención a la disposición de la función y los parámetros de entrada, ya que no sigue el patrón habitual. El primer parámetro de assertRaises, es el error que quiero comprobar que se genera (en nuestro caso "ArithmeticError"), seguido del nombre de la función a probar y por último, los dos parámetros de entrada en el orden en el que están declarados en la función original:

![Testeando la generación de la excepción](_images/divisor_test_exception.png)

### 1.5.2.2 Test parametrizado

Cuando queremos probar muchos valores, escribir muchas líneas repetidas de código puede hacerse pesado, por ello, existe la posibilidad de utilizar los tests **PARAMETRIZADOS**, que permiten determinar previamente el banco de pruebas. Deberemos instalar primero la librería **parameterized**:

> pip install parameterized

![Imagen de la instalación de parameterized](_images/instalacion_parameterized.png)

Se puede observar un ejemplo en la siguiente imagen:

![Ejemplo de test parametrizado](_images/ejemplo_test_parametrizado.png)

## 1.5.3 Cobertura de las pruebas

Aunque gracias a nuestra intuición y buen hacer, podemos crear un buen banco de pruebas, es posible que alguna de las ramas del algoritmo quede sin probar, con el correspondiente peligro que esto supone. Una forma de medir la **cobertura** de nuestro banco de pruebas sobre el código, es utilizando herramientas externas que nos muestren esto de forma gráfica. Para ello vamos a utilizar **COVERAGE**.

Para trabajar de forma independiente y poder instalar librerías sin que afecte al sistema anfitrión, trabajaremos sobre entornos virtuales Python. El primer paso será instalar pipx:

![Instalación de pipx](_images/paso1.png)

Una vez pipx instalado, lo utilizaremos para instalar el entorno virtual de Python "**virtualenv**":

![Instalación de virtualenv](_images/paso2.png)

Ahora lo correcto, sería crear una carpeta para el proyecto en el que vamos a trabajar, y crear también el entorno virtual de ese proyecto (normalmente, por convención se llama venv al entorno virtual, aunque puede tener otro nombre, en este caso, se crea como carpeta oculta **.venv**):

![Creación del proyecto](_images/paso3.png)

![Vista del proyecto creado](_images/paso4.png)

Dentro del proyecto de trabajo se crea el entorno virtual .venv utilizando la palabra clave "**venv**":

![Creación del entorno virtual](_images/paso5.png)

Si observamos, dentro de la carpeta .venv se han creado diferentes ficheros y subcarpetas que contienen el entorno virtual de Python y el que se podrán añadir las librerías que se necesiten, sin afectar al sistema anfitrión:

![Vista del entorno virtual creado](_images/paso6.png)

El entorno virtual se debe activar para que entre en funcionamiento, aparece el prefijo (.venv) indicando que se ha activado:

![Activación del entorno virtual](_images/paso7.png)

Y se desactiva con "**deactivate**":

![Desactivación del entorno virtual](_images/paso8.png)

No obstante, también puede ser activado directamente desde Visual Studio Code (botón derecho del ratón sobre la zona marcada en rojo):

![Activar entorno virtual desde Visual Studio Code](_images/paso9.png)

![Seleccionar el entorno virtual](_images/paso10.png)

Una vez activado el entorno virtual, instalaremos la libreria coverage dentro de este haciendo:

> pip install coverage

Una vez instalado, lo llamaremos para que realice las pruebas mediante:

![pruebas con coverage, ejecución de la prueba](_images/coverage_A.png)

Una vez realizadas las pruebas mediante Coverage, crearemos un fichero XML que podrá ser utilizado, por ejemplo en Visual Studio Code y otro HTML (opcional), si queremos que se nos muestre en una página web como documentación.

![creación XML y HTML de coverage](_images/coverage_B.png)

Los resultados con los porcentajes se muestran a continuación (HTML):

![coverage general](_images/coverage_C.png)

Como se puede observar, la línea 6 nunca se ejecuta con el banco de pruebas que hemos elegido:

![coverage missing](_images/coverage_D.png)

Otra forma alternativa al formato HTML, sería utilizando un plugin para Visual Studio Code, este plugin se apoya en el XML que generamos con la orden **coverage xml** vista anteriormente:

![Visual Studio Code Coverage plugin](_images/coverage_F.png)

Que nos ofrece una forma alternativa (revisa el pie de pantalla para ver el porcentaje de cobertura):

![Cobertura en VSC](_images/coverage_E.png)

> NOTA: Cada vez que se modifica el código, se debe ejecutar de nuevo las sentencias:
> - coverage run fichero.py
> - coverage html
> - coverage xml

## 1.5.4 Pruebas de caja negra
Así como en la prueba de caja blanca teníamos acceso al código a testear, en la prueba de caja negra NO disponemos de los detalles de implementación del código, solo sabemos la "**funcionalidad**" del algoritmo, los parámetros de entrada y el resultado que debería obtener.

![Diagrama pruebas de caja negra](_images/capsa_negra.jpg)

El objetivo de este tipo de pruebas es comprobar la funcionalidad del algoritmo, basándonos en los parámetros de entrada, verificando que el resultado es el adecuado. Para ello, las pruebas de caja negra se apoyan en la técnica de la "**Partición equivalente**" y se refuerzan mediante la técnica de prueba de los "**Valores límite**".

### 1.5.4.1 Partición equivalente
La técnica de la partición equivalente consiste en determinar, para cada dominio de entrada distinto, un grupo representativo de valores de prueba, también llamados "**Clases de equivalencia**". Se obtendrán valores de prueba tanto para entradas válidas, como para entradas inválidas. Evidentemente, no se van a poder probar todos los posibles valores de entrada, or lo que se asume el riesgo de utilizar solo un rango representativo de cada dominio de entrada.

Para determinar las clases de equivalencia, veremos un ejemplo de diferentes dominios de entrada:

Dominio de entrada|Número de clases válidas|Número de clases inválidas
--|--|--
Rango de valores [10..20]|1: Valor dentro del rango (ej. 15)|2: Un valor por debajo y otro por encima del rango (ej. 7 y 22)
Conjunto finito de valores {1, 3, 5, 7, 9, 11}|1: Valor dentro del conjunto (ej. 3)|2: Valores por debajo y por encima (ej. 0 y 13)
Condiciones booleanas (T/F) "Valor mayor que 0"|1: Valor evaluado a True (ej. 12)|1: Valor evaluado a False (ej. -5)
Doble rango 5 < valor < 10 y 15 < valor < 20|2: Valor rango 1 y valor rango 2 (ej. 7 y 18)|4: un valor por encima y por debajo de cada rango (ej. 3, 12, 13 y 22) aunque en este caso con 3 valores sería suficiente (3, 13, y 22).

### 1.5.4.2 Análisis de valores límite
Se especializa en probar los límites de los rangos, las estructuras y los bucles, para intentar encontrar errores. Para ello, dentro de los valores de las clases de equivalencia, escoge los valores que están justo en los límites de cada rango, pues se ha demostrado que **es aquí donde se suelen concentrar los errores**.

Dominio de entrada|Número de clases válidas|Número de clases inválidas
--|--|--
Rango de valores [10..20]|4: Valores dentro del rango (ej. 10, 11, 19 y 20)|2: Valores por debajo y por encima del rango (ej. 9 y 21)
Conjunto finito de valores {1, 3, 5, 7, 9, 11}|4: Valores **mínimo** y **máximo** dentro del conjunto y adyacentes (ej. 1, 3, 9 y 11)|2: Valores justo por debajo y por encima (ej. 0 y 12)