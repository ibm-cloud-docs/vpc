---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-19"

keywords: dedicated-host.00001

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did a dedicated host fail to create?
{: #why-did-a-dedicated-host-fail-to-create}
{: troubleshoot}
{: support}

If you see an entry in the error log with message ID `dedicated-host.00001`, then the specified dedicated host creation failed due to insufficient capacity in the requested region. 
{: shortdesc}

The dedicated host with the specified ID in the error message failed to create:
   `dedicated-host.00001: Failed to create dedicated host <Dedicated Host ID> due to insufficient capacity in zone.`
{: tsSymptoms}

The specified dedicated host could not be created due to a lack of capacity in the requested region.
{: tsCauses}

At the time of the error message, the requested region did not have enough capacity to create the specified dedicated host. Try creating another dedicated host with different hardware requirements or in another region.
{: tsResolve}
