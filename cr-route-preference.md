---

copyright:
  years: 2019, 2023
lastupdated: "2023-11-09"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Determining route preference
{: #cr-determining-route-preference}

Designate the route priority (0-4) of VPC routes to determine which routes have a higher priority when there are overlapping/multiple routes for a given destination.
{: shortdesc}

Setting priority values allows you to manage which VPC routing tables can accept/learn routes that are propagated via API from IBM Cloud and third-party services, such as a VPN gateway. You can select whether routes can be propagated to their routing table and which "Route Origin" types are supported. For example, if your traffic traverses through direct links, transit gateways, and the internet, you can determine the preferred order in which traffic is routed in your VPC.

## Route priority rules and considerations
{: #rules-considerations}

*  If multiple routes exist with the same destination CIDR/prefix, the route with the lowest priority value is used. Routes with the same destination CIDR/prefix and priority use Equal Cost Multi-path (ECMP).
* Existing routes and new routes created with a priority value will inherit a 0 default value.
* The next-hop-ip for a route must be in the same zone as the zone the traffic is sourcing from.
* New routes, created without priority value, will automatically be set to have the default priority.
