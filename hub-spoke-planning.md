---

copyright:
  years: 2023, 2025
lastupdated: "2025-11-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

 
# DNS sharing planning considerations
{: #vpe-dns-sharing-planning-considerations}

Before you configure DNS sharing for VPE gateways, make sure that you review the following considerations.
{: shortdesc}

## Hub considerations
{: #hub-considerations}

Review the following planning considerations for the hub VPC:

* It's important to ensure you establish connectivity between the hub VPC and DNS-shared VPCs. IBM Cloud does not manage or validate connectivity.
* You can designate a VPC as a hub when you create a VPC, or any time after it's created. When you designate a VPC as a hub, it cannot have any DNS resolution bindings.
* You can create more than one hub VPC in an account, as long as you don't have overlapping DNS-shared VPCs. 
* You cannot delete the hub VPC if it has DNS resolution bindings to DNS-shared VPCs.
* You can only initiate the creation of a DNS resolution binding from a DNS-shared VPC.
* Only an authorized user can delete the DNS resolution binding from a hub VPC and a DNS-shared VPC. To delete the DNS resolution binding from a hub VPC, the user must be assigned the `is.vpc.dns-resolution-binding.delete` and `is.vpc.dns-resolution-binding.disconnect` IAM actions. You can assign the user with the `DNSBindingConnector` access policy and Administrator or Editor platform roles.

## DNS-shared VPC considerations
{: #dns-shared-vpc-considerations}

Review the following planning considerations for the DNS-shared VPCs:

* You can assign a DNS resolution binding to a hub VPC either by selecting it from a list of VPCs (if the hub VPC exists in the same account), or by entering the hub VPC cloud resource name (CRN) (if the hub VPC exists in another account).
* If DNS-shared VPCs contain VPE gateways that conflict with the DNS hub VPEs or any of the VPEs shared with its existing DNS-shared VPCs, the DNS-shared VPC must disable the DNS resolution binding for that VPE, before it is created. You will have similar issues if you try to create a conflicting VPE after the DNS resolution binding is already created.
* You can enable or disable DNS sharing on each individual endpoint gateway in the DNS-shared VPC. When enabling or disabling DNS resolution binding for endpoint gateways, if the process takes less than 5 minutes, it will fail. To fix this issue, ensure the binding process takes longer than 6 minutes.
* You can view the DNS resolution binding details to get information about the hub VPC, as well as which endpoint gateways in the DNS-shared VPC have DNS sharing enabled. 
* You cannot create more than one DNS resolution binding for a DNS-shared VPC.
* You cannot delete the DNS resolution binding on a DNS-shared VPC if the DNS resolver type is set to Delegated. You must first update the DNS resolver type to System or Manual, and then delete the DNS resolution binding.  
* For a DNS-shared VPC to use the delegated DNS server on the hub VPC, the hub must run a custom resolver on its networks. The DNS-shared VPC authorized user can configure its DNS resolver to that custom resolver.
* You can enable or disable DNS sharing on each individual endpoint gateway in the DNS-shared VPC.
* A DNS-shared VPC authorized user is able to change its DNS resolver to the custom resolver on the hub VPC. This is essential when the DNS-shared VPC attempts to connect to endpoint gateways on the hub VPC or any other DNS-shared VPCs sharing their DNS network to the same hub.

   * A DNS-shared VPC authorized user can create or delete a DNS resolution binding. The user must have appropriate permission levels on both VPCs:
   * The user must have binding-create permissions on the DNS-shared VPC.
   * The user must have binding-connect and read permissions on the DNS-hub VPC.

      When the DNS hub and DNS-shared VPCs are in different accounts, a delegation process is required to create a service-to-service policy, allowing the DNSBinding service role from the DNS-shared VPC to the DNS-hub VPC.

   * To change a DNS-shared VPC's DNS resolver to the custom resolver,  the authorized user must have an Operator role (minimum) on the DNS-shared VPC, and viewer role on the DNS-Hub VPC.

* Custom resolver authorized users on the hub VPC can operate the custom resolver independently. As a result, if the IP addresses of the custom resolver are updated, this automatically propagates to all delegated DNS-shared VPCs.

You are responsible for designing and managing your own architecture among multiple VPCs and accounts. IBM Cloud does not manage or control on-prem DNS servers.
{: note}

## Limitations when configuring DNS sharing for VPE gateways
{: #vpe-dns-sharing-limitation}

Review the following limitations before you configure DNS sharing for VPE gateways:
{: shortdesc}

* The DNS-Hub VPC and DNS-Shared VPCs must be in the same region. There is no support for DNS-Shared VPCs in a remote region.
* VPEs on the DNS hub VPC will always be shared with their associated DNS-shared VPCs. You must configure all VPEs on the DNS hub with `allow_dns_resolution` enabled before the VPC can be enabled as a DNS hub. 
* IBM does not manage or control on-prem DNS servers. It is the customer's responsibility to configure them to point at the hub and spoke custom resolver appropriately. 
* When the hub VPC has DNS resolution bindings to DNS-shared VPCs, you cannot disable it as a DNS hub or delete the VPC.
* A DNS-shared VPC can only be associated to a single hub. 
* Zone affinity is not supported for a custom resolver when it has multiple addresses. One custom resolver address always becomes the primary DNS address for all availability zones for this VPC.
* You cannot disable or delete the custom resolver on the DNS hub VPC as long as it is in use.
* When deleting or recreating a DNS resolution binding, if the process takes less than 5 minutes, it will fail. To fix this issue, ensure the binding process takes longer than 6 minutes.
* When disabling or enabling DNS resolution binding for endpoint gateways, if the process takes less than 5 minutes, it will fail. To fix this issue, ensure the binding process takes longer than 6 minutes.
* If you remove and recreate the same VPE on any combination of hub or DNS-shared VPCs within a span of 5 minutes, the creation of the VPE may fail.
