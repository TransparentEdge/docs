# API Protection

If your site exposes an API, **API Protection** helps you safeguard it from malicious activity and ensure secure access. You describe how your API is supposed to work by giving Transparent Edge its **OpenAPI or Swagger schema**, and the protection is enabled for the site you choose.

You'll find it under **Configuration → Provisioning → API Protection**.

API Protection is configured per site. In Sites Overview, the **API Protection** card shows whether an API schema is being enforced for each site.

### How it works

API Protection follows a **positive security** model: your schema defines what your API accepts, and anything else is rejected. Endpoints, methods and parameters that are not defined in the schema are blocked, so only the traffic your API is designed to receive reaches your origin.

Because of this, your schema has to describe your API completely and stay up to date. If you release a new endpoint or parameter and don't update the schema, legitimate requests to it may be blocked.&#x20;

### Before you start

You'll need:

* A **site** already added to your account (see Sites).
* The **OpenAPI or Swagger schema** of your API, either published at a public URL (for example, `https://api.example.com/openapi.yml`) or as the schema document itself, ready to paste.

### The API Protection page

The page lists every site with API Protection configured. The table shows:

| Column         | What it shows                                                   |
| -------------- | --------------------------------------------------------------- |
| **Site**       | The site the protection applies to.                             |
| **Schema URL** | The address of its schema, with a copy button.                  |
| **Active**     | Whether the protection is currently enabled (**Yes** / **No**). |
| **Actions**    | **Update** and **Delete**.                                      |

At the bottom of the table you can change how many **rows per page** are shown and turn on **Auto-refresh** to keep the list up to date automatically. As in the rest of Provisioning, a reminder at the top of the page shows the **CNAME** your domains must point to.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2026-10-27 a las 17.17.43.png" alt=""><figcaption></figcaption></figure>

### Protecting an API

1. Click **Protect API**.
2. In the **Create API Protection** window, choose the **Site** you want to protect. You can type to search among your sites.
3. Under **Schema source**, choose how you want to provide the schema:
   * **Schema URL** — point to an OpenAPI or Swagger file hosted somewhere reachable. Enter its **public URL** (for example, `https://petstore.swagger.io/v2/swagger.json`).
   * **JSON schema** — a text box appears where you paste the schema document itself. It's useful if your schema isn't published anywhere.
4. Leave **Active** switched on to enable API Protection for this site right away. If you'd rather prepare it first and turn it on later, switch it off.
5. Click **Protect**. **Discard** closes the window without saving anything.

If you use a **Schema URL**, it has to be **public**: Transparent Edge must be able to download the file without logging in.

### Updating or pausing a protection

Click **Update** next to a site to open the **Update API Protection** window. From there you can:

* Replace the **schema** with a new URL, or paste a new one — for example, after releasing a new version of your API.
* Switch **Active** off to pause the protection without losing its settings, and back on when you want it again.

Your changes only take effect once you click **Save changes**.

### Removing a protection

Click **Delete** next to a site to remove its API Protection. If you only want to stop it temporarily, switch **Active** off instead, so you can bring it back without setting it up again.
