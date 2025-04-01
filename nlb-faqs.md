---

copyright:
  years: 2019, 2025
lastupdated: "2025-04-01"

keywords: load balancer, network, faqs

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for network load balancers
{: #nlb-faqs}

This section contains answers to some frequently asked questions about {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB).
{: shortdesc}

For FAQs on Private Path NLBs, see [FAQs for Private Path network load balancers](/docs/vpc?topic=vpc-nlb-faqs#ppnlb-faqs).
{: note}

## FAQs for private and public network load balancers
{: #public-private-nlb-faqs}

You might encounter the following FAQs when you use IBM Cloud private and public NLBs.

### Can I use a different DNS name for my load balancer?
{: #can-i-use-a-different-dns-name-for-my-load-balancer}
{: faq}

The auto-assigned DNS name for the load balancer is not customizable. However, you can add a CNAME (Canonical Name) record that points your preferred DNS name to the auto-assigned load balancer DNS name. For example, if your load balancer in `us-south` has the ID `dd754295-e9e0-4c9d-bf6c-58fbc59e5727`, and the auto-assigned load balancer DNS name is `dd754295-us-south.lb.appdomain.cloud`, then your preferred DNS name is `www.myapp.com`. You can add a CNAME record (through the DNS provider that you use to manage `myapp.com`) pointing `www.myapp.com` to the load balancer DNS name `dd754295-us-south.lb.appdomain.cloud`.

### How are DNS names for my load balancer registered?
{: #how-are-dns-names-for-my-load-balancer-registered}
{: faq}

An NLB automatically assigns DNS hostnames for your load balancers in the common DNS zone `lb.appdomain.cloud`. For maximum portability, these DNS names are registered publicly, even for private load balancers. The hostname has a portion of the randomly generated load balancer ID and does not expose any identifying information. Private load balancer names can be resolved publicly, but the addresses they resolve to are not routable from the internet, and can be reached only from inside your own private network environment.

### Do NLBs support layer-7 switching?
{: #does-the-network-load-balancer-support-layer-7-switching}
{: faq}

No, NLBs for VPC do not support layer-7 switching.

### What's the maximum number of front-end listeners that I can define with my load balancer?
{: #what-s-the-maximum-number-of-front-end-listeners-i-can-define-with-my-load-balancer}
{: faq}

You can define a maximum of ten front-end listeners for an NLB.

### What's the maximum number of virtual server instances that I can attach to my back-end pool?
{: #what-s-the-maximum-number-of-server-instances-i-can-attach-to-my-back-end-pool}
{: faq}

You can attach a maximum of 50 virtual server instances to your back-end pool for an NLB.

### Is an NLB horizontally scalable?
{: #is-the-network-load-balancer-horizontally-scalable}
{: faq}

No, an NLB is not horizontally scalable.

### What should I do if I'm using ACLs on the subnets that are used to deploy an NLB?
{: #what-should-i-do-if-i-am-using-acls-on-the-subnets-that-are-used-to-deploy-the-load-balancer-nlb}

Make sure that the proper ACL rules are in place to allow incoming traffic for configured listener ports. Also, make sure that traffic between an NLB and back-end instances are allowed.

### What are the default settings and allowed values for health check options?
{: #what-are-the-default-settings-and-allowed-values-for-health-check-options}
{: faq}

The following default settings apply to NLB health check options:

* **Health check interval:** Five seconds, and the range is 2 - 60 seconds.
* **Health check response timeout:** Two seconds, and the range is 1 - 59 seconds.
* **Maximum retry attempts:** Two retry attempts, and the range is 1-10 retries.

The health check response timeout value must be less than the health check interval value.
{: tip}

### Is an NLB's IP address fixed?
{: #is-the-network-load-balancer-ip-address-fixed}
{: faq}

The IP address is fixed for both public and private NLBs. However, route-mode NLBs toggle between primary and standby appliance IPs throughout their lifetime.

### Does IBM complete quarterly ASV scans of data-plane LBaaS appliances?
{: #lbaas-asv}
{: faq}

Approved Scanning Vendor (ASV) quarterly scanning is a requirement of the Payment Card Industry (PCI) Security Standards Council. ASV scanning of LBaaS data-plane appliances is solely a customer responsibility. IBM does not use ASVs to scan data-plane appliances because these scans can negatively impact customer workload functions and performance.

### Why is my listener not receiving traffic?
{: #nlbaas-listener-security-group}
{: faq}

Make sure that the security group rules that are attached to your load balancer allow ingress and egress traffic on your listener's port. Security groups attached to your load balancer can be found on the load balancer overview page. Locate the **Attached security groups** tab in the overview, then select the security groups whose rules you want to view and modify them if necessary.

## FAQs for Private Path network load balancers
{: #ppnlb-faqs}

You might encounter the following FAQs when you use IBM Cloud Private Path NLBs.

### My Private Path NLB has an IP address from only one zone. Is a Private Path NLB regional?
 {: #ppnlb-regional}
 {: faq}
 {: support}

Yes, a Private Path NLB is regional. A Private Path NLB can withstand zonal failures and continue to serve requests. Beyond high availability, a Private Path NLB is regional in the sense that traffic is balanced across all zones. The subnet is used for choosing the source IPs for the traffic arriving in the members. Even if the zone is down where the subnet resides, requests are served.

### All the private IPs of the Private Path NLB are in a single subnet. Is the Private Path NLB zonal? What happens if a zone with the subnet fails?
{: #ppnlb-single-subnet}
{: faq}
{: support}

A Private Path NLB is a regional offering. Connections to the associated Virtual Private Endpoints (VPEs) are load balanced across all healthy zones. If any zone fails, new connections are directed to the remaining healthy zones and existing connections are directed to unimpacted healthy zones. This is true even if the failed zone is the zone hosting the subnet from which the Private Path NLB private IPs are allocated. Think of the allocated IPs as logical IPs instead of an indication of where the Private Path NLB is running.

### For a given consumer VPC, only a single VPE gateway and its IP can be associated with a Private Path NLB (or Private Path service). Does this mean that if the zone containing the VPE gateway IP fails, that the VPE endpoint is down and I can't reach the Private Path NLB from other zones in my VPC?
{: #ppnlb-single-vpe-gateway-ip}
{: faq}
{: support}

No, if the zone holding a VPE associated with a Private Path service or Private Path NLB fails, other zones in your VPC can still use the VPE gateway and reach the Private Path NLB. VPE gateways that are associated with Private Path NLBs have IP addresses that do not indicate which zone they run in. These VPE gateways run in all zones, so even if the zones that contain their IPs are down, the VPE gateways remain functional.

### Do Private Path NLBs support layer-7 switching?
{: #ppnlb-layer-seven}
{: faq}
{: support}

No, similar to any NLB, a Private Path NLB does not support layer-7 switching.

### Is a Private Path NLB horizontally scalable?
{: #ppnlb-scalable}
{: faq}
{: support}

Yes, a Private Path NLB can be scaled up and use multiple systems to serve the workload. IBM performs this work; no consumer input is required.

### Does a Private Path NLB support security groups or Network ACLs (NACLs)?
{: #ppnlb-sg-acls}
{: faq}
{: support}

No, security groups and NACLs are not supported. Instead, you can use the associated Private Path service to control the set of consumers that attach to your Private Path NLB.

### What is the DNS name for my Private Path NLB?
{: #ppnlb-dns-name}
{: faq}
{: support}

A Private Path NLB is only reachable from VPEs in consumer VPCs. There is no FQDN for the load balancer itself. Instead, define the service FQDN that maps to each consumer VPE in the Private Path service 'service_endpoints' property.

### What's the maximum number of front-end listeners that I can define with my Private Path NLB?
{: #ppnlb-front-end-listeners}
{: faq}
{: support}

The default quota is 10 front-end listeners for a Private Path NLB. To increase this quota, [contact IBM Support](/unifiedsupport/cases/form){: external}.

### What's the maximum number of virtual server instances that I can attach to my back-end pool?
{: #ppnlb-vsi-max}
{: faq}
{: support}

For more information, see [Quotas and service limits for Private Path network load balancers](/docs/vpc?topic=vpc-quotas#ppnlb-quotas). To increase the quota for your Private Path network load balancer, you must [create a support case](/docs/account?topic=account-open-case).
