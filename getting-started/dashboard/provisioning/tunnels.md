# Tunnels

Many services live in places the Internet can't easily reach: a development server on a laptop, an internal tool at the office, a web app behind a home router. **Tunnels** lets you publish them through the Transparent Edge network without opening ports, configuring your router or needing a public IP address.

You create a tunnel in the dashboard, download its configuration and run a small client (`frpc`) next to your service. From then on, your service is reachable at a public address under `tunnels.transparentedge.io`.

Tunnels is an optional service. If you don't see it in your dashboard, it may not be included in your plan: contact your account manager to enable it.&#x20;

### How it works

A traditional setup needs the Internet to reach _into_ your network: you need a public IP, and your router or firewall has to forward incoming connections to the right machine.

A tunnel flips that around. The client you run next to your service opens an **outgoing** connection to the Transparent Edge network and keeps it open. When someone visits your tunnel's public address, the request travels through that existing connection to your service, and the response goes back the same way.

<figure><img src="../../../.gitbook/assets/How a tunnel works@2x.png" alt=""><figcaption></figcaption></figure>

This has a few practical consequences:

* **No port forwarding.** You don't need to touch your router or open inbound ports.
* **Works behind NAT.** Since the connection starts from the inside, it works even if you don't have a public IP address, or if your provider shares one IP between many customers (CG-NAT).
* **Dynamic IPs are not a problem.** There's no DNS record pointing to your IP that could go stale when it changes. If the connection drops, `frpc` simply reconnects.
* **The service stays where it is.** Your application keeps running on your own machine or network; the tunnel only carries the traffic.

### Before you start

Make sure you have:

* A service that **already works locally**. Note down its address and port, for example a web app on `http://localhost:8080`. If `frpc` has nothing to connect to, the tunnel has nothing to deliver.
* A machine that can run `frpc` and can reach your service. It can be the same machine that hosts the service, or any other machine on the same network that can access it.
* An Internet connection that allows **outgoing connections**. On home networks this is almost always the case; on restrictive corporate firewalls you may need to allow outbound traffic to the tunnel server.

### Creating a tunnel

1. Open **Tunnels** from the side menu (**Configuration → Provisioning → Tunnels**) and click **Create tunnel**.
2. In the **Create Tunnel** window, optionally write a **Description**: a label to help you tell this tunnel apart from the others later (for example, _Jenkins at the office_). You can change it at any time.
3. Click **Create tunnel**.

That's it. The **subdomain**, **token** and **site** are generated for you, so there's nothing else to fill in. You'll see the message _Tunnel created successfully_ and the details window of your new tunnel opens right away, with the status **Pending** (ready, but no client has connected yet). From there you can download the configuration and copy the token. The tunnel also appears in the list.

### Connecting your service

1. In the tunnels list, click the download icon next to your tunnel, or open its details and click **Download config (.toml)**. This is your `frpc` configuration file.
2. Install `frpc` on the machine that will connect to your service. It's the client from the open-source [frp](https://github.com/fatedier/frp) project: download the build that matches your operating system and architecture.
3. Open the configuration file and set **`localPort`** to the port where your service is listening (for example, `8080`).
4. Run `frpc` with your configuration file:

```
   frpc -c <your-config-file>.toml
```

5. Back in the dashboard, check that your tunnel's status has changed. Then open the **public domain** from a different network (for example, from your phone using mobile data) to confirm your service is reachable.

\{% hint style="success" %\} Need a hand? In the tunnel's details window, **Download examples (.zip)** gives you the full package: sample configurations plus a README that walks through the setup step by step. \{% endhint %\}

The tunnel only works **while `frpc` is running**. If you close the terminal, shut down the machine or lose the connection, your public address stops responding until `frpc` is running again. If you plan to use it day-to-day, run `frpc` as a background service that starts automatically with your machine.

### Managing your tunnels

The **Tunnels** page lists every tunnel you've created. From here you can create a tunnel, download its config and run `frpc` locally:

| Column          | What it shows                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Subdomain**   | The tunnel's unique name (it ends with the server identifier, e.g. `-mad1`), with its full public address underneath. Click the copy icon to copy the subdomain.                                                                                                                                                                                                                                                                                                                                         |
| **Description** | Your optional label.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **Status**      | <p>The current state of the tunnel:<br>• <strong>Pending</strong>: just created, no client has connected yet. This is what you'll see until you run <code>frpc</code> for the first time.<br>• <strong>Disconnected</strong>: it was connected before, but there's no client connected right now. Check that <code>frpc</code> is still running and that your machine has Internet access.<br>Once <code>frpc</code> connects successfully, the status updates to show the tunnel is up and running.</p> |
| **Server**      | The tunnel server your tunnel is assigned to.                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Created**     | When the tunnel was created.                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **Actions**     | **View details**, **Download config** and **Delete**.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

Use the **Search by subdomain** box to find a tunnel quickly if you have many. At the bottom of the table you can change how many **rows per page** are shown, and turn on **Auto-refresh** to keep the list (and the statuses) updated automatically while you set things up.

### Tunnel details

Click the eye icon on any tunnel to open its details (the same window that opens right after creating it):

* **Description**: click the pencil icon to add or edit it.
* **Subdomain**: the tunnel's unique identifier, with a copy button.
* **Public domain**: the full address where your service is reachable. Use the copy button to copy it, or the link icon to open it in a new tab.
* **Token**: the credential that links your `frpc` client to this tunnel. It's hidden by default: click the eye icon to reveal it, or the copy icon to copy it without displaying it.
* **Status**, **Site**, **Server** and **Created**: the current state, the site automatically generated for this tunnel (identified by a number, for example _#22572_), the server it's assigned to, and its creation date.
* **Configuration**: buttons to **Download config (.toml)** and **Download examples (.zip)**.

Treat the **token** like a password. Anyone who has it can connect a client to your tunnel. Don't share it or publish it in code repositories, and if you think it may have been exposed, delete the tunnel and create a new one.

### Exposing more than one service

A single configuration file can define several **proxies**, one for each local service you want to publish. When you add more proxies to your configuration, use the tunnel's **subdomain** value _exactly as shown_ in both the `name` and `subdomain` fields. You can copy it from the tunnel's details window.

### Deleting a tunnel

If you no longer need a tunnel, click the trash icon in the list, or **Delete tunnel** in the tunnel's details window. Once deleted, its public address stops working and its token can no longer be used to connect. If you need one later, you'll have to create a new tunnel, with a new subdomain and a new token.

### Keeping your tunnel secure

A tunnel makes a service that used to be private reachable from the Internet. Keep this in mind:

* **Anyone who knows the public address can try to access it.** Don't rely on the address being hard to guess. Make sure your application has its own authentication (a login, an API key...), especially if it's an admin panel, a dashboard or anything that shows private data.
* **Only publish what you need.** Point the tunnel at the specific service you want to expose, not at anything else on the machine or network.
* **Protect your token** as described above, and delete tunnels you're no longer using.
* **Keep `frpc` up to date** by regularly installing new versions from the frp project.

### Good to know

* **Availability depends on you.** The tunnel is only as reliable as the machine running `frpc`, your service and your Internet connection. If your computer goes to sleep or your connection drops, the tunnel goes down with it.
* **Upload speed matters.** The traffic travels through your connection, so the upload speed of your network limits how fast your service can respond to visitors.
* **Great for testing, demos and internal tools.** Tunnels are especially handy for showing a work-in-progress site to a colleague or client, receiving webhooks on a development machine, or giving access to a tool that lives on a private network.
