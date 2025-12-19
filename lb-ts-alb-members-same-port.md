---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-19"

keywords: load balancer, network, faqs

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I add members with the same server port to my ALB?
{: #troubleshoot-lb-alb-members-same-port}
{: troubleshoot}
{: support}

Use the following information to troubleshoot an error message that appears when back-end members with the same server port are added to your ALB.
{: shortdesc}

An error message displays when you try to add two back-end members with the same port to the same pool within an ALB. In this case, the error reads, "An instance exists with the same IP address and port value".
{: tsSymptoms}

You cannot add two back-end members with the same port to the same pool within an ALB to avoid conflicts in ALB traffic routing. However, you can add the same back-end member with the same port in different ALB backend pools.
{: tsCauses}

Choose different ports for the members in your NLB's back-end pool for your NLB to function properly with a passing health check. You can add the same member with different ports within the same pool. 
{: tsResolve}
