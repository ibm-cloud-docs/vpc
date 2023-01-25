---

copyright:
  years: 2019, 2023
lastupdated: "2023-01-25"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing block storage volumes
{: #managing-block-storage}

You can manage your block storage volumes in the UI, from the CLI, or with the API. You can detach a volume from a virtual server instance or transfer a volume from one instance to another. You can attach a previously attached volume or rename a volume. You can set automatic volume deletion or manually delete a volume. You can assign access to a volume, and you can access volume read/write metrics for monitoring performance. Apply user tags that are associated with a backup policy to a volume to create automated backups.
{: shortdesc}

## Manage block storage volumes in the UI
{: #manage-block-storage-vol-UI}
{: ui}

Use the UI to manage your block storage volumes. You can:

* Detach a volume from a virtual server instance.
* Transfer a volume from one instance to another.
* Attach a previously attached block storage data volume.
* Rename a block storage volume.
* Delete a block storage data volume.

### Detach a block storage volume from a virtual server instance
{: #detach}
{: help}
{: support}

You can detach a block storage volume that is attached to a virtual server instance. Detaching frees the volume for use by another instance.

To detach a volume:

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Locate the volume and then, click the overflow icon (...) to open a list of options.

1. From the options menu, click **Detach from instance**.
1. Confirm by clicking **Detach instance** in the open window.

Alternatively, you can click an individual volume in the list of all block storage volumes and go to the **Volume Details** page for that volume. Under **Attached instances**, click the minus sign next to the virtual server instance to detach the volume from that instance.

When you create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, detaching the data volume that is attached to a running instance causes the workload that's running on the instance to fail, therefore it is recommended that you do not detach the data volume.
{: note}

### Transfer a block storage volume from one virtual server instance to another
{: #transfer}

To transfer a block storage volume to another virtual server instance:

1. [Detach the volume from its virtual server instance](#detach).
1. Go to the virtual server instance to which you want to transfer the volume. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Select a virtual server instance from the list.
1. Under Attached Storage volumes, click the plus sign to add a volume. All block storage volumes are displayed.
1. From the list of volumes, select the volume that you previously detached.

### Attach a previously attached block storage data volume
{: #reattach}

A block storage data volume is attached by default when you provision the volume during virtual server instance creation. When you detach a volume from an instance, it exists as an unattached volume and is displayed in the list of [all block storage volumes](/docs/vpc?topic=vpc-viewing-block-storage#viewvols). You can attach it to another instance from the list of block storage volumes.

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Locate the volume and then click overflow icon (...) to open a list of options.
1. From the options menu, click **Attach to instance**.
1. Select an available virtual server instance.
1. Confirm your selection.

### Rename a block storage volume
{: #rename}

You can change the name of an existing volume to make it more meaningful.

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Locate the volume and then click the name of the volume to go to the Volume Details page.
1. Click the pencil icon after the name of the volume to edit the name. Provide a [valid volume name](/docs/vpc?topic=vpc-managing-block-storage#volume-name-conventions).
1. Confirm your edit.

### Guidelines for naming volumes
{: #volume-name-conventions}

Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter.

Volume names must be unique across the entire VPC infrastructure. For example, if you create two volumes with the same name in the same account and region, a "volume name duplicate" error is triggered.

### Delete a block storage data volume
{: #delete}
{: help}
{: support}

Deleting a block storage volume completely removes its data. The volume cannot be restored.

You cannot delete an active block storage volume. To delete a volume, first [detach it](#detach) from the virtual server instance. If you took snapshots of the volume, all snapshots must be in a `stable` state.

To delete a volume:

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Locate the volume that you want to delete and then click the overflow icon (...) to open a list of options.
1. From the options menu, click **Delete**.
1. Confirm the deletion.

### Automatically delete block storage data volumes
{: #auto-delete}

By using the Auto Delete feature, you can specify that a block storage data volume is automatically deleted when you delete an instance to which it is attached.

You don't need to set automatic deletion for boot volumes. Boot volumes are created during instance creation and automatic deletion is enabled by default. When you delete the instance, the boot volume is also deleted.
{: note}

To enable Auto Delete for an existing block storage data volume that is attached to an instance, follow these steps:

1. Locate the virtual server instance to which the data volume is attached. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Under **Attached block storage volumes**, select a volume.
1. On the next page, click **Auto Delete** to enable.
1. Confirm your selection.

Alternatively, select a data volume from the list of block storage volumes (**Storage > Block storage volumes**). On the volume details page, under **Attached instances**, click the **Auto delete** toggle to enable or disable automatic deletion.

You can also enable Auto Delete on a new data volume when you create an instance. For more information, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).

### Add user tags to a block storage volume
{: #add-user-tags-volumes-ui}

Add user tags to block storage volumes from the list of volumes or the volumes details page.

1. Go to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Locate the volume from the list that you want to add user tags.
3. In the **tags** column, click **Add tags**.
4. In the Add tags menu, enter the user tags that you want to apply to this volume. Tags display as you type.

   You can also add **access management tags** to a volume from the Add tags menu. For more information about creating and adding access management tags, see [Apply access management tags to a volume](#storage-add-access-mgt-tags).
   {: note}

5. When you're done adding tags, click **Save**. When you refresh the screen, the list of block storage volumes shows the number of tags that are added in the **Tags** column.

You can also add tags from the volume details page:

1. Go to the list of block storage volumes.
2. On the volume details, click **Add tags** next to the volume name.
3. In the Add tags menu, enter the user tags that you want to apply to this volume. When finished, click **Save**.

### Add user tags that are associated with a backup policy to a volume in the UI
{: #apply-tags-volumes-ui}

You can add user tags that are associated with a backup policy to a block storage volume. Backup policies schedule automatic creation of backup snapshots. When one volume tag matches a backup policy tag for target resources, it triggers a backup of the volume contents. A backup policy defines a backup plan that schedules when backup snapshots are taken.

From the [volume details](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#view-vol-details-ui) page, you can view the backup policies that are applied to the volume and add user tags that are associated with a backup policy.

1. Go to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Locate the volume that you want and click the name link.
3. From the block storage volumes details page, click the **Backup policies** tab.
4. Click **Attach**.
5. In the side panel, select a backup policy from the list of available policies, and then select the policy tags to apply to the volume. You can also view the plan details that can help you decide whether to use that policy.
6. Click **Apply policy and tags**. The backup policy shows in the list of backup policies that are associated with the volume.

When you go to the [backup policy page](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-vol-backup-policies), the volume for which you added tags shows up in the list of volumes.

For more information about creating backups, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-about). For more information about user tags, see [Working with tags](/docs/account?topic=account-tag).

## Manage block storage volumes from the CLI
{: #managing-block-storage-cli}
{: cli}

Manage your block storage volumes from the command-line interface (CLI). You can update a volume name, update a volume attachment, detach a volume, and delete a volume.

Before you begin, [install the IBM Cloud CLI and VPC CLI plug-in](/docs/vpc?topic=vpc-creating-block-storage#before-creating-block-storage-cli).

### Update a volume name
{: #update-vol-name-cli}

To change a volume name, specify either the volume name or ID and then indicate the new name. The volume name can be up to 63 alpha-numeric characters and include special characters, and must begin with a lowercase letter. The volume name must be unique across the VPC infrastructure.

```zsh
ibmcloud is volume-update VOLUME_ID [--name NEW_NAME] [--json]
```
{: pre}

Example:

```bash
$ ibmcloud is volume-update 933c8781-f7f5-4a8f-8a2d-3bfc711788ee --name demo-volume-update
Updating volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account MyAccount 01 as user user1@mycompany.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo-volume-update
Capacity                                100
IOPS                                    1000
Profile                                 5iops-tier
Encryption Key                          -
Encryption                              provider-managed
Status                                  available
Created                                 2022-04-22 10:09:28
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Zone                                    us-south-2
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Volume Attachment Instance Reference    none
```
{: screen}

### Update a volume attachment from the CLI
{: #update-vol-attachment-cli}

You can update the volume attachment name and change the default auto delete setting with the `instance-volume-attachment-update` command.

```zsh
ibmcloud is instance-volume-attachment-update INSTANCE_ID VOLUME_ATTACHMENT_ID [--name NEW_NAME] [--auto-delete true | false] [--json]
```
{: pre}

Use the `--name` parameter and specify a new name for the volume attachment.

Specify`--auto-delete true` to automatically delete a volume that is attached to an instance, when you delete the instance.

Example showing details for `Volume Attachment Instance Reference`:

```text
VDisk Name    VDisk ID                                    VDisk type   Auto delete   Instance name   Instance ID
VDisk-data1   0738-fd146b1f-e1bb-4eab-ba78-3109e6bc3a2d   data         true          vsi-test1       0738-8b56da93-7990-4ccf-9dc5-5aee6a5f08f9
```
{: screen}

### Detach a volume from the CLI
{: #detach-vol-attachment-cli}
{: help}
{: support}

Use the `instance-volume-attachment-detach` command to detach a volume from an instance and delete the volume attachment. The block storage volume is not deleted; you can later [attach it to another instance](/docs/vpc?topic=vpc-attaching-block-storage-cli).

```text
ibmcloud is instance-volume-attachment-detach INSTANCE_ID VOLUME_ATTACHMENT_ID [-f, --force]
```
{: pre}

### Delete a block storage volume from the CLI
{: #delete-vol-cli}
{: help}
{: support}

Use the `volume-delete` command and specify the volume ID to delete a block storage volume.

You cannot delete an active block storage volume. You must first [detach it from the virtual server](#detach-vol-attachment-cli).
{: note}

```zsh
ibmcloud is volume-delete (VOLUME_NAME | VOLUME_ID) [-f, --force]
```
{: pre}

Example:

```bash
$ ibmcloud is volume-delete 64d85f0f-6c08-4188-9e9a-0057b3aa1b69
This deletes volume 64d85f0f-6c08-4188-9e9a-0057b3aa1b69 and cannot be undone. Continue?> y
Deleting volume 64d85f0f-6c08-4188-9e9a-0057b3aa1b69 under account MyAccount 01 as user user1@mycompany.com...
OK
Volume ID 0738-64d85f0f-6c08-4188-9e9a-0057b3aa1b69 is deleted.
```
{: screen}


## Manage block storage volumes with the API
{: #managing-block-storage-api}
{: api}

Manage your block storage volumes programmatically by making calls to the [VPC REST APIs](/apidocs/vpc). You can update a volume name, update a volume attachment, detach a volume, and delete a volume.

### Update the name of a volume with the API
{: #update-vol-name-api}

Make a `PATCH /volumes/{id}` call and specify a new name for the volume.

```curl
curl -X PATCH "$vpc_api_endpoint/v1/volumes?version=2022-04-22&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-volume-4-update"
    }'
```
{: pre}

A successful response looks like the following example:

```json
{
  "capacity": 50,
  "created_at": "2022-04-22T23:16:53.000Z",
  "crn": "crn:[...]",
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
  "id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
  "iops": 100,
  "name": "my-volume-4-update",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/custom",
    "name": "custom"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
    "id": "4bbce614c13444cd8fc5e7e878ef8e21",
    "name": "Default"
  },
  "status": "available",
  "status_reasons": [],
  "volume_attachments": [],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
    "name": "us-south-2"
  }
}
```
{: codeblock}

### Update a volume attachment with the API
{: #update-vol-attachment-api}

Make a `PATCH /instances` call and specify the ID of the new volume attachment.

```zsh
PATCH /instances/{instance_id}/volume_attachments/{id}
```
{: pre}

```curl
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id/volume_attachments/$volume_attachment_id?version=2022-04-22&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "delete_volume_on_instance_delete": false,
      "name": "my-volume-attachment-data-5iops-updated"
    }'
```
{: pre}

A successful response looks like the following example:

```json
{
  "created_at": "2022-04-22T16:35:47.000Z",
  "delete_volume_on_instance_delete": false,
  "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/8f06378c-ed0e-481e-b98c-9a6dfbee1ed5/volume_attachments/9f2a645e-19c1-4f8f-b062-46b9e0671999",
  "id": "9f2a645e-19c1-4f8f-b062-46b9e0671999",
  "name": "my-volume-attachment-data-5iops-updated",
  "status": "attached",
  "type": "data",
  "volume": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/d8b26921-1409-4c2f-9b46-39b5b6e0b945",
    "id": "d8b26921-1409-4c2f-9b46-39b5b6e0b945",
    "name": "my-volume-data-5iops"
  }
}
```
{: codeblock}


### Detach a volume with the API
{: #detach-vol-attachment-api}

Make a `DELETE /instances` request and specify the volume attachment ID to delete a volume attachment. Deleting a volume attachment detaches a volume from an instance.

```zsh
DELETE /instances/{instance_id}/volume_attachments/{id}
```
{: pre}

```curl
curl -X DELETE "$vpc_api_endpoint/v1/instances/$instance_id/volume_attachments/$volume_attachment_id?version=2022-04-22&generation=2" \
-H "Authorization: $iam_token"

```
{: codeblock}

Verify that the volume is detached from the instance by making a `GET /instances/{instance_id}`
call.

### Delete a block storage volume with the API
{: #delete-vol-api}

Make a `DELETE /volumes/{id}` call.

```curl
curl -X DELETE "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-04-22&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

To verify that the volume is deleted, list the volumes by making a `GET /volumes` call.

### Apply user tags that are associated with a backup policy to a volume with the API
{: #block-storage-add-tags-api}

To add user tags to a volume, you first make a `GET /volumes/{volume_id}` call and copy the hash string from `Etag` property in the response header. You then use the hash string when you specify `If-Match` in a `PATCH /volumes/{volume_id}` request to create new user tags.

For more information and examples, see [Apply tags to block storage volumes](/docs/vpc?topic=vpc-backup-use-policies&interface=api#backup-apply-tags-volumes-api) in the VPC backup service documentation.

## Apply access management tags to a block storage volume
{: #storage-add-access-mgt-tags}

Access management tags are metadata that you can add to your block storage volumes to help organize access control resource relationships. You first create the tag and then apply it to a new volume or to an existing volume. You can apply the same access management tag to multiple block storage volumes. You then assign access to the tag in IAM. Optionally, you can create an IAM access group and manage users.

Access management tags are not used by [backup policies](/docs/vpc?topic=vpc-backup-use-policies) to create backup snapshots. Backup snapshots are created when user tags match backup policy tags for target resources to volume user tags.
{: note}

### Step 1 - Create an access management tag in IAM
{: #storage-create-access-mgt-tag}

In the UI:

1. Go to **Manage > Account**, and then select **Tags**.
2. Click the **Access management tags** tab. Add a tag name in the field. Access management tags require a `key:value` format.
3. Click **Create Tags**.

With the [Global Search and Tagging API](/docs/account?topic=account-tag&interface=api#create-access-api):

Make a `POST/ tags` call to [create an access management tag](/apidocs/tagging#create-tag) and specify the tag in the `tag_names` property. For an example, see [Creating access management tags by using the API](/docs/account?topic=account-tag&interface=api#create-access-api).

### Step 2 - Add an access management tag to a volume
{: #storage-add-access-mgt-tag}

Add an access management tag to an existing volume or when you [create a volume](/docs/vpc?topic=vpc-creating-block-storage). To add access management tags to an existing volume:

1. Go to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Locate the volume from the list.
3. In the **tags** column, click **Add tags**.
4. In the Add tags menu, enter the access management tags in the access management tag field. Tags that you created display as you type.
5. Click **Save**.

### Additional steps
{: #storage-access-mgt-additional-steps}

After you create an access management tag and apply it to a volume, complete the following steps to assign access and add users:

1. [Create an access group](/docs/account?topic=account-access-tags-tutorial#tagging-create-access-group). Access groups are assigned policies that grant roles and permissions to the members of that group. You assign access to the specific access management tags for the block storage service. For more information about access groups, see [Setting up access groups](/docs/account?topic=account-groups&interface=ui).

2. [Assign an access policy to a group](/docs/account?topic=account-access-tags-tutorial#tagging-assign-policy).

3. [Add users to the access group](/docs/account?topic=account-access-tags-tutorial#tagging-add-users-access-group).

When you look at the specific resources for the VPC infrastructure and specify {{site.data.keyword.block_storage_is_short}} as the resource type, you can see the access management tags for the block storage service.

## Access volume read/write metrics
{: #block-storage-metrics}

You can view read/write metrics for your block storage volumes that are attached to a virtual server instance:

* [Cumulative number of bytes read for a volume](/docs/vpc?topic=vpc-vpc-monitoring-metrics#bytes-read-for-volume-gen2) since the virtual server instance started.
* [Cumulative number of bytes written for a volume](/vpc?topic=vpc-vpc-monitoring-metrics#bytes-written-for-volume-gen2) since virtual server instance started.

## Block storage data eradication
{: #block-storage-data-eradication}

When you delete a block storage volume, that data immediately becomes inaccessible. All pointers to the data on the physical disk are removed. If you later create a new volume in the same or another account, a new set of pointers is assigned. The account can't access any data that was on the physical storage because those pointers are deleted. When new data is written to the disk, any inaccessible data from the deleted volume is overwritten.

IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten and eradicated. Further, when you delete a block storage volume, those blocks must be overwritten before that block storage is made available again, either to you or to another customer.

Further, when IBM decommissions a physical drive, the drive is destroyed before disposal. Decommissioned drives are unusable and any data on them is inaccessible.

## Sanitize your data before you delete a volume
{: #block-storage-sanitization}

When you delete a block storage volume, IBM guarantees that your data is inaccessible on the physical disk and is eventually [eradicated](#block-storage-data-eradication). If you have extra compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For more information, see the [NIST 800-88 Guidelines for Media Sanitation](https://csrc.nist.gov/publications/detail/sp/800-88/rev-1/final){: external}.

Sanitizing and deleting the volume means your data can't be restored.

## Assign access to a block storage volume
{: #assign-volume-access}

With Administrator privileges, you can assign volume-level user access to the {{site.data.keyword.block_storage_is_short}} service. The following table shows the information that you must provide in the [Identity and Access Management (IAM) UI](/docs/account?topic=account-account_setup).

| IAM field | Action |
|--------|-------------|
| Services | Select **Infrastructure Services**. |
| Resource Type | Select **Block Storage for VPC**. |
| Volume ID | Enter the [volume ID](/docs/vpc?topic=vpc-viewing-block-storage#view-vol-details) to assign access to a specific volume. |
| Select Roles | Assign platform access roles, typically, Operator. |
{: caption="Table 1. Information for IAM" caption-side="bottom"}

For more information, see the [best practices for assigning access](/docs/account?topic=account-account_setup#account_setup). For the complete IAM process, which includes inviting users to your account and assigning Cloud IAM access, see the [IAM getting started tutorial](/docs/account?topic=account-iamoverview).

## Monitor block storage performance
{: #block-storage-monitor}

You can monitor certain block storage volume performance from the VPC virtual server instance metrics. These metrics include:

* Cumulative number of [bytes read for a volume](/docs/vpc?topic=vpc-vpc-monitoring-metrics#bytes-read-for-volume-gen2) since the virtual server instance was started.

* Cumulative number of [bytes written for a volume](/docs/vpc?topic=vpc-vpc-monitoring-metrics#bytes-written-for-volume-gen2) since the virtual server instance was started.

To see these metrics in the UI:

1. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **VPC Infrastructure > Compute > Virtual server instances**.

2. Click the instance name to go to the instance details.

3. Click the **Monitoring** tab and scroll to the **Volume** metrics.

4. Select a volume. The read and write metrics are displayed. Figure 1 shows this view.

![Figure showing volume metrics.](/images/volume-read-write-metrics.png "Figure showing volume read/write metrics."){: caption="Figure 1. Read/write metrics for block storage volumes." caption-side="bottom"}

## Volume performance when you restore from a snapshot
{: #block-vol-restore-snap}

Boot and data volume performance is initially degraded when you restore them from a snapshot. During the restoration, your data is copied from {{site.data.keyword.cos_full}} to {{site.data.keyword.block_storage_is_short}}. After the restoration process is complete, full IOPS can be realized on the new volume.

Restoring a boot volume from a "bootable" snapshot and then provisioning an instance with it results in slower performance because restored boot volume is not yet fully hydrated (that is, fully provisioned). Performance is slower than creating an instance from a regular boot volume.

## Block storage volume status
{: #block-storage-vpc-status}

The following table shows statuses that you might see when you create, view, or manage your block storage volumes, and the associated health state. For more information about volume health states, see [Block storage volume health states](#block-storage-volume-health-states).

| Status | Meaning | Health state |
|--------|---------|--------------|
| Available | A volume is available and can be attached to an instance. \n An attached data volume is available. \n A boot volume is available. | OK |
| Failed  | A volume creation failed. | Inapplicable |
| Pending | A volume is being created. | Inapplicable |
| Pending deletion | A volume is being deleted. If you're unsure the volume is deleted, verify this state. Attempting to delete a volume again while in this state results in a conflict error. | Inapplicable |
| Updating | A volume's capacity is [expanding](/docs/vpc?topic=vpc-expanding-block-storage-volumes) or volume IOPS being [adjusted](/docs/vpc?topic=vpc-adjusting-volume-iop). | OK |
| Unusable | A volume is unusable because the customer root key (CRK) was [deleted](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys) or [disabled](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys). | Inapplicable |
{: caption="Table 2. Block storage statuses" caption-side="bottom"}

## Block storage volume health states
{: #block-storage-vpc-health-states}

Volume health states correspond with [volume statuses](#block-storage-vpc-status). Table 3 describes the health states and health reasons.

| Health State | Reason |
|--------|-------------|
| OK | A volume is performing at the expected I/O performance and capacity is sufficient, no network connection issues are present, or a volume was restored from a snapshot (hydration is completed). |
| Degraded | A volume is experiencing degraded performance for any of the following reasons: \n  - Volume data is being restored (hydrated) and shows degraded until hydration is completed. \n - Initialization from a snapshot failed and the volume hydration failed. \n - Volume hydration is not started. \n - Volume hydration is paused. \n - Snapshot is in an unusable state. |
| Inapplicable | The volume is being created, volume creation failed, volume is pending, pending deletion, or unusable. No health reason is reported. |
| Faulted | The volume is unreachable, inoperative, or entirely incapacitated. |
{: caption="Table 3. Block storage health states and reasons" caption-side="bottom"}

For more information about the health states and reason codes in the API, see the [API reference](/apidocs/vpc/latest#create-volume) for creating, listing, and updating volumes.

## Managing security and compliance
{: #block-storage-vpc-manage-security}

{{site.data.keyword.block_storage_is_short}} is integrated with the Security and Compliance Center to help you manage security and compliance for your organization. You can set up goals that check whether volumes are encrypted by using customer-managed keys. By using the Security and Compliance Center to validate the block storage configurations in your account against a profile, you can identify potential issues as they arise.

For more information about monitoring security and compliance for VPC, see [Monitoring security and compliance posture with VPC](/docs/vpc?topic=vpc-manage-security-compliance#monitor-vpc). For more information about creating security and compliance goals, see [Defining rules](/docs/security-compliance?topic=security-compliance-rules-define&interface=ui) in the Security and Compliance documentation.

## Next steps
{: #next-step-managing-block-storage}

You can [create more volumes](/docs/vpc?topic=vpc-creating-block-storage).

For issues with existing block storage volumes, you might be able to troubleshoot and fix the problems yourself. For more information, see [troubleshooting {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-troubleshooting-block-storage).
