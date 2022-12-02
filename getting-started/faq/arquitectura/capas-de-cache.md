# Capas de caché

Transparent Edge cuenta con dos capas de caché, que nuestros clientes pueden usar de manera opcional. El objetivo es mejorar el [_hit ratio_](https://soporte.transparentcdn.com/projects/incidencias/wiki/Hit\_Ratio) y descargar de peticiones innecesarias a sus servidores de origen. Además, este tipo de arquitectura protege ante aluviones de tráfico provenientes, por ejemplo, de una invalidación recursiva del sitio web con mucho tráfico.[**​**](https://soporte.transparentcdn.com/projects/incidencias/wiki/Arquitectura\_de\_caches/edit?section=2)[![Edit](https://soporte.transparentcdn.com/images/edit.png)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Arquitectura\_de\_caches/edit?section=2)

#### Capa 1

La capa 1 está compuesta por un elevado número de servidores repartidos a lo largo del [mundo](https://www.transparentedge.eu/distribucion-de-contenidos-cdn/cdn-nueva-generacion/), que son los atienden las peticiones del usuario final en primera instancia. Actúan como terminadores SSL y servidores de caché, sirviendo cerca del 95 % de Transparent Edge.

Estos servidores de capa 1 pueden tener configurado como origen todos los servidores de capa 2 por lo que, cuando se ven en la necesidad de refrescar un objeto, envían la petición directamente a estos.[![Edit](https://soporte.transparentcdn.com/images/edit.png)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Arquitectura\_de\_caches/edit?section=3)

#### Capa 2 (Mid-Tier)

El Mid-Tier de Transparent Edge está pensado para hacer de embudo y limitar en la medida de lo posible el número de peticiones que llegan a los servidores de origen de nuestros clientes. Esta capa de servidores se encuentra repartida en varios centros de datos por motivos de alta disponibilidad y es la encargada de hablar en última instancia con los servidores web de origen.

Para poder entender mejor el beneficio que supone esta arquitectura, vamos a ver un ejemplo:

Supongamos que solo contamos con una capa de diez servidores de caché y que todos hablan directamente con los servidores de origen. Con un tráfico fluido, en el momento en el que un objeto caduque, lo hará más o menos a la vez en todas las cachés, por lo que cada uno de los diez servidores enviará una petición al servidor de origen para refrescar el objeto.

```
Total: 10 peticiones simultáneas a origen.
```

Ahora añadamos una segunda capa de cachés compuesta por dos servidores. Esta capa hablará directamente con los servidores del cliente y con la capa 1. La capa 1 seguirá teniendo diez servidores y solo hablará con la segunda capa que acabamos de añadir (embudo).

\
Bajo la misma casuística de tráfico fluido, un objeto caduca en todos los servidores a la vez y los diez servidores pedirán el objeto a la segunda capa de caché. Esta capa pedirá el objeto al origen dos veces (una por cada servidor), siendo capaz de servir las otras ocho peticiones sin tener que bajar a buscar el objeto a los servidores de origen del cliente. Por tanto, en este caso estamos reduciendo el tráfico a origen en un 80 %.

```
Total: 2 peticiones simultáneas a origen.
```
