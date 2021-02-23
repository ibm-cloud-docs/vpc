---

copyright:
  years: 2020, 2021 
lastupdated: "2021-02-23"

keywords: dedicated host instance, instance on vpc dedicated host, dedicated instance

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:beta: .beta}
{:table: .aria-labeledby="caption"}

# Creating instances on dedicated hosts 
{: #creating-instance-on-dh}

When you have a dedicated host created in your {{site.data.keyword.vpc_short}}, you can provision virtual server instances on the dedicated host by using the {{site.data.keyword.cloud_notm}} console, the CLI, or the API.
{:shortdesc}

## Creating instances on dedicated hosts by using the console
{: #creating-dedicated-instance}

When you have a dedicated host created, you can start provisioning virtual server instances on the dedicated host by using the {{site.data.keyword.cloud_notm}} console. 

To create an instance on a dedicated host:
1. In the [{{site.data.keyword.cloud_notm}} console]](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Dedicated hosts**. 
2. If you want to create an instance on a specific dedicated host, on the **Dedicated hosts** tab click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the host where you want the instance created and select **New instance**.
3. If you want to create an instance on any dedicated host within a dedicated group, on the **Dedicated groups** tab click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the dedicated group where you want the instance created and select **New instance**.
4. For more information, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers). 

## Creating instances on dedicated hosts by using the CLI
{: #creating-dedicated-instance-cli}

When you have a dedicated host created, you can start provisioning virtual server instances on the dedicated host by using the command-line interface (CLI). 

### Before you begin
{: before-you-create-dh-instance-cli}

1. Make sure that you downloaded, installed, and intialized the following CLI plug-ins. For more information, see [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup). 
  * IBM Cloud CLI
  * The infrastructure-service plug-in
2. Have an [{{site.data.keyword.vpc_short}} created](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).

### Gathering information to create an instance on a dedicated host by using the CLI
{: #gather-info-dedicated-host-vsi-cli}

Ready to create an instance on a dedicated host or dedicated group? Before you can run the `ibmcloud is instances` command, you need to know the details about the instance, such as what profile or image you want to use. You also need to know the ID of the dedicated host or the dedicated group where you want to create the virtual server instance. 

Gather the following information:

|    Instance details   |  Listing options                |
| --------------------- | --------------------------------|
| Image                 | `ibmcloud is images`            | 
| Profile               | `ibmcloud is instance-profiles` |
| Key                   | `ibmcloud is keys`              | 
| VPC                   | `ibmcloud is vpcs`              | 
| Subnet                | `ibmcloud is subnets`           | 
| Zone                  | `ibmcloud is zones`             | 
| Dedicated host        | `ibmcloud is dedicated-hosts`   |  
| Dedicated group       | `ibmcloud is dedicated-host-groups`|  
{: caption="Table 1. Required instance details" caption-side="top"} 

For more information, see [Gathering information to create an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli#gather-info-to-create-virtual-servers-cli). 

### Creating an instance on a dedicated host by using the CLI
{: #create-dedicated-host-vsi-cli}

To create an instance on a dedicated group by using the CLI, use the **ibmcloud is instance-create** command. Specify the name you want to give the instance, the VPC, the zone, the profile that you want to use for the instance, the subnet, the image id, and the dedicated group where you want to provision the instance. The instance will be provisioned on an available dedicated host within the dedicated group.

The following example command creates a dedicated host with the following parameters:
* Name: `my-instance-name` 
* VPC: `r006-e49dbfc6-03b5-4609-b680-684311be5457` 
* Zone: `us-south-1`
* Profile: `mx2-2x16` 
* Subnet: `0076-2249dabc-8c71-4a54-bxy7-953701ca3999` 
* Image: `r006-72b27b5c-f4b0-48bb-b954-5becc7c1dcb8` 
* Key ID: `r006-c23faac2-5983-428c-91b8-2959620e1b96`
* Dedicated group `0076-edf611ff-0fd6-44bf-b5f3-102eeb3cf928` 
```
ibmcloud is instance-create my-instance-name r006-e49dbfc6-03b5-4609-b680-684311be5457 us-south-1 mx2-2x16 0076-2249dabc-8c71-4a54-bxy7-953701ca3999 --image-id r006-72b27b5c-f4b0-48bb-b954-5becc7c1dcb8 --key-ids r006-c23faac2-5983-428c-91b8-2959620e1b96 --dedicated-host-group 0076-edf611ff-0fd6-44bf-b5f3-102eeb3cf928
```
{:pre}

For a full list of command options, see [ibmcloud is instance-create](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-create).

## Creating instances on dedicated hosts by using the API
{: #creating-dedicated-instance-api}

The following request example creates an instance on a dedicated host with a dedicated group specified as the placement target. You can specify your desired dedicated group (or dedicated host) as the `placement_target`. When you specify a dedicated group for the `placement_target`, the instance is provisioned to an available dedicated host within the group.

```
curl -X POST \
”$vpc_api_endpoint/v1/instances?version=2020-11-17&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "primary_network_interface": {
        "name": "my-network-interface",
        "subnet": {
          "id": "0076-2249dabc-8c71-4a54-bxy7-953701ca3999"
        }
      },
      "placement_target": {
        "id": "0076-edf611ff-0fd6-44bf-b5f3-102eeb3cf928"
      },
      "name": "my-instance-2",
      "zone": {
        "name": "us-south-1"
      },
      "vpc": {
        "id": "r006-e49dbfc6-03b5-4609-b680-684311be5457"
      },
      "profile": {
        "name": "mx2-2x16"
      },
      "image": {
        "id": "r006-72b27b5c-f4b0-48bb-b954-5becc7c1dcb8"
      },
      "keys": [
        {
          "id": "r006-c23faac2-5983-428c-91b8-2959620e1b96"
        }
      ]
    }'
```
{:pre}

For more information, see [Create an instance](https://cloud.ibm.com/apidocs/vpc#create-instance). 

For details about the `$vpc_api_endpoint` and `$iam_token` variables, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](https://cloud.ibm.com/apidocs/vpc#about-vpc-api).
