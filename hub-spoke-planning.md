---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

 
# DNS sharing planning considerations
{: #vpe-dns-sharing-planning-considerations}

Before you configure DNS sharing for VPE gateways, make sure that you review the following considerations.
{: shortdesc}

## VPC Hub considerations
{: #hub-considerations}

Review the following planning considerations for the hub VPC:

* It's important to ensure you establish connectivity between the hub VPC and DNS-shared VPCs. IBM Cloud does not manage or validate connectivity.
* You can designate a VPC as a hub when you create a VPC, or any time after it's created. When you designate a VPC as a hub, it cannot have any DNS resolution bindings.
* You can create more than one hub VPC in an account, as long as you don't have overlapping DNS-shared VPCs. 
* You can't delete the hub VPC if it has DNS resolution bindings to DNS-shared VPCs.
* You can only initiate the creation of a DNS resolution binding from a DNS-shared VPC.
* Only an authorized user can delete the DNS resolution binding from a hub VPC and a DNS-shared VPC. To delete the DNS resolution binding from a hub VPC, the user must be assigned the `is.vpc.dns-resolution-binding.delete` and `is.vpc.dns-resolution-binding.disconnect` IAM actions. You can assign the user with the `DNSBindingConnector` access policy and Administrator or Editor platform roles.

## DNS-shared VPC considerations
{: #dns-shared-vpc-considerations}

Review the following planning considerations for the DNS-shared VPCs:

* You can assign a DNS resolution binding to a hub VPC either by selecting it from a list of VPCs (if the hub VPC exists in the same account), or by entering the hub VPC cloud resource name (CRN) (if the hub VPC exists in another account).
* If DNS-shared VPCs contain VPE gateways that conflict with the hub VPC VPEs or any of the VPEs shared with its existing DNS-shared VPCs, the DNS-shared VPC must disable the DNS resolution binding for that VPE, before it is created. Similar issues occur if you try to create a conflicting VPE after the DNS resolution binding is created.
* You can enable or disable DNS sharing on each individual endpoint gateway in the DNS-shared VPC.
* You can view the DNS resolution binding details to get information about the hub VPC, as well as which endpoint gateways in the DNS-shared VPC have DNS sharing enabled. 
* You cannot create more than one DNS resolution binding for a DNS-shared VPC.
* You cannot delete the DNS resolution binding on a DNS-shared VPC if the DNS resolver type is set to Delegated. You must first update the DNS resolver type to System or Manual, and then delete the DNS resolution binding.  
* For a DNS-shared VPC to use the delegated DNS server on the hub VPC, the hub must run a custom resolver on its networks. The DNS-shared VPC authorized user can configure its DNS resolver to that custom resolver.
* You can enable or disable DNS sharing on each individual endpoint gateway in the DNS-shared VPC.
* A DNS-shared VPC authorized user is able to change its DNS resolver to the custom resolver on the hub VPC. This is essential when the DNS-shared VPC attempts to connect to endpoint gateways on the hub VPC or any other DNS-shared VPCs sharing their DNS network to the same hub.

   * A DNS-shared VPC authorized user can create or delete a DNS resolution binding. The user must have appropriate permission levels on both VPCs:
   * The user must have `binding-create` permissions on the DNS-shared VPC.
   * The user must have `binding-connect` and read permissions on the DNS-hub VPC.

      When the DNS hub and DNS-shared VPCs are in different accounts, a delegation process is required to create a service-to-service policy that allows the DNSBinding service role from the DNS-shared VPC to access the DNS-hub VPC.

   * To change a DNS-shared VPC's DNS resolver to the custom resolver,  the authorized user must have an Operator role (minimum) on the DNS-shared VPC, and viewer role on the DNS hub VPC.

* Custom resolver authorized users on the hub VPC can operate the custom resolver independently. If its IP addresses change, they are automatically propagated to all delegated DNS-shared VPCs.
* IBM Cloud does not manage or control on-prem DNS servers; customers must manage their own on-prem DNS configuration.

## Limitations when configuring DNS sharing for VPE gateways
{: #vpe-dns-sharing-limitation}

Review the following limitations before you configure DNS sharing for VPE gateways:
{: shortdesc}

* The DNS hub VPC and DNS-shared VPCs must be in the same region. There is no support for DNS-shared VPCs in a remote region.
* VPEs on the DNS hub VPC are always shared with their associated DNS-shared VPCs. You must configure all VPEs on the DNS hub with `allow_dns_resolution` enabled before the VPC can be enabled as a DNS hub. 
* IBM Cloud does not manage or control on-prem DNS servers; customers must ensure on-prem servers point to the hub or spoke custom resolver as needed.
* When the hub VPC has DNS resolution bindings to DNS-shared VPCs, you cannot disable it as a DNS hub or delete the VPC.
* A DNS-shared VPC can only be associated to a single hub. 
* Zone affinity is not supported for a custom resolver when it has multiple addresses. One custom resolver address always becomes the primary DNS address for all availability zones for this VPC.
* You cannot disable or delete the custom resolver on the DNS hub VPC as long as it is in use.
* Timing requirement for binding operations: creating, deleting, enabling, or disabling DNS resolution bindings, including endpoint gateway binding changes, might fail if the operation completes in under 5 minutes. To avoid failure, ensure these operations take longer than 6 minutes. 
* If you remove and recreate the same VPE on any combination of hub or DNS-shared VPCs within a span of 5 minutes, the creation of the VPE might fail.

## Local-access VPE planning considerations
{: #local-access-vpe-considerations} 

[Select availability]{: tag-green}

When deploying local-access VPEs in a DNS-shared VPC:

* Configure DNS sharing before you create a local-access endpoint gateway. If you already have DNS sharing configured, no changes or migration are required to begin using a local-access endpoint gateway.
* Verify the service supports VPE. Currently, only Cloud Object Storage is supported. 
* Ensure that a VPE in the hub VPC exists for the service before creating a local-access VPE in the DNS-shared VPC.
* Check that the DNS-shared VPC has a DNS resolution binding to the hub.  
* Each local-access VPE can be created with specific resource bindings, which must be unique across the overall topology.
* Each DNS-shared VPC can contain only one local-access VPE per resource within the service endpoint.
* Ensure resource bindings for the DNS-shared VPC VPE are configured on the target service.
* Traffic for a resource (for example, an Object Storage bucket) bound to the local-access VPE is routed directly between the shared VPC and resource, bypassing the DNS hub VPC.
* The hub-VPC VPE is required to remain in place as long as any local-access VPEs exist for the same service endpoint.
* If `allow_dns_resolution_binding` is disabled on a VPE, DNS records are not propagated between the hub and DNS-shared VPCs.
* Make sure to update your CBR and security group rules to allow traffic from the appropriate DNS-shared VPC.

### VPE mode and resource-binding rules
{: #vpe-mode-resource-binding-rules}

[Select availability]{: tag-green}

This section outlines the different VPE modes and the rules for resource binding across various VPC configurations. Each VPE type has specific requirements regarding the VPE mode and whether resource binding is allowed. Understanding these rules is critical to ensuring proper network architecture and compliance with DNS-sharing policies. The following rules clarify which modes are permissible for each VPE type and the conditions under which resource bindings can be applied. 

**Note**: A “standalone VPC VPE” is a VPE in a VPC that is not part of the DNS hub and shared VPC topology.

* Hub VPC VPE
   * Hub VPC VPE should always be `primary` mode only; `per_resource_binding` and `disabled` modes are not allowed.
   * A hub-VPC VPE must exist for the service before creating any local-access VPE in a DNS-shared VPC.
   * The hub-VPC VPE is required to remain in place as long as any local-access VPEs exist for the same service endpoint.
* Standalone VPC VPE
   * Standalone VPC VPE can have `disabled` as the mode and have resource bindings.
   * Standalone VPC VPE can have `primary` as the mode and no resource bindings.
   * Standalone VPC VPE can have `per_resource_binding` as the mode and resource binding allowed.
* DNS-shared VPC VPE
   * DNS-shared VPC VPE can have `primary` as the mode and no resource bindings.
   * DNS-shared VPC VPE can have `disabled` as the mode and still have resource bindings.
   * DNS-shared VPC VPE can have `per_resource_binding` as the mode and resource binding allowed.

## Related links
{: #vpe-related-links-dns-sharing}

* [Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-vpe-planning-considerations)
* [FAQ for virtual private endpoint gateways](/docs/vpc?topic=vpc-faqs-vpe)
