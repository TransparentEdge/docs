# HLS (HTTP Live Streaming)

Es un protocolo de comunicaciones de transmisión de velocidad de bits adaptable, basado en HTTP e implementado por Apple Inc. como parte de su software QuickTime, Safari, OS X e iOS. Funciona de forma similar a MPEG-DASH en cuanto a que también divide la transmisión en pequeños segmentos HTTP descargables.

Mediante listas de reproducción m3u extendidas, se hace llegar al cliente el listado de los distintos canales de transmisión (_streams_) junto a las tasas de bits (_bitrates_) disponibles. Al estar basado en HTTP, este protocolo pasa por cualquier cortafuegos o _proxy_ que acepte tráfico HTTP estándar, al contrario que otros protocolos como RTP. Esto también habilita a que el contenido sea ofrecido mediante servidores HTTP convencionales y CDN.
