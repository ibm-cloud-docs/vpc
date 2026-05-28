---

copyright:
  years: 2021, 2026
lastupdated: "2026-05-28"

keywords: ike policy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an IKE policy
{: #creating-ike-policy}

You can use custom Internet Key Exchange (IKE) policies to define security parameters that are used during Phase 1 of IKE negotiation. In this phase, the VPN and peer device exchange credentials and security policies to authenticate each other and establish a secure communication channel that is used for Phase 2 negotiation.
{: shortdesc}

IKEv1 is a deprecated protocol and you are strongly encouraged to use IKEv2. If your VPN connection uses an IKEv1 policy, the associated IPsec policy must also contain only one algorithm because IKEv1 supports only a single algorithm. IPsec policies with multiple algorithms are not supported by IKEv1.
{: note}

To ensure consistent algorithm selection, match exact IKE and IPsec algorithms and their order of priority on both the IBM Cloud VPN gateway and peer gateway. For more information on what factors affect algorithm selection, see [How are encryption algorithms chosen for IKE and IPsec in a site-to-site VPN connection?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-18)
{: important}

## Creating an IKE policy in the console
{: #vpn-using-ui-create-ike-policy}
{: ui}

To create an IKE policy in the console, follow these steps:

1. From the VPNs for VPC list page, select **Site-to-site gateways > IKE policies**.
1. Click **Create** and specify the following information:
   * **Location** - Select a region for this IKE policy.
   * **Name** - Enter a name for the IKE policy.
   * **Resource group** - Select the resource group for this IKE policy.
   * **IKE version** - Select the IKE protocol version. Some vendors don't support both IKEv1 and IKEv2. Check the peer vendor documentation to verify which IKE version is supported.
   * **Encryption** - Select the encryption algorithm to use for IKE. By default, the lowest-strength algorithm is selected. You can select multiple values for each field and reorder them by priority. The VPN gateway negotiates with the peer gateway to select the best mutually supported algorithm.
   * **Authentication** - Select the authentication algorithm to use for IKE. By default, the lowest-strength algorithm is selected. You can select multiple values for each field and reorder them by priority.
   * **Diffie-Hellman group** - Select the DH group to use for IKE. By default, the lowest DH group is selected. You can select multiple DH groups and reorder them by priority.
   * **Key lifetime** - Select the lifetime, in seconds, for the Phase 1 tunnel.
1. Click **Create**.
1. From the **VPN connection details** page, set the **IKE policies** field to use the wanted IKE policy.

   To ensure successful IKE/IPsec negotiation, configure both peers with at least one matching algorithm in each category (authentication, encryption, and DH group). Aligning these settings across peers helps avoid connection failures.
   {: tip}

## Creating an IKE policy from the CLI
{: #vpn-using-cli-create-ike-policy}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create an IKE policy from the CLI, enter the following command:

```sh
ibmcloud is ike-policy-create IKE_POLICY_NAME AUTHENTICATION_ALGORITHMS DH_GROUPS ENCRYPTION_ALGORITHMS IKE_VERSION [--key-lifetime KEY_LIFETIME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

- **IKE_POLICY_NAME** - The name of the IKE policy.
- **AUTHENTICATION_ALGORITHMS** - The authentication algorithms. One of: `sha256`, `sha384`, `sha512`, or a comma-separated list of authentication algorithms (`sha512, sha384, sha256,`). The order of the algorithms determines their priority during negotiation.
- **DH_GROUPS** - The Diffie-Hellman groups. One of: `14`, `15`, `16`, `17`, `18`, `19`, `20`, `21`, `22`, `23`, `24`, `31`, or a comma-separated list of DH groups (`14,15,16,17,18`). The order of the groups determines their priority during negotiation.
- **ENCRYPTION_ALGORITHMS** - The encryption algorithms. One of: `aes128`, `aes192`, `aes256`, or a comma-separated list of encryption algorithms (`aes256,aes192,aes128`). The order of the algorithms determines their priority during negotiation.
- **IKE_VERSION** - The IKE protocol version. One of: `1`, `2`.
- **--key-lifetime** - The key lifetime in seconds. Maximum: `86400`. Minimum: `1800`. The default value is `28800`.
- **--resource-group-id** - The ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name** - The name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--output** - Specify output in JSON format.
- **-q, --quiet** - Suppress verbose output.



`md5` and `sha1` authentication algorithms, `2` and `5` DH groups, and the `triple_des` encryption algorithm were deprecated on 20 September 2022 and are no longer supported in the console.
{: deprecated}

## Updating an IKE policy from the CLI
{: #vpn-using-cli-update-ike-policy}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To update an IKE policy from the CLI, enter the following command:

```sh
ibmcloud is ike-policy-update IKE_POLICY [--name NEW_NAME] [--authentication-algorithms AUTHENTICATION_ALGORITHMS] [--dh-groups DH_GROUPS] [--encryption-algorithms ENCRYPTION_ALGORITHMS] [--authentication-algorithm sha256 | sha384 | sha512] [--dh-group 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 31] [--encryption-algorithm aes128 | aes192 | aes256] [--ike-version 1 | 2] [--key-lifetime KEY_LIFETIME] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

- **--name** - The name of the IKE policy.
- **--authentication-algorithms** - A comma-separated list of authentication algorithms (recommended). The order of the algorithms determines their priority during negotiation.
- **--authentication-algorithm** - The authentication algorithm (deprecated). One of: `sha256`, `sha384`, `sha512`.
- **--dh-groups** - A comma-separated list of DH groups (recommended). The order of the groups determines their priority during negotiation.
- **--dh-group** - The DH group (deprecated). One of: `14`, `15`, `16`, `17`, `18`, `19`, `20`, `21`, `22`, `23`, `24`, `31`.
- **--encryption-algorithms** - A comma-separated list of encryption algorithms (recommended). The order of the algorithms determines their priority during negotiation.
- **--encryption-algorithm** - The encryption algorithm (deprecated). One of: `aes128`, `aes192`, `aes256`.
- **--ike-version** - The IKE protocol version. One of: `1`, `2`.

   Do not mix singular and array-based properties for the same algorithm category in a single command.
   {: note}



### Command examples
{: #command-examples-vpn-create-ike-policy}

The singular values for `authentication algorithms`, `dh_groups`, and `encryption algorithms`, to create IKE policy from the CLI are deprecated. Use multiple comma-separated values instead.
{: important}

- Create an IKE policy by using comma-separated authentication algorithms (SHA 512 and SHA 256), DH groups (14, 15, 16, 17), encryption algorithms (AES 256, AES 192, AES 128), and IKE Version 2:

   ```sh
   ibmcloud is ike-policy-create my-ike-policy sha512,sha256 14,15,16,17 aes256,aes192,aes128 2
   ```
   {: pre}

- Create an IKE policy by using single authentication algorithm (SHA 256), single encryption algorithm (AES 128), DH Group 14, and IKE Version 2:

   ```sh
   ibmcloud is ike-policy-create my-ike-policy sha256 14 aes128 2
   ```
   {: pre}

- Create an IKE policy with the same parameters and a 3600-second lifetime:

   ```sh
   ibmcloud is ike-policy-create my-ike-policy sha256 14 aes128 2 --key-lifetime 3600
   ```
   {: pre}

- Create an IKE policy with the same parameters and a specific resource group ID:

   ```sh
   ibmcloud is ike-policy-create my-ike-policy sha256 14 aes128 2 --resource-group-id fee82deba12e4c0fb69c3b09d1f12345 --output JSON
   ```
   {: pre}

- Update an IKE policy to change the name, authentication algorithms to SHA 512 and SHA 256, encryption algorithms to AES 256, AES 192, and DH Groups to 15 and 16.

   ```sh
   ibmcloud is ike-policy-update my-ike-policy --name new-ike-policy --authentication-algorithms sha512,sha256 --encryption-algorithms aes256,aes192 --dh-groups 15,16 --output JSON
   ```
   {: pre}

   To ensure successful IKE/IPsec negotiation, configure both peers with at least one matching algorithm in each category (authentication, encryption, and DH group). Aligning these settings across peers helps avoid connection failures.
   {: tip}

## Creating an IKE policy with the API
{: #vpn-using-api-create-ike-policy}
{: api}

To create an IKE policy with multiple algorithms by using the array-based properties (recommended), follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

   ```sh
   export ResourceGroupId=<your_resourcegroup_id>
   ```
   {: pre}

1. Create the IKE policy:

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/ike_policies?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name": "my-new-ike-policy",
            "authentication_algorithms": ["sha256","sha384","sha512"],
            "encryption_algorithms": ["aes128","aes256"],
            "dh_groups": [14,15,16],
            "ike_version": 2,
            "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}

The singular properties `authentication_algorithm`, `dh_group`, and `encryption_algorithm` for IKE negotiation are deprecated. Use the array-based properties to create IKE policy. To know more about the singular and array-based algorithm properties for IKE policy when using the API, see [Updating to multiple IKE and IPsec algorithms](/docs/vpc?topic=vpc-vpn-update-multiple-algorithms).
{: important}

To create an IKE policy with the API by using singular properties (deprecated), follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

   ```sh
   export ResourceGroupId=<your_resourcegroup_id>
   ```
   {: pre}

1. Create the IKE policy:

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

## Updating an IKE policy with the API
{: #vpn-using-api-update-ike-policy}
{: api}

To update an IKE policy with the API by using the array-based properties (recommended), follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

   ```sh
   export ResourceGroupId=<your_resourcegroup_id>
   ```
   {: pre}

1. Update the IKE policy:

   ```sh
      curl -X PATCH "$vpc_api_endpoint/v1/ike_policies/$ike_policy_id?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
            "name": "my-updated-ike-policy",
            "authentication_algorithms": ["sha512","sha384","sha256"],
            "encryption_algorithms": ["aes256","aes128"],
            "dh_groups":[15,16,14],
            "ike_version": 2,
            "key_lifetime": 3600,
            "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}

To update an IKE policy with the API by using singular properties (deprecated), follow these steps:

   You can update IKE policies by using either deprecated singular properties or array-based properties. Both forms are accepted, but you must not mix singular and array-based properties for the same algorithm category in a single request.
   {: #note}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

   ```sh
   export ResourceGroupId=<your_resourcegroup_id>
   ```
   {: pre}

1. Update the IKE policy:

   ```sh
      curl -X PATCH "$vpc_api_endpoint/v1/ike_policies/$ike_policy_id?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
            "name": "my-updated-ike-policy",
            "authentication_algorithm": "sha384",
            "encryption_algorithm": "aes256",
            "dh_group": 15,
            "ike_version": 2,
            "key_lifetime": 3600,
            "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}

To view the complete set of APIs for site-to-site VPN gateways, see the [VPC API reference](/apidocs/vpc/latest#create-ike-policy).
{: tip}


## Next steps
{: #vpn-ike-policy-next-steps}

After you create an IKE policy, complete the following tasks as needed:

* [Create an IPsec policy](/docs/vpc?topic=vpc-creating-ipsec-policy-copy) if you decide to use a custom IPsec policy instead of an auto-negotiated IPsec policy.
* If you didn't create a VPN connection during VPN gateway creation, you can create one after the gateway is provisioned. For more information, see [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections).
* For a route-based VPN, select or [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table). Then, [create a route using the VPN connection type](/docs/vpc?topic=vpc-create-vpc-route).

## Related links
{: #related-links-vpn-ike-policy}

* [How do I check IPsec logs?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-19)
* [Why does my IPsec connection status appear to be up but the BGP state isn't established?](/vpc?topic=vpc-troubleshoot-ipsec-up-bgp-not-established)
