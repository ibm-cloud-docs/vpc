---

copyright:
  years: 2018, 2024
lastupdated: "2024-07-19"

keywords: vpc, troubleshoot, tips, error, bearer, API, CLI, problem, debug, token, trace

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting your VPC
{: #troubleshooting-vpc}

This document covers common difficulties you might encounter, and offers some helpful tips.
{: shortdesc}


## Using DEBUG mode or `verbose` mode when using the API
{: #troubleshoot-debug}
{: api}

For example, you can add `--verbose` to your `curl` command and send us the `X-Request-Id:` value so we can troubleshoot the problem. The `--verbose` flag is particularly helpful if you are experiencing connectivity problems.

Another option is to add the `-i` flag to your `curl` command so that the support team can see the headers in the response of your request. This flag is helpful in most situations when you need to contact support.

## API calls require a Bearer token
{: #troubleshoot-api-calls}
{: api}

When you use the API with a cURL command, you might need to include "Bearer" in the Authorization header, depending on what is in the `$iam_token`. If it includes the word "Bearer", you don't include it again in the header. Most examples assume that "Bearer" is included in the header.


## Turning on `TRACE` mode when using the CLI
{: #troubleshoot-trace}
{: cli}

To turn on `TRACE` (debug) mode when you use the CLI, set `IBMCLOUD_TRACE=true` before the CLI command.

For example:

```sh
IBMCLOUD_TRACE=true ibmcloud is pubgws
```

## Using a different endpoint for CLI
{: #troubleshoot-endpoint-cli}
{: cli}

To change the VPC API endpoint that the {{site.data.keyword.cloud}} CLI uses by default, set the environment variable `IBMCLOUD_IS_API_ENDPOINT` before you use the CLI. For example,

```sh
export IBMCLOUD_IS_API_ENDPOINT=api.dev.domain.com
```
{: pre}


## Common problems
{: #troubleshoot-common}

Here are a few difficulties you might encounter.

### Not authorized (401 or 403 error)
{: #troubleshoot-not-authorized}

Your account might not be authorized for VPC. Make sure that you are using an account that has been onboarded.

### IAM token expired
{: #troubleshoot-token-expired}

The service is no longer returning any JSON, instead of just giving an HTTP "401 Unauthorized" to all requests. This error occurs after about an hour if your IAM token expired. Refresh your IAM token by rerunning `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`.

### Cannot create VPC or other resources
{: #troubleshoot-cannot-create}

If you cannot create a VPC or other resources, make sure that the owner of the account has granted you the correct [permissions](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources#managing-user-permissions-for-vpc-resources).

### No response from API
{: #troubleshoot-no-response}
{: api}

If the API is no longer returning any JSON, it is likely your IAM token expired and needs to be refreshed. Log in to IBM Cloud again or refresh your token by running `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`.


## Cannot delete resources
{: #troubleshoot-cannot-delete}

Certain operations--creating and deleting virtual server instances, and creating and deleting subnets, for example--are completed asynchronously through the API. Because of this fact, it is recommended to poll the resources you're deleting, to check for deletion before proceeding.

It can take several minutes for resources to be deleted from the system, due to these asynchronous operations. To facilitate deletion, the best practice is to do things in this order:

1. Delete your instances
2. Delete your public gateways
3. Delete your subnets
4. Delete your VPCs

For more information, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting).

## Trace ID is blank
{: #troubleshoot-trace-id-blank}

Usually, when the Trace ID is blank, it is because the JSON returned doesn't match what is expected from the API. Try running the command `RIAAS_DEBUG=yes ibmcloud is server-rm 3fb7c1eb-45fd-4c85-962e-617f216e7393` (substitute your correct server ID token) and check the output.

## VPC public gateway drops connection
{: #troubleshoot-public-gateway}

The TCP connection between the IKS cluster and back-end database is dropped by the VPC public gateway during the TCP idle time (for example, 5 minutes).
{: tsSymptoms}

The VPC public gateway has a fixed 4-minute timeout for TCP connections, and it is not configurable. This problem occurs because the Linux default timeout for idle time is 2 hours (only if connection is idle for 2 hours, TCP keep-alive packets are being sent).
{: tsCauses}

Set the idle time to less than 4 minutes (for example, 180 seconds).
{: tsResolve}
