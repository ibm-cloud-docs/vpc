---

copyright:
  years: 2021, 2024
lastupdated: "2024-09-26"

keywords: file share, file storage, rename share, increase size, adjust IOPS, mount target

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing file shares, accessor share bindings, and mount targets
{: #file-storage-managing}

Manage the file shares that you created. You can rename a file share. You can increase its capacity and modify its IOPS. You can add mount targets to a file share, and use the mount path to mount a file share from virtual server instances. You can rename or delete a mount target. Or you can delete a file share if you no longer need it.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_short}} service requires IAM permissions for role-based access control. For example, to create a file share, you need to have at least editor permissions. For more information, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=ui) for file shares.
{: requirement}

## Differences in handling normal file shares and accessor shares
{: #file-storage-manage-shares-comparison}

The following sections contain instructions to modify and update various properties of file shares and mount targets. Due to the nature of the accessor shares, some properties (for example, file share profile, access control mode or allowed transit encryption modes) cannot be changed. Those properties are inherited from the origin share. For the accessor share properties that can be modified, the steps are the same as for updating a normal share.

## Managing file shares, accessor share bindings, and mount targets in the UI
{: #file-storage-manage-ui}
{: ui}

In the console, you can:

* [Rename a file share](#rename-file-share-ui).
* [Rename a mount target of a file share](#rename-mount-target-ui).
* [Update a file share profile](#fs-update-profile-ui) (not applicable for accessor shares).
* [Update allowed transit encryption modes](#fs-update-transit-encryption-ui) (not applicable for accessor shares).
* [Delete mount target of a file share](#delete-mount-target-ui).
* [Delete a file share](#delete-file-share-ui).
* [Add user tags to file shares](#fs-add-user-tags).

In the console, you can manage normal file shares and accessor shares. Only the share owner can modify properties like access control mode, IOPS, and profile. The accessor account cannot edit the origin share, and can modify a smaller set of properties of the accessor share.

### Renaming a file share in the UI
{: #rename-file-share-ui}

1. On the [file shares details](/docs/vpc?topic=vpc-file-storage-view#fs-view-single-share-ui) page, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") next to the file share name.

2. Provide a new name for the file share.

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. File share names must begin with a lowercase letter.

### Renaming a mount target of a file share in the UI
{: #rename-mount-target-ui}

1. Go to the [file shares details](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-view-single-share-ui) page.
2. Click the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions").
3. Select **Rename**.
4. Enter a new name and click **Rename**.

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

### Updating a file share profile in the UI
{: #fs-update-profile-ui}

These instructions are for the previous generation of file share profiles (general purpose, 5-iops, 10-iops, or custom). To access the latest features, you must change the IOPS profile of your share to dp2. The profiles of file shares that were created with the dp2 profile cannot be changed.
{: important}

You can change the profile for a file share from the current profile to another **IOPS tier** profile, to a **custom** profile, or to a high-performance **dp2** profile. Your billing adjusts based on the type of profile that you choose.

1. Go to the [file shares details](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-view-single-share-ui) page.
2. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") next to the current profile or use the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Edit IOPS profile**. A side panel shows the current profile, file share size, and maximum IOPS.
3. For a **New profile**, click the down arrow. You can select a new IOPS tier, a custom profile, or dp2. For **Custom IOPS** or **dp2**, specify a new max IOPS based on the file share size. The file share price is automatically calculated based on your selection.
4. Click **Save and continue**.

### Updating allowed transit encryption modes in the UI
{: #fs-update-transit-encryption-ui}

The owner of share can change the allowed transit encryption modes. However, before this property can be changed, all bindings must be deleted. Deleting the bindings severs the network path between origin file share and accessor share, and puts the mount target that is attached to the accessor share in a failed state. For more information, see [Removing access to a file share from other accounts](/docs/vpc?topic=vpc-file-storage-accessor-delete&interface=ui).
{: important}

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view).
1. On the File share details page, locate the allowed transit encryption modes.
1. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to change the current value.

### Deleting file shares, accessor share bindings, and mount targets in the UI
{: #delete-targets-shares-ui}

Before you delete a file share, make sure that it is [unmounted](#fs-mount-unmount-vsi) from all virtual server instances and that all mount targets that belong to the file share are [deleted](#delete-mount-target-ui). If your file share is shared with another account, delete the accessor bindings before you delete the share. Also, if the file share has a replica file share, you must remove the replication relationship. For more information, see [Remove the replication relationship in the UI](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=ui#fs-remove-replication-ui).

#### Deleting share bindings of a file share in the UI
{: #delete-bindings-ui}

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view#file-storage-view-shares-targets-ui).
1. On the File share details page, scroll to the Accessor share bindings section to locate the binding that you want to delete.
1. At the end of the row of the binding, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Delete**.

#### Deleting mount target of a file share in the UI
{: #delete-mount-target-ui}

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view#file-storage-view-shares-targets-ui).
2. On the File share details page, select a mount target that you want to delete.
3. Click the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Delete**.

#### Deleting a file share in the UI
{: #delete-file-share-ui}

The file share must be in a `stable` state or `failed` state.

1. Select a file share from the [list of file shares](/docs/vpc?topic=vpc-file-storage-view).
2. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") at the end of the row and select **Delete**.

## Managing file shares, accessor share bindings, and mount targets from the CLI
{: #file-storage-manage-cli}
{: cli}

By using the CLI, you can:

* [Rename a file share](#rename-file-share-cli).
* [Rename a mount target of a file share](#rename-mount-target-cli).
* [Update a file share profile](#fs-update-profile-cli) (not applicable for accessor share).
* [Update allowed transit encryption modes](#fs-update-transit-encryption-cli) (not applicable for accessor share).
* [Delete mount target of a file share](#delete-mount-target-cli).
* [Delete a file share](#delete-file-share-cli).
* [Add user tags to file shares](#fs-add-user-tags).

### Renaming a file share from the CLI
{: #rename-file-share-cli}

1. Locate the file share that you want to rename by listing the file shares in the region with the `ibmcloud is shares` command. Take a note of the file shares name and ID.

   ```sh
   $ ibmcloud is shares
   Listing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   ID                                          Name                                 Lifecycle state   Zone         Profile       Size(GB)   Resource group   Replication role
   r006-600f9bff-3c2e-4542-9fc3-ef3be15da04a   my-file-share-2                      stable            us-south-2   dp2           100        defaults         replica
   r006-52c68ba5-2754-4c9d-8345-1fe6aa930073   disposal-snare-revivable-chitchat    stable            us-south-3   dp2           10         defaults         replica
   r006-46541dc4-9e73-453a-9075-90ced0d612c3   trapdoor-urgency-attitude-imposing   stable            us-south-1   dp2           10         defaults         source
   r006-72604692-0dc5-49fb-8eca-08b16a6a4854   fiction-create-platter-decidable     stable            us-south-3   dp2           10         defaults         replica
   r006-c23ce229-fee9-4d40-a509-44886b21bb69   prismoid-evergreen-chains-granola    stable            us-south-1   dp2           10         defaults         source
   r006-89b34134-2be6-4281-ae8e-b1c625d533ae   my-test-share                        stable            us-south-1   dp2           10         defaults         none
   r006-cc7ab6a0-bb71-4e03-8ef7-dcffca43717f   my-old-file-share                    stable            us-south-1   tier-3iops    40         defaults         none
   ```
   {: screen}

1. Run the `ibmcloud is share-update` command and specify a new file share name with the `--name` options.

   ```sh
   ibmcloud is share-update r006-600f9bff-3c2e-4542-9fc3-ef3be15da04a --name my-renamed-share
   Updating file share r006-600f9bff-3c2e-4542-9fc3-ef3be15da04a under account Test Account as user test.user@ibm.com...

   ID                           r006-600f9bff-3c2e-4542-9fc3-ef3be15da04a
   Name                         my-renamed-share
   CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-600f9bff-3c2e-4542-9fc3-ef3be15da04a
   Lifecycle state              stable
   Access control mode          vpc
   Zone                         us-south-2
   Profile                      dp2
   Size(GB)                     100
   IOPS                         100
   Encryption                   user_managed
   Mount Targets                ID                          Name
                                No mounted targets found.

   Resource group               ID                                 Name
                                6edefe513d934fdd872e78ee6a8e73ef   defaults

   Created                      2023-08-01T17:02:01+00:00
   Encryption key               crn:v1:bluemix:public:kms:eu-de:a/a1234567:key:f602ae93-b915-49bc-a0e1-af29c73e7788
   Latest job                   Job status   Job status reasons
                                -            -

   Replication cron spec        00 11 * * 0
   Replication role             replica
   Replication status           none
   Replication status reasons   Status code   Status message
                                -             -

   Source share                 ID                                          Name   Resource type
                                r006-9272410d-38b2-447e-b98f-abc944ea02cc   dal1   share
   ```
   {: screen}

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. File share names must begin with a lowercase letter.
{: important}

For more information about the command options, see [`ibmcloud is share-update`](/docs/vpc?topic=vpc-vpc-reference#share-update).

### Renaming a mount target of a file share from the CLI
{: #rename-mount-target-cli}

1. List the file shares in the region with the `ibmcloud is shares` command. Take a note of the file share's name and ID that has the mount target that you want to rename.

1. Use the share's name or ID to find its mount targets with the `ibmcloud is share-mount-targets` command.
   ```sh
   $ ibmcloud is share-mount-targets r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a
   Listing share mount target of r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   ID                                          Name                     VPC      Lifecycle state   Transit Encryption
   r006-fdbffc45-618c-49f1-bb08-ec530d7be378   my-source-mount-target   my-vpc   stable            none
   ```
   {: screen}

1. To rename the mount target, run the `share-mount-target-update` command with the file share name or ID, and the mount target name. Specify a new mount target name with the `--name` option.
   ```sh
   ibmcloud is share-mount-target-update r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a r006-fdbffc45-618c-49f1-bb08-ec530d7be378 --name my-renamed-mount-target
   ```
   {: screen}

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

For more information about the command options, see [`ibmcloud is share-mount-target-update`](/docs/vpc?topic=vpc-vpc-reference#share-mount-target-update).

### Updating a file share profile in the CLI
{: #fs-update-profile-cli}

These instructions are for the previous generation of file share profiles (general purpose, 5-iops, 10-iops, or custom). To access the latest features, you must change the IOPS profile of your share to dp2. The profiles of file shares that were created with the dp2 profile cannot be changed.
{: important}

1. Locate the file share that you want to update by listing the file shares in the region with the `ibmcloud is shares` command.

   ```sh
   $ ibmcloud is shares
   Listing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   ID                                          Name                                 Lifecycle state   Zone         Profile       Size(GB)   Resource group   Replication role
   r006-52c68ba5-2754-4c9d-8345-1fe6aa930073   disposal-snare-revivable-chitchat    stable            us-south-3   dp2           10         defaults         replica
   r006-46541dc4-9e73-453a-9075-90ced0d612c3   trapdoor-urgency-attitude-imposing   stable            us-south-1   dp2           10         defaults         source
   r006-72604692-0dc5-49fb-8eca-08b16a6a4854   fiction-create-platter-decidable     stable            us-south-3   dp2           10         defaults         replica
   r006-c23ce229-fee9-4d40-a509-44886b21bb69   prismoid-evergreen-chains-granola    stable            us-south-1   dp2           10         defaults         source
   r006-89b34134-2be6-4281-ae8e-b1c625d533ae   test-share                           stable            us-south-1   dp2           10         defaults         none
   r006-cc7ab6a0-bb71-4e03-8ef7-dcffca43717f   my-old-file-share                    stable            us-south-1   tier-3iops    40         defaults         none
   ```
   {: screen}

1. Use the `share-update` command with the `--profile` parameter to indicate the new file share profile by name.

   ```sh
   $ ibmcloud is share-update my-old-file-share --profile dp2 --size 1000 --iops 3000
   Updating file share my-file-share-8 under account Test Account as user test.user@ibm.com...

   ID                           r006-cc7ab6a0-bb71-4e03-8ef7-dcffca43717f
   Name                         my-old-file-share
   CRN                          crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:r006-cc7ab6a0-bb71-4e03-8ef7-dcffca43717f
   Lifecycle state              updating
   Access control mode          vpc
   Zone                         us-south-1
   Profile                      dp2
   Size(GB)                     1000
   IOPS                         3000
   User Tags                    env:dev,env:prod
   Encryption                   provider_managed
   Mount Targets                ID                                          Name
                                r006-c9d82a15-7ead-4388-abc8-88e81c12ed28   my-target121

   Resource group               ID                                 Name
                                6edefe513d934fdd872e78ee6a8e73ef   defaults

   Created                      2023-03-27T20:43:36+00:00
   Replication role             none
   Replication status           none
   Replication status reasons   Status code   Status message
                                -             -
   ```
   {: screen}

For more information about the command options, see [`ibmcloud is share-update`](/docs/vpc?topic=vpc-vpc-reference#share-update).

### Deleting file shares, accessor share bindings, and mount targets from the CLI
{: #delete-share-targets-cli}

Before you delete a file share, make sure that it is [unmounted](#fs-mount-unmount-vsi) from all virtual server instances and that all mount targets that belong to the file share are [deleted](#delete-mount-target-cli).If your file share is shared with another account, delete the accessor bindings before you delete the share. Also, if the file share has a replica file share, you must remove the replication relationship. For more information, see [Remove the replication relationship from the CLI](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=cli#fs-remove-replication-cli).

1. Locate the file share that you want to delete by listing all the file shares with the `ibmcloud is shares` command.

   ```sh
   ibmcloud is sharesListing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   ID                                          Name                    Lifecycle state   Zone         Profile   Size(GB)   Resource group   Replication role
   r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   my-replica-file-share   stable            us-south-1   dp2       1500       Default          replica
   r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   my-source-file-share    stable            us-south-2   dp2       1500       Default          source
   r006-6d1719da-f790-45cc-9f68-896fd5673a1a   my-replica-share        stable            us-south-3   dp2       1000       Default          replica
   r006-925214bc-ded5-4626-9d8e-bc4e2e579232   my-new-file-share       stable            us-south-2   dp2       500        Default          none
   r006-97733317-35c3-4726-9c28-1159de30012e   my-file-share-8         stable            us-south-1   dp2       40         Default          none
   r006-b1707390-3825-41eb-a5bb-1161f77f8a58   my-vpc-file-share       stable            us-south-2   dp2       1000       Default          none
   r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   my-file-share           stable            us-south-2   dp2       1000       Default          source
   ```
   {: screen}

1. Retrieve the file share details to see the attached mount target and replication information with the `ibmcloud is share` command.

   ```sh
   $ ibmcloud is share my-file-share-8
   Getting file share my-file-share-8 under account Test Account as user test.user@ibm.com...

   ID                           r006-97733317-35c3-4726-9c28-1159de30012e
   Name                         my-file-share-8
   CRN                          crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:r006-97733317-35c3-4726-9c28-1159de30012e
   Lifecycle state              stable
   Access control mode          vpc
   Zone                         us-south-1
   Profile                      dp2
   Size(GB)                     40
   IOPS                         2000
   User Tags                    env:dev,env:prod
   Encryption                   provider_managed
   Mount Targets                ID                                          Name
                                r006-36d67ada-ca83-44be-adad-dc58e7c38dc5   my-new-mount-target

   Resource group               ID                                 Name
                                db8e8d865a83e0aae03f25a492c5b39e   Default

   Created                      2023-10-18T23:52:45+00:00
   Replication role             none
   Replication status           none
   Replication status reasons   Status code   Status message
                                -             -
   ```
   {: screen}



#### Deleting share bindings of a file share from the CLI
{: #delete-bindings-cli}

Run the `share-binding-delete` command and specify the origin file share and binding by either their names or IDs. Type `y` when you're prompted. If more than one bindings are attached to the file share, repeat this step until all bindings are deleted. See the following example.

```sh
$ ibmcloud is share-bindings-delete my-origin-share r-006-c2e53b1b-3b15-4792-8d96-c9c035fd65c3
This will delete accessor binding r-006-c2e53b1b-3b15-4792-8d96-c9c035fd65c3 for share ID my-origin-share and cannot be undone. Continue [y/N] ?> y
Deleting binding r-006-c2e53b1b-3b15-4792-8d96-c9c035fd65c3 for share ID my-origin-share under account Test Account as user test.user@ibm.com...
OK
Binding r-006-c2e53b1b-3b15-4792-8d96-c9c035fd65c3 is deleted.
```
{: screen}


#### Deleting a mount target of a file share from the CLI
{: #delete-mount-target-cli}

Run the `share-mount-target-delete` command and specify the file share and mount target by either their names or IDs. Type `y` when you're prompted. If more than one mount targets are attached to the file share, repeat this step until all mount targets are deleted.

```sh
$ ibmcloud is share-mount-target-delete my-file-share-8 my-new-mount-target
This will delete mounted target my-new-mount-target for share ID my-file-share-8 and cannot be undone. Continue [y/N] ?> y
Deleting mounted target my-new-mount-target for share ID my-file-share-8 under account Test Account as user test.user@ibm.com...
OK
Share mount target my-new-mount-target is deleted.
```
{: pre}

For more information about the command options, see [`ibmcloud is share-mount-target-delete`](/docs/vpc?topic=vpc-vpc-reference#share-mount-target-delete).

#### Deleting a file share from the CLI
{: #delete-file-share-cli}

The file share must be in a `stable` or `failed` state. To delete the file share, run the `share_delete` command and specify the file share by its name or ID.

```sh
$ ibmcloud is share-delete my-file-share-8
This will delete file share my-file-share-8 and cannot be undone. Continue [y/N] ?> y
Deleting file share my-file-share-8 under account Test Account as user test.user@ibm.com...
OK
File share my-file-share-8 is deleted.
```
{: screen}

For more information about the command options, see [`ibmcloud is share-delete`](/docs/vpc?topic=vpc-vpc-reference#share-delete).



### Update allowed transit encryption modes from the CLI
{: #fs-update-transit-encryption-cli}

The owner of share can change the allowed transit encryption modes type to `user_managed,none`,`user_managed` or `none`. However, before this property can be changed, all bindings and [mount targets must be deleted](#delete-mount-target-cli). Deleting the bindings severs the network path between origin file share and accessor share, and puts the mount target that is attached to the accessor share in a failed state. For more information, see [Removing access to a file share from other accounts](/docs/vpc?topic=vpc-file-storage-accessor-delete&interface=cli).
{: important}

Example command to update the encryption policy TBD.

```sh
$ ibmcloud is share-update my-origin-share --allowed-transit-encryption-modes user_managed
Updating file share my-file-share under account Test Account as user test.user@ibm.com...

ID                               r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
Name                             my-origin-share
CRN                              crn:v1:bluemix:public:is:us-south-2:a/1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
Lifecycle state                  stable
Access control mode              security_group
Accessor binding role            origin
Allowed transit encryption modes user_managed
Origin share                     CRN                                                                                             Name            Remote account  Remote region
                                 crn:v1:bluemix:public:is:us-south-2:a/7654321::share:r006-d73v40a6-e08f-4d07-99e1-d28cbf2188ed  my-origin-share a7654321        -
Zone                             us-south-2
Profile                          dp2
Size(GB)                         1000
IOPS                             1000
Encryption                       provider_managed
Mount Targets                    ID                          Name
                                 No mounted targets found.

Resource group                   ID                                 Name
                                 db8e8d865a83e0aae03f25a492c5b39e   Default

Created                          2024-06-25T22:15:15+00:00
Replication role                 none
Replication status               none
Replication status reasons       Status code   Status message
                                 -             -
TBD
```
{: screen}


## Managing file shares, accessor share bindings, and mount targets with the API
{: #file-storage-manage-api}
{: api}

By using the API, you can:

* [Rename a file share](#rename-file-share-api).
* [Rename a mount target of a file share](#rename-mount-target-api).
* [Update a file share profile](#fs-update-profile-api) (not applicable for accessor shares).
* [Update allowed transit encryption modes with the API](#fs-update-transit-encryption-api) (not applicable for accessor shares).
* [Delete mount target of a file share](#delete-mount-target-api).
* [Delete a file share](#delete-file-share-api).
* [Manage user tags for file shares](#fs-add-user-tags).

To see information about the {{site.data.keyword.filestorage_vpc_short}} API methods, see the following section in the [API reference](/apidocs/vpc/latest#list-share-profiles).

### Renaming a file share with the API
{: #rename-file-share-api}

Make a `PATCH /shares/$share_id` call to rename a specific file share. See the following example.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id?version=2023-08-08&generation=2"\
  -H "Authorization: Bearer ${API_TOKEN}"\
  -d '{"name": "share-renamed1"}'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-07-17T23:31:59Z",
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

Valid file share names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. File share names must begin with a lowercase letter.

### Renaming a mount target of a file share with the API
{: #rename-mount-target-api}

Make a `PATCH /shares/$share_id/mount_targets/$target_id` call to rename a mount target of a file share. See the following example.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets/$target_id?version=2023-08-08&generation=2"\
  -H "Authorization: Bearer ${API_TOKEN}" \
  -d '{"name": "target-renamed1"}
```
{: pre}

A successful response looks like the following example.

```json
{
  "access_control_mode": "vpc",
  "created_at": "2023-07-18T23:31:59Z",
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

Valid mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Mount target names must begin with a lowercase letter.

### Updating a file share profile with the API
{: #fs-update-profile-api}

These instructions are for the previous generation of file share profiles (general purpose, 5-iops, 10-iops, or custom). To access the latest features, you must change the IOPS profile of your share to dp2. The profiles of file shares that were created with the dp2 profile cannot be changed.
{: important}

Make a `PATCH /shares/{share_ID}` call and specify the profile name in the `profile` property. The following example changes the profile to a `dp2` profile.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/shares/432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token"\
-d '{"profile": {"name": "dp2"}}'
```
{: codeblock}

### Updating allowed transit encryption modes with the API
{: #fs-update-transit-encryption-api}

The owner of share can change the allowed transit encryption modes. However, before this property can be changed, all bindings and must be deleted. Deleting the bindings severs the network path between origin file share and accessor share, and puts the mount target that is attached to the accessor share in a failed state. For more information, see [Removing access to a file share from other accounts](/docs/vpc?topic=vpc-file-storage-accessor-delete&interface=api).
{: important}

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id?version=2023-08-08&generation=2"\
  -H "Authorization: Bearer ${API_TOKEN}"
  -d '{"allowed_transit_encryption_modes": "user-managed"}'
```
{: codeblock}

### Deleting file shares, accessor share bindings, and mount targets with the API
{: #delete-share-targets-api}

Before you delete a file share, make sure that it is [unmounted](#fs-mount-unmount-vsi) from all virtual server instances and that all mount targets that belong to the file share are [deleted](#delete-mount-target-api). If your file share is shared with another account, delete the accessor bindings before you delete the share.Also, if the file share has a replica file share, you must remove the replication relationship. For more information, see [Remove the replication relationship with the API](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-remove-replication-api).

#### Deleting share bindings of a file share with the API
{: #delete-bindings-api}

You can programmatically delete a share binding by calling the `/shares` method in the [VPC API](/apidocs/vpc/latest){: external} as shown in the following sample request.
{: api}

```sh
curl -X DELETE "$vpc_api_endpoint/v1/shares/v1/shares/$share_id/bindings/$binding_id?version=2024-06-21&generation=2"
```
{: pre}

#### Deleting mount target of a file share with the API
{: #delete-mount-target-api}

Make a `DELETE /shares/{share_ID}/mount_targets/{target_id}` call to delete a mount target of a file share. The file share must be in a `stable` state. A file share cannot be deleted, if an existing mount target is associated with that file share or if replica operations are in progress.

See the following example.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets/$target_id?version=2023-08-08&generation=2"\
  -H "Authorization: Bearer ${API_TOKEN}"
```
{: pre}

A successful response has a confirmation of acceptance for deletion and a response that contains the target information. Status of mount target shows _deleting_ while the deletion is underway.

The following example is the response to a delete request of a mount target where `access_control_mode` is `security_group`. The response shows the security group and subnet. You can also see the specifics of the reserved IP address that was used for the virtual network interface of the mount target in the `primary_ip` section. By default the virtual network interface is deleted along with the mount target when the mount target is deleted.

```json
{
  "access_control_mode": "security_group",
  "created_at": "2022-08-08T01:59:46.000Z",
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
      "href": "https://us-south.iaas.cloud.ibm.com/v1/security_groups/r006-1dfeccef-3ad6-4760-8653-a202bc795db4",
      "id": "r006-1dfeccef-3ad6-4760-8653-a202bc795db4",
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

The mount target is deleted in the background. Confirm the deletion by trying to view the mount target information. If you get an `404 Not Found` error, the mount target is successfully deleted.

#### Deleting a file share in the API
{: #delete-file-share-api}

The file share must be in a `stable` or `failed` state.

Make a `DELETE /shares/$share_id` call to delete a file share. The file share must be in a `stable` state or `failed` state (that is, when provisioning fails). A file share cannot be deleted, if an existing mount target is associated with that file share or if replica operations are in progress.

See the following example.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id?version=2023-08-08&generation=2"\
  -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

A successful response confirms acceptance for deletion and shows file share information. The status of file share is updated to _pending_deletion_.

Example:

```json
{
  "access_control_mode": "vpc",
  "created_at": "2022-08-08T23:31:59Z",
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

The file share is deleted in the background. Confirm the deletion by trying to view the mount target information. If you get a `404 Not Found` error, the mount target is successfully deleted.

A `DELETE /shares/$share_id` call can optionally include an `If-Match` header that specifies an `ETag` hash string. Make a `GET /shares/{share_id}` call and copy the `ETag` hash string from the response header. For more information, see [User tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-about-user-tags).
{: note}

## Managing file shares, accessor share bindings, and mount targets with Terraform
{: #file-storage-manage-terraform}
{: terraform}

With Terraform, you can:

* [Change file share attributes such as name, size, profile, tags](#file-storage-share-update-terraform),
* [Add](/docs/vpc?topic=vpc-file-storage-create-replication&interface=terraform#fs-create-replica-terraform) or [remove a replica relationship](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=terraform#fs-remove-replication-terraform).
* [Change the name of a mount target](#file-storage-mount-target-update-terraform).
* [Delete a file share or a mount target](#delete-file-share-terraform).

### Updating attributes of a file share with Terraform
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

Valid file share and mount target names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. File share names must begin with a lowercase letter.
{: important}

Some attributes, such as profile, mount target access mode, allowed transit encryption modes, and encryption at rest, are not editable for accessor shares.

The owner of share can change the allowed transit encryption modes type to `user_managed`, `none`, or `user_managed,none`. However, before this property can be changed, all bindings and [mount targets must be deleted](#delete-file-share-terraform). Deleting the bindings severs the network path between origin file share and accessor share, and puts the mount target that is attached to the accessor share in a failed state. For more information, see [Removing access to a file share from other accounts](/docs/vpc?topic=vpc-file-storage-accessor-delete&interface=terraform).
{: important}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

### Updating attributes of a mount target with Terraform
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

### Deleting file shares, accessor share bindings, or mount targets with Terraform
{: #delete-file-share-terraform}

Before you delete a file share, make sure that it is [unmounted](#fs-mount-unmount-vsi) from all virtual server instances and that all mount targets that belong to the file share are deleted.If your file share is shared with another account, delete the accessor bindings before you delete the share. Also, if the file share has a replica file share, you must remove the replication relationship. For more information, see [Remove the replication relationship with Terraform](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=terraform#fs-remove-replication-terraform).

Use the `terraform destroy` command to conveniently destroy a remote object such as a file share. The following example shows the syntax for deleting a share. Substitute the actual ID of the share in for `ibm_is_share.example.id`. To delete a mount target or a share bindings, use their IDs with the same command.

```terraform
terraform destroy --target ibm_is_share.example.id
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

## Adding user tags to a file share
{: #fs-add-user-tags}

You can add user tags to new or existing file shares, modify, and delete tags for a file share with the UI, CLI, API, and Terraform. You can view tags throughout your account by filtering by tags from your resource list. You can also add user tags to replica file shares.

Up to 100 tags can be attached or detached in the same operation. When you edit your tags, the new tags overwrite the existing tags. To keep tags manageable, create only as many user tags as you require to effectively handle the resource.
{: tip}

You can manage your tags in the {{site.data.keyword.cloud_notm}} with the [Global Tagging API](/apidocs/tagging). With this API, you can create, delete, search, attach, or detach tags. For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag).

### Adding user tags to file share in the UI
{: #fs-add-tags-shares-ui}
{: ui}

You can add user tags to a file share in the UI.

1. Go to the list of file shares. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File Shares**.
2. Select a file share to view its details.
3. On the file share details page, user tags appear next to the file share name. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit tags.
4. In the **Edit tags** window, type a tag in the User tags text box.
5. Click **Save**.

### Adding or modify file share user tags from the CLI
{: #fs-add-tags-cli}
{: cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

You can add tags when you provision a file share with the `ibmcloud is share-create` command. The `--user-tags` option specifies tags for the file share. For more information, see [Creating a file share with a mount target from the CLI](/docs/vpc?topic=vpc-file-storage-create&interface=cli#fs-create-share-target-cli).

You can add and remove tags when you update a file share with the `ibmcloud is share-update` command.

1. Locate your share from the CLI by listing your file shares in the region with the `ibmcloud is shares` command.

   ```sh
   $ ibmcloud is shares
   Listing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   ID                                          Name                    Lifecycle state   Zone         Profile   Size(GB)   Resource group   Replication role
   r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   my-replica-file-share   stable            us-south-1   dp2       1500       Default          replica
   r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   my-source-file-share    stable            us-south-2   dp2       1500       Default          source
   r006-6d1719da-f790-45cc-9f68-896fd5673a1a   my-replica-share        stable            us-south-3   dp2       1500       Default          replica
   r006-925214bc-ded5-4626-9d8e-bc4e2e579232   my-new-file-share       stable            us-south-2   dp2       500        Default          none
   r006-b1707390-3825-41eb-a5bb-1161f77f8a58   my-vpc-file-share       stable            us-south-2   dp2       1000       Default          none
   r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   my-file-share           stable            us-south-2   dp2       1500       Default          source
   ```
   {: screen}

1. Retrieve the details of the file share with the `ibmcloud is share` command.

   ```sh
   $ ibmcloud is share my-file-share
   Getting file share my-file-share under account Test Account as user test.user@ibm.com...

   ID                           r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
   Name                         my-file-share
   CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
   Lifecycle state              stable
   Access control mode          security_group
   Zone                         us-south-2
   Profile                      dp2
   Size(GB)                     1000
   IOPS                         1000
   Encryption                   provider_managed
   Mount Targets                ID                                          Name
                                r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   my-cli-share-mount-target-1

   Resource group               ID                                 Name
                                db8e8d865a83e0aae03f25a492c5b39e   Default

   Created                      2023-10-18T22:15:15+00:00
   Replication role             none
   Replication status           none
   Replication status reasons   Status code   Status message
                                -             -
   ```
   {: screen}

1. Use the `ibmcloud is share-update` command with the `--user-tags` option to add a tag to the file share. If the file share had tags previously, they are overwritten by the tags that are specified in the command.

   ```sh
   ibmcloud is share-update my-file-share --user-tags env:dev
   Updating file share my-file-share under account Test Account as user test.user@ibm.com...

   ID                           r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
   Name                         my-file-share
   CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6
   Lifecycle state              stable
   Access control mode          security_group
   Zone                         us-south-2
   Profile                      dp2
   Size(GB)                     1500
   IOPS                         2000
   User Tags                    env:dev
   Encryption                   provider_managed
   Mount Targets                ID                                          Name
                                r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   my-cli-share-mount-target-1

   Resource group               ID                                 Name
                                db8e8d865a83e0aae03f25a492c5b39e   Default

   Created                      2023-10-18T22:15:15+00:00
   Replication role             none
   Replication status           none
   Replication status reasons   Status code   Status message
                                -             -
   ```
   {: screen}

### Adding or modify file share user tags with the API
{: #fs-add-tags-api}
{: api}

When you're working with the API, you must provide the `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc/latest#api-generation-parameter).
{: requirement}

#### Adding a user tag when a file share is created
{: #fs-add-tags-new-share-api}

Make a `POST /shares` request and specify the `user_tags` property. This example creates a share with three user tags, `env:test1`, `env:test2`, and `env:prod`.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2"\
    -H "Authorization: Bearer $iam_token"\
    -H 'Content-Type: application/json'\
    -d '{
        "name": "share-name1",
        "size": 2300,
        "iops": 6000,
        "profile": {"name": "dp2"},
        "user_tags": [
           "env:test1",
           "env:test2",
           "env:prod"
        ],
        "zone": {"name": "us-south-1"}
      }'
```
{: codeblock}

#### Modifying user tags for an existing file share
{: #fs-add-tags-share-api}

Add new user tags to an existing file share by making a `PATCH /shares` request and specify the user tags in the `user_tags` property. If the file share did not have any tags, the new tags in the request are added. If the file share had tags previously, the new tags overwrite the previous tags.

The following example modifies a file share that is identified by ID by renaming the share and adding user tags.

```sh
curl -X PATCH\
"$vpc_api_endpoint/v1/shares/432f1a4d-4aac-4ba1-922c-76fdbcbeb1e3?version=2023-08-08&generation=2"\
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

#### Modifying file share user tags with ETag verification
{: #fs-modify-etag-api}

To ensure that updates to a file share are valid, you can obtain an `ETag` hash string value and specify it in the `If-Match` header in the `PATCH /shares/{share_id}` call. That confirms that no change happened to the share object between the last observed state and the state at the time of the `PATCH` call.

To modify existing user tags that are added to a file share, you first make a `GET /shares/{share_id}` call and copy the hash value from `ETag` property in the response header. Then, you send the `ETag` value by using the `If-Match` header in a `PATCH /shares/{share_id}` request. Specifying an `Etag` value ensures any updates to or deletions of a file share fail if the `If-Match` value does not match the file share's current `Etag`. Follow these steps:

1. Make a `GET /shares/{share_id}` call and copy the hash string from `ETag` property in the response header. Use need the hash string value for when you specify `If-Match` in the `PATCH /shares/{share_id}` request to modify user tags for the share in step 2.

   ```sh
   curl -sSL -D GET\ "https://us-south.cloud.ibm.com/v1/shares/{share_id}?version=2023-08-08&generation=2"\
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

2. Make a `PATCH /shares/{share_id}` request. Specify the `ETag` hash string for the `If-Match` property in the header. Specify the user tag in the `user_tags` property.

   This example updates the file share user tags to `env:test` and `env:prod`. The hash string value that you obtained from the `ETag` property (`W/xxxyyyzzz123`) is specified in the `If-Match` header in the call.

   ```sh
   curl -X PATCH\
   "$vpc_api_endpoint/v1/shares/50fda9c3-eecd-4152-b473-a98018ccfb10?version=2023-08-08&generation=2"\
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

## Adding access management tags to a file share
{: #fs-add-access-mgt-tags}
{: ui}

Access management tags are metadata that you can add to your file shares to help organize access control resource relationships. You first create the tag and then add it to an existing file share or when you create a file share. You can apply the same access management tag to multiple file shares. You can assign access to the tag in {{site.data.keyword.iamshort}} (IAM). Optionally, you can create an IAM access group and manage users.

### Step 1 - Creating an access management tag in IAM
{: #fs-create-access-mgt-tag}

In the console:

1. Go to **Manage > Account**, and then select **Tags**.
2. Click the **Access management tags** tab. Add a tag name in the field. Access management tags require a `key:value` format.
3. Click **Create Tags**.

From the [Global Search and Tagging API](/docs/account?topic=account-tag&interface=api#create-access-api):

Make a `POST/ tags` call to [create an access management tag](/apidocs/tagging#create-tag) and specify the tag in the `tag_names` property. For an example, see [Creating access management tags with the API](/docs/account?topic=account-tag&interface=api#create-access-api).

### Step 2 - Adding an access management tag to a file share
{: #fs-add-access-mgt-tag}

Add an access management tag to an existing file share or when you [create a file share](/docs/vpc?topic=vpc-file-storage-create). For an existing file share:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, Go to the Resource list, and select a file share under **Storage** resources.
2. In the **Access management tags** field, type the name of an access management tag that you previously created. The tag appears as you type.
3. Save your changes.

### Step 3 - Creating an access group and assign access to users
{: #fs-access-mgt-additional-steps}

After you create an access management tag and apply it to a file share, complete the following steps to assign access and add users:

1. [Create an access group](/docs/account?topic=account-access-tags-tutorial#tagging-create-access-group). Access groups are assigned to policies that grant roles and permissions to the members of that group. You assign access to the specific access management tags for the file service. For more information about access groups, see [Setting up access groups](/docs/account?topic=account-groups&interface=ui).
2. [Assign an access policy to a group](/docs/account?topic=account-access-tags-tutorial#tagging-assign-policy).
3. [Add users to the access group](/docs/account?topic=account-access-tags-tutorial#tagging-add-users-access-group).

When you look at the specific resources for the VPC infrastructure and specify {{site.data.keyword.filestorage_vpc_short}} as the resource type, you can see the access management tags for the file service.

## Removing access to a file share from other accounts
{: #fs-xaccount-access-removal}

Revoking access to a file share is a 2-step process.

1. Remove the IAM authorization between the accounts. Removal of IAM authorization prevents the other account or service from creating an accessor share and mount target.
   - [Removing an authorization in the console](/docs/account?topic=account-serviceauth&interface=ui#remove-auth){: ui}[Removing an authorization by using the CLI](/docs/account?topic=account-serviceauth&interface=cli#remove-auth-cli){: cli}[Removing an authorization by using the API](/docs/account?topic=account-serviceauth&interface=api#remove-auth-api){: api}[Removing an authorization by using Terraform](/docs/account?topic=account-serviceauth&interface=terraform#remove-auth-tf){: terraform}

1. Delete the share binding that connects the origin share with the accessor share. This action causes the mount target of the other account to enter a `failed` state.
   - [Deleting share bindings from the UI](/docs/vpc?topic=vpc-file-storage-accessor-delete&interface=ui){: ui}[Deleting share bindings from the CLI](/docs/vpc?topic=vpc-file-storage-accessor-delete&interface=cli){: cli}[Deleting share bindings with the API](/docs/vpc?topic=vpc-file-storage-accessor-delete&interface=api){: api}[Deleting share bindings with Terraform](/docs/vpc?topic=vpc-file-storage-accessor-delete&interface=terraform){: terraform}

## Mounting and unmounting file shares on a virtual server instance
{: #fs-mount-unmount-vsi}

Mounting is a process by which a server's operating system makes files and directories on the storage device available for users to access through the server's file system. To mount a file share to a virtual server instance, [locate the mount path information](/docs/vpc?topic=vpc-file-storage-view). The mount path is created when you create a mount target for the file share. See the following information for mounting on a few Linux operating systems. Other Linux distributions follow similar procedures.

* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL).
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu).
* [Mounting file shares on z/OS](/docs/vpc?topic=vpc-file-storage-vpc-mount-zos)

## Monitoring file shares
{: #fs-manage-monitor}

[New]{: tag-new}

You can check the status and health states of your file share by using the console, the CLI, or the API. You can monitor the total throughput, the total IOPS, the number of mount targets, and capacity usage of your share over time in the {{site.data.keyword.cloud_notm}} console. You can use {{site.data.keyword.atracker_full}} to configure how to route auditing events for file shares. You can also configure {{site.data.keyword.logs_routing_full_notm}} for handling logs. For more information, see [Monitoring file share health states, lifecycle status, and events](/docs/vpc?topic=vpc-fs-vpc-monitoring).

File shares can be integrated with {{site.data.keyword.mon_full}} to gain operational visibility into the performance and health of your shares. In the Sysdig web UI you can view the throughput, IOPS, and capacity metrics in more detail, customize your dashboards, and set up alerts. For more information, see [Monitoring metrics for {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-fs-vpc-monitoring-sysdig).
