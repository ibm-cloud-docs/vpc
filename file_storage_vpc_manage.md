---

copyright:
  years: 2021, 2023
lastupdated: "2023-06-02"

keywords: file share, file storage, rename share, increase size, adjust IOPS, mount target

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing file shares
{: #file-storage-managing}

Manage file shares that you created. You can rename a file. You can increase its capacity and modify its IOPS. You can add mount targets to a file share, mount, and unmount a file share from virtual server instances. You can rename or delete a mount target, and you can delete a file share.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Madrid, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

{{site.data.keyword.filestorage_vpc_short}} service requires IAM permissions for role-based access control. For example, to create a file share, you need to at least editor permissions. For more information, see the [required permissions](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls) for file shares.
{: requirement}

## Manage file shares and mount points in the UI
{: #file-storage-manage-ui}
{: ui}

In the console, you can:

* [Rename a file share](#rename-file-share-ui).
* [Add mount target to a file share](#add-mount-target-ui).
* [Rename a mount target of a file share](#rename-mount-target-ui).
* [Update a file share profile](#fs-update-profile-ui)
* [Delete mount target of a file share](#delete-mount-target-ui).
* [Delete a file share](#delete-file-share-ui).
* [Add user tags to file shares](#fs-add-user-tags).

### Rename a file share in the UI
{: #rename-file-share-ui}

1. On the [file shares details](/docs/vpc?topic=vpc-file-storage-view#fs-view-single-share-ui) page, click the pencil icon next to the file share name.

2. Provide a new name for the file share.

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. File share names must begin with a lowercase letter.

### Add mount target to a file share in the UI
{: #add-mount-target-ui}

To mount a file share to an instance, you must create a mount target by providing a VPC or subnet information. If you want to connect a file share to instances that are running in multiple VPCs in the same zone, you can create multiple mount targets for different VPCs.

1. Locate a file share to which you want to add a mount target from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view#file-storage-view-shares-targets-ui).

2. On the File shares details page, under Mount targets, click **Create**.

   You must have at least one VPC to create a mount target. If you don't have one, first [create a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc).
   {: note}

3. In the **Create mount target** window, provide a name for the mount target.

4. Select a VPC from the list and then click **Create**.

### Rename a mount target of a file share in the UI
{: #rename-mount-target-ui}

1. Go to the [file shares details](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-view-single-share-ui) page.

2. Click the overflow menu (ellipsis).

3. Select **Rename**.

4. Enter a new name and click **Rename**.

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

### Update a file share profile in the UI
{: #fs-update-profile-ui}

You can change the profile for a file share from the current profile to another **IOPS tier** profile, to a **custom** profile, or to a high-performance **dp2** profile. Your billing adjusts based on the type of profile that you choose.

1. Go to the [file shares details](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-view-single-share-ui) page.

2. Click the pencil icon next to the current profile or use the **Actions** menu and select **Edit IOPS profile**. A side panel shows the current profile, file share size, and maximum IOPS.

3. For **New profile**, click the down arrow. You can select a new IOPS tier, a custom profile, or dp2. For **Custom IOPS** or **dp2**, specify a new max IOPS based on the file share size. The file share price is automatically calculated based on your seletion.

4. Click **Save and continue**.

### Delete file shares and mount targets in the UI
{: #delete-targets-shares-ui}

To delete a file share, you must first delete all its associated mount targets.

If the file share is configured for replication, you need to take extra steps. For more information, see [Deleting a replica file share](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-delete-replicas).
{: note}

#### Delete mount target of a file share in the UI
{: #delete-mount-target-ui}

To delete a mount target, the file share must be in a `stable` state.

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view#file-storage-view-shares-targets-ui).

2. On the File share details page, select a mount target that you want to delete.

3. Click the overflow menu (ellipsis) and select **Delete**.

#### Delete a file share in the UI
{: #delete-file-share-ui}

Before you delete a file share, make sure that it is [unmounted](#fs-mount-unmount-vsi) from all virtual server instances and that all mount targets that belong to the file share are [deleted](#delete-mount-target-ui).

The file share must be in a `stable` state or `failed` state (that is, when provisioning fails).

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view).

2. Click the overflow menu at the end of the row and select **Delete**.

## Add user tags to a file share
{: #fs-add-user-tags}

You can add user tags to new or existing file shares, modify, and delete tags for a file share with the UI, CLI, API, and Terraform. You can view tags throughout your account by filtering by tags from your resource list. You can also add user tags to replica file shares.

You can create as many user tags as you like for a file share. However, to keep tags manageable, create only as many user tags as you require to effectively manage the resource.
{: tip}

You can manage your tags in the {{site.data.keyword.cloud_notm}} with the [Global Tagging API](/apidocs/tagging). With this API, you can create, delete, search, attach, or detach tags. For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag).

### Add user tags to file share in the UI
{: #fs-add-tags-shares-ui}
{: ui}

You can add user tags to a file share in the UI.

1. Go to the list of file shares. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. Select a file share to view its details.

3. On the file share details page, user tags appear next to the file share name. Click the pencil icon to edit tags.

4. In the **Edit tags** window, type a tag in the User tags text box.

5. Click **Save**.

### Add or modify file share user tags from the CLI
{: #fs-add-tags-cli}
{: cli}

To add tags when you provision a file share, run the `ibmcloud is share-create` command. To update an existing file share, run the `ibmcloud is share-update` command. The `--user-tags` option specifies tags for the file share.

The following example updates an existing file share that is identified by ID and adds the user tags `env:test` and `env:prod`.

```sh
$ ibmcloud is share-update 50fda9c3-eecd-4152-b473-a98018ccfb10 --user-tags env:test,env:prod
Updating share 50fda9c3-eecd-4152-b473-a98018ccfb10 under account VPC1 as user user.mycompany.com...

ID                50fda9c3-eecd-4152-b473-a98018ccfb10
Name              myshare-1
CRN               crn:[...]
Lifecycle State   stable
Zone              us-south-1
Profile           tier-3iops
Size(GB)          40
IOPS              3000
Encryption        provider_managed
Mount targets     ID                                       Name           VPC ID                                   VPC Name
                  87171615-f8b4-4ccd-a216-bab12eb1a973     my-target121   81ee2425-7c2f-4042-9d8f-d02b0ee8886c     vpc1
                  b041fc54-f0b9-4397-b68e-2ab032cd1206     my-target122   c338ca08-1256-4420-9932-a9960a0f1cfd     vpc2
Resource Group    ID                                 Name
                  bdd96715c2a44f2bb60df4ff14a543f5   Default
User Tags         env:test,env:prod

Created           2023-01-07T15:26:21+05:30
```
{: screen}

### Add or modify file share user tags with the API
{: #fs-add-tags-api}
{: api}

As described in the [Beta VPC API](/apidocs/vpc-beta) reference [versioning](/apidocs/vpc-beta#api-versioning-beta) policy, support for older versions of the beta API is limited to 45 days. Therefore, beta API requests must specify a `version` query parameter date value within the last 45 days. You must also provide `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc#api-generation-parameter).
{: requirement}

#### Add user tag when a file share is created
{: #fs-add-tags-new-share-api}

Make a `POST /shares` request and specify the `user_tags` property. This example creates a share with three user tags, `env:test1`, `env:test2`, and `env:prod`.

```sh
curl -X POST \
"$rias_endpoint/v1/shares?version=2023-07-11&generation=2&maturity=beta&maturity=beta"\
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
{: codeblock}

#### Modify user tags for an existing file share
{: #fs-add-tags-share-api}

Add new user tags to an existing file share by making a `PATCH /shares` call and specify the user tags in the `user_tags` property. You can specify new tags for a file share without any tags and the user tags are added. If you specify different tags, the existing tags are modified.

The following example modifies a file share that is identified by ID by renaming the share and adding user tags.

```sh
curl -X PATCH\
"$rias_endpoint/v1/shares/432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3?version=2023-07-11&generation=2&maturity=beta&maturity=beta"\
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
    "access_control_mode": "vpc",
    "created_at": "2023-01-28T22:31:50Z",
    "crn": "crn": "crn:[...]",
    "encryption": "provider_managed",
    "href": "https://us-south-1.cloud.ibm.com/v1/shares/432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3",
    "id": "432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3",
    "initial_owner": {
      "gid": 0,
      "uid": 0
    },
    "iops": 3000,
    "lifecycle_state": "stable",
    "name": "myshare-patch-1",
    "profile": {
      "href": "https://us-south-1.cloud.ibm.com/v1/share/profiles/tier-3iops",
      "name": "tier-3iops",
      "resource_type": "share_profile"
    },
    "replication_role": "none",
    "replication_status": "none",
    "replication_status_reasons": [],
    "resource_group": {
      "crn": "crn": "crn:[...]",
      "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/86ccf0a1315646d4bc719fe34ff4d1e3",
      "id": "86ccf0a1315646d4bc719fe34ff4d1e3",
      "name": "Default"
    },
    "resource_type": "share",
    "size": 100,
    "mount_targets": [],
    "user_tags": [
      "ut8",
      "ut9"
    ],
    "zone": {
      "href": "https://us-south-1.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
      "name": "us-south-1"
    }
  }
```
{: screen}

#### Modify file share user tags with ETag verification
{: #fs-modify-etag-api}

To ensure that updates to a file share are valid, you can obtain an `ETag` hash string value and specify it in the `If-Match` header in the `PATCH /shares/{share_id}` call. That confirms that no change happened to the share object between the last observed state and the state at the time of the `PATCH` call.

To modify existing user tags that are added to a file share, you first make a `GET /shares/{share_id}` call and copy the hash value from `ETag` property in the response header. Then, you send the `ETag` value by using the `If-Match` header in a `PATCH /shares/{share_id}` request. Specifying an `Etag` value ensures any updates to or deletions of a file share fail if the `If-Match` value does not match the file share's current `Etag`. Follow these steps:

1. Make a `GET /shares/{share_id}` call and copy the hash string from `ETag` property in the response header. Use need the hash string value for when you specify `If-Match` in the `PATCH /shares/{share_id}` request to modify user tags for the share in step 2.

   ```sh
   curl -sSL -D GET\ "https://us-south.cloud.ibm.com/v1/shares/{share_id}?version=2023-07-11&generation=2&maturity=beta&maturity=beta"\
   -H "Authorization: Bearer $TOKEN" -o /dev/null
   ```
   {: codeblock}

   The response header looks similar to the following example:

   ```json
   HTTP/2 200
   date: Mon, 09 January 2023 17:48:03 GMT
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

   This example updates the file share user tags to `env:test` and `env:prod`. The hash string value that you obtained from the `ETag` property (`W/xxxyyyzzz123`) is specified in the `If-Match` header in the call.

   ```sh
   curl -X PATCH\
   "$vpc_api_endpoint/v1/shares/50fda9c3-eecd-4152-b473-a98018ccfb10?version=2023-07-11&generation=2&maturity=beta&maturity=beta"\
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

## Add access management tags to a file share
{: #fs-add-access-mgt-tags}
{: ui}

Access management tags are metadata that you can add to your file shares to help organize access control resource relationships. You first create the tag and then add it to an existing file share or when you create a file share. You can apply the same access management tag to multiple file shares. You can assign access to the tag in {{site.data.keyword.iamshort}} (IAM). Optionally, you can create an IAM access group and manage users.

### Step 1 - Create an access management tag in IAM
{: #fs-create-access-mgt-tag}

In the console:

1. Go to **Manage > Account**, and then select **Tags**.
2. Click the **Access management tags** tab. Add a tag name in the field. Access management tags require a `key:value` format.
3. Click **Create Tags**.

From the [Global Search and Tagging API](/docs/account?topic=account-tag&interface=api#create-access-api):

Make a `POST/ tags` call to [create an access management tag](/apidocs/tagging#create-tag) and specify the tag in the `tag_names` property. For an example, see [Creating access management tags with the API](/docs/account?topic=account-tag&interface=api#create-access-api).

### Step 2 - Add an access management tag to a file share
{: #fs-add-access-mgt-tag}

Add an access management tag to an existing file share or when you [create a file share](/docs/vpc?topic=vpc-file-storage-create). For an existing file share:

1. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, Go to the Resource list, and select a file share under **Storage** resources.
2. In the **Access management tags** field, type the name of an access management tag that you previously created. The tag appears as you type.
3. Save your changes.

### Step 3 - Create an access group and assign access to users
{: #fs-access-mgt-additional-steps}

After you create an access management tag and apply it to a file share, complete the following steps to assign access and add users:

1. [Create an access group](/docs/account?topic=account-access-tags-tutorial#tagging-create-access-group). Access groups are assigned to policies that grant roles and permissions to the members of that group. You assign access to the specific access management tags for the file service. For more information about access groups, see [Setting up access groups](/docs/account?topic=account-groups&interface=ui).

2. [Assign an access policy to a group](/docs/account?topic=account-access-tags-tutorial#tagging-assign-policy).

3. [Add users to the access group](/docs/account?topic=account-access-tags-tutorial#tagging-add-users-access-group).

When you look at the specific resources for the VPC infrastructure and specify {{site.data.keyword.filestorage_vpc_short}} as the resource type, you can see the access management tags for the file service.

## Manage file shares and mount targets from the CLI
{: #file-storage-manage-cli}
{: cli}

By using the CLI, you can:

* [Rename a file share](#rename-file-share-cli).
* [Rename a mount target of a file share](#rename-mount-target-cli).
* [Delete mount target of a file share](#delete-mount-target-cli).
* [Delete a file share](#delete-file-share-cli).
* [Update a file share profile](#fs-update-profile-cli)

As of 30 May 2023, you can use `--mount-targets` instead of `--targets` option. To see and use the updated option, set the feature environment variable `IBMCLOUD_IS_FEATURE_FILESHARE_CHANGE_TO_MOUNT_TARGETS` to true.
{: beta}

   ```text
   export IBMCLOUD_IS_FEATURE_FILESHARE_CHANGE_TO_MOUNT_TARGETS=true
   ```
   {: pre}

### Rename a file share from the CLI
{: #rename-file-share-cli}

Run the `share-update` command and specify a new file share name:

```sh
ibmcloud is share-update SHARE_ID --name NEW_NAME [--output JSON] [-q, --quiet]
```
{: pre}

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. File share names must begin with a lowercase letter.

### Rename a mount target of a file share from the CLI
{: #rename-mount-target-cli}

Run the `share-mount-target-update` command with the file share name or ID, and the mount target name. Specify a new mount target name.

```sh
ibmcloud is share-mount-target-update SHARE/SHARE_ID MOUNT_TARGET_ID --name NEW_NAME [--output JSON] [-q, --quiet]
```
{: pre}

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

### Update a file share profile in the CLI
{: #fs-update-profile-cli}

Use the `share-update` command with the `--profile` parameter to indicate the new file share profile by name.

```sh
ibmcloud is share-update SHARE_NAME --profile PROFILE_NAME
```
{: pre}

This example updates a file share profile to a dp2 profile.

```sh
ibmcloud is share-update my-file-share --profile dp2
Updating file share my-file-share under account VPC1 as user user1@mycompany.com...

ID                e1180bae-6799-4156-8bb7-e4d24013cda2
Name              my-file-share
CRN               crn:v1:public:is:us-south-1:a/0ef3f509-04f4-465e-88c6-2d60b164d805::share:e1180bae-6799-4156-8bb7-e4d24013cda2
Lifecycle state   updating
Zone              us-south-1
Profile           dp2
.
.
.
```
{: screen}

### Delete file shares and mount targets from the CLI
{: #delete-share-targets-cli}

Before you can delete a file share, make sure that all mount targets that belong to the file share are [deleted](#delete-mount-target-cli). The file share must be in a `stable` or `failed` state (that is, when provisioning fails).

If the file share is configured for replication, you must take extra steps. For more information, see [Deleting a replica file share](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-delete-replicas).
{: note}

#### Delete a mount target of a file share from the CLI
{: #delete-mount-target-cli}

Run the `share-mount-target-delete` command and specify the file share ID and mount target ID. To delete a mount target, the file share must be in a `stable` state.

```sh
ibmcloud is share-mount-target-delete <SHARE_ID> <MOUNT_TARGET_ID>
```
{: pre}

#### Delete a file share from the CLI
{: #delete-file-share-cli}

Run the `share_delete` command and specify the file share ID.

Before you can delete a file share, make sure that all mount targets that belong to the file share are [deleted](#delete-mount-target-cli). The file share must be in a `stable` or `failed` state (that is, when provisioning fails).

Also, if the file share has a replica file share, you must first remove the replication relationship. For more information, see [Remove the replication relationship in the CLI](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=cli#fs-remove-replication-cli).

```sh
ibmcloud is share-delete <SHARE_ID>
```
{: pre}

## Manage file shares and mount points with the API
{: #file-storage-manage-api}
{: api}

By using the API, you can:

* [Rename a file share](#rename-file-share-api).
* [Add mount target to a file share](/docs/vpc?topic=vpc-file-storage-create&interface=api#fs-create-mount-target-api).
* [Rename a mount target of a file share](#rename-mount-target-api).
* [Update a file share profile](#fs-update-profile-api)
* [Delete mount target of a file share](#delete-mount-target-api).
* [Delete a file share](#delete-file-share-api).
* [Manage file share user tags](#fs-add-tags-api).

To see information about the {{site.data.keyword.filestorage_vpc_short}} API methods, see the following section in the [API reference](/apidocs/vpc-beta#list-share-profiles).

{{site.data.keyword.filestorage_vpc_short}} regional API is available for customers with special approval to preview this feature.
{: beta}

### Rename a file share in the API
{: #rename-file-share-api}

Make a `PATCH /shares/$share_id` call to rename a specific file share. See the following example.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id?version=2023-07-11&generation=2&maturity=beta" \
  -H "Authorization: Bearer ${API_TOKEN}" \
  -d '{
    "name": "share-renamed1"
  }'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-03-30T23:31:59Z",
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
  "mount_targets": [],
  "zone": {
    "href": "$vpc_api_endpoint/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Rename a mount target of a file share with the API
{: #rename-mount-target-api}

Make a `PATCH /shares/$share_id/mount_targets/$target_id` call to rename a mount target of a file share. See the following example.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets/$target_id?version=2023-07-11&generation=2&maturity=beta" \
  -H "Authorization: Bearer ${API_TOKEN}" \
  -d '{
    "name": "target-renamed1"
  }
```
{: pre}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-05-30T23:31:59Z",
  "href": "$vpc_api_endpoint/v1/shares/0995b8c6-c323-4e59-9ea9-fa14dd66bba8/mount_targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "lifecycle_state": "stable",
  "mount_path": "domain.com:/vol_xyz_2891fd0a_64ea_4deb_9ed5_1159e37cb5aa",
  "name": "target-renamed1",
  "resource_type": "share_target",
  "transit_encryption": "none",
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

### Update a file share profile in the API
{: #fs-update-profile-api}

Make a `PATCH /shares/{share_ID}` call and specify the profile name in the `profile` property. The following example changes the profile to a `dp2` profile.

```sh
curl -X PATCH "$rias_endpoint/v1/shares/432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3?version=2023-07-11&generation=2&maturity=beta" \
-H "Authorization: $iam_token" \
-d '{
    "profile": {
        "name": "dp2"
      }
  }'
```
{: codeblock}

### Delete file shares and mount targets with the API
{: #delete-share-targets-api}

Before you can delete a file share, make sure that all mount targets that belong to the file share are [deleted](#delete-mount-target-api). The file share must be in a `stable` or `failed` state (that is, when provisioning fails).

If the file share is configured for replication, you myust take extra steps. For more information, see [Deleting a replica file share](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-delete-replicas).
{: note}

#### Delete mount target of a file share with the API
{: #delete-mount-target-api}

Make a `DELETE /shares/{share_ID}/mount_targets/{target_id}` call to delete a mount target of a file share. The file share must be in a `stable` state.

See the following example.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets/$target_id?version=2023-07-11&generation=2&maturity=beta" \
  -H "Authorization: Bearer ${API_TOKEN}"
```
{: pre}

A successful response has a confirmation of acceptance for deletion and response that contains the target information. Status of mount target shows _deleting_ while the deletion is underway.

The following example deletes a mount target where `access_control_mode` is `security_group`. The response shows the security group and subnet. You can also see the specifics of the reserved IP address that was used for the virtual network interface of the mount target in the `primary_ip` section. By default the virtual network interface is deleted along with the mount target when the mount target is deleted.

```json
{
  "access_control_mode": "security_group",
  "created_at": "2023-05-30T01:59:46.000Z",
  "href": "$vpc_api_endpoint/v1/shares/0995b8c6-c323-4e59-9ea9-fa14dd66bba8/mount_targets/9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "id": "9fdf4438-f5b4-4b6f-8bca-602494fd6c31",
  "lifecycle_reasons": [],
  "lifecycle_state": "deleting",
  "mount_path": "domain.com:/vol_xyz_2891fd0a_63aa_4deb_9ed5_1159e37cb5aa",
  "name": "target-name1",
  "primary_ip": {
    "address": "10.10.12.64",
    "href": "$vpc_api_endpoint/v1/subnets/2302-ea5fe79f-52c3-4f05-86ae-9540a10489f5/reserved_ips/0716-6fd4925d-7774-4e87-829e-7e5765d454ad",
    "id": "0716-6fd4925d-7774-4e87-829e-7e5765d454ad",
    "name": "my-reserved-ip",
    "resource_type": "subnet_reserved_ip"
  },
  "resource_type": "share_mount_target",
  "security_groups": [
    {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/security_groups/r134-1dfeccef-3ad6-4760-8653-a202bc795db4",
      "id": "r134-1dfeccef-3ad6-4760-8653-a202bc795db4",
      "name": "my-security-group",
      "resource_type": "security_group"
    }
  ],
  "subnet": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/2302-ea5fe79f-52c3-4f05-86ae-9540a10489f5",
    "id": "2302-ea5fe79f-52c3-4f05-86ae-9540a10489f5",
    "name": "my-subnet",
    "resource_type": "subnet"
  },
  "transit_encryption": "none",
  "virtual_network_interface": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/virtual_network_interfaces/4727d842-f94f-4a2d-824a-9bc9b02c523b",
    "id": "4727d842-f94f-4a2d-824a-9bc9b02c523b",
    "name": "my-virtual-network-interface",
    "resource_type": "virtual_network_interface"
  },
  "vpc": {
  "crn": "crn:[...]",
  "href": "$vpc_api_endpoint/v1/vpcs/8c95b3c1-fe3c-45c-97a6-e43d14088287",
  "id": "82a7b841-9586-43b4-85dc-c0ab5e8b1c7a",
  "name": "vpc-name1",
  "resource_type": "vpc"
  }
}
```
{: codeblock}

The mount target is deleted in the background. Confirm the deletion by trying to view the mount target information. If you get `_404 Not Found_` error, the mount target is successfully deleted.

#### Delete a file share in the API
{: #delete-file-share-api}

Before you delete a file share, make sure that it is [unmounted](#fs-mount-unmount-vsi) from all virtual server instances and that all mount targets that belong to the file share are [deleted](#delete-mount-target-api). Also, if the file share has a replica file share, you must remove the replication relationship. For more information, see [Remove the replication relationship with the API](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-remove-replication-api).

Make a `DELETE /shares/$share_id` call to delete a file share. The file share must be in a `stable` state or `failed` state (that is, when provisioning fails).

Example:

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id?version=2023-07-11&generation=2&maturity=beta" \
  -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

A successful response confirms acceptance for deletion and shows file share information. The status of file share is updated to _pending_deletion_.

Example:

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-05-30T23:31:59Z",
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
  "mount_targets": [],
  "zone": {
    "href": "$vpc_api_endpoint/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

The file share is deleted in background. Confirm the deletion by trying to view the mount target information. If you get `_404 Not Found_` error, the mount target is successfully deleted.

A `DELETE /shares/$share_id` call can optionally include an `If-Match` header that specifies an `ETag` hash string. Make a `GET /shares/{share_id}` call and copy the `ETag` hash string from the response header. For more information, see [User tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-abou#fs-about-user-tags).
{: note}

## Manage file shares and mount targets with Terraform
{: #file-storage-manage-terraform}
{: terraform}

With Terraform, you can:

* [Change file share attributes such as name, size, profile, tags](#file-storage-share-update-terraform),
* [Add](/docs/vpc?topic=vpc-file-storage-create-replication&interface=terraform#fs-create-replica-terraform) or [remove a replica relationship](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=terraform#fs-remove-replication-terraform).
* [Change the name of a mount target](#file-storage-mount-target-update-terraform).
* [Delete a file share or a mount target](#delete-file-share-terraform).

### Update attributes of a file share with Terraform
{: #file-storage-share-update-terraform}

Update the `ibm_is_share` resource to change any attributes of the file share such as name, size, profile, tags. When applied, the following example updates the share's name to `new_name`.

```terraform
resource "ibm_is_share" "example" {
  name    = "new_name"
  size    = 300
  iops    = 5000
  profile = "dp2"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

### Update attributes of a mount target with Terraform
{: #file-storage-mount-target-update-terraform}

Update the `is_share_target` resource to change the name of the mount target. When applied, the following resource changes the name of the mount target to `my-new-share-target`.

```terraform
resource "is_share_target" "is_share_target" {
  share = is_share.is_share.id
  subnet = ibm_is_subnet.example.id
  name = "my-new-share-target"
}`
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share_target](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share_target){: external}.

### Delete a file share or a mount target with Terraform
{: #delete-file-share-terraform}

Use the `terraform destroy` command to conveniently destroy a remote object such as a file share. The following example shows the syntax for deleting a share. Substitute the actual ID of the share in for `ibm_is_share.example.id`. To delete a mount target, use the ID of the mount target.

```terraform
terraform destroy --target ibm_is_share.example.id
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

## Mount and unmount file shares from a virtual server instance
{: #fs-mount-unmount-vsi}

To mount a file share to a virtual server instance, [locate the mount path information](/docs/vpc?topic=vpc-file-storage-view). The mount path is created when you create a mount target for the file share. See the following information for mounting on these Linux operating systems. Other Linux distributions follow similar procedures.

* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL).
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu).
