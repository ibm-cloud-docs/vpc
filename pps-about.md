---

copyright:
  years: 2023, 2025
lastupdated: "2025-01-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Private Path services
{: #private-path-service-intro}

Private Path services provide private connectivity for {{site.data.keyword.cloud_notm}} and third-party services. A Private Path service requires a Private Path network load balancer to deploy a service on IBM Cloud and a Virtual Private Endpoint (VPE) gateway for consumers to connect to the service. Traffic stays on the IBM backbone without traversing the internet.
{: shortdesc}

The typical process for creating private connectivity between Providers and Consumers is as follows:

 1. The provider creates a Private Path service.
 1. The provider associates their Private Path service with a Private Path NLB.
 1. The provider shares pertinent information with service consumers, including a unique Private Path service Cloud Resource Name (CRN).
 1. The consumer creates a VPE gateway that configures the Private Path service's CRN. In turn, a connection request is sent to the service Provider.
 1. The provider permits or denies the consumer's request and sets up an account policy, if need be (alternatively, the provider can set up an account policy to automatically permit or deny consumer requests).
 1. The consumer is notified of the status of the connection request. If permitted, the consumer can access the service; if denied, the consumer can contact the provider for further details.

For more information, see the [Private Path solution guide](/docs/private-path).

Your ability to complete the following actions depends on the level of IAM permissions that are associated with your IBM Cloud account. For more information, see [Required permissions](/docs/account?topic=account-iam-service-roles-actions#is.private-path-service-gateway-roles).

## Getting started with Private Path service
{: #pps-getting-started}

As a service provider, follow these steps to get started:

1. Make sure that you have a Virtual Private Cloud (VPC) and at least one subnet in the selected VPC.
1. Create a Private Path NLB.

   * You can create a Private Path NLB when you create your Private Path service, or you can use the [Load balancer for VPC](/infrastructure/provision/loadBalancer) provisioning page to create one. To create a Private Path load balancer separate from the Private Path service, see [Creating a Private Path network load balancer](/docs/vpc?topic=vpc-ppnlb-ui-creating-private-path-network-load-balancer&interface=ui).
   * You must use the same account within the same VPC region for your Private Path NLB and Private Path service.

1. [Create a Private Path service](/docs/vpc?topic=vpc-private-path-service-about&interface=ui).

   * Set the default policy for when an account doesn’t have a specific policy that is assigned to it. The default policy (**Review**) allows you to permit or deny each request, whereas **Permit** and **Deny** automate the process for connection requests without specific account policies.
   * Create account policies for specific account IDs now or later. These policies determine what action to take when the provider receives a request from a specific account, and take precedence over the default policy.

## Private Path service use cases
{: #pps-use-cases}

The following use cases show you the various ways you can use Private Path services. 

### Use case 1: Connecting a service to a single consumer
{: #pps-use-case-1}

As a Provider, you want to connect your service to a Consumer without traffic traversing the internet and without giving access to your entire VPC. Your Consumer can be a customer, other division in your company, or something else.

Figure 1 illustrates how to establish a Private Path service. Establishing a Private Path service enables you to expose a service to a customer privately.

First, a Consumer's application connects to a VPE gateway in the Consumer's VPC. Then, the VPE gateway connects to the Private Path NLB in the Provider's VPC. In turn, the Private Path NLB connects to the Provider's service. The Provider's service then responds to the Consumer's request through Direct Server Return (DSR). This Private Path service activity is completely contained in a single region (US South) in an IBM Cloud private network.

![A Private Path exposing a service to a customer](images/private_path_use_case_1.svg "A Private Path exposing a service to a customer"){: caption="A Private Path exposing a service to a customer without using public internet" caption-side="bottom"}

### Use case 2: Connecting a service to multiple consumers
{: #pps-use-case-2}

Figure 2 illustrates how to establish a Private Path service with connections to multiple Consumers VPE gateways.

First, a Consumer's application connects to a VPE gateway in the Consumer's VPCs. Then, the VPE gateway connects to the Private Path NLB in the Provider's VPC. In turn, the Private Path NLB connects to the Provider's service. The Provider's service then responds to the Consumer's request through DSR. This Private Path service activity is completely contained in a single region (US South) in an IBM Cloud private network.

![A Private Path exposing a service to multiple customers](images/private_path_detailed_2.svg "A Private Path exposing a service to multiple customers"){: caption="A Private Path exposing a service to multiple customers without using public internet" caption-side="bottom"}

### Use case 3: Connecting a service to a customer within your VPC
{: #pps-use-case-3}

Figure 3 illustrates how to establish a Private Path service with connections to the VPE gateway of a Consumer within your VPC.

First, a Consumer's application connects to the Consumer's VPE gateway within the Provider's VPC. Then, the VPE gateway connects to the Private Path NLB in the Provider's VPC. In turn, the Private Path NLB connects to the Provider's service. The Provider's service then responds to the Consumer's request through DSR. This Private Path service activity is completely contained in a single region (US South) in an IBM Cloud private network.

![A Private Path exposing a service to a customer within the same VPC](images/private_path_detailed_3.svg "A Private Path exposing a service to a customer within the same VPC"){: caption="A Private Path exposing a service to a customer within the same VPC without using public internet" caption-side="bottom"}

### Use case 4: Enabling an IBM Cloud service to connect to a provider's VPC
{: #pps-use-case-4}

Private Path allows connection between an IBM Cloud service like IBM Cloud Code Engine and your VPC without compromising security or putting your VPC at risk. Code Engine is a multi-tenant compute service that runs source-code or containerized workloads. Its dynamic scaling capabilities allow your apps to automatically scale up and down, even to zero, based on incoming requests. With it’s pay-per-use model, Code Engine only charges for the compute capacity you actually use. For more information, see [IBM Cloud Code Engine](https://www.ibm.com/products/code-engine).

Figure 4 illustrates how to establish a Private Path service with connections to the VPE gateway of a Code Engine application and your VPC. First, the Code Engine application connects to the VPE gateway within the Code Engine's VPC. Then, the VPE gateway connects to the Private Path NLB in the Provider's VPC. In turn, the Private Path NLB connects to the Provider's application. The Provider's application then responds to the request. This Private Path service activity is completely contained in a single region (US South) in an IBM Cloud private network.

![Use Code Engine and Private Path to deploy complex architecture with dynamic and static scaling needs](images/private_path_detailed_4.svg "Use Code Engine and Private Path to deploy complex architecture with dynamic and static scaling needs"){: caption="Use Code Engine and Private Path to deploy complex architecture with dynamic and static scaling needs" caption-side="bottom"}  

## Related links
{: #pps-related-links}

These links provide additional information about Private Path services for VPC:

* [Quotas and service limits](/docs/vpc?topic=vpc-quotas)
* [Private Path services CLI reference](/docs/vpc?topic=vpc-vpc-reference)
* [Private Path services API reference](/apidocs/vpc/latest#list-private-path-service-gateways)
* [Private Path services resources for Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_private_path_service_gateway){: external}
* [Private Path solution tutorial using Terraform](/docs/solution-tutorials?topic=solution-tutorials-vpc-pps-basics)

* [IAM permissions](/docs/account?topic=account-iam-service-roles-actions#is.private-path-service-gateway-roles)
* [Activity Tracker events](/docs/vpc?topic=vpc-at_events#events-private-path-service-events)
