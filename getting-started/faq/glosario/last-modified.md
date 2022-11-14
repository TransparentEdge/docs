# Last-Modified

El _header_ _Last-Modified_ es una cabecera HTTP de respuesta de última modificación que contiene información sobre la fecha y la hora en las que el servidor de origen considera que el recurso se modificó por última vez.

Se utiliza como validador para determinar si un recurso recibido o almacenado es el mismo. Es menos preciso que una cabecera _ETag_, pero es una alternativa para recurrir a un recurso.

Las solicitudes condicionales que contienen cabeceras _If-Modified-Since_ o _If-Unmodified-Since_ hacen uso de este campo.\
