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

## 1.5.3 Cobertura de las pruebas

## 1.5.4 Pruebas de caja negra







