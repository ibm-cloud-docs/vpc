---

copyright:
  years: 2019, 2021
lastupdated: "2023-02-23"

keywords: load balancer, network, 401

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I receiving a `401 Unauthorized Error` code?
{: #troubleshoot-lb-401}
{: troubleshoot}
{: support}

Use the information in this topic to troubleshoot 401 error codes.
{: shortdesc}

You receive a `401` error code for your load balancer.
{: tsSymptoms}

HTTP 401 codes indicate that a client request has not been completed because it lacks valid authentication credentials for the requested resource.
{: tsCauses}

Check the following access policies for your user:
{: tsResolve}

* The access policy for the load balancer resource type.
* The access policy for the resource group.
