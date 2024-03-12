---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-12"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I configure my virtual network interface?
{: #troubleshoot-vni-2}
{: troubleshoot}
{: support}

If you cannot configure your virtual network interface, it might be because you do not have IAM Administrator permissions.
{: shortdesc}

This may be indicated with an authorization error message when running the following commands to configure the virtual network interface:
{: tsSymptoms}

`is.virtual-network-interface.virtual-network-interface.manage-infrastructure-nat`

`is.virtual-network-interface.virtual-network-interface.manage-ip-spoofing`

A lack of Administrator permissions can prevent you from being able to configure your virtual network interface.
{: tsCauses}

Contact the account administrator associated with the virtual network interface's IP address to finish configuring your virtual network interface.
{: tsResolve}
