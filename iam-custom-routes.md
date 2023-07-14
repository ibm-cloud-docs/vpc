---

copyright:
  years: 2023
lastupdated: "2023-06-13"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# Updated IAM actions for VPC routing tables
{: #iam-actions-vpc-routing-tables}

As part of the IBM commitment to continually improve the security of services, the following IAM actions have been introduced to provide more granular control of VPC routing tables.
{: shortdesc}

- `is.vpc.routing-tables.list`
- `is.vpc.routing-tables.read`
- `is.vpc.routing-tables.create`
- `is.vpc.routing-tables.update`
- `is.vpc.routing-tables.delete`
- `is.vpc.routing-tables.operate`

Routing table-related operations check these actions instead of the broader VPC-wide actions. 

With this change, you can authorize users to administer VPC routing tables without needing to authorize those users to administer VPCs overall.

## Are you affected by this change?
{: #are-you-affected}

Custom roles with the following actions are affected:

- `is.vpc.vpc.read`
- `is.vpc.vpc.update`
- `is.vpc.vpc.operate`

If you want your custom role to provide the same level of access to VPC routing tables as it does today, you must add the new IAM actions to your custom role.

If the custom role was intended to provide access to routing tables specifically, then the IAM actions can be replaced depending on what access is required.

Roles provided by IBM have been automatically updated. Only custom roles require action.
{: note}

## Updating your custom roles
{: #update-custom-roles-to-get-access-for-routing-tables}

If you have identified that you are affected by this change, update your custom roles with the required IAM actions now.  

1. In the IBM Cloud console, go to **Manage** > **Access (IAM)**, and select **Roles**.
2. Find your custom roles.
3. Click a custom role's name to review what actions the selected role contains.
4. Click the Actions menu icon ![Actions menu](images/overflow.png) for the custom role you want to update and click **Edit**.
5. Find the action you want to add and click **Add** for that action.
6. To remove an action from this custom role, find the action in the **Summary** panel, and click **x**.

## Custom role changes required
{: #custom-role-changes-required}

The explicit mapping of the changes to each existing IAM action follows.

### Providing equivalent access to VPC routing tables
{: #providing-equivalent-access}

If you want the custom roles to continue to work exactly as they did before this feature release, add the following actions:

|Custom role|Updated actions|
|:----------|:----------|
|`is.vpc.vpc.read`|`is.vpc.routing-table.list`  \n `is.vpc.routing-table.read`|
|`is.vpc.vpc.update`|`is.vpc.routing-table.create`  \n `is.vpc.routing-table.update`  \n `is.vpc.routing-table.delete`|
|`is.vpc.vpc.operate`|`is.vpc.routing-table.operate`|
{: caption="Table 1. Updated IAM actions to keep custom roles" caption-side="bottom"}

### Providing granular access to only the VPC routing table
{: #providing-granular-access}

If you want the custom role to be limited to VPC routing tables (without broader access to VPCs), replace your actions as follows:

|Operation|Previous action|Current action|
|:--------|:--------------|:---------|
|List routing tables in a vpc|`is.vpc.vpc.read`|`is.vpc.routing-table.list`|
|Read a routing table in a vpc|`is.vpc.vpc.read` |`is.vpc.routing-table.read`|
|Create a new routing table in a vpc|`is.vpc.vpc.update` |`is.vpc.routing-table.create`|
|Update routing table|`is.vpc.vpc.update` |`is.vpc.routing-table.update`|
|Create, update, and delete routes in a routing table in a vpc|`is.vpc.vpc.update` |`is.vpc.routing-table.update`|
|Delete a routing table in a vpc|`is.vpc.vpc.update`|`is.vpc.routing-table.delete`|
|Operate a routing table in a vpc|`is.vpc.vpc.operate` |`is.vpc.routing-table.operate`|
{: caption="Table 2. Updated IAM granular IAM actions without administrator permission" caption-side="bottom"}
