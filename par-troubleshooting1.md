---

copyright:
  years: 2025
lastupdated: "2025-09-18"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn’t traffic reaching my VNF appliance with a public address range?
{: #troubleshoot-public-address-ranges-traffic}
{: troubleshoot}
{: support}

You can now create and use public address ranges in the Frankfurt and Madrid regions, if your account has been approved for access. To request access to Public Address Ranges for VPC, contact your IBM representative.
{: preview}

When using Public Address Ranges for VPC, traffic might fail to flow as expected due to issues in either the control path or the data path. These issues can manifest during the attachment of the public address range or during runtime routing of traffic to/from the VNF appliance.
{: shortdesc}

You might notice that traffic intended for a VNF appliance does not reach its destination or response traffic is dropped. The problem typically presents in two ways:
{: tsSymptoms}

* If a control path issue, the lifecycle state is not stable, often showing Failed, Pending, or Updating status.
* If a data path issue, the lifecycle state of the  range is stable and appears correctly attached, but traffic is not routed or forwarded as expected.
 
Routing issues with a  range can arise from improper lifecycle management (attach failure or misconfiguration) or misconfigured routing rules or next-hop appliances, even when the public address range appears successful. 
{: tsCauses}

Follow these steps to troubleshoot this issue:
{: tsResolve} 

1. Verify the public address range lifecycle state:

   1. Check if the public address range is in a stable state using the IBM Cloud console or CLI.
   1. If not in a stable state, it's a control path issue. Retry the attach process or check for configuration or IAM permission errors.
1. Validate the routing configuration:

   1. If the public address range is stable, ensure that the route table correctly maps the public address range to the VNF appliance as the next hop.
   1. Confirm that public ingress routing is set up properly and that the VNF is reachable.
1. Verify that the VNF appliance is correctly processing traffic, preserving source IPs on egress, and that firewall/security rules allow the traffic.
1. If this issue persists, gather logs and routing details and [contact IBM Cloud support](/docs/account?topic=account-open-case&interface=ui).

## Related links
{: #ts-related-links-par}

[About public address ranges](/docs/vpc?topic=vpc-about-par)
