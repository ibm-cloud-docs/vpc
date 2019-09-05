---

copyright:
  years: 2019
lastupdated: "2019-07-31"

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

# Creating block storage volumes using the CLI
{: #creating-block-storage-cli}

You can create {{site.data.keyword.block_storage_is_short}} volumes by using the command line interface (CLI).
{:shortdesc}

## Before you begin
{: #before-creating-block-storage-cli}

1. Make sure you have downloaded, installed, and initialized the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plugin

   For more information, see the [CLI Reference](/docs/vpc?topic=vpc-cli-reference).
   
   When you install the vpc-infrastructure plugin for the first time, you must set the target generation to gen 2, `ibmcloud is target --gen 2`.
   {:important}
   
2. Make sure you have [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-getting-started).

## Create a block storage volume using the CLI
{: #create-vol-cli}

Run the following command to create a block storage volume. Provide a volume name, profile name, and the name of the availability zone in your region. For information on block storage profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). Optional parameters are shown in brackets.

```bash
ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]
```

Example:

```bash
$ ibmcloud is volume-create demovolume1 custom us-south-1 --iops 1000
Creating volume demovolume1 in resource group Default under account VPC 01 as user rtuser1@mycompany.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demovolume1
Capacity                                100
IOPS                                    1000
Profile                                 custom
Encryption Key                          -
Encryption                              provider_managed
Status                                  pending
Resource Group                          Default(dbb12715c2a22f2bb60df4ffd4a543f2)
Created                                 2019-07-15 10:09:28
Zone                                    us-south-1
Volume Attachment Instance Reference    none
```
{:screen}

Capacity, indicated in megabytes, can range from 10 to 2,000 GBs. If not specified, the default capacity is 100 GBs. IOPS values can be 1,000 to 20,000 IOPS, depending on volume size. If not specified, the IOPS value defaults to the valid configuration per volume profile. For information, see the table of [IOPS ranges based on volume size](/docs/vpc?topic=vpc-block-storage-profiles#custom).

The volume ID in the above example is used when attaching block storage to a virtual server instance, viewing block storage volume details, and deleting volumes.

Do you prefer to create block storage volumes from the {{site.data.keyword.cloud}} console? For information, see [Creating block storage volumes](/docs/vpc?topic=vpc-creating-block-storage).
{: tip}

## Next steps
{: #next-step-creating-block-storage-cli}

[Attach a block storage volume using the CLI](/docs/vpc?topic=vpc-attaching-block-storage-cli).
