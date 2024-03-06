---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-06"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations
{: #hub-spoke-planning-considerations}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

Before you configure hub and spoke DNS sharing for VPE gateways, make sure that you review the following considerations.
{: shortdesc}

## Hub considerations
{: #hub-considerations}

Review the following planning considerations for the hub VPC:

* The hub VPC and DNS-shared (spoke) VPCs must be in the same region.
* It's important to ensure connectivity is established between the hub VPC and DNS-shared VPCs. IBM Cloud does not manage or validate connectivity.
* You can designate a VPC as a hub when you create a VPC, or any time after it's created. When a VPC is designated as a hub, it cannot have any DNS resolution bindings.
* You can create more than one hub VPC in an account, as long as you don't have overlapping DNS-shared VPCs.
* You cannot cancel or delete the hub designation on a VPC if it has a DNS resolution binding to a DNS-shared VPC.
* You can initiate a DNS resolution binding only on a DNS-shared VPC.
* Only an authorized user can delete the DNS resolution binding from a hub VPC and a DNS-shared VPC. To delete the DNS resolution binding from a hub VPC, the user must be assigned the `is.vpc.dns-resolution-binding.delete` and `is.vpc.dns-resolution-binding.disconnect` IAM actions. You can assign the user with the `DNS Binding Connector` access policy and Administrator or Editor platform roles.

## DNS-shared VPC considerations
{: #spoke-considerations}

Review the following planning considerations for the DNS-shared (spoke) VPCs:

* You can create a DNS resolution binding to a hub VPC either by selecting from a list of VPCs (when the hub VPC exists in the same account), or by entering the hub VPC cloud resource name (CRN) if the hub VPC exists in another account.
* If DNS-shared VPCs contain VPE gateways that conflict with the DNS hub VPEs or any of the VPEs shared with its existing DNS-shared VPC's, the DNS-shared VPC must disable the DNS resolution binding for that VPE, before the DNS resolution binding is created. Likewise, if you try to create a conflicting VPE after the DNS resolution binding is already created.
* When the DNS-shared VPC has its resolver being delegated to the DNS hub VPC's custom resolver, disabling DNS sharing on an individual endpoint gateway in the DNS-shared VPC might cause DNS resolution failure on the DNS-shared VPC for this endpoint gateway.
* You cannot create more than one DNS resolution binding for a DNS-shared VPC.
* You can view the DNS resolution binding details to get information about the hub VPC and which endpoint gateways in the DNS-shared VPC have DNS sharing enabled.
* You cannot delete the DNS resolution binding on a DNS-shared (spoke) VPC if the DNS resolver type is set to Delegated. You must first update the DNS resolver type to System or Manual, and then delete the DNS resolution binding.
* For the DNS-shared VPC to use the delegated DNS server on the hub VPC, the hub VPC must run the custom resolver on its networks.
* You can enable or disable DNS sharing on each individual endpoint gateway in the DNS-shared VPC.
* You can configure the DNS resolver of the DNS-shared VPC to the custom resolver on the hub VPC.
* A DNS-shared VPC authorized user is able to change its DNS resolver to the custom resolver on the hub VPC. This is essential when the DNS-shared VPC attempts to connect to endpoint gateways on the hub VPC or any other DNS-shared VPCs sharing their DNS network to the same hub.
* A DNS-shared VPC authorized user can create or delete a DNS resolution binding, or change its DNS resolver to the custom resolver on the hub VPC. The DNS-shared authorized user must be granted the appropriate level of permission on both VPCs. The permission granted depends on whether the hub and DNS-shared VPCs are in the same account.
   * When the VPCs are in the same account, the authorized user must have an Operator role (minimum) on the DNS-shared VPC and a Viewer role on the hub VPC.
   * When the DNS hub VPC and DNS-shared VPC are in different accounts, then an "out-of-band" delegation process is required to create the VPC-to-VPC linkage between the DNS hub VPC and DNS-shared VPC. The DNS-shared VPC owner has to ask the DNS hub VPC owner to assign the Viewer role to the DNS-shared VPC to fulfill the delegation, which is achieved by the hub VPC owner when creating a cross-account IAM service-to-service authorization policy allowing the DNS-shared VPC to have read permission on the hub VPC.
* Custom resolver authorized users on the hub VPC can operate the custom resolver independently. Consequently, if the IP addresses of the custom resolver are updated, this is automatically propagated to all DNS-shared VPCs being delegated.
