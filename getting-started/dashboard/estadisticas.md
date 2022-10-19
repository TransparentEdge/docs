# Analítica

En esta página encontrarás más información actualizada y en tiempo real, muy valiosa para tu negocio. A diferencia del histórico, la analítica tiene una retención máxima de datos de las últimas 36 horas (si quieres visualizar información más antigua, puedes consultarla en el histórico). Se puede filtrar según las necesidades de cada cliente, pero por defecto se aplica a la hora previa al momento consultado. El perido máximo de tiempo que se puede aplicar en los filtros es de 6 horas.&#x20;

El panel se puede personalizar de acuerdo a las necesidades de cada cliente y muestra las siguientes opciones:

![](<../../.gitbook/assets/Captura de pantalla 2020-06-09 a las 14.29.26.png>)

Encontrarás también la información de los _requests_ y ancho de banda:

![](<../../.gitbook/assets/image (1) (1).png>)

Después se despliegan las gráficas del _hit ratio_, donde está toda la información de la eficiencia de la caché. Así, muestra los objetos que no han logrado ser cacheados (_miss_), los objetos caducados (_expired_) y los que estaban caducados y se han servido a partir del objeto caducado (_stale_). Y conjuntamente, según la configuración, se puede visualizar todo el tiempo de respuesta (_response time_) de las peticiones.\


![](<../../.gitbook/assets/image (3) (1).png>)

Cada vez que el _bot_ de Google visita el _site_ que esté bajo Transparent Edge Services, queda evidenciado en la gráfica _Google bot access._ El gráfico _Google crawler percent_ despliega el código de respuesta que ha tenido el _bot_ de Google en cada uno de sus accesos, lo que aporta una herramienta de monitorización interesante para el SEO de nuestros clientes.

![](<../../.gitbook/assets/image (4) (1).png>)

Los siguientes gráficos muestran los códigos de respuesta que han tenido todas las peticiones que ha recibido el _site_ y de qué lugar geográfico han venido esas peticiones.

![](<../../.gitbook/assets/image (5) (1).png>)

Los siguientes gráficos muestran qué protocolo se está utilizando (https o http) y de dónde vienen las peticiones en términos de dispositivos:

![](<../../.gitbook/assets/image (6).png>)

Las siguientes tablas muestran las peticiones por país y las URL más pedidas de cada _site_:

![](<../../.gitbook/assets/image (7).png>)

El gráfico siguiente muestra todos los purgados que realizan los clientes a través del panel. A su lado encuentras la tabla de todos los contenidos que tiene cada _site,_ cuántas _requests_ recibe y cuánto ancho de banda consume.

![](<../../.gitbook/assets/image (8).png>)

El gráfico de códigos de status (_status codes_) permite visualizarlos. La tabla de referencias (_referer_) muestra de qué URL vienen las peticiones.

![](<../../.gitbook/assets/image (9).png>)

La tabla de _IP Address table_ muestra las IP de las que proceden las peticiones. El gráfico _Content Type,_ por su parte, permite visualizar la información sobre los contenidos que tiene cada _site_.

![](<../../.gitbook/assets/image (10).png>)



{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [_dashboard_](https://dashboard.transparetncdn.com) __ puedes hacerlo también desde nuestro [API](../faq/glosario/api.md).
{% endhint %}
