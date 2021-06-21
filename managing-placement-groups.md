---
copyright:
  years: 2021
lastupdated: "2021-06-22"

keywords: virtual private cloud, private cloud network, placement group, placement group strategy, host spread, power spread, generation 2, gen 2

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# DRAFT-Managing placement groups (Beta)
{: #managing-placement-group}

With placement groups for {{site.data.keyword.vpc_full}}, you can create policies for managing high availability workloads. You can specify a choice of placement policies for a group of provisioned instances. You can use these policies to influence the physical placement of select VPC virtual servers to meet certain workload demands. 
{:shortdesc}

Placement groups are only available to accounts with special approval to preview this beta feature. Contact your IBM Sales representative if you are interested in getting access.
{: beta}

After the placement group is created, you can assign a selected virtual server instance or a group of virtual server instances to the placement group. When you provision these instances, the instance is then placed on a computer host in the designated zone for the instance based on the placement group strategy. You can use the same placement group for instances across multiple VPCs.

Placement groups can be managed with the UI, CLI, or API. You can create extra placement groups, delete resources, and display the details of the placement groups, which are helpful when you assign placement groups to instances. Additionally, all existing placement groups can be listed and renamed if needed.

## Managing the placement group with the UI
{: #managing-placement-groups-UI}
{: ui}

You can manage a placement group in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console. 

To complete management tasks on your placement groups, complete the following steps.
1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Placement Groups**. 
2. On the Placement groups for VPC page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the placement that you want to manage. You can select 
from the following actions:

| Action | Description |
|--------|-------------|
| Create | Create a new placement group |
| Delete | If you no longer need the placement group, you can delete it to permanently remove it from your account. Before you can delete the placement group, you must first delete any virtual server instances that are assigned to the placement group.|
| Rename | Update the placement group name with a new name. |
{: caption="Table 1. Actions available for placement groups" caption-side="top"}

The VPC must exist before you create a placement group. If the VPC is not created first, you receive an error when you create the placement group and the placement group is not created.
{: important}

## Creating the placement group with the UI
{: #creating-placement-group-UI}
{: ui}

You can create a placement group in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console. A placement group must be created first before an instance can use it. After a placement group is created, an instance can be attached to it during provisioning.

Use the following steps to create a placement group:
1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Placement Groups**. 
2. Click **Create** and enter the information in Table 1 on the New placement for VPC page.

| Field | Value |
|-------|-------|
| Placement group name  | A unique resource name within the region is required for your placement group. If you don't specify a name, a name is generated and assigned to your placement group. |
| Resource group | Select the resource group that contains the account resources and users that you want to be able to access the placement group. For more information about resource groups, see [Managing resource groups](/docs/account?topic=account-rgs). |
| Region | The region of the placement group. |
| User tags | Keywords that are used for profile management. |
| Placement strategy | Host spread - All provisioned instances are placed on separate hosts that might be in the same rack.<br><br>Power spread - All provisioned instances are placed on different racks with separate power and network supplies. Each rack also contains dual power supplies from different sources.|
{: caption="Table 2. Fields required to create a placement group" caption-side="top"}

## Creating a placement group with the CLI
{: #creating-placement-group-CLI}
{: cli}

A placement group must be created first before an instance can use it. After a placement group is created, an instance can be attached to it during provisioning.

### Before you begin
{: before-you-create-placement-group-cli}
{: cli}

1. Make sure that you download, install, and initialize the following CLI plug-ins. For more information, see [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup). 
  * IBM Cloud CLI
  * The vpc-infrastructure plug-in
2. Have an [{{site.data.keyword.vpc_short}} created](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).

### Gathering information to create a placement group with the CLI
{: #gather-info-placement-group-cli}
{: cli}

Ready to create a placement group? Before you can run the `ibmcloud is placement-group-create` command, you need to know the following information. 

Gather the following required information:

| Field | Value |
|-------|-------|
| Placement group details | Options |
| **Strategy** | The placement group strategy. Either `host_spread` or `power_spread`|
| **Name** | The placement group name |
| **Resource group ID** | The ID of the resource group |
| **Resource group name** | The name of the resource group |
{: caption="Table 3. Information required to create a placement group using the CLI" caption-side="top"}

## Creating the placement group with the CLI
{: #creating-placement-group-cli-a}
{: cli}

You can create a placement group in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To create a placement group by using the CLI, use the **ibmcloud is placement-group-create** command. Specify the placement group strategy for the placement group, the name of the placement group, the resource group ID, and the name of the resource group.

The following example creates a placement group with a host spread strategy, named `MyPlacementGroup`, with resource group ID `RESOURCE_GROUP_ID`, and a resource group name of `RESOURCE_GROUP_NAME`.

```
ibmcloud is placement-group-create --strategy host_spread --name MyPlacementGroup --resource-group-id RESOURCE_GROUP_ID --resource-group-name RESOURCE_GROUP_NAME
```

## Creating a placement group with the API
{: #creating-placement-group-API}
{: api}

A placement group must be created first before an instance can use it. After a placement group is created, an instance can be attached to it during provisioning.

The following example creates a placement group that uses the host spread placement group strategy.

```
curl -X POST "$vpc_api_endpoint/v1/placement_groups?version=2021-04-20&generation=2" -H "Authorization: $iam_token" -d '{
      "name": "my-placement-group"
      "strategy": "host_spread"
    }'
```
For more information on the `host_spread` and `power_spread` strategy variables, see [Create a placement group](https://test.cloud.ibm.com/apidocs/vpc-scoped#create-placement-group) in the Virtual Private Cloud API documentation.

## Changing the placement group name with the UI
{: #changing-placement-group-name-ui}
{: ui}

You can update the name of a placement group in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console. 

To rename a placement group:
1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Placement Groups**. 
2. Select the placement group and click **Rename**.
3. Change the placement group name.

## Changing the placement group name with the CLI
{: #changing-placement-group-name-cli}
{: cli}

You can update the name of a placement group in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To update the placement group name by using the CLI, use the **ibmcloud is placement-group-update** command. Specify the placement group ID and the new name of the placement group.

The following example updates the placement group with the ID `72251a2e-d6c5-42b4-97b0-b5f8e8d1f479` to the name of `NEW_NAME`.

```
ibmcloud is placement-group-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --name NEW_NAME
```

## Changing the placement group name with the API
{: #changing-placement-group-name-API}
{: api}

You can update the name of a placement group in your {{site.data.keyword.vpc_short}} by using the API. The following example updates the placement group name with the new name `my-updated=placement-group`.

```
curl -X PATCH "$vpc_api_endpoint/v1/placement_groups/$id?version=2021-05-04&generation=2" -H "Authorization: $iam_token" -d '{
      "name": "my-updated-placement-group",
    }'
```

## Deleting a placement group with the UI
{: #deleting-placement-group-UI}
{: ui}

You can delete a placement group in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console. A placement group can't be deleted if instances are attached to it. All instances must be removed first. For more information about deleting a virtual private cloud instance and its associated resources, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting). 

Use the following steps to delete a placement group:
1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Placement Groups**. 
2. Select the placement group and click **Delete**.
3. A confirmation message is displayed. Click **Delete**.

## Deleting a placement group with the CLI
{: #deleting-placement-group-cli}
{: cli}

You can delete a placement group in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI). A placement group can't be deleted if instances are attached to it. All instances must be removed first. For more information about deleting a virtual private cloud instance and its associated resources, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting). 

To delete placement group by using the CLI, use the **ibmcloud is placement-group-delete** command. Specify the placement group ID.

The following example deletes a placement group named `PLACEMENT_GROUP`.

```
ibmcloud is placement-group-delete PLACEMENT_GROUP --output JSON
```

## Deleting a placement group with the API
{: #deleting-placement-group-API}
{: api}

A placement group can't be deleted if instances are attached to it. All instances must be removed first. For more information about deleting a virtual private cloud instance and its associated resources, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting). 

The following example deletes the placement group.

```
curl -X DELETE "$vpc_api_endpoint/v1/placement_groups/$id?version=2021-04-20&generation=2" -H "Authorization: $iam_token"
```

## Listing all placement groups with the UI
{: #listing-placement-group-UI}
{: ui}

You can generate a list of placement groups for a region. 

To display a list of all existing placement groups:
1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Placement Groups**. 
1. All existing placement groups are displayed.

## Listing all placement groups with the CLI
{: #listing-placement-group-cli}
{: cli}

You can generate a list of placement groups for a region. You can list all the existing placement group in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To list all the existing placement groups by using the CLI, use the **ibmcloud is placement-groups** command. Specify the name and ID of the resource group that you are listing placement groups.

The following example lists all the existing placement groups for the resource group ID `RESOURCE_GROUP_ID` and name `RESOURCE_GROUP`.

```
ibmcloud is placement-groups --resource-group-id RESOURCE_GROUP_ID --resource-group-name RESOURCE_GROUP_NAME --all-resource-groups --output JSON
```

## Listing all placement groups with the API
{: #listing-placement-group-API}
{: api}

You can generate a list of placement groups for a region. The following example generates a list of all placement groups.

 ```
 curl -X GET "$vpc_api_endpoint/v1/placement_groups?version=2021-04-20&generation=2" -H "Authorization: $iam_token"
 ```

## Viewing placement group details with the UI
{: #view-placement-group-details-UI}
{: ui}

You can view details about a placement group, such as the placement group name, assigned resource group, placement group ID, location, date placement group was created, and the specified placement group strategy.

To view details about a placement group, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Placement Groups**. 
1. Click the name of the placement group about which you want to view details.

## Viewing placement group details with the CLI
{: #view-placement-group-details-cli}
{: cli}

You can view details about a placement group, such as the placement group name, assigned resource group, placement group ID, location, date placement group was created, and the specified placement group strategy.

You can retrieve the details a placement group in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To retrieve the details of a placement group by using the CLI, use the **ibmcloud is placement-group** command. Specify the placement group ID.

The following example retrieves the details for a placement group named `PLACEMENT_GROUP`.

```
ibmcloud is placement-group PLACEMENT_GROUP --output JSON
```

## Viewing placement group details with the API
{: #view-placement-group-details-API}
{: api}

You can view details about a placement group, such as the placement group name, assigned resource group, placement group ID, location, date placement group was created, and the specified placement group strategy.


The following example retrieves a single placement group that is specified by the identifier in the URL.

```
curl -X GET "$vpc_api_endpoint/v1/placement_groups/$id?version=2021-04-20&generation=2" -H "Authorization: $iam_token"
```


    
