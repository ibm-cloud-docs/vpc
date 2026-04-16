---

copyright:
  years: 2018, 2026
lastupdated: "2026-04-16"

keywords: application load balancer, limitations, issues, unique combinations, mapping, listener, pool, port

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for application load balancers
{: #lb-limitations}

Known issues are identified bugs or unexpected behaviors that were not fixed before release, but weren’t critical enough to delay it. These issues are communicated to you, often with workarounds, and are prioritized for resolution in the near term by the development team.
{: shortdesc}

Known issues for {{site.data.keyword.alb_full}} (ALB) are as follows:

* Two members with the same instance X and same port Y cannot exist at the same time for an ALB. This means that adding two members with the same server port to an ALB is not allowed. This case is not supported and your traffic might not be routed correctly. To learn more, see [Why can't I add members with the same server port to my ALB?](/docs/vpc?topic=vpc-troubleshoot-lb-alb-members-same-port)

* The default load balancer quota is 50 per region. To increase the number, you must [create a support case](/docs/support?topic=support-open-case).

* When a member target instance is deleted, an Application load balancer pool member is not automatically deleted.
