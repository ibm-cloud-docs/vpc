---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, unknown status, server status, IMS token, expired token

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are all my virtual server instances showing an `Unknown` status?
{: #troubleshooting-servers-unknown-status}
{: troubleshoot}
{: support}

All of your virtual server instances display an `Unknown` status.
{: shortdesc}

All of your virtual server instances show an `Unknown` status in the console.
{: tsSymptoms}

This issue can be caused by one of the following reasons:

- You do not have adequate permissions to view the server status.
- Your IMS token has expired.
{: tsCauses}

Try one of the following solutions:
{: tsResolve}

Insufficient permissions
:   Make sure that you have the correct permissions to view the server status. For more information, see [Granting user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources#managing-user-permissions-for-vpc-resources)

Expired IMS token
:   Run `bx sl init` again and rebuild the `ims_subject` with the new token. Make sure that you pass the `X-Subject-Token:$ims_subject` parameter in the request header.
