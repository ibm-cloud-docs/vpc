---

copyright:
  years: 2026
lastupdated: "2026-05-06"

keywords: file share, file storage, access control mode, vpc access mode, security group, migration

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Migrating from VPC access control mode to security group access control mode
{: #fs-migrate-access-mode}

The VPC access control mode for zonal file shares is deprecated and scheduled to reach End of Support on 06 May 2027. If you have file shares that use the VPC access control mode, you must migrate them to security group access control mode before the End of Support date.
{: shortdesc}

The VPC access control mode allows all virtual server instances and bare metal servers in a VPC to access a file share. The security group access control mode provides more granular control by restricting access to specific compute resources based on security group rules. Security group access control mode also supports additional features such as encryption in transit, cross-zone mounting, and snapshots.

## Before you begin
{: #fs-migrate-access-mode-prereqs}

Before you migrate your file shares from VPC access control mode to security group access control mode, review the following requirements:

* Ensure that you have the necessary [IAM permissions](/docs/vpc?topic=vpc-iam-getting-started) to manage file shares and mount targets.
* Identify all file shares in your account that use VPC access control mode.
* Plan your security group configuration to ensure that authorized compute resources can access the file share after migration.
* Schedule a maintenance window for the migration, as the file share is temporarily unavailable during the process.

## Migration overview
{: #fs-migrate-access-mode-overview}

The migration process involves the following steps:

1. [Unmount the file share from all compute hosts](#fs-migrate-unmount)
2. [Delete all mount targets from the file share](#fs-migrate-delete-targets)
3. [Delete replica file shares (if applicable)](#fs-migrate-delete-replica)
4. [Update the access control mode to security group](#fs-migrate-update-mode)
5. [Create new mount targets with security group access](#fs-migrate-create-targets)
6. [Recreate replica file shares (if applicable)](#fs-migrate-recreate-replica)
7. [Mount the file share on compute hosts](#fs-migrate-remount)

## Step 1: Unmount the file share from all compute hosts
{: #fs-migrate-unmount}

Before you can delete the mount targets, you must unmount the file share from all virtual server instances and bare metal servers where it is currently mounted.

First, identify the file shares that use VPC access control mode. You can list all file shares, then filter the results to show only shares with `access_control_mode` set to `vpc` by using the CLI or the API.

If your environment supports JSON output, you can use [`jq`](https://jqlang.org/){: external} to filter the output of [`ibmcloud is shares`](/docs/vpc?topic=vpc-vpc-reference#shares-list):
{: cli}

```sh
ibmcloud is shares --output json | jq '.[] | select(.access_control_mode == "vpc") | {id, name, access_control_mode}'
```
{: pre}
{: cli}

Make a [`GET /shares`](/apidocs/vpc/latest#list-shares) request and pipe the JSON response to [`jq`](https://jqlang.org/){: external}:
{: api}

```sh
curl -s -X GET "$vpc_api_endpoint/v1/shares?version=2026-04-28&generation=2" \
  -H "Authorization: Bearer $iam_token" | jq '.shares[] | select(.access_control_mode == "vpc") | {id, name, access_control_mode}'
```
{: pre}
{: api}

After you identify the shares that use VPC access control mode, connect to each compute host where those shares are mounted, and run the `umount` command with the mount point name. For example:

```sh
umount /mnt/my-file-share
```
{: pre}

For more information about unmounting file shares on different operating systems, see [Mounting and unmounting file shares](/docs/vpc?topic=vpc-file-storage-managing#fs-mount-unmount-vsi).

## Step 2: Delete all mount targets from the file share
{: #fs-migrate-delete-targets}

After unmounting the file share from all compute hosts, delete all mount targets associated with the file share. You must delete all mount targets before you can update the access control mode.

### Deleting mount targets in the console
{: #fs-migrate-delete-targets-ui}
{: ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
2. Select the file share that you want to migrate from the list.
3. On the File share details page, locate the mount target that you want to delete in the Mount targets section.
4. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Delete**.
5. Repeat steps 3-4 for each mount target associated with the file share.

For more information, see [Deleting mount target of a file share in the console](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#delete-mount-target-ui).

### Deleting mount targets from the CLI
{: #fs-migrate-delete-targets-cli}
{: cli}

Run the `share-mount-target-delete` command for each mount target associated with the file share. You can use either the mount target name or ID.

```sh
ibmcloud is share-mount-target-delete SHARE_ID MOUNT_TARGET_ID
```
{: pre}

Where:
- `SHARE_ID` is the ID or name of the file share.
- `MOUNT_TARGET_ID` is the ID or name of the mount target to delete.

Example:

```sh
ibmcloud is share-mount-target-delete my-file-share my-mount-target
```
{: pre}

When prompted, type `y` to confirm the deletion. Repeat this command for each mount target associated with the file share.

For more information, see [Deleting a mount target of a file share from the CLI](/docs/vpc?topic=vpc-file-storage-managing&interface=cli#delete-mount-target-cli).

### Deleting mount targets with the API
{: #fs-migrate-delete-targets-api}
{: api}

Make a `DELETE /shares/{share_id}/mount_targets/{mount_target_id}` request to delete each mount target associated with the file share.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets/$mount_target_id?version=2026-04-28&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: pre}

Repeat this request for each mount target associated with the file share.

For more information, see [Deleting a mount target with the API](/docs/vpc?topic=vpc-file-storage-managing&interface=api#delete-mount-target-api).

### Deleting mount targets with Terraform
{: #fs-migrate-delete-targets-terraform}
{: terraform}

To delete a mount target with Terraform, remove the `ibm_is_share_mount_target` resource from your Terraform configuration file, then run `terraform apply`.

```terraform
# Remove or comment out the mount target resource
# resource "ibm_is_share_mount_target" "example" {
#   share = ibm_is_share.example.id
#   name  = "my-mount-target"
#   vpc   = ibm_is_vpc.example.id
# }
```
{: codeblock}
## Step 3: Delete replica file shares (if applicable)
{: #fs-migrate-delete-replica}

If your file share has a replica file share, you must delete the replica before you can update the access control mode of the source file share. The replication relationship must be removed first, which creates two independent file shares. Then, you can delete the replica file share and its mount targets.

If your file share does not have a replica, skip this step and proceed to [Step 4: Update the access control mode to security group](#fs-migrate-update-mode).
{: tip}

### Removing the replication relationship in the console
{: #fs-migrate-delete-replica-ui}
{: ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
2. Click the name of the source file share to go to its details page.
3. In the **File share replication relationship** section, click **Remove replication relationship**.
4. In the confirmation window, click **Unlink**. The replication relationship is removed, creating two independent file shares.
5. After the replication relationship is removed, delete the mount targets of the replica file share by following the same steps as in [Step 2: Delete all mount targets from the file share](#fs-migrate-delete-targets).
6. Return to the list of file shares, select the replica file share, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), and select **Delete**.
7. Confirm the deletion when prompted.

For more information, see [Removing the replication relationship in the console](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=ui#fs-remove-replication-ui) and [Deleting replica and source file shares](/docs/vpc?topic=vpc-file-storage-manage-replication#fs-delete-replicas).

### Removing the replication relationship from the CLI
{: #fs-migrate-delete-replica-cli}
{: cli}

1. List your file shares to identify the replica file share:

   ```sh
   ibmcloud is shares
   ```
   {: pre}

   Look for the file share with `replica` in the **Replication role** column.

2. Remove the replication relationship by running the `share-replica-split` command with the replica file share ID:

   ```sh
   ibmcloud is share-replica-split REPLICA_SHARE_ID
   ```
   {: pre}

   Where `REPLICA_SHARE_ID` is the ID or name of the replica file share.

   Example:

   ```sh
   ibmcloud is share-replica-split my-replica-file-share
   ```
   {: pre}

   When prompted, type `y` to confirm the split operation.

3. After the replication relationship is removed, delete the mount targets of the replica file share:

   ```sh
   ibmcloud is share-mount-target-delete REPLICA_SHARE_ID MOUNT_TARGET_ID
   ```
   {: pre}

   Repeat for each mount target associated with the replica file share.

4. Delete the replica file share:

   ```sh
   ibmcloud is share-delete REPLICA_SHARE_ID
   ```
   {: pre}

   When prompted, type `y` to confirm the deletion.

For more information, see [Removing the replication relationship from the CLI](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=cli#fs-remove-replication-cli).

### Removing the replication relationship with the API
{: #fs-migrate-delete-replica-api}
{: api}

1. Remove the replication relationship by making a `DELETE /shares/{replica_id}/source` request:

   ```sh
   curl -X DELETE \
   "$vpc_api_endpoint/v1/shares/$replica_share_id/source?version=2026-04-28&generation=2" \
   -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

   A successful response indicates that the replication relationship was removed.

2. Delete the mount targets of the replica file share by making a `DELETE /shares/{replica_id}/mount_targets/{mount_target_id}` request for each mount target:

   ```sh
   curl -X DELETE \
   "$vpc_api_endpoint/v1/shares/$replica_share_id/mount_targets/$mount_target_id?version=2026-04-28&generation=2" \
   -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

3. Delete the replica file share by making a `DELETE /shares/{replica_id}` request:

   ```sh
   curl -X DELETE \
   "$vpc_api_endpoint/v1/shares/$replica_share_id?version=2026-04-28&generation=2" \
   -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

For more information, see [Removing the replication relationship with the API](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-remove-replication-api).

### Removing the replication relationship with Terraform
{: #fs-migrate-delete-replica-terraform}
{: terraform}

1. Use the `ibm_is_share_replica_operations` resource to split the source and replica shares:

   ```terraform
   resource "ibm_is_share_replica_operations" "split" {
     share_replica = ibm_is_share.replica.id
     split_share   = true
   }
   ```
   {: codeblock}

2. After the split operation completes, remove the replica file share and its mount targets from your Terraform configuration, then run `terraform apply`:

   ```terraform
   # Remove or comment out the replica share and its mount targets
   # resource "ibm_is_share" "replica" {
   #   ...
   # }
   # resource "ibm_is_share_mount_target" "replica_target" {
   #   ...
   # }
   ```
   {: codeblock}

For more information, see [Removing the replication relationship with Terraform](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=terraform#fs-remove-replication-terraform).


For more information, see [Deleting a mount target with Terraform](/docs/vpc?topic=vpc-file-storage-managing&interface=terraform#delete-file-share-terraform).

## Step 4: Update the access control mode to security group
{: #fs-migrate-update-mode}

After deleting all mount targets and any replica file shares, update the file share's access control mode from `vpc` to `security_group`.

### Updating access control mode in the console
{: #fs-migrate-update-mode-ui}
{: ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
2. Select the file share that you want to migrate from the list.
3. On the File share details page, locate the **Access control mode** field.
4. Click the **Edit** icon !![Edit icon](../icons/edit-tagging.svg) next to the Access control mode field.
5. Select **Security group** from the dropdown menu.
6. Click **Save**.

### Updating access control mode from the CLI
{: #fs-migrate-update-mode-cli}
{: cli}

Run the `share-update` command with the `--access-control-mode` option set to `security_group`.

```sh
ibmcloud is share-update SHARE_ID --access-control-mode security_group
```
{: pre}

Where `SHARE_ID` is the ID or name of the file share.

Example:

```sh
ibmcloud is share-update my-file-share --access-control-mode security_group
```
{: pre}

### Updating access control mode with the API
{: #fs-migrate-update-mode-api}
{: api}

Make a `PATCH /shares/{share_id}` request with the `access_control_mode` property set to `security_group`.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id?version=2026-04-28&generation=2" \
-H "Authorization: Bearer $iam_token" \
-d '{
  "access_control_mode": "security_group"
}'
```
{: pre}

### Updating access control mode with Terraform
{: #fs-migrate-update-mode-terraform}
{: terraform}

Update the `access_control_mode` attribute in your `ibm_is_share` resource to `security_group`, then run `terraform apply`.

```terraform
resource "ibm_is_share" "example" {
  name                = "my-file-share"
  zone                = "us-south-2"
  profile             = "dp2"
  size                = 1000
  iops                = 500
  access_control_mode = "security_group"
}
```
{: codeblock}

## Step 5: Create new mount targets with security group access
{: #fs-migrate-create-targets}

After updating the access control mode, create new mount targets with security group access. The mount targets must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about) that is associated with a security group.

Before you create the mount targets, ensure that you have a security group configured with rules that allow inbound access for the TCP protocol on the NFS port (2049) from all compute hosts where you want to mount the file share.

### Creating mount targets in the console
{: #fs-migrate-create-targets-ui}
{: ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
2. Select the file share from the list.
3. On the File share details page, click **Create** in the Mount targets section.
4. Provide a name for the mount target.
5. Select the VPC where you want to create the mount target.
6. Configure the virtual network interface:
   - Provide a name for the virtual network interface.
   - Select the subnet where the mount target is created.
   - Optionally, specify a reserved IP address, or let the system assign one automatically.
   - Select the security group that controls access to the file share.
7. Optionally, enable encryption in transit if you want to protect data during transmission.
8. Click **Create**.

Repeat these steps to create mount targets for each VPC where you want to access the file share.

For more information, see [Creating a mount target in the console](/docs/vpc?topic=vpc-file-storage-create&interface=ui#fs-create-mount-target-ui).

### Creating mount targets from the CLI
{: #fs-migrate-create-targets-cli}
{: cli}

Run the `share-mount-target-create` command with the virtual network interface parameters.

```sh
ibmcloud is share-mount-target-create SHARE_ID \
  --name MOUNT_TARGET_NAME \
  --vpc VPC_ID \
  --vni-name VNI_NAME \
  --vni-subnet SUBNET_ID \
  --vni-sgs SECURITY_GROUP_ID
```
{: pre}

Where:
- `SHARE_ID` is the ID or name of the file share.
- `MOUNT_TARGET_NAME` is the name for the new mount target.
- `VPC_ID` is the ID or name of the VPC.
- `VNI_NAME` is the name for the virtual network interface.
- `SUBNET_ID` is the ID or name of the subnet.
- `SECURITY_GROUP_ID` is the ID or name of the security group.

Example:

```sh
ibmcloud is share-mount-target-create my-file-share \
  --name my-new-mount-target \
  --vpc my-vpc \
  --vni-name my-vni \
  --vni-subnet my-subnet \
  --vni-sgs my-security-group
```
{: pre}

For more information, see [Creating mount targets from the CLI](/docs/vpc?topic=vpc-file-storage-create&interface=cli#fs-create-mount-target-cli).

### Creating mount targets with the API
{: #fs-migrate-create-targets-api}
{: api}

Make a `POST /shares/{share_id}/mount_targets` request with the virtual network interface configuration.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets?version=2026-04-28&generation=2" \
-H "Authorization: Bearer $iam_token" \
-d '{
  "name": "my-new-mount-target",
  "virtual_network_interface": {
    "name": "my-vni",
    "subnet": {
      "id": "'"$subnet_id"'"
    },
    "security_groups": [
      {
        "id": "'"$security_group_id"'"
      }
    ]
  }
}'
```
{: pre}

For more information, see [Creating mount targets with the API](/docs/vpc?topic=vpc-file-storage-create&interface=api#fs-create-mount-target-api).

### Creating mount targets with Terraform
{: #fs-migrate-create-targets-terraform}
{: terraform}

Use the `ibm_is_share_mount_target` resource to create a mount target with a virtual network interface.

```terraform
resource "ibm_is_share_mount_target" "example" {
  share = ibm_is_share.example.id
  name  = "my-new-mount-target"

  virtual_network_interface {
    name = "my-vni"
    subnet = ibm_is_subnet.example.id
    security_groups = [ibm_is_security_group.example.id]
  }
}
```
{: codeblock}

For more information, see [Creating mount targets with Terraform](/docs/vpc?topic=vpc-file-storage-create&interface=terraform#fs-create-mount-target-terraform).

## Step 6: Recreate replica file shares (if applicable)
{: #fs-migrate-recreate-replica}

If you deleted a replica file share in Step 3, you can now recreate it with the updated security group access control mode. The new replica file share inherits the security group access control mode from the source file share.

If you did not have a replica file share, skip this step and proceed to [Step 7: Mount the file share on compute hosts](#fs-migrate-remount).
{: tip}

### Creating a replica file share in the console
{: #fs-migrate-recreate-replica-ui}
{: ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
2. Select the source file share from the list.
3. On the File share details page, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Create replica**.
4. On the File share replica create page, provide the following information:
   - **Name**: Provide a unique name for the replica share.
   - **Replica location**: Select the region and zone where you want to create the replica.
   - **Resource group**: Select the resource group for the replica.
   - **Profile**: The `dp2` profile is preselected. Specify the maximum IOPS value.
   - **Mount targets**: Optionally, create mount targets for the replica share with security group access.
   - **Sync frequency**: Specify how often to synchronize changes from the source to the replica (hourly, daily, weekly, monthly, or cron-spec).
   - **Encryption**: Configure encryption settings. For cross-region replication, you must select a customer root key if the source uses customer-managed encryption.
5. Click **Create file share**.

For more information, see [Adding replication to a file share in the console](/docs/vpc?topic=vpc-file-storage-create-replication&interface=ui#fs-create-replica-ui).

### Creating a replica file share from the CLI
{: #fs-migrate-recreate-replica-cli}
{: cli}

Use the `share-replica-create` command to create a replica for your source file share:

```sh
ibmcloud is share-replica-create \
  --source-share SOURCE_SHARE_ID \
  --name REPLICA_NAME \
  --zone REPLICA_ZONE \
  --profile dp2 \
  --replication-cron-spec "CRON_SPEC"
```
{: pre}

Where:
- `SOURCE_SHARE_ID` is the ID or name of the source file share.
- `REPLICA_NAME` is the name for the new replica share.
- `REPLICA_ZONE` is the zone where you want to create the replica.
- `CRON_SPEC` is the replication frequency in cron format (for example, `30 17 * * *` for daily at 5:30 PM).

Example:

```sh
ibmcloud is share-replica-create \
  --source-share my-file-share \
  --name my-replica-file-share \
  --zone us-south-1 \
  --profile dp2 \
  --replication-cron-spec "30 17 * * *"
```
{: pre}

For more information, see [Creating a replica for an existing file share from the CLI](/docs/vpc?topic=vpc-file-storage-create-replication&interface=cli#fs-create-share-replica-cli).

### Creating a replica file share with the API
{: #fs-migrate-recreate-replica-api}
{: api}

Make a `POST /shares` request to create a replica file share:

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2026-04-28&generation=2" \
-H "Authorization: Bearer $iam_token" \
-d '{
  "name": "my-replica-file-share",
  "profile": {
    "name": "dp2"
  },
  "zone": {
    "name": "us-south-1"
  },
  "replication_cron_spec": "30 17 * * *",
  "source_share": {
    "id": "'"$source_share_id"'"
  },
  "size": 1000,
  "iops": 500
}'
```
{: pre}

For more information, see [Creating a replica file share with the API](/docs/vpc?topic=vpc-file-storage-create-replication&interface=api#fs-create-replica-api).

### Creating a replica file share with Terraform
{: #fs-migrate-recreate-replica-terraform}
{: terraform}

Add an `ibm_is_share` resource for the replica to your Terraform configuration:

```terraform
resource "ibm_is_share" "replica" {
  name    = "my-replica-file-share"
  zone    = "us-south-1"
  profile = "dp2"
  size    = 1000
  iops    = 500

  source_share            = ibm_is_share.source.id
  replication_cron_spec   = "30 17 * * *"
}
```
{: codeblock}

Then run `terraform apply` to create the replica.

For more information, see [Creating replica file shares with Terraform](/docs/vpc?topic=vpc-file-storage-create-replication&interface=terraform#fs-create-replica-terraform).

## Step 7: Mount the file share on compute hosts
{: #fs-migrate-remount}

After creating the new mount targets with security group access, mount the file share on your compute hosts using the new mount path.

To mount a file share, you need the mount path information. You can retrieve the mount path from the mount target details in the console, CLI, or API.

For detailed instructions on mounting file shares on different operating systems, see the following topics:

* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-mount-RHEL)
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos)
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-mount-ubuntu)

For a complete list of mounting instructions for all supported operating systems, see [Mounting file shares](/docs/vpc?group=mounting-file-shares).

## Next steps
{: #fs-migrate-access-mode-next-steps}

After completing the migration, verify that:

* The file share is accessible from all authorized compute hosts.
* Security group rules are properly configured to allow access.
* Applications that use the file share are functioning correctly.
* The mount is persistent across reboots (check `/etc/fstab` entries).

For more information about managing file shares, see [Managing file shares and mount targets](/docs/vpc?topic=vpc-file-storage-managing).
