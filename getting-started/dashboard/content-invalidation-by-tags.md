# Content invalidation by tags

### What is invalidation by tags and how is it implemented?

A fundamental part of a website's proper functioning has to do with controlling cached objects and how and when to invalidate them. By using tag invalidation, we tag groups of objects so that we can quickly, efficiently, and selectively invalidate them. This avoids the need to devise complex scripts to construct the set of URLs to invalidate whenever the content is updated and we want to invalidate selectively.

At Transparent Edge, we use the Surrogate-Keys header to determine which tags are assigned to an object. When a miss occurs, which means a currently uncached object is requested, we go to the origin. For each object in the origin's response, we check for the existence of the Surrogate-Keys header and, if it exists, we assign its value as a tag to the object. If the value of the Surrogate-Keys header consists of several words separated by spaces, we assign all of them to the object, which means they can be invalidated using any of them.

Example:

```http
# request
GET /portada.html HTTP/1.1
Host: www.example.com

# response
HTTP/1.1 200 OK
Surrogate-Keys: main statics1
Content-Type: text/html
...
```

The object will be labeled with "main" and "static1". Therefore, if we make an invalidation request for any of those two tags, we will invalidate that object and all those that include that tag. This allows us to group related objects that need to be invalidated at the same time, regardless of what URL they have.

Each object can have multiple associated tags, if a request is made to invalidate by tag, all the objects that share that tag will be invalidated at once.

### Relationship between tags and objects

The power and control provided by invalidating by tags is that it allows for a many-to-many relationship between tags and objects. The origin server's response can associate multiple tags with an object, and those tags can be shared by other objects, creating relationships between them.

An example of this would be the following two requests with their backend response:

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

In the example above, we have two objects (`/shop and /shop/product/24656-tshirt`) with three tags (s`hop, main-shop, and product-tshirt`). Two of them (`main-shop and product-tshirt`) are assigned to a single object, and a third (`shop`) is assigned to both.

This enables us to create associations between objects at will and invalidate them in a completely selective way.

### Generating and assigning tags

This can be done in two ways:

* From the origin server (recommended).
* From the CDN using VCL code.

#### From the origin server

It must be implemented directly in the application or in the webserver that serves the content from the origin.

An example if the origin server is using Nginx would be:

```python
  location ~* ^/content/.*\.(css|js|png|gif)$ {
      add_header Surrogate-Keys "statics content";
```

This will tag all the objects with the extension `css`, `js`, `png` or `gif` under the path `/content/` with the tags `statics` and `content`.

#### From VCL configuration

We need to incorporate the tags into the subroutine `vcl_backend_response`.

```python
sub vcl_backend_response {
    if (bereq.url ~ "\.(jpg|jpeg|png|gif)($|\?)"){  
       set beresp.http.Surrogate-Keys = "images " + beresp.http.Surrogate-Keys;
    }
}
```

The above example will add the tag `images` to all the resources with the extension `jpg`, `jpeg`, `png` or `gif`.

The space after the tag plus the addition of the header `Surrogate-Keys`:

<pre><code><strong>"images " + beresp.http.Surrogate-Keys;
</strong></code></pre>

is recommended. if the origin sends additional tags in the `Surrogate-Keys` header they will not work without it.​

### Purge/invalidate objects using tags

#### Purging via panel (dashboard)

Login into our [dashboard](https://dashboard.transparentcdn.com/) and go to:

Invalidation -> Purge -> Purge TAGs

Enter the tags that you want to purge and click "Purge TAGs".

#### Purging via API

You'll need to make an authenticated **POST** request to `/v1/companies/<company_id>/tag_invalidate/`

The body of the request must have the key "tags", for example:

```javascript
// tags.json
{
    "tags": [
        "main",
        "rss"
    ]
}
```

With [URL](https://en.wikipedia.org/wiki/CURL), you can do it like this:

```bash
$ curl -kv --request POST --data @tags.json \
 -H "Authorization: Bearer <API_TOKEN>" \
 -H "Content-Type: application/json" \
 "https://api.transparentcdn.com/v1/companies/512/tag_invalidate/"
```
