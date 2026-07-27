---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-27"

keywords: vpc, troubleshoot, create

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Where can I find information on API error handling
{: #troubleshoot-api-error-handling-1}
{: troubleshoot}
{: support}

This API uses standard HTTP response codes to indicate the outcome of a request. A 4xx-series response indicates a failure that the client must resolve. A 5xx-series response indicates a service failure.
{: shortdesc}

You are having difficulty in using the cURL API commands.
{: tsSymptoms}

API calls require a Bearer token
{: tsCauses}

When you use the API with a cURL command, you might need to include `Bearer` in the Authorization header. Whether or not `Bearer` needs to be included in the Authorization header depends on what is in the `$iam_token`. If `$iam_token` includes the word `Bearer`, you don't need to include it again in the header. The API examples in the VPC API reference content assume that "Bearer" is included in the header.
{: tsResolve}

For more information, see [Error handling](/docs/apis/vpc/latest#api-http-response-codes) in the VPC API content.
