---

copyright:
  years: 2019 - 2021
lastupdated: "2021-01-26"

keywords: block storage, virtual private cloud, block storage volume, instances, virtual server instance, customer-managed encryption

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# API changes for VPC resources using block storage volumes
{: #byok-block-storage-remediation}

{{site.data.keyword.vpc_full}} (VPC) has some new features in development for block storage volumes. We want to make sure that behavior changes we're making to API, CLI, and Terraform does not cause disruption for you.
{:shortdesc}

Upcoming enhancements to block storage volumes add a new enumeration status to the volume APIs. The new status occurs when you expand an existing volume. Your block storage volumes will go into an updating state and show the API status code `updating`. You can still access the data while the volume is being resized. For more information on this feature, see [Expanding block storage volume capacity (Beta](/docs/vpc?topic=vpc-expanding-block-storage-volumes)

If you've written custom integrations using the VPC API, CLI, or Terraform, we recommend that you prepare your code for the new values based on the behavior described in this topic.

We have also included this notice in our [API change log](/docs/vpc?topic=vpc-api-change-log#upcoming-changes).

For further guidance, contact IBM [customer support](/docs/get-support?topic=get-support-using-avatar).
