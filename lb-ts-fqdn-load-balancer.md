---

copyright:
  years: 2019, 2026
lastupdated: "2026-10-26"

keywords: load balancer, network, traffic, members

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my front-end application load balancer's health check and FQDN resolution failing?
{: #troubleshoot-lb-health-check-fqdn-resolution}
{: troubleshoot}
{: support}

Use the following information to help troubleshoot an issue where your front-end application load balancer's (ALB) health check and FQDN resolution are failing.
{: shortdesc}

My front-end load balancer's health check and FQDN resolution are failing.
{: tsSymptoms}

This issue might occur if the security group attached to your front-end ALB is missing required outbound rules.
{: tsCauses}
 
To resolve this issue, add the following outbound rules to the security group attached to your front-end ALB:
{: tsResolve}

1. Add the following back-end health probe rule to permit the ALB to send the HTTP/TCP health check probes directly to the resolved back-end destination:

- Protocol: 6 TCP
- Destination Type: CIDR block
- Destination: <attached_fqdn_subnet_range>
- Port (Min/Max): <attached_fqdn_health_check_port>

1. Add the following delegated DNS proxy rule to authorize the ALB to communicate with the {{site.data.keyword.cloud}} core regional infrastructure DNS proxies to resolve external delegated FQDN domains:

- Protocol: 17 UDP
- Destination Type: CIDR block
- Destination: `161.26.0.0/16`
- Port (Min/Max): 53
