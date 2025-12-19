---
copyright:
  years: 2022, 2025
lastupdated: "2025-12-19"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why don't I get a response from the destination floating-IP?
{: #troubleshoot-pi-floatingip-response}
{: troubleshoot}
{: support}

Correcting your public ingress route configuration is a way to resolve public ingress traffic routing errors.
{: shortdesc}

You try to get a response from the destination floating-IP, but get no response.
{: tsSymptoms}

This troubleshooting error can occur with the following conditions:

- The packet source is not `internet`
- The custom routing table **Traffic source** is not `INTERNET_FIP`
- The route **Destination** is not the public IP
- The next hop IP is not VSI private IP
- The next hop IP is not for a VSI running in the Floating IP VSI zone
-	The next hop VSI does not belong to the same VPC as the Floating IP VSI
- The next hop VSI has no setting `antispoofing disable`=`true`
- The next_hop virtual server instance is not defined, or its **State** is not `Running`

No response message was sent back from the destination floating-IP. If the next hop is not the original floating-IP, the packet is dropped.
{: tsCauses}

Make sure all of the configuration issues are addressed.
{: tsResolve}

Use the following example Linux command to add a secondary IP (of the floating-IP **public IP**) on one interface in the virtual server instance:

```sh
ip address add <address> dev <device>
```
{: pre}

```sh
ip address add 99.74.80.2/32 dev test4
```
{: pre}

Add address `99.74.80.2` with netmask `32` to device `test4`. To get the needed parameters, use the **ip -a** command.
