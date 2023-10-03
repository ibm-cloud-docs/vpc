---

copyright:
  years: 2018, 2023
lastupdated: "2023-01-31"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my connection unstable?
{: #troubleshoot-unstable-connection}
{: troubleshoot}
{: support}

If you notice that your connection is going up and down occasionally, it might be due to a rekey timing issue.
{: shortdesc}

My connection is not stable and keeps going down.
{: tsSymptoms}

A rekey timing conflict might exist.
{: tsCauses}

Follow the steps to resolve any timing issues:
{: tsResolve}

1. Make sure that NAT-Traversal is enabled on the peer, if it is a configurable option.
1. If you are using IKEv2, try different lifetimes so that the side with the shorter lifetime always initiates the rekeying process. 
1. Try disabling and reenabling the connection.
