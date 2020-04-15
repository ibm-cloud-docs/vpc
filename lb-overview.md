---

copyright:
  years: 2018, 2020
lastupdated: "2020-04-07"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, layer-7

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Using load balancers
{: #load-balancers}

Use the {{site.data.keyword.cloud}} Load Balancer for VPC service to distribute traffic among multiple server instances within the same region of your VPC.
{:shortdesc}

## Types of load balancers
{: #types-load-balancer}

You can create a public or private load balancer. Table 1 shows a summary comparison of features.

| Feature | Public Load Balancer | Private Load Balancer |
|--------|-------|-------|
| Accessible on internet? |  Yes, with a fully qualified domain name (FQDN) | No, internal clients only, on same region and VPC |
| Accepts all traffic? | Yes | No, RFC 1918 traffic only |
| How is domain name registered? | Public IP addresses | Private IP addresses |
{: caption="Table 1. Comparison of public and private load balancers" caption-side="top"}

### Public load balancer
{: #public-load-balancer}

A public load balancer service instance is assigned a publicly accessible FQDN, which you must use to access your applications that are hosted behind the load balancer. This domain name can be registered with one or more public IP addresses.

Over time, the number and value of these public IP addresses might change due to maintenance and scaling activities. The back-end virtual server instances (VSIs) hosting your application must run in the same region, and under the same VPC.

Use the assigned FQDN to send traffic to the public load balancer to avoid connectivity problems to your applications during system maintenance or scaling down activities.
{: important}

### Private load balancer
{: #private-load-balancer}

The private load balancer is accessible only to internal clients on your private subnets, within the same region and VPC. The private load balancer accepts traffic only from [RFC1918](https://tools.ietf.org/html/rfc1918){: external} address spaces.

Similar to a public load balancer, your private load balancer service instance is assigned a fully qualified domain name (FQDN). However, this domain name is registered with one or more private IP addresses.

{{site.data.keyword.cloud_notm}} operations might change the number and value of your assigned private IP addresses over time, based on maintenance and scaling activities. The back-end virtual server instances (VSIs) hosting your application must run in the same region, and under the same VPC.

Use the assigned FQDN to send traffic to the private load balancer to avoid connectivity problems to your applications during system maintenance or scaling down activities.
{: important}

## Front-end listeners and back-end pools
{: #front-end-listeners-and-back-end-pools}

You can define up to 10 front-end listeners (application ports) and map them to back-end pools on the back-end application servers. The fully qualified domain name (FQDN) assigned to your load balancer and the front-end listener ports are exposed to the public internet. Incoming user requests are received on these ports.

The supported front-end listener protocols are:
* HTTP
* HTTPS
* TCP

The supported back-end pool protocols are:
* HTTP
* TCP

Incoming HTTPS traffic is terminated at the load balancer to allow for plain-text HTTP communication with the back-end server.

You can attach up to 50 virtual server instances to a back-end pool. Traffic is sent to each instance on its specified data port. This data port doesn't need to be the same as the front-end listener port.

Ports 56500 - 56520 can't be used as front-end listener ports because they are reserved for management purposes.
{: important}

## Load balancing methods
{: #load-balancing-methods}

Three load balancing methods are available for distributing traffic across the back-end application servers:

### Round-robin
{: #round-robin}
Round-robin is the default load balancing method. With this method, the load balancer forwards incoming client connections in round-robin fashion to the back-end servers. As a result, all back-end servers receive roughly an equal number of client connections.

### Weighted Round-robin
{: #weighted-round-robin}
With this method, the load balancer forwards incoming client connections to the back-end servers in proportion to the weight assigned to these servers. Each server is assigned a default weight of 50, which can be customized to any value in the range 0 - 100.

For example, if application servers A, B and C have the weights 60, 60, and 30, then servers A and B receive an equal number of connections, while server C receives half that number of connections.

Setting a server weight to '0' means that no new connections are forwarded to that server, but any existing traffic continues to flow. Using a weight of '0' can help bring down a server gracefully and remove it from service rotation.
{: tip}

The server weight values are applicable only with the weighted round-robin method. They are ignored with round-robin and least-connections load balancing methods.

### Least connections
{: #least-connections}

With this method, the back-end server instance that serves the least number of connections at a given time receives the next client connection.

## Horizontal scaling
{: #horizontal-scaling}

The load balancer adjusts its capacity automatically according to the load. When this adjustment occurs, you may see a change in the number of IP addresses associated with the load balancer's DNS name.

## Health checks
{: #health-checks}

Health check definitions are mandatory for back-end pools. Health checks can be configured on back-end ports or on a separate health check port, based on the application.

The load balancer conducts periodic health checks to monitor the health of the back-end ports, and it forwards client traffic to them accordingly. If a back-end server port is found to be unhealthy, no new connections are forwarded to it. The load balancer continues to monitor health of unhealthy ports, and it resumes their use if they become healthy again, which means that they successfully pass two consecutive health check attempts.

The health checks for HTTP and TCP ports are conducted as follows:

* **HTTP:** An `HTTP GET` request against a pre-specified URL is sent to the back-end server port. The server port is marked healthy upon receiving a `200 OK` response. The default `GET` health path is "/" through the UI, and it can be customized.

* **TCP:** The load balancer attempts to open a TCP connection with the back-end server on the specified TCP port. The server port is marked healthy if the connection attempt is successful, and the connection is closed.

By default, health checks are sent every five seconds on the same port on which traffic is sent to the instance. By default, the load balancer waits two seconds for a response to the health check, and an instance is no longer considered healthy after two failed health checks.

## SSL offloading and required authorizations
{: #ssl-offloading-and-required-authorizations}

For all incoming HTTPS connections, the load balancer service terminates the SSL connection and establishes a plain-text HTTP communication with the back-end server instance. With this technique, CPU-intensive SSL handshakes and encryption or decryption tasks are shifted away from the back-end server instances, allowing them to use all their CPU cycles for processing application traffic.

An SSL certificate is required for the load balancer to perform SSL offloading tasks. You can manage the SSL certificates through [IBM Certificate Manager](/docs/certificate-manager?topic=certificate-manager-getting-started).

To give a load balancer access to your SSL certificate, you must enable **service-to-service authorization**, which grants your load balancer service instance access to your certificate manager instance. For more information, see [Granting access between services](/docs/iam?topic=iam-serviceauth#create-auth). Make sure to choose **Infrastructure Service** as the source service, **Load Balancer for VPC** as the resource type, **Certificate Manager** as the target service, and assign the **Writer** service access role.

If the required authorization is removed, errors might occur for your load balancer.
{: important}

Only TLS 1.2 is supported. The following list details the supported ciphers (listed in order of precedence):

* TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256

## Configuring ACLs for use with load balancers
{: #configuring-acls-for-use-with-load-balancers}

If you use access control lists (ACLs) to block traffic on the subnets in which load balancer is deployed, make sure the ACL rules listed are configured.

| Inbound/Outbound| Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound |TCP| AnyIP | AnyPort| AnyIP | 56501|
| Inbound |TCP| AnyIP | 443, 10514, 8834| AnyIP | AnyPort|
| Inbound |UDP| 161.26.0.0/16 | 53| AnyIP | AnyPort|
| Outbound | TCP | AnyIP | 56501| AnyIP | AnyPort|
| Outbound | TCP | AnyIP | AnyPort| AnyIP |443, 10514, 8834|
| Outbound | UDP | AnyIP | AnyPort| 161.26.0.0/16 |53|

Additionally, if a load balancer has listeners configured, then the corresponding inbound and outbound ACL rules should be configured for traffic to go through.

## MZR support
{: #mzr-support}

{{site.data.keyword.cloud_notm}} Load Balancer for VPC supports Multi-Zone-Regions (MZRs). You can achieve high availability and redundancy by deploying a load balancer with subnets from different zones. When subnets from multiple zones are used to provision a load balancer, the load balancer appliances get deployed to multiple zones.
