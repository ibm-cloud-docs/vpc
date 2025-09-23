---

copyright:
  years: 2025
lastupdated: "2025-09-23"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I bind a public address range at creation or delete one that's already bound?
{: #troubleshoot-binding-public-address-ranges}
{: troubleshoot}
{: support} 

Binding a public address range during creation or deleting a bound range fails due to insufficient permissions. 
{: shortdesc}

You won't be able to create or delete the public address range.
{: tsSymptoms}

Insufficient IAM role or required IAM actions.
{: tsCauses}

To perform these tasks, you must first have either the Administrator or Editor IAM role. To create a public address range, the action `is.public-address-range.public-address-range.create` is required. To delete a public address range, `is.public-address-range.public-address-range.delete` is necessary. If the public address range is to be bound during creation, or is already bound when you want to delete the range, you must also have the `is.vpc.vpc.operate` permission.
{: tsResolve} 

For more information, see [IAM roles and permissions](/docs/vpc?topic=vpc-about-par&interface=ui#par-access-management).
{: note}

## Related links
{: #troubleshooting-related-link-par}

[About public address ranges](/docs/vpc?topic=vpc-about-par)
