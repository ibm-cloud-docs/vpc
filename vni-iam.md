---

copyright:
  years: 2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing access for virtual network interfaces
{: #vni-iam}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

Virtual network interfaces use {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) platform access roles to manage access to the service's resources. IAM access roles allow account administrators to assign different levels of permission for calling the service's APIs and accessing the UI.

The following table provides example actions that you can take against the VNI service and its resources, depending on the user's assigned roles.

## Platform-access roles
{: #vni-platform-access-roles}

VNI supports Administrator, Editor, Operator, and Viewer platform-access roles. The following table describes IAM authorization actions for VNIs.

| Role | Description of Actions | Example Actions |
|---------------|------------------|--------------------|
| Administrator | All Operator actions, plus actions that modify the state of a network interface, such as create, delete, and update. | * List all network interfaces.  \n * List a specific network interface and view its details.  \n * Create a network interface and bind a reserved IP to it.[^A]  \n * Update the name of an existing network interface.  \n * Delete a network interface and unbind its reserved IP address.  \n * Bind a reserved IP address to a network interface.[^B] |
| Editor | All Operator actions, plus actions that modify the state of a network interface, such as create, delete, and update. | * List all network interfaces.  \n * List a specific network interface and view its details.  \n * Create a network interface and bind a reserved IP to it.[^C]  \n * Update the name of an existing network interface.  \n * Delete a network interface and unbind its reserved IP address.  \n * Bind a reserved IP address to a network interface.[^D] |
| Operator | All Viewer actions, plus the action to bind a reserved IP to a network interface. | * List all network interface.  \n * List a specific network interface and view its details.  \n * Bind a reserved IP address to a network interface.[^E] |
| Viewer | Performs actions that don't change the state of the network interface. | * List all network interfaces.  \n * List a specific network interface and view its details.|
{: caption="Table 1. IAM platform-access user role and actions" caption-side="bottom"}

[^A]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when a network interface is created. To create a network interface without binding a reserved IP, a user requires only Administrator or Editor privileges.

[^B]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when a network interface is created. To create a network interface without binding a reserved IP, a user requires only Administrator or Editor privileges.

[^C]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when a network interface is created. To create a network interface without binding a reserved IP, a user requires only Administrator or Editor privileges.

[^D]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when a network interface is created. To create a network interface without binding a reserved IP, a user requires only Administrator or Editor privileges.

[^E]:A user must have Operator privileges for a reserved IP resource to bind a reserved IP when a network interface is created. To create a network interface without binding a reserved IP, a user requires only Administrator or Editor privileges.
