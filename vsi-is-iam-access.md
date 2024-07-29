---

copyright:
  years: 2017, 2024
lastupdated: "2024-07-29"

keywords: IAM access for vpc infrastructure services, permissions for vpc infrastructure services, identity and access management for vpc infrastructure services, roles for vpc infrastructure services, actions for vpc infrastructure services, assigning access for vpc infrastructure services

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing IAM access for VPC Infrastructure Services
{: #iam-getting-started}

Access to {{site.data.keyword.vpc_full}} service instances for users in your account is controlled by {{site.data.keyword.iamshort}} (IAM). Every user that accesses the `VPC Infrastructure Services` service in your account must be assigned an access policy with an IAM role. Review the following roles, actions, and more to help determine the best way to assign access to `VPC Infrastructure Services`.
{: shortdesc}

The access policy that you assign users in your account determines what actions a user can perform within the context of the service or specific instance that you select. The allowable actions are customized and defined by the `VPC Infrastructure Services` as operations that you are allowed to perform on the service. Each action is mapped to an IAM platform or service role that you can assign to a user.

If a specific role and its actions don't fit the use case that you're looking to address, you can [create a custom role](/docs/account?topic=account-custom-roles&interface=ui#custom-access-roles) and pick the actions to include.
{: tip}

IAM access policies enable access to grant at different levels. The following are some options that are included:

* Access across all instances of the service in your account
* Access to an individual service instance in your account
* Access to a specific resource within an instance, such as `vpcId` or `instanceId`.

The following table lists the VPC resource attributes. For more information, see [VPC resource attributes](/docs/vpc?topic=vpc-resource-attributes&interface=cli).

{{site.data.content.vpc-resource-attributes-table}}

Review the following tables that outline what types of tasks each role allows when you're working with the `VPC Infrastructure Services` service. Platform management roles enable users to perform tasks on service resources at the platform level. For example, assign user access to the service, create or delete instances, and bind instances to applications. Service access roles enable user access to `VPC Infrastructure Services` and the ability to call the `VPC Infrastructure Services` API.

| Platform role |  Description of actions |
|---------------|-------------------------|
| Viewer                 | You can view service instances, but you can't modify them. |
| Operator               | You can perform platform actions that are required to configure and operate service instances, such as viewing a service dashboard. |
| Editor                 | You can perform all platform actions except for managing the account and assigning access policies. |
| Administrator          | You can perform all platform actions based on the resource this role is assigned, including assigning access policies to other users.|
{: row-headers}
{: class="simple-tab-table"}
{: caption="Table 2. IAM VPC Infrastructure Services platform roles" caption-side="bottom"}
{: #iamrolesplatformvpcinfrastructureservices}
{: tab-title="VPC Infrastructure Services Platform roles"}
{: tab-group="IAMVPC}

| Service role |  Description of actions |
|--------------|------------------------|
| Reader         | You can perform read-only actions within a service, such as viewing service-specific resources. |
| Writer         | You have permissions beyond the reader role, including creating and editing service-specific resources. |
| Manager        | You have permissions beyond the writer role that allows you to complete privileged actions, as defined by the service. In addition, you can create and edit service-specific resources.|
| VPNClient | You need to select only this role if you need to assign access to VPN clients that have user ID and passcode authentication configured. If you need to configure the user ID and passcode authentication, see [Configuring user IDs and passcodes](/docs/vpc?topic=vpc-client-to-site-authentication#client-to-site-configuration-passcode). |
| Bare Metal Advanced Network Operator | You have access to modify IP spoofing and infrastructure NAT on bare metal interfaces. |
| Bare Metal Console Admin | You can access the bare metal server console. |
| IP Spoofing Operator | You can enable or disable the IP spoofing check on virtual server instances. Grant this role only if necessary. |
| Console Administrator | You can access the virtual server instance console. This role provides only console access and must be combined with another role that has operator access to the virtual server, such as Operator, Editor, or Administrator. |
{: row-headers}
{: class="simple-tab-table"}
{: caption="Table 2. IAM VPC Infrastructure service access roles" caption-side="bottom"}
{: #iamrolesvpcinfrastructureservice}
{: tab-title="VPC Infrastructure Service roles"}
{: tab-group="IAMVPC"}

For more information about the exact actions that are mapped to each role, see [`Infrastructure Services`](/docs/account?topic=account-iam-service-roles-actions#is-roles) on the IAM roles and actions page.

The following links take you directly to the specific infrastructure service on the IAM roles and actions page.

## Network IAM roles and actions
{: #network-iam-roles-actions}

- [Floating IP for VPC](/docs/account?topic=account-iam-service-roles-actions#is.floating-ip-roles)
- [Flow Logs for VPC](/docs/account?topic=account-iam-service-roles-actions#is.flow-log-collector-roles)
- [Load Balancer for VPC](/docs/account?topic=account-iam-service-roles-actions#is.load-balancer-roles)
- [Network ACL](/docs/account?topic=account-iam-service-roles-actions#is.network-acl-roles)
- [Public Gateway](/docs/account?topic=account-iam-service-roles-actions#is.public-gateway-roles)
- [Routing tables](/docs/account?topic=account-iam-service-roles-actions&q=virtual+private+cloud&tags=account#is.vpc-roles) (See Virtual Private Cloud section > Actions tab)
- [Security Group for VPC](/docs/account?topic=account-iam-service-roles-actions#is.security-group-roles)
- [Subnet](/docs/account?topic=account-iam-service-roles-actions#is.subnet-roles)
- [Virtual Private Cloud](/docs/account?topic=account-iam-service-roles-actions#is.vpc-roles)
- [Virtual Private Endpoint for VPC](/docs/account?topic=account-iam-service-roles-actions#is.endpoint-gateway-roles)
- [VPN for VPC](/docs/account?topic=account-iam-service-roles-actions#is.vpn-roles)
- [VPN Client for VPC](/docs/account?topic=account-iam-service-roles-actions#is.vpn-server-roles)
- [Virtual Network Interface](/docs/account?topic=account-iam-service-roles-actions#is.virtual-network-interface-roles)

## Compute IAM roles and actions
{: #compute-iam-roles-actions}

- [Auto Scale for VPC](/docs/account?topic=account-iam-service-roles-actions#is.instance-group-roles)
- [Bare Metal Server for VPC](/docs/account?topic=account-iam-service-roles-actions#is.bare-metal-server-roles)
- [Dedicated Host for VPC](/docs/account?topic=account-iam-service-roles-actions#is.dedicated-host-roles)
- [Image Service for VPC](/docs/account?topic=account-iam-service-roles-actions#is.image-roles)
- [Placement Groups for VPC](/docs/account?topic=account-iam-service-roles-actions#is.placement-group-roles)
- [SSH Key for VPC](/docs/account?topic=account-iam-service-roles-actions#is.key-roles)
- [Virtual Server for VPC](/docs/account?topic=account-iam-service-roles-actions#is.instance-roles)
- [Reservations for VPC](/docs/account?topic=account-iam-service-roles-actions#is.reservation-roles)

## Storage IAM roles and actions
{: #storage-iam-roles-actions}

- [Block Storage for VPC](/docs/account?topic=account-iam-service-roles-actions#is.volume-roles)
- [Block Storage Snapshots for VPC](/docs/account?topic=account-iam-service-roles-actions#is.snapshot-roles)
- [Block Storage Multi Volume Snapshots for VPC](/docs/account?topic=account-iam-service-roles-actions#is.snapshot-consistency-group-roles)
- [Backup as a Service for VPC](/docs/account?topic=account-iam-service-roles-actions#is.backup-policy-roles)
- [File Storage for VPC](/docs/account?topic=account-iam-service-roles-actions#is.share-roles)

Some VPC tasks require authorizations for multiple IAM actions. For example, create virtual server instance not only requires `is.instance.instance.create`, it also requires `is.vpc.vpc.operate`, `is.subnet.subnet.operate`, `is.security-group.security-group.operate`, and `is.volume.volume.create`. Additional conditional actions might be required. For example, if you provision an instance on a dedicated host, you need `is.dedicated-host.dedicated-host-group.operate` and `is.dedicated-host.dedicated-host.operate`. The Virtual Private Cloud API reference includes an Authorization section for each API call, for example, [Create an instance](/apidocs/vpc/latest#create-instance).
{: note}

## Assigning access to VPC Infrastructure Services in the console
{: #vpc-infrastructure-services-assign-access-console}
{: ui}

You have two common ways to assign access in the console:

* Access policies per user. You can manage access policies per user from the **Manage** > **Access (IAM)** > **Users** page in the console. For more information about the steps to assign IAM access, see [Managing access to resources](/docs/account?topic=account-assign-access-resources&interface=ui#access-resources-console).
* Access groups. Access groups are used to streamline access management by assigning access to a group once, then you can add or remove users as needed from the group to control their access. You manage access groups and their access from the **Manage** > **Access (IAM)** > **Access groups** page in the console. For more information, see [Assigning access to a group in the console](/docs/account?topic=account-groups&interface=ui#access_ag).

## Assigning access to VPC Infrastructure Services in the CLI
{: #vpc-infrastructure-services-assign-access-cli}
{: cli}

For step-by-step instructions for assigning, removing, and reviewing access, see [Assigning access ro resources by using the CLI](/docs/account?topic=account-assign-access-resources&interface=cli#access-resources-cli).

The following example shows a command for assigning the `Viewer` role for `VPC Infrastructure Services`:

Use `is` for the service name. Also, use quotations around role names that are more than one word as shown in the example here.
{: tip}

```bash
ibmcloud iam user-policy-create USER@EXAMPLE.COM --service-name is --roles "Viewer"
```
{: pre}

## Assigning access to VPC Infrastructure Services by using the API
{: #vpc-infrastructure-services-assign-access-api}
{: api}

For step-by-step instructions for assigning, removing, and reviewing access, see [Assigning access to resources by using the API](/docs/account?topic=account-assign-access-resources&interface=api) or [Create a policy API](/apidocs/iam-policy-management#create-policy). Role cloud resource names (CRN) in the following table are used to assign access with the API.

| Role name | Role CRN |
|---------------|-----------------|
| Viewer                 | `crn:v1:bluemix:public:iam::::role:Viewer`        |
| Operator               | `crn:v1:bluemix:public:iam::::role:Operator`      |
| Editor                 | `crn:v1:bluemix:public:iam::::role:Editor`        |
| Administrator          | `crn:v1:bluemix:public:iam::::role:Administrator` |
| Reader         | `crn:v1:bluemix:public:iam::::serviceRole:Reader`        |
| Writer         | `crn:v1:bluemix:public:iam::::serviceRole:Writer`        |
| Manager        | `crn:v1:bluemix:public:iam::::serviceRole:Manager`       |
| VPNClient      | `crn:v1:bluemix:public:iam::::serviceRole:VPNClient` |
| Bare Metal Advanced Network Operator | `crn:v1:bluemix:public:iam::::serviceRole:BareMetalAdvancedNetworkOperator` |
| Bare Metal Console Admin | `crn:v1:bluemix:public:iam::::serviceRole:BareMetalConsoleAdmin` |
| IP Spoofing Operator | `crn:v1:bluemix:public:iam::::serviceRole:IPSpoofingOperator` |
| Console Administrator | `crn:v1:bluemix:public:iam::::serviceRole:VirtualServerConsoleAdmin` |
{: caption="Table 2. VPC Infrastructure Services role ID values for API use" caption-side="bottom"}

The following example is for assigning the `Viewer` role for `VPC Infrastructure Services`:

Use `is` for the service name and refer to the Role ID values table to make sure that you're using the correct value for the CRN.
{: tip}

```curl
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -d '{
  "type": "access",
  "description": "Object Writer role for Cloud Object Storage",
  "subjects": [
    {
      "attributes": [
        {
          "name": "iam_id",
          "value": "IBMid-123453user"
        }
      ]
    }'
  ],
  "roles":[
    {
      "role_id": "crn:v1:bluemix:public:iam::::role:Viewer"
    }
  ],
  "resources":[
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "is"
        }
      ]
    }
  ]
}
```
{: curl}
{: codeblock}

```java
SubjectAttribute subjectAttribute = new SubjectAttribute.Builder()
      .name("iam_id")
      .value("IBMid-123453user")
      .build();

PolicySubject policySubjects = new PolicySubject.Builder()
      .addAttributes(subjectAttribute)
      .build();

PolicyRole policyRoles = new PolicyRole.Builder()
      .roleId("crn:v1:bluemix:public:iam::::role:Viewer")
      .build();

ResourceAttribute accountIdResourceAttribute = new ResourceAttribute.Builder()
      .name("accountId")
      .value("ACCOUNT_ID")
      .operator("stringEquals")
      .build();

ResourceAttribute serviceNameResourceAttribute = new ResourceAttribute.Builder()
      .name("serviceName")
      .value("is")
      .operator("stringEquals")
      .build();

PolicyResource policyResources = new PolicyResource.Builder()
      .addAttributes(accountIdResourceAttribute)
      .addAttributes(serviceNameResourceAttribute)
      .build();

CreatePolicyOptions options = new CreatePolicyOptions.Builder()
      .type("access")
      .subjects(Arrays.asList(policySubjects))
      .roles(Arrays.asList(policyRoles))
      .resources(Arrays.asList(policyResources))
      .build();

Response<Policy> response = service.createPolicy(options).execute();
Policy policy = response.getResult();

System.out.println(policy);
```
{: java}
{: codeblock}

```javascript
const policySubjects = [
  {
    attributes: [
      {
        name: 'iam_id',
        value: 'IBMid-123453user',
      },
    ],
  },
];
const policyRoles = [
  {
    role_id: 'crn:v1:bluemix:public:iam::::role:Viewer',
  },
];
const accountIdResourceAttribute = {
  name: 'accountId',
  value: 'ACCOUNT_ID',
  operator: 'stringEquals',
};
const serviceNameResourceAttribute = {
  name: 'serviceName',
  value: 'is',
  operator: 'stringEquals',
};
const policyResources = [
  {
    attributes: [accountIdResourceAttribute, serviceNameResourceAttribute]
  },
];
const params = {
  type: 'access',
  subjects: policySubjects,
  roles: policyRoles,
  resources: policyResources,
};

iamPolicyManagementService.createPolicy(params)
  .then(res => {
    examplePolicyId = res.result.id;
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: javascript}
{: codeblock}

```python
policy_subjects = PolicySubject(
  attributes=[SubjectAttribute(name='iam_id', value='IBMid-123453user')])
policy_roles = PolicyRole(
  role_id='crn:v1:bluemix:public:iam::::role:Viewer')
account_id_resource_attribute = ResourceAttribute(
  name='accountId', value='ACCOUNT_ID')
service_name_resource_attribute = ResourceAttribute(
  name='serviceName', value='is')
policy_resources = PolicyResource(
  attributes=[account_id_resource_attribute,
        service_name_resource_attribute])

policy = iam_policy_management_service.create_policy(
  type='access',
  subjects=[policy_subjects],
  roles=[policy_roles],
  resources=[policy_resources]
).get_result()

print(json.dumps(policy, indent=2))
```
{: python}
{: codeblock}

```go
subjectAttribute := &iampolicymanagementv1.SubjectAttribute{
  Name:  core.StringPtr("iam_id"),
  Value: core.StringPtr("IBMid-123453user"),
}
policySubjects := &iampolicymanagementv1.PolicySubject{
  Attributes: []iampolicymanagementv1.SubjectAttribute{*subjectAttribute},
}
policyRoles := &iampolicymanagementv1.PolicyRole{
  RoleID: core.StringPtr("crn:v1:bluemix:public:iam::::role:Viewer"),
}
accountIDResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("accountId"),
  Value:    core.StringPtr("ACCOUNT_ID"),
  Operator: core.StringPtr("stringEquals"),
}
serviceNameResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("serviceName"),
  Value:    core.StringPtr("is"),
  Operator: core.StringPtr("stringEquals"),
}
policyResources := &iampolicymanagementv1.PolicyResource{
  Attributes: []iampolicymanagementv1.ResourceAttribute{
    *accountIDResourceAttribute, *serviceNameResourceAttribute}
}

options := iamPolicyManagementService.NewCreatePolicyOptions(
  "access",
  []iampolicymanagementv1.PolicySubject{*policySubjects},
  []iampolicymanagementv1.PolicyRole{*policyRoles},
  []iampolicymanagementv1.PolicyResource{*policyResources},
)

policy, response, err := iamPolicyManagementService.CreatePolicy(options)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policy, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}

## Assigning access to VPC Infrastructure Services by using Terraform
{: #vpc-infrastructure-services-assign-access-terraform}
{: terraform}



The following example is for assigning the `Viewer` role for `VPC Infrastructure Services`:

Use `is` for the service name.
{: tip}



```terraform
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@example.com"
  roles  = ["Viewer"]

  resources {
    service = "is"
  }
}
```
{: codeblock}

For more information, see [ibm_iam_user_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_user_policy){: external}.

### Tips
{: #iam-tips}

- Access to a container resource doesn't automatically grant access to its subresources. For example, granting access to a VPC doesn't grant access to subnets in that VPC.
- Similarly, access to a subresource does not grant access to its container resource. For example, granting access to a subnet doesn't grant access to that subnet's VPC.
- In general, to change the relationship between multiple resources, the user must have access to each resource. For example, to attach a network interface to a security group, the user must have access to both the network interface and the security group. 

For more information about assigning user roles in the UI, see [Managing user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).

You can also assign user roles by using {{site.data.keyword.cloud}} Command Line Interface (CLI). You can select resources by using resource attributes. For more information, see [VPC resource attributes](/docs/vpc?topic=vpc-resource-attributes).

## Resource groups
{: #iam-resource-groups}

A _resource group_ is a collection of resources, such as an entire VPC or a single subnet that are associated for establishing authorization and usage. You can think of a resource group as a collection of infrastructure resources that might be used by a project, a department, or a team.

Large enterprises might divide a VPC into various resource groups, whereas smaller companies might need only one resource group because all team members have access to the entire VPC. If you are familiar with _OpenStack_, a resource group is similar in concept to a _Project_ in _OpenStack Keystone_.

Assignment of a resource to a resource group can be done only when the resource is created. Resources can't change resource groups after they are created.

If you want to use multiple resource groups, it’s good to plan for how you want to assign the resources and the users in your organization to each resource group.
{: tip}

For more information about resources group, see [Resource groups](/docs/account?topic=account-rgs).

## Access management tags
{: #iam-access-management-tags}

[Access management tags](/docs/account?topic=account-access-tags-tutorial) are metadata that is added to resources to help organize access control relationships. Tags create flexible resource groupings for you to administer.

When you use tags to control access to your resources, your team's projects can grow without requiring updates to IAM policies. You can attach Access Management Tags to VPC Infrastructure Resources and you can define access level to those resources based on tags.

VPC Infrastructure resources have a complex authorization model where a single API call might check authorizations on multiple resources. For such APIs, tags must be attached to all resources that an API needs access to. For more information about required API authorizations, see the [VPC API Reference](/apidocs/vpc/latest).

If the API call returns `UNKNOWN` for a resource group name, add Viewer access to resource groups when you create the access management tags policy.
{: tip}

For more information about using access management tags, see the following resources:

* [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial) UI tutorial
* For UI, CLI, API, and Terraform information on using tags, see [Working with tags](/docs/account?topic=account-tag).

### Limitations
{: #iam-access-limitations}

1. Some gaps exist between current implementation and the API Spec, which are documented [here](/docs/vpc?topic=vpc-known-issues).

1. Instance templates and dedicated host groups don't support access management tags. As a result, you can't completely manage [Auto scale](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=cli) and [Dedicated hosts](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=cli) access by using these tags.

1. Internet Key Exchange (IKE) and IPsec policy resources that are used in [site-to-site VPNs](/docs/vpc?topic=vpc-vpn-overview) don't support access management tags.

#### UI Limitations for Assigning Service Defined roles
{: #limitations-service-defined-roles-ui}
{: ui}

Assigning Service Defined roles to an access policy with an access management tag by using the UI isn't supported. This feature is supported only through the API and CLI.

### Using API for Assigning Service Defined roles
{: #assign-service-defined-roles-api}
{: api}

Assigning Service Defined roles to an access policy with an access management tag by using the UI isn't supported. This feature is supported only through the API and CLI. Use the following example for API.

```sh
curl --location --request POST 'https://iam.cloud.ibm.com/v1/policies' \
--header 'Content-Type: application/json' \
--header 'Authorization: <your token>' \
--data-raw '{
    "description": "useful description here",
    "type": "access",
    "subjects": [
        {
            "attributes": [
                {
                    "name": "iam_id",
                    "value": "<user iam-id>"
                }
            ]
        }
    ],
    "roles": [
        {
            "role_id": "crn:v1:bluemix:public:is::::serviceRole:VPNClient"
        }
    ],
    "resources": [
        {
            "attributes": [
                {
                            "name": "serviceName",
                            "value": "is",
                            "operator": "stringEquals"
                        },
                        {
                            "name": "accountId",
                            "value": "<your account id>"
                        }
            ],
            "tags":[
                {
                    "name": "abc",
                    "value": "test"
                }
            ]
        }
    ]
}'
```
{: codeblock}

### Using CLI for Assigning Service Defined roles
{: #assign-service-defined-roles-cli}
{: cli}

Assigning Service Defined roles to an access policy with an access management tag by using the UI isn't supported. This feature is supported only through the API and CLI. Use the following example for CLI.

```sh
ic iam access-group-policy-create Developers_MyApp --roles Viewer --service-name kms --tags abc:test
```
{: pre}

## Related links
{: #iam-related-links}

For more information about IAM, resource groups, access groups, and access management tags, see the following {{site.data.keyword.cloud_notm}} topics:

* [{{site.data.keyword.cloud_notm}} IAM](/docs/account?topic=account-iamoverview)
* [Access groups](/docs/account?topic=account-cloudaccess)
