---

copyright:
  years: 2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is the secondary IP unreachable?
{: #troubleshoot-vni-1}
{: troubleshoot}
{: support}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

The secondary IP is most likely unreachable because you haven't configured it properly.
{: shortdesc}

You cannot reach the secondary IP.
{: tsSymptoms}


The secondary IP might be unreachable for the following reasons:
- You forgot to configure a secondary IP on the guest OS after attaching a virtual network interface to a virtual server instance.
- You did not configure the secondary IP in the same subnet with the primary IP.
{: tsCauses}

Check for the following issues:
{: tsResolve}

* If you only configured the secondary IP through the API, UI, CLI, or Terraform, then you must login to the virtual server instance and configure it again on the instance's operating system.
* If you configured the secondary IP in a different subnet from the primary IP, you must correct the secondary IP configuration to assure it's in the same subnet as the primary IP.
