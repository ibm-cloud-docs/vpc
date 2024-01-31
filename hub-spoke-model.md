---

copyright:
  years: 2023
lastupdated: "2023-09-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About DNS sharing for VPE gateways
{: #hub-spoke-model}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

Large-scale enterprise customers may need network-level isolation of resources used by different business units, workloads, or environments. This kind of isolation enables granular control over network traffic for each group of resources. It can also help meet legal and regulatory data-separation requirements. Configuring a hub and spoke (DNS-shared) network is one way to efficiently manage your communication services as well as meet security requirements at scale.
{: shortdesc}

With this type of network, your VPCs can be in different accounts in the enterprise. The Virtual Private Endpoint (VPE) gateways in the DNS hub and DNS-shared VPCs must be accessible to each other and from on-prem networks. Connecting a VPC hub to the DNS-shared VPCs through a transit gateway is straight forward, but without additional configuration, there will be no DNS resolution for the VPE services.

VPC private DNS resolution works within the same VPC, but it requires complex scripts manually copied to other VPCs or on-prem environments. Configuring DNS sharing for VPEs simplifies this scenario by allowing DNS resolution of services regardless of location. Complex scripts are not required to maintain copies of DNS records and it is easier to leverage the fine-grain data access control provided by a VPC to protect access to catalog services.

## Getting started
{: #hub-spoke-process}

When multiple VPCs are connected together using transit gateways, direct links, or other connectivity options, a VPC in the connected topology can be enabled as a hub to centralize the DNS resolution for VPCs, including resolution for all endpoint gateways from all VPCs in the topology. General steps to configure DNS sharing for VPE gateways are as follows:

1. Ensure that you create or have existing VPCs to use for your hub VPC and DNS-shared VPCs. These VPCs can be in multiple accounts. These VPCs must already be configured using a transit gateway or other datapath connectivity.

   For information about creating a VPC, see [Using the IBM Cloud console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console){: external} or [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=api).
{: note}

1. [Enable a VPC as the hub VPC](/docs/vpc?topic=vpc-hub-spoke-configure-hub).
1. [Create endpoint gateways](/docs/vpc?topic=vpc-ordering-endpoint-gateway) for multi-tenant services in the hub VPC and enable DNS sharing for each endpoint gateway.
1. If you have different accounts for the hub VPC and a DNS-shared VPC, the hub VPC administrator must create a service-to-service (s2s) authorization policy to give Read access to the DNS-shared VPC account. For more information, see [Establishing service-to-service authorization](/docs/vpc?topic=vpc-hub-spoke-s2s-auth&interface=api).
1. Ensure that DNS resolution binding (`allow_dns_resolution`) is disabled on redundant or conflicting VPEs on the DNS-shared VPCs. By default, the DNS resolution binding switch is enabled on VPEs.
1. [Create a DNS resolution binding](/docs/vpc?topic=vpc-hub-spoke-resolution-bindings&interface=ui) on the VPCs on which you want to allow VPE gateway DNS records between the VPC and the DNS hub VPC. These VPCs are called DNS-shared VPCs.
1. [Configure a DNS custom resolver](/docs/dns-svcs?topic=dns-svcs-ui-create-cr) on the hub VPC to be responsible for resolving DNS queries from hub and DNS-shared VPCs, as well as those from on-prem networks.
1. The DNS-shared VPC user [sets its DNS resolver type](/docs/vpc?topic=vpc-hub-spoke-configure-dns-resolver&interface=ui) to Delegated to point to the hub VPC's custom resolver.
1. The DNS-shared VPC user [creates endpoint gateways](/docs/vpc?topic=vpc-ordering-endpoint-gateway) for single-tenant services in the DNS-shared VPC.
1. For each DNS-shared VPC, the DNS-shared VPC user must repeat steps 4-9.

## Use case: DNS sharing in a hub and spoke network topology
{: #hub-spoke-use-cases}

In an enterprise environment, managing access to IBM services using Virtual Private Endpoints (VPEs), spread over multiple accounts, VPCs, and organizations can be an overwhelming task. Connecting to these services from on-premises can add complexity and challenges for a network administrator. This use case showcases IBM’s innovative approach to solve this complex problem by simplifying network topology and DNS management.

The following customer has three applications, which they want to isolate on separate VPCs, but maintain a common connection to their on-prem infrastructure. The customer routes all traffic through their on-premises, and relies on private connectivity through VPE for access to IBM services.

Each of the application VPCs contains an IBM Cloud Kubernetes Service (IKS) cluster and an IBM Cloud Databases (ICD) database. The VPCs contain VPEs to each of these services.

Each of these VPCs also relies on common IBM services, such as IBM Cloud Object Storage (COS) for backups, IBM Cloud Identity and Access Management (IAM) for authorization, IBM Cloud Business Support Server (BSS) for billing, and Container Registry for managing the application images. The customer also uses these services through VPE for private connectivity.

While the network connectivity between VPCs is already provided by Transit Gateway, by default the DNS resolution of VPE DNS records is not resolvable by each other. Without DNS sharing for VPE, management of the VPE is overwhelming, as the customer needs to manage:

* Managing deploy of multiple IBM service VPCS for each of their application VPCs, including managing tracking of application dependancies.
* Manage security (security groups, network ACLs, and so on) on a per-VPC basic.
* Manage DNS between each VPC and on-premise, potentially using custom resolver.

Figure 1 shows how the customer can use DNS sharing to simplify their deployment and operations in context of hub and spoke architecture. Here, the customer has used DNS sharing to consolidate:

- All common IBM service VPEs are created one time in the DNS hub VPC and shared by all other VPCs. The customer no longer requires common VPEs on each application VPC, and can instead manage these centrally.
- All DNS traffic through a single DNS hub VPC, which can centralize the deployment of security groups and network ACLs.
- All DNS is consolidated into a single custom resolver to manage access for all VPEs from both the DNS hub VPC and the application VPCs. The high availability of the custom resolver can be achieved by specifying resolver appliances in different locations. This custom resolver can also be used to configure rules of connecting to on-premises.
- The customer's applications continue to be deployed within the application VPCs.

![hub-spoke-use-case](/images/hub-spoke-use-case1.png){: caption="Figure 1. Use case" caption-side="bottom"}

To achieve this hub and spoke architecture with DNS sharing enabled for VPE gateways, a customer can following these steps to configure their hub and DNS-shared (spoke) VPCs.

For the VPC that is used as a DNS hub:

* Customer creates VPE gateway for common IBM services, such as IAM, billing, Container Registry, and so on.
* Customer creates a DNS custom resolver on the VPC.
* Customer designates this VPC as a DNS hub.

For each DNS-shared VPC:

* Customer creates VPE gateways for application services, such as IKS and ICD instances (as needed).
* Customer creates a DNS resolution binding to the DNS hub VPC to share its VPE DNS records to the DNS hub VPC.
* Customer configures its DNS to use the custom resolver on a DNS hub VPC by changing its resolver type to Delegated with the DNS hub VPC.

## Related links
{: #dns-hub-spoke-related-links}

* [Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-dns-resolution-bindings)
* [IAM permissions](/docs/vpc?topic=vpc-at-events#events-dns-resolution-bindings)
* [Troubleshooting issues](/docs/vpc?topic=vpc-troubleshoot-hub-spoke-1)
