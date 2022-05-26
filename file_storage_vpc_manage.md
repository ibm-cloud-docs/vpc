---

copyright:
  years: 2021, 2022
lastupdated: "2022-05-23"

keywords:

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

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, Sydney, Sao Paulo, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Manage file shares and mount points in the UI
{: #file-storage-manage-ui}
{: ui}

Using the UI, you can:

* [Rename a file share](#rename-file-share-ui)
* [Add mount target to a file share](#add-mount-target-ui)
* [Rename a mount target of a file share](#rename-mount-target-ui)
* [Delete mount target of a file share](#delete-mount-target-ui)
* [Delete a file share](#delete-file-share-ui)
* [Add user tags to file shares](#fs-add-user-tags)

### Rename a file share in the UI
{: #rename-file-share-ui}

1. On the [file shares details](/docs/vpc?topic=vpc-file-storage-view#fs-view-single-share-ui) page, click the pencil icon next to the file share name.

2. Provide a new name for the file share.

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. File share names must begin with a lowercase letter.

### Add mount target to a file share in the UI
{: #add-mount-target-ui}

To mount a file share to an instance using the API, you create a mount target by providing a VPC or subnet information. If you want to connect a file share to instances running in multiple VPCs in the same zone, you can create multiple mount targets for different VPCs.

1. Locate a file share to which you want to add a mount target from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view#file-storage-view-shares-targets-ui).

2. On the File shares details page, under Mount targets, click **Create**.

   You must have at least one VPC to create a mount target. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc).
   {: note}

3. In the **Create mount target** window, provide a name for the mount target.

4. Select a VPC from the list and then click **Create**.

### Rename a mount target of a file share in the UI
{: #rename-mount-target-ui}

1. Navigate to the [file shares details](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-view-single-share-ui) page.

2. Click the overflow menu (hellipsis).

3. Select **Rename**. 

4. Enter a new name and click **Rename**.

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

### Delete file shares and mount targets in the UI
{: #delete-targets-shares-ui}

To delete a file share, you must first delete all its associated mount targets. 

If the file share is configured for replication, there are additional steps. For information, see [Deleting a replica file share](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-delete-replicas).
{: note}

#### Delete mount target of a file share in the UI
{: #delete-mount-target-ui}

To delete a mount target, the file share must be in a `stable` state.

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view#file-storage-view-shares-targets-ui).

2. On the File share details page, select a mount target you want to delete.

3. Click the overflow menu (hellipsis) and select **Delete**.

#### Delete a file share in the UI
{: #delete-file-share-ui}

Before deleting a file share, make sure that it's [unmounted ](#fs-mount-unmount-vsi) from virtual server instances and that all mount targets belonging to the file share are [deleted](#delete-mount-target-ui).

The file share must be in a `stable` state or `failed` state (i.e., when provisioning fails).

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view).

2. Click the overflow menu at the end of the row and select **Delete**.

## Add user tags to a file share
{: #fs-add-user-tags}

You can add user tags when to new or existing file shares, and modify and delete tags for a file share. You can view tags throughout your account by filtering by tags from your resource list. 

You can create as many tags as you like for a file share. However, to keep tags managable, create only as many user tags as you require to effectively manage the resource.
{: note}

Add tags to a file shares in any of these ways:

* Specify user tags when creating a new file share from the [UI](/docs/vpc?topic=vpc-file-storage-create&interface=ui#file-storage-create-ui) or [API](#fs-add-tags-api).

* Add or update tags to an existing file share from the [file share details page](#fs-add-tags-shares-ui) in the UI.

* Use the [VPC API](#fs-add-tags-shares-api) to add or modify tags.

You can also manage your tags in IBM Cloud by using the [Global Tagging API](/apidocs/tagging). With this API, you can create, delete, search, attach, or detach tags.
{: note}

For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag&interface=ui).

### Add user tags to file share in the UI
{: #fs-add-tags-shares-ui}
{: ui}

You can add user tags to a file share in the UI.

1. Navigate to the list of file shares. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. Select a file share to view its details.

3. On the file share details page, user tags next to the file share name. Click the pencil icon to edit tags.

4. In the **Edit tags** popup, type a tag in the User tags text box.

5. Click **Save**.

### Add or modify file share user tags in the API
{: #fs-add-tags-api}
{: api}

#### Add user tags when creating a new file share
{: #fs-add-tags-new-share-api}

Make a `POST /shares` request and specify the `user_tags` property. This example creates a share with three user tags, `env:test1`, `env:test2`, and `env:prod`.

```curl
curl -X POST \ 
"$rias_endpoint/v1/shares?version=2022-05-10&generation=2"\
    -H "Authorization: Bearer $iam_token"\
    -H 'Content-Type: application/json'\
    -d '{
        "name": "share-name1",
        "size": 2300,
        "profile": {
           "name": "tier-3iops"
        },
        "user_tags": [
           "env:test1",
           "env:test2",
           "env:prod"
        ],
        "zone": {
          "name": "us-south-1"
        }
      }'
```
{: pre}

#### Modify user tags for an existing file share
{: #fs-add-tags-share-api}

Add new user tags to an existing file by making a `PATCH /shares` call and specify the user tags in the `user_tags` property. You can specify new tags for a file share without any tags and the user tags are added. If you specify different tags, the existing tags are modified.

The following example modifies a file share identified by ID by renaming the share and adding user tags.

```curl
curl -X PATCH\
"$rias_endpoint/v1/shares/432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3?version=2022-05-10&generation=2"\
-H "Authorization: $iam_token" \
-d '{
    "name": "myshare-patch-1",
    "user_tags": [
      "ut8",
      "ut9"
    ],
  }'
```
{: codeblock}

Response:

```json
{
    "created_at": "2022-05-28T22:31:50Z",
    "crn": "crn": "crn:[...]",
    "encryption": "provider_managed",
    "href": "https://us-south-stage01.iaasdev.cloud.ibm.com/v1/shares/432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3",
    "id": "432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3",
    "initial_owner": {
      "gid": 0,
      "uid": 0
    },
    "iops": 3000,
    "lifecycle_state": "stable",
    "name": "myshare-patch-1",
    "profile": {
      "href": "https://us-south-stage01.iaasdev.cloud.ibm.com/v1/share/profiles/tier-3iops",
      "name": "tier-3iops",
      "resource_type": "share_profile"
    },
    "replication_role": "none",
    "replication_status": "none",
    "replication_status_reasons": [],
    "resource_group": {
      "crn": "crn": "crn:[...]",
      "href": "https://resource-controller.test.cloud.ibm.com/v2/resource_groups/86ccf0a1315646d4bc719fe34ff4d1e3",
      "id": "86ccf0a1315646d4bc719fe34ff4d1e3",
      "name": "Default"
    },
    "resource_type": "share",
    "size": 100,
    "targets": [],
    "user_tags": [
      "ut8",
      "ut9"
    ],
    "zone": {
      "href": "https://us-south-stage01.iaasdev.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
      "name": "us-south-1"
    }
  }
```
{: codeblock}

#### Modify file share user tags with ETag verification
{: #fs-modify-etag-api}

To assure updates to a file share are valid, you can obtain an `ETag` hash string value and specify it in the `If-Match` header while making a `PATCH /shares/{share_id}` call. This assures that no change has happened to the share object between the last observed state and the state at the time of the PATCH call.

To modify existing user tags added to a file share, you first make a `GET /shares/{share_id}` call and copy the hash value from `ETag` property in the response header. You then send the `ETag` value using the `If-Match` header in a `PATCH /shares/{share_id}` request. Specifying an Etag value ensures any updates to or deletions of a file share will fail if the `If-Match` value does not match the file share's current `Etag`. Follow these steps:

1. Make a `GET /shares/{share_id}` call and copy the hash string from `ETag` property in the response header. You will use the hash string when you specify `If-Match` in the `PATCH /shares/{share_id}` request to modify user tags for the share in step 2. For example, to generate the response header information, make a call similar to this:

   ```curl
   curl -sSL -D GET\
   "https://us-south.iaas.cloud.ibm.com/v1/shares/{share_id}?version=2022-05-26&generation=2"\
   -H "Authorization: Bearer $TOKEN" -o /dev/null
   ```
   {: pre}

   In the response header, you'll see something like this:

   ```text
   HTTP/2 200
   date: Tue, 26 May 2022 17:48:03 GMT
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

2. Make a `PATCH /shares/{share_id}` request. Specify the `ETag` hash string for the `If-Match` property in the header. Specify the user tags in the `user_tags` property.

   This example updates the file share user tags to `env:test` and `env:prod`. Note that the hash string value you obtained from the `ETag` property (W/xxxyyyzzz123) is specified in the `If-Match` header in the call.

   ```curl
   curl -X PATCH\
   "$vpc_api_endpoint/v1/shares/50fda9c3-eecd-4152-b473-a98018ccfb10?version=2022-05-26&generation=2"\
      -H "Authorization: Bearer"\
      -H "If-Match: W/xxxyyyzzz123"\
      -d `{
         "user_tags": [
            "env:test2",
            "env:prod2"
         ]
      }'
   ```
   {: codeblock}

## Manage file shares and mount points in the CLI
{: #file-storage-manage-cli}
{: cli}

Using the CLI, you can:

* [Rename a file share](#rename-file-share-cli)
* [Rename a mount target of a file share](#rename-mount-target-cli)
* [Delete mount target of a file share](#delete-mount-target-cli)
* [Delete a file share](#delete-file-share-cli)

### Rename a file share in the CLI
{: #rename-file-share-cli}

Run the `share-update` command and specify a new file share name:

```
ibmcloud is share-update SHARE_ID --name NEW_NAME [--output JSON] [-q, --quiet]
```
{: pre}

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. File share names must begin with a lowercase letter.

### Rename a mount target of a file share in the CLI
{: #rename-mount-target-cli}

Run the `share-target-update` command with the file share name and target name and specify a new mount target name.

```
ibmcloud is share-target-update SHARE_ID TARGET_ID --name NEW_NAME [--output JSON] [-q, --quiet]
```
{: pre}

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

### Delete file shares and mount targets in the CLI
{: #delete-share-targets-cli}

Before you can delete a file share, make sure that all mount targets belonging to the file share are [deleted](#delete-mount-target-cli). The file share must be in a `stable` or `failed` state (i.e., when provisioning fails). 

If the file share is configured for replication, there are additional steps. For information, see [Deleting a replica file share](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-delete-replicas).
{: note}

#### Delete mount target of a file share in the CLI
{: #delete-mount-target-cli}

Run the `share-target-delete` command and specify the file share ID and mount target ID. To delete a mount target, the file share must be in a `stable` state.

```
ibmcloud is share-target-delete SHARE_ID TARGET_ID
```
{: pre}

#### Delete a file share in the CLI
{: #delete-file-share-cli}

Run the `share_delete` command ans specify the file share ID.

Before you can delete a file share, make sure that all mount targets belonging to the file share are [deleted](#delete-mount-target-cli). The file share must be in a `stable` or `failed` state (i.e., when provisioning fails). 


Also, if the file share has a replica file share, you must first remove the replication relationship. For more information, see [Remove the replication relationship in the CLI](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=cli#fs-remove-replication-cli).

```
ibmcloud is share-delete<SHARE_ID.
```
{: pre}

## Manage file shares and mount points in the API
{: #file-storage-manage-api}
{: api}

Using the API, you can:

* [Rename a file share](#rename-file-share-api)
* [Add mount target to a file share](#add-mount-target-api)
* [Rename a mount target of a file share](#rename-mount-target-api)
* [Delete mount target of a file share](#delete-mount-target-api)
* [Delete a file share](#delete-file-share-api)
* [Manage file share user tags](#fs-add-tags-api)

To see information about the File Storage for VPC API methods, see this section in the [API reference](/apidocs/vpc-beta#list-share-profiles).

File Storage for VPC regional API is for customers with special approval to preview this feature. 
{: note}

### Rename a file share in the API
{: #rename-file-share-api}

Make a `PATCH /shares/$share_id` call to rename a specific file share. For example:

```
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id?version=2022-05-03&generation=2" \
  -H "Authorization: Bearer ${API_TOKEN}" \
  -d '{
    "name": "share-renamed1"
  }'
```
{: pre}

A successful response will look like this:

```json
{
  "created_at": "2022-05-15T23:31:59Z",
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

### Add mount target to a file share in the API
{: #add-mount-target-api}

Make a `POST /shares/{share_ID}/targets` call to create a mount target for an existing file share. You must specify a VPC in the request. For example:

```curl
curl -X POST \
"$rias_endpoint/v1/shares/$share_id/targets?version=2022-05-03&generation=2\
-H "Authorization: $iam_token" \
-H 'Content-Type: application/json' \
-d '{
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
  "created_at": "2022-05-15T23:31:59Z",
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

### Rename a mount target of a file share in the API
{: #rename-mount-target-api}

Make a `PATCH /shares/$share_id/targets/$target_id` call to rename a mount target of a file share. For example:

```curl
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id/targets/$target_id?version=2022-05-03&generation=2" \
  -H "Authorization: Bearer ${API_TOKEN}" \
  -d '{
    "name": "target-renamed1"
  }
```
{: pre}

A successful response will look like this:

```json
{
  "created_at": "2022-05-15T23:31:59Z",
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

### Delete file shares and mount targets in the API
{: #delete-share-targets-api}

Before you can delete a file share, make sure that all mount targets belonging to the file share are [deleted](#delete-mount-target-api). The file share must be in a `stable` or `failed` state (i.e., when provisioning fails). 

If the file share is configured for replication, there are additional steps. For information, see [Deleting a replica file share](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-delete-replicas).
{: note}

#### Delete mount target of a file share in the API
{: #delete-mount-target-api}

Make a `DELETE /shares/{share_ID}/targets/{target_id}` call to delete a mount target of a file share. The file share must be in a `stable` state.

For example:

```
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id/targets/$target_id?version=2022-05-03&generation=2" \
  -H "Authorization: Bearer ${API_TOKEN}"
```
{: pre}

A successful response will have confirmation of acceptance for deletion and response containing the target information. Status of mount target will be updated to _pending_deletion_. 

For example:

```json
{
  "created_at": "2022-05-15T23:31:59Z",
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

#### Delete a file share in the API
{: #delete-file-share-api}

Before deleting a file share, make sure that it's [unmounted ](#fs-mount-unmount-vsi) from virtual server instances and that all mount targets belonging to the file share are [deleted](#delete-mount-target-api). Also, if the file share has a replica file share, you must remove the replication relationship. For more information, see [Remove the replication relationship in the API](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-remove-replication-api).

Make a `DELETE /shares/$share_id` call to delete a file share. The file share must be in a `stable` state or `failed` state (i.e., when provisioning fails).

For example:

```
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id?version=05-03-15&generation=2" \
  -H "Authorization: Bearer ${API_TOKEN}"
```
{: pre}

A successful response will confirm acceptance for deletion and show file share information. The status of file share will be updated to _pending_deletion_. 

For example:

```json
{
  "created_at": "2022-02-15T23:31:59Z",
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

A `DELETE /shares/$share_id` call can optionally include an `If-Match` header that specifies the an `ETag` hash string. Make a `GET /shares/{share_id}` call and copy the `ETag` hash string from the response header. For more information, see [User tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-abou#fs-about-user-tags).
{: note}

## Mount and unmount file shares from a virtual server instance
{: #fs-mount-unmount-vsi}

To mount a file share to a virtual server instance, [locate the mount path information](/docs/vpc?topic=vpc-file-storage-view). The mount path is created when you created mount target for a file share. See the following information for mounting on these Linux operating systems. Other Linux distributions follow similar procedures.

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

File Storage for VPC service require IAM permissions for role-based access control. For example, to create a file share, you need to at least editor permissions. For more information, see the [required permissions](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls) for file shares.

## File share lifecycle states
{: #file-storage-vpc-status}

Table 1 describes the states in the file share lifecycle.

| Status | Explanation |
|-----------------|-------------|
| Stable | The file share or mount target is stable and available for use. |
| Pending | The file share or mount target is being created. |
| Failed | The file share or mount target has failed creation. You can delete the failed share and try creating new one. |
| Deleting | The file share or mount target being deleted. |
| Deleted | The file share or mount target has been deleted. |
| Initializing | The file share replica is being created. |
| Split_pending | The replica file share is being split from the source share. |
| Failover_pending | The source file share is failing over to the replica file share, which becomes read-write and the new source share. |
{: caption="Table 2. File storage lifecycle states" caption-side="top"}

## Managing security and compliance
{: #fs-vpc-manage-security}

File Storage for VPC is integrated with the Security and Compliance Center to help you manage security and compliance for your organization. You can set up goals that check whether file shares are encrypted using customer-managed keys. By using the Security and Compliance Center to validate the file service configurations in your account against a profile, you can identify potential issues as they arise.

For information about monitoring security and compliance for VPC, see [Monitoring security and compliance posture with VPC](/docs/vpc?topic=vpc-manage-security-compliance#monitor-vpc). For information about creating security and compliance goals, see [Defining rules](/docs/security-compliance?topic=security-compliance-rules-define&interface=ui) in the Security and Compliance documentation.

## Next steps
{: #fs-manage-next-steps}

Mount and use your file shares:

* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL)
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos)
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu)
