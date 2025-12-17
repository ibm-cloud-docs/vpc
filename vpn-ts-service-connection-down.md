---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my service connection show down even after it shows attached on Transit Gateway?
{: #troubleshoot-service-connection-down}
{: troubleshoot}
{: support}

A site-to-site dynamic route-based VPN connection in {{site.data.keyword.cloud_notm}} might display a "down" status despite being attached to the Transit Gateway. This situation typically arises from network issues, incorrect routing configurations, or unstable lifecycle states of the VPN Gateway or service connection.
{: shortdesc}

The service connection shows a "down" status even when it appears attached to the Transit Gateway.
{: tsSymptoms}

Common causes include network connectivity problems, misconfigured routes, or an unstable lifecycle state of the VPN Gateway or service connection. If the VPN Gateway is unhealthy or the service connection is in a failed state, these issues can prevent successful communication.
{: tsCauses}

Follow these steps to fix the service connection problem:
{: tsResolve}

1. **Check the Transit Gateway attachment**: Make sure that the connection is showing as "attached" on the Transit Gateway. If not, re-create the connection.
1. **Verify VPN Gateway health**: Check the lifecycle state of the VPN Gateway. If it is unstable or unhealthy, wait until it stabilizes before you try again. If the issue persists, contact IBM support.
1. **Inspect the service connection lifecycle state**: Confirm that the service connection on the IBM VPN Gateway is stable. If it is in a failed state, attempt to re-create the connection, provided the VPN Gateway is healthy. If not, contact IBM support for assistance.
