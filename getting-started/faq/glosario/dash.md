# DASH

Transmisión Dinámica Adaptativa sobre HTTP (DASH), también conocida como MPEG-DASH. Es una técnica de reproducción en tiempo real (_streaming_) de tasa de bits (_bitrate_) adaptativa, que permite la transmisión de contenido multimedia de alta calidad a través de internet por medio de servidores web HTTP convencionales. MPEG-DASH funciona al dividir el contenido en una secuencia de pequeños segmentos HTTP. Cada segmento contiene un corto intervalo de vídeo/audio y existen múltiples segmentos distintos para cada _bitrate_.

Cuando el contenido se reproduce mediante un cliente MPEG-DASH, este se encarga de seleccionar los segmentos con el mayor _bitrate_ posible que puede descargar a tiempo para obtener una reproducción fluida.
