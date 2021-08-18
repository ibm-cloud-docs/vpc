---

copyright:
  years: 2019, 2021
lastupdated: "2021-08-17"

keywords: block storage, VPC, virtual private cloud, boot volume, data volume, volume, data storage, VSI, virtual server instance, instance, IOPS

subcollection: vpc


---

{:shortdesc: .Shortdesc}
{: codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .Aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}


# Creating block storage volumes 
{: #creating-block-storage}
[comment]: # (linked help topic)

Create a block storage volume by using the UI, CLI, or programically with the API. You can create a volume as part of instance provisioning, or as a stand-alone volume that you can later attach to an instance.
{:shortdesc}

Before you get started, make sure that you [created a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

## Creating block storage volumes by using the UI
{: #creating-block-storage-ui}
{:ui}

Use the {{site.data.keyword.cloud_notm}} console to create a block storage volume when you create a virtual server instance or as a stand-alone volume.

### Create and attach a block storage volume when you create a new instance
{: #create-from-vsi}
{: help}
{: support}

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

Be sure to select VPC infrastructure from the Menu icon.  
{: tip}

1. Click **New instance** and enter the information in Table 1.

1. Click **Create virtual server instance** when you are ready to provision.

| Field | Value |
|-------|-------|
| Name  | Required; provide a name for your virtual server instance. |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. |
| Resource group | Select a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) for the instance. Resource groups help organize your account resources for access control and billing purposes. |
| Location | Locations are specific geographic areas where the data center is located (for example, Dallas) and zone (for example, Dallas 01). Select the location where you want your virtual server instance to be created. |
| Image | All images use cloud-init, which allows you to enter user metadata associated with the instance for post provisioning scripts. |
| Profile |  Select from popular profiles or all available vCPU and RAM combinations. For more information, see [Instance profiles](/docs/vpc?topic=vpc-profiles). |
| SSH Key | Required; select an existing SSH key or upload a new SSH key. SSH keys are used to securely connect to the instance after it's running. |
| | **Note:** Alpha-numeric combinations are limited to 100 characters. |
| | For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| User data | You can add user data that automatically performs common configuration tasks or runs scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). |
| Boot volume | The default boot volume size for all profiles is 100 GB. If you are importing a custom image, the boot volume capacity can be 10 GB to 250GB, depending on what the image requires. (Images smaller than 10 GB are rounded up to 10 GB.) Boot volumes are encrypted by default using IBM-managed encryption. You can change the encryption method to customer-managed encryption and use your own root keys. For information, see [Provisioning virtual server instances customer-managed encryption volumes in the UI](/docs/vpc?topic=vpc-creating-instances-byok#provision-byok-ui). |
| Attached block storage volume | You can add one or more secondary data volumes to be included when you provision the instance. To add a a volume, click **New block storage volume** and specify the information in Table 2. When finished, click **Create volume**. |
| Network interfaces | Assign networking options to connect into the IBM Cloud VPC. You can create and assign up to five network interfaces to each instance. |
{: caption="Table 1. Virtual Server Instance provisioning selections" caption-side="top"}

| Field | Value |
|-------|-------|
| Name  | Specify a unique, meaningful name for your volume, for example, a name that describes your compute or workload function. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. Volume names must be unique the entire VPC infrastructure. |
| Profile | Select [Tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) and select the performance level that you require from the IOPS list. If your performance requirements don't fall within a predefined IOPS tier, select [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) and select an IOPS value within the range for that volume size. Click the **storage size** link to see a table of size and IOPS ranges. |
| Auto Delete | Enable this feature to automatically delete this volume when the attached virtual server instance is deleted. You can change this setting later on the virtual server details page. |
| IOPS | Select 3, 5, or 10 IOPS/GB for a Tiered profile |
| Size | Enter a volume size in GBs. Volume sizes can be between 10 GB and 16,000 GB. |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also choose **Customer Managed** and use your own encryption key. For a one-time set-up procedure, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs). |
{: caption="Table 2. Block storage volume values specified when provisioning an instance" caption-side="top"}

A block storage volume is created and attached to the virtual server instance. On the instance details page, the **Attached block storage volumes** list is updated to show the new volume.

A block storage volume can be attached to only one virtual server at a time. On the block storage volume summary page, you can view details about the virtual server instance by selecting the instance name under **Attached instances**.

### Create a stand-alone block storage volume
{: #create-standalone-vol}
{: help}
{: support}

You can create a block storage volume independent of virtual server provisioning and attach the volume to an instance later.

1.  [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

Be sure to select VPC infrastructure from the Menu icon.
{: tip}

1. Select **New volume**.
1. Enter the information in Table 3 to define your new block storage volume.
1. When finished, click **Create volume**. You're returned to the Block storage volumes page, where a message indicates that the volume is being created (volume status is _pending_). A second message displays when the volume is created (volume status is _available_).
1. To see details of the new volume, select the **View resource** link in the second message to go to the Volume details page.

| Field | Value |
|-------|-------|
| Name  | Specify a meaningful name for your volume. For example, provide a name that describes your compute or workload function. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. You can later edit the name. |
| Resource Group | Specify a 
[resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups). |
| Tags | Specify a tag to organize your resources. A tag is a label that you assign to a resource for easy filtering of resources in your resource list. For more information about tags, see [Working with tags](/docs/account?topic=account-tag). |
| Location | The availability zone, inherited from the VPC (for example, Dallas-1). You can select a different zone in your location from the dropdown menu. |
| Size | Enter a volume size in GBs. Volume sizes can be between 10 GB - 16,000 GB. |
| IOPS | Select [IOPS Tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) and then select the tile with performance level you require. |
| | Select [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) to specify a custom IOPS value based on the size of the volume you're creating. Click the **storage size** link to see a table of size and IOPS ranges. For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom). |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also choose **Customer Managed** and use [your own encryption key](/docs/vpc?topic=vpc-block-storage-vpc-encryption). |
{: caption="Table 3. Values for defining a block storage volume" caption-side="top"}


## Create block storage volumes by using the CLI
{: #creating-block-storage-cli}
{:cli}

You can create  block storage volumes by using the command line interface (CLI).

### Before you begin
{: #before-creating-block-storage-cli}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. After you install the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.
   
3. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).


### Create a block storage volume by using the CLI
{: #create-vol-cli}
{: help}
{: support}

Run the following command to create a block storage volume. Provide a volume name, profile name, and the name of the availability zone in your region. For information about block storage profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). Optional parameters are shown in brackets.

```
$ ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]
```
{:pre}

Example:

```bash
$ ibmcloud is volume-create demovolume1 custom us-south-1 --iops 1000
Creating volume demovolume1 in resource group Default under account VPC 01 as user rtuser1@mycompany.com...
ID                                      584ab34a-6d25-4226-a5a4-4c6c8aa9186d
Name                                    demovolume1
Capacity                                100
IOPS                                    1000
Profile                                 custom
Encryption Key                          -
Encryption                              provider_managed
Status                                  pending
Resource Group                          Default(dbb12715c2a22f2bb60df4ffd4a543f2)
Created                                 2021-04-24 10:09:28
Zone                                    us-south-1
Volume Attachment Instance Reference    none
```
{:screen}

Capacity, indicated in megabytes, can range 10 - 16,000 GBs. If not specified, the default capacity is 100 GBs. IOPS values can be 1,000 - 48,000 IOPS, depending on volume size. If not specified, the IOPS value defaults to the valid configuration per volume profile. For more information, see the table of [IOPS ranges based on volume size](/docs/vpc?topic=vpc-block-storage-profiles#custom).

The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure.

You need to specify the volume ID when you attach block storage to a virtual server instance, view block storage volume details, or delete volumes.

## Create block storage volumes from the API
{: #creating-block-storage-api}
{:api}

You can create block storage volumes by directly calling the [VPC REST APIs](https://{DomainName}/apidocs/vpc){:external}. For more information the file shares VPC API, see the [VPC API reference](https://cloud.ibm.com/apidocs/vpc).

### Before you begin – Set up your API environment
{: #block-storage-api-prereqs}

Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

### Create a data volume as part of instance provisioning from the API
{: #block-storage-create-instance-api}

Make a `POST /instances` request to create an instance, and define the volume using the `volume_attachments` parameter. Specify a volume name, capacity, and profile. Also specify `generation=2` in the request.

This example also specifies customer-managed encryption.

```
curl -X POST "$vpc_api_endpoint/v1/instances?version=2021-04-06&generation=2" -H "Authorization: $iam_token" -d '{
  "boot_volume_attachment": {
    "volume": {
      "encryption_key": {
        "crn": "crn:[...]"
      },
      "name": "my-boot-volume",
      "profile": {
        "name": "general-purpose"
      }
    }
  },
  "image": {
    "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"
  },
  "keys": [
    {
      "id": "363f6d70-0000-0001-0000-00000013b96c"
    }
  ],
  "name": "my-instance",
  "placement_target": {
    "id": "0787-8c2a09be-ee18-4af2-8ef4-6a6060732221"
  },
  "primary_network_interface": {
    "name": "my-network-interface",
    "subnet": {
      "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4"
    }
  },
  "profile": {
    "name": "bx2-2x8"
  },
  "volume_attachments": [
    {
      "volume": {
        "capacity": 1000,
        "encryption_key": {
          "crn": "crn:[...]"
        },
        "name": "my-data-volume",
        "profile": {
          "name": "5iops-tier"
        }
      }
    }
  ],
  "vpc": {
    "id": "f0aae929-7047-46d1-92e1-9102b07a7f6f"
  },
  "zone": {
    "name": "us-south-1"
  }
}'
```
{:pre}

A successful response will look like this:

```
{
  "bandwidth": 4000,
  "boot_volume_attachment": {
    "device": {
      "id": "a8a15363-a6f7-4f01-af60-715e85b28141"
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/eb1b7391-2ca2-4ab5-84a8-b92157a633b0/volume_attachments/7389-a8a15363-a6f7-4f01-af60-715e85b28141",
    "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
    "name": "my-boot-volume-attachment",
    "volume": {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/49c5d61b-41e7-4c01-9b7a-1a97366c6916",
      "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
      "name": "my-boot-volume"
    }
  },
  "created_at": "2021-04-24T16:11:57Z",
  "crn": "crn:[...]",
  "dedicated_host": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/dedicated_hosts/0787-8c2a09be-ee18-4af2-8ef4-6a6060732221",
    "id": "0787-8c2a09be-ee18-4af2-8ef4-6a6060732221",
    "name": "test-new",
    "resource_type": "dedicated_host"
  },
  "disks": [],
  "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/eb1b7391-2ca2-4ab5-84a8-b92157a633b0",
  "id": "eb1b7391-2ca2-4ab5-84a8-b92157a633b0",
  "image": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images/9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366",
    "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366",
    "name": "my-image"
  },
  "memory": 8,
  "name": "my-instance",
  "network_interfaces": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/network_interfaces/7ca88dfb-8962-469d-b1de-1dd56f4c3275",
      "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
      "name": "my-network-interface",
      "primary_ipv4_address": "10.0.0.32",
      "resource_type": "network_interface",
      "subnet": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7389-bea6a632-5e13-42a4-b4b8-31dc877abfe4",
        "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4",
        "name": "my-subnet"
      }
    }
  ],
  "placement_target": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/dedicated_hosts/0787-8c2a09be-ee18-4af2-8ef4-6a6060732221",
    "id": "0787-8c2a09be-ee18-4af2-8ef4-6a6060732221",
    "name": "test-new",
    "resource_type": "dedicated_host"
  },
  "primary_network_interface": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/network_interfaces/7ca88dfb-8962-469d-b1de-1dd56f4c3275",
    "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
    "name": "my-network-interface",
    "primary_ipv4_address": "10.0.0.32",
    "resource_type": "network_interface",
    "subnet": {
      "crn": "crn:[...]",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/bea6a632-5e13-42a4-b4b8-31dc877abfe4",
      "id": "bea6a632-5e13-42a4-b4b8-31dc877abfe4",
      "name": "my-subnet"
    }
  },
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instance/profiles/bx2-2x8",
    "name": "bx2-2x8"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
    "id": "4bbce614c13444cd8fc5e7e878ef8e21",
    "name": "Default"
  },
  "status": "running",
  "status_reasons": [],
  "vcpu": {
    "architecture": "amd64",
    "count": 2
  },
  "volume_attachments": [
    {
      "device": {
        "id": "a8a15363-a6f7-4f01-af60-715e85b28141"
      },
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/volume_attachments/7389-a8a15363-a6f7-4f01-af60-715e85b28141",
      "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
      "name": "my-boot-volume-attachment",
      "volume": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/49c5d61b-41e7-4c01-9b7a-1a97366c6916",
        "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
        "name": "my-boot-volume"
      }
    },
    {
      "device": {
        "id": "e77125cb-4df0-4988-a878-531ae0ae0b70"
      },
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/volume_attachments/7389-e77125cb-4df0-4988-a878-531ae0ae0b70",
      "id": "e77125cb-4df0-4988-a878-531ae0ae0b70",
      "name": "my-volume-attachment-1",
      "volume": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2cc091f5-4d46-48f3-99b7-3527ae3f4392",
        "id": "2cc091f5-4d46-48f3-99b7-3527ae3f4392",
        "name": "my-data-volume"
      }
    }
  ],
  "vpc": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/f0aae929-7047-46d1-92e1-9102b07a7f6f",
    "id": "f0aae929-7047-46d1-92e1-9102b07a7f6f",
    "name": "my-vpc"
  },
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{:codeblock}


### Create a stand-alone block storage volume from the API
{: #block-storage-create-vol-api}

Make a `POST /volumes` request to create a volume. Specify a name, IOPS, capacity, the profile, and zone.  Also specify `generation=2` in the request.

This example also specifies customer-managed encryption and a resource group.

```
curl -X POST "$vpc_api_endpoint/v1/volumes?version=2021-04-06&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-volume-4",
      "iops": 100,
      "capacity": 50,
      "zone": {
        "name": "us-south-2"
      },
      "profile": {
        "name": "custom"
      },
      "encryption_key":{
        "crn":"crn:[...]"
      },
      "resource_group": {
        "id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054"
      }
    }'
```
{:pre}

A successful response will look like this:

```
{
  "capacity": 50,
  "created_at": "2021-04-06T23:16:53.000Z",
  "crn": "crn:[...]",
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
  "id": "2d1bb5a8-40a8-447a-acf7-0eadc8aeb054",
  "iops": 100,
  "name": "my-volume-4",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/custom",
    "name": "custom"
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
}
```
{:codeblock}

## Next steps
{: #next-step-creating-block-storage-vpc}

When you refresh the Block storage volumes page, the new volume appears at the beginning of the list of volumes. If the volume was created successfully, it shows a status of Available. For stand-alone volumes, the Attachment Type column is blank (-). The Action menu (...) at the end of a table row provides a link for [attaching a block storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

You can continue creating more block storage volumes or manage existing volumes.

* [View details about a block storage volume](/docs/vpc?topic=vpc-viewing-block-storage)
* [Detach a volume from a virtual server instance](/docs/vpc?topic=vpc-managing-block-storage#detach)
* [Delete a block storage volume](/docs/vpc?topic=vpc-managing-block-storage#delete)


