---

copyright:
  years: 2022, 2024
lastupdated: "2024-05-14"

keywords: file storage, file share, view share details, mount targets, view targets, view share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}
<!-- comment: linked help topic -->

# Viewing file shares and mount targets
{: #file-storage-view}

View all file shares and mount targets in the UI, CLI, API, or Terraform. View details of a single file share or mount target.
{: shortdesc}

## View file shares and mount targets in the UI
{: #file-storage-view-shares-targets-ui}
{: ui}

### View all file shares in the UI
{: #fs-view-all-shares-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File Shares**.

2. The File Shares for VPC list page shows all file shares that are created in that zone. **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") options are used to manage the file shares. The following table describes the information and actions on the list page.

| Field | Value |
|-------|-------|
| Region | Account region for the file share. Select a different region to see file shares for zones in that region. |
| Name  | The file share name. It can be the original file share or a replica file share. Click the name to see details about that file share. |
| Status | For a list of statuses for file shares, see [File share lifecycle states](/docs/vpc?topic=vpc-file-storage-managing#file-storage-vpc-status). |
| Mount targets | Number of mount targets that are associated with the file share. You can have one mount target per VPC per file share. |
| Size | Size of the file share, in GBs. |
| IOPS profile| It shows the performance profile that is associated with the file share.|
| Replication role | Relationship to the source file share. "Replica of" indicates that the file share a replica of the source share, which is linked. "Source of" indicates that the share the source of the replica, which is linked. "None" indicates that the file share does not replicate with another share. |
| Encryption type | It shows the encryption type of the file share, either provider-managed or customer-managed. [Customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption) uses your own root keys to protect your data. The UI also identifies the key management service (KMS), either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. |
| Actions menu| Options for managing the file share, depending on its state. For a file share in a _stable_ state, you can rename the share, create a replica, or delete a file share. **Delete** and **Create replica** are disabled if you set up replication to a replica file share already. For more information, see [Creating replica file shares](/docs/vpc?topic=vpc-file-storage-create-replication&interface=ui). |
{: caption="Table 1. File shares list page." caption-side="bottom"}

### View details of a file share in the UI
{: #fs-view-single-share-ui}

1. Go to the list of all file shares. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File Shares**.

2. Click the name of a file share to see the details page.

The editable name and status of the file share is shown. If you applied user or access management tags to the file share, they are listed. Click **Add tags** to apply new [tags](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-about-fs-tags) to the share.

The following table describes the information on the file share details page.

| Field | Value |
|-------|-------|
| **File share details** | |
| Name  | The file share name. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to change the name. |
| Zone | Zone for the file share (for example, Dallas 2). |
| Max IOPS | Maximum IOPS for the IOPS tier, custom, or dp2 [profile](/docs/vpc?topic=vpc-file-storage-profiles) associated with the file share. |
| Resource group | Resource groups associated with the file share in your account. |
| Replication role | Source file share or replica. |
| Encryption | Specifies provider-managed or [customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption). |
| Encryption instance | For customer-managed encryption, link to the {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance. |
| Key ID |  Copiable customer root key ID. |
| ID | For customer-managed encryption, the UUID generated when you created the file share. |
| Size | File share size in GB. |
| Created | Date the file share was created. |
| Mount target access mode   | Access to the file share is granted by either a security group within a subnet or to any virtual server instance in the VPC. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to switch access modes. Security group access is available only to file shares created with the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#dp2-profile). For more information, see the [Mount target access modes](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=api#fs-mount-access-mode). |
| **Profile, size, and IOPS**| |
| Size | File share size in GB. |
| IOPS tier | IOPS [profile](/docs/vpc?topic=vpc-file-storage-profiles) that defines the file share performance. In most cases, the dp2 profile is shown. |
| Max IOPS | Maximum IOPS for the specified profile. |
| **Mount targets** | Number of mount targets associated with the file share. You can have one mount target per VPC per file share. You can create more mount targets for other VPCs. Click ![Actions icon](../icons/action-menu-icon.svg) to rename or delete the mount target, or to view the mount path. |
| Name | Name of the mount target. |
| Status | Status of the mount target on the VPC. |
| Virtual private cloud | This is shown if the file share has VPC access mode. Click the name to go to the details page for that VPC, where you can see a [list of file shares](#fs-view-shares-vpc) that have a mount target in that VPC. |
| Subnet | This is shown if the file share has Security group access mode. Click the name of the subnet to see its details.|
| Security group | This is shown if the file share has Security group access mode. It's the number of security groups that the share is a member of. |
| Reserved IP | This is shown if the file share has Security group access mode. The IP address of the virtual network interface that is attached to the mount target |
| Encryption in Transit |This is shown if the file share has Security group access mode. Can be enabled or disabled. |
| **File share replication relationship** | Shows the name, location, and status of the source and the replica file shares \n * If no replica file shares were created, click **Create replica** to [create one](/docs/vpc?topic=vpc-file-storage-create-replication). \n * To break the replication relationship, click **Remove replication relationship**. Then, the replica file share becomes an independent read/write file share.|
| Replication frequency | Hover over the information icon to see an explanation of the cron replication schedule. |
| Status | Replication status; for example, _suspended_ or _available_. |
| Last sync start time | The date and time of the last replication start. |
| Last sync completion time | The date and time of the last replication ended. |
| Transfer rate | It shows the speed at which data was copied from the source file share to its replica during the last sync. |
| Transfer amount | The amount of data that is copied from the source file share to its replica during the last sync. |
| Replication role | Source or replica file share. |
| File share Name | Click the file share name to see its details. |
| Location | It displays the zone information of the file share. |
| Status   | It displays the lifecycle status of the file share. The status `Stable` is expected.|
{: caption="Table 2. File shares details page" caption-side="bottom"}

### View all file shares for a VPC in the UI
{: #fs-view-shares-vpc}

You can see all file shares that have a mount target to a VPC by viewing the VPC details page.

1. Go to a VPC:

    1. From the [file shares details page](#fs-view-single-share-ui), click the VPC link in the list of mount targets.
    2. From the UI, go to the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Network > VPCs**. Click the name of a VPC in the list.

2. On the VPC details page, scroll to **File shares in this VPC**.

### View mount target details in the UI
{: #fs-get-mountpath-ui-vpc}

1. Go to the list of all file shares. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File Shares**.
2. Click the name of a file share to see the details page. 
3. Scroll to the Mount targets section to see the list of mount targets. The list contains the names and statuses of the mount target, and the VPC that the mount target belongs to.
4. click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") to reveal the Actions menu. The Actions menu has 3 options: Rename, View path, and Delete. 
5. Click **View path** to see the mount path information that you can copy and paste in your mounting commands.

## Viewing file share and mount targets from the CLI
{: #file-storage-view-shares-targets-cli}
{: cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

### Viewing all file shares from the CLI
{: #fs-view-all-shares-cli}

You can list all your file shares in a region with the `ibmcloud is shares` command.

```sh
$ ibmcloud is shares
Listing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
ID                                          Name                    Lifecycle state   Zone         Profile   Size(GB)   Resource group   Replication role   
r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   my-replica-file-share   stable            us-south-1   dp2       1500       Default          replica   
r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   my-source-file-share    stable            us-south-2   dp2       1500       Default          source   
r006-6d1719da-f790-45cc-9f68-896fd5673a1a   my-replica-share        stable            us-south-3   dp2       1000       Default          replica   
r006-925214bc-ded5-4626-9d8e-bc4e2e579232   my-new-file-share       stable            us-south-2   dp2       500        Default          none   
r006-b1707390-3825-41eb-a5bb-1161f77f8a58   my-vpc-file-share       stable            us-south-2   dp2       1000       Default          none   
r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   my-file-share           stable            us-south-2   dp2       1000       Default          source   
```
{: screen}

For more information about the command options, see [`ibmcloud is shares`](/docs/vpc?topic=vpc-vpc-reference#shares-list).

### Viewing details of a file share from the CLI
{: #fs-share-details-cli}

To see the details of a file share, run the `ibmcloud is share` command and specify the file share by ID or name. 

The following example identifies the file share by ID. This share is a replica that is based on the `dp2` profile, and access to the share is granted by using security groups. The output provides information about the source file share and the replication details, too.

```sh
$ ibmcloud is share r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516
Getting file share r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516 under account IBM as user Viktoria.Muirhead@ibm.com...
                                
ID                           r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   
Name                         my-replica-file-share   
CRN                          crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   
Lifecycle state              stable   
Access control mode          security_group   
Zone                         us-south-1   
Profile                      dp2   
Size(GB)                     1500   
IOPS                         100   
Encryption                   provider_managed   
Mount Targets                ID                          Name      
                             No mounted targets found.      
                                
Resource group               ID                                 Name      
                             db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
Created                      2023-10-19T15:42:56+00:00   
Latest job                   Job status   Job status reasons      
                             succeeded    -      
                                
Replication cron spec        55 09 * * *   
Replication role             replica   
Replication status           active   
Replication status reasons   Status code   Status message      
                             -             -      
                                
Source share                 ID                                          Name                   Resource type      
                             r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   my-source-file-share   share           
```
{: screen}

You can use the name of the source share to retrieve its details. See the following example.

```sh
$ ibmcloud is share my-source-file-share
Getting file share my-source-file-share under account Test Account as user test.user@ibm.com...
                                
ID                           r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   
Name                         my-source-file-share   
CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567b::share:r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   
Lifecycle state              stable   
Access control mode          security_group   
Zone                         us-south-2   
Profile                      dp2   
Size(GB)                     1500   
IOPS                         2000   
User Tags                    env:dev   
Encryption                   provider_managed   
Mount Targets                ID                                          Name      
                             r006-fdbffc45-618c-49f1-bb08-ec530d7be378   my-source-mount-target      
                                
Resource group               ID                                 Name      
                             db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
Created                      2023-10-19T15:42:53+00:00   
Latest job                   Job status   Job status reasons      
                             succeeded    -      
                                
Replication share            ID                                          Name                    Resource type      
                             r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   my-replica-file-share   share      
                                
Replication role             source   
Replication status           active   
Replication status reasons   Status code   Status message      
                             -             -
```
{: screen}

For more information about the command options, see [`ibmcloud is share`](/docs/vpc?topic=vpc-vpc-reference#share-view).

### View mount targets for a file share from the CLI
{: #fs-view-mount-shares-cli}

To see all the mount targets that are created for a file share, run the `ibmcloud is share-mount-targets` command and specify the file share ID.

```sh
$ ibmcloud is share-mount-targets  r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a 
Listing share mount target of r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
ID                                          Name                     VPC      Lifecycle state   Transit Encryption   
r006-fdbffc45-618c-49f1-bb08-ec530d7be378   my-source-mount-target   my-vpc   stable            none   
```
{: screen}

For more information about the command options, see [`ibmcloud is share-mount-targets`](/docs/vpc?topic=vpc-vpc-reference#share-mount-targets-list).

### View mount target details from the CLI
{: #fs-get-mountpath-cli}

To view more detailed information about a mount target, run the `ibmcloud is share-mount-target` command and specify the share ID or name with the mount target name or ID. See the following example.

```sh
$ ibmcloud is share-mount-target  r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a my-source-mount-target
Getting mount target ID my-source-mount-target for share ID r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a under account Test Account as user test.user@ibm.com...
                               
ID                          r006-fdbffc45-618c-49f1-bb08-ec530d7be378   
Name                        my-source-mount-target   
VPC                         ID                                          Name      
                            r006-6e8fb140-5668-45b8-b98a-d5cb0e0bf39b   my-vpc      
                               
Access control mode         security_group   
Resource type               share_mount_target   
Virtual network interface   ID                                          Name      
                            r006-3b0c00fa-0ce3-4ff8-9a5d-c7a645fbe530   my-source-vni      
                               
Lifecycle state             stable   
Mount path                  10.240.64.6:/5975a795_e5e7_474c_82d3_46c1d4159c6a   
Transit Encryption          none   
Created                     2023-10-19T15:42:54+00:00   
```
{: screen}

For more information about the command options, see [`ibmcloud is share-mount-target`](/docs/vpc?topic=vpc-vpc-reference#share-mount-target-view).

## View file shares and mount targets with the API
{: #file-storage-view-shares-targets-api}
{: api}

You can programmatically view shares and mount targets by calling the `/shares` method in the [VPC API](/apidocs/vpc/latest#list-shares){: external} as shown in the following sample requests.

You must provide the `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc/latest#api-generation-parameter).
{: requirement}

### Viewing replication status and lifecycle_state with the API
{: #share-states-api}

- `lifecycle_state`
   - This property provides the current state of a resource through the [Retrieve a file share](/apidocs/vpc/latest#get-share) request. The values that `lifecycle_state` provides are generic and are meant to apply to various resources, not only file shares. `lifecycle_state` indicate whether the file share is stable, updating, deleting, suspended, and so on.
- `replication_status`
   - This property provides the current replication status of the file through the [Retrieve a file share](/apidocs/vpc/latest#get-share) request. The values that `replication_status` returns are specific for file shares. For more information, see the [Virtual Private Cloud API](/apidocs/vpc/latest) content.

### View all file shares with the API
{: #fs-view-all-shares-api}

Make a `GET /shares` request to list all file shares for a region.

```sh
curl -X GET "$vpc_api_endpoint/v1/shares?version=2023-07-18?limit=50&generation=2" -H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example. In the example, the `limit` query parameter specifies a limit of 50 file shares, all though there is only one in the response. The `access_control_mode` property value is `vpc`, which means that the file share can be mounted on all virtual server instances in a VPC.

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/shares?limit=50"
  },
  "limit": 50,
  "shares": [
    {
      "access_control_mode": "vpc",
      "created_at": "2023-07-18T13:02:17Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/51bba578-0dce-4f8a-aa6e-f06c899e2c8e",
      "id": "51bba578-0dce-4f8a-aa6e-f06c899e2c8e",
      "iops": 3000,
      "lifecycle_state": "stable",
      "name": "share-name1",
      "profile": {
        "family": "tiered",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/tier-10iops",
        "name": "tier-10iops",
        "resource_type": "share_profile"
      },
      "resource_group": {
        "crn": "crn:[...]",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/60fc731a-1794-4f5d-ba51-ea24b5357207",
        "id": "60fc731a-1794-4f5d-ba51-ea24b5357207",
        "name": "Default"
      },
      "resource_type": "share",
      "size": 40,
      "mount_targets": [
        {
          "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/51bba578-0dce-4f8a-aa6e-f06c899e2c8e/mount_targets/d5fd8173-f519-4ff7-8f63-0ead23ecf1f4",
          "id": "d5fd8173-f519-4ff7-8f63-0ead23ecf1f4",
          "name": "mount-target-name1",
          "resource_type": "share_target",
          "vpc": {
            "crn": "crn:[...]",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/c2d941de-27f5-432c-b4d0-37a8491c3216",
            "id": "c2d941de-27f5-432c-b4d0-37a8491c3216",
            "name": "vpc-name1",
            "resource_type": "vpc"
          }
        }
      ],
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
      }
    }
  ]
}
```
{: codeblock}

### View a single file share with the API
{: #fs-single-file-shares-api}

Make a `GET /shares/{share_id}` request to get details about a single file share.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/$share_id?version=2023-07-18&generation=2"\
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example. In this example, the share was created based on a `dp2` profile. The `access_ control_mode` property value is `security_group`, which means that access to the share is determined by the rules of a security group.

```json
{
  "access_control_mode": "security_group",
  "created_at": "2023-07-18T22:58:49.000Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63",
  "id": "199d78ec-b971-4a5c-a904-8f37ae710c63",
  "iops": 14400,
  "lifecycle_state": "stable",
  "mount_targets": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63/mount_targets/d5fd8173-f519-4ff7-8f63-0ead23ecf1f4",
      "id": "d5fd8173-f519-4ff7-8f63-0ead23ecf1f4",
      "name": "my-share-mount-target",
      "resource_type": "share_mount_target",
      "vpc": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/c2d941de-27f5-432c-b4d0-37a8491c3216",
        "id": "c2d941de-27f5-432c-b4d0-37a8491c3216",
        "name": "my-vpc",
        "resource_type": "vpc"
      }
    }
  ],
  "name": "my-share",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
    "family": "defined-performance",
    "name": "dp2",
    "resource_type": "share_profile"
  },
  "replication_role": "none",
  "replication_status": "none",
  "replication_status_reasons": [],
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada915d32640909956",
    "id": "678523bcbe2b4eada915d32640909956",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 4800,
  "user_tags": [],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1",
    "resource_type": "zone"
  }
}
```
{: codeblock}

### List all mount targets of a file share with the API
{: #fs-list-targets-api}

Make a `GET /shares/{share_id}/mount_targets` request to list all mount targets of a file share.

See the following example.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets?version=2023-07-18?limit=50&generation=2"\
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example:

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63/mount_targets?limit=50"
  },
  "limit": 50,
  "mount_targets": [
    {
      "access_control_mode": "security_group",
      "created_at": "2023-07-18T01:59:46.000Z",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63/mount_targets/r006-1b5571cb-536d-48d0-8452-81c05c6f7b80",
      "id": "r006-1b5571cb-536d-48d0-8452-81c05c6f7b80",
      "lifecycle_reasons": [],
      "lifecycle_state": "stable",
      "mount_path": "fsf-dal1099a-fz.adn.networklayer.com:/nxg_s_voll_mz0716_a4cc07a3_4425_4adf_aed6_0d7e142bee0c",
      "name": "my-target",
      "primary_ip": {
        "address": "192.0.2.0",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/35984145-9c3a-4626-8ee7-52c7a8742752/reserved_ips/0716-6fd4925d-7774-4e87-829e-7e5765d454ad",
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
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/35984145-9c3a-4626-8ee7-52c7a8742752",
        "id": "35984145-9c3a-4626-8ee7-52c7a8742752",
        "name": "my-subnet",
        "resource_type": "subnet"
      },
      "transit_encryption": "none",
      "virtual_network_interface": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/virtual_network_interfaces/388f01db-41bb-42aa-b5cd-34ba41288d47",
        "id": "388f01db-41bb-42aa-b5cd-34ba41288d47",
        "name": "my-virtual-network-interface",
        "resource_type": "virtual_network_interface"
      },
      "vpc": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/4c0bb0df-5ca2-43ca-a3de-a4f86010a906",
        "id": "4c0bb0df-5ca2-43ca-a3de-a4f86010a906",
        "name": "my-vpc",
        "resource_type": "vpc"
      }
    }
  ],
  "total_count": 1
}
```
{: codeblock}

### View a single mount target with the API
{: #fs-get-target-api}

Make a `GET /shares/{share_id}/mount_targets/{mount_target_id}` request to information of a single mount target of a file share. This call includes mount path information. Use the mount path to attach a file share to an instance.

See the following example

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets/$mount_target_id?version=2023-07-18&generation=2"\
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example. In this example, [data encryption in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit) is not enabled. The `transit_encryption` property value is `provider_managed`. 

```json
{
    "access_control_mode": "security_group",
    "created_at": "2023-07-18T01:59:46.000Z",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/shares//199d78ec-b971-4a5c-a904-8f37ae710c63/mount_targets/d5fd8173-f519-4ff7-8f63-0ead23ecf1f4",
    "id": "d5fd8173-f519-4ff7-8f63-0ead23ecf1f4",
    "lifecycle_reasons": [],
    "lifecycle_state": "stable",
    "mount_path": "fsf-dal1099a-fz.adn.networklayer.com:/nxg_s_vol_xyz_2891fd0a_64ea_4deb_9ed5_1159e37cb5aa",
    "name": "my-mount-target2",
    "primary_ip": {
      "address": "192.0.2.0",
      "href": "https://us-south.iaas.cloud.ibm.com/subnets/c2338e66-dcb5-4e9b-b572-108d47ca479a/reserved_ips/b96d456e-88f7-42a7-b02d-450a6d758534",
      "id": "b96d456e-88f7-42a7-b02d-450a6d758534",
      "name": "my-reserved-ip",
      "resource_type": "subnet_reserved_ip"
    },
    "resource_type": "share_mount_target",
    "security_groups": [
      {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/security_groups/b79365be-f626-45d4-94ae-83f16fa4edd3",
        "id": "b79365be-f626-45d4-94ae-83f16fa4edd3",
        "name": "my-security-group",
        "resource_type": "security_group"
      }
    ],
    "subnet": {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/c2338e66-dcb5-4e9b-b572-108d47ca479a",
      "id": "c2338e66-dcb5-4e9b-b572-108d47ca479a",
      "name": "my-subnet",
      "resource_type": "subnet"
    },
    "transit_encryption": "provider_managed",
    "virtual_network_interface": {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/virtual_network_interfaces/4551a68d-b45d-4443-b6b3-aba7a4a18c98",
      "id": "4551a68d-b45d-4443-b6b3-aba7a4a18c98",
      "name": "my-virtual-network-interface",
      "resource_type": "virtual_network_interface"
    },
    "vpc": {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/5821d0c4-a089-4957-b5fa-03b7ac636c15",
      "id": "5821d0c4-a089-4957-b5fa-03b7ac636c15",
      "name": "my-vpc",
      "resource_type": "vpc"
    }
  }
```
{: codeblock}

### View a source file share for a replica file share with the API
{: #fs-get-source-api}

Make a `GET /shares/{replica_id}/source` request and specify the replica share ID to retrieve the source file share details.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/$replica_id/source?version=2023-07-18&generation=2"\
-H "Authorization: $iam_token"\
```
{: pre}

A successful response provides details of the source file share. Notice that the replication role is `source`.

```json
{
    "access_control_mode": "security_group",
    "created_at": "2023-07-18T22:58:49.000Z",
    "crn": "crn:[...]",
    "encryption": "provider_managed",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/207721a9-aff9-4b16-9823-fe68096aeac3",
    "id": "207721a9-aff9-4b16-9823-fe68096aeac3",
    "iops": 14400,
    "lifecycle_state": "stable",
    "mount_targets": [
      {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/207721a9-aff9-4b16-9823-fe68096aeac3/mount_targets/ce244454-0919-45e2-b14b-f4285afcd856",
        "id": "ce244454-0919-45e2-b14b-f4285afcd856",
        "name": "my-share-mount-target",
        "resource_type": "share_mount_target",
        "vpc": {
          "crn": "crn:[...]",
          "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/c8b8fa2d-ccf7-4f42-9d38-df6d123c867d",
          "id": "c8b8fa2d-ccf7-4f42-9d38-df6d123c867d",
          "name": "my-vpc",
          "resource_type": "vpc"
        }
      }
    ],
    "name": "my-share-3",
    "profile": {
      "family": "defined-performance",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
      "name": "dp2",
      "resource_type": "share_profile"
    },
    "replication_role": "source",
    "replication_status": "active",
    "replication_status_reasons": [],
    "resource_group": {
      "crn": "crn:[...]",
      "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada915d32640909956",
      "id": "678523bcbe2b4eada915d32640909956",
      "name": "Default"
    },
    "resource_type": "share",
    "size": 4800,
    "user_tags": [],
    "zone": {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
      "name": "us-south-1",
      "resource_type": "zone"
    }
  }
```
{: codeblock}

## View file shares and mount targets with Terraform
{: #file-storage-view-shares-targets-terraform}
{: terraform}

You can use Terraform to view information about your file share and mount targets.

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

### View all file shares with Terraform
{: #fs-view-all-share-terraform}

Import the list of file shares that belong to an account as a read-only data source. You can filter by share name or resource group ID.

```terraform
data "ibm_is_shares" "example" {
}
```
{: codeblock}

The attributes that are exported include the total count of shares and the list of shares. The nested attributes include share ID, name, creation date, size, IOPS, CRN, access tags, encryption type and key, lifecycle state, replication role and status, mount target, and other attributes.

For more information, see [ibm_is_shares](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_shares){: external}.

### View file share information with Terraform
{: #fs-view-share-terraform}

Import the details of a file share as a read-only data source. You must identify the share by ID or name.

```terraform
data "ibm_is_share" "example" {
  share = ibm_is_share.example.id
}
```
{: codeblock}

```terraform
data "ibm_is_share" "example1" {
  name = ibm_is_share.example.name
}
```
{: codeblock}

The attributes that are exported include ID, name, creation date, size, IOPS, CRN, access tags, encryption type, encryption key, lifecycle state, replication role and status, mount target, and other attributes.

For more information, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_share){: external}.

### View mount targets that are associated to a file share with Terraform
{: #fs-view-mount-targets-terraform}

Import the list of mount targets that are associated with a file share as a read-only data source. Identify the file share by its ID.

```terraform
data "ibm_is_share_targets" "example" {
  share = ibm_is_share.example.id
}
```
{: codeblock}

The attributes that are exported include the list of mount targets and their IDs, names, creation dates, mount paths, subnet information, and so on.

For more information, see [ibm_is_share_targets](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_share_targets){: external}.

### View mount target information with Terraform
{: #fs-view-mount-target-terraform}

Import the details of a mount target as a read-only data source. Identify the mount target by specifying the share ID and the mount target ID. Both arguments are required.

```terraform
data "ibm_is_share_target" "example" {
  share        = ibm_is_share.example.id
  share_target = ibm_is_share_target.example.share_target
}
```
{: codeblock}

The attributes that are exported include ID, name, creation date, mount path, subnet information, and other attributes.

For more information, see [ibm_is_share_target](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_share_target){: external}.

## Next steps
{: #fs-view-next-steps}

Mount your file shares. Mounting is a process by which a server's operating system makes files and directories on the storage device available for users to access through the server's file system. For more information, see the following topics:
* [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility)
* [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL).
* [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
* [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu).
* [Mounting file shares on z/OS](/docs/vpc?topic=vpc-file-storage-vpc-mount-zos)

Manage your file shares and data.
* [Manage your file shares](/docs/vpc?topic=vpc-file-storage-managing). You can rename a file share. You can increase its capacity and modify its IOPS. You can add mount targets to a file share. You can rename or delete a mount target. You can delete a file share when you no longer need it.
* [Create a file share with replication](/docs/vpc?topic=vpc-file-storage-create-replication). With the replication feature, you can keep a read-only copy of your file share in another zone. The replica share is updated from the source share on a schedule that you specify. Replication provides a way to recover from an incident at the primary site, when data becomes inaccessible or an application fails. Replication can also be used for geographical expansion.
