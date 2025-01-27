---

copyright:
  years: 2019, 2025
lastupdated: "2025-01-27"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing {{site.data.keyword.block_storage_is_short}} volumes
{: #managing-block-storage}

You can manage your {{site.data.keyword.block_storage_is_full}} in the console, from the CLI, or with the API. You can detach a volume from a virtual server instance or transfer a volume from one instance to another. You can attach a previously attached volume or rename a volume. You can set automatic volume deletion or manually delete a volume. You can assign access to a volume, and you can access volume read/write metrics for monitoring performance. Apply user tags that are associated with a backup policy to a volume to create automated backups.
{: shortdesc}

## Managing {{site.data.keyword.block_storage_is_short}} in the console
{: #manage-block-storage-vol-UI}
{: ui}

Use the UI to manage your {{site.data.keyword.block_storage_is_short}} volumes. In the console, you can complete the following actions:

* Detach a volume from a virtual server instance.
* Transfer a volume from one instance to another.
* Attach a previously attached {{site.data.keyword.block_storage_is_short}} data volume.
* Rename a {{site.data.keyword.block_storage_is_short}} volume.
* Add user tags to a {{site.data.keyword.block_storage_is_short}} volume.
* Adjust the IOPS of a data volume. For more information, see [Adjusting IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops).
* Increase the capacity of a volume. For more information, see [Increasing capacity of a data volume](/docs/vpc?topic=vpc-expanding-block-storage-volumes) and [Increasing capacity of a boot volume](/docs/vpc?topic=vpc-resize-boot-volumes). 
* Delete a {{site.data.keyword.block_storage_is_short}} data volume.

### Detaching a {{site.data.keyword.block_storage_is_short}} volume from a virtual server instance
{: #detach}
{: help}
{: support}

You can detach a {{site.data.keyword.block_storage_is_short}} volume that is attached to a virtual server instance. Detaching frees the volume for use by another instance.

To detach a volume, complete the following steps.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. Locate the volume and click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") to open a list of options.
1. From the options menu, click **Detach from instance**.
1. Confirm by clicking **Detach instance** in the open window.

Alternatively, you can click an individual volume in the list of all {{site.data.keyword.block_storage_is_short}}and go to the **Volume Details** page for that volume. Under **Attached instances**, click the minus sign next to the virtual server instance to detach the volume from that instance.

When you use a {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, detaching the data volume that is attached to a running instance causes the workload that's running on the instance to fail. Therefore, it is recommended that you do not detach the data volume.
{: note}

### Transferring a {{site.data.keyword.block_storage_is_short}} volume from one virtual server instance to another
{: #transfer}

To transfer a {{site.data.keyword.block_storage_is_short}} volume to another virtual server instance, complete the following steps.

1. [Detach the volume from its virtual server instance](#detach).
1. Go to the virtual server instance to which you want to transfer the volume. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**.
1. Select a virtual server instance from the list.
1. Under Attached Storage volumes, click the plus sign to add a volume. All {{site.data.keyword.block_storage_is_short}}are displayed.
1. From the list of volumes, select the volume that you previously detached.

### Attaching a previously attached {{site.data.keyword.block_storage_is_short}} data volume
{: #reattach}

A {{site.data.keyword.block_storage_is_short}} data volume is attached by default when you provision the volume during virtual server instance creation. When you detach a volume from an instance, it exists as an unattached volume and is displayed in the list of [all {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage#viewvols). You can attach it to another instance from the list of {{site.data.keyword.block_storage_is_short}} volumes.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. Locate the volume and then click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") to open a list of options.
1. From the options menu, click **Attach to instance**.
1. Select an available virtual server instance.
1. Confirm your selection.

### Updating the name of a {{site.data.keyword.block_storage_is_short}} volume
{: #rename}

You can change the name of an existing volume to make it more meaningful.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. Locate the volume and then click the name of the volume to go to the Volume Details page.
1. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") after the name of the volume to edit the name. Provide a valid volume name.
   Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. For example, if you create two volumes with the same name in the same account and region, a `volume name duplicate` error is triggered.
   {: important}

1. Confirm your edit.

### Adding user tags to a {{site.data.keyword.block_storage_is_short}} volume in the console
{: #add-user-tags-volumes-ui}

Add user tags to {{site.data.keyword.block_storage_is_short}} from the list of volumes or the volumes details page.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
2. Locate the volume from the list that you want to add user tags.
3. In the **tags** column, click **Add tags**.
4. Enter the user tags that you want to apply to this volume. Tags display as you type.

   You can also add **access management tags** to a volume. For more information about creating and adding access management tags, see [Apply access management tags to a volume](#storage-add-access-mgt-tags).
   {: note}

5. When you're done adding tags, click **Save**. When you refresh the screen, the list of {{site.data.keyword.block_storage_is_short}} shows the number of tags that are added in the **Tags** column.

You can also add tags from the volume details page. To do so, follow these steps.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes.
2. On the volume details, click **Add tags** next to the volume name.
3. Enter the user tags that you want to apply to this volume. When finished, click **Save**.

### Adding user tags that are associated with a backup policy to a volume in the console
{: #apply-tags-volumes-ui}

You can add user tags that are associated with a backup policy to a {{site.data.keyword.block_storage_is_short}} volume. Backup policies schedule automatic creation of backup snapshots. When one volume tag matches a backup policy tag for target resources, it triggers a backup of the volume contents. A backup policy defines a backup plan that schedules when backup snapshots are taken.

From the [volume details](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#view-vol-details-ui) page, you can view the backup policies that are applied to the volume and add user tags that are associated with a backup policy.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
2. Locate the volume that you want and click the name link.
3. From the {{site.data.keyword.block_storage_is_short}} details page, click the **Backup policies** tab.
4. Click **Attach**.
5. In the side panel, select a backup policy from the list of available policies, and then select the policy tags to apply to the volume. You can also view the plan details that can help you decide whether to use that policy.
6. Click **Apply policy and tags**. The backup policy shows in the list of backup policies that are associated with the volume.

When you go to the [backup policy page](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-vol-backup-policies), the volume for which you added tags shows up in the list of volumes.

For more information about creating backups, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-service-about). For more information about user tags, see [Working with tags](/docs/account?topic=account-tag).

## Managing {{site.data.keyword.block_storage_is_short}} from the CLI
{: #managing-block-storage-cli}
{: cli}

Manage your {{site.data.keyword.block_storage_is_short}} from the command-line interface (CLI). From the CLI, you can: 

* Rename a {{site.data.keyword.block_storage_is_short}} volume.
* Add user tags to a {{site.data.keyword.block_storage_is_short}} volume.
* Update the volume attachment.
* Detach a volume from a virtual server instance.
* Adjust the IOPS of a data volume. For more information, see [Adjusting IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops).
* Increase the capacity of a volume. For more information, see [Increasing capacity of a data volume](/docs/vpc?topic=vpc-expanding-block-storage-volumes) and [Increasing capacity of a boot volume](/docs/vpc?topic=vpc-resize-boot-volumes). 
* Delete a {{site.data.keyword.block_storage_is_short}} data volume.

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Updating a volume name
{: #update-vol-name-cli}

To change a volume name, specify either the volume name or ID and then indicate the new name. The volume name can be up to 63 alpha-numeric characters and include special characters, and must begin with a lowercase letter. The volume name must be unique across the VPC infrastructure.

```sh
ibmcloud is volume-update VOLUME_ID [--name NEW_NAME] [--json]
```
{: pre}

See the following example.

```sh
ibmcloud is volume-update r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34 --name my-data-volume
Updating volume r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34 under account Test Account as user test.user@ibm.com...
                                          
ID                                     r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34   
Name                                   my-data-volume   
CRN                                    crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34   
Status                                 available   
Attachment state                       unattached   
Capacity                               100   
IOPS                                   3000   
Bandwidth(Mbps)                        393   
Profile                                general-purpose   
Encryption key                         -   
Encryption                             provider_managed   
Resource group                         defaults   
Created                                2025-01-22T00:54:01+00:00   
Zone                                   us-south-2   
Health State                              
Volume Attachment Instance Reference   -   
Source snapshot                        ID                                          Name      
                                       r006-8428038a-a399-4894-8c84-c8d7a4a75fae   wdc-fst-rstore-c6a092f34118-4505      
                                          
Active                                 false   
Adjustable IOPS                        false   
Busy                                   false   
Tags                                      
```
{: screen}

### Adding user tags to a {{site.data.keyword.block_storage_is_short}} volume in from the CLI
{: #add-user-tags-volumes-cli}

Issue the `ibmcloud is volume-update VOLUME` command with the `--tags` option to update the user tags of a volume. The volume argument can be defined by either the volume ID or the volume name.

Use the same option to add tags to a volume when you create a volume by using `ibmcloud is volume-create`.
{: tip}

The following example adds user tags `env:test` and `bkp:test` to a volume identified by ID. The output shows information such as name, status, capacity, performance profile, and location. The updated tags appear at the end of the response.

```sh
ibmcloud is volume-update r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34 --tags dev:test
Updating volume r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34 under account Test Account as user test.user@ibm.com...
                                          
ID                                     r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34   
Name                                   my-data-volume   
CRN                                    crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34   
Status                                 available   
Attachment state                       unattached   
Capacity                               100   
IOPS                                   3000   
Bandwidth(Mbps)                        393   
Profile                                general-purpose   
Encryption key                         -   
Encryption                             provider_managed   
Resource group                         defaults   
Created                                2025-01-22T00:54:01+00:00   
Zone                                   us-south-2   
Health State                              
Volume Attachment Instance Reference   -   
Source snapshot                        ID                                          Name      
                                       r006-8428038a-a399-4894-8c84-c8d7a4a75fae   wdc-fst-rstore-c6a092f34118-4505      
                                          
Active                                 false   
Adjustable IOPS                        false   
Busy                                   false   
Tags                                   dev:test   
```
{: screen}

### Updating a volume attachment from the CLI
{: #update-vol-attachment-cli}

You can update the volume attachment name and change the default auto delete setting with the `instance-volume-attachment-update` command.

```sh
ibmcloud is instance-volume-attachment-update INSTANCE_ID VOLUME_ATTACHMENT_ID [--name NEW_NAME] [--auto-delete true | false] [--json]
```
{: pre}

Use the `--name` option and specify a new name for the volume attachment. Specify `--auto-delete true` to automatically delete a volume that is attached to an instance, when you delete the instance. Specify `--auto-delete false`, if you want to keep the volume as a stand-alone volume after the instance is deleted.

```sh
$ ibmcloud is instance-volume-attachment-update kj-test-ro otp1 --name one-true-pairing --auto-delete false
Updating volume attachment otp1 of instance kj-test-ro under account Test Account as user test.user@ibm.com...
ID                0757-6757e676-0bf5-4b79-9a5b-29c24e17420c
Name              one-true-pairing
Volume            ID                                          Name
                  r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac   demo-volume-update
 
Status            attached
Bandwidth(Mbps)   393
Type              data
Device            0757-6757e676-0bf5-4b79-9a5b-29c24e17420c-bxsh7
Auto delete       false
Created           2023-06-29T18:14:57+00:00
```
{: screen}

For more information about available command options, see [`ibmcloud is instance-volume-attachment-update`](/docs/cli?topic=cli-vpc-reference#instance-volume-attachment-update).

### Detaching a volume from the CLI
{: #detach-vol-attachment-cli}
{: help}
{: support}

Use the `instance-volume-attachment-detach` command to detach a volume from an instance and delete the volume attachment. The {{site.data.keyword.block_storage_is_short}} volume is not deleted; you can later [attach it to another instance](/docs/vpc?topic=vpc-attaching-block-storage&interface=cli#attaching-block-storage-cli).

In the syntax for this command, INSTANCE is the ID or name of the instance. VOLUME_ATTACHMENT is the ID or name of the volume attachment. You can specify multiple volume attachments. For more information about volume attachments, see the CLI reference for [creating a volume attachment](/docs/vpc?topic=vpc-vpc-reference&interface=cli#instance-volume-attachment-add).

```sh
ibmcloud is instance-volume-attachment-detach INSTANCE (VOLUME_ATTACHMENT1 VOLUME_ATTACHMENT2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

```sh
$ ibmcloud is instance-volume-attachment-detach kj-test-ro one-true-pairing
This will delete volume attachment one-true-pairing and cannot be undone. Continue [y/N] ?> y
Deleting volume attachment one-true-pairing from instance kj-test-ro under account Test Account as user test.user@ibm.com...
OK
Volume attachment one-true-pairing is deleted.

```
{: screen}

For more information about available command options, see [`ibmcloud is instance-volume-attachment-detach`](/docs/cli?topic=cli-vpc-reference#instance-volume-attachment-detach).

## Managing {{site.data.keyword.block_storage_is_short}} with the API
{: #managing-block-storage-api}
{: api}

Manage your {{site.data.keyword.block_storage_is_short}} programmatically by making requests to the [VPC REST APIs](/apidocs/vpc). With the API, you can:

* Rename a {{site.data.keyword.block_storage_is_short}} volume.
* Add user tags to a {{site.data.keyword.block_storage_is_short}} volume.
* Update the volume attachment.
* Detach a volume from a virtual server instance.
* Adjust the IOPS of a data volume. For more information, see [Adjusting IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops).
* Increase the capacity of a volume. For more information, see [Increasing capacity of a data volume](/docs/vpc?topic=vpc-expanding-block-storage-volumes) and [Increasing capacity of a boot volume](/docs/vpc?topic=vpc-resize-boot-volumes). 
* Delete a {{site.data.keyword.block_storage_is_short}} data volume.

### Updating the name of a volume with the API
{: #update-vol-name-api}

Make a `PATCH /volumes/{id}` call and specify a new name for the volume.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/volumes?version=2022-04-22&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-volume-4-update"
    }'
```
{: pre}

A successful response looks like the following example.

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
{: screen}

### Adding user tags to a {{site.data.keyword.block_storage_is_short}} volume with the API
{: #add-user-tags-volumes-api}

To add user tags to a volume, you first make a `GET /volumes/{volume_id}` call and copy the hash string from `Etag` property in the response header. You then use the hash string when you specify `If-Match` in a `PATCH /volumes/{volume_id}` request to create new user tags.

To apply tags to a {{site.data.keyword.block_storage_is_short}} volume, follow these steps:

1. Make a `GET /volumes/{volume_id}` call and copy the hash string from the `Etag` property in the response header. You need to use the hash string when you specify `If-Match` in the `PATCH /volumes/{volume_id}` request to create user tags for the volume in step 2. To generate the response header information, make an API call similar to the following example:

   ```sh
   curl -sSL -D GET\
   "https://us-south.iaas.cloud.ibm.com/v1/volumes/{volume_id}?version=2022-04-25&generation=2"\
   -H "Authorization: Bearer $TOKEN" -o /dev/null
   ```
   {: pre}

   In the response header, you see something like this:

   ```json
   HTTP/2 200
   date: Tue, 28 Apr 2022 17:48:03 GMT
   content-type: application/json; charset=utf-8
   content-length: 1049
   cf-ray: 69903d250c4966ef-DFW
   cache-control: max-age=0, no-cache, no-store, must-revalidate
   expires: -1
   strict-transport-security: max-age=31536000; includeSubDomains
   cf-cache-status: DYNAMIC
   expect-ct: max-age=604800, report-uri="[uri...]"
   pragma: no-cache
   x-content-type-options: nosniff
   x-request-id: 1fbe2384-6828-4503-ae7d-050426d1b11b
   x-xss-protection: 1; mode=block
   server: cloudflare
   etag: W/xxxyyyzzz123
   ```
   {: codeblock}

2. Make a `PATCH /volumes/{volume_id}` request. Specify the _Etag-hash-string_ for the `If-Match` property in the header. Specify the user tag in the `user_tags` property.

   You can also add tags when you make a `POST /volumes` call to create a volume and specify the `user_tags` property.
   {: tip}

   This example updates the volume by specifying user tags `env:test` and `env:prod`. The value that you obtained from the `Etag` parameter is specified in the `If-Match` header in the call.

   ```sh
   curl -X PATCH\
   "$vpc_api_endpoint/v1/volumes/r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34?version=2025-01-21&generation=2"\
      -H "Authorization: Bearer"\
      -H "If-Match: <_Etag-hash-string_>"\
      -d `{
         "user_tags": [
            "env:test",
            "env:prod"
         ]
      }'
   ```
   {: codeblock}

   The response shows the tags that were added to the volume:

   ```json
   {
    "active": false,
    "adjustable_iops_supported": false,
    "attachment_state": "unattached",
    "bandwidth": 393,
    "busy": false,
    "capacity": 100,
    "created_at": "2025-01-22T00:54:01.000Z",
    "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34",
    "encryption": "provider_managed",
    "health_reasons": [],
    "health_state": "",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34",
    "id": "r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34",
    "iops": 3000,
    "name": "my-data-volume",
    "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
        "name": "general-purpose"
    },
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6edefe513d934fdd872e78ee6a8e73ef",
        "id": "6edefe513d934fdd872e78ee6a8e73ef",
        "name": "defaults"
    },
    "source_snapshot": {
        "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-8428038a-a399-4894-8c84-c8d7a4a75fae",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-8428038a-a399-4894-8c84-c8d7a4a75fae",
        "id": "r006-8428038a-a399-4894-8c84-c8d7a4a75fae",
        "name": "wdc-fst-rstore-c6a092f34118-4505",
        "resource_type": "snapshot"
    },
    "status": "available",
    "status_reasons": [],
    "adjustable_capacity_states": [
        "attached"
    ],
    "user_tags": [
        "env:test",
        "env:prod"
    ],
    "volume_attachments": [],
    "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
        "name": "us-south-2"
    }
   }
   ```
   {: codeblock}

### Updating a volume attachment with the API
{: #update-vol-attachment-api}

Make a `PATCH /instances` call and specify the ID of the new volume attachment.

```sh
PATCH /instances/{instance_id}/volume_attachments/{id}
```
{: pre}

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id/volume_attachments/$volume_attachment_id?version=2022-04-22&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "delete_volume_on_instance_delete": false,
      "name": "my-volume-attachment-data-5iops-updated"
    }'
```
{: pre}

A successful response looks like the following example.

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
{: screen}

### Detaching a volume with the API
{: #detach-vol-attachment-api}

Make a `DELETE /instances` request and specify the volume attachment ID to delete a volume attachment. Deleting a volume attachment detaches a volume from an instance.

```sh
DELETE /instances/{instance_id}/volume_attachments/{id}
```
{: pre}

```sh
curl -X DELETE "$vpc_api_endpoint/v1/instances/$instance_id/volume_attachments/$volume_attachment_id?version=2022-04-22&generation=2" \
-H "Authorization: $iam_token"

```
{: pre}

Verify that the volume is detached from the instance by making a `GET /instances/{instance_id}` call.

## Managing {{site.data.keyword.block_storage_is_short}} with Terraform
{: #managing-block-storage-terraform}
{: terraform}

Manage your {{site.data.keyword.block_storage_is_short}} as a code with Terraform. With the Terraform, you can:

* Rename a {{site.data.keyword.block_storage_is_short}} volume.
* Add user tags to a {{site.data.keyword.block_storage_is_short}} volume.
* Adjust the IOPS of a data volume. For more information, see [Adjusting IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops).
* Increase the capacity of a volume. For more information, see [Increasing capacity of a data volume](/docs/vpc?topic=vpc-expanding-block-storage-volumes) and [Increasing capacity of a boot volume](/docs/vpc?topic=vpc-resize-boot-volumes). 
* Delete a {{site.data.keyword.block_storage_is_short}} data volume.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: screen}

To create and manage volumes, the `ibm_is_volume` resource is used. You can change various attributes of a volume, for example its name, tags, capacity, IOPS. However, some changes force the creation of a new resource. Such changes are the ones that affect zone, resource group, resource controller, and encryption key attributes. For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

### Updating the name of a volume with Terraform
{: #update-vol-name-terraform}

To change the name of a volume, use the `ibm_is_volume` resource. The following example specifies the volume `r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4` and its new name as `my-new-name-volume`. When applied, the volume is renamed.

```terraform
resource "ibm_is_volume" "example" {
  name    = "my-new-name-volume"
  id      = "r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4"
  profile = "10iops-tier"
  zone    = "us-south-1"
}
```
{: codeblock}

### Applying tags to volumes with Terraform
{: #block-storage-add-tags-terraform}

To apply user tags to a volume, use the `ibm_is_volume` resource. The following example specifies the volume `my-new-volume` and the tag `dev:test` to be attached to the volume. When applied, the tag is added to the volume.

```terraform
resource "ibm_is_volume" "example" {
  name    = "my-new-volume"
  profile = "10iops-tier"
  zone    = "us-south-1"
  tags    = ["dev:test"]
}
```
{: codeblock}

## Applying access management tags to a {{site.data.keyword.block_storage_is_short}} volume
{: #storage-add-access-mgt-tags}

Access management tags are metadata that you can add to your {{site.data.keyword.block_storage_is_short}}to help organize access control resource relationships. You first create the tag and then apply it to a new volume or to an existing volume. You can apply the same access management tag to multiple {{site.data.keyword.block_storage_is_short}} volumes. You then assign access to the tag in IAM. Optionally, you can create an IAM access group and manage users.

Access management tags are not used by [backup policies](/docs/vpc?topic=vpc-backup-use-policies) to create backup snapshots. Backup snapshots are created when user tags match backup policy tags for target resources to volume user tags.
{: note}

Each resource can have up to 1000 user tags, and no more than 250 access tags. However, only 100 tags can be attached or detached in the same operation.

### Step 1 - Creating an IAM access management tag in the console
{: #storage-create-access-mgt-tag-ui}
{: ui}

In the console, complete the following steps.

1. Go to **Manage > Account**, and then select **Tags**.
2. Click the **Access management tags** tab. Add a tag name in the field. Access management tags require a `key:value` format.
3. Click **Create Tags**.

### Step 1 - Creating an IAM access management tag from the cli
{: #storage-create-access-mgt-tag-cli}
{: cli}

From the command line, enter the `ibmcloud resource tag-create` command to create an access management tag in your account. The following example creates a tag that is called `project:myproject`:

    ```sh
    ibmcloud resource tag-create --tag-names project:myproject
    ```
    {: codeblock}

For more information, see the [`ibmcloud resource` command reference](/docs/cli?topic=cli-ibmcloud_commands_resource).

### Step 1 - Creating an IAM access management tag with the API
{: #storage-create-access-mgt-tag-api}
{: api}

With the [Global Search and Tagging API](/docs/account?topic=account-tag&interface=api#create-access-api), make a `POST/ tags` call to [create an access management tag](/apidocs/tagging#create-tag). Specify the tag in the `tag_names` property. For an example, see [Creating access management tags by using the API](/docs/account?topic=account-tag&interface=api#create-access-api).

### Step 1 - Creating an IAM access management tag with Terraform
{: #storage-create-access-mgt-tag-terraform}
{: terraform}

Create an argument in your `main.tf` file. The following example creates the access management tag `ibm_tag` that is added to the `ibm` resource for the resource ID `ibm_is_volume.example.crn`.

   ```terraform
   resource "ibm_resource" "ibm" {
   resource_id = ibm_is_volume.example.crn
   tags        = [ "ibm_tag" ]
   }
   ```
    {: codeblock}

### Step 2 - Adding an access management tag to a volume
{: #storage-add-access-mgt-tag}

Add an access management tag to an existing volume or when you [create a volume](/docs/vpc?topic=vpc-creating-block-storage). To add access management tags to an existing volume, complete the following steps.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
2. Locate the volume from the list.
3. In the **tags** column, click **Add tags**.
4. Enter the access management tags in the access management tag field. Tags that you created display as you type.
5. Click **Save**.

### Step 3 - Assigning access and users
{: #storage-access-mgt-additional-steps}

After you create an access management tag and apply it to a volume, complete the following steps to assign access and add users.

1. [Create an access group](/docs/account?topic=account-access-tags-tutorial#tagging-create-access-group). Access groups are assigned policies that grant roles and permissions to the members of that group. You assign access to the specific access management tags for the {{site.data.keyword.block_storage_is_short}} service. For more information about access groups, see [Setting up access groups](/docs/account?topic=account-groups&interface=ui).

2. [Assign an access policy to a group](/docs/account?topic=account-access-tags-tutorial#tagging-assign-policy).

3. [Add users to the access group](/docs/account?topic=account-access-tags-tutorial#tagging-add-users-access-group).

When you look at the specific resources for the VPC infrastructure and specify {{site.data.keyword.block_storage_is_short}} as the resource type, you can see the access management tags for the {{site.data.keyword.block_storage_is_short}} service.

## Deleting a {{site.data.keyword.block_storage_is_short}} volume and data eradication
{: #block-storage-data-eradication}

When you delete a {{site.data.keyword.block_storage_is_short}} volume, that data immediately becomes inaccessible. All pointers to the data on the physical disk are removed. If you later create a volume in the same or another account, a new set of pointers is assigned. The account can't access any data that was on the physical storage because those pointers are deleted. When new data is written to the disk, any inaccessible data from the deleted volume is overwritten.

IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten and eradicated. Further, when you delete a {{site.data.keyword.block_storage_is_short}} volume, those blocks must be overwritten before that {{site.data.keyword.block_storage_is_short}} is made available again, either to you or to another customer.

Further, when IBM decommissions a physical drive, the drive is destroyed before disposal. Decommissioned drives are unusable and any data on them is inaccessible.

### Sanitizing your data before you delete a volume
{: #block-storage-sanitization}

When you delete a {{site.data.keyword.block_storage_is_short}} volume, IBM guarantees that your data is inaccessible on the physical disk and is eventually [eradicated](#block-storage-data-eradication). If you have extra compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For more information, see the [NIST 800-88 Guidelines for Media Sanitation](https://csrc.nist.gov/pubs/sp/800/88/r1/final){: external}.

Sanitizing and deleting the volume means your data can't be restored.

### Deleting a {{site.data.keyword.block_storage_is_short}} data volume in the console
{: #delete}
{: ui}
{: help}
{: support}

Deleting a {{site.data.keyword.block_storage_is_short}} volume completely removes its data. The volume cannot be restored.

You cannot delete an active {{site.data.keyword.block_storage_is_short}} volume. To delete a volume, first [detach it](#detach) from the virtual server instance. If you took snapshots of the volume, all snapshots must be in a `stable` state.

To delete a volume, complete the following steps.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. Locate the volume that you want to delete and then click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") to open a list of options.
1. From the options menu, click **Delete**.
1. Confirm the deletion.

### Automatically delete {{site.data.keyword.block_storage_is_short}} data volumes
{: #auto-delete}
{: ui}

By using the Auto Delete feature, you can specify that a {{site.data.keyword.block_storage_is_short}} data volume is automatically deleted when you delete an instance to which it is attached.

You don't need to set automatic deletion for boot volumes. Boot volumes are created during instance creation and automatic deletion is enabled by default. When you delete the instance, the boot volume is also deleted.
{: note}

To enable Auto Delete for an existing {{site.data.keyword.block_storage_is_short}} data volume that is attached to an instance, follow these steps:

1. Locate the virtual server instance to which the data volume is attached. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**.
1. Under **Attached {{site.data.keyword.block_storage_is_short}} volumes**, select a volume.
1. On the next page, click **Auto Delete** to enable.
1. Confirm your selection.

Alternatively, select a data volume from the list of {{site.data.keyword.block_storage_is_short}}(**Storage > Block Storage volumes**). On the volume details page, under **Attached instances**, click the **Auto delete** toggle to enable or disable automatic deletion.

You can also enable Auto Delete on a new data volume when you create an instance. For more information, see [Create and attach a {{site.data.keyword.block_storage_is_short}} volume when you create an instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).

### Deleting a {{site.data.keyword.block_storage_is_short}} volume from the CLI
{: #delete-vol-cli}
{: help}
{: support}

Use the `volume-delete` command and specify the volume ID to delete a {{site.data.keyword.block_storage_is_short}} volume.

You cannot delete an active {{site.data.keyword.block_storage_is_short}} volume. You must first [detach it from the virtual server](#detach-vol-attachment-cli).
{: note}

```sh
ibmcloud is volume-delete (VOLUME_NAME | VOLUME_ID) [-f, --force]
```
{: pre}

See the following example.

```sh
$ ibmcloud is volume-delete demovolume1
This will delete volume demovolume1 and cannot be undone. Continue [y/N] ?> y
Deleting volume demovolume1 under account Test Account as user test.user@ibm.com...
OK
Volume demovolume1 is deleted.
```
{: screen}

### Deleting a {{site.data.keyword.block_storage_is_short}} volume with the API
{: #delete-vol-api}
{: api}

Make a `DELETE /volumes/{id}` call.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-04-22&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

To verify that the volume is deleted, list the volumes by making a `GET /volumes` call.

### Deleting a {{site.data.keyword.block_storage_is_short}} volume with Terraform
{: #delete-vol-terraform}
{: help}
{: support}

Use the `terraform destroy` command to conveniently destroy a remote object such as a block volume. The following example shows the syntax for deleting a volume. Substitute the actual ID of the volume in for `ibm_is_volume.example.id`.

```terraform
terraform destroy --target ibm_is_volume.example.id
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

## Next steps
{: #next-step-managing-block-storage}

You can [create more volumes](/docs/vpc?topic=vpc-creating-block-storage), or [monitor your volumes' health states, volume status, and metrics](/docs/vpc?topic=vpc-block-storage-vpc-monitoring).

For issues with existing {{site.data.keyword.block_storage_is_short}} volumes, you might be able to troubleshoot and fix the problems yourself. For more information, see [troubleshooting {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-troubleshooting-block-storage).
