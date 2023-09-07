# IDOR (Insecure Direct Object Reference)

## Introducción.
**IDOR** o la _referencia directa a objetos de forma insegura_, es una vulnerabilidad derivada de un error en el proceso de control de acceso de una aplicación web. Es decir, un objeto que solo debería ser accedido por un usuario concreto, debido a una falta de control en el sistema de acceso, podría ser alcanzable por otro usuario sin permisos o con permisos más bajos, lo que representaría un serio problema de seguridad.
Podemos ver los objetos como recursos del sistema o de la aplicación, como por ejemplo, el acceso a uno o más elementos de la base de datos o a un fichero.
## Ejemplo.
Imaginemos una aplicación web de un hospital especializado en oncología, a la que podemos acceder, mediante un inicio de sesión previo, a nuestro historial de intervenciones realizadas, medicaciones, visionado de radiografías, resonancias, etc. Imaginemos también, que debido a un deficiente control de acceso, esta aplicación es vulnerable a IDOR. Esto significaría que un usuario cualquiera, podría acceder a los datos de todos los pacientes del hospital e incluso utilizar esta información con fines maliciosos, tales como la amenaza de divulgación, etc.

Nos conectamos a la página del hospital con nuestro usuario:

!!! Ací cal una imatge del formulari d'exemple !!!

Al fijarnos en la URL, observamos la variable "**user**" seguida de un valor, supuestamente, nuestro identificador en la base de datos. ¿Y si cambiamos el identificador?

`https://hospitalEjemplo.com/historiales/usuarios?user=3245`

Modificamos el identificador por otro al azar, esperando que esté registrado en la base de datos:

`https://hospitalEjemplo.com/historiales/usuarios?user=3244`

Cual es nuestra sorpresa al comprobar que hemos podido acceder a los datos clínicos de otro paciente.

De la misma forma que hablamos de una aplicación de hospital, podríamos hablar de una aplicación de banca, imagina que puedes observar las cuentas de otros clientes o incluso realizar transacciones... :scream:
## Medidas de seguridad.
Será necesario modificar la aplicación de forma que, solo el usuario con los permisos adecuados sea capaz de ver/actualizar/borrar el objeto al que se pretende tener acceso, para ello, la aplicación deberá comprobar, en cada momento, qué usuario está accediendo y qué permisos tiene, para determinar si puede acceder o no al recurso.

```
if(($_USER['userID'] != user)){
    header('location : error_acces_page.html')
}

#show user data
```

Como se puede adivinar en el código anterior, si el contenido de la variable "**user**" es distinto del identificador de usuario con el que se ha iniciado la sesión, la aplicación mostrará un mensaje de error, indicando que no puede acceder a datos de un usuario distinto al suyo.
## Bibliografía.
- PortSwigger. (s.f.).Insecure direct object references (IDOR). [Enlace a la web](https://portswigger.net/web-security/access-control/idor)
- Grimmick, R. (14 de octubre de 2022). What is IDOR (Insecure Direct Object Reference)?. [Enlace a la web](https://www.varonis.com/blog/what-is-idor-insecure-direct-object-reference)
- OWASP.org. (s.f.). Testing for Insecure Direct Object References. [Enlace a la web](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References)

