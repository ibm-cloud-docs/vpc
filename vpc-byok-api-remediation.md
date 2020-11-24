---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-20"

keywords: block storage, virtual private cloud, block storage volume, instances, virtual server instance, customer-managed encryption, BYOK

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

# API changes for VPC resources using customer-managed encryption
{: #byok-api-remediation-plan}

{{site.data.keyword.vpc_full}} (VPC) has some new features in development for customer-managed encryption. We want to make sure that behavior changes we're making to API, CLI, and Terraform does not cause disruption for you.
{:shortdesc}

Upcoming enhancements to customer-managed encryption add a new enumeration status to the image and volume APIs. The new status occurs when you delete or disable a customer root key (CRK). Your custom images and block storage volumes will go into an unusable state, with a status code `unusable` in the API, along with associated reason codes `encryption_key_deleted` or `encryption_key_disabled`. 

When this happens, your image and volume resources become unusable for normal operations unless you [restore](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-restore-root-key) the deleted root key or enable a disabled root key. If you don't restore or enable the root key, several consequences result. For example, any virtual server instances with attached volumes in an unusable state are stopped. For more information about this and other actions that occur when you disable or delete a root key, see the following topics:

* [Disabling root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys)
* [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys)

If you've written custom integrations using the VPC API, CLI, or Terraform, we recommend that you prepare your code for the new values based on the behavior described in this topic.

We have also included this notice in our [API change log](/docs/vpc?topic=vpc-api-change-log#upcoming-changes).

For further guidance, contact IBM [customer support](/docs/get-support?topic=get-support-using-avatar).
