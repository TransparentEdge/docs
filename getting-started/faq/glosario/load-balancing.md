# Load Balancing

Load balancing is a way to distribute network traffic across multiple servers that uses balancing methods to improve the overall response time and availability of applications. It ensures equal distribution so that no one server has to handle too many requests.

There are several balancing methods or algorithms:

* Least connections: Directs the traffic to the server with the fewest active connections.
* Least response time: Directs the traffic to the server with the fewest active connections and the lowest average response time.
* Round robin: The traffic rotates through the available servers, which can be useful when the servers all have equal value and don’t require persistent connections.
* IP hash: The client’s IP address determines which server receives its request.
