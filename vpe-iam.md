---

copyright:
  years: 2019
lastupdated: "2020-10-30"

keywords: virtual private endpoint, IAM, vpe permissions, vpe, endpoint gateway

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:term: .term}
{:generic: data-hd-programlang="generic"}
{:download: .download}

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
| Administrator | All Operator actions, plus actions that modify the state of an endpoint gateway, such as create, delete, and update. | <ul><li>List all endpoint gateways.</li><li>List a specific endpoint gateway and view its details.</li><li>Create an endpoint gateway and bind a reserved IP to it.<sup>1</sup></li><li>Update the name of an existing endpoint gateway.</li><li>Delete an endpoint gateway and unbind its reserved IP address.</li><li>Bind a reserved IP address to an endpoint gateway.<sup>1</sup></li></ul> |
| Editor | All Operator actions, plus actions that modify the state of an endpoint gateway, such as create, delete, and update. | <ul><li>List all endpoint gateways. </li><li>List a specific endpoint gateway and view its details.</li><li>Create an endpoint gateway and bind a reserved IP to it.<sup>1</sup></li><li>Update the name of an existing endpoint gateway.</li><li>Delete an endpoint gateway and unbind its reserved IP address.</li><li>Bind a reserved IP address to an endpoint gateway.<sup>1</sup></li></ul> |
| Operator | All Viewer actions, plus the action to bind a reserved IP to an endpoint gateway. | <ul><li>List all endpoint gateways. </li><li>List a specific endpoint gateway and view its details.</li><li>Bind a reserved IP address to an endpoint gateway.<sup>1</sup></li></ul> |
| Viewer | Performs actions that don't change the state of the endpoint gateway. | <ul><li>List all endpoint gateways. </li><li>List a specific endpoint gateway and view its details.</li></ul> |
{: caption="Table 1. IAM platform-access user role and actions" caption-side="top"}

<sup>1</sup> A user must have Operator privileges for a reserved IP resource to bind a reserved IP when an endpoint gateway is created. To create an endpoint gateway without binding a reserved IP, a user requires only Administrator or Editor privileges.

For information about assigning user roles in the console, see [Managing access to resources](/docs/account?topic=account-assign-access-resources#iammanidaccser).

## Viewing VPE resources in the Resource list

To view VPE resources in the {{site.data.keyword.cloud_notm}} resource list, users need an IAM policy for the VPE service. VPE resource-type specific policies aren't sufficient.
