# Invalidación por etiquetas (tags)

### Qué es y cómo lo implementa Transparent Edge

Una parte fundamental del buen funcionamiento de un sitio web tiene que ver con el control de los objetos cacheados y cómo y cuándo invalidarlos.&#x20;

Mediante la invalidación por _tags_, etiquetamos grupos de objetos para luego poder invalidarlos de forma rápida, eficiente y selectiva. Esto nos evita el tener que idear complejos _scripts_ que construyan el conjunto de las URL a invalidar cada vez que se actualiza el contenido y se quiere invalidar selectivamente.&#x20;

En Transparent Edge utilizamos la cabecera _Surrogate-Keys_ para determinar qué etiquetas se le asignan a un objeto. Cuando se produce un _miss_, es decir, se pide un objeto actualmente no cacheado, bajamos a origen. Para cada objeto de la respuesta de origen se comprueba la existencia de la cabecera _Surrogate-Keys_ y, en el caso de existir, se le asigna el valor de dicha cabecera como etiqueta. Si el valor de la cabecera _Surrogate-Keys_ está formado por varias palabras separadas por espacios, al objeto se le asignan todas y así es  posible invalidarlo por cualquiera de ellas.&#x20;

Un ejemplo:

```http
# petición
GET /portada.html HTTP/1.1
Host: www.example.com

# respuesta
HTTP/1.1 200 OK
Surrogate-Keys: portada estaticos1
Content-Type: text/html
...
```

El objeto quedará etiquetado con "portada" y "estaticos1". Por lo tanto, si realizamos una petición de invalidación por cualquiera de esos dos _tags_, invalidaremos dicho objeto y todos los que incluyan dicho _tag_. Esto nos permite agrupar los objetos relacionados entre sí que deban ser invalidados a la vez, sin importar qué URL tengan.&#x20;

Cada objeto puede tener múltiples _tags_ asociados y se invalidará ese objeto y todos los que contengan dicho _tag_ y estén cacheados si uno de los _tags_ coincide con una petición a invalidar.

### Relaciones entre _tags_ y objetos

El poder y el control que nos proporciona el invalidar mediante _tags_ se debe a que se permite una relación de _varios a varios_ entre _tags_ y objetos. La respuesta del servidor de origen puede asociar múltiples _tags_ a un objeto y esos _tags_ pueden ser compartidos por otros objetos creando así relaciones entre ellos.&#x20;

Un ejemplo serían estas dos peticiones con su respuesta del _backend_:

```http
GET /shop/ HTTP/1.1
Host: www.example.com

HTTP/1.1 200 OK Content-Type: text/html
Content-Length: 875
Surrogate-Key: shop main-shop
```

```http
GET /shop/product/24656-tshirt HTTP/1.1
Host: www.example.com

HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 4123
Surrogate-Key: shop product-tshirt
```

Tenemos dos objetos en el ejemplo anterior (`/shop` y `/shop/product/24656-tshirt`) con tres _tags_ (`shop`, `main-shop` y `product-tshirt`). Dos de ellos (`main-shop` y `product-tshirt`) están asignados a un solo objeto y un tercero (`shop`) está asignado en ambos.

Esto nos permite crear asociaciones entre objetos a placer e invalidar de una forma totalmente selectiva.

### Generando y asignando los _tags_

Se puede realizar de dos maneras:

* Desde el propio servidor de origen (recomendado).
* Desde Transparent Edge mediante configuración VCL.

#### Desde origen

Es la forma recomendada. Se realiza desde la aplicación que sirve contenido en origen y depende por completo de la implementación en origen.&#x20;

Un ejemplo sencillo si se está utilizando Nginx sería:

```python
  location ~* ^/content/.*\.(css|js|png|gif)$ {
      add_header Surrogate-Keys "estaticos content";
```

Etiquetaría a todos los objetos con extensión `css`, `js`, `png` y `gif` bajo la ruta `/content/*` con dos _tags_: `estaticos` y `content`.

#### Desde Transparent Edge Services mediante configuración VCL avanzada

Para ello debemos incorporar los _tags_ que queramos en la subrutina `vcl_backend_response.`

```python
# Este ejemplo agrega el tag "images" a todos los recursos
# con extensión jpg, jpeg, png o gif.
sub vcl_backend_response {
    if (bereq.url ~ "\.(jpg|jpeg|png|gif)($|\?)"){  
       set beresp.http.Surrogate-Keys = "images " + beresp.http.Surrogate-Keys;
    }
}
```

Es muy importante el espacio que existe después de `images`, ya que de lo contrario, si origen también define algunos _tags_ para esos objetos, el primer _tag_ se concatenará con el tag `images`.

### Purgar/invalidar objetos mediante _tags_

#### Desde el panel

Es la parte más sencilla del proceso. Simplemente tenemos que entrar en el panel de Transparent Edge ([https://dashboard.transparentcdn.com](https://dashboard.transparentcdn.com)) y navegar hasta el apartado de invalidaciones (Home -> Invalidaciones -> TAG).

Pulsamos sobre "PURGAR TAGS" y escribimos los _tags_ por los que queramos invalidar, uno por línea:

![](<../../.gitbook/assets/Captura de pantalla 2022-12-21 a las 17.35.53.png>)

#### Mediante API

Todo lo que puedes hacer desde nuestro [_dashboard_](https://dashboard.transparetncdn.com/) puedes hacerlo desde nuestro [API](https://docs.transparentedge.eu/getting-started/faq/glosario/api).

En este caso, se debe realizar una petición POST autenticada al endpoint`/v1/companies/<company_id>/tag_invalidate/.` Puedes conseguir el **\<API\_TOKEN>** siguiendo los pasos detallados en: [https://docs.transparentedge.eu/api/autenticacion](https://docs.transparentedge.eu/api/autenticacion)

```javascript
// tags.json
{
    "tags": [
        "portadas",
        "rss"
    ]
}
```

```bash
$ curl -kv --request POST --data @tags.json \
 -H "Authorization: Bearer <API_TOKEN>" \
 -H "Content-Type: application/json" \
 "https://api.transparentcdn.com/v1/companies/512/tag_invalidate/"
```

En el ejemplo anterior, se muestra cómo invalidar todos los objetos que tengan asociados los _tags_ _`portada`_ y _`rss`_ en una sola petición. Para adaptarlo, tan solo se debe modificar la compañía número 512 del ejemplo sustituyéndola por la tuya y proporcionar un [token válido](https://docs.transparentedge.eu/api/autenticacion).

Como ves, es muy sencillo invalidar contenido programáticamente, ya sea utilizando _curl_ y _shell scripts_ o con cualquier lenguaje de programación, por ejemplo [Python requests](https://docs.python-requests.org/en/master/).

{% hint style="warning" %}
Recuerda que si se modifica el conjunto de _tags_ que se aplica a un recurso, ya sea desde origen o desde nuestro panel con configuración VCL, los nuevos _tags_ no se aplicarán hasta que el objeto expire o sea invalidado por los _tags_ antiguos.

Es en el momento en el que se produce un _miss_ cuando un objeto recibe los _tags_.
{% endhint %}



