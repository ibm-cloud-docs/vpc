---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-27"

keywords: vpc, troubleshoot, create

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How can I use the API DEBUG mode?
{: #troubleshoot-debug-1}
{: troubleshoot}
{: support}

You need to use the API DEBUG mode.
{: shortdesc}

You need to use the API DEBUG mode.
{: tsSymptoms}

You have two options for collecting information from your API commands
{: tsCauses}

Add `--verbose` or add the `-i` flag to your `curl` command.
{: tsResolve}

You can add `--verbose` to your `curl` command, then send the support team the `X-Request-Id:` value. This enables support to troubleshoot the problem. The `--verbose` flag is particularly helpful if you are experiencing connectivity problems.

You can also add the `-i` flag to your `curl` command. This enables the support team to see the headers in the response of your request. This flag is helpful in most situations when you need to contact support.

If you have collected information and want to create a support case, see [Creating support cases](/docs/support?topic=support-open-case).
