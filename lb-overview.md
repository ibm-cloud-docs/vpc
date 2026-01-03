---

copyright:
  years: 2018, 2026
lastupdated: "2026-01-03"

keywords: listener, pool, round-robin, weighted, layer 7, datapath logging, http2, websocket

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About application load balancers
{: #load-balancers-about}

Use {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) to distribute traffic among multiple server instances within the same region of your VPC.
{: shortdesc}

If you have public and private workloads and layer 7 traffic, use an application load balancer.

## Types of application load balancers
{: #types-load-balancer}

As discussed in the [Load balancers for VPC overview](/docs/vpc?topic=vpc-nlb-vs-elb&interface=ui), you can create a public or private ALB.

This table shows a comparison of public versus private features.

| Feature | Public load balancer | Private load balancer |
|--------|-------|-------|
| Accessible on the internet? |  Yes, with a fully qualified domain name (FQDN) | No, internal clients only, on same region and VPC |
| Accepts all traffic? | Yes | Yes \n (The restriction to accept traffic only from the RFC-1918 address space is removed.) |
| How is the domain name registered? | Public IP addresses | Private IP addresses |
{: caption="Comparison of public and private load balancers" caption-side="bottom"}

### Public application load balancer
{: #public-load-balancer}

A public application load balancer instance is assigned a publicly accessible fully qualified domain name (FQDN), which you must use to access your applications that are hosted behind the load balancer. This domain name can be registered with one or more public IP addresses.

Over time, the number and value of these public IP addresses might change due to maintenance and scaling activities. The back-end virtual server instances that host your application must run in the same region and under the same VPC.

Use the assigned FQDN to send traffic to the public application load balancer to avoid connectivity problems to your applications during system maintenance or scaling down activities.
{: important}

### Private application load balancer
{: #private-load-balancer}

A private application load balancer is accessible through your private subnets that you configured to create the load balancer.

Similar to a public application load balancer, an FDQN is assigned to your private application load balancer instance. However, this domain name is registered with one or more private IP addresses.

{{site.data.keyword.cloud_notm}} operations might change the number and value of your assigned private IP addresses over time, based on maintenance and scaling activities. The back-end virtual server instances that host your application must run in the same region, and under the same VPC.

Use the assigned FQDN to send traffic to the private application load balancer to avoid connectivity problems to your applications during system maintenance or scaling down activities.
{: important}

## Load-balancing methods
{: #load-balancing-methods}

Three load-balancing methods are available for distributing traffic across the back-end application servers:

### Round-robin
{: #round-robin}

Round-robin is the default load-balancing method. With this method, an application load balancer forwards incoming client connections in round-robin fashion to the back-end servers. As a result, all back-end servers receive roughly an equal number of client connections.

### Weighted round-robin
{: #weighted-round-robin}

With this method, an application load balancer forwards incoming client connections to the back-end servers in proportion to the weight that was assigned to these servers. A default weight of `50` is assigned to each server. The weight can be customized to any value in the range `0` - `100`.

For example, if application servers A, B and C have the weights `60`, `60`, and `30`, then servers A and B receive an equal number of connections, while server C receives half that number of connections.

Setting a server weight to `0` means that no new connections are forwarded to that server, but any existing traffic continues to flow. Using a weight of `0` can help bring down a server gracefully and remove it from service rotation.
{: tip}

The server weight values are applicable only with the weighted round-robin method. They are ignored with round-robin and least-connections load-balancing methods.

### Least connections
{: #least-connections}

With this method, the back-end server instance that serves the least number of connections at a specific time receives the next client connection.

## Front-end listeners and back-end pools
{: #front-end-listeners-and-back-end-pools}

Front-end listeners are load balancer application ports for receiving incoming requests, while back-end pools are the application servers behind the load balancers.

### Guidelines for using listeners
{: #listener-guidelines}

Review the following guidelines for front-end listeners:

* You can define up to 10 front-end listeners and map them to back-end pools on your back-end application servers.
* The FQDN assigned to your load balancer and the front-end listener ports are exposed to the public internet. Incoming user requests are received on these ports.
* The supported front-end listener and back-end pool protocols are HTTP, HTTPS, and TCP.
* You can configure an HTTP/HTTPS front-end listener with an HTTP/HTTPS back-end pool.
* HTTP/2 is supported only for listeners.
* HTTP and HTTPS listeners and pools are interchangeable.
* You can configure only a TCP front-end listener with a TCP back-end pool.
* You can attach up to 50 virtual server instances to a back-end pool. Traffic is sent to each instance on its specified data port. This data port does not need to be the same one as the front-end listener port.
* "Private only" endpoints for Secrets Manager are not supported with HTTPS listeners. To configure an HTTPS listener in an ALB, you must upload your TLS certificates to a "Public and private" endpoint.

### HTTPS redirect listener
{: #https-redirect-listener}

HTTPS redirect listeners redirect the traffic from an HTTP listener to an HTTPS listener. This action does not require any rules to be applied on the listener.

For instance, if a service listens on port 443 with HTTPS and a user tries to access the service on port 80 by using HTTP, then the request automatically redirects to port 443 with HTTPS.

If policies are present on the HTTPS redirect listener, then the policies are evaluated first. If no policy matches exist, then the request redirects to a configured HTTPS listener.

### HTTPS redirect listener properties
{: #https_redirect_listener_properties}

Property  | Description
------------- | -------------
Listener | The HTTPS listener to which a request redirects.
HTTP status code | The status code of the response returned by the application load balancer. The acceptable values are: `301`, `302`, `303`, `307`, or `308`.
URI | The relative URI to which a request redirects. This property is optional.
{: caption="HTTPS redirect listener properties" caption-side="bottom"}

## Elasticity
{: #alb-elasticity}

The application load balancer scales out by adding compute resources when load increases.

## SSL offloading and required authorizations
{: #ssl-offloading-and-required-authorizations}

Secure Sockets Layer (SSL) offloading allows the application load balancer to terminate all incoming HTTPS connections.

When an HTTPS listener is configured with an HTTP pool, the HTTPS request is terminated at the front-end and the load balancer establishes a plain-text HTTP communication with the back-end server instance. With this technique, CPU-intensive SSL handshakes and encryption or decryption tasks are shifted away from the back-end server instances, allowing them to use all their CPU cycles for processing application traffic.

SSL offloading requires you to provide an SSL certificate for the application load balancer to perform SSL offloading tasks. You can manage the SSL certificates through the [{{site.data.keyword.secrets-manager_full_notm}}](/docs/secrets-manager?topic=secrets-manager-getting-started).

{{site.data.content.load-balancer-grant-service-auth}}

To prevent errors, you must establish the required authorization between your load balancer and {{site.data.keyword.secrets-manager_full_notm}}. In addition, updating certificates in Secrets Manager does not automatically update your ALB. For your load balancer to reflect any changes in the certificate, make a small update (such as changing the health check interval or timeout value) to cause a refresh. This action updates the certificate on your load balancer to match the certificate in Secrets Manager. You can then revert any changes that you made back to their original values.
{: important}

Transport Layer Security (TLS) 1.2 and 1.3 are supported. However, TLS 1.3 is used by default unless you specifically configure the client side to use 1.2. Application load balancers accept all supported TLS 1.3 ciphers that are sent by the client-side request.
{: note}

The following lists the supported ciphers (in order of precedence):

* `TLS_AES_256_GCM_SHA384`
* `TLS_CHACHA20_POLY1305_SHA256`
* `TLS_AES_128_GCM_SHA256`
* `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256`

### Locating the certificate CRN
{: #locating-alb-crn}

When configuring authentication for an application load balancer during provisioning in the console, you can choose to specify the Secrets Manager and SSL certificate, or the certificate's CRN. You might want to do this if you cannot view the Secrets Manager in the drop-down menu, which means you don't have access to the Secrets Manager instance. Keep in mind that you must enter the CRN if you're using the API to create an ALB.

To obtain the CRN, you must have permission to access the Secrets Manager instance.
{: note}

To find a certificate's CRN, follow these steps:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu icon ![Navigation menu icon](../icons/icon_hamburger.svg) > Resource list**.
1. Click to expand **Security**, then select the Secrets Manager that you want to find the CRN for.
1. Select anywhere in the table row of the certificate to open the Certificate details side panel. The CRN of the certificate is listed.

## End-to-end SSL encryption
{: #end-to-end-ssl-encryption}

Configuring an HTTPS listener with an HTTPS pool enables end-to-end SSL encryption. The ALB terminates the incoming HTTPS request at the front-end listener and establishes an HTTPS connection with the back-end instances. End-to-end encryption allows all traffic that's going through the load balancer to the back-end members to be encrypted over HTTPS.

To configure end-to-end SSL encryption:

1. Configure an HTTPS front-end listener with your SSL certificate as you would when you're configuring SSL offloading.
2. Configure an HTTPS back-end pool.
3. Add your back-end member instance to the HTTPS back-end pool. Make sure that your back-end member instances are configured to handle HTTPS traffic.
4. Configure health check with type HTTPS to perform encrypted health checks with your back-end members.

An application load balancer does not verify the SSL certificates of the back-end member instances.
{: important}

## Horizontal scaling
{: #horizontal-scaling}

An application load balancer adjusts its capacity automatically according to the load. When this adjustment occurs, you might see a change in the number of IP addresses associated with the load balancer's DNS name.

## MZR support
{: #mzr-support}

{{site.data.keyword.cloud_notm}} Application Load Balancer for VPC supports Multi-Zone-Regions (MZRs). You can achieve high availability and redundancy by deploying an application load balancer with subnets from different zones. When subnets from multiple zones are used to provision an application load balancer, the load balancer appliances get deployed to multiple zones.

## Integration with instance groups
{: #lbaas-integration-with-instance-groups-overview}

{{site.data.keyword.cloud_notm}} Application Load Balancer for VPC integrates with instance groups, which can `auto scale` your back-end members. Pool members are dynamically added and deleted based on your usage and requirements.

## Datapath log forwarding
{: #datapath-log-forwarding}

When datapath logging is enabled, load balancer logs are forwarded to the [{{site.data.keyword.logs_full_notm}}](/catalog){: external} service, where you can view your datapath logs.

## HTTP2 support
{: #http2-support}

Application load balancers support end-to-end HTTP2 traffic, and work with listener protocols set as either HTTPS or TCP.

## WebSocket support
{: #websocket-support}

WebSocket provides full-duplex communication channels over a single TCP connection. Application load balancers support WebSocket with every type of listener protocol (HTTP/HTTPS/TCP).

## High availability and application load balancers
{: #ha-and-alb}

To make sure that High Availability (HA) works with your ALB, attach three subnets from different zones to the ALB and deploy appliances across these zones. To do this, you first select your subnets during the ALB creation process. You can select two subnets in different zones (such as `us-south-1` and `us-south-2`). Doing so creates the ALB's IP addresses (such as the appliance IPs) in two different subnets.

You can also do this with already existing ALBs. Go to the **Attached Resources** section on your load balancer details page. From the **Subnet** section, click **Edit subnets**. Then, attach more subnets. The ALB goes into the “Migrating” state. When the migration completes, you get a new IP for the appliance from the subnet that you just attached. You now have two IP addresses from different subnets in different zones.

## Related links
{: #permissions-related-links-alb}

* [Load balancer CLI reference](/docs/vpc?topic=vpc-vpc-reference#lb-anchor)
* [Load balancer API reference](/apidocs/vpc#list-load-balancer-profiles)
* [ALB for VPC infrastructure resources for Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_lb){: external} (VPC infrastructure > Resources)
* [{{site.data.keyword.cloudaccesstraillong_notm}} events](/docs/vpc?topic=vpc-at_events#events-load-balancers)
* [FAQs for application load balancers](/docs/vpc?topic=vpc-load-balancer-faqs)
* [Quotas](/docs/vpc?topic=vpc-quotas&interface=ui#alb-quotas)
