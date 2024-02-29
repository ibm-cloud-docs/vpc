---

copyright:
  years: 2019
lastupdated: "2020-10-30"

keywords: virtual private endpoint, IAM, VPE endpoint gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing access for virtual private endpoints
{: #vpe-iam}

Virtual private endpoint gateways use {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) platform access roles to manage access to the service's resources. IAM access roles allow account administrators to assign different levels of permission for calling the service's APIs and accessing the UI.
{: shortdesc}

The following table provides example actions that you can take against the VPE service and its resources, depending on the user's assigned roles.

## Platform-access roles
{: #vpe-platform-access-roles}

VPE supports Administrator, Editor, Operator, and Viewer platform-access roles. The following table describes IAM authorization actions for VPEs.

| Role | Description of Actions | Example Actions |
|---------------|------------------|--------------------|
| Administrator | All Operator actions, plus actions that modify the state of an endpoint gateway, such as create, delete, and update. | * List all endpoint gateways.  \n * List a specific endpoint gateway and view its details.  \n * Create an endpoint gateway and bind a reserved IP to it.[^A]  \n * Update the name of an existing endpoint gateway.  \n * Delete an endpoint gateway and unbind its reserved IP address.  \n * Bind a reserved IP address to an endpoint gateway.[^B] |
| Editor | All Operator actions, plus actions that modify the state of an endpoint gateway, such as create, delete, and update. | * List all endpoint gateways.  \n * List a specific endpoint gateway and view its details.  \n * Create an endpoint gateway and bind a reserved IP to it.[^C]  \n * Update the name of an existing endpoint gateway.  \n * Delete an endpoint gateway and unbind its reserved IP address.  \n * Bind a reserved IP address to an endpoint gateway.[^D] |
| Operator | All Viewer actions, plus the action to bind a reserved IP to an endpoint gateway. | * List all endpoint gateways.  \n * List a specific endpoint gateway and view its details.  \n * Bind a reserved IP address to an endpoint gateway.[^E] |
| Viewer | Performs actions that don't change the state of the endpoint gateway. | * List all endpoint gateways.  \n * List a specific endpoint gateway and view its details.|
{: caption="Table 1. IAM platform-access user role and actions" caption-side="bottom"}

[^A]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when an endpoint gateway is created. To create an endpoint gateway without binding a reserved IP, a user requires only Administrator or Editor privileges.

[^B]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when an endpoint gateway is created. To create an endpoint gateway without binding a reserved IP, a user requires only Administrator or Editor privileges.

[^C]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when an endpoint gateway is created. To create an endpoint gateway without binding a reserved IP, a user requires only Administrator or Editor privileges.

[^D]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when an endpoint gateway is created. To create an endpoint gateway without binding a reserved IP, a user requires only Administrator or Editor privileges.

[^E]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when an endpoint gateway is created. To create an endpoint gateway without binding a reserved IP, a user requires only Administrator or Editor privileges.

For more information about assigning user roles in the console, see [Managing access to resources](/docs/account?topic=account-assign-access-resources).

## Viewing VPE resources in the Resource list
{: #viewing-vpe-resources-in-resource-list}

To view VPE resources in the {{site.data.keyword.cloud_notm}} resource list, users need an IAM policy for the VPE service. VPE resource-type specific policies aren't sufficient.
