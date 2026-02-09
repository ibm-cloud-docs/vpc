---

copyright:
  years: 2020, 2026
lastupdated: "2026-02-09"

keywords: dedicated host, dedicated host group

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Creating dedicated hosts and groups
{: #creating-dedicated-hosts-instances}

You can create one or more dedicated hosts with associated dedicated host groups in your {{site.data.keyword.cloud}} VPC by using the {{site.data.keyword.cloud_notm}} console, the CLI, or the API. Dedicated Host for VPC is fully integrated into {{site.data.keyword.cloud_notm}}.
{: shortdesc}

 For IBM Z or LinuxONE (s390x processor architecture), dedicated hosts are only supported in the Spain (Madrid) and US South (Dallas) region for {{site.data.keyword.waziaas_full_notm}}.
{: note}


## Dedicated hosts
{: #about-dedicated-hosts}

You can create a *dedicated host* to carve out a single-tenant compute node, free from users outside of your organization. Within that dedicated space, you can create virtual server instances according to your needs. Additionally, you can create dedicated host groups that contain dedicated hosts for a specific purpose. Because a dedicated host is a single-tenant space, only users within your account that have the required permissions can create instances on the host.

When you create a dedicated host, you are billed by the usage of the host on an hourly basis. You are not billed for the vCPU and RAM associated with instances that are running on the host.

When you provision a dedicated host, the host is owned and managed by {{site.data.keyword.IBM_notm}}. For more information about your responsibilities for managing virtual server instances provisioned on the host, see [Understanding your responsibilities when using Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc).

To create a dedicated host and associated dedicated host group you must have a minimum role of Editor assigned in IBM {{site.data.keyword.iamshort}} (IAM) access policies. Within VPC Infrastructure Services access, Dedicated Host for VPC access is given as a set of permissions, including access to dedicated hosts, dedicated host groups, and to provision instances on dedicated hosts. If you want to assign granular access for users to a specific resource, such as a dedicated host group, you can create a separate [resource group](/docs/account?topic=account-rgs) for that resource with a separate access policy and minimum access role assigned.  For more information about dedicated host permissions, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=ui).

You can also view usage information for your dedicated hosts including estimated charges and resources that are used. For more information, see [Viewing your usage](/docs/account?topic=account-viewingusage).

## Dedicated host groups
{: #dedicated-groups}

A *dedicated host group* is a collection of dedicated hosts in a single region and zone. When you create a dedicated host, you must assign it to a dedicated host group. A dedicated host can be a member of only one group. 

You can create dedicated host groups that contain dedicated hosts for a distinct function. For example, if you have multiple business units within your organization, you might want to separate the physical compute infrastructure that is used by each business. If you want to assign compute resources to be used by only one business group within your organization, you can create a group with dedicated hosts for that unique goal.

When you provision an instance, you can provision it to either a dedicated host or to a dedicated host group.

## Creating dedicated hosts and groups by using the console
{: #creating-dedicated-host}
{: ui}

You can create one or more dedicated hosts in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console.

Before you can create a dedicated host, you need to [create an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{: important}

If you do not already have a dedicated group, you must create it as part of this task. The profile family and class of the dedicated host and dedicated group must be the same. The profile that you select for the dedicated host when you create it determines the profiles that can be used for the dedicated group and for provisioning instances to hosts in the group. For example, if you select a memory profile for the dedicated host, the associated dedicated group and instances provisioned on hosts in the group must also be provisioned with memory profiles.

To create a dedicated host:
1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Dedicated hosts**.
2. Click **Create** and enter the information in Table 1 on the New dedicated host for VPC page.
3. Click **Create dedicated host** when you are ready to provision.

| Field | Value |
|-------|-------|
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want your dedicated host to be created. |
| Name  | A unique resource name within the region is required for your dedicated host. If you don't specify a name, a name is generated and assigned to your dedicated host. |
| Resource group | Select the resource group that contains the account resources and users that you want to be able to access the dedicated host. For more information about resource groups, see [Managing resource groups](/docs/account?topic=account-rgs).  |
| Tags | You can assign a label to this resource so that you can easily filter resources in your resource list. |
| Instance placement | Select whether you want to enable the placement of instances on this dedicated host. The default value of instance placement is set to *On*. If you set the instance placement value to *Off*, no instances can be created on this dedicated host until the value is changed to *On*.|
| Profile | Click **Change profile** to select a profile that defines the vCPU and memory for the dedicated host. On the dedicated host profile page, you can select the architecture to use for your dedicated host; x86 architecture is selected by default. The profile family that you select for the dedicated host determines the profile family that must be used when you provision virtual server instances on the host. If you choose a memory profile for your dedicated host, all instances that are provisioned on the host must also be created with a memory profile. If you choose an [instance storage](/docs/vpc?topic=vpc-instance-storage) profile for the dedicated host (a profile that includes *d* in the prefix, such as *mx2d*), all instances that are provisioned on the dedicated host must be provisioned with an instance storage profile in the corresponding family. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles).  |
| Dedicated group | Select the dedicated group where you want this dedicated host created. Or you can [create a new dedicated group](#creating-dedicated-groups). |
{: caption="Dedicated host provisioning selections" caption-side="bottom"}

### Creating dedicated groups
{: #creating-dedicated-groups}

If you don't have a dedicated group, or if you want to create a new dedicated group, you can create one as part of the dedicated host creation process.

1. On the **Dedicated host for VPC** page, click **Create dedicated group** near the bottom of the page.
2. Enter the information in Table 2.
3. Click **Create** when your dedicated group information is complete.

| Field | Value |
|-------|-------|
| Name | Specify a unique resource name within the region for your dedicated group. |
| Resource group | Select the resource group that contains the account resources and users that you want to be able to access the group. For more information about resource groups, see [Managing resource groups](/docs/account?topic=account-rgs).  |
{: caption="Dedicated group creation selections" caption-side="bottom"}

## Creating dedicated hosts and groups by using the CLI
{: #creating-dedicated-host-CLI}
{: cli}

You can create one or more dedicated groups and hosts in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

### Before you begin
{: #before-you-create-group-cli}

1. Make sure that you downloaded, installed, and intialized the following CLI plug-ins. For more information, see [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
* IBM Cloud CLI
* The vpc-infrastructure plugin

2. Have an [{{site.data.keyword.vpc_short}} created](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli).

### Gathering information to create a dedicated group by using the CLI
{: #gather-info-dedicated-group-cli}



Ready to create a dedicated host group? Before you can run the `ibmcloud is dedicated-host-group-create` command, you need to know the zone where you want to create it.

Gather the following required information:

| Dedicated group details | Options                          |
|-------------------------|----------------------------------|
| **Zone**                  | Name of the data center within the region where you want to create the group.        |
| **Family**                | The profile family that you want to assign to the group, such as Memory, Balanced, or Compute. |
| **Class**                 | The profile class that you want to assign to the group, such as mx2. |
| **Name**                  | Unique name for your new group. |
{: caption="Required host group details" caption-side="bottom"}

Use the following commands to determine the required information for creating a new group.

1. List the regions associated with your account.

   ```sh
   ibmcloud is regions
   ```
   {: pre}

   See the following example.

   ```sh
   $ ibmcloud is regions
   Listing regions under account Test Account as user test.user@ibm.com...
   Name       Endpoint                              Status
   au-syd     https://au-syd.iaas.cloud.ibm.com     available
   br-sao     https://br-sao.iaas.cloud.ibm.com     available
   ca-tor     https://ca-tor.iaas.cloud.ibm.com     available
   eu-de      https://eu-de.iaas.cloud.ibm.com      available
   eu-es      https://eu-es.iaas.cloud.ibm.com      available
   eu-gb      https://eu-gb.iaas.cloud.ibm.com      available
   jp-osa     https://jp-osa.iaas.cloud.ibm.com     available
   jp-tok     https://jp-tok.iaas.cloud.ibm.com     available
   us-east    https://us-east.iaas.cloud.ibm.com    available
   us-south   https://us-south.iaas.cloud.ibm.com   available
   ```
   {: screen}

1. Switch to your target region.

   ```sh
   ibmcloud target -r <region-name>
   ```
   {: pre}

1. List the zones associated with the target region.

   ```sh
   ibmcloud is zones
   ```
   {: pre}

   In the following example, the command is run in the `us-south` region and the output shows the available zones in the region.

   ```sh
   $ ibmcloud is zones
   Listing zones in target region us-south under account Test Account as user test.user@ibm.com...
   Name         Region     Status
   us-south-1   us-south   available
   us-south-2   us-south   available
   us-south-3   us-south   available
   ```
   {: screen}

1. List the profiles that are available for creating a dedicated host to determine what profile family and class you want to assign to the dedicated host group. The family and class that you assign to the group when it is created determines the profiles that can be used to provision dedicated hosts and instances in the group. All dedicated hosts and virtual server instances that are provisioned to the dedicated host group must be from the same family and class of profiles. For more information, see [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles).

   ```sh
   ibmcloud is dedicated-host-profiles
   ```
   {: pre}

   For this example, you'd see a response similar to the following output. Check the Family and Class columns that are associated with the dedicated host profile that you want to provision in the dedicated host group. If you want to provision a memory profile for your host, note the associated family, `memory`, and associated class, `mx2`.

   ```sh
   Name                 Architecture   CPU Socket Count   vCPUs   Memory   Family          Class
   cx2-host-152x304     amd64          4                  152     304      compute         cx2
   bx2-host-152x608     amd64          4                  152     608      balanced        bx2
   mx2-host-152x1216    amd64          4                  152     1216     memory          mx2
   ```
   {: screen}

### Creating a dedicated host group by using the CLI
{: #creating-dedicated-group-cli}

You can create one or more dedicated host groups in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To create a dedicated host group by using the CLI, use the **ibmcloud is dedicated-host-group-create** command. Specify the name for your dedicated host group and the zone name where you want the group created.

The following example creates a dedicated host group named `my-host-group` in the `us-south-1` zone and assigns the `memory` profile family and `mx2` class.

```sh
ibmcloud is dedicated-host-group-create --zone us-south-1 --family memory --class mx2 --name my-host-group
```
{: pre}

In the output, be sure to note the ID for the dedicated host group that is created. In this example the ID for myDedicatedHostGroup is `0076-edf611ff-0fd6-44bf-b5f3-102eeb3cf928`.

For a full list of command options, see [ibmcloud is dedicated-host-group-create](/docs/vpc?topic=vpc-vpc-reference#dedicated-host-group-create).

### Gathering information to create a dedicated host by using the CLI
{: #gather-info-dedicated-host-cli}



Ready to create a dedicated host within the dedicated host group? Before you can run the `ibmcloud is dedicated-host-create` command, you need to know the dedicated host profile that you want to use for the dedicated host. The profile family that you select determines the profile family that must be used when you provision virtual server instances on the host.

List the profiles that are available for creating a dedicated host.

```sh
ibmcloud is dedicated-host-profiles
```
{: pre}

For this example, you'd see a response similar to the following output. The profile that you select must be from the same family and class as your dedicated host group where you plan to provision the host.

```sh
Name                 Architecture   CPU Socket Count   vCPUs   Memory   Family          Class
cx2-host-152x304     amd64          4                  152     304      compute         cx2
bx2-host-152x608     amd64          4                  152     608      balanced        bx2
mx2-host-152x1216    amd64          4                  152     1216     memory          mx2
```
{: screen}

### Creating a dedicated host by using the CLI
{: #creating-dedicated-host-cli}

You can create one or more dedicated hosts in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To create a dedicated host by using the CLI, use the **ibmcloud is dedicated-host-create** command. Specify the profile that you want to use for the dedicated host and the dedicated host group that you want the host to be a part of.

The following example creates a dedicated host named `my-host` with the memory profile `mx2-host-152x1216` in the dedicated host group `0076-edf611ff-0fd6-44bf-b5f3-102eeb3cf928`.

```sh
ibmcloud is dedicated-host-create --profile mx2-host-152x1216 --dhg 0076-edf611ff-0fd6-44bf-b5f3-102eeb3cf928 --name my-host
```
{: pre}

For a full list of command options, see [ibmcloud is dedicated-host-create](/docs/vpc?topic=vpc-vpc-reference#dedicated-host-create).


## Creating dedicated hosts and groups by using the API
{: #creating-dedicated-host-API}
{: api}

When you created a dedicated host by using the API, you can create the host in a specific dedicated host group, or you can simply specify a zone (such as us-south-1) where you want it created. If you specify a zone instead of a group, a new group is created automatically for your dedicated host.

The following request example creates a dedicated host in a specific group.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/dedicated_hosts?version=2020-11-17&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-host",
      "group": {
        "id": "0076-edf611ff-0fd6-44bf-b5f3-102eeb3cf928"
      },
      "profile": {
        "name": "mx2-host-152x1216"
      }
    }'
```
{: codeblock}


The dedicated group information is optional. If you want a dedicated group to be created for you automatically, you can omit the group information and add a zone instead.

For more information, see [Create a dedicated host](/apidocs/vpc/latest#create-dedicated-host).

For details about the `$vpc_api_endpoint` and `$iam_token` variables, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](/apidocs/vpc/latest#about-vpc-api).

## Next steps
{: #dh-next-steps}

When you have a dedicated host created, you can start provisioning virtual server instances on the dedicated host. For more information, see [Creating instances on dedicated hosts](/docs/vpc?topic=vpc-creating-instance-on-dh).
