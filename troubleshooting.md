---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: vpc, troubleshoot, tips, error, bearer, API, CLI, problem, debug, token, trace

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:troubleshoot: .troubleshoot}

# Troubleshooting your VPC
{: #troubleshooting-vpc}

This document covers common difficulties you might encounter, and offers some helpful tips.
{:shortdesc}


## Using DEBUG mode or `verbose` mode when using the API
{: #troubleshoot}

For example, you can add `--verbose` to your `curl` command and send us the `X-Request-Id:` value so we can troubleshoot the problem. The `--verbose` flag is particularly helpful if you are experiencing connectivity problems.

Another option is to add the `-i` flag to your `curl` command so that the support team can see the headers in the response of your request. This flag is helpful in most situations when you need to contact support.

## API calls require a Bearer token
{: #troubleshoot}

When using the API with a cURL command, you may need to include "Bearer" in the Authorization header, depending on what is in the `$iam_token`. If it includes the word "Bearer" you don't include it again in the header. Most of our examples assume that "Bearer" is included in the header.


## Turning on `TRACE` mode when using the CLI
{: #troubleshoot}

To turn on `TRACE` (debug) mode when using the CLI, set `IBMCLOUD_TRACE=true` before the CLI command.

For example:

 ```
IBMCLOUD_TRACE=true ibmcloud is pubgws
```

## Using a different endpoint for CLI
{: #troubleshoot}

To change the VPC API endpoint that the {{site.data.keyword.cloud}} CLI uses by default, set the environment variable `IBMCLOUD_IS_API_ENDPOINT` before using the CLI. For example,

```
export IBMCLOUD_IS_API_ENDPOINT=api.dev.domain.com
```
{: pre}


## Common problems
{: #troubleshoot}

Here are a few difficulties you might encounter.

### Not authorized (401 or 403 error)
{: #troubleshoot}

Your account may not be authorized for VPC. Make sure you are using an account that has been onboarded. 

### IAM token expired
{: #troubleshoot}

The service is no longer returning any JSON, instead of just giving an HTTP "401 Unauthorized" to all requests. This error will happen after about an hour if your IAM token has expired. Refresh your IAM token by re-running `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`.

### Cannot create resources
{: #troubleshoot}

If you cannot create a VPC or other resources, make sure the owner of the account has granted you the correct [permissions](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources#managing-user-permissions-for-vpc-resources).

### No response from API
{: #troubleshoot}

If the API is no longer returning any JSON, it is likely your IAM token has expired and needs to be refreshed. Log in to IBM Cloud again or refresh your token by running `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`.


## Cannot delete resources
{: #troubleshoot}

Certain operations--creating and deleting VSIs, and creating and deleting subnets, for example--are completed asynchronously through the API. Because of this fact, it is recommended to poll the resources you're deleting, to check for deletion before proceeding. 

It can take several minutes for resources to be deleted from the system, due to these asynchronous operations. To facilitate deletion, the best practice is to do things in this order:

1. Delete your instances
2. Delete your public gateways
3. Delete your subnets
4. Delete your VPCs

For more information, see [Deleting a VPC](/docs/vpc?topic=vpc-delete-vpc).
## Trace ID is blank
{: #troubleshoot}

Usually, when the Trace ID is blank, it is because the JSON returned doesn't match what is expected from the API. Try running the command `RIAAS_DEBUG=yes ibmcloud is server-rm 3fb7c1eb-45fd-4c85-962e-617f216e7393` (substitute your correct server ID token) and check the output.

