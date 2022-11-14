# Last Modified

El header HTTP de respuesta de última modificación (_last modified_) contiene información sobre la fecha y la hora en las que el servidor de origen considera que el recurso se modificó por última vez.&#x20;

Se utiliza como validador para determinar si un recurso recibido o almacenado es el mismo. Es menos preciso que un encabezado ETag, pero es una alternativa para recurrir a un recurso.

Las solicitudes condicionales que contienen encabezados If-Modified-Since o If-Unmodified-Since hacen uso de este campo.\
