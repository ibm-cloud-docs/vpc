---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-10"

keywords: bulk provision instances, vsis, virtual server instance, bulk provision with instance group

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Bulk provisioning instances with instance groups
{: #bulk-provisioning}

You can provision many virtual server instances at the same time by using the {{site.data.keyword.cloud_notm}} command-line interface to create an instance group.
{: shortdesc}

## Before you begin
{: #before-bp-cli-tutorial}

1. Make sure that you completed the setup for your [{{site.data.keyword.cloud}} CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup) and your [{{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli).
2. Make sure that you have the required IBM {{site.data.keyword.iamshort}} (IAM) permissions to create instance group resources. For more information, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=ui).
3. Make sure that you created an [instance template](/docs/vpc?topic=vpc-create-instance-template) with the instance details that are used to provision like instances in your group.

## Creating an instance group
{: #creating-instance-group-cli-bulk}

After you created an instance template, you can proceed with creating an instance group. When you create the instance group, you can specify 0-1000 for the membership count. The membership count determines how many instances are provisioned in the instance group.

If you want to include a load balancer for your instance group to balance incoming requests across instances, you must
create the load balancer before you create the instance group. For more information, see [About application load balancers](/docs/vpc?topic=vpc-load-balancers). If you attach a load balancer to your instance group, you are limited to a membership count of 50.
{: important}

Gather the following information before you run the `ibmcloud is instance-group-create` command.

| Instance group details   |       Description              |
|--------------------------|--------------------------------|
| Instance template | ID of the instance template that you want to use. You can list templates with the `ibmcloud is instance-templates` command.   |
| Subnets | Comma-separated IDs of subnets that you want to use from the `ibmcloud is subnets` command. |
| Membership count | Number of instances that you want to provision in the instance group. |
{: caption="Required instance group details" caption-side="bottom"}

After you collect these values, use them to run the `instance-group-create` command. In addition, you must specify a name for the instance group.

```sh
ibmcloud is instance-group-create INSTANCE_GROUP_NAME --instance-template INSTANCE_TEMPLATE --subnets SUBNETS [--membership-count MEMBERSHIP_COUNT] [--lb LB --lb-pool LB_POOL --application-port APPLICATION_PORT] [--vpc VPC] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

For example, if you create an instance group that is called _my-instance-group_ with instance template ID _0738-c3809e5b-8d48-4629-b258-33d5b14fa84f_ and _100_ members, your `instance-group-create` command would look like the following sample.

```sh
ibmcloud is instance-group-create my-instance-group --instance-template 0738-c3809e5b-8d48-4629-b258-33d5b14fa84f --subnets 0076-2249dabc-8c71-4a54-bxy7-953701ca3999,0767-173bn4aa-060b-47e7-am45-b3395a593897 --membership-count 100
```
{: pre}

Where:
   - `INSTANCE_GROUP_NAME` is _my-instance-group_
   - `INSTANCE_TEMPLATE` is _0738-c3809e5b-8d48-4629-b258-33d5b14fa84f_
   - `SUBNETS` are _0076-2249dabc-8c71-4a54-bxy7-953701ca3999_ and _0767-173bn4aa-060b-47e7-am45-b3395a593897_
   - `MEMBERSHIP_COUNT` is 100

For this example, the command response looks like the following example.

```sh
ID                  r006-4f7d0010-33f5-40bf-9f21-ab5bee04fd71
Name                my-instance-group
Status              healthy
Instances           100
Instance Template   0738-c3809e5b-8d48-4629-b258-33d5b14fa84f
Subnets             Name       Subnet ID
                    subnet-1   0076-2249dabc-8c71-4a54-bxy7-953701ca3999
                    subnet-2   0767-173bn4aa-060b-47e7-am45-b3395a593897

Resource group      ID                                 Name
                    11caaa983d9c4beb82690daab08717e9   Default
```
{: screen}

For more examples of the `ibmcloud is instance-group-create` command, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference#instance-group-create).

Need more help? You can run the `ibmcloud is instance-group-create --help` command to display help for creating an instance group.
{: tip}

## Next steps
{: #next-steps-bulk-provisioning}

After your instance group is created with the number of instances you need, you can use the following commands to manage the instance group and the instance memberships of the group.

| Instance group command  | Description                    |
|-------------------------|--------------------------------|
| [`ibmcloud is instance-group-update`](/docs/vpc?topic=vpc-vpc-reference#instance-group-update) | Update an instance group. |
| [`ibmcloud is instance-group-delete`](/docs/vpc?topic=vpc-vpc-reference#instance-group-delete) | Delete instance groups. |
| [`ibmcloud is instance-group-membership`](/docs/vpc?topic=vpc-vpc-reference#instance-group-membership-view) | View details of a member of an instance group. |
| [`ibmcloud is instance-group-membership-delete`](/docs/vpc?topic=vpc-vpc-reference#instance-group-membership-delete) | Delete membership for an instance group. |
| [`ibmcloud is instance-group-memberships`](/docs/vpc?topic=vpc-vpc-reference#instance-group-memberships-list) | List all members for an instance group. |
| [`ibmcloud is instance-group-memberships-delete`](/docs/vpc?topic=vpc-vpc-reference#instance-group-memberships-delete) | Delete all memberships for an instance group. |
{: caption="CLI commands for managing instance groups" caption-side="top"}

Optionally, you can set auto scaling policies for your instance group to dynamically add or remove virtual server instances from your group. For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).
