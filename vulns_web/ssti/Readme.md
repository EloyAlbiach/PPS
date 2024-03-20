[Volver al inicio](../Readme.md)
# SSTI (Server Side Template Injection)
## Introducción.
La vulnerabilidad SSTI se produce cuando un atacante tiene la posibilidad de incluir código malicioso sobre una de las plantillas (templates) que se ejecutan en el servidor web. Las plantillas funcionan combinando partes fijas que dan forma a la página web, junto con datos volátiles que son generados mediante la interacción con el usuario.

Imagina una plantilla Jinja en la que aparece el siguiente código para obtener el nombre del usuario y mostrarlo en la web:

```css
mostrar = template.render(name=request.args.get('nombre'))
```

Como se puede observar, el parámetro 'nombre' se pasa directamente a la plantilla para que sea utilizado sin ningún filtrado, con lo que, si se pasa un código malicioso en este parámetro, pasará a ser ejecutado por el servidor.

```
http://misitiovulnerable.com/?nombre={{payload}}
```

## Detección
Para la detección de la vulnerabilidad, se suele inyectar una secuencia de caracteres especiales que son utilizados por las plantillas, como por ejemplo ``` ${{<%[%'"}}%\ ```. Si al introducir una de estas secuencias se obtiene un error, significaría que la inyección de plantillas puede ser explotada.

Si por ejemplo se accede al sitio web con la siguiente secuencia:
```
http://misitiovulnerable.com/?nombre=${{5*4}}
```

Y se obtiene como resultado algo como "Bienvenido **20**" en la página de presentación, significaría que la plantilla ha ejecutado la multiplicación y por lo tanto es susceptible de ataques SSTI.

## Mitigación de los ataques
- Restringir el acceso en modo edición a las plantillas.
- Sanitizar las entradas.
- Utilización de plantillas "Logic-less" que limitan el uso de la lógica para evitar que interpreten los payloads.


## Bibliografía.
- Hacktricks. (s.f.).SSTI (Server Side Template Injection). https://book.hacktricks.xyz/v/es/pentesting-web/ssti-server-side-template-injection
- PortSwigger. (s.f.). Server-side template injection. https://portswigger.net/web-security/server-side-template-injection
- imperva. (s.f.). SSTI (Server-Side Template Injection). https://www.imperva.com/learn/application-security/server-side-template-injection-ssti/