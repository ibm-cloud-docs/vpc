---

copyright:
  years: 2021, 2023
lastupdated: "2023-01-17"

keywords: ike policy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an IKE policy
{: #creating-ike-policy}

You can use custom Internet Key Exchange (IKE) policies to define security parameters to use during Phase 1 of IKE negotiation. In this phase, the VPN and peer device exchange credentials and security policies to authenticate each other and establish a secure communication channel to be used for Phase 2 negotiation.
{: shortdesc}

## Creating an IKE policy in the UI
{: #vpn-using-ui-create-ike-policy}
{: ui}

To create an IKE policy using the UI, follow these steps:

1. From the VPNs for VPC list page, select the **Site-to-site gateways > IKE policies** tabs. 
1. Click **Create +** and specify the following information:   
   * **Name** - Enter a name for the IKE policy.
   * **Resource group** - Select the resource group for this IKE policy.
   * **Region** - Select the region for this IKE policy.
   * **IKE version** - IKE protocol version. Some vendors do not support both IKE versions. Check with peer vendor documentation to verify IKE version support.
   * **Authentication** - Authentication algorithm to use for IKE Phase 1.
   * **Encryption** - Encryption algorithm to use for IKE Phase 1.
   * **Diffie-Hellman group** - DH group to use for IKE Phase 1.
   * **Key lifetime** - Lifetime in number of seconds of Phase 1 tunnel.
1. Click **Create IKE policy**.
1. From the **VPN connection details** page, set the **IKE policies** field to use the wanted IKE policy.

## Creating an IKE policy from the CLI
{: #vpn-using-cli-create-ike-policy}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create an IKE policy using the CLI, enter the following command:

```sh
ibmcloud is ike-policy-create IKE_POLICY_NAME AUTHENTICATION_ALGORITHM DH_GROUP ENCRYPTION_ALGORITHM IKE_VERSION
    [--key-lifetime KEY_LIFETIME]
    [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`md-5` and `sha-1` authentication algorithms, `2` and `5` DH groups, and the `triple_des` encryption algorithm were deprecated on 20 September 2022 and are no longer supported in the UI. If you currently use these ciphers, you must [upgrade weak cipher suites on a VPN gateway](/docs/vpc?topic=vpc-upgrading-weak-ciphers&interface=ui) before end of support is announced for use with the CLI and API.
{: deprecated}

- **IKE_POLICY_NAME** - Name of the IKE policy.
- **AUTHENTICATION_ALGORITHM** - The authentication algorithm. One of: `sha256`, `sha384`, `sha512`.
- **DH_GROUP** - The Diffie-Hellman group. One of: `14`, `15`, `16`, `17`, `18`, `19`, `20`, `21`, `22`, `23`, `24`, `31`.
- **ENCRYPTION_ALGORITHM** - The encryption algorithm. One of: `aes128`, `aes192`, `aes256`.
- **IKE_VERSION** - The IKE protocol version. One of: `1`, `2`.
- **--key-lifetime value** - The key lifetime in seconds. Maximum: `86400`, Minimum: `1800`. The default value is `28800`.
- **--resource-group-id value** - ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name value** - Name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--output value** - Specify output in JSON format.
- **-q, --quiet** - Suppress verbose output.

### Command examples
{: #command-examples-vpn-create-ike-policy}

- Create an IKE policy using SHA 256 authentication, DH Group 14, AES 128 encryption, and IKE Version 2:
   - `ibmcloud is ike-policy-create my-ike-policy sha256 14 aes128 2`
- Create an IKE policy with the same parameters and a 3600-seconds lifetime:
   - `ibmcloud is ipsec-policy-create my-ipsec-policy sha256 14 aes128 2 --key-lifetime 3600`
- Create an IKE policy with the same parameters and a resource group ID:
   - `ibmcloud is ipsec-policy-create my-ipsec-policy sha256 14 aes128 2 --resource-group-id fee82deba12e4c0fb69c3b09d1f12345 --output JSON`

## Creating an IKE policy with the API
{: #vpn-using-api-create-ike-policy}
{: api}

To create an IKE policy using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

1. When all variables are initiated, create the IKE policy:

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/ike_policies?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name": "my-new-ike-policy",
           "dh_group": 14,
           "authentication_algorithm": "sha256",
           "encryption_algorithm": "aes128",
           "ike_version": 2,
           "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}
   
## Creating an IKE policy by using Terraform
{: #vpn-using-terraform-create-ike-policy}
{: terraform}

In the following example, you can create a IKE policy using Terraform:

```terraform
   resource "ibm_is_ike_policy" "is_ike_policy" {
     name                     = "my-ike-policy"
     authentication_algorithm = "sha256"
     encryption_algorithm     = "aes128"
     dh_group                 = 14
     ike_version              = 2
   }
```
{: codeblock}

See the [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_ike_policy){: external} for more information.

## Next steps
{: #vpn-ike-policy-next-steps}

* [Create an IPsec policy](/docs/vpc?topic=vpc-creating-ipsec-policy) if you decide to use custom IPsec policy instead of auto-negotiation.
* Create a VPN connection if you have not already done so when creating your VPN gateway. If you did not create a VPN connection, you can do so after the VPN gateway is provisioned. For more information, see [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections).  
* For a route-based VPN, select or [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table). Then, [create a route using the VPN connection type](/docs/vpc?topic=vpc-create-vpc-route).
