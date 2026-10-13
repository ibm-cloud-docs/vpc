---

copyright:
  years: 2019, 2026
lastupdated: "2026-10-13"

keywords: load balancer, network, traffic, members

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my front-end load balancer's health check and FQDN resolution failing?
{: #troubleshoot-lb-health-check-fqdn-resolution}
{: troubleshoot}
{: support}

Use the following information to help troubleshoot issue where your front-end load balancer's health check and FQDN resolution are failing.
{: shortdesc}

My front-end load balancer's health check and FQDN resolution are failing.
{: tsSymptoms}

This issue might be caused by the following issue:
{: tsCauses}

* Your security group attached to the front-end load balancer is lacking outbound rules. 

Check and resolve the issue by adding the following outbound rules to the security group attached to your front-end load balancer:
{: tsResolve}


1. Add the following back-end health probe rule to permits the application load balancer (ALB) to send the actual HTTP/TCP health check probes directly to the resolved back-end destination:

- Protocol: 6 TCP
- Destination Type: CIDR block
- Destination: <Attached FQDN's subnet range>
- Port (Min/Max): <Attached FQDN's health check port>


1. Add the following delegated DNS proxy rule to authorize the ALB to communicate with IBM Cloud’s core regional infrastructure DNS proxies to resolve external delegated FQDN domains:

- Protocol: 17 UDP
- Destination Type: CIDR block
- Destination: `161.26.0.0/16`
- Port (Min/Max): 53
