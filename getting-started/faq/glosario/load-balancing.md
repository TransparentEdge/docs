# Load balancing

El balanceo de carga o _load balancing_ es una técnica de distribución del tráfico de red entre varios servidores que utiliza métodos de balanceo para mejorar la capacidad de respuesta general de las aplicaciones y su disponibilidad. Asegura una distribución equitativa para que ningún servidor atienda demasiadas peticiones.

Existen varios métodos o algoritmos de balanceo:

* _Least connections_: Dirige el tráfico al servidor con menos conexiones activas.&#x20;
* _Least response time_: Dirige el tráfico al servidor con menos conexiones activas y menor tiempo de respuesta.&#x20;
* _Round robin_: Va rotando el tráfico entre los servidores disponibles, algo útil cuando todos tienen unas especificaciones similares y no se requieren conexiones persistentes.&#x20;
* _IP hash_: La dirección IP del cliente determina a qué servidor irá la petición.

