---

copyright:
  years: 2024, 2025
lastupdated: "2025-04-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Restoring data from a file share snapshot
{: #fs-snapshots-restore}

Restoring data from a snapshot creates a new, fully provisioned share. Shares can be restored from snapshots that were created manually or by a backup policy. You can create shares from snapshots in the console, from the CLI, with the API, or Terraform. The share that you create by using a snapshot must have the same file share profile as the snapshot. You can also restore single files from snapshots of your file share. 
{: shortdesc}

Shares can be created only in the same availability zone as the origin share of the snapshot. When the new share is created, it contains only pointers to the original share, and the data copy process begins. While the data is being copied, the share is in a _pending_ state. While the new share can be mounted for read/write in _pending_ state, a few operations like creating a replica or snapshots are not permitted. After the data-copy operation is complete, the new share is split from the parent share to become independent, completing the initialization process. After the initialization process is complete, the share moves to the _stable_ state and can be used as any other share. 

It is possible to change the customer-managed encryption key when the new share is created. This process registers the new share in the key management service with a new key. The new key is used to encrypt the new Share.

## Limitations
{: #fs-snapshots-restore-limitations}

The following limitations apply when you restore a share from a snapshot.

* To restore a share, the snapshot must be in a _stable_ and _available_ state.
* You can't delete the snapshot from which the share is restored from unless the initialization is complete.
* If snapshot is protected with customer-managed encryption and you don't specify a different root key CRN, the restored share is encrypted with the snapshot's encryption key. The encryption cannot be changed later. If the encryption key of the parent share is deleted while the new share is initialized, it disrupts the initialization process and causes it to fail.

## Restoring shares from snapshots in the console
{: #fs-snapshots-restore-ui}
{: ui}

You can create shares from various pages in the {{site.data.keyword.cloud_notm}} console.

### Creating a share from a snapshot in the console
{: #fs-snapshots-restore-snaphot-list-ui}

From the list of {{site.data.keyword.filestorage_vpc_short}} snapshots, you can create a {{site.data.keyword.filestorage_vpc_short}} share. The new shares are added to the [list of {{site.data.keyword.filestorage_vpc_short}} shares](/vpc-ext/storage/fileShares).

1. Go to the list of {{site.data.keyword.filestorage_vpc_short}} snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
1. Select the share that has the data that you want to use to create another share.
1. On the snapshots tab of the Share details page, locate the snapshot that you want to use. It must be in a `stable` state.
1. From the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Restore**.
1. In the provisioning page, the details of the snapshot are shown. Some file share configurations such as zone and encryption type are inherited from the snapshot and can't be changed.
1. Provide the required share details.

    | Field | Description |
    |-------|-------------|
    | **Share details** | Define the new share. |
    | Name | Enter a name for the new share. |
    | Resource group | Use the defaults or select from the list. |
    | User tags | Specify [user tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-user-tags) to organize your resources and for use by [backup policies](/docs/vpc?topic=vpc-backup-service-about) | 
    | Access management tags | Specify [access management tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-mgt-tags) that were created in IAM to help you manage access to your shares. |
    | **Profile** | Defaults to dp2. The profile of the new share must match the profile of the snapshot. |
    | Size | Enter a share size that is allowed by the profile. It must be bigger or equal to the size of the snapshot. |
    | IOPS | Select the IOPS that is within the range that the share profile allows. |
    | **Mount targets** | The creation of a mount target is optional, but you need to have a mount target if you want to connect to your file share from a virtual server instance. |
    | **Encryption** | Inherited from the snapshot.|
    {: caption="Create share options." caption-side="bottom"}

1. When finished, click **Create**. The new share is created.

## Restoring a share from a snapshot from the CLI
{: #fs-snapshots-restore-CLI}
{: cli}

You can create a {{site.data.keyword.filestorage_vpc_short}} share from a snapshot from the CLI. When the share is created, you can create a mount target and mount the share to an instance later.

### Before you begin
{: #fs-snapshots-restore-prereq-CLI}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to {{site.data.keyword.cloud}}.

   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

1. Gather information about the snapshot that you want to use to restore a share.
   1. You can use the CLI to [list all the snapshots of a specific share](/docs/vpc?topic=vpc-fs-snapshots-view&interface=cli#fs-snapshots-view-all-snapshots-cli) and choose one from the output. 
   1. Then, use the `ibmcloud is share-snapshot SHARE SNAPSHOT` command to list the details of the chosen snapshot.

### Creating a share from a snapshot from the CLI
{: #snapshots-restore-create-share-cli}

Run the `ibmcloud is share-create` command and specify the `--snapshot` parameter and the ID or the CRN of the snapshot.

```sh
$ ibmcloud is share-create --name my-file-share-from-snapshot --snapshot cli-share-snapshot --share my-file-share --zone us-south-1 --profile dp2 --size 100 --iops 2000  --user-tags env:test
Creating file share my-file-share-from-snapshot under account Test Account as user test.user@ibm.com...
                                      
ID                                 r134-c956e095-afb1-408c-a589-887e77afab20   
Name                               my-file-share-from-snapshot   
CRN                                crn:v1:staging:public:is:us-south-1:a/a1234567::share:r134-c956e095-afb1-408c-a589-887e77afab20   
Lifecycle state                    pending   
Access control mode                security_group   
Accessor binding role              none   
Allowed transit encryption modes   user_managed,none   
Zone                               us-south-1   
Profile                            dp2   
Size(GB)                           100   
IOPS                               2000  
User Tags                          env:test   
Encryption                         provider_managed   
Mount Targets                      ID                          Name      
                                   No mounted targets found.      
                                      
Resource group                     ID                                 Name      
                                   11caaa983d9c4beb82690daab08717e9   Default      
                                      
Created                            2024-12-17T11:21:57+05:30   
Replication role                   none   
Replication status                 none   
Replication status reasons         Status code   Status message      
                                   -             -   
```
{: screen}

The following example creates a share from a snapshot by using the CRN of the snapshot. If you don't provide a name for your new share, a name is auto-generated for you. The size and IOPS values are inherited from the snapshot, and you can change them after the share is created.

```sh
$ ibmcloud is share-create --profile dp2 --snapshot crn:v1:staging:public:is:us-south-1:a/a1234567::share-snapshot:r134-2ae87eb2-b26c-4126-ab34-e6e64f6f1773/r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3 --user-tags dev:tags
Creating file share  under account Test Account as user test.user@ibm.com...
                                      
ID                                 r134-5fb2d6aa-544d-4469-8e59-36e0fc21d0fa   
Name                               portion-unsafe-itinerary-oppressor   
CRN                                crn:v1:staging:public:is:us-south-1:a/a123456::share:r134-5fb2d6aa-544d-4469-8e59-36e0fc21d0fa   
Lifecycle state                    pending   
Access control mode                security_group   
Accessor binding role              none   
Allowed transit encryption modes   user_managed,none   
Zone                               us-south-1   
Profile                            dp2   
Size(GB)                           40   
IOPS                               100   
User Tags                          dev:tags   
Encryption                         provider_managed   
Mount Targets                      ID                          Name      
                                   No mounted targets found.      
                                      
Resource group                     ID                                 Name      
                                   11caaa983d9c4beb82690daab08717e9   Default      
                                      
Created                            2024-12-17T11:23:15+05:30   
Replication role                   none   
Replication status                 none   
Replication status reasons         Status code   Status message      
                                   -             -      
```
{: screen}

For more information about available command options, see [`ibmcloud is share-create`](/docs/vpc?topic=vpc-vpc-reference#share-create).

## Restoring a share from a snapshot with the API
{: #fs-snapshots-restore-API}
{: api}

You can programmatically restore a share by calling the `/shares` method in the [VPC API](/apidocs/vpc/latest#create-share).

Before you begin, gather information about the snapshot that you want to use to restore a share.
   - First, locate the snapshot and view its details. You can use the API to [list all the snapshots of a file share](/apidocs/vpc/latest#list-share-snapshots){: external} and select from the list. 
   - Then, [retrieve the snapshot](/apidocs/vpc/latest#get-share-snapshot){: external} details.

### Creating a share from a snapshot with the API
{: #fs-snapshots-restore-share-api}

To restore a share from a snapshot, make a `POST /shares` request and specify the ID or CRN of the snapshot in the `source_snapshot` property. The restored share capacity (in GBs) must be at least the snapshot's minimum_capacity.

The following example request creates a 100-GB share that is based on the source snapshot `r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4`.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/?version=2024-12-10&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
-d '{
     "name": "my-share-from-snapshot",
     "capacity": 100,
     "iops": 2000,
     "source_snapshot:" {"id": "r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4"},
     "profile": {"name": "dp2"},
     "zone": {"name": "us-south-1"},
     "encryption_key":{"crn":"crn:[...]"},
     "mount_targets": [
        {"virtual_network_interface": {"subnet": {"id": "2302-ea5fe79f-52c3-4f05-86ae-9540a10489f5"}},
        {"vpc": {"id": "7ec86020-1c6e-4889-b3f0-a15f2e50f87e"}}],
     "allowed_transit_encryption_modes": ["none","user_managed"],
     "resource_group": {},
     "user_tags": ["env:test"]
   }`
```
{: codeblock}

## Restoring a share from a snapshot with Terraform
{: #fs-snapshots-restore-terraform}
{: terraform}

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

### Creating a share from a snapshot with Terraform
{: #fs-snapshots-restore-share-terraform}

To create a share from a snapshot, use the `ibm_is_share` resource. The following example creates a share from a snapshot with the dp2 profile.

```terraform
resource "ibm_is_share" "storage" {
  name            = "restore-share-new"
  profile         = "dp2"
  zone            = "us-south-1"
  source_snapshot = "ibm_is_snapshot.example.crn"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

## Restoring files from a snapshot
{: #fs-snapshots-restore-single-file}

To perform single-file restoration, you can use native OS functions. You can browse to the share's NFS mount target to open the hidden `/.snapshot` directory and access the data that is contained within each snapshot of your share. Then, you can copy the selected file to a different location.

Although the snapshot directory is hidden, it is accessible and viewable when it is addressed directly with commands such as `cd .snapshot` or `ls .snapshot/`. All the snapshots that are present in the share are visible as subdirectories inside that hidden `/.snapshot` directory, and each snapshot directory is named with the snapshot's fingerprint that you see in the console, from the CLI, or with the API. See the following example.

```sh
[root@server .snapshot]# ls -lah
total 34K
drwxr-xr-x  3 root root  3 May 10 08:28 .
drwxr-xr-x 22 root root 56 May 10 08:28 ..
drwxr-xr-x  2 root root  2 May 10 08:28 c2c2439c-cbeb-4f12-8d9d-6059a3b85502
```
{: screen}

These subdirectories represent the state of the share as of the point-in-time when the snapshot was taken. The subdirectories are read-only. It's not possible to delete data from these snapshots through NFS methods. Similarly, creating new subdirectories is prohibited. This view is only for convenience in performing single-file restoration, and point-in-time inspection of the data.

## Next steps
{: #fs_snapshots_restore_next_steps}

You can [create](/docs/vpc?topic=vpc-fs-snapshots-create) more snapshots or [manage](/docs/vpc?topic=vpc-fs-snapshots-manage) existing snapshots.
