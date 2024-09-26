---
copyright:
  years: 2019, 2024
lastupdated: "2024-09-26"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, data center

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to {{site.data.keyword.cos_full_notm}}
{: #connecting-vpc-cos}

To use some features of {{site.data.keyword.vpc_short}} such as importing or exporting custom images, or using flow log collectors, you must have a service instance of {{site.data.keyword.cos_full}} available. Use the following information to learn how to connect to {{site.data.keyword.cos_full}} from your virtual private cloud.
{: shortdesc}

{{site.data.keyword.cos_full_notm}} is a web-scale platform that stores unstructured data. It provides reliability, security, availability, and disaster recovery without replication.

Information that is stored with {{site.data.keyword.cloud_notm}} Object Storage is encrypted and dispersed across multiple geographic locations. It is accessible through an implementation of the S3 API. This service uses the distributed storage technologies that are provided by the {{site.data.keyword.cloud}} Object Storage System. For more information, see [What is IBM Cloud Object Storage?](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage). 


## {{site.data.keyword.cos_full_notm}} direct endpoints for use with VPC
{: #cos-direct-endpoints-for-use-with-vpc}

To send a REST API request, you must set a target endpoint or a URL, and each {{site.data.keyword.cos_full_notm}} location must have its own set of URLs. Direct endpoints provide your servers with high-speed, direct connections to {{site.data.keyword.cloud_notm}} services, which incur no added bandwidth costs. With the {{site.data.keyword.cos_full_notm}} direct endpoint, a VPC can connect to an {{site.data.keyword.cos_short}} bucket located anywhere in the world. For more information about resiliency and direct endpoints for {{site.data.keyword.cos_full_notm}}, see [Endpoints and storage locations](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints).

* A VPC client also has access to the {{site.data.keyword.cos_full_notm}} bucket over the public endpoint.
* A VPC client has access to the [{{site.data.keyword.cos_full_notm}} configuration API](/apidocs/cos/cos-configuration) over the public endpoint and direct endpoint. You can use the {{site.data.keyword.cos_full_notm}} configuration API to configure some {{site.data.keyword.cos_full_notm}} features on buckets, and to view bucket metadata.
* A VPC client can access an {{site.data.keyword.cos_full_notm}} bucket when [context-based restrictions are in place](/docs/cloud-object-storage?topic=cloud-object-storage-setting-a-firewall) by setting the network zone to the VPC ID. VPC clients can't access buckets that are restricted to using a traditional bucket firewall.

## How to connect to {{site.data.keyword.cos_full_notm}} from a VPC
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. Provision an {{site.data.keyword.cos_full_notm}} instance. For more information, see [Getting started with {{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage).
2. Create an {{site.data.keyword.cos_full_notm}} bucket.
3. Use the endpoint to communicate with your {{site.data.keyword.cos_full_notm}} bucket.
