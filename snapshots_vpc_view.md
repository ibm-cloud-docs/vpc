---

copyright:
  years: 2021
lastupdated: "2021-12-20"

keywords: snapshots, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:tip: .tip}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}


# Viewing snapshots
{: #snapshots-vpc-view}

You can view a list of all snapshots and drill down to see information about a particular snapshot. Choose the UI, CLI, or API to retrieve this information.
{: shortdesc}

## List snapshots in the UI
{: #snapshots-vpc-view-list-ui}
{: ui}

### List all snapshots in the UI
{: #snapshots-vpc-view-list-ui}

View a list of all snapshots that you created, with the most recent one at the beginning of the list. You can filter the list to view specific snapshots. Follow these steps:

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**. By default, the newest snapshots display at the beginning of the list.

You can also view a list of snapshots for a volume. For more information, see [View all snapshots created from the block storage volume](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#view-snapshots-for-volume).
{: tip}

1. View snapshots in your account region. If you created snapshots in a different region, expand the list and select the region.

1. As your list of snapshots grows, use the filter to indicate the number of snapshots to display per page. Use the page navigation arrows to move forward and back through the list.

Table 1 describes the information for all snapshots in the list of snapshots. 

| Field | Value |
|-------|-------|
| Name  | The name you provided when you created the snapshot. Click the name of the snapshot to see its [details](#snapshots-vpc-view-snapshot-ui). |
| Status | Status of the snapshot, depending on whether it's usable (_active_ status), unusable, being created, and so on. For more information, see [Snapshot statuses](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-status). |
| Size | Size of the snapshot in GBs, inherited from the source volume. |
| Encryption | Encryption with [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-provider-managed-encryption) or [provider-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-customer-managed-encryption), inherited from the source volume. |
| Source volume | The boot or data volume from which the snapshot was created. Click the name of the volume to see its [details](/docs/vpc?topic=vpc-viewing-block-storage). |
| Bootable | Whether the snapshot is of a boot volume. |
| Created | The date and time that snapshot was first taken. |
| Tags | Number of tags applied to this snapshot from the source volume. If there are no tags applied to the snapshot that were inherited from the source volume, **Add tags** allows you to apply tags to the snapshot. | 
| Actions (ellipsis) | Click the overflow icon to display a menu of context-specific actions you can take: | 
| | Copy the snapshot ID |
| | Copy the Cloud Resource Name (CRN) |
| | [Delete the snapshot](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete) |
| | [Delete all snapshots for a volume](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-all-ui) | 
{: caption="Table 1. List of all snapshots" caption-side="top"}

You can also list all snapshots that were created from a block storage volume from the volume details page. For more information, see [List all snapshots for a volume](/docs/vpc?topic=vpc-view-snapshots-for-volume).

### View snapshot details in the UI
{: #snapshots-vpc-view-snapshot-ui}

To see details about a snapshot:

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**.

1. Click the name of a snapshot. The snapshot details side panel shows the information that is described in Table 2.

| Field | Value |
|-------|-------|
| Status | Current status of the snapshot, such as _Stable_. For a list of snapshot statuses, see [Snapshot statuses](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status). |
| Tags | User tags inherited from the source volume when the snapshot is created, or that you manually apply to a snapshot. |
| Name  | The name of the snapshot, which you can change by clicking the pencil icon. For more information, see [Change the snapshot name](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-rename). |
| Resource group | |
| ID | Copiable GUID of the snapshot. |
| CRN | Copiable CRN of the snapshot. |
| Created | Date and time that the snapshot resource creation process started. |
| Captured | Date and time when the snapshot creation completed. |
| Region | Region of your account, such as us-south |
| Size| Size in GBs of the snapshot, inherited from the source volume. |
| Source volume | Source volume from which the first snapshot was taken. Click the link for volume details. If the volume was deleted, the name appears without a link. |
| KMS instance | Link to the Key Protect or HPCS instance that is used for customer-managed encryption. |
| Key name | Name of the root key that is protecting the volume. |
| Key | Copiable GUID of the root key. |
{: caption="Table 2. Snapshot details" caption-side="top"}

## View snapshots from the CLI
{: #snapshots-vpc-view-cli}
{: cli}

You can list all snapshots, all snapshots for a volume, and details about a particular snapshot.

### View all snapshots
{: #snapshots-vpc-view-all-cli}

Run the `snapshots` command to list all snapshots.

```zsh
ibmcloud is snapshots [--json]
```
{: pre}

Example:

```zsh
# ibmcloud is snapshots
Listing snapshots for generation 2 compute in all resource groups and region us-south under account VPC 01 as user rtuser1@mycompany.com...

ID                                          Name         Status    Progress   Source Volume                               Bootable   Resource group   Created   
6a093fdd-9e33-4e96-9ca8-cf5b45d097d8   snapshot1    stable    -          
0f136bf8-6530-47e4-9482-ff0c06a7edc4   false      Default          2021-11-16T16:18:56+08:00   
50308933-05b4-4363-9c45-00584fc52a43   snapshot2   pending   0          0f136bf8-6530-47e4-9482-ff0c06a7edc4   false      Default          2021-11-16T16:26:04+08:00 
```
{: screen}


### View all snapshots for a volume from the CLI
{: #snapshots-vpc-view-cli}

Run the `snapshots` command and specify the volume ID.

```zsh
ibmcloud is snapshots --volume VOLUME-ID [--json]
```
{: pre}

Example:

```zsh
$ ibmcloud is snapshots --volume 728b2d3c-2165-46c7-9863-9397e0a9af42
Listing snapshots for generation 2 compute in all resource groups and region us-south under account VPC 01 as user rtuser1@mycompany.com...

ID                                          Name   Status   Progress   Source Volume                               Bootable   Resource group   Created   
b2168769-a4dc-4cb8-9fc6-e62d45918858   t2b1   stable   -          728b2d3c-2165-46c7-9863-9397e0a9af42   false      Default          2021-12-20T16:28:58+08:00   
6e7ac183-3223-43d1-8f15-bea30c94eda0   t2b2   stable   -          728b2d3c-2165-46c7-9863-9397e0a9af42   false      Default          2021-12-20T16:29:01+08:00   

```
{: screen}

### View details of a snapshot from the CLI
{: #snapshots-vpc-view-details-cli}

Run the `snapshots` command and specify the snapshots ID.

```zsh
ibmcloud is snapshots SNAPSHOT_ID [--json]
```
{: pre}

Example:

```zsh
ibmcloud is snapshot c2bc3194-0cab-40c4-9434-db9f26218700
Getting snapshot c2bc3194-0cab-40c4-9434-db9f26218700 under account vpc1 as user user@mycompany.com...
                          
ID                     c2bc3194-0cab-40c4-9434-db9f26218700   
Name                   my-snapshot   
CRN                    crn:v1:bluemix:public:is:us-south:a/7f75c7b025e54bc5635f754b2f888665::snapshot:c2bc3194-0cab-40c4-9434-db9f26218700   
Status                 stable   
Source volume          ID                                          Name      
                       fe027a90-19e7-4cb5-bda3-4c0e35d2bcdf        test5-vol      
                          
Progress               -   
Bootable               true   
Encryption             provider_managed   
Encryption key         -   
Minimum capacity(GB)   100   
Size(GB)               1   
Source image           ID                                          Name      
                       fb4d7950-8ff4-4d9a-9d9f-3056cb8c36d9        centos-8-2-minimal-amd64-2      
                          
Operating system       Name             Vendor   Version                 Family   Architecture   Display name      
                       centos-8-amd64   CentOS   8.x - Minimal Install   CentOS   amd64          CentOS 8.x - Minimal Install (amd64)      
                          
Resource group         ID                                 Name      
                       ef2694fe-d6d1-4136-94c3-0ae315204e6b   Default      
                          
Created                2021-21-20T01:53:15+05:30  
```
{: codeblock}

## List snapshots with the API
{: #snapshots-vpc-view-all-api}
{: api}

### List all snapshots with the API
{: #snapshots-vpc-view-all-api}

Using the [VPC API](https://{DomainName}/apidocs/vpc), make a `GET/snapshots` request to list all snapshots of your volumes. By default, the list shows the most recent snapshots first, followed by older snapshots in descending order. 

   ```curl
   curl -X GET \
   "$vpc_api_endpoint/v1/snapshots?version=2021-12-16&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

You can filter the list by using the resource group ID, source volume ID, or source volume CRN, and further filter the results by using these options:

* Limit - To control the number of snapshots displayed on a page.
* Start - To specify which snapshot (by ID) to start the list.
* Sort - To control the order in which you sort the page, such as by date created or alphabetically by snapshot name.

For more information, see the [VPC API reference](https://{DomainName}/apidocs/vpc).

For example, this call filters the list to show snapshots that were created for a single volume and limits the results to five per page.

```curl
curl -X GET \
"$vpc_api_endpoint/v1/snapshots?version=2021-12-21&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "limit": 5,
      "source_volume": 
        "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"
      }
    }'
   ```
   {: pre}

A successful response looks like the following example:

```json
{
  "snapshots": [
    {
      "id": "1eeae628-b24c-41fb-91b9-bc0e02167f1e",
      "crn": "crn: [...]",
      "href": "https://resource-controller.test.cloud.ibm.com/v2/
      resource_groups/bd081e1c-3c0d-4242-84fb-9d02fc963402",
      "resource_group": {
        "id": "bd081e1c-3c0d-4242-84fb-9d02fc963402",
        "href": "https://resource-controller.test.cloud.ibm.com/v2/
        resource_groups/bd081e1c-3c0d-4242-84fb-9d02fc963402",
        "name": "Default"
      },
      "encryption": "provider_managed",
      "source_volume": {
        "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
        "crn": "crn: [...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/
        volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
        "name": "my-data-volume-1"
      },
      "created_at": "2021-12-21T07:02:01Z",
      "captured_at": "2021-12-21T07:02:17Z",
      "lifecycle_state": "stable",
      "minimum_capacity": 100,
      "operating_system": {
        "architecture": "amd64",
        "display_name": "CentOS 7.x - Minimal Install (amd64)",
        "family": "CentOS",
        "gpu_supported": null,
        "href": "https://us-south-iaas.cloud.ibm.com/v1/operating_systems/centos-7-amd64",
        "name": "centos-7-amd64",
        "vendor": "CentOS",
        "version": "7.x - Minimal Install"
      },
      "source_image": {
        "id": "7bdec860-e092-4f07-8481-b0b041a9224a",
        "crn": "crn: [...]",
        "href": "/v1/images/7bdec860-e092-4f07-8481-b0b041a9224a",
        "name": "ibm-centos-7-6-minimal-amd64-2"
      },
      "size": 3,
      "resource_type": "snapshot",
      "bootable": true
    },
    {
      "id": "dd43ba12-8a76-4f09-8839-9b258162e535",
      "crn": "crn: [...]",
      "resource_group": {
        "id": "bd081e1c-3c0d-4242-84fb-9d02fc963402",
        "href": "https://resource-controller.cloud.ibm.com/v2/
        resource_groups/bd081e1c-3c0d-4242-84fb-9d02fc963402",
        "name": "Default"
      },
      "encryption": "provider_managed",
      "source_volume": {
        "id": "6c1f512c-da2f-41f7-ac3b-294e5be0d1dd",
        "crn": "crn: [...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/6c1f512c-da2f-41f7-ac3b-294e5be0d1dd",
        "name": "vol-6c1f512c-da2f-41f7-ac3b-294e5be0d1dd"
      },
      "created_at": "2021-12-19T17:50:43Z",
      "captured_at": "2021-12-19T17:50:58Z",
      "lifecycle_state": "stable",
      "minimum_capacity": 1999,
      "size": 237,
      "resource_type": "snapshot",
      "bootable": false
    }
  ],
  "first": {
    "href": "https://us-south.iaasdev.cloud.ibm.com/v1/snapshots?limit=5"
  },
  "next": {
    "href": "https://us-south.iaasdev.cloud.ibm.com/v1/snapshots?limit=5&start=569704e5-f6a8-4758-b9d6-2264074a4b31"
  },
  "limit": 5,
  "total_count": 63
}

```
{: codeblock}

### List details of a snapshot with the API
{: #snapshots-vpc-view-api}

For details about a single snapshot, use the VPC API to make a `GET/snapshots` call and specify the snapshot ID.

   ```curl
   curl -X GET \
   "$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2021-12-21&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

A successful response looks like the following example:

```json
{
  "id": "7528eb61-bc01-4763-a67a-a414a103f96d",
  "crn": "crn: [...]",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/
  snapshots/7528eb61-bc01-4763-a67a-a414a103f96d",
  "name": "my-snapshot-4",
  "resource_group": {
    "id": "59ff2d74-b0e5-4b40-a553-b812e50c72e9",
    "href": "https://resource-controller.cloud.ibm.com/v2/
    resource_groups/59ff2d74-b0e5-4b40-a553-b812e50c72e9",
    "name": "Default"
  },
  "encryption": "provider_managed",
  "source_volume": {
    "id": "7528eb61-bc01-4763-a67a-a414a103f96d",
    "crn": "crn: [...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/7528eb61-bc01-4763-a67a-a414a103f96d",
    "name": "my-data-volume-1"
  },
  "created_at": "2021-12-13T07:02:01Z",
  "captured_at": "2021-12-13T07:02:17Z",
  "lifecycle_state": "stable",
  "minimum_capacity": 100,
  "operating_system": {
    "architecture": "amd64",
    "display_name": "CentOS 7.x - Minimal Install (amd64)",
    "family": "CentOS",
    "gpu_supported": null,
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/centos-7-amd64",
    "name": "centos-7-amd64",
    "vendor": "CentOS",
    "version": "7.x - Minimal Install"
  },
  "source_image": {
    "id": "dc021ec6-759d-4b87-8e0a-04106b8aa635",
    "crn": "crn:v1:staging:public:is:us-south:::image:dc021ec6-759d-4b87-8e0a-04106b8aa635",
    "href": "/v1/images/dc021ec6-759d-4b87-8e0a-04106b8aa635",
    "name": "centos-7-6-minimal-amd64-2"
  },
  "size": 3,
  "resource_type": "snapshot",
  "bootable": true
}

```
{: codeblock}

## Next steps
{: #snapshots_vpc_view_next_steps}

* [Modify or delete snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage).
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
