---
copyright:
  years: 2019, 2023
lastupdated: "2023-06-22"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, data center

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to IBM Cloud Object Storage
{: #connecting-vpc-cos}

Use the following information to learn how to connect to {{site.data.keyword.cloud}} Object Storage from your virtual private cloud.
{: shortdesc}


## What is IBM Cloud Object Storage?
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} Object Storage is a web-scale platform that stores unstructured data. It provides reliability, security, availability, and disaster recovery without replication.

Information that is stored with {{site.data.keyword.cloud_notm}} Object Storage is encrypted and dispersed across multiple geographic locations. It is accessible through an implementation of the S3 API. This service uses the distributed storage technologies that are provided by the {{site.data.keyword.cloud}} Object Storage System.

Cloud Object Storage is available with three types of configurations for resiliency: **Cross Region**, **Regional**, and **Single data center**.
- **Cross Region** service provides higher durability and availability than using a single region, at the cost of slightly higher latency. This service is available today in the US, E.U., and A.P. areas.
- **Regional** service reverses the tradeoffs. It distributes objects across multiple availability zones within a single region. It is available in the US, E.U., and A.P. regions. If a region or zone is unavailable, the object store continues to function without impediment.
- **Single data center** service distributes objects across multiple machines within the same physical location. Check this document regularly for available regions.

### Cloud Object Storage direct endpoints for use with VPC
{: #cos-direct-endpoints-for-use-with-vpc}

To send a REST API request, you must set a target endpoint or a URL, and each Cloud Object Storage location must have its own set of URLs. Direct endpoints provide your servers with high-speed, direct connections to {{site.data.keyword.cloud_notm}} services, which incur no added bandwidth costs. With the Cloud Object Storage direct endpoint, a VPC can connect to a Cloud Object Storage bucket located anywhere in the world.

You don't incur charges for traffic from your VPCs to all Cloud Object Storage endpoints that are listed in the following tables.
{: note}

* A VPC client also has access to the Cloud Object Storage bucket over the public endpoint.
* A VPC client has access to the [Cloud Object Storage configuration API](/apidocs/cos/cos-configuration) over the public endpoint and direct endpoint. You can use the Cloud Object Storage configuration API to configure some Cloud Object Storage features on buckets, and to view bucket metadata.
* A VPC client can access a Cloud Object Storage bucket when [context-based restrictions are in place](/docs/cloud-object-storage?topic=cloud-object-storage-setting-a-firewall) by setting the network zone to the VPC ID. VPC clients can't access buckets that are restricted to using a traditional bucket firewall.

## How to connect to IBM Cloud Object Storage from a VPC
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. Provision a Cloud Object Storage instance. For more information, see [Getting started with Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage).
2. Create a Cloud Object Storage bucket in one of the regions listed in the following section.
3. Use the endpoint to communicate with your Cloud Object Storage bucket.

### Regional endpoints
{: #regional-endpoints}

Buckets that are created at a regional endpoint distribute data across three data centers, which are spread across a metropolitan area. Any one of these data centers can suffer an outage, or even destruction, without affecting availability.

| Location| Region | Endpoint |
| ------- | ------ | ------ | 
| US South (Dallas) | us-south | `s3.direct.us-south.cloud-object-storage.appdomain.cloud`| 
| US East (Washington DC) | us-east | `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
| Brazil (São Paulo) | br-sao | `s3.direct.br-sao.cloud-object-storage.appdomain.cloud` |
| Canada (Toronto) | ca-tor | `s3.direct.ca-tor.cloud-object-storage.appdomain.cloud` |
{: class="simple-tab-table"}
{: tab-title="Americas"}
{: caption="Table 1. VPC regional endpoints for North and South America" caption-side="bottom"}
{: summary="This table displays the VPC Regional Endpoints."}
{: tab-group="vpc-reg-endpoints"}
{: #vpc-americas-reg-endpoints}

| Location| Region | Endpoint |
| ------- | ------ | ------ | 
| United Kingdom (London) | eu-gb | `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
| EU Germany (Frankfurt) | eu-de | `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
| Spain (Madrid)       | eu-es | `s3.direct.eu-es.cloud-object-storage.appdomain.cloud` |
{: class="simple-tab-table"}
{: tab-title="Europe"}
{: caption="Table 1. VPC regional endpoints for Europe" caption-side="bottom"}
{: summary="This table displays the VPC Regional Endpoints."}
{: tab-group="vpc-reg-endpoints"}
{: #vpc-europe-reg-endpoints}

|   Location     | Region | Endpoint |
| ------- | ------ | ------ | 
| Japan (Tokyo) | jp-tok | `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |
| Japan (Osaka) | jp-osa | `s3.direct.jp-osa.cloud-object-storage.appdomain.cloud` |
| Australia (Sydney) | au-syd | `s3.direct.au-syd.cloud-object-storage.appdomain.cloud` |
{: class="simple-tab-table"}
{: tab-title="Asia Pacific"}
{: caption="Table 1. VPC regional endpoints for Asia Pacific" caption-side="bottom"}
{: summary="This table displays the VPC Regional Endpoints."}
{: tab-group="vpc-reg-endpoints"}
{: #vpc-asia-pacific-reg-endpoints}

### Cross-region endpoints
{: #cross-region-endpoints}

Buckets that are created at a cross-region endpoint distribute data across three regions. Any one of these regions can suffer an outage or even destruction without affecting availability. Requests are routed to the nearest region's data center by using Border Gateway Protocol (BGP) routing.

If an outage occurs, requests are automatically rerouted to an active region. Advanced users who want to write their own failover logic can do so by sending requests to the specific region's endpoint and bypassing the BGP routing.

| US cross-region access point | Endpoint |
|------------|-------------------------------|
| US | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| Dallas | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud` |
| San Jose | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud` |
{: caption="Table 2. Cross-region endpoints for the US" caption-side="bottom"}

| E.U. cross-region access point | Endpoint |
|------------|-------------------------------|
| E.U. | `s3.direct.eu.cloud-object-storage.appdomain.cloud` |
| Amsterdam | `s3.direct.ams.eu.cloud-object-storage.appdomain.cloud` |
| Frankfurt | `s3.direct.fra.eu.cloud-object-storage.appdomain.cloud` |
| Milan | `s3.direct.mil.eu.cloud-object-storage.appdomain.cloud` |
{: caption="Table 3. Cross-region endpoints for the EU" caption-side="bottom"}

| Asia Pacific cross-region access point | Endpoint |
|------------|-------------------------------|
| A.P. | `s3.direct.ap.cloud-object-storage.appdomain.cloud` |
| Tokyo | `s3.direct.tok.ap.cloud-object-storage.appdomain.cloud` |
| Seoul | `s3.direct.seo.ap.cloud-object-storage.appdomain.cloud` |
| Hong Kong S.A.R. of the PRC | `s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud` |
{: caption="Table 4. Cross-region endpoints for Asia Pacific" caption-side="bottom"}

### Single data center endpoints
{: #single-datacenter-endpoints}

Single data centers aren't colocated with {{site.data.keyword.cloud}} services, such as IAM or Key Protect, and they offer no resiliency if a site has an outage or is destroyed.

If a networking failure results in a partition, such that the data center can't reach a core {{site.data.keyword.cloud}} region for access to IAM, authorization information is read from a cache that might become stale. This action can result in a lack of enforcement of new or altered IAM policies for up to 24 hours.
{: important}

| Region access point | Endpoint |
|------------|-------------------------------|
| Amsterdam, Netherlands | `s3.direct.ams03.cloud-object-storage.appdomain.cloud` |
| Chennai, India | `s3.direct.che01.cloud-object-storage.appdomain.cloud` |
| Hong Kong S.A.R. of the PRC | `s3.direct.hkg02.cloud-object-storage.appdomain.cloud` |
| Melbourne, Australia | `s3.direct.mel01.cloud-object-storage.appdomain.cloud` |
| Mexico City, Mexico | `s3.direct.mex01.cloud-object-storage.appdomain.cloud` |
| Milan, Italy | `s3.direct.mil01.cloud-object-storage.appdomain.cloud` |
| Montréal, Canada | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
| Oslo, Norway | `s3.direct.osl01.cloud-object-storage.appdomain.cloud` |
| San Jose, US | `s3.direct.sjc04.cloud-object-storage.appdomain.cloud` |
| São Paulo, Brazil | `s3.direct.sao01.cloud-object-storage.appdomain.cloud` |
| Seoul, South Korea | `s3.direct.seo01.cloud-object-storage.appdomain.cloud` |
| Toronto, Canada | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
{: caption="Table 5. Single datacenter endpoints" caption-side="bottom"}
