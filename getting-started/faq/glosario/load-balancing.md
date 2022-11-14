# Load balancing

El balanceo de carga o _load balancing_ es la distribución del tráfico de red entre varios servidores utilizando métodos de balanceo para mejorar la capacidad de respuesta general de las aplicaciones y la disponibilidad de las mismas. Esto asegura que ningún servidor atienda demasiadas peticiones.

Existen varios métodos o algoritmos de balanceo:

* Least Connections: Dirige el tráfico al servidor con menos conexiones activas.
* Least Response Time: Dirige el tráfico al servidor con menos conexiones activas y menor tiempo de respuesta.
* Round Robin: Va rotando el tráfico entre los servidores disponibles, algo útil cuando todos tienen unas especificaciones similares y no requerimos conexiones persistentes.
* IP Hash: La dirección IP del cliente determina a qué servidor irá la petición.

