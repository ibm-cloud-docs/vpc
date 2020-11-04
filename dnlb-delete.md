---

copyright:
  years: 2020
lastupdated: "2020-10-26"

keywords: distributed network load balancer,network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, delete

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

# Deleting a distributed network load balancer
{: #dnlb-deleting}

You can delete an {{site.data.keyword.cloud}} Distributed Network Load Balancer (DNLB) for VPC using the API.
{: shortdesc}

To delete a DNLB by using the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with
the right variables.

2. Store and populate the `lbid` variable to be used in the API commands:

  ```
  bash
  export lbid=<your_distributed_load_balancer_id>
  ```
  {: pre}
    
3. Run the following command to delete the load balancer:

  ```
  bash
  curl -H "Authorization: $iam_token" -X DELETE "$vpc_api_endpoint/internal/v1/distributed_load_balancers/$lbid? version=$api_version&generation=2"
  ```
  {: codeblock}
  
  ```
  bash
  version (*required)(string)
  Requests the version of the API as of a date in the format YYYY-MM-DD. Any date up to the current date may be provided.  Specify the current date to request the latest version.

  generation (*required)(integer)
  The infrastructure generation for the request. For the API behavior documented here, use 2.
  Available values : 2
  ```
  {: codeblock}

  Sample output:
  
  ```
  The load balancer deletion request was accepted.
  ```
  {: pre}

  Sample output, if an error occurred:
  
  ```
  A load balancer with the specified identifier could not be found.
  ```
  {: pre}
