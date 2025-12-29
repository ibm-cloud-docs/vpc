---

copyright:
  years: 2018, 2025
lastupdated: "2025-12-29"


keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc network, layer 7, auto scale, managed pool, instance group

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for application load balancers
{: #load-balancer-faqs}

The following sections contain answers to some frequently asked questions about the {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB).

## Can I use a different DNS name for my ALB?
{: #different-dns-name}
{: faq}

The auto-assigned DNS name for the application load balancer is not customizable. However, you can add a CNAME (Canonical Name) record that points your preferred DNS name to the auto-assigned load balancer DNS name. For example, your load balancer in `us-south` has ID `dd754295-e9e0-4c9d-bf6c-58fbc59e5727`, and the auto-assigned load balancer DNS name is `dd754295-us-south.lb.appdomain.cloud`. Your preferred DNS name is `www.myapp.com`. You can add a CNAME record (through the DNS provider that you use to manage `myapp.com`) pointing `www.myapp.com` to the load balancer DNS name `dd754295-us-south.lb.appdomain.cloud`.

## What's the maximum number of front-end listeners I can define with my application load balancer?
{: #max-number-listeners}
{: faq}

10 is the maximum number of front-end listeners that you can define with your ALB.

## What's the maximum number of virtual server instances I can attach to my back-end pool?
{: #server-instances-back-end-pool-alb}
{: faq}

50 is the maximum number of virtual server instances that you can attach to a back-end pool.

## What's the maximum number of subnets I can attach to my application load balancer?
{: #max-number-subnets-alb}
{: faq}

15 is the maximum number of subnets that you can define with your ALB.

## Is the load balancer horizontally scalable?
{: #horizontally-scalable}
{: faq}

Yes. The {{site.data.keyword.alb_full}} automatically adjusts its capacity based on the load. When horizontal scaling takes place, the number of IP addresses associated with the application load balancer's DNS changes.

## What do I do if I use ACLs on the subnets that are used to deploy the application load balancer?
{: #using-acls-on-subnets}

Make sure that the proper ACL rules are in place to allow incoming traffic for configured listener ports. Traffic between the application load balancer and back-end instances need also be allowed.

## Why is my application load balancer in `maintenance_pending` state?
{: #lb-maintenance-pending}
{: faq}

The application load balancer is in `maintenance_pending` state during various maintenance activities, such as:

* Horizontal scaling activities
* Recovery activities
* Rolling upgrades to address vulnerabilities and apply security patches

## Why do I need to choose multiple subnets during provisioning?
{: #subnets-during-provisioning}
{: faq}

The {{site.data.keyword.alb_full}} (ALB) is Multi-Zone Region (MZR) ready. Load balancer appliances are deployed to the subnets you selected. To achieve higher availability and redundancy, deploy the application load balancer to subnets in different zones.

## Do I need extra IPs in the subnet for application load balancer operations?
{: #additional-ips-subnet}
{: faq}

It is recommended to allocate 8 extra IPs per MZR to accommodate horizontal scaling and maintenance operations. If you provision your application load balancer with one subnet, allocate 16 extra IPs.

## What are the default settings and allowed values for health check parameters?
{: #alb-health-check-parameters}
{: faq}

* Health check interval - Default is 5 seconds, and the range is 2 - 60 seconds.
* Health check response timeout - Default is 2 seconds, and the range is 1 - 59 seconds.
* Maximum retry attempts - Default is two retry attempts, and the range is 1-10 retries.

The health check response timeout value must be less than the health check interval value.
{: tip}

## Are the ALB IP addresses fixed?
{: #ip-addresses-fixed}
{: faq}

Application load balancer IP addresses are not guaranteed to be fixed. During system maintenance or horizontal scaling, you see changes in the available IPs associated with the FQDN of your load balancer.

Use FQDN, rather than cached IP addresses.
{: important}

## Does the load balancer support layer 7 switching?
{: #layer-7-switching-alb}
{: faq}

Yes, the load balancer supports layer 7 switching.

##  Can customers modify an application load balancer's timeout value?
{: #timeout-value-alb}
{: faq}

Yes, a customer can modify their ALB's client and server timeout value by modifying the front-end-listener's timeout value from the portal. The timeout value range is between 50 to 7200 seconds. To modify an ALB in the IBM Cloud console, log in to the [{{site.data.keyword.cloud_notm}} console](/login){: external}. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers** from the Network section. Click the name of the load balancer that you want to edit, then select the Front-end listener tab. Click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") next to the Front-end listener that you want to edit, then select **Edit**. Set the desired Timeout value (range from 50 to 7200 seconds).

## Why does HTTPS listener creation or update tell me that my certificate is invalid?
{: #why-does-https-listener-creation-or-update-tell-me-that-my-certificate-is-invalid}
{: faq}

Check for these possibilities:

* The provided certificate CRN might not be valid.
* The certificate instance in the Secrets Manager might not have an associated private key.

## What is the role of application load balancer front-end listeners?
{: #role-load-balancer-listeners}
{: faq}

Load balancer front-end listeners are the listening ports for the application. They act as proxies for back-end pools.

## Why are there only 2 IPs instead of 3?
{: #why-only-2-ips}
{: faq}

The {{site.data.keyword.alb_full}} (ALB) operates in ACTIVE-ACTIVE mode, a configuration that makes it highly available. Horizontal scaling might further add extra appliances when your load increases. The recommendation is that you choose subnets in different zones to make your load balancers support MZR. This way, if a zone is negatively impacted, a new load balancer is provisioned in a different zone.

## If a pool is attached to an instance group, what is the maximum number of back-end members that I can have in a pool?
{: #lbaas-ig-pool-member}
{: faq}

The maximum number of back-end members that are allowed in a pool is 50. So if an instance group is attached to a pool, the number of instances in the group can't scale up beyond this limit.

## Why is my listener not receiving traffic?
{: #lbaas-listener-security-group}
{: faq}

Make sure that the security group rules that are attached to your load balancer allow incoming ingress and outgoing egress traffic on your listener's port. Security groups attached to your load balancer can be found on your load balancer's overview page. Locate the `Attached security groups` tab from the load balancer overview, then select the security groups that you want to view and modify their rules.

## Does IBM complete quarterly ASV scans of data-plane LBaaS appliances?
{: #alb-asv}
{: faq}

Approved Scanning Vendor (ASV) quarterly scanning is a requirement of the Payment Card Industry (PCI) Security Standards Council. ASV scanning of LBaaS data-plane appliances is solely a customer responsibility. IBM does not use ASVs to scan data-plane appliances because these scans can negatively impact customer workload functions and performance.

## How are active connections handled when a load balancer is scaled down?
{: #faqs-active-connections}
{: faq}

When a load balancer appliance undergoes a scale down due to horizontal scaling or maintenance, the service waits for the active connections to close to allow for traffic to move to other appliances. After 24 hours, the service will complete its scale down event, which may terminate any active connections on those scaled down appliances.

## How does the load balancer account disablement policy work?
{: #account-disablement-policy}
{: faq}

If you receive a notification that your load balancer has been suspended, then any load balancers on your account will be deleted. If the suspension on your account is removed, your previous load balancers will be restored only if their pre-requisite resources are still active, such as VPCs, subnets, and security groups. If these resources are no longer available, then you need to provision a new load balancer.

## Can I attach the same backend member with same port on two different ALBs?
{: #backend-member-same-port-two-albs}
{: faq}

Yes, it is possible to attach the same backend member with the same port on two different ALBs.

## Can I attach the same backend member with the same port more than once to the same ALB?
{: #backend-member-same-port-one-albs}
{: faq}

No, it is not possible to attach the same backend member with the same port on a single ALB within same pool. However, you can try to attach the same backend member with the same port on a single ALB through different pools on that ALB. 

You may also attach the same backend member with different ports, or you can attach different backend members with the same ports.

## Is gRPC supported on ALB?
{: #grpc-supported-albs}
{: faq}

GRPC relies on HTTP/2 as its underlying transport protocol. IBM Cloud ALBs support HTTP/2 protocol end-to-end for gRPC traffic. However, IBM Cloud ALBs do not offer native support for gRPC as of now. You can configure TCP protocol instead, as ALBs do not recognize gRPC traffic. Instead, they treat gRPC traffic as TCP traffic. 

## Can I know the address pool for appliance public IPs?
{: #address-pool-appliance-public-ips}
{: faq}

Public IP addresses are random. They get assigned automatically. Address ranges cannot be confirmed. However,
Private IPs are taken from the subnets you choose while provisioning a load balancer.

## What happens to session stickiness behavior when the appliance is auto-scaled down?
{: #session-stickiness-appliance-auto-scale-down}
{: faq}

The persistence will be moved to the one of the available backend members.

## If certificates attached to load balancers are auto-rotated after getting renewed, why do I have to manually update my certificate this time?
{: #load-balancer-certificate-manual-update}
{: faq}

Secrets Manager supports a maximum of 50 rotation versions for a secret. Within this limit, when a secret is rotated, the ALB automatically refreshes and uses the updated certificate. However, when the rotation count exceeds 50, a new certificate must be manually updated in the ALB’s front-end listener. Before reaching this limit, no manual update of the front-end listener is required.
