---

copyright:
  years: 2020
lastupdated: "2020-06-29"

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

# About {{site.data.keyword.cloud_notm}} Network Load Balancer
{: #network-load-balancers}

You can use the {{site.data.keyword.cloud}} Network Load Balancer to distribute traffic among multiple server instances within the same region of your VPC.
{:shortdesc}

The beta release of IBM Cloud Network Load Balancer is only available to allowlisted users. Contact your IBM Cloud Sales representative if you are interested in getting early access to this beta offering. When network load balancer is made generally available, you'll need to change to a Standard plan to continue using the instances you created during the Beta. Any instances that continue to use a Beta plan for this service beyond 30 days notice of general availability are deleted. See the [{{site.data.keyword.cloud_notm}} Service Description](https://www.ibm.com/software/sla/sladb.nsf/pdf/6605-19/$file/i126-6605-19_10-2019_en_US.pdf){: external} and [IBM VPC Service Description](https://www.ibm.com/software/sla/sladb.nsf/pdf/8265-02/$file/i126-8265-02_07-2019_en_US.pdf){: external} for more information about Beta services.
{: beta}

## Use cases
{: #nlb-use-cases}

### Use case 1: High traffic volume
The following diagram illustrates how IBM Cloud Network Load Balancer supports sudden high volume inbound TCP requests. Notice that the network load balancer supports a static IP address, not proxies.

Often times a client might submit a request that is fairly small with little performance hit on the load balancer; however, the information returned from the backend targets (VSI or container workloads) can be significant. With Direct Server Return (DSR), the information is sent directly back to the clients, thus minimizing latency and optimizing performance.

![Network load balancer traffic flow](images/nlb-use-case.png)

## Creating a network load balancer  
{: #ordering-nlb}

To order and start using the {{site.data.keyword.cloud_notm}} Network Load Balancer, refer to [these instructions](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#nlb-ui).

## Types of network load balancers
{: #types-network-load-balancers}  

The network load balancer only supports public load balancers. A public load balancer is a load balancer where a publicly accessible IP address is registered with DNS.  

## Load balancing methods
{: #network-load-balancing-methods}

There are 3 load balancing methods available for distributing traffic across back-end application servers:

### Round-robin
{: #round-robin-method}

Round-robin is the default load-balancing method. With this method, the load balancer forwards incoming client connections in round-robin fashion to the back-end servers. As a result, all back-end servers receive roughly an equal number of client connections.

### Weighted round-robin
{: #weighted-round-robin-method}

With this method, the load balancer forwards incoming client connections to the back-end servers in proportion to the weight assigned to these servers. Each server is assigned a default weight of `50`, which can be customized to any value in the range `0 - 100`.

  For example, if application servers A, B, and C have the weights `60`, `60`, and `30`, then servers A and B receive an equal number of connections, while server C receives half that number of connections.

  Setting a server weight to `0` means that no new connections are forwarded to that server, but any existing traffic continues to flow. Using a weight of `0` can help bring down a server gracefully and remove it from service rotation.
  {: tip}

  The server weight values are applicable only with the weighted round-robin method. They are ignored with round-robin and least connections, load-balancing methods.

### Least connections
{: #least-connections-method}

With this method, the back-end server instance that serves the least number of connections at a given time receives the next client connection.

## Front-end listeners and back-end pools
{: #nlb-front-end-listeners-and-back-end-pools}

You can define up to ten front-end listeners (application ports) and map them to back-end pools on the back-end application servers. The FQDN assigned to your load balancer and the front-end listener ports are exposed to the public internet. Incoming user requests are received on these ports. TCP is the supported protocol for front-end listeners and back-end pools.

You can attach up to 50 virtual server instances to a back-end pool. Traffic is sent to each instance on its specified data port. This data port does not need to be the same as the front-end listener port.

Ports `56500 - 56520` cannot be used as front-end listener ports. These are reserved for load balancer management purposes.
{: important}

## Network load balancer data flow path
{: #nlb-data-flow}

![Network load balancer traffic flow](images/nlb-datapath.png)

## Configuring ACLs for use with network load balancers
{: #nlb-configuring-acls}

If you use access control lists (ACLs) and security groups (SGs) to block traffic on the subnets where you deploy the load balancer, make sure that ACL and SG rules are in place to allow incoming traffic for the configured listener ports and management ports (ports `56500 - 56520`). You must also allow traffic between the load balancer and back-end instances.

## Network load balancer limitations
{: #nlb-limitations}

Known limitations for network load balancer are as follows:

* The network load balancer requires the member and port combination to be unique. In other words, a member with instance X and port Y can not be added to a pool if a member with instance X and port Y already exists in another pool.
* There is a one-to-one mapping between listener and pool.
* All members associated with a network load balancer must be in the same zone as the load balancer.
* Two members with the same instance X and same port Y cannot exist at the same time for application and network load balancer. This case is not supported and your traffic might not be routed correctly.

## Layer-4 load balancing
{: #nlb-layer4}
Network Load balancer provides a Layer-4 (known as the transport layer) load balancing service to user’s servers in VPC. It makes decision based on the source and destination IP addressed and port in the packed header, and no check will be performed on the contents of the packet. Then, this Layer-4 load balancer requests less computation comparing to another sophisticated load balancer, such as Layer-7. The usage of CPU and memory will be more sufficient and faster.  

## Related links
{: #nlb-permissions-related-links}

* [Network load balancer CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#nlb-anchor)
* [Load balancer API reference](https://{DomainName}/apidocs/nlb-beta)
* [Required permissions for VPC resources](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls)
* [Activity Tracker with LogDNA events](/docs/vpc?topic=vpc-at-events)
* [FAQs for network load balancers](/docs/vpc?topic=vpc-nlb-faqs)
