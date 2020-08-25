---

copyright:
  years: 2018, 2020
lastupdated: "2020-07-30"  

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, layer-7, auto scale, managed pool, instance group

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

# FAQs for application load balancers
{: #load-balancer-faqs}

This section contains answers to some frequently asked questions about the {{site.data.keyword.cloud}} Application Load Balancer for VPC service.

## Can I use a different DNS name for my application load balancer?
{: #different-dns-name}
{: faq}

The auto-assigned DNS name for the application load balancer is not customizable. However, you can add a CNAME (Canonical Name) record that points your preferred DNS name to the auto-assigned load balancer DNS name. For example, your load balancer in `us-south` has ID `dd754295-e9e0-4c9d-bf6c-58fbc59e5727`, and the auto-assigned load balancer DNS name is `dd754295-us-south.lb.appdomain.cloud`. Your preferred DNS name is `www.myapp.com`. You can add a CNAME record (through the DNS provider that you use to manage `myapp.com`) pointing `www.myapp.com` to the load balancer DNS name `dd754295-us-south.lb.appdomain.cloud`.

## What's the maximum number of front-end listeners I can define with my application load balancer?
{: #max-number-listeners}
{: faq}

10 is the maximum number of front-end listeners that you can define with your application load balancer.

## What's the maximum number of virtual server instances I can attach to my back-end pool?
{: #server-instances-back-end-pool-alb}
{: faq}

50 is the maximum number of virtual server instances that you can attach to a back-end pool.

## Is the load balancer horizontally scalable?
{: #horizontally-scalable}
{: faq}

Yes. The application load balancer automatically adjusts its capacity based on the load. When horizontal scaling takes place, the number of IP addresses associated with the application load balancer's DNS changes.

## What should I do if I'm using ACLs on the subnets that are used to deploy the application load balancer?
{: #using-acls-on-subnets}

Make sure that the proper ACL rules are in place to allow incoming traffic for configured listener ports and management ports. Traffic between the application load balancer and back-end instances should also be allowed.

For detailed information on the ACLs configuration required, see [Configuring ACLs for use with application load balancers](/docs/vpc?topic=vpc-load-balancers#configuring-acls-alb).

## Why am I receiving an error message: `certificate instance not found`?
{: #error-certificate-instance}
{: faq}

* The certificate instance CRN might not be valid.
* You might not have granted service-to-service authorization. See [SSL offloading](#ssl-offloading-and-required-authorizations) for details.

## Why am I receiving a `401 Unauthorized Error` code?
{: #401-unauthorized-error}
{: faq}

Check the following access policies for your user:

* The access policy for the application load balancer resource type
* The access policy for the resource group
* If `HTTPS` listeners are used, also check the service-to-service authorization for the Certificate Manager instance.

## Why is my application load balancer in `maintenance_pending` state?
{: #maintenance-pending}
{: faq}

The application load balancer is in `maintenance_pending` state during various maintenance activities, such as:

* Horizontal scaling activities
* Recovery activities
* Rolling upgrades to address vulnerabilities and apply security patches
* ACLs blocking management traffic to the load balancer

  This puts your load balancer in a `maintenance_pending` state. Ensure that the ACLs on your load balancer subnets are configured properly. Refer to [Configuring ACLs for use with load balancers](/docs/vpc?topic=vpc-load-balancers#configuring-acls-alb).
  {: note}

## Why do I need to choose multiple subnets during provisioning?
{: #subnets-during-provisioning}
{: faq}

The {{site.data.keyword.cloud_notm}} Application Load Balancer for VPC is Multi-Zone Region (MZR) ready. Load balancer appliances are deployed to the subnets you selected. To achieve higher availability and redundancy, deploy the application load balancer to subnets in different zones.

## Do I need additional IPs in the subnet for application load balancer operations?
{: #additional-ips-subnet}
{: faq}

It is recommended to allocate 8 additional IPs per zone/MZR to accommodate horizontal scaling and maintenance operations. If provisioning your application load balancer with one subnet, allocate 16 additional IPs.

## Why is back-end member health under my pool `unknown`?
{: #member-health-unknown}
{: faq}

Either the pool is not associated with any listeners, or configuration changes might have been made to the pool or its associated listener.

## Why is back-end member health under my pool `faulted` ?
{: #member-health-failing}
{: faq}

Verify the following configurations:

* Does the port of the configured back-end protocol match the port the application is listening on?
* Does the configured health-check have the correct protocol (TCP or HTTP), port, and URL (for HTTP) information? For HTTP, ensure your application responds with `200 OK` for the configured health check URL.
* Is the back-end server a virtual server with an enabled security group? If so, ensure that the security group rules allow traffic between the load balancer and the virtual server.

See [Health Checks](#health-checks) section for additional information.

## What are the default settings and allowed values for health check parameters?
{: #alb-health-check-parameters}
{: faq}

* Health check interval - Default is 5 seconds, and the range is 2 - 60 seconds.
* Health check response timeout - Default is 2 seconds, and the range is 1 - 59 seconds.
* Maximum retry attempts - Default is two retry attempts, and the range is 1-10 retries.

The health check response timeout value must be less than the health check interval value.
{:tip}

## Are the application load balancer IP addresses fixed?
{: #ip-addresses-fixed}
{: faq}

Application load balancer IP addresses are not guaranteed to be fixed. During system maintenance or horizontal scaling, you will see changes in the available IPs associated with the FQDN of your load balancer.

Use FQDN, rather than cached IP addresses.
{:important}

## Does the load balancer support Layer-7 switching?
{: #layer-7-switching-alb}
{: faq}

Yes, the load balancer supports Layer-7 switching.

## Why does HTTPS listener creation or update tell me that my certificate is invalid?
{: #why-does-https-listener-creation-or-update-tell-me-that-my-certificate-is-invalid}
{: faq}

Check for these possibilities:

* The provided certificate CRN might not be valid.
* The certificate instance in the Certificate Manager might not have an associated private key.

### What is the role of load balancer front end listeners?
{: #role-load-balancer-listeners}
{: faq}

Load balancer front end listeners are the application listening ports. They act as proxies for backend pools.

### Why are there only 2 IPs instead of 3?
{: #why-only-2-ips}
{: faq}

The application load balancer operates in ACTIVE-ACTIVE mode, a configuration that makes it highly available. Horizontal scaling may further add extra appliances when your load increases. It's recommended that you choose subnets in different zones to make your load balancers support MZR. This way, if a zone is negatively impacted, a new load balancer will be provisioned in a different zone.

### Why am I receiving a 409 Conflict Error code when updating or deleting a load balancer?
{: #409-lbaas-conflict-error}
{: faq}

If you are receiving the HTTP status code `409`, it could be for one of the following reasons:
* You cannot `update` the load balancer configuration if the load balancer status is not `ACTIVE`.
* You cannot `delete` a load balancer if the load balancer status is not `ACTIVE` or `ERROR`.
* You cannot `delete` a load balancer which is already in `DELETE_PENDING` state.
* You cannot `delete` a load balancer if it has one or more pools that are attached to instance groups.
* You cannot `delete` a load balancer pool if it is attached to an instance group.
* You cannot `add`/`delete`/`update` members to a pool that is managed by an instance group.

### If a pool is attached to an instance group, what is the maximum number of backend members I can have in a pool?
{: #lbaas-ig-pool-member}
{: faq}

The maximum number of backend members allowed in a pool is 50. So if an instance group is attached to a pool, the number of instances in the group will not scale up beyond this limit.
