# Query String

La cadena de consulta (_query string_) es la parte de una [URL](https://es.wikipedia.org/wiki/URL) que contiene los datos que deben pasarse a la aplicación web, por lo general incluye campos añadidos a la base de la URL por la aplicación de cliente o, por ejemplo, como parte de un formulario HTML.

`www.ejemplo.net/web.php?campo1=valor1&campo2=valor2`

El ejemplo anterior hace referencia a un sitio web dinámico. En este caso, el servidor crea automáticamente la página de respuesta cuando el navegador la solicita. Para ello se vale de los datos incluidos en la URL.

Existen otros métodos para generar páginas dinámicas, por ejemplo, separando las _query strings_ mediante el caracter `/` . Gracias a la configuración del servidor podemos generar URL más amigables como la siguiente:

`www.ejemplo.net/principal/blog/132`

En este caso, el nombre de la variable viene predefinido en el servidor según la posición que tenga en la URL y el valor es directamente su contenido (principal, blog, 132).
