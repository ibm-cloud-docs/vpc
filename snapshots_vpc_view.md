---

copyright:
  years: 2021
lastupdated: "2021-05-26"

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

---

# Viewing snapshots
{: #snapshots-vpc-view}

You can view a list of all snapshots and drill down to see information about a particular snapshot. Choose the UI, CLI, or API to retrieve this information.
{: shortdesc}

## List snapshots by using the UI
{: #snapshots-vpc-view-list-ui}
{: ui}

### List all snapshots by using the UI
{: #snapshots-vpc-view-list-ui}

View a list of all snapshots you created, with the most recent one at the beginning of the list. You can filter the list to view specific snapshots. Follow these steps:

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**. By default, the newest snapshots display at the beginning of the list.

1. View snapshots in your account region. If you created snapshots in a different region, expand the list and select the region.

1. As your list of snapshots grows, use the filter to indicate the number of snapshots to display per page. Use the page navigation arrows to move forward and back through the list.

Table 1 describes the information for all snapshots in the list of snapshots. 

| Field | Value |
|-------|-------|
| Status | Status of the snapshot, depending on whether it's usable (_active_ status), unusable, being created, and so on. For more information, see [Snapshot statuses](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-status). |
| Name  | The name you provided when you created the snapshot. Click the name of the snapshot to see its [details](#snapshots-vpc-view-snapshot-ui). |
| Size | Size of the snapshot in GBs, inherited from the source volume. |
| Encryption | Encryption inherited from the source volume, either IBM-managed or customer-managed encryption. |
| Source volume | The boot or data volume from which the snapshot was created. Click the name of the volume to see its [details](/docs/vpc?topic=vpc-viewing-block-storage). |
| Created | The date you created the snapshot. The list order is newest to oldest snapshots. |
| Actions (hellipsis) | Click the overflow icon to display a menu of context-specific actions you can take: | 
| | Copy the UUID - Useful when you want to identify snapshots in SDKs, the CLI, or Terraform  |
| | Copy the Cloud Resource Name (CRN) |
| | Copy the snapshot ID |
| | [Delete the snapshot](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete) |
| | [Delete all snapshots for a volume](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-all-ui) | 
{: caption="Table 1. List of all snapshots" caption-side="top"}

You can also list all snapshots that were created from a block storage volume from the volume details page. For more information, see [List all snapshots for a volume](/docs/vpc?topic=vpc-view-snapshots-for-volume).

### View snapshot details by using the UI
{: #snapshots-vpc-view-snapshot-ui}

To see details about a snapshot:

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
1. Click the name of a snapshot. The snapshot details page displays with the information that is described in Table 2.

| Field | Value |
|-------|-------|
| Name  | The name of the snapshot, which you can change by clicking the pencil icon. For more information, see [Change the snapshot name](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-rename). |
| ID | Copiable GUID of the snapshot. |
| CRN | Copiable CRN of the snapshot. |
| Created | Creation date of the snapshot. |
| Region | Region of your account, such as us-south |
| Size| Size in GBs of the snapshot, inherited from the source volume. |
| Source volume | Source volume from which the first snapshot was taken. Click the link for volume details. If the volume was deleted, the name appears without a link. |
| Encryption instance | Link to the Key Protect or HPCS instance that is used for customer-managed encryption. |
| Key name | Name of the root key that is protecting the volume. |
| Key | Copiable GUID of the root key. |
| Tags | Tags to organize this resource in your resource list. For more information about tags, see [Working with tags](/docs/account?topic=account-tag).|
{: caption="Table 2. Snapshot details" caption-side="top"}

## View snapshots by using the CLI
{: #snapshots-vpc-view-cli}
{: cli}

You can list all snapshots, all snapshots for a volume, and details about a particular snapshot.

### Viewing all snapshots
{: #snapshots-vpc-view-all-cli}

Run the `snapshots` command to list all snapshots.

```
ibmcloud is snapshots [--json]
```
{: pre}

Example:

```
# ibmcloud is snapshots
Listing snapshots for generation 2 compute in all resource groups and region us-south under account VPC 01 as user rtuser1@mycompany.com...

ID                                          Name         Status    Progress   Source Volume                               Bootable   Resource group   Created   
6a093fdd-9e33-4e96-9ca8-cf5b45d097d8   snapshot1    stable    -          
0f136bf8-6530-47e4-9482-ff0c06a7edc4   false      Default          2021-02-16T16:18:56+08:00   
50308933-05b4-4363-9c45-00584fc52a43   snapshot2   pending   0          0f136bf8-6530-47e4-9482-ff0c06a7edc4   false      Default          2021-02-16T16:26:04+08:00 
```
{: screen}


### Viewing details of a snapshot by using the CLI
{: #snapshots-vpc-view-cli}

Run the `snapshots {id)` command to see the details of a particular snapshot.

```
ibmcloud is snapshots SNAPSHOT_ID [--json]
```
{: pre}

Example:

```
$ ibmcloud is snapshots --volume 728b2d3c-2165-46c7-9863-9397e0a9af42
Listing snapshots for generation 2 compute in all resource groups and region us-south under account VPC 01 as user rtuser1@mycompany.com...

ID                                          Name   Status   Progress   Source Volume                               Bootable   Resource group   Created   
b2168769-a4dc-4cb8-9fc6-e62d45918858   t2b1   stable   -          728b2d3c-2165-46c7-9863-9397e0a9af42   false      Default          2021-02-26T16:28:58+08:00   
6e7ac183-3223-43d1-8f15-bea30c94eda0   t2b2   stable   -          728b2d3c-2165-46c7-9863-9397e0a9af42   false      Default          2021-02-26T16:29:01+08:00   

```
{: screen}

## Listing snapshots by using the API
{: #snapshots-vpc-view-all-api}
{: api}

### Listing all snapshots by using the API
{: #snapshots-vpc-view-all-api}

Using the [VPC API](https://{DomainName}/apidocs/vpc), make a `GET/snapshots` request to list all snapshots of your volumes. By default, the list shows the most recent snapshots first, followed by older snapshots in descending order. 

```
curl -X GET \
"$vpc_api_endpoint/v1/snapshots?version=2021-02-16&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

You can filter the list by using the resource group ID, source volume ID, or source volume CRN, and further filter the results by using these options:

* Limit - To control the number of snapshots displayed on a page.
* Start - To specify which snapshot (by ID) to start the list.
* Sort - To control the order in which you sort the page, such as by date created or alphabetically by snapshot name.

For more information, see the [VPC API reference](https://{DomainName}/apidocs/vpc).

For example, this call filters the list to show snapshots that were created for a single volume and limits the results to five per page.

   ```
   curl -X GET \
   "$vpc_api_endpoint/v1/snapshots?version=2021-02-16&generation=2" \
   -H "Authorization: $iam_token" \
   -d '{
         "limit": 5,
         "source_volume": {
           "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"
         }
       }
   ```
   {: pre}

A successful response looks like the following example:

   ```
   {
     "snapshots": [
       {
         "id": "1eeae628-b24c-41fb-91b9-bc0e02167f1e",
         "crn": "crn: [...]",
         "name": "my-snapshot-1",
         "resource_group": {
           "id": "bd081e1c-3c0d-4242-84fb-9d02fc963402",
           "href": "https://resource-controller.test.cloud.ibm.com/v2/
           resource_groups/bd081e1c-3c0d-4242-84fb-9d02fc963402",
           "name": "Default"
         },
         "encryption": "provider_managed",
         "encryption_key": null,
         "source_volume": {
           "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
           "crn": "[...]",
           "href": "https://us-south.iaas.cloud.ibm.com/v1/
           volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
           "name": "my-data-volume-1"
         },
         "created_at": "2021-02-16T11:39:04Z",
         "lifecycle_state": "stable",
         "minimum_capacity": 100,
         "source_image": {
           "id": "9eea9ca3-7e67-457d-855e-9b1e751b661b"
         },
         "size": 0,
         "deletable": false,
         "resource_type": "snapshot"
       },
       {
         "id": "3170394c-717f-43b1-8276-35e3fdef53d8",
         "crn": "crn: [...]",
         "href": "/v1/snapshots/3170394c-717f-43b1-8276-35e3fdef53d8",
         "name": "my-snapshot-2",
         "resource_group": {
           "id": "bd081e1c-3c0d-4242-84fb-9d02fc963402",
           "href": "https://resource-controller.test.cloud.ibm.com/v2/
           resource_groups/bd081e1c-3c0d-4242-84fb-9d02fc963402",
           "name": "Default"
         },
         "encryption": "provider_managed",
         "encryption_key": null,
         "source_volume": {
           "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
           "crn": "",
           "href": "https://us-south.iaas.cloud.ibm.com/v1/
           volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
           "name": "my-data-volume-1"
         },
         "created_at": "2021-02-16T07:29:36Z",
         "lifecycle_state": "stable",
         "minimum_capacity": 100,
         "source_image": {
           "id": "9eea9ca3-7e67-457d-855e-9b1e751b661b"
         },
         "size": 0,
         "deletable": false,
         "resource_type": "snapshot"
       },
       {
         "id": "c518cef6-ce08-461e-87e0-549274741feb",
         "crn": "crn: [...]",
         "href": "/v1/snapshots/c518cef6-ce08-461e-87e0-549274741feb",
         "name": "my-snapshot-3",
         "resource_group": {
           "id": "bd081e1c-3c0d-4242-84fb-9d02fc963402",
           "href": "https://resource-controller.test.cloud.ibm.com/v2/
           resource_groups/bd081e1c-3c0d-4242-84fb-9d02fc963402",
           "name": "Default"
         },
         "encryption": "provider_managed",
         "encryption_key": null,
         "source_volume": {
           "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
           "crn": "crn: [...]",
           "href": "https://us-south.iaas.cloud.ibm.com/v1/
           volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
           "name": "my-data-volume-1"
         },
         "created_at": "2021-02-16T07:27:51Z",
         "lifecycle_state": "stable",
         "minimum_capacity": 100,
         "source_image": {
           "id": "9eea9ca3-7e67-457d-855e-9b1e751b661b"
         },
         "size": 0,
         "deletable": false,
         "resource_type": "snapshot"
       }
     ],
     "first": {
       "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots?limit=5"
     },
     "limit": 5,
     "total_count": 3
   }
   ```
   {: codeblock}

### Listing details of a snapshot from the API
{: #snapshots-vpc-view-api}

For details about a single snapshot, use the VPC API to make a `GET/snapshots` call and specify the snapshot ID.

   ```
   curl -X GET \
   "$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?   version=2021-02-16&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

A successful response looks like the following example:

   ```
   {
     "id": "7528eb61-bc01-4763-a67a-a414a103f96d",
     "crn": "crn: [...]",
     "href": "https://us-south.iaas.cloud.ibm.com/v1/
     snapshots/7528eb61-bc01-4763-a67a-a414a103f96d",
     "name": "my-snapshot-4",
     "resource_group": {
       "id": "59ff2d74-b0e5-4b40-a553-b812e50c72e9",
       "href": "https://resource-controller.test.cloud.ibm.com/v2/
       resource_groups/59ff2d74-b0e5-4b40-a553-b812e50c72e9",
       "name": "Default"
     },
     "encryption": "provider_managed",
     "encryption_key": null,
     "source_volume": {
       "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
       "crn": "crn: [...]",
       "href": "https://us-south.iaas.cloud.ibm.com/v1/
       volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
       "name": "my-data-volume-1"
     },
     "created_at": "2021-02-16T11:39:04Z",
     "lifecycle_state": "stable",
     "minimum_capacity": 100,
     "source_image": {
       "id": "r134-32045dc2-b463-4cda-b424-bc3dcf51dfbb"
     },
     "size": 0,
     "deletable": false,
     "resource_type": "snapshot"
   }
   ```
   {: codeblock}

## Next steps
{: #snapshots_vpc_view_next_steps}

* [Modify or delete snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage).
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
