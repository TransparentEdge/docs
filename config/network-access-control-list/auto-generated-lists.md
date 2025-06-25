# Auto generated lists

Some services will automatically create network ACLs, these lists will have specific functions which we will explain below.

## Auto AntiDDoS list

The AntiDDoS system will generate a list of suspicious IPs and add them to a Network ACL named "**ddos\_identified\_ips**".

This list serves an informational purpose only and does not take any further action against the IPs. You can utilize this list as needed.

## **Block this IP button**

In the [Analytics](../../getting-started/dashboard/estadisticas.md) section, you will find a button "**Block this IP**" under the "IP address table".

The blocked IPs will be added to a Network ACL named "**auto\_blacklist**". Also a [deny VCL configuration](../../getting-started/dashboard/auto-provisioning/network-acls.md#deny-list-example) will be created for the IPs in this list.

IPs on this list will be unconditionally blocked unless a TTL is set when blocking the IPs.

## Auto Block IPs anomaly reaction

The "[Block IP](../../security/anomaly-detection/automatic-reactions.md#reactions-types)" reaction in [Anomaly Detection service](../../security/anomaly-detection/), automatically blocks an IP if its requests per second exceed the predefined threshold. These IPs are then added to the "**auto\_blacklist**" ACL.

By default, blocked IPs have a TTL of 1 hour, though this can be adjusted when setting the threshold.

## Captcha reaction

The "[Captcha](../../security/anomaly-detection/automatic-reactions.md#reactions-types)" reaction in [Anomaly Detection service](../../security/anomaly-detection/), automatically adds an IP to the "**auto\_captcha**" ACL if its requests per second exceed the predefined threshold. These IPs are then required to solve a Captcha puzzle for each request.

By default, Captcha have a TTL of 1 hour, though this can be adjusted when setting the threshold.

## JS Challenge reaction

The "[JS Challenge](../../security/anomaly-detection/automatic-reactions.md#reactions-types)" reaction in [Anomaly Detection service](../../security/anomaly-detection/), automatically adds an IP to the "**auto\_jschallenge**" ACL if its requests per second exceed the predefined threshold. These IPs are then required to pass a JS Challenge for each request.

By default, JS Challenge have a TTL of 1 hour, though this can be adjusted when setting the threshold.
