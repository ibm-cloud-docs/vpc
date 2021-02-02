---

copyright:
  years: 2020
lastupdated: "2020-08-31"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:term: .term}
{:beta: .beta}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# About {{site.data.keyword.cloud_notm}} {{site.data.keyword.nlb_full}}
{: #network-load-balancers}

You can use the {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) to distribute traffic among multiple server instances within the same region of your VPC.
{:shortdesc}

The following diagram illustrates the deployment architecture for a network load balancer.

![Network load balancer for VPC](images/nlb_arc.png "Network load balancer")
{: caption="Network load balancer" caption-side="top"}

The NLB is zonal, which means that the members must be in the same zone as the load balancer. Refer to [Multi-zone](/docs/vpc?topic=vpc-nlb-vs-elb#nlb-mz-support) for information on setting up a multi-zone network load balancers deployment.

## Getting started
{: #nlb-getting-started}

To configure a network load balancer, make sure that you have met the following prerequisites:

* If you don't have a VPC, create a VPC in the region that you want to create your NLB.
* Create a subnet in your preferred zone in your VPC.
* Create instances in the same zone as your network load balancer. Instances can be attached to your load balancer later. 

After you've completed all prerequisites, you can create your NLB. For more information, see [Creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer).

## Types of network load balancers
{: #types-network-load-balancers}  

{{site.data.keyword.nlb_full}} supports only public load balancers. A public load balancer is a load balancer where a publicly accessible IP address is registered with DNS.  

## Load-balancing methods
{: #network-load-balancing-methods}

Three load-balancing methods are available for distributing traffic across back-end application servers: round-robin, weighted round-robin, and least connections.

### Round-robin
{: #round-robin-method}

Round-robin is the default load-balancing method. With this method, the load balancer forwards incoming client connections in a round-robin fashion to the back-end servers. As a result, all back-end servers receive roughly an equal number of client connections.

### Weighted round-robin
{: #weighted-round-robin-method}

With this method, the load balancer forwards incoming client connections to the back-end servers in proportion to the weight assigned to these servers. Each server is assigned a default weight of `50`, which can be customized to any value in the range `0 - 100`.

For example, if application servers A, B, and C have the weights `60`, `60`, and `30`, then servers A and B receive an equal number of connections, while server C receives half that number of connections.

Setting a server weight to `0` means that no new connections are forwarded to that server, but any existing traffic continues to flow. Using a weight of `0` can help bring down a server gracefully and remove it from service rotation.
{: tip}

The server weight values are applicable only with the weighted round-robin method. They are ignored with the round-robin and least connections load-balancing methods.

### Least connections
{: #least-connections-method}

With this method, the back-end server instance that serves the least number of connections at a given time receives the next client connection.

## Use case: High traffic volume
{: #nlb-use-case-1}

The following diagram illustrates how {{site.data.keyword.cloud_notm}} Network Load Balancer supports sudden high volume inbound TCP requests. Notice that the network load balancer supports a static IP address, not proxies.

Often times a client might submit a request that is fairly small with little performance hit on the load balancer; however, the information that is returned from the back-end targets (VSI or container workloads) can be significant. With Direct Server Return (DSR), the information is sent directly back to the clients, thus minimizing latency and optimizing performance.

![Network load balancer traffic flow](images/nlb-use-case.png)

## Use case: Multi-zone network load balancer reference architecture
{: #nlb-use-case-2}

The following diagram illustrates how you can deploy {{site.data.keyword.cloud_notm}} {{site.data.keyword.nlb_full}} (NLB) to support multiple zones. This deployment scenario requires the use of the global load balancer option in [IBM Cloud Internet Services (CIS)](/docs/cis?topic=cis-configure-glb).

A common deployment that you might want to leverage is the NLB’s performance through Direct Server Return (DSR) and deploy the workloads in multiple zones to increase their availability. Combining the network and global load balancers can provide this capability.

![Multi-zone network load balancer](images/nlb_glb.png)

## Front-end listeners and back-end pools
{: #nlb-front-end-listeners-and-back-end-pools}

You can define up to ten front-end listeners (application ports) and map them to back-end pools on the back-end application servers. The FQDN assigned to your load balancer and the front-end listener ports are exposed to the public internet. Incoming user requests are received on these ports. TCP is the supported protocol for front-end listeners and back-end pools.

You can attach up to 50 virtual server instances to a back-end pool. Traffic is sent to each instance on its specified data port. This data port does not need to be the same as the front-end listener port.

## VPC representation of a network load balancer
{: #vpc-nlb-representation}

The following diagram shows the VPC representation of a typical network load balancer setup. The NLB is provisioned on a VPC subnet. To configure the network data path on the network load balancer, a listener, a pool, and at least one member must be created. A listener is the front-end port that the network load balancer is listening on for customer requests. These requests are forwarded to the targets in the pool that is associated to the listener. A pool is a group of targets that are used to distribute the network requests coming into the network load balancer for a specific listener. A member is the back-end target and the port that the target is listening for request.

![Network load balancer work flow](images/nlb-workflow-customer-view.png "Network load balancer work flow"){: caption="Network load balancer work flow" caption-side="top"}

## Layer-4 load balancing
{: #nlb-layer4}

{{site.data.keyword.nlb_full}} provides a Layer-4 (known as the transport layer) load-balancing service to user’s servers in VPC. It decides where traffic should be directed based on the source and destination IP addresses and the port in the packed header. The load balancer does not perform a check on the contents of the packet.

Since Layer-4 load balancing requires fewer computations compared to more sophisticated load balancing, such as Layer-7, CPU usage and memory are more efficiently used.

## Related links
{: #nlb-permissions-related-links}

* [Network load balancer CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#nlb-anchor)
* [Load balancer API reference](https://{DomainName}/apidocs/vpc#list-load-balancer-profiles)
* [Network load balancer infrastructure resources for Terraform](/docs/terraform?topic=terraform-vpc-gen2-resources#lb)
* [Network load balancer in {{site.data.keyword.cloud}} Kubernetes Service](/docs/containers?topic=containers-vpc-lbaas#nlb_vpc)
* [Required permissions for VPC resources](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls)
* [Activity Tracker with LogDNA events](/docs/vpc?topic=vpc-at-events#events-load-balancers)
* [FAQs for network load balancers](/docs/vpc?topic=vpc-nlb-faqs)
* [Quotas](/docs/vpc?topic=vpc-quotas#load-balancer-quotas)
