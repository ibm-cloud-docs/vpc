---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-19"

keywords: load balancer, network, faqs

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I add members with the same server port to my NLB?
{: #troubleshoot-lb-nlb-members-same-port}
{: troubleshoot}
{: support}

Use the following information to troubleshoot an error message that appears when back-end members with the same server port are added to your NLB.
{: shortdesc}

You receive an error message when you try to add the same back-end member with the same port twice in the following circumstances:
{: tsSymptoms}

On the same pool within an NLB:
:    You receive an error message similar to "An instance exists with the same IP address and port value".

On different pools within the same NLB:
:    You receive an error message similar to "Can't update load balancer x pool An error occurred and load balancer pool 'test-pool' wasn't updated. Something went wrong: "Member with target id '02u7_21c4c5c2-6ae7-4d9t-817d-5f9b745ef5f7' and port '22' already exists in a pool".

On different pools on different NLBs: 
:    You receive an error message similar to "Member with target id '02u7_21c4c5c2-6ae7-4d9t-817d-5f9b745ef5f7' and port '22' already exists in a pool".


You can't add the same member with the same server port to an NLB because of the NLB's Direct Server Return (DSR) feature. With DSR, a back-end member responds directly to the consumer. Therefore, it needs a unique member IP and port combination to respond to the consumer properly. For more information, see [Use case 1: Public network load balancer](https://cloud.ibm.com/docs/vpc?topic=vpc-network-load-balancers#nlb-use-case-1).
{: tsCauses}

Choose different ports for the members in your NLB's back-end pool for your NLB to function properly with a passing health check. You can add the same member with different port within the same pool. 
{: tsResolve}
