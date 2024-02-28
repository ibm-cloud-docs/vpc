---

copyright:
  years: 2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I configure my virtual network interface?
{: #troubleshoot-vni-2}
{: troubleshoot}
{: support}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

If you cannot configure your virtual network interface, it might be because you do not have IAM Administrator permissions.
{: shortdesc}

This may be indicated with an authorization error message when running the following commands to configure the virtual network interface:
{: tsSymptoms}

`is.virtual-network-interface.virtual-network-interface.manage-infrastructure-nat`

`is.virtual-network-interface.virtual-network-interface.manage-ip-spoofing`

A lack of Administrator permissions can prevent you from being able to configure your virtual network interface.
{: tsCauses}

Contact the account administrator associated with the virtual network interface's IP address to finish configuring your virtual network interface. To learn more, see [IAM permissions](/docs/account?topic=account-iam-service-roles-actions#is.virtual-network-interface-roles).
{: tsResolve}
