---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-27"

keywords: vpc, troubleshoot, create

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I use the cURL API command
{: #troubleshoot-api-calls}
{: troubleshoot}
{: support}

You are having difficulty in using the cURL API commands.
{: shortdesc}

You are having difficulty in using the cURL API commands.
{: tsSymptoms}

API requests require a Bearer token.
{: tsCauses}

You might need to include `Bearer` in the Authorization header.
{: tsResolve}

Whether or not `Bearer` needs to be included in the Authorization header depends on what is in the `$iam_token`. If `$iam_token` includes the word `Bearer`, you don't need to include it again in the header. The Bearer token looks like this:

```sh
-H "Authorization: Bearer $iam_token"
```
{: pre}

 The API examples in the VPC API reference content assume that "Bearer" is included in the header.

For more information, see [Authentication](/docs/apis/vpc/latest#api-authentication) in the VPC API content.
