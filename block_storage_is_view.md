---

copyright:
  years: 2019, 2021
lastupdated: "2021-04-23"

keywords: block storage, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Viewing block storage volume details
{: #viewing-block-storage}

View details about a {{site.data.keyword.block_storage_is_short}} volume or summary information about all volumes.
{:shortdesc}

## View information about block storage volumes using the UI
{: #viewvols}
{:ui}

List all block storage volumes and view details for a single volume. View attached block storage volume details in instance details. View all snapshots created from the block storage volume.

### View information about all block storage volumes using the UI
{: #viewvols-ui}

Navigate to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

By default, block storage volumes display for all resource groups in your region. In the list of all **Block storage for VPC volumes**, you see the following information.

| Field | Description |
|-------|-------------|
| Region | The region where these volumes are located, for example, US South. Click the dowwn arrow to see volumes in a different region in which you have an account.)
| Name | Click the name of the volume to see individual volume details. |
| Status | Status of the volume, which functions as the default filter for all rows. For information about volume statuses, see [Block Storage volume statuses](/docs/vpc?topic=vpc-managing-block-storage#status). |
| Resource Group | Resource groups are defined when you set up your VPC. Resource groups help organize your account resources for access control and billing purposes. For more information, see [Managing resource groups](/docs/account?topic=account-rgs). |
| Location | Availability zone in your region, inherited from the VPC (for example, US South 1).|
| Size | Size of the volume you specified, in GBs.|
| Max IOPS | Maximum IOPS available on the volume, which is defined by the general-purpose IOPS tier or custom IOPS value you specified. |
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also use your own root keys in a Key Protect or HPCS instance to protect your data. For information, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption). |
| Tags | Specify a tag to organize your resources. A tag is a label that you assign to a resource for easy filtering of resources in your resource list. For more information about tags, see [Working with tags](/docs/account?topic=account-tag).
| Actions (...) | Click the overflow icon (...) to display a menu of context-specific actions you can take. For example, an available, unattached volume would have menu options for attaching to an instance or deleting the volume.|
{: caption="Table 1. Details about all volumes" caption-side="top"}

By default, 10 volumes are shown in the list of all block storage volumes.  Change this default by clicking the Page Control down arrow and increase the list to 20 or 50 volumes.  Use the Page Control arrows after the list to go to the following page or return to the current page.

Overflow menu actions depend on whether the volume is a boot volume, attached data volume, or unattached data volume. Table 2 describes these actions.

| Volume type | Action | Description |
|-------------|--------|-------------|
| Boot | Create image | Create an image from the boot volume. For more information, see [Create an image from the list of boot volumes](/docs/vpc?topic=vpc-create-ifv#import-custom-image-vol).
| | Create snapshot | Create a "bootable snapshot" from the boot volume. A snapshot is a point in time copy of the volume. For more information, see [Create a snaphot by using the UI](https://test.cloud.ibm.com/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create-from-volume-list).
| | Detach from instance | If the boot volume is attached as a secondary volume, you can [detach it](/docs/vpc?topic=vpc-managing-block-storage#detach) from the instance. |
| Data | Create snapshot | Create a point in time copy of the data volume. | 
| | Detach from instance | [Detach](/docs/vpc?topic=vpc-managing-block-storage#detach) the data volume from the instance. |
| Delete | [Delete](/docs/vpc?topic=vpc-managing-block-storage#delete) the volume. You must first detach the volume from an instance before deleting it. |
| Unattached (-) | Attach to an instance | [Attach](/docs/vpc?topic=vpc-attaching-block-storage) the volume to an available virtual server instance. |
| | Delete | Delete the unattached volume. |
{: caption="Table 2. Overflow menu options for volumes" caption-side="top"}

### View details about a block storage volume
{: #view-vol-details-ui}

To view details about a block storage volume, navigate to the list of all block storage volumes and select a volume. For a single volume, you see the following information.

| Field | Description |
|-------|-------------|
| Name  | Name of the volume you specified when you created the volume. Click the pencil icon to edit the volume name. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. |
| Resource group | Resource group defined when you set up your VPC. Resource groups manage access to resources but do not affect billing or monitoring.|
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| ID | System-generated volume ID. |
| Created | System-generated date when the volume was created.|
| Location | Availability zone in your region.|
| Size | Size of the volume you specified. When the volume is attached to a virtual server instance and the volume is not at maximum capacity for its range, you can click the icon to expand the volume. For information on expanding volume capacity, see [Expanding block storage volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes). |
| Profile | [IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) or [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile.|
| Max IOPS | Maximum IOPS value for a predefined IOPS tier or the value you specified for custom IOPS. |
| Throughput | The performance a disk can deliver, measured in Gigabits/second (Gbps). Based on your volume profile, throughput is calculated as the amount of IOPS * 16 K block size.|
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also use your own root keys to protect your data. The Encryption field shows the name of the key management service (KMS) you provisioned (for example, Key Protect) and **customer-managed**. For more information, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)|
| Encryption Instance | _Optional._ A link to the provisioned KMS instance for a customer-managed encryption volume. |
| Key | _Optional._ The name and copyable ID of the root key used to encrypt the passphrase securing a customer-managed encryption volume. |
{: caption="Table 3. Volume details" caption-side="top"}

Volumes attached to a virtual server instance are displayed under **Attached instances** on the **Volume details** page. You can also [attach a volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

For a volume attached to an instance, you can see information about the instance by clicking on the instance name and then return to a list of all Block Storage volumes.

### View attached block storage volume details in instance details
{: #view-vol-details-instance-ui}

You can view information about an attached block storage volume from the **Virtual server instance details** page:

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances** and select an instance.
2. Under **Attached block storage volumes**, click the name of a volume to go to the volume details page.

### View all snapshots created from the block storage volume
{: #view-snapshots-for-volume}

If you created snapshots of a block storage boot or data volume, you can see the snapshots on the volume details page.

3. Navigate to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**. Alternatively, click on the volume name from a [virtual server instance](#view-vol-details-instance}).

4. In the side navigation panel, click **Snapshots**. A list of snapshots displays with the name, status, size, encryption type, and when it was created. The snapshots display in descending order, with the most recently created snapshot on the top.

You can see details for a snapshot, create a new snapshot, and manage snapshots from this page. For example, the overflow menu (hellipsis) lets you delete the most recent snapshot. For more information, see:

* [View details of a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui)
* [Create a new snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui)
* [Delete all snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-all-ui)

## Viewing block storage volumes by using the CLI
{: #viewing-block-storage-cli}
{:cli}

View details about a block storage volume or summary information about all volumes from the CLI.

Before you begin, [install the IBM Clous CLI and VPC CLI plugin](/docs/vpc?topic=vpc-creating-block-storage#before-creating-block-storage-cli).

### View details about a block storage volume by using the CLI
{: #viewvol-cli}

Specify this command to show details about a volume.

```
ibmcloud is volume VOLUME_ID [--json]
```
{:pre}

Example:

This example uses the volume ID to show volume details.

```bash
$ ibmcloud is volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Getting volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account MyAccount01 as user user1@mycompany.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo_volume
Capacity                                100
IOPS                                    1000
Profile                                 custom
Encryption Key                          -
Encryption                              provider_managed
Profile                                 custom
Status                                  available
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Created                                 2021-04-22 10:09:28
Zone                                    us-south-1
Volume Attachment Instance Reference    none
```
{:screen}

If your volume is attached to a virtual server instance, the name and ID of the volume attachment and instance is also displayed.

### View all block storage volumes by using the CLI
{: #viewall-vol-cli}

Run this command to list summary information about all volumes:

```
ibmcloud is volumes [--json]
```
{:pre}

Example:

The following example shows all volumes for all resource groups in your availability zone.

```bash
$ ibmcloud is volumes
Listing volumes under account MyAccount 01 as user user1@mycompany.com...
ID                                     Name              Capacity   IOPS   Auto Delete   Encryption        Profile         Created               Status      Zone         Resource Group
0738-a3f4d60a-c9cf-4666-9a2a-0b1d85f92c19   demo_volume1      50         10     Manual        provider managed  generalpurpose   2021-04-22 11:04:46  pending     us-south-1   (c16d1edd-.)
0738-3aaa0beb-83ac-4b2f-b4f5-eab382d1a5aa   demo_volume2      50         100    Manual        provider managed  custom           2021-04-22 10:26:34  available   us-south-1   (c16d1edd-.)
0738-6d9713cf-9688-486d-b8c9-d9f1c6e7876c   demo_volume3      50         100    Manual        provider managed  custom           2021-04-22 10:39:24  available   us-south-1   (c16d1edd-.)
```
{:screen}

Specifying the resource group ID or name filters the list to volumes that belong to a resource group. When you omit this parameter, it defaults to all resource groups. You can also view all volumes in a particular availability zone.

By default, the first 25 volumes are displayed per page.

### View details about a volume attachment by using the CLI
{: #viewvol-attachment-cli}

Run this command to view details of a volume attachment to a virtual server instance:

```
ibmcloud is instance-volume-attachment INSTANCE_ID VOLUME_ATTACHMENT_ID [--json]
```
{:pre}

Specify an instance ID and a volume attachment ID.  Optionally, export the details in JSON format.

### View details about all volume attachments by using the CLI
{: #viewvol-all-attachments-cli}

Run this command to view all volume attachments for a virtual server instance.

```
ibmcloud is instance-volume-attachments INSTANCE_ID [--json]
```
{:pre}

### View volume profiles by using the CLI
{: #viewvol-profiles-cli}

Run this command to list all volume profiles available in your region.

```
ibmcloud is volume-profiles [--json]
```
{:pre}

Example:

```bash
$ ibmcloud is volume-profiles
Listing volume profiles under account MyAccount 01 as user user1@mycompany.com...
Name             Family   Crn
generalpurpose1  tiered   crn:v1:public:globalcatalog::::volume.profile:generalpurpose
generalpurpose2  tiered   crn:v1:public:globalcatalog::::volume.profile:generalpurpose
custom           custom   crn:v1:public:globalcatalog::::volume.profile:custom
generalpurpose3  tiered   crn:v1:public:globalcatalog::::volume.profile:generalpurpose
```
{:screen}

### View a specific volume profile by using the CLI
{: #viewvol-profile-cli}

Run this command to view a specific volume profile in your region.

```
ibmcloud is volume-profile PROFILE_NAME [--json]
```
{:pre}

PROFILE_NAME values are 10iops-tier, 5iops-tier, general-purpose, and custom.

Example:

```bash
$ ibmcloud is volume-profile generalpurpose1
Getting volume profile generalpurpose1 under account MyAccount 01 as user user1@mycompany.com...
Name     generalpurpose1
Family   tiered
Crn      crn:v1:public:globalcatalog::::volume.profile:generalpurpose
```
{:screen}

## Viewing block storage volumes from the API
{: #viewing-block-storage-api}
{:api}

View block storage volumes programically by making calls to the [VPC REST APIs](https://{DomainName}/apidocs/vpc){:external}. You can list all volumes and view details for a specific volume.

Before you begin, make sure that you [set up your API environment](/docs/vpc?topic=vpc-creating-block-storage#block-storage-api-prereqs).

### View all block storage volumes from the API
{: #viewall-vol-api}

Make a `GET /volume` call to list summary information about all volumes. For example:

```
curl -X GET "$vpc_api_endpoint/v1/volumes?version=2021-04-20&generation=2" \
-H "Authorization: $iam_token"
```
{:pre}

A successful response will look like this:

```
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes?limit=50"
  },
  "limit": 50,
  "volumes": [
    {
      "capacity": 100,
      "created_at": "2021-04-23T06:26:17Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/ccbe6fe1-5680-4865-94d3-687076a38293",
      "id": "ccbe6fe1-5680-4865-94d3-687076a38293",
      "iops": 1000,
      "name": "my-volume-1",
      "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
        "name": "general-purpose"
      },
      "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
        "id": "4bbce614c13444cd8fc5e7e878ef8e21",
        "name": "Default"
      },
      "status": "available",
      "status_reasons": [],
      "volume_attachments": [
        {
          "delete_volume_on_instance_delete": true,
          "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/33bd5872-7034-462b-9f3e-d400c49d347a/volume_attachments/b31c1a5a-122a-4e32-a10b-f2c31271de85",
          "id": "b31c1a5a-122a-4e32-a10b-f2c31271de85",
          "instance": {
            "crn": "crn:[...]",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/33bd5872-7034-462b-9f3e-d400c49d347a",
            "id": "33bd5872-7034-462b-9f3e-d400c49d347a",
            "name": "instance-1"
          },
          "name": "volume-attachment-1",
          "type": "data"
        }
      ],
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
        "name": "us-south-2"
      }
    },
    {
      "capacity": 100,
      "created_at": "2019-03-23T16:46:54Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/9de3e18c-cec9-4cac-a64a-0bdfab21e9d4",
      "id": "9de3e18c-cec9-4cac-a64a-0bdfab21e9d4",
      "iops": 1000,
      "name": "volume-2",
      "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
        "name": "general-purpose"
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
    },
    {
      "capacity": 100,
      "created_at": "2019-07-13T02:22:43Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/89ba05e9-6e35-4964-9747-7ae3f9b30303",
      "id": "89ba05e9-6e35-4964-9747-7ae3f9b30303",
      "iops": 1000,
      "name": "volume-3",
      "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/5iops-tier",
        "name": "5iops-tier"
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
        "name": "us-south-2",
        "resource_type": "zone"
      }
    }
  ]
}
```
{:codeblock}

### View a specific volume profile from the API
{: #viewvol-details-api}

Make a `GET /volumes/{id}` call to see details of a volume. For example:

```
curl -X GET "$vpc_api_endpoint/v1/volumes/$volume_id?version=2021-04-20&generation=2" \
-H "Authorization: $iam_token"
```
{:pre}

A successful response will provide details of the volume, including capacity and IOPS, the volume status, and whether the volume is attached to an instance.

```
{
  "capacity": 100,
  "created_at": "2021-04-23T06:26:17Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/ccbe6fe1-5680-4865-94d3-687076a38293",
  "id": "ccbe6fe1-5680-4865-94d3-687076a38293",
  "iops": 1000,
  "name": "my-volume-1",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
    "name": "general-purpose"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
    "id": "4bbce614c13444cd8fc5e7e878ef8e21",
    "name": "Default"
  },
  "status": "available",
  "status_reasons": [],
  "volume_attachments": [
    {
      "delete_volume_on_instance_delete": true,
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/33bd5872-7034-462b-9f3e-d400c49d347a/volume_attachments/b31c1a5a-122a-4e32-a10b-f2c31271de85",
      "id": "b31c1a5a-122a-4e32-a10b-f2c31271de85",
      "instance": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/33bd5872-7034-462b-9f3e-d400c49d347a",
        "id": "33bd5872-7034-462b-9f3e-d400c49d347a",
        "name": "instance-1"
      },
      "name": "volume-attachment-1",
      "type": "data"
    }
  ],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
    "name": "us-south-2"
  }
}
```
{:codeblock}

### View volume profiles from the API
{: #viewvol-profiles-api}

To list all volume profiles available in your region, make a `GET /volume/profiles` call. For example:

```
curl -X GET "$vpc_api_endpoint/v1/volume/profiles?version=2021-04-20&generation=2" 
-H "Authorization: $iam_token"
```
{:pre}

When all volume profiles are available, you'll see a response like this:

```
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles?limit=50"
  },
  "limit": 50,
  "profiles": [
    {
      "family": "tiered",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/10iops-tier",
      "name": "10iops-tier"
    },
    {
      "family": "tiered",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/5iops-tier",
      "name": "5iops-tier"
    },
    {
      "family": "tiered",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
      "name": "general-purpose"
    },
    {
      "family": "custom",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/custom",
      "name": "custom"
    }
  ],
  "total_count": 4
}
```
{:codeblock}



## Next steps
{: #next-step-viewing-block-storage}

Create more volumes or manage your existing block storage volumes.

* [Creating block storage volumes](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing block storage volumes](/docs/vpc?topic=vpc-managing-block-storage)
