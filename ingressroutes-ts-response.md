---
copyright:
  years: 2022
lastupdated: "2024-06-20"

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

- the packet source is not `internet`
- the custom routing table **Traffic source** is not `INTERNET_FIP`
- the route **Destination** is not the public IP
- the nexthop IP is not VSI private IP
- the nexthop IP is not for a VSI running in the Floating IP VSI zone
-	the nexthop VSI does not belong to the same VPC as the Floating IP VSI
- the nexthop VSI has no setting `antispoofing disable`=`true`
- the next_hop virtual server instance is not defined, or its **State** is not `Running`

No response message was sent back from the destination floating-IP. If the next hop is not the original floating-IP, the packet is dropped.
{: tsCauses}

Make sure all of the configuration issues mentioned on this page are addressed.
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
