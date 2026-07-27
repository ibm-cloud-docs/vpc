---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-27"

keywords: vpc, troubleshoot, create

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I no longer receiving a response from the API
{: #troubleshoot-no-response-1}
{: troubleshoot}
{: support}

You are no longer receiving a response from the API.
{: shortdesc}

The API is no longer returning any JSON.
{: tsSymptoms}

Your IAM toke expired and needs refreshed.
{: tsCauses}

Use one of the following steps to refresh your token.
{: tsResolve}

- Log out and back in to IBM Cloud.
- Run `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`

For more information, see [Authentication](/docs/apis/vpc/latest#api-authentication) in the VPC API content.
