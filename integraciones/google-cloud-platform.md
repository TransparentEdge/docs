# Google Cloud Platform

Un caso de uso muy evidente es el de colocar Transparent Edge Services por delante de tu plataforma de Google Cloud Platform, incluso aunque tengas el servicio de **Cloud CDN** contratado en GCP, no solo gozarás de un mayor portfolio de funcionalidades en el capa de CDN, si no que verás reducido considerablemente el coste de GCP debido principalmente a que en Transparent CDN dispondrás de tarifas más reducidas con respecto al tráfico que en GCP, llegando incluso a suponer, según el caso por supuesto, ahorro de costes de entre 35-45%.

Todo esto es posible sin tocar absolutamente nada de tu plataforma en AWS, lo único que tienes que hacer es configurar el CNAME que te dá aws como [origen](../getting-started/dashboard/autoprovisionamiento/sites-and-backends.md) de Transparent Edge Services.

### Google Cloud Storage

Del mismo modo que puedes colocar Cloud CDN o tu plataforma de autoescalado de GCP configurando el CNAME del servicio como origen de Transparent Edge Services, puedes hacer lo mismo con **Cloud Storage**. De manera que puede reducir el tráfico que llegue a **Cloud Storage** de manera considerable cacheando el contenido en Transparent Edge Services y sirviendo todos los objetos que tengas en ese bucket desde nuestros edge.
