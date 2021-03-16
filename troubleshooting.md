---

copyright:
  years: 2018, 2020, 2021
lastupdated: "2021-03-16"

keywords: troubleshoot, tips, error, bearer, API, CLI, problem, debug, token, trace

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:note: .note}
{:tip: .tip}
{:download: .download}
{:troubleshoot: .troubleshoot}

# Troubleshooting your VPC
{: #troubleshooting-vpc}

This document covers common difficulties you might encounter, and offers some helpful tips.
{:shortdesc}


## Using DEBUG mode or `verbose` mode when using the API
{: #troubleshoot-debug}

For example, you can add `--verbose` to your `curl` command and send us the `X-Request-Id:` value so we can troubleshoot the problem. The `--verbose` flag is particularly helpful if you are experiencing connectivity problems.

Another option is to add the `-i` flag to your `curl` command so that the support team can see the headers in the response of your request. This flag is helpful in most situations when you need to contact support.

## API calls require a Bearer token
{: #troubleshoot-api-calls}

When you use the API with a cURL command, you might need to include "Bearer" in the Authorization header, depending on what is in the `$iam_token`. If it includes the word "Bearer", you don't include it again in the header. Most examples assume that "Bearer" is included in the header.


## Turning on `TRACE` mode when using the CLI
{: #troubleshoot-trace}

To turn on `TRACE` (debug) mode when you use the CLI, set `IBMCLOUD_TRACE=true` before the CLI command.

For example:

 ```
IBMCLOUD_TRACE=true ibmcloud is pubgws
```

## Using a different endpoint for CLI
{: #troubleshoot-endpoint-cli}

To change the VPC API endpoint that the {{site.data.keyword.cloud}} CLI uses by default, set the environment variable `IBMCLOUD_IS_API_ENDPOINT` before you use the CLI. For example,

```
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

The service is no longer returning any JSON, instead of just giving an HTTP "401 Unauthorized" to all requests. This error occurs after about an hour if your IAM token has expired. Refresh your IAM token by rerunning `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`.

### Cannot create resources
{: #troubleshoot-cannot-create}

If you cannot create a VPC or other resources, make sure that the owner of the account has granted you the correct [permissions](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources#managing-user-permissions-for-vpc-resources).

### No response from API
{: #troubleshoot-no-response}

If the API is no longer returning any JSON, it is likely your IAM token has expired and needs to be refreshed. Log in to IBM Cloud again or refresh your token by running `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`.


## Cannot delete resources
{: #troubleshoot-cannot-delete}

Certain operations--creating and deleting VSIs, and creating and deleting subnets, for example--are completed asynchronously through the API. Because of this fact, it is recommended to poll the resources you're deleting, to check for deletion before proceeding. 

It can take several minutes for resources to be deleted from the system, due to these asynchronous operations. To facilitate deletion, the best practice is to do things in this order:

1. Delete your instances
2. Delete your public gateways
3. Delete your subnets
4. Delete your VPCs

For more information, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting).
## Trace ID is blank
{: #troubleshoot-trace-id-blank}

Usually, when the Trace ID is blank, it is because the JSON returned doesn't match what is expected from the API. Try running the command `RIAAS_DEBUG=yes ibmcloud is server-rm 3fb7c1eb-45fd-4c85-962e-617f216e7393` (substitute your correct server ID token) and check the output.

## How to manually enable nested virtualization on a virtual server instance?
{: #troubleshoot-enable-nested-virtualization}

Nested virtualization on virtual server instances is disabled by default for now. But you can try enabling it manually by taking the following steps.

The steps documented below may be changed. Please contact your IBM support if you couldn't enable this feature.
{: note}

### Prerequisite

You need to power off all running VMs on the virtual server instance before enabling the nested virtualization feature.

### Steps to enable nested virtualization

1. Access the virtual server instance using SSH or by opening a serial console.

2. Unload the KVM module using `$ sudo modprobe -r kvm_intel`

3. Reload the KVM module with the nested feature enabled using `$ sudo modprobe kvm_intel nested=1`

4. Open "/etc/modprobe.d/kvm.conf" file using `$ sudo vi /etc/modprobe.d/kvm.conf`
  
    **Note:** Create this file if it doesn't exist yet.

5. Add the following line in the file: `options kvm_intel nested=1`

6. Save and close the file.

7. Verify if nested virtualization is enabled using `cat /sys/module/kvm_intel/parameters/nested`

    If it returns "Y" or "1", it means that your system supports nested virtualization. If the output is "N" or "0", your system won't support nested virtualization.

  Try rebooting the virtual server instance if the feature is not enabled.
  {: tip}
