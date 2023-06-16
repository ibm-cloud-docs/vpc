---

copyright:
  years: 2019, 2021
lastupdated: "2023-02-23"

keywords: load balancer, network, faqs

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for network load balancers
{: #nlb-faqs}

This section contains answers to some frequently asked questions about the {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB).

## Can I use a different DNS name for my load balancer?
{: #can-i-use-a-different-dns-name-for-my-load-balancer}
{: faq}

The auto-assigned DNS name for the load balancer is not customizable. However, you can add a CNAME (Canonical Name) record that points your preferred DNS name to the auto-assigned load balancer DNS name. For example, if your load balancer in `us-south` has the ID `dd754295-e9e0-4c9d-bf6c-58fbc59e5727`, and the auto-assigned load balancer DNS name is `dd754295-us-south.lb.appdomain.cloud`, then your preferred DNS name is `www.myapp.com`. You can add a CNAME record (through the DNS provider that you use to manage `myapp.com`) pointing `www.myapp.com` to the load balancer DNS name `dd754295-us-south.lb.appdomain.cloud`.

## How are DNS names for my load balancer registered?
{: #how-are-dns-names-for-my-load-balancer-registered}
{: faq}

DNS hostnames for your load balancers are automatically assigned by the load balancer service under the common DNS zone `lb.appdomain.cloud`. For maximum portability, these DNS names are registered publicly, even for private load balancers. The hostname has a portion of the randomly generated load balancer ID and does not expose any identifying information. Private load balancer names can be resolved publicly, but the addresses they resolve to are not routable from the internet, and can only be reached from inside your own private network environment.

## Does the NLB support layer 7 switching?
{: #does-the-load-balancer-support-layer-7-switching}
{: faq}

No. The network load balancer does not support layer 7 switching.

## What's the maximum number of front-end listeners I can define with my load balancer?
{: #what-s-the-maximum-number-of-front-end-listeners-i-can-define-with-my-load-balancer}
{: faq}

You can define a maximum of ten front-end listeners for an NLB.

## What's the maximum number of virtual server instances I can attach to my back-end pool?
{: #what-s-the-maximum-number-of-server-instances-i-can-attach-to-my-back-end-pool}
{: faq}

You can attach a maximum of 50 virtual server instances to your back-end pool for a network load balancer.

## Is an NLB horizontally scalable?
{: #is-the-network-load-balancer-horizontally-scalable}
{: faq}

No. A network load balancer is not horizontally scalable. 

## What should I do if I'm using ACLs on the subnets that are used to deploy an NLB?
{: #what-should-i-do-if-i-am-using-acls-on-the-subnets-that-are-used-to-deploy-the-load-balancer-nlb}

Make sure that the proper ACL rules are in place to allow incoming traffic for configured listener ports and traffic between a network load balancer and back-end instances should also be allowed.

## What are the default settings and allowed values for health check parameters?
{: #what-are-the-default-settings-and-allowed-values-for-health-check-parameters}
{: faq}

The following default settings apply to network load balancer health check parameters:

* **Health check interval:** Five seconds, and the range is 2 - 60 seconds.
* **Health check response timeout:** Two seconds, and the range is 1 - 59 seconds.
* **Maximum retry attempts:** Two retry attempts, and the range is 1-10 retries.

The health check response timeout value must be less than the health check interval value.
{: tip}

## Is the network load balancer IP address fixed?
{: #is-the-network-load-balancer-ip-address-fixed}
{: faq}

The IP address is fixed for both public and private network load balancers. However, route mode NLBs toggle between primary and standby appliance IPs throughput their lifetime.

## Does IBM complete quarterly ASV scans of data-plane LBaaS appliances?  
{: #lbaas-asv}
{: faq}

Approved Scanning Vendor (ASV) quarterly scanning is a requirement of the Payment Card Industry (PCI) Security Standards Council. ASV scanning of LBaaS data-plane appliances is solely a customer responsibility. IBM does not use ASVs to scan data-plane appliances because these scans can negatively impact customer workload functions and performance.
