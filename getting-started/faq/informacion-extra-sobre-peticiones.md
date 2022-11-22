# Cabeceras predefinidas

Todas las peticiones que pasan a través de Transparent Edge Services automáticamente suman una serie de cabeceras que creemos que pueden ser útiles al origen. Son las siguientes:

### True-Client-IP

Por el funcionamiento de las CDN como _proxy_ inverso, en tus orígenes (y, por tanto, en tus logs), si no cambias nada, dejarás de ver la IP de tus usuarios para ver solo unas pocas IP, que son las de los servidores de Transparent Edge Services. Puesto que los usuarios establecen conexión contra nuestros servidores, somos nosotros los que hacemos la petición a los tuyos en caso de que la petición resulte un MISS. Para que tengas trazabilidad del usuario final, incluimos esta cabecera, donde (a diferencia de `X-Forwarded-For` ), enviamos solo la IP del usuario final, sin _proxies_ intermedios.

### geo\_country\_code y geo\_region\_code

Para cada petición, geolocalizamos las direcciones IP de los usuarios mediante el uso de una base de datos específica de geolocalización. A pesar de que mediante VCL se pueden recabar más datos, tomamos los más habituales e útiles y los incluimos en sendas cabeceras disponibles tanto en VCL como en vuestros orígenes, con los datos del código ISO del país y la región geográfica donde se localiza la IP.&#x20;

A pesar de que mantenemos actualizadas las bases de datos lo más posible (se actualizan semanalmente), la precisión a nivel de región está limitada por temas como el CGNAT de los proveedores, si bien el código de país tiene una alta fiabilidad.

### X-Device

Sabemos que gestionar agentes de usuario puede ser una pesadilla, por lo que, a partir del header `User-Agent` de la petición original, hacemos una normalización de este para establecer una cabecera extra que puede tomar tres valores: `mobile`, `tablet` y `desktop`.

### X-Forwarded-Proto

A pesar de la generalización cada vez más global de los certificados HTTPS, todavía hay servicios web que, por el motivo que sea, utilizan el protocolo HTTP para comunicarse en plano. En la cabecera X-Forwarded-Proto, reflejada también en los logs, tendrás los valores "http" y "https" indicando si la petición original se realizó en plano o cifrada.

&#x20;``&#x20;

