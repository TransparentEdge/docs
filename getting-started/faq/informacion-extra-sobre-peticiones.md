# Cabeceras predefinidas

Todas las peticiones que pasan a través de Transparent Edge automáticamente suman una serie de cabeceras que creemos que pueden ser útiles al origen. Son las siguientes:

### True-Client-IP

Por el funcionamiento de las CDN como _proxy_ inverso, en tus orígenes (y, por tanto, en tus _logs_), si no cambias nada, dejarás de ver la IP de tus usuarios para ver solo algunas IP, que son las de los servidores de Transparent Edge. Puesto que los usuarios establecen conexión contra nuestros servidores, somos nosotros los que hacemos la petición a los tuyos en caso de que la petición resulte un _MISS_. Para que tengas trazabilidad del usuario final, incluimos esta cabecera, donde (a diferencia de _X-Forwarded-For_), enviamos solo la IP del usuario final, sin _proxies_ intermedios.

### Geo\_Country\_Code y Geo\_Region\_Code

Para cada petición, geolocalizamos las direcciones IP de los usuarios mediante el uso de una base de datos específica de geolocalización. A pesar de que mediante VCL se pueden recabar más datos, tomamos los más habituales y útiles y los incluimos en sendas cabeceras, disponibles tanto en VCL como en los orígenes del cliente, con los datos del código ISO del país y la región geográfica donde se localiza la IP.

A pesar de actualizamos las bases de datos semanalmente, la precisión a nivel de región está limitada por temas como el CGNAT de los proveedores, si bien el código de país tiene una alta fiabilidad.

### X-Device

Sabemos que gestionar agentes de usuario puede ser una pesadilla, por lo que, a partir del header `User-Agent` de la petición original, hacemos una normalización de este para establecer una cabecera extra que puede tomar tres valores: `mobile`, `tablet` y `desktop`.

### X-Forwarded-Proto

A pesar de la generalización cada vez más global de los certificados HTTPS, todavía hay servicios web que utilizan el protocolo HTTP para comunicarse en plano. En la cabecera _X-Forwarded-Proto_, reflejada también en los _logs_, tendrás los valores "http" y "https" indicando si la petición original se realizó en plano o cifrada.

&#x20;``&#x20;

