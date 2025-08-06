---

copyright:
  years: 2019, 2025
lastupdated: "2025-08-06"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I update a volume name by using the API or the CLI?
{: #troubleshoot-topic-2}
{: troubleshoot}
{: support}

You receive an error message when you attempt to rename an existing volume.
{: tsSymptoms}

This condition might appear in the API or CLI in the following scenarios.

* In the API, when you send a `PATCH` request to rename the volume.
* In the CLI, when you specify the `ibmcloud is volume-update` command.

You might be renaming the volume with an invalid volume name. In this case, you see a `400 - validation_invalid_name` error.
You might also be specifying a valid volume name, but one that is already in the VPC. For example, if you create two volumes from compute resources that are in the same account, the same region, and have the same name, you see a `400 - volume_name_duplicate` error.
{: tsCauses}

The UI prevents you from entering an invalid volume name.
{: note}

Follow these guidelines for valid volume names:
{: tsResolve}

* Volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters.
* Volume names must begin with a lowercase letter.
* Volume names must be unique across a VPC region.
