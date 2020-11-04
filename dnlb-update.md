---

copyright:
  years: 2020
lastupdated: "2020-10-26"

keywords: distributed network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, update

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Updating a distributed network load balancer
{: #dnlb-updating}

You can update an {{site.data.keyword.cloud}} Distributed Network Load Balancer (DNLB) for VPC using the API.

The following example illustrates using the API to update the front-end listener port of a distributed network load balancer. For example, if the front-end listener port was set to 80 and you want to update the port value to 90.

To update a distributed network load balancer by using the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
2. Get the listener id that you will need for the update by doing the following:

  Save the ID of the load balancer:

  ```
  bash
  export lbid="dd754295-e9e0-4c9d-bf6c-58fbc59e5727"
  ```
  {: pre}

  ```
  bash
  curl -H "Authorization: $iam_token" -X PATCH
  "$vpc_api_endpoint/internal/v1/distributed_load_balancers/$lbid?version=$api_version&generation=2"
  ```
  {: codeblock}

  Sample output:
  
  ```
  DistributedLoadBalancer{
          created_at*	string($date-time)
          The date and time that this load balancer was created

          crn*	string
          example: crn:v1:bluemix:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727
          The load balancer's CRN

          hostname	string
          example: myloadbalancer-123456-us-south-1.lb.bluemix.net
          Fully qualified domain name assigned to this load balancer

          href*	string
          example: https://us-south.iaas.cloud.ibm.com/internal/v1/distributed_load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727
          pattern: ^http(s)?:\/\/([^\/?#]*)([^?#]*)(\?([^#]*))?(#(.*))?$
          The load balancer's canonical URL.

          id*	string
          example: dd754295-e9e0-4c9d-bf6c-58fbc59e5727
          maxLength: 64
          minLength: 1
          pattern: ^[-0-9a-z_]+$
          The unique identifier for this load balancer

          is_public*	boolean
          example: true
          The type of this load balancer, public or private

          is_service*	boolean
          Indicates whether this load balancer is a service load balancer.

          listeners*	[
          The listeners of this load balancer

          LoadBalancerListenerReference{...}]
          name*	string
          example: my-load-balancer
          maxLength: 63
          minLength: 1
          pattern: ^([a-z]|[a-z][-a-z0-9]*[a-z0-9])$
          The unique user-defined name for this load balancer

          operating_status*	string
          The operating status of this load balancer

          Enum:
          Array [ 2 ]
          pools*	[
          The pools of this load balancer

          LoadBalancerPoolReference{...}]
          private_ips*	[
          The private IP addresses assigned to this load balancer.

          IP{...}]
          profile*	LoadBalancerProfileReference{
          description:	
          The profile to use for this load balancer

          family*	string
          example: network
          The product family this load balancer profile belongs to

          href*	string
          example: https://us-south.iaas.cloud.ibm.com/internal/v1/distributed_load_balancer/profiles/network-fixed
          pattern: ^http(s)?:\/\/([^\/?#]*)([^?#]*)(\?([^#]*))?(#(.*))?$
          The URL for this load balancer profile

          name*	string
          example: network-fixed
          maxLength: 63
          minLength: 1
          pattern: ^([a-z]|[a-z][-a-z0-9]*[a-z0-9]|[0-9][-a-z0-9]*([a-z]|[-a-z][-a-z0-9]*[a-z0-9]))$
          The name for this load balancer profile

          }
          provisioning_status*	string
          The provisioning status of this load balancer

          Enum:
          Array [ 6 ]
          public_ips*	[
          The public IP addresses assigned to this load balancer. These are applicable only for public load balancers.

          IP{...}]
          resource_group*	ResourceGroupReference{
          description:	
          The resource group for this load balancer

          href*	string
          example: https://resource-controller.cloud.ibm.com/v2/resource_groups/fee82deba12e4c0fb69c3b09d1f12345
          pattern: ^http(s)?:\/\/([^\/?#]*)([^?#]*)(\?([^#]*))?(#(.*))?$
          The URL for this resource group

          id*	string
          example: fee82deba12e4c0fb69c3b09d1f12345
          pattern: ^[0-9a-f]{32}$
          The unique identifier for this resource group

          name*	string
          example: my-resource-group
          maxLength: 40
          minLength: 1
          pattern: ^[a-zA-Z0-9-_ ]+$
          The user-defined name for this resource group

          }
          service_ips	[
          Collection of zonal endpoints for this distributed load balancer

          DistributedLoadBalancerZoneEndpoint{...}]
          subnets*	[
          The subnets this load balancer is part of

          SubnetReference{...}]
          vpc	VPCReference{
          description:	
          The VPC this load balancer belongs to

          crn*	string
          example: crn:v1:bluemix:public:is:us-south:a/123456::vpc:4727d842-f94f-4a2d-824a-9bc9b02c523b
          The CRN for this VPC

          href*	string
          example: https://us-south.iaas.cloud.ibm.com/internal/v1/vpcs/4727d842-f94f-4a2d-824a-9bc9b02c523b
          pattern: ^http(s)?:\/\/([^\/?#]*)([^?#]*)(\?([^#]*))?(#(.*))?$
          The URL for this VPC

          id*	string
          example: 4727d842-f94f-4a2d-824a-9bc9b02c523b
          maxLength: 64
          minLength: 1
          pattern: ^[-0-9a-z_]+$
          The unique identifier for this VPC

          name*	string
          example: my-vpc
          maxLength: 63
          minLength: 1
          pattern: ^([a-z]|[a-z][-a-z0-9]*[a-z0-9])$
          The unique user-defined name for this VPC

          resource_type*	string
          The resource type

          Enum:
          Array [ 1 ]
          }
          }
  ```
  {: screen}
