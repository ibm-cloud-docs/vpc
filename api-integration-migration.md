---

copyright:
  years: 2019, 2021
lastupdated: "2021-05-21"

keywords: Generation 1, Generation 2, API, migration, update, scripts, integration, application

subcollection: vpc

---

<!-- Common attributes used in the template are defined as follows: -->
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# API application migration considerations
{: #api-integration-migration}

If you intend to update API applications to generation 2 profiles, you must understand the current implementation before you modify existing API scripts.

Generation 1 and generation 2 resources are not compatible and must be created in separate VPCs.
{:important}

We are continually adding [new features and support](/docs/vpc?topic=vpc-api-change-log).
{:note}

## Pagination
{: #pagination}

Pagination is not currently supported for instance profiles or keys.

## Network interface filtering 
{: #network-interface-filtering}

Filtering by subnet is not currently supported.

## Network access control list (ACL) rules 
{: #network-acl-rules}

A standards-compliant ([RFC 7396](https://tools.ietf.org/html/rfc7396)) API is used for updating network rules. As a result, some properties were removed from the update API, and other properties have different semantics from the update API for generation 1 resources. For example, network ACLs are scoped to a VPC, rather than to a region. This change has several implications: 

- Network ACL resource names need to be unique only within a VPC.
- At VPC creation, an existing network ACL might not be associated. However, the VPC’s default network ACL can be configured.
- At VPC deletion, any network ACLs associated with that VPC are also deleted.

If a network ACL is created with no rules, all traffic is denied. This behavior differs from first-generation behavior, which allows all traffic. Rules are not automatically added when a network ACL is created. 

The `port_min` field was renamed to `destination_port_min` and the `port_max` field was renamed to `destination_port_max`, for network ACL rules.

## Resource naming 
{: #resource-names}

The unified naming policy requires that resource names be in the range 1 - 63 characters. Only numbers, lowercase letters, and hyphens (`-`) are allowed. Names can't start with a number or `-`, and can't end with `-`. 

In contrast, first-generation resources in the API have different name-length and character-set restrictions, depending on the resource.

Volume names must begin with an alphabetic character. 
{: note}

## Instance profiles 
{: #instance-profiles}

CRN support on instance profiles is dropped in this release. 

CRNs are deprecated in the VPC API for use with first-generation resources. 
{: note}

## Security groups 
{: #security-groups}

A standards-compliant ([RFC 7396](https://tools.ietf.org/html/rfc7396)) API is used for updating security group rules. As a result, some properties were removed from the update API, and other properties have different semantics from the first-generation update API.

## VPC routes
{: #vpc-routes}

In this release:
- Multiple routes to the same `destination` (ECMP) are not supported. 
- Multiple routes to overlapping `destination` ranges are not supported. 
- Cross-zone routes are not supported. That is, the `next_hop` of a route must be an IP address within a subnet that is assigned to the route’s `zone`.

## Volumes BYOK  
{: #volumes-byok}

Customer-provided encryption keys (BYOK) are not supported.

## Instance actions 
{: #instance-actions}

This release drops support for all deprecated properties in the first-generation instance actions API. <br>The `restart` value for `type` is no longer supported.

To restart, use value `reboot` with `force` set to `true`. 
{: tip}

## Generation query parameter
{: #generation-query-parameter}

The `generation=2` query parameter must be included in API requests to retrieve second-generation resources. See [Generation](https://{DomainName}/apidocs/vpc#api-generation-parameter){: external} in the Virtual Private Cloud API.


See also the following resources:
* [Start your migration](https://www.ibm.com/cloud/blog/announcements/start-your-vpc-gen1-to-vpc-gen2-migration){:external} blog
* [Comparing compute generations in VPC](/docs/cloud-infrastructure?topic=cloud-infrastructure-compare-vpc-vpcoc)
* [Virtual Private Cloud API](/apidocs/vpc) (for generation 2 virtual server profiles)
* [Virtual Private Cloud API (Gen1 compute)](/apidocs/vpc-on-classic) (for generation 1 virtual server profiles)
