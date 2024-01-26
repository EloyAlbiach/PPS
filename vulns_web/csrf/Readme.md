[Volver al inicio](../Readme.md)
# CSRF (Cross Site Request Forgery)
## Introducción.
Tal como indica el nombre de la vulnerabilidad, se trata de una falsificación de peticiones entre dos sitios web. Esto significa, que un atacante podría inducir a la víctima a realizar acciones que en principio no desearía hacer, como por ejemplo, modificar la contraseña o cambiar el correo electrónico.
Para que esta vulnerabilidad funcione, el atacante debe engañar a la víctima mediante algún tipo de phishing.
## ¿Qué condiciones se deben cumplir para que el ataque tenga éxito?
- Evidentemente, el sitio debe ser vulnerable a CSRF.
- Se debe realizar una acción relevante, tal como cambiar la contraseña o modificar el correo electrónico.
- Que el sitio web gestione las sesiones en base a cookies y que la víctima esté logueada.
- Que en el proceso de realizar una acción relevante, como el cambio de contraseña, **NO se exija** algún dato impredecible, como por ejemplo la contraseña actual o algún tipo de token aleatorio que envie el sitio web al usuario y que no pueda ser adivinado de antemano por el atacante.
## ¿Cómo funciona el ataque?

## Bibliografía.
- PortSwigger. (s.f.). Cross-site request forgery (CSRF). https://portswigger.net/web-security/csrf
- Snyk Learn. (s.f.). Cross site request forgery (CSRF). https://learn.snyk.io/lesson/csrf-attack/
- KirstenS. (s.f.). Cross Site Request Forgery (CSRF). https://owasp.org/www-community/attacks/csrf
- Moradov, O. (27/04/2022). 3 Simple CSRF Examples: Understand CSRF Once and For All. Bright. https://brightsec.com/blog/csrf-example/
- Imperva. (s.f.). Cross site request forgery (CSRF) attack. https://www.imperva.com/learn/application-security/csrf-cross-site-request-forgery/
- Ranjan, R. (27/08/2022). Cross site request forgery (CSRF) attack. Medium. https://medium.com/@rajeevranjancom/cross-site-request-forgery-csrf-attack-6949edb9e405
- Andrzej Nidecki, T. (s.f.). Cross-site request forgery (CSRF). Invicti. https://www.invicti.com/learn/cross-site-request-forgery-csrf/
