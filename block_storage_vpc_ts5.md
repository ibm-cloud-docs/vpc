---

copyright:
  years: 2019, 2025
lastupdated: "2025-08-06"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are my root keys still registered to volumes and images after I removed IAM authorization from the Cloud Block Storage service?
{: #troubleshoot-topic-5}
{: troubleshoot}

The root keys in the Key management service (KMS) instance remain registered to the deleted Block Storage volume or image resources.
{: tsSymptoms}

If you remove IAM authorization from Cloud Block Storage to the KMS before you delete all BYOK volumes or images, the root key fails to unregister from the resource.
{: tsCauses}

As best practice, delete all storage or image resources before you remove IAM authorization. If you already removed authorization, you must restore the IAM authorization between Cloud Block Storage (source service) and your KMS (target service). For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth) to establish IAM service-to-service authorizations in the console, CLI, or API.
{: tsResolve}
