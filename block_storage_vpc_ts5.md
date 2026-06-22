---

copyright:
  years: 2019, 2026
lastupdated: "2026-06-22"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are my root keys still registered to volumes and images after I removed IAM authorization from the Cloud Block Storage service?
{: #troubleshoot-topic-5}
{: troubleshoot}

Resolve issues when root keys in your Key Management Service instance remain registered to deleted {{site.data.keyword.block_storage_is_short}} volumes or images after removing IAM authorization.
{: shortdesc}

The root keys in the Key Management Service (KMS) instance remain registered to the deleted {{site.data.keyword.block_storage_is_short}} volume or image resources.
{: tsSymptoms}

If you remove IAM authorization from Cloud Block Storage to the KMS before you delete all BYOK volumes or images, the root key fails to unregister from the resource.
{: tsCauses}

As best practice, delete all storage or image resources before you remove IAM authorization. If you already removed authorization, you must restore the IAM authorization between Cloud Block Storage (source service) and your KMS (target service). For more information, see [Using authorizations to grant access between services](/docs/iam?topic=iam-serviceauth) to establish IAM service-to-service authorizations in the console, CLI, or API.
{: tsResolve}
