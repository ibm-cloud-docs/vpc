---

copyright:
  years: 2021
lastupdated: "2021-07-17"

keywords: file storage, virtual private cloud, file share

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:preview: .preview}
{:external: target="_blank" .external}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing file shares
{: #file-storage-managing}

Manage file shares you've created. For this release, you can rename a file share, delete a file share, add mount targets to a file share, mount and unmount a file share from virtual server instances, rename a mount target and, delete a mount target.
{: shortdesc}

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, and London regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Use the UI to manage file shares and mount points
{: #file-storage-manage-ui}
{: ui}

Using the UI, you can:

* [Rename a file share](#rename-file-share-ui)
* [Add mount target to a file share](#add-mount-target-ui)
* [Rename a mount target of a file share](#rename-mount-target-ui)
* [Delete mount target of a file share](#delete-mount-target-ui)
* [Delete a file share](#delete-file-share-ui)


### Rename a file share
{: #rename-file-share-ui}

1. On the [file shares details](/docs/vpc?topic=vpc-file-storage-view#fs-view-single-share-ui) page, click the pencil icon next to the file share name.

2. Provide a new name for the share.

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Share names must begin with a lowercase letter.

### Add mount target to a file share
{: #add-mount-target-ui}

To mount a share to an instance using the API, you create a mount target by providing a VPC or subnet information. If you want to connect a share to instances running in multiple VPCs in the same zone, you can create multiple mount targets for different VPCs.

1. Locate a file share to which you want to add a mount target from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view#file-storage-view-shares-targets-ui).

2. On the File shares details page, under Mount targets, click **Create**.

   You must have at least one VPC to create a mount target. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc).
   {: note}

3. In the **Create mount target** window, provide a name for the mount target.

4. Select a VPC from the list and then click **Create**.

### Rename a mount target of a file share
{: #rename-mount-target-ui}

1. Navigate to the [file shares details](/docs/vpc?topic=vpc-file-storage-view) page.

2. Click the overflow menu (hellipsis).

3. Select **Rename**. 

4. Enter a new name and click **Rename**.

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

### Delete mount target of a file share
{: #delete-mount-target-ui}

To delete a mount target, the share must be in a `stable` state.

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view).

2. On the File share details page, select a mount target you want to delete.

3. Click the overflow menu (hellipsis) and select **Delete**.

### Delete a file share
{: #delete-file-share-ui}

Before deleting a file share, make sure that it's unmounted from virtual server instances and that all mount targets belonging to the file share are [deleted](#delete-mount-target-ui). To delete a file share, the it must be in a `stable` state or `failed` state (i.e., when provisioning fails).

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view).

2. Click the overflow menu (hellipsis) and select **Delete**.


## Use the CLI to manage file shares and mount points
{: #file-storage-manage-cli}
{: cli}

Using the CLI, you can:

* [Rename a file share](#rename-file-share-cli)
* [Rename a mount target of a file share](#rename-mount-target-cli)
* [Delete mount target of a file share](#delete-mount-target-cli)
* [Delete a file share](#delete-file-share-cli)

### Rename a file share
{: #rename-file-share-cli}

Run the `share-update` command and spedify a new file share name:

```
ibmcloud is share-update SHARE_ID --name NEW_NAME [--output JSON] [-q, --quiet]
```
{: pre}

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Share names must begin with a lowercase letter.

### Rename a mount target of a file share
{: #rename-mount-target-cli}

Run the `share-target-update` command with the share name and target name and specify a new mount target name.

```
ibmcloud is share-target-update SHARE_ID TARGET_ID --name NEW_NAME [--output JSON] [-q, --quiet]
```
{: pre}

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

### Delete mount target of a file share
{: #delete-mount-target-cli}

Run the `share-target-delete` command and specify the share ID and mount target ID. To delete a mount target, the share must be in a `stable` state.

```
ibmcloud is share-target-delete SHARE_ID TARGET_ID
```
{: pre}

### Delete a file share
{: #delete-file-share-cli}

Run the `share_delete` command ans specify the share ID.

Before you can delete a file share, make sure that it's unmounted from virtual server instances and that all mount targets belonging to the file share are [deleted](#delete-mount-target-cli). To delete a file share, the share must be in a `stable` or `failed` state (i.e., when provisioning fails).

```
ibmcloud is share-delete<SHARE_ID.
```
{: pre}

## Use the API to manage file shares and mount points
{: #file-storage-manage-api}
{: api}

Using the API, you can:

* [Rename a file share](#rename-file-share-api)
* [Add mount target to a file share](#add-mount-target-api)
* [Rename a mount target of a file share](#rename-mount-target-api)
* [Delete mount target of a file share](#delete-mount-target-api)
* [Delete a file share](#delete-file-share-api)

To see information about the File Storage for VPC API methods, see this section in the [API reference](/apidocs/vpc-beta#list-share-profiles).

File Storage for VPC regional API is a beta-level release for customers with special approval to preview this feature. 
{: note}

### Rename a file share
{: #rename-file-share-api}

Make a `PATCH /shares/$share_id` call to rename a specific share. For example:

```
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id?version=2021-02-15&generation=2" \
  -H "Authorization: Bearer ${API_TOKEN}" \
  -d $'{
    "name": "share-renamed1"
  }'
```
{: pre}

A successful response will look like this:

```json
{
  "created_at": "2021-03-31T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "$vpc_api_endpoint/v1/shares/0995b8c6-c323-4e59-9ea9-fa14dd66bba8",
  "id": "0995b8c6-c323-4e59-9ea9-fa14dd66bba8",
  "iops": 3000,
  "lifecycle_state": "stable",
  "name": "share-renamed1",
  "profile": {
    "href": "$vpc_api_endpoint/v1/share/profiles/tier-10iops",
    "name": "tier-10iops",
    "resource_type": "share_profile"
  },
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/bfb4a7c7-00d8-400b-98ba-5a67e5851970",
    "id": "bfb4a7c7-00d8-400b-98ba-5a67e5851970",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 100,
  "targets": [],
  "zone": {
    "href": "$vpc_api_endpoint/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Add mount target to a file share
{: #add-mount-target-api}

Make a `POST /shares/{share_ID}/targets` call to create a mount target for an existing file share. You must specify a VPC in the request. For example:

```curl
curl -X POST \
"$rias_endpoint/v1/shares/$share_id/targets?version=2021-03-15&generation=2\
-H "Authorization: $iam_token" \
-H 'Content-Type: application/json' \
-d $'{
  "name": "target-name1",
  "vpc": {
    "id": 4d1c51ac-1144-4e32-af86-a0349fabc266"
  }
}
```
{: pre}

A successful response will look like this:

```json
{
  "created_at": "2021-03-31T23:31:59Z",
  "href": "$vpc_api_endpoint/v1/shares/0995b8c6-c323-4e59-9ea9-fa14dd66bba8/targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "lifecycle_state": "pending",
  "mount_path": "domain.com:/vol_xyz_2891fd0a_64ea_4deb_9ed5_1159e37cb5aa",
  "name": "target-name1",
  "resource_type": "share_target",
  "vpc": {
    "crn": "crn:[...]",
    "href": "$vpc_api_endpoint/v1/vpcs8c95b3c1-fe3c-45c-97a6-e43d14088287",
    "id": "82a7b841-9586-43b4-85dc-c0ab5e8b1c7a",
    "name": "vpc-name1",
    "resource_type": "vpc"
  }
}
```
{: codeblock}

### Rename a mount target of a file share
{: #rename-mount-target-api}

Make a `PATCH /shares/$share_id/targets/$target_id` call to rename a mount target of a file share. For example:

```curl
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id/targets/$target_id?version=2021-03-15&generation=2" \
  -H "Authorization: Bearer ${API_TOKEN}" \
  -d $'{
    "name": "target-renamed1"
  }
```
{: pre}

A successful response will look like this:

```json
{
  "created_at": "2021-03-31T23:31:59Z",
  "href": "$vpc_api_endpoint/v1/shares/0995b8c6-c323-4e59-9ea9-fa14dd66bba8/targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "lifecycle_state": "stable",
  "mount_path": "domain.com:/vol_xyz_2891fd0a_64ea_4deb_9ed5_1159e37cb5aa",
  "name": "target-renamed1",
  "resource_type": "share_target",
  "vpc": {
    "crn": "crn:[...]",
    "href": "$vpc_api_endpoint/v1/vpcs8c95b3c1-fe3c-45c-97a6-e43d14088287",
    "id": "82a7b841-9586-43b4-85dc-c0ab5e8b1c7a",
    "name": "vpc-name1",
    "resource_type": "vpc"
  }
}
```
{: pre}

### Delete mount target of a file share
{: #delete-mount-target-api}

Make a `DELETE /shares/{share_ID}/targets/{target_id}` call to delete a mount target of a file share. The file share must be in a `stable` state.

For example:

```
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id/targets/$target_id?version=2021-03-15&generation=2" \
  -H "Authorization: Bearer ${API_TOKEN}"
```
{: pre}

A successful response will have confirmation of acceptance for deletion and response containing the target information. Status of mount target will be updated to _pending_deletion_. 

For example:

```json
{
  "created_at": "2021-03-31T23:31:59Z",
  "href": "$vpc_api_endpoint/v1/shares/0995b8c6-c323-4e59-9ea9-fa14dd66bba8/targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "lifecycle_state": "pending_deletion",
  "mount_path": "domain.com:/vol_xyz_2891fd0a_63aa_4deb_9ed5_1159e37cb5aa",
  "name": "target-name1",
  "resource_type": "share_target",
  "vpc": {
    "crn": "crn:[...]",
    "href": "$vpc_api_endpoint/v1/vpcs8c95b3c1-fe3c-45c-97a6-e43d14088287",
    "id": "82a7b841-9586-43b4-85dc-c0ab5e8b1c7a",
    "name": "vpc-name1",
    "resource_type": "vpc"
  }
}
```
{: codeblock}

The mount target is deleted in the background. Confirm the deletion by trying to view the mount target information. If you get _404 Not Found_ error, the mount target is successfully deleted.

### Delete a file share
{: #delete-file-share-api}

Before deleting a file share, make sure that it's unmounted from virtual server instances and that all mount targets belonging to the file share are [deleted](#delete-mount-target-api). Then, make a `DELETE /shares/$share_id` call to delete a file share. The file share must be in a `stable` state or `failed` state (i.e., when provisioning fails).

For example:

```
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id?version=2021-03-15&generation=2" \
  -H "Authorization: Bearer ${API_TOKEN}"
```
{: pre}

A successful response will confirm acceptance for deletion and show file share information. The status of file share will be updated to _pending_deletion_. 

For example:

```json
{
  "created_at": "2021-03-31T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "$vpc_api_endpoint/v1/shares/0995b8c6-c323-4e59-9ea9-fa14dd66bba8",
  "id": "0995b8c6-c323-4e59-9ea9-fa14dd66bba8",
  "iops": 3000,
  "lifecycle_state": "pending_deletion",
  "name": "share-name1",
  "profile": {
    "href": "$vpc_api_endpoint/v1/share/profiles/tier-10iops",
    "name": "tier-10iops",
    "resource_type": "share_profile"
  },
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/bfb4a7c7-00d8-400b-98ba-5a67e5851970",
    "id": "bfb4a7c7-00d8-400b-98ba-5a67e5851970",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 100,
  "targets": [],
  "zone": {
    "href": "$vpc_api_endpoint/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

The file share is deleted in background. Confirm the deletion by trying to view the mount target information. If you get _404 Not Found_ error, the mount target is successfully deleted.

## Mount and Unmount File Share from a virtual server instance
{: #fs-mount-unmount-vsi}

To mount a file share to a virtual server instance, [locate the share mount path information](vpc?topic=vpc-file-storage-view). The mount path is created when you created mount target for a file share. See the following information for mounting on these Linux operating systems. Other Linux distributions follow similar procedures.

* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL)
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos)
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu)

## File storage data eradication
{: #file-storage-data-eradication}

When you delete a file share, that data immediately becomes inaccessible. All pointers to the data on the physical disk are removed. If you later create a new file share in the same or another account, a new set of pointers is assigned. The account can't access any data that was on the physical storage because those pointers are deleted. When new data is written to the disk, any inaccessible data from the deleted file storage is overwritten. 

IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten and eradicated. When you delete a file share, those blocks must be overwritten before that file storage is made available again, either to you or to another customer.

Further, when IBM decomissions a physical drive, the drive is destroyed prior to disposal. Decomissioned physical drives are unusable and any data on them is completely inaccessible.

## IAM Roles for creating and managing file shares
{: #file-storage-vpc-iam}

File Storage for VPC service require IAM permissions for role-based access control. Table 1 describes these roles are they pertain to snapshots actions.

| VPC File Storage action | IAM role |
|-----------------|----------|
| Create share | Administrator, editor |
| List all shares | Administrator, editor, operator, viewer |
| View share details | Administrator, editor, operator, viewer |
| Update share | Administrator, editor |
| Delete share | Administrator, editor |
| Create mount target | Administrator, editor |
| List all mount target | Administrator, editor, operator, viewer |
| View mount target details | Administrator, editor, operator, viewer |
| Update mount target | Administrator, editor |
| Delete mount target | Administrator, editor |
{: caption="Table 1. IAM Roles for using the file service" caption-side="top"}

## File share lifecycle states
{: #file-storage-vpc-status}

Table 1 describes the states in the file share lifecycle.

| Status | Explanation |
|-----------------|-------------|
| Stable | The file share or mount target is stable and available for use. |
| Pending | The file share or mount target is being created. |
| Failed | The file share or mount target has failed creation. You can delete the failed share and try creating new one. |
| Deleting | The share or mount target being deleted. |
| Deleted | The share or mount target has been deleted. |
{: caption="Table 2. File storage lifecycle states" caption-side="top"}

## Next steps
{: #fs-manage-next-steps}

Mount and use your file shares:

* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL)
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos)
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu)
