# Amazon Web Services

Un caso de uso muy evidente es el de colocar Transparent Edge Services por delante de tu plataforma de **Amazón Web Services**, incluso aunque tengas el servicio de **CloudFront** contratado, no solo gozarás de un mayor portfolio de funcionalidades en el capa de CDN, si no que verás reducido considerablemente el coste de **AWS** debido principalmente a que en Transparent CDN dispondrás de tarifas más reducidas con respecto al tráfico que en AWS, llegando incluso a suponer, según el caso por supuesto, ahorro de costes de entre 35-45%.

Todo esto es posible sin tocar absolutamente nada de tu plataforma en AWS, lo único que tienes que hacer es configurar el CNAME que te dá aws como [origen](../getting-started/dashboard/autoprovisionamiento/sites-and-backends.md) de Transparent Edge Services.

### S3

Del mismo modo que puedes colocar CloudFront o tu plataforma de autoescalado configurando el CNAME del servicio de AWS como origen de Transparent Edge Services, puedes hacer lo mismo con **S3**. De manera que puede reducir el tráfico que llegue a **S3** de manera considerable cacheando el contenido en Transparent Edge Services y sirviendo todos los objetos que tengas en ese bucket desde nuestros edge.
