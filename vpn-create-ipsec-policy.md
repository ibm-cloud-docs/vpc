---

copyright:
  years: 2021
lastupdated: "2021-08-09"

keywords: vpn, ipsec policy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an IPsec policy
{: #creating-ipsec-policy}

You can use custom IPsec policies to define security parameters to use during Phase 2 of IKE negotiation. In this phase, the VPN and peer device use the security association that is established during Phase 1 to negotiate what traffic to send and how to authenticate and encrypt that traffic.
{: shortdesc}

## Creating an IPsec policy by using the UI
{: #vpn-using-ui-create-ipsec-policy}
{: ui}

To create an IPsec policy by using the UI, follow these steps:

1. From the VPNs for VPC list page, select the **Site-to-site gateways > IPsec policies** tabs. 
1. Click **Create +** and specify the following information: 
   * **Name** - Enter a name for the IPsec policy.
   * **Resource group** - Select the resource group for this IPsec policy.
   * **Region** - Select the region for this IPsec policy.
   * **Authentication** - Authentication algorithm to use for IKE Phase 2.
   * **Encryption** - Encryption algorithm to use for IKE Phase 2.
   * **Perfect Forward Secrecy** - Enable PFS.
   * **Diffie-Hellman Group (If PFS is enabled)** - DH group to use for IKE Phase 2 key exchange.
   * **Key lifetime** - Lifetime in number of seconds of the Phase 2 tunnel.
1. Click **Create IPsec policy**.
1. From the **VPN connection details** page, set the **IPsec policies** field to use the wanted IPsec policy.

## Creating an IPsec policy by using the CLI
{: #vpn-using-cli-create-ipsec-policy}
{: cli}

To create an IPsec policy by using the CLI, enter the following command:

```sh
ibmcloud is ipsec-policy-create IPSEC_POLICY_NAME AUTHENTICATION_ALGORITHM ENCRYPTION_ALGORITHM PFS
    [--key-lifetime KEY_LIFETIME]
    [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **IPSEC_POLICY_NAME** - Name of the IPsec policy.
- **AUTHENTICATION_ALGORITHM** - The authentication algorithm. One of: `md5`, `sha1`, `sha256`.
- **ENCRYPTION_ALGORITHM** - The encryption algorithm. One of: `triple_des`, `aes128`, `aes256`.
- **PFS** - The Diffie-Hellman group. One of: `disabled`, `group_2`, `group_5`, `group_14`.
- **--key-lifetime value** - The key lifetime in seconds. Maximum: `86400`, Minimum: `1800`. The default value is `3600`.
- **--resource-group-id value** - ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name value** - Name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--output value** - Specify output in JSON format.
- **-q, --quiet** - Suppress verbose output.

### Command examples
{: #command-examples-vpn-create-ipsec-policy}

- Create an IPsec policy using MD5 authentication, AES 128 encryption, and PFS with DH Group 2:
   `ibmcloud is ike-policy-create my-ike-policy md5 2 aes128 group_2`
- Create an IPsec policy with the same parameters and a 3600-seconds lifetime:
   `ibmcloud is ike-policy-create my-ike-policy md5 2 aes128 group_2 --key-lifetime 3600`
- Create an IPsec policy with the same parameters and a resource group ID:
   `ibmcloud is ike-policy-create my-ike-policy md5 2 aes128 group_2 --resource-group-id fee82deba12e4c0fb69c3b09d1f12345 --output JSON`

## Creating an IPsec policy by using the API
{: #vpn-using-api-create-ipsec-policy}
{: api}

To create an IPsec policy by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

1. When all variables are initiated, create the IPsec policy:

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/ipsec_policies?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name": "my-new-ipsec-policy",
           "authentication_algorithm": "sha1",
           "encryption_algorithm": "aes128",
           "pfs": "group_2",
           "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}
   

## Creating an IPsec policy by using Terraform
{: #vpn-using-terraform-create-ipsec-policy}
{: terraform}

In the following example, you can create a IPsec policy using Terraform:

```terraform
   resource "ibm_is_ipsec_policy" "is_ipsec_policy" {
     name                     = "my-ipsec-policy"
     authentication_algorithm = "sha1"
     encryption_algorithm     = "aes128"
     pfs                      = "group_2"
   }
```
{: codeblock}

See the [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_ipsec_policy)for more information.

## Next steps
{: #vpn-create-ipsec-next-steps}

* [Create an IKE policy](/docs/vpc?topic=vpc-creating-ipsec-policy) if you decide to use custom IKE policy instead of auto-negotiation.
* Create a VPN connection if you have not already done so when creating your VPN gateway. If you did not create the VPN connection, you can do so after the VPN gateway is provisioned. For more information, see [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections).  
* For a route-based VPN, select or [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table). Then, [create a route using the VPN connection type](/docs/vpc?topic=vpc-create-vpc-route).
