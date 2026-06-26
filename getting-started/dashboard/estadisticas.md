# Analytics

On this page you will find more real-time information that is very valuable for your business. Unlike the historic, analytics retains data for the past 36 hours (to view older information, you can consult the historic section).

It can be filtered according to your needs, but the previous hour filter it is applied by default. The maximum period that can be applied for filtering is six hours.

The panel can be customized according to your needs. It displays the following options.

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

You will also find the requests and bandwidth information.

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Hit ratio graphs are then displayed, which include all the cache efficiency information. The missed objects are displayed, along with the expired objects and stale objects. As a whole, depending on the settings, the entire response time of the requests can be displayed.

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

The following graphs display the response codes of all the requests received by the site and the geographical location from where these requests are sent.

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

The following graphics show every response code returned to the requests received and the geographical origin of this requests.

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

The following graphs display the protocol being used (HTTPS or HTTP) and the source of the requests in terms of devices.

![](<../../.gitbook/assets/image (12).png>)

Requests by country and the most requested URLs per site are found here.

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

The next graph displays all the purges performed by clients through the panel. Next to it is the table of all the contents of each site, how many requests it receives, and how much bandwidth it consumes.

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

All this is displayed in the status codes graph, and the referrer table displays the URLs from where the requests are sent.

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

### IP Address table

The IP address table shows the IPs from where the requests are sent. Each row in the table represents a source IP address and provides information about the number of requests, bandwidth consumed, its risk level, and the available actions to manage it.

**IP:** Source IP address of the requests. A colored dot provides a visual indication of the risk level.

**Risk:** Risk score of the IP address (0–100). A score of 0 indicates no known risk.

**Requests:** Number of requests received from the IP address and its percentage of the total traffic.

**Bandwidth:** Volume of data transferred from the IP address and its percentage of the total bandwidth.

**Actions:** Three action buttons: IP details, block/allow, and filter.



#### **IP Information**

Clicking the **IP Information** button opens a modal with a comprehensive analysis of the selected IP address, including geolocation, network information, risk score, and threat analysis.

The **Threat Analysis** section is arguably the most relevant from a security perspective. It consolidates abuse reports, threat intelligence sources, and behavioral flags associated with the IP address.

{% hint style="info" %}
Remember that everything you can do from our [dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F), you can do from our [API](https://docs.transparentedge.eu/v/english/guias/api).
{% endhint %}
