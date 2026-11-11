# Streaming Logs

With **Streaming Logs**, Transparent Edge sends you the raw access logs of your services **in real time**. Instead of waiting for an hourly file, you subscribe to a **Kafka** queue and receive each log as it's generated, so you can process it with your own tools: a monitoring system, a log platform, an analysis script...

You'll find it under **Observability → Streaming Logs**.

If you prefer to receive the logs in batches, as compressed files every hour on an FTP, SFTP or S3 server, use Logs instead.&#x20;

Streaming Logs has to be activated first. You can do it from **Services** in the dashboard.&#x20;

### How it works

1. You allow the **IP addresses** of the machines that will read the logs (your _consumers_).
2. You download the **ZIP** with your certificates and ready-to-use templates.
3. You configure your consumer with them and connect to Transparent Edge's Kafka servers.

### Allowing your consumers

The page lists the IP addresses that are allowed to connect, each with a copy button and a **Delete** button. At the bottom you'll see how many there are.

To add them, click **Add IPs** and enter the address of the machine where your consumer will run. You can add several at once, separated by commas.

Add the IP **before** trying to connect: connections from addresses that aren't on the list are rejected. Firewall changes can take about 5 minutes to take effect.&#x20;

### Downloading the configuration files

At the top of the page, **Configuration Templates** gives you everything you need to connect. Click **Download ZIP** to get all the necessary files, including your password and the templates. The ZIP contains:

| File                                                 | What it is                                                              |
| ---------------------------------------------------- | ----------------------------------------------------------------------- |
| `c<ID>.crt.pem`                                      | Your client public certificate.                                         |
| `c<ID>.key.pem`                                      | Your client private certificate.                                        |
| `c<ID>.keystore.p12`                                 | Your certificates in PKCS12 format.                                     |
| `password.txt`                                       | The password of the keystore and the truststore.                        |
| `transparentcdnCA.pem`                               | The public certificate of the Transparent Edge certification authority. |
| `truststore.p12`                                     | The truststore, needed by some consumers.                               |
| `filebeat.yml`, `kafka-logstash.conf`, `consumer.py` | Configuration templates for Filebeat, Logstash and a Python consumer.   |

Here `<ID>` is your company ID (for example, `c123`).

{% hint style="warning" %}
Keep the certificates, the keystore and the password safe: make them readable only by the user that runs your consumer, and don't share them or leave the password in configuration files that other people can see.&#x20;
{% endhint %}

### Connecting

Connect to the three Kafka servers using **SSL** on port **9093**:

```
kafka1.edgetcdn.io:9093
kafka2.edgetcdn.io:9093
kafka3.edgetcdn.io:9093
```

#### Topics

Each type of log has its own topic, named after your company ID:

| Logs                             | Topic               |
| -------------------------------- | ------------------- |
| Delivery logs (the default ones) | `c<ID>`             |
| Midtier (L2) delivery logs       | `c<ID>_delivery_l2` |
| Backend delivery logs            | `c<ID>_backend`     |
| WAF audit logs                   | `c<ID>_waf`         |
| Bot Management logs              | `c<ID>_botm`        |

#### Consumer groups

Your consumer's **group ID** must start with `c<ID>_` (for example, `c123_group1`).

* Consumers in the **same group** share the work: each one receives part of the logs, which gives you parallel processing and high availability.
* Consumers in **different groups** each receive **all** the logs, which is useful if you want to process them in different ways.

Topics have 2 partitions by default. For logs, one consumer is usually enough; add more if you need to read in parallel or want a backup if one fails. If you need more partitions, contact our support team.

### Reading the logs

You can use any Kafka client. The ZIP includes templates for three common options:

* **Filebeat** — the simplest choice in most cases. Install Filebeat, enable its Kafka module and edit `filebeat.yml` with the paths of your certificate, key and CA, and the folder where you want the logs to be written.
* **Logstash** — useful if you need to transform the logs or send them to other destinations. Copy `kafka-logstash.conf` to your Logstash configuration folder and point it to the keystore and the truststore.
* **Python** — use `consumer.py` as a starting point (it needs the `confluent-kafka` library). Set the paths of the CA and the keystore, the password and the topic you want to read.

### Log formats

| Logs                      | Format                                                                                                                                     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Delivery and Midtier (L2) | Text lines in Apache style, with additional fields from the CDN.                                                                           |
| Backend                   | Text lines with timing and backend fields.                                                                                                 |
| WAF                       | JSON. Each attack found in a request is sent as a separate log entry, and each one includes a `ruleId` that you can use to set exceptions. |
| Bot Management            | JSON, with fields such as `country`, `crawler`, `whitelisted`, `status`, `reason` and `action` (block, captcha, jschallenge or bypass).    |
