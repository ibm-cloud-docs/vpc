---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-22"

keywords: context-based restrictions for VPC Infrastructure Services

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions
{: #cbr}

Context-based restrictions give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud}} resources based on the context of access requests. Access to VPC Infrastructure Services can be controlled with context-based restrictions and identity and access management (IAM) policies.
{: shortdesc}

These restrictions work with traditional IAM policies, which are based on identity, to provide an extra layer of protection. Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configured. Since both IAM access and context-based restrictions enforce access, context-based restrictions offer protection even in the face of compromised or mismanaged credentials. For more information, see [What are context-based restrictions](/docs/account?topic=account-context-restrictions-whatis).

A user must have the Administrator role on the VPC Infrastructure Services to create, update, or delete rules that target VPC Infrastructure Services. A user must have either the Editor or Administrator role on the context-based restrictions service to create, update, or delete network zones. A user with the Viewer role on the context-based restrictions service can add network zones to a rule only.
{: note}

The context-based restriction service generates audit logs every time when a context-based policy is enforced. For more information, see [Monitoring context-based restrictions](/docs/account?topic=account-cbr-monitor).

To get started with protecting your VPC Infrastructure Services with context-based restrictions, see the tutorial for [Leveraging context-based restrictions to secure your resources](/docs/account?topic=account-context-restrictions-tutorial).

## How VPC Infrastructure Services integrates with context-based restrictions
{: #cbr-overview}

VPC Infrastructure Services is a composite service that is made up of a number of child services. Context-based restrictions can be created for {{site.data.keyword.vpc_short}} or any of its child services; for example, [subnets](/docs/vpc?topic=vpc-about-subnets-vpc), [virtual server instances](/docs/vpc?topic=vpc-vsi_best_practices), and [Block Storage volumes](/docs/vpc?topic=vpc-creating-block-storage&interface=cli).

See the [rule creation section](#cbr-rules) for details on how you can create rules with the required attributes for each service.

## Creating network zones
{: #network-zone}

A network zone represents an allowlist of IP addresses where an access request is created. It defines a set of one or more network locations that are specified by the following attributes:

- IP addresses, which include individual addresses, ranges, or subnets
- VPCs
- Service references, which allow access from other IBM Cloud® services

### Service references
{: #service-references}

A *service reference, which is defined as part of a network zone, allows the specified service to talk to the restricted resources or APIs that are targeted by a context-based restriction policy. The following table contains the service references that need to be included when context-based restrictions are used in the context of VPC Infrastructure Services. You can also look at the [service-to-service IAM authorizations](/docs/account?topic=account-serviceauth&interface=ui) in your account when you deliberate which service references to add in your network zone. You can find service-to-service authorization in your account by going to **Manage -> Access (IAM)** and selecting the **Authorizations** tab.

| Service with context-based restrictions | Impacted service       | Service references that are required in the network zone |
|------------------------------------|--------------------------------------------|-----------------------------------------|
| Cloud Object Storage                    | * [Image service](/docs/vpc?topic=vpc-planning-custom-images) \n * [Flow logs](/docs/vpc?topic=vpc-flow-logs)                                                                                                                                                  | VPC Infrastructure Services (`is`) |
| Key Protect                             | * [Snapshot](/docs/vpc?topic=vpc-snapshots-vpc-planning) \n * [Block Storage volume](/docs/vpc?topic=vpc-creating-block-storage&interface=ui) \n * [File Storage](/docs/vpc?topic=vpc-file-storage-planning) \n * [Image service](/docs/vpc?topic=vpc-planning-custom-images)  | Cloud Block Storage (`server-protect`), \n File Storage for VPC (`is.share`)|
| Hyper Protect Crypto service            | * [Snapshot](/docs/vpc?topic=vpc-snapshots-vpc-planning) \n * [Block Storage volume](/docs/vpc?topic=vpc-creating-block-storage&interface=ui) \n * [File Storage](/docs/vpc?topic=vpc-file-storage-planning) \n * [Image service](/docs/vpc?topic=vpc-planning-custom-images) | Cloud Block Storage (`server-protect`), \n File Storage for VPC (`is.share`)|
| Virtual Server Instances                | * [Auto scale](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) | VPC Infrastructure Services (`is`) |
| Various VPC Infrastructure Services - Block Storage Volume, Security Groups, Subnets | * [IBM Kubernetes Service](/docs/containers) | IBM Kubernetes Service (`containers-kubernetes`) |
| Secrets Manager                         | * [Client-to-site VPN](/docs/vpc?topic=vpc-vpn-client-to-site-overview) \n * [Load balancers](/docs/vpc?topic=vpc-nlb-vs-elb)  | VPC Infrastructure Services (`is`) |
{: caption="Service references needed for context-based restrictions" caption-side="bottom"}

### Creating network zones by using the API
{: #network-zone-api}
{: api}

You can create network zones by using the `create-zone` command. For more information, see the [API docs](/apidocs/context-based-restrictions#create-zone).

The `serviceRef` attribute for VPC Infrastructure Services is `is`. Cloud Block Storage is `server-protect`, and File Storage for VPC is `is.share`.
{: tip}

```json
{
  "name": "Example zone 1",
  "description": "",
  "addresses": [
    {
      "type": "serviceRef",
      "ref": {
        "service_name": "is"
      }
    }
  ]
}
```
{: codeblock}

```json
{
  "name": "Example zone 1",
  "description": "",
  "addresses": [
    {
      "type": "serviceRef",
      "ref": {
        "service_name": "server-protect"
      }
    }
  ]
}
```
{: codeblock}

### Creating network zones by using the CLI
{: #network-zone-cli}
{: cli}

1. To create network zones from the CLI, [install the CBR CLI plug-in](/docs/account?topic=account-cbr-plugin&interface=ui).
1. Use the `cbr-zone-create` command to add network locations, VPCs, and service references to network zones. For more information, see the CBR [CLI reference](/docs/account?topic=account-cbr-plugin&interface=ui#cbr-zones-cli).

   To find a list of available service reference targets, run the `ibmcloud cbr service-ref-targets` [command](/docs/account?topic=account-cbr-plugin&interface=ui#cbr-cli-service-ref-targets-command). The `service_name` for *VPC Infrastructure Services* is `is`. *Cloud Block Storage* is `server-protect`, and *File Storage for VPC* is `is.share`.
   {: tip}

   The following example command adds the `is` service, referred to as *VPC Infrastructure Services* in {{site.data.keyword.cloud}} console, to a network zone.

   ```sh
   ibmcloud cbr zone-create --name example-zone-1 --description "Example zone 1" --service-ref service_name=is
   ```
   {: pre}

   The following example command adds the `server-protect` service, referred to as *Cloud Block Storage*
   in {{site.data.keyword.cloud}} console, to a network zone.

   ```sh
   ibmcloud cbr zone-create --name example-zone-1 --description "Example zone 1" --service-ref service_name=server-protect
   ```
   {: pre}

### Creating network zones from the console
{: #create-network-zone-console}
{: ui}

1. Determine which resources you want to add to your allowlist.
1. Check the [service references](#service-references) section to see whether you need to add any service reference to your network zone.
1. Follow these steps to [create context-based restrictions in the console](/docs/account?topic=account-context-restrictions-create).

## Creating rules
{: #cbr-rules}

Within VPC Infrastructure Services, each child service needs certain attributes in a context-based rule.

When you use the API, Terraform or the {{site.data.keyword.cloud_notm}} CLI to create, update, or delete {{site.data.keyword.cloud_notm}} context-based restrictions, you can specify the target VPC Infrastructure resource by using resource attributes.

See [VPC resource attributes](/docs/vpc?topic=vpc-resource-attributes) for the resource attributes that correspond with individual VPC resources.
{: tip}

You can select a resource by entering the ID of the object. Alternatively, you can use the `*` wildcard
to select all applicable resources. For example, the attribute `vpcId:*` sets the
context-based restrictions to apply to all VPCs in the account. You can also specify which resource
group the policy is applied to in the command.

### Creating rules by using the API
{: #rules-api}
{: api}

Review the following example requests to create rules. For more information about the `v1/rules`
API, see the [API docs](/apidocs/context-based-restrictions#create-rule).

After you create a rule, it might take up to 10 minutes before you can update that rule.
{: note}

The following example payload creates a rule that protects the Virtual Server Instances and allows
access only from the specified network zone through a private endpoint.

```json
{
  "contexts": [
    {
      "attributes": [
        {
          "name": "endpointType",
          "value": "private"
        },
        {
          "name": "networkZoneId",
          "value": "45ec64065b7c2bf84b3edec803335111"
        }
      ]
    }
  ],
  "resources": [
    {
      "attributes": [
        {
          "name": "resourceGroupId",
          "value": "1171749e3a8545069d04e6fca1ded18f"
        },
        {
          "name": "serviceName",
          "value": "is"
        },
        {
          "name": "instanceId",
          "value": "*"
        }
      ]
    }
  ]
}
```
{: codeblock}

### Creating rules by using the CLI
{: #rules-cli}
{: cli}

1. To create rules from the CLI, [install the CBR CLI plug-in](/docs/account?topic=account-cbr-plugin&interface=ui).
1. Use the `ibmcloud cbr rule-create` [command](/docs/account?topic=account-cbr-plugin&interface=ui#cbr-cli-rule-create-command) to create CBR rules. For more information, see the CBR [CLI reference](/docs/account?topic=account-cbr-plugin&interface=ui#cbr-zones-cli).

The following examples are enforcement rules. You can make them report-only by adding `--enforcement-mode report`.

The following example CLI command creates a context-based restriction rule for all of the virtual server instances in the current account:

```sh
ibmcloud cbr rule-create  --zone-id a7eeb5dd8e6bdce670eba1afce18e37f --description "Test CBR for VSIs" --service-name is --resource-attributes "instanceId=*"
```
{: pre}

The following example CLI command creates a context-based restriction rule for a specific block
storage volume in the current account:

```sh
ibmcloud cbr rule-create --zone-id a7eeb5dd8e6bdce670eba1afce18e37f --description "Test CBR for volume" --service-name is --resource-attributes "volumeId=UUID_OF_THE_VOLUME"
```
{: pre}

The following example CLI command creates a context-based restriction rule for all of the virtual
server instances in the current account and restricts access to private endpoints:

```sh
ibmcloud cbr rule-create --context-attributes 'endpointType=private' --zone-id a7eeb5dd8e6bdce670eba1afce18e37f --description "Test CBR for VSIs" --service-name is --resource-attributes "instanceId=*"
```
{: pre}

The following example CLI command creates a context-based restriction rule for all the virtual
server instances in the resource group and restricts access to private endpoints:

```sh
ibmcloud cbr rule-create --context-attributes 'endpointType=private' --zone-id a7eeb5dd8e6bdce670eba1afce18e37f --description "Test CBR for VSIs" --service-name is --resource-attributes "instanceId=*" --resource-group-id 1171749e3a8545069d04e6fca1ded18f
```
{: pre}

### Creating rules in the console
{: #rules-ui}
{: ui}

1. Go to **Manage** > **Context-based restrictions** in the {{site.data.keyword.cloud}} console.
1. Select **Rules** and click **Create**.
1. Select **VPC Infrastructure Services** and click **Next**.
1. Scope the rule to **All resources** or **Specific resources**. For more information, see [Protecting VPC Infrastructure Services resources](/docs/vpc?topic=vpc-cbr). Click **Continue**.
1. Define the allowed endpoint types.
   - Keep the toggle set to **No** to allow all endpoint types.
   - Set the toggle to **Yes** to allow only specific endpoint types, then choose from the list.
1. Select an existing network zone or zones, or create a network zone by clicking **Create**.

   Contexts define the location from which your resources can be accessed, effectively linking your network zone to your rule.
   {: tip}

1. Click **Add** to add your configuration to the summary. Click **Next**.
1. Name your rule and select how you want to enforce it. Then, click **Create**.

## Monitoring context-based restrictions in VPC Infrastructure Services
{: #cbr-vpc-monitoring}

The context-based restriction service generates audit logs every time when a context-based policy is
enforced. For more information, see
[Monitoring context-based restrictions](/docs/account?topic=account-cbr-monitor).

## Troubleshooting failures due to context-based restrictions
{: #cbr-vpc-troubleshooting}

### 403 Unauthorized error messages
{: #cbr-vpc-tbs-403-unauth}

When a request is denied because of improper access permissions or the request originates outside of the network zone that is defined in context-based restrictions rule, the error response is similar to the following examples.

#### 403 unauthorized error in the CLI response
{: #cbr-vpc-tbs-403-unauth-cli}
{: cli}

```text
Error code: not_authorized and
Error message: the provided token is not authorized
Trace ID: b266a31c-b359-4ecb-8b81-a27c1b2cdb7b
```
{: screen}

#### 403 unauthorized error in the IBM Cloud console
{: #cbr-vpc-tbs-403-unauth-ui}

```text
reference_id b266a31c-b359-4ecb-8b81-a27c1b2cdb7b
code not_authorized
message the provided token is not authorized
```
{: screen}

#### 403 unauthorized error in the API response
{: #cbr-vpc-tbs-403-unauth-api}
{: api}

```text
"code":"not_authorized",
"message":"the provided token is not authorized to list floating-ips in this account"
"trace":"b266a31c-b359-4ecb-8b81-a27c1b2cdb7b"
```
{: screen}

### Locating context-based restriction events
{: #cbr-vpc-locate-events-AT}

You can use the value of `Trace ID` or `Reference ID` to search in your {{site.data.keyword.logs_full_notm}} instance for the events that are generated by the context-based restrictions service. The `requestData.environment` section shows the source IP address of the call and other information that you can use to verify why the authorization was denied. The `requestData.action` and `requestData.resource` fields can be used determine which underlying resource was denied access.

The following example is a context-based restriction event. Some fields were removed to improve readability.

```json
{
  "level": "critical",
  "action": "context-based-restrictions.policy.eval",
  "message": "Context based restriction rendered a deny",
  "severity": "critical",
  "correlationId": "TXID-b266a31c-b359-4ecb-8b81-a27c1b2cdb7b-df2f912c-80a1-422c-8b17-6092651fea00",
  "requestData": {
    "action": "is.vpc.vpc.create",
    "reqOrder": 0,
    "environment": {
      "attributes": {
        "ipAddress": "192.168.0.1",
        "networkType": "public"
      }
    },
    "resource": {
      "attributes": {
        "accountId": "67db3d7ff3f34220b40e2d81480754c9",
        "resource": "NEWRESOURCE",
        "vpcId": "NEWRESOURCE",
        "resourceGroupId": "d272ce832535498e9142b557ff8638ed",
        "region": "au-syd",
        "serviceName": "is",
        "resourceType": "vpc"
      }
    },
    "subject": {
      "attributes": {
        "scope": "ibm openid",
        "token.account.bss": "67db3d7ff3f34220b40e2d81480754c9",
        "id": "IBMid-550009FB7N"
      }
    }
  },
  "responseData": {
    "decision": "Deny",
    "isEnforced": true
  }
}
```
{: codeblock}

You can then use the same Trace ID or Reference ID to look at the specific VPC Infrastructure service event in your {{site.data.keyword.logs_full_notm}} instance.

```json
{
  "action": "is.vpc.vpc.create",
  "outcome": "failure",
  "message": "Virtual Private Cloud: create vpc -failure",
  "severity": "critical",
  "dataEvent": false,
  "logSourceCRN": "crn:v1:bluemix:public:is:au-syd:a/67db3d7ff3f34220b40e2d81480754c9::vpc:",
  "saveServiceCopy": true,
  "initiator": {
    "id": "IBMid-ABCDEFGH",
    "typeURI": "service/security/account/user",
    "name": "test@ibm.com",
    "authnName": "test",
    "authnId": "IBMid-ABCDEFGH",
    "host": {
      "address": "192.168.0.1",
      "addressType": "IPv4"
    },
    "credential": {
      "type": "token"
    }
  },
  "target": {
    "id": "crn:v1:bluemix:public:is:au-syd:a/67db3d7ff3f34220b40e2d81480754c9::vpc:",
    "typeURI": "is.vpc/vpc",
    "name": "",
    "resourceGroupId": "crn:v1:bluemix:public:resource-controller::a/67db3d7ff3f34220b40e2d81480754c9::resource-group:d272ce832535498e9142b557ff8638ed"
  },
  "reason": {
    "reasonCode": 403,
    "reasonType": "Forbidden",
    "reasonForFailure": "Your access token is invalid or does not have the necessary permissions to perform this task"
  },
  "requestData": {
    "generation": "2",
    "requestId": "b266a31c-b359-4ecb-8b81-a27c1b2cdb7b"
  },
  "responseData": {
    "responseURI": "/v1/vpcs/"
  }
}
```
{: codeblock}

You can also search events by using a resource identifier. A resource ID is a UUID and is the last identifier in a CRN. See the following example

```text
crn:v1:bluemix:public:is:au-syd:a/67db3d7ff3f34220b40e2d81480754c9::vpc:<resourceID>
```
{: screen}

Requests for provisioning resources do not contain a resource ID.

If you do not find a context-based restriction event in the {{site.data.keyword.logs_full_notm}}, it's possible that access was denied due to IAM access policies. For more information, see the [VPC IAM getting started guide](/docs/vpc?topic=vpc-iam-getting-started).

### Client-to-site VPN data plane impact with context-based restrictions
{: #cbr-c2s-vpn-data-plane}

If you enable **User ID and passcode** in the [client authentication](/docs/vpc?topic=vpc-client-to-site-vpn-planning#client-authentication) configuration for a client-to-site VPN, and create context-based rules on VPC resources, the client connection to the VPN is impacted by context-based restrictions. If the VPN client remote IP is not in the **Network Zone**, the connection to the VPN server returns an `Auth Failed` error.

After the VPN client connects to the VPN server, the requests to the VPE endpoint or Cloud Service Endpoint (CSE) are controlled with context-based restrictions. Also, the requests' source IP is the VPC [Cloud service endpoint source addresses](/docs/vpc?topic=vpc-vpc-behind-the-curtain#cse-source-addresses). Make sure that the VPC CSE source addresses are in the CBR Network Zone; otherwise, the request is denied.

## Limitations
{: #cbr-limitations}

- Context-based restrictions protect only the actions that are associated with the [VPC Infrastructure Services APIs](/apidocs/vpc) or the `is` plug-in of the IBM CLI. The SDK and Terraform options are also supported. Actions that are associated with the following platform APIs are *not* protected by context-based restrictions:
   - [Resource Instance List APIs](/apidocs/resource-controller/resource-controller#list-resource-instances)
   - [Resource Instance Delete resource API](/apidocs/resource-controller/resource-controller#delete-resource-instance)
   - [Global Search APIs](/apidocs/search)
   - Global Tagging [Attach](/apidocs/tagging#attach-tag) and [Detach](/apidocs/tagging#detach-tag) APIs
- When you create a rule, it might take up to 10 minutes to become enforced.
- Due to a limitation that is being addressed, context-based restrictions must not be in place for [Secrets Manager APIs](/apidocs/secrets-manager) when you use the [Load balancer for VPC service](/docs/vpc?topic=vpc-nlb-vs-elb). Because it results in failing to attach the certificates to the listener that is associated with the load balancer.
