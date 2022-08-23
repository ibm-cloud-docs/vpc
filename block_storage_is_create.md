---

copyright:
  years: 2019, 2022
lastupdated: "2022-08-23"

keywords:

subcollection: vpc

---

{:shortdesc: .Shortdesc}
{:codeblock: .codeblock}
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

Create a block storage volume by using the UI, CLI, or programically with the API. You can create a volume as part of instance provisioning, as a stand-alone volume that you can later attach to an instance, or by restoring from a snapshot.
{: shortdesc}

Before you get started, make sure that you [created a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: important}

## Creating block storage volumes in the UI
{: #creating-block-storage-ui}
{: ui}

Use the {{site.data.keyword.cloud_notm}} console to create a block storage volume when you create a virtual server instance or as a stand-alone volume.

### Create and attach a block storage volume when you create a new instance
{: #create-from-vsi}
{: help}
{: support}

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

2. From the list of virtual server instances, click **Create**. For information on provisioning the instance, see [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui). 

   A boot volume is created and appears under **Boot volume** on the provision page. You can edit the boot volume by clicking the pencil icon to change the size, encryption, and add any user tags to identify this resource.

3. To create a new data volume and attach it to the instance, in the Data volumes section of the instance provisioning page, click **Create**. Specify the volume details in the side panel. Enter the information in Table 1.

4. Optionally, if you created volume snapshots, you can import the snapshot and use it to create the volume. Click the **Snapshots** toggle on, select a snapshot, and then provision as for a regular volume.

5. Click **Create virtual server** when finished.

| Field | Value |
|-------|-------|
| Name  | Specify a unique, meaningful name for your volume, for example, a name that describes your compute or workload function. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. Volume names must be unique the entire VPC infrastructure. |
| Resource group | The resource group is inherited from the instance. |
| Location | Location inherited from the instance (for example, Dallas 2) |.
| Auto Delete | Enable this feature to automatically delete this volume when the attached virtual server instance is deleted. You can change this setting later on the virtual server details page. |
| Tags | Specify user tags to associate with this volume. For more information about organizing resources with user tags, see [Working with tags](/docs/account?topic=account-tag&interface=ui). |
| Size | Enter a volume size in GBs. Volume sizes can be 10 - 16,000 GB. |
| Profile | Select [Tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) and select the IOPS tier dropdown list. If your performance requirements don't fall within a predefined IOPS tier, select [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) and select an IOPS value within the range for that volume size. |
| Encryption | Provider-managed encryption with IBM-managed keys is enabled by default on all volumes. You can also choose a Key Management Service by selecting either Key Protect or Hyper Protect Crypto Service, and then specify your own encryption key. For a one-time set-up procedure, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs). |
{: caption="Table 1. Block storage volume values specified when provisioning an instance." caption-side="bottom"}

When you're finished defining the volume, click **Create**. A block storage volume is created and attached to the virtual server instance. On the instance details page, the Data volumes list is updated to show the new volume.

A block storage volume can be attached to only one virtual server at a time. On the block storage volume summary page, you can view details about the virtual server instance by selecting the instance name under **Attached instances**.

### Create a stand-alone block storage volume
{: #create-standalone-vol}
{: help}
{: support}

You can create a block storage volume independent of virtual server provisioning and attach the volume to an instance later.

1.  [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

Be sure to select VPC infrastructure from the menu icon.
{: tip}

1. Select **New volume**.
1. Enter the information in Table 3 to define your new block storage volume.
1. When finished, click **Create volume**. You're returned to the Block storage volumes page, where a message indicates that the volume is being created (volume status is _pending_). A second message displays when the volume is created (volume status is _available_).
1. To see details of the new volume, select the **View resource** link in the second message to go to the Volume details page.

| Field | Value |
|-------|-------|
| Name  | Specify a meaningful name for your volume. For example, provide a name that describes your compute or workload function. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. You can later edit the name. |
| Resource Group | Specify a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups). |
| Tags | Specify a tag to organize your resources. A tag is a label that you assign to a resource for easy filtering of resources in your resource list. For more information about tags, see [Working with tags](/docs/account?topic=account-tag). |
| Location | The availability zone, inherited from the VPC (for example, Dallas-1). You can select a different zone in your location from the dropdown menu. |
| Size | Enter a volume size in GBs. Volume sizes can be 10 - 16,000 GB. |
| IOPS | Select [IOPS Tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) and then select the tile with performance level you require. |
| | Select [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) to specify a custom IOPS value based on the size of the volume you're creating. Click the **storage size** link to see a table of size and IOPS ranges. For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom). |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also choose **Customer Managed** and use [your own encryption key](/docs/vpc?topic=vpc-block-storage-vpc-encryption). |
{: caption="Table 3. Values for defining a block storage volume" caption-side="bottom"}

## Create block storage volumes from the CLI
{: #creating-block-storage-cli}
{: cli}

You can create  block storage volumes by using the command-line interface (CLI).

### Before you begin
{: #before-creating-block-storage-cli}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. After you install the VPC CLI plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.
   
3. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

### Create a block storage volume from the CLI
{: #create-vol-cli}
{: help}
{: support}

Run the following command to create a block storage volume. Provide a volume name, profile name, and the name of the availability zone in your region. For information about block storage profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). Optional parameters are shown in brackets.

```zsh
ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--tags  TAG_NAME1,TAG_NAME2,...] [--json]
```
{: pre}

Example:

```bash
$ ibmcloud is volume-create demovolume1 custom us-south-1 --capacity 500 --iops 3000 --tags env:test,env:prod
Creating volume demovolume1 in resource group Default under account VPC 01 as user rtuser1@mycompany.com...

ID                                     584ab34a-6d25-4226-a5a4-4c6c8aa9186d
Name                                   my-volume
CRN                                    crn:[...]
Status                                 available
Capacity                               500
IOPS                                   3000
Bandwidth(Mbps)                        393
Profile                                general-purpose
Encryption key                         -
Encryption                             provider_managed
Resource group                         Default
Created                                2022-04-07T11:42:22.287+05:30
Zone                                   us-south-1
Volume Attachment Instance Reference   -
Active                                 false
Busy                                   false
Tags                                   env:test,env:cli
```
{: screen}

Capacity, indicated in megabytes, can range 10 - 16,000 GBs. If not specified, the default capacity is 100 GBs. IOPS values can be 100 - 48,000 IOPS, depending on the profile and volume size. If not specified, the IOPS value defaults to the valid configuration per volume profile. For more information, see the table of [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom).

The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter.
Volume names must be unique across the entire VPC infrastructure.

Note the volume ID. You need to specify the ID when you attach block storage to a virtual server instance, view block storage volume details, or delete volumes.

User tags are added to identify the volume resource. When these tags are matched with the tags in a backup policy, the volume is backed up according to the schedule in the backup plan. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-policy-create).

### Create an instance and add user tags to volumes with the CLI
{: #create-instance-vol-cli}
{: cli}

You can specify user tags for boot and data volumes when you create a virtual server instance. Run the `instance-create` command and specify user tags in the `user_tags` parameter. Note that you can later edit the tags or add more to the specified volume. For more information, see the [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference&interface=ui).

In the example command, 

* The boot volume is defined by the `boot_volume` parameter that specifies the user tags `env:test4` and `env:dev4`.
* The data volume attachment is defined by the `volume-attach` parameter and specifies the same user tags.

You can see the user tags when you look at the details of the boot or data volume after the instance is created.

```
ibmcloud is instance-create my-vsi-4 my-vpc-2 us-south-1 bx2-2x8 cli-subnet-1 --image ibm-centos-7-9-minimal-amd64-6 --keys my-keys4 --boot-volume '{"name": "boot-vol-4", "volume": {"name": "my-boot-vol-4", "profile": {"name": "general-purpose"},"user_tags": ["env:test4", "env:dev4"]}}' --volume-attach  '[{"name": "my-vol-att1", "volume": {"name":"my-vol-4", "profile": {"name": "general-purpose"}, "capacity": 10 ,"user_tags": ["env:test4", "env:dev4"]}}]'

Creating instance my-vsi-4 under account vpcdemo1 as user myuser@mycompany.com...
                                         
ID                                    efd06503-f514-4ea2-ba6c-2b9c715e6269   
Name                                  my-vsi-4   
CRN                                   crn:v1:bluemix:public:is:us-south-1:a/7f75c7b025e54bc5635f754b2f888665::instance:efd06503-f514-4ea2-ba6c-2b9c715e6269   
Status                                pending   
Availability policy on host failure   restart   
Startable                             true   
Profile                               bx2-2x8   
Architecture                          amd64   
vCPUs                                 2   
Memory(GiB)                           8   
Bandwidth(Mbps)                       4000   
Volume bandwidth(Mbps)                1000   
Network bandwidth(Mbps)               3000   
Metadata service                      Enabled      
                                      false      
                                         
Image                                 ID                                          Name      
                                      r006-c5471ef3-15f5-4468-964b-f9b4d3ca8120   ibm-centos-7-9-minimal-amd64-6      
                                         
VPC                                   ID                                          Name      
                                      r006-dd1c6831-cc4d-4c40-aab2-6b0c17f41424   my-vpc-2      
                                         
Zone                                  us-south-1   
Resource group                        ID                                 Name      
                                      bdd96715c2a44f2bb60df4ff14a543f5   Default      
                                         
Created                               2022-08-09T09:25:06+05:30   
Boot volume                           ID   Name   Attachment ID                               Attachment name      
                                      -    -      0717-1bcf5342-c12f-4908-ad0f-99218cd603b9   boot-vol-4
                                         
Data volumes                          ID   Name   Attachment ID                               Attachment name      
                                      -    -      0717-c034f0f9-2269-44e4-a76a-4f4ad8c47b65   my-vol-att1 
```
{: screen}

### Create an instance template and add user tags to volumes with the CLI
{: #create-instance-template-vol-cli}
{: cli}

You can specify user tags for boot and data volumes when you create an instance template. Run the `instance-template-create` command and specify user tags in the `user_tags` parameter.

This example command adds two user tags to the instance template that will be applied to the volumes.

```
ibmcloud is instance-template-create my-tpl-1 my-vpc-2 us-south-1 bx2-2x8 cli-subnet-1 --image ibm-centos-7-9-minimal-amd64-6 --keys my-keys4 --boot-volume '{"name": "boot-vol-1", "volume": {"name": "my-boot-vol-1", "profile": {"name": "general-purpose"},"user_tags": ["env:test1", "env:dev1"]}}' --volume-attach  '[{"name": "my-vol-att", "volume": {"name":"my-vol-1", "profile": {"name": "general-purpose"}, "capacity": 10 ,"user_tags": ["env:test1", "env:dev1"] }}]' 

Creating instance template my-tpl-1 under account vpcdemo as user myuser@mycompany.com...
                               
ID                          38ef87e9-3a4d-4d72-8450-86c16b76ae0d   
Name                        my-tpl-1   
CRN                         crn:v1:bluemix:public:is:us-south-1:a/7f75c7b025e54bc5635f754b2f888665::instance-template:38ef87e9-3a4d-4d72-8450-86c16b76ae0d   
Resource group              Default   
Image                       c5471ef3-15f5-4468-964b-f9b4d3ca8120   
VPC                         dd1c6831-cc4d-4c40-aab2-6b0c17f41424   
Zone                        us-south-1   
Profile                     bx2-2x8   
Metadata service            Enabled      
                            false      
                               
Keys                        Key IDs      
                            e7d18039-0e0e-4207-8598-de7cca949030      
                               
Boot volume                 Name            Capacity   Profile           IOPS   Attachment name   Auto delete   Tags      
                            my-boot-vol-1   100        general-purpose   0      boot-vol-1     true          env:test1,env:dev1      
                               
Data volumes                ID   Name       Capacity   Profile           IOPS   Attachment name   Auto delete   Tags      
                            -    my-vol-1   10         general-purpose   0      my-vol-att        false         env:test1,env:dev1      
                               
Primary Network Interface   Name      Subnet ID                                   Security Groups                             Allow source IP spoofing   Reserved IP Address   Reserved IP ID   Reserved IP Name   Reserved IP Auto Delete      
                            primary   0617-c1f1e468-3283-4f44-9d08-3d57b77817f1   7c2db5ac-19e0-4d83-8226-e6178fedfe56   false                      -                     -                -                  -      
                               
Created                     2022-08-22T09:10:34+05:30 

```
{: screen}

Use the instance template to create an instance and volumes with user tags. For example:

```
ibmcloud is instance-template-create --template my-tpl-1 --name my-vsi-3 --boot-volume '{"name": "boot-vol-3", "volume": {"name": "my-boot-vol-3", "profile": {"name": "general-purpose"},"user_tags": ["env:test3", "env:dev3"]}}' --volume-attach  '[{"name": "my-vol-att1", "volume": {"name":"my-vol-3", "profile": {"name": "general-purpose"}, "capacity": 10 ,"user_tags": ["env:test3", "env:dev3"] }}]'
```
{: screen}

## Create block storage volumes with the API
{: #creating-block-storage-api}
{: api}

You can create block storage volumes by directly calling the [VPC REST APIs](https://{DomainName}/apidocs/vpc){: external}. For more information the file shares VPC API, see the [VPC API reference](https://cloud.ibm.com/apidocs/vpc).

### Before you begin – Set up your API environment
{: #block-storage-api-prereqs}

Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

### Create a data volume as part of instance provisioning with the API
{: #block-storage-create-instance-api}

Make a `POST /instances` request to create an instance, and define the volume by using the `volume_attachments` parameter. Specify a volume name, capacity, and profile. Also, specify `generation=2` in the request.

This example specifies customer-managed encryption and user tags for the boot and data volumes.

```curl
curl -X POST "$vpc_api_endpoint/v1/instances?version=2022-06-14&generation=2" -H "Authorization: $iam_token" -d '{
  "boot_volume_attachment": {
    "volume": {
      "encryption_key": {
        "crn": "crn:[...]"
      },
      "name": "my-boot-volume",
      "profile": {
        "name": "general-purpose"
      },
      "user_tags": {
        "env:test",
        "env:prod"
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
        },
        "user_tags": {
          "env:test",
          "env:prod"
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
{: pre}

A successful response looks like this:

```json
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
  "created_at": "2022-06-15T16:11:57Z",
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
{: codeblock}

### Create a stand-alone block storage volume with the API
{: #block-storage-create-vol-api}

Make a `POST /volumes` request to create a volume. Specify a name, IOPS, capacity, the profile, and zone.  Also, specify `generation=2` in the request.

This example also specifies customer-managed encryption and a resource group.

```curl
curl -X POST "$vpc_api_endpoint/v1/volumes?version=2022-06-14&generation=2" \
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
{: pre}

A successful response looks like this:

```json
{
  "capacity": 50,
  "created_at": "2022-06-14T23:16:53.000Z",
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
{: codeblock}

### Create a data volume from a snapshot of an unattached volume with the API
{: #block-storage-create-vol-snapshot-api}

You can specify a snapshot ID in a `POST /volumes` call to create a stand-alone data volume. Data is fully restored when you attach the data volume to a virtual server instance. For more information, see [About restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-restore-concepts). For an example API call, see [Restoring a data volume from a snapshot of an unattached volume](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-unattached-api).

## Next steps
{: #next-step-creating-block-storage-vpc}

When you refresh the Block storage volumes page, the new volume appears at the beginning of the list of volumes. If the volume was created successfully, it shows a status of Available. For stand-alone volumes, the Attachment Type column is blank (-). The Action menu (...) at the end of a table row provides a link for [attaching a block storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage#snapshots-vpc-restore-unattached-vol).

You can continue creating more block storage volumes or manage existing volumes.

* [View details about a block storage volume.](/docs/vpc?topic=vpc-viewing-block-storage)
* [Detach a volume from a virtual server instance.](/docs/vpc?topic=vpc-managing-block-storage#detach)
* [Delete a block storage volume.](/docs/vpc?topic=vpc-managing-block-storage#delete)

