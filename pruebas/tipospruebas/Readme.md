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
Se trata de un tipo de prueba adecuado para los algoritmos más sencillos, nos ayudaremos de grafos para determinar los posibles caminos que se deben evaluar. ESte tipo de prueba nos permitirá calcular la **COMPLEJIDAD CICLOMÁTICA**.

Según el tipo de estructura de código, le corresponderá un tipo de grafo, se enumeran a continuación:

- #### Secuencia


