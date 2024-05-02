---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-29"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# When updating the manual server for one zone with zone affinity, why are the manual servers of three zones updated?
{: #troubleshoot-hub-3}
{: troubleshoot}
{: support}

When you configure DNS sharing for Virtual Private Endpoint (VPE) gateways, you might encounter issues. Often, you can recover by following a few steps.
{: shortdesc}

When updating a manual DNS server with zone affinity enabled and only one IP address is provided for one zone in the API request, the manual DNS server is updated for all three zones with the same IP address.
{: tsSymptoms}

This is the expected behavior.
{: tsCauses}

If you want to update the manual DNS server for only one specific zone, you must provide the other manual DNS server IP addresses for the additional two zones in the request.
{: tsResolve}
