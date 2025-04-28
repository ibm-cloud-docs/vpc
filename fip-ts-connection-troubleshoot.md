---

copyright:
  years: 2025
lastupdated: "2025-04-28"

keywords: troubleshooting floating ip address, floating ip

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I connect to a floating IP after I assign it to my virtual server instance?
{: #fip-connection-troubleshoot}
{: troubleshoot}
{: support}

You are unable to connect to a floating IP after you assign it to a virtual server instance.
{: shortdesc}

The floating IP is successfully assigned, but network traffic to the virtual server instance is blocked or fails to reach the instance.
{: tsSymptoms}

The issue is caused by missing or misconfigured rules in Access Control Lists (ACLs) or security groups that prevent network traffic from reaching the virtual server instance.
{: tsCauses}

Check your ACLs and security groups to confirm that the rules allow connectivity to the virtual server instance.
{: tsResolve}
