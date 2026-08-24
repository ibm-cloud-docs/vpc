---

copyright:
  years: 2025, 2026
lastupdated: "2026-08-24"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot, sdp, defined performance, allowlist, boot volume profile, bad_field

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I get a "volume profile does not exist" error when I create an instance or volume?
{: #troubleshoot-sdp-profile-not-allowlisted}
{: troubleshoot}
{: support}

Resolve a `bad_field` error that occurs when the `sdp` volume profile is specified in an API, CLI, or Terraform request by an account that is not allowlisted for the Defined Performance storage feature.
{: shortdesc}

When you provision a virtual server instance or a {{site.data.keyword.block_storage_is_short}} volume by using the API, CLI, or Terraform, the request fails with HTTP status `400` and the following error:
{: tsSymptoms}

```json
{
  "errors": [
    {
      "code": "bad_field",
      "message": "the provided volume profile does not exist",
      "target": {
        "name": "boot_volume_attachment.volume.profile",
        "type": "field"
      }
    }
  ]
}
```
{: screen}

Your account is not allowlisted for the `sdp` (Defined Performance) volume profile. The `sdp` profile is a limited-availability feature. Accounts that are not on the allowlist cannot provision `sdp` volumes. If the profile name `sdp` is specified in the request body, for example, in a script, template, or Terraform configuration, the API rejects the request with a `bad_field` error because the profile does not exist for your account.
{: tsCauses}

Remove the `sdp` profile reference from your request and replace it with a supported profile such as `general-purpose`, `5iops-tier`, `10iops-tier`, or `custom`. For more information about available profiles, see [{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc?topic=vpc-block-storage-profiles).
{: tsResolve}

If your workload requires the Defined Performance profile, contact your IBM representative or submit an [allowlisting request](https://forms.monday.com/forms/6f855ea28400d75ef31e540e39c1d31a?r=use1&SDSallowlist=){: external}.{: external}.
