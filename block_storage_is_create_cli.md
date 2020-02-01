---

copyright:
  years: 2019, 2020
lastupdated: "2019-11-06"

keywords: block storage, IBM Cloud, VPC, CLI, block storage volume, volume, IOPS

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}

# Creating block storage volumes by using the CLI
{: #creating-block-storage-cli}

You can create {{site.data.keyword.block_storage_is_short}} volumes by using the command line interface (CLI).
{:shortdesc}

## Before you begin
{: #before-creating-block-storage-cli}

1. Make sure that you downloaded, installed, and initialized the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in

   For more information, see the [CLI Reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
   
   After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the command `ibmcloud is target --gen 2`.
   {:important}
   
2. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-getting-started).

## Create a block storage volume by using the CLI
{: #create-vol-cli}

Run the following command to create a block storage volume. Provide a volume name, profile name, and the name of the availability zone in your region. For information about block storage profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). Optional parameters are shown in brackets.

```bash
ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]
```

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
Created                                 2019-11-15 10:09:28
Zone                                    us-south-1
Volume Attachment Instance Reference    none
```
{:screen}

Capacity, indicated in megabytes, can range 10 - 2,000 GBs. If not specified, the default capacity is 100 GBs. IOPS values can be 1,000 - 20,000 IOPS, depending on volume size. If not specified, the IOPS value defaults to the valid configuration per volume profile. For more information, see the table of [IOPS ranges based on volume size](/docs/vpc?topic=vpc-block-storage-profiles#custom).

The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter.

Note the volume ID. You need to specify the ID when you attach block storage to a virtual server instance, view block storage volume details, or delete volumes.

Do you prefer to create block storage volumes from the {{site.data.keyword.cloud}} console? For information, see [Creating block storage volumes](/docs/vpc?topic=vpc-creating-block-storage).
{: tip}

## Next steps
{: #next-step-creating-block-storage-cli}

[Attach a block storage volume by using the CLI](/docs/vpc?topic=vpc-attaching-block-storage-cli).
