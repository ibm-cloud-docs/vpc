---
copyright:
  years: 2022
lastupdated: "2022-10-03"

keywords: 

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why don't I get a response from the destination floating-IP?
{: #troubleshoot-pi-floatingip-response}
{: troubleshoot}
{: support} 

Correct destination floating-IP conditions as a way to resolve public ingress traffic routing errors.
{: shortdesc}

You try to get a response from the destination floating-IP, but get no response.
{: tsSymptoms}

This troubleshooting error occurs with the following conditions: 

- the packet source is `internet`
- the custom routing table **Traffic source** is `INTERNET_FIP`
- the route **Destination** is the public IP
- the route **Next_hop** is the destination floating-IP's private IP
- the local port has the setting `antispoofing disable`=`true`
- the next_hop virtual server instance is defined, and the **State** is `Running` 

No response message was sent back from the destination floating-IP. If the next hop is not the original floating-IP, the packet is dropped. This occurs because spoofing to public is not allowed, even if anti-spoofing is disabled. 
{: tsCauses}

Use the following Linux command to add a secondary IP (of the floating-IP **public IP**) on one interface in the virtual server instance:
{: tsResolve}

```sh
ip address add 99.74.80.2/32 dev test4 
```
{: pre}

Add address `99.74.80.2` with netmask `32 ` to device test `4`. For getting the needed parameters, use the **ip -a** command.
