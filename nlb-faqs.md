---

copyright:
  years: 2019, 2021
lastupdated: "2021-02-24"

keywords: load balancer, network, faqs

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:beta: .beta}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# FAQs for {{site.data.keyword.cloud_notm}} {{site.data.keyword.nlb_full}}
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

No. A network load balancer is not horizontally scalable. However, it comes with a high-availability option and several different sizes (small, medium, and large), which allow you to select a network load balancer size that fits your performance needs.

## What should I do if I'm using ACLs on the subnets that are used to deploy an NLB?
{: #what-should-i-do-if-i-am-using-acls-on-the-subnets-that-are-used-to-deploy-the-load-balancer-nlb}

Make sure that the proper ACL rules are in place to allow incoming traffic for configured listener ports and traffic between a network load balancer and back-end instances should also be allowed.

## Why am I receiving a `401 Unauthorized Error` code?
{: #401-unauthorized-error}
{: faq}

If you are receiving a `401` error code for your network load balancer, check the following access policies for your user:
* The access policy for the load balancer resource type
* The access policy for the resource group

## Why is my network load balancer in `maintenance_pending` state?
{: #maintenance-pending}
{: faq}

An NLB enters a `maintenance_pending` state during various maintenance activities, such as:
* Recovery activities
* Load balancer failover
* Upgrades to address vulnerabilities and apply security patches

## Why is the back-end member health under my pool `unknown`?
{: #back-end-member-health-unknown}
{: faq}

The pool might not associated with any listeners, or configuration changes were made to the pool or its associated listener.

## What are the default settings and allowed values for health check parameters?
{: #what-are-the-default-settings-and-allowed-values-for-health-check-parameters}
{: faq}

The following default settings apply to network load balancer health check parameters:

* **Health check interval:** Five seconds, and the range is 2 - 60 seconds
* **Health check response timeout:** Two seconds, and the range is 1 - 59 seconds
* **Maximum retry attempts:** Two retry attempts, and the range is 1-10 retries

The health check response timeout value must be less than the health check interval value.
{:tip}

## Is the network load balancer IP address fixed?
{: #is-the-network-load-balancer-ip-address-fixed}
{: faq}

Yes. The NLB IP address is fixed.

## Does IBM complete quarterly ASV scans of data-plane LBaaS appliances?  
{: #lbaas-asv}
{: faq}

Approved Scanning Vendor (ASV) quarterly scanning is a requirement of the Payment Card Industry (PCI) Security Standards Council. ASV scanning of LBaaS data-plane appliances is solely a customer responsibility. IBM does not use ASVs to scan data-plane appliances because these scans can negatively impact customer workload functions and performance.
