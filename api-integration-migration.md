---

copyright:
  years: 2019
lastupdated: "2019-10-17"

keywords: Generation 1, Generation 2, VPC, VPC on Classic, API, migration, integration, application

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

If you intend to migrate API applications to generation 2 profiles, it is important to understand the current implementation before you modify existing API scripts. 

Generation 1 and generation 2 resources are not compatible and must be created in separate VPCs.
{:tip}

We are always adding new features and support. Over time, there will be fewer considerations between generations of compute resources.
{:note}

| Feature | Client code migration considerations  | 
|-----------------|-------------|
|Pagination | Pagination is not currently supported for instances, instance profiles, or keys.| 
|Network interface filtering | Filtering by subnet is not currently supported. | 
|Naming | The unified naming policy requires that resource names be between 1 and 63 characters. Only numbers, lowercase letters, and hyphens (`-`) are permitted.  Names may not start with a number or `-`, and may not end with `-`. <br> In contrast, first generation resources in the API have different name-length and character-set restrictions, depending on the resource.<br> **Note:** Volumes for generation 2 resources do not adhere to this naming policy. | 
|Instance profiles | CRN support on instance profiles is dropped in this release. <br> **Note:** CRNs are deprecated in the VPC API for use with first generation resources. | 
|Device IDs | Device IDs are not shown when retrieving an instance's volume attachments. <br> **Tip:** Find the device ID in the `volume_attachments` property in the  `GET /volumes/<id>` response.| 
|Security groups | Filtering is not supported on security group lists by VPC name or VPC CRN. <br> A standards-compliant ([RFC 7396](https://tools.ietf.org/html/rfc7396)) API is used for updating security group rules. As a result, some properties have been removed from the update API, and other properties have different semantics from the first generation update API.| 
|VPC routes | For this release:<br> - Multiple routes to the same `destination` (ECMP) are not supported. <br> - Multiple routes to overlapping `destination` ranges are not supported. <br> - Cross-zone routes are not supported. That is, the `next_hop` of a route must be an IP address within a subnet assigned to the route’s `zone`.| 
|Volumes BYOK  | Customer-provided encryption keys (BYOK) are not supported.| 
|Stop-before-delete constraint | In the current release, a virtual server instance must be stopped before it can be deleted.| 
| Instance actions | This release drops support for all deprecated properties in the first generation instance actions API. <br>The `restart` value for `type` is no longer supported.<br> **Tip:** To restart, use value `reboot` with `force` set to `true`. |
|Generation query parameter| The `generation=2` query parameter must be included in API requests to retrieve second generation resources. See [Generation](https://{DomainName}/apidocs/vpc#api-generation-parameter){: external} in the Virtual Private Cloud API.|


See also the following pages:
* [Comparing compute generations in VPC](/docs/overview?topic=overview-compare-vpc-vpcoc)
* [Virtual Private Cloud API](/apidocs/vpc) (for generation 2 virtual server profiles)
* [Virtual Private Cloud API (Gen1 compute)](/apidocs/vpc-on-classic) (for generation 1 virtual server profiles)
