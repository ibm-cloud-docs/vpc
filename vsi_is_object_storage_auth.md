---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-24"

keywords: create authorization for IBM Cloud Object storage, import image to vpc infrastructure, migrate virtual server, migrate instance

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}

# Granting access to {{site.data.keyword.cos_full_notm}} to import images
{: #object-storage-prereq}

To import a custom image to {{site.data.keyword.vpc_short}}, you must have an instance of {{site.data.keyword.cos_full}} 
available. You must also create a bucket in {{site.data.keyword.cos_full_notm}} to store your images. Finally, you must 
create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}.
{:shortdesc}

## Creating an {{site.data.keyword.cos_full_notm}} service instance
{: #migrate-prereq-icos-instance}

If you need to create an instance of {{site.data.keyword.cos_full_notm}}, see 
[Getting started with {{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started).


## Granting access between services
{: #migrate-prereq-create-service-authorization}

From IBM {{site.data.keyword.iamshort}}, you must create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. 

1. From the [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc){: external} menu bar, click **Manage** &gt; **Access (IAM)**, and select **Authorizations**.
2. Click **Create**.
3. Select a source and target service for the authorization. Specify **Infrastructure Service** as the source service. Specify **Image Service for VPC** as the resource type. Specify **Cloud Object Storage** as the target service.
4. Select a role to assign access to the source service that accesses the target service.
5. Click **Authorize**.

For more information, see [Using authorizations to grant access between services](/docs/iam?topic=iam-serviceauth#serviceauth).

## Next steps
{: #next-grant-icos-auth}

When you've completed these steps so that Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}, continue with one of the following topics:
 * [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
 * [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image)
 * [Importing a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image)
