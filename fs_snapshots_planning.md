---

copyright:
  years: 2024, 2025
lastupdated: "2025-01-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #fs-snapshots-planning}



When you plan a snapshot strategy for your {{site.data.keyword.filestorage_vpc_short}} shares, you might find this checklist helpful.
{: shortdesc}

## Planning to create and use snapshots
{: #planning-for-fs-snapshots}

Consider the following topics and prerequisites before you create snapshots.

| Item | Considerations |
|------|----------------|
| Interface | Choose between the console, CLI, API, or Terraform to create and manage your snapshots. |
| Share | - Evaluate which shares are most important to snapshot. \n - Evaluate the amount of change that you expect in the data that you intend to snapshot. A share with numerous changes and a lengthy retention period requires more attention than a share with moderate changes. Also, the cumulative size of all snapshots for a share can't exceed 100 TB. |
| Naming conventions | Select a unique name for your snapshot. It's easier to filter and search for them later. For more information, see [Naming snapshots](/docs/vpc?topic=vpc-fs-snapshots-planning#fs-snapshots-naming). |
| Snapshots retention | Evaluate how many snapshots to retain, how long you need to retain them, and when to delete snapshots. Review the [snapshots limitations](/docs/vpc?topic=vpc-fs-snapshots-about#fs-snapshots-limitations) before you perform these actions. |
| Restoring a share | Consider when you might want to create a share from a snapshot. If you need to revert to an earlier version of the share, plan which snapshot you want to use to create the share. |
| Billing | Think about the number of snapshots that you want to keep. You're charged for the storage that is used. |
{: caption="Checklist for planning snapshots of shares" caption-side="bottom"}

## Naming snapshots
{: #fs-snapshots-naming}

Share snapshot names must be unique at the share level. Snapshot names adhere to the same requirements as share names. Valid names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Snapshot names must begin with a lowercase letter and must be unique across the VPC. The UI provides name checking as a convenience. For example, if you end a snapshot name with a hyphen (-), the console notifies you of the error.

## Next Steps
{: #fs-snapshots-planning-next-steps}

After you planned your snapshots, you can [create snapshots](/docs/vpc?topic=vpc-fs-snapshots-create#fs-snapshots-create) of your shares.
