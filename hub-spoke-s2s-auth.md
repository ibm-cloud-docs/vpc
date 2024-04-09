---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Establishing service-to-service authorization
{: #hub-spoke-s2s-auth}

To configure DNS sharing for VPE gateways between hub and DNS-shared VPCs on different accounts, the hub VPC administrator must establish an IAM service-to-service (s2s) authorization policy. For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth).
{: shortdesc}

**Notes:**

* If a DNS-shared VPC and the hub VPC are using the same or different accounts, as the hub VPC administrator, you must create an IAM service-to-service authorization policy that gives this DNS-shared VPC the `DNSBindingConnector` permission on the hub VPC. 
* Currently, the UI allows only **All VPC instances** to have the `DNSBindingConnector` permission on the hub VPC. To indicate a "specific" VPC, you must use the API or wrap an API request in a JSON file to use with the CLI. Example code is provided for use with the [CLI](/docs/vpc?topic=vpc-hub-spoke-s2s-auth&interface=cli#indicate-specific-vpc-instance-cli) and [API](/docs/vpc?topic=vpc-hub-spoke-s2s-auth&interface=api#indicate-specific-vpc-instance).

You can establish s2s authorization with the UI, CLI, API, or Terraform.

## Creating an IAM s2s authorization policy in the UI
{: #hub-spoke-s2s-auth-procedure-ui}
{: ui}

To create an IAM s2s authorization policy in the UI, follow these steps:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Manage > Access (IAM)**. The IBM Cloud Identity and Access Management page displays.
1. From the side panel, select **Authorizations**.
1. On the Manage authorizations page, click **Create**.
1. On the Grant a service authorization page, select source account.
   - If you're setting up authorization in your account, select **This account**.
   - If you're setting up authorization in the enterprise account, select **Other account**.
1. For the source service, select **VPC Infrastructure Services** from the list.
1. Select the scope. Choose **Specific resources**.
1. Click **Resource type**. From the list, select **Virtual Private Cloud**.
1. For the target service, select **VPC Infrastructure Services** from the list.
1. Select the scope. Choose **Specific resources**.
1. Click **Resource type**. From the list, select **Virtual Private Cloud**.
1. In the Roles section under Service access, select **DNSBindingConnector**.
1. Click **Authorize**.
1. When you are returned to the **Manage authorizations** page, click **Create** again and follow the same steps to set up authorizations for the other two services.

## Creating an IAM s2s authorization policy from the CLI
{: #hub-spoke-s2s-auth-procedure-cli}
{: cli}

To create an IAM s2s authorization policy from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your account with the CLI. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Create an IAM s2s authorization policy:

   ```bash
   ibmcloud iam authorization-policy-create is DNSBindingConnector --source-resource-type vpc --target-resource-type vpc
   ```
   {: pre}

## Indicating a specific VPC instance from the CLI
{: #indicate-specific-vpc-instance-cli}
{: cli}

If a DNS-shared VPC and the hub VPC are using the same account, as the hub VPC administrator, you must create an IAM service-to-service authorization policy that gives this DNS-shared VPC the `DNSBindingConnector` permission on the hub VPC.

Currently, the UI `Source service instance` only allows **All VPC instances** to have the `DNSBindingConnector` permission on the hub VPC.
{: note}

To indicate a specific VPC from the CLI, you must use a JSON file with API request content. For example:

```bash
ibmcloud iam authorization-policy-create --file JSON_FILE
```
{: pre}

Where the `JSON_FILE` contains:

```json
{"type":"authorization","subjects":[{"attributes":[{"name":"accountId","value":"e38cfd44b6e44b249ac75dd5de0cc8aa"},{"name":"serviceName","value":"is"},{"name":"resourceType","value":"vpc"},{"name":"resource","value":"r134-fd189c4f-52a0-4694-abfd-186cb6011a3c"}]}],"roles":[{"role_id":"crn:v1:bluemix:public:is::::serviceRole:DNSBindingConnector"}],"resources":[{"attributes":[{"name":"accountId","value":"e38cfd44b6e44b249ac75dd5de0cc8aa"},{"name":"serviceName","value":"is"},{"name":"vpcId","value":"r134-ffaedf75-4f44-4ead-989d-cd37816a13d2"}]}]}
```
{: pre}

## Creating an IAM s2s authorization policy with the API
{: #hub-spoke-s2s-auth-procedure-api}
{: api}

To create an IAM s2s authorization policy with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API commands:

   ```sh
   export dns_shared_account_id=<dns_shared_vpc_account_id>
   export hub_account_id=<hub_vpc_account_id>
   export dns_shared_vpc_id=<dns_shared_vpc_id>
   export hub_vpc_id=<hub_vpc_id>
    ```
    {: pre}

1. To create and delete a DNS resolution binding between hub and DNS-shared VPCs across different accounts, a s2s policy must exist in the hub VPC account. This gives the DNS-shared VPC a `DNSBindingConnector` role on the hub VPC. To create the s2s policy, refer to the "Create a Policy" API.

### Example request
{: #example-req-body}

```sh
{
  "type": "authorization",
  "subjects": [
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "'$dns_shared_account_id'"
        },
        {
          "name": "serviceName",
          "value": "is"
        },
        {
          "name": "resourceType",
          "value": "vpc"
        },
        {
          "name": "resource",
          "value": "'$dns_shared_vpc_id'"
        }
      ]
    }
  ],
  "roles": [
    {
      "role_id": "crn:v1:bluemix:public:is::::serviceRole:DNSBindingConnector"
    }
  ],
  "resources": [
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "'$hub_account_id'"
        },
        {
          "name": "serviceName",
          "value": "is"
        },
        {
          "name": "vpcId",
          "value": "'$hub_vpc_id'"
        }
      ]
    }
  ]
}
```
{: screen}

### Command example
{: #example-api-command}

```curl
curl -sX POST "$iam_api_endpoint/v1/policies" -H "Authorization: Bearer ${iam_token}" -d '$request_body'
```
{: pre}

## Indicating a specific VPC instance with the API
{: #indicate-specific-vpc-instance}
{: api}

If a DNS-shared VPC and the hub VPC are using the same account, as the hub VPC administrator, you must create an IAM service-to-service authorization policy that gives this DNS-shared VPC the `DNSBindingConnector` permission on the hub VPC.

Currently, the UI `Source service instance` only allows **All VPC instances** to have the `DNSBindingConnector` permission on the hub VPC.
{: note}

Here is a sample API to grant the DNS-shared VPC to have `DNSBindingConnector` permission on the hub VPC. Note that the `role_id` is `crn:v1:bluemix:public:is::::serviceRole:DNSBindingConnector`.

```sh
{
"type": "authorization",
"subjects": [
{
"attributes": [
{ "name": "accountId", "value": "Account S (Spoke)" }
 ,
   { "name": "serviceName", "value": "is" }
 ,
{ "name": "resourceType", "value": "vpc" }
 ,
{ "name": "resource", "value": "Spoke VPC ID" }
 ]
}
],
"roles": [
{ "role_id": "crn:v1:bluemix:public:is::::serviceRole:DNSBindingConnector" }
 ],
"resources": [
{
"attributes": [
{ "name": "accountId", "value": "Account H (Hub)" }
 ,
{ "name": "serviceName", "value": "is" }
 ,
{ "name": "vpcId", "value": "Hub VPC ID" }
 ]
}
]
}
```
{: screen}

## Creating an IAM s2s authorization policy with Terraform
{: #hub-dns-shared-s2s-auth-procedure-terraform}
{: terraform}

You can use Terraform to create an IAM s2s authorization policy.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a regional specific endpoint, which targets `us-south` by default. If you create your VPC in another region, make sure to target the appropriate region in the provider block of the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
resource "ibm_iam_authorization_policy" "policy" {

    roles                  = [

        "DNSBindingConnector",

    ]

    subject_attributes {

        name  = "accountId"

        value = "var.accountId1"

    }

    subject_attributes {

        name  = "serviceName"

        value = "is"

    }

     subject_attributes {

        name  = "resourceType"

        value = "vpc"

    }

    subject_attributes {

        name  = "resource"

        value = ibm_is_vpc.spoke_vpc.id

    }

    resource_attributes {

        name     = "accountId"

        value    =  "var.accountId2"

    }

    resource_attributes {

        name     = "serviceName"

        value    = "is"

    }

   resource_attributes {

      name  =  "vpcId"

      value =  ibm_is_vpc.hub_vpc.id

   }

}
```
{: codeblock}

## Related link
{: #related-link-iam}

[IAM permissions](/docs/account?topic=account-iam-service-roles-actions#is.vpc-roles)
