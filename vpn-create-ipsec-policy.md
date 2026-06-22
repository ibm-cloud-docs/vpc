---

copyright:
  years: 2021, 2026
lastupdated: "2026-06-22"

keywords: vpn, ipsec policy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an IPsec policy
{: #creating-ipsec-policy}

You can use custom IPsec policies to define security parameters that are used during Phase 2 of the negotiation. In this phase, the VPN and peer device use the security association that is established during Phase 1 to negotiate what traffic to send and how to authenticate and encrypt that traffic.
{: shortdesc}

To ensure consistent algorithm selection, match exact IKE and IPsec algorithms and their order of priority on both the IBM Cloud VPN gateway and peer gateway. For more information on what factors affect algorithm selection, see [How are encryption algorithms chosen for IKE and IPsec in a site-to-site VPN connection?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-18)
{: important}

## Creating an IPsec policy in the console
{: #vpn-using-ui-create-ipsec-policy}
{: ui}

To create an IPsec policy in the console, follow these steps:

1. From the VPNs for VPC list page, select **Site-to-site gateways > IPsec policies**.
1. Click **Create** and specify the following information:
   * **Location** - Select a region for this IPsec policy.
   * **Name** - Enter a name for the IPsec policy.
   * **Resource group** - Select the resource group for this IPsec policy.
   * **Encryption** - Select the encryption algorithm to use for Phase 2. By default, the lowest-strength algorithm is selected. You can select multiple values for each field and reorder them by priority. The VPN gateway negotiates with the peer gateway to select the best mutually supported algorithm.
   * **Authentication** - Select the authentication algorithm to use for Phase 2. By default, the lowest-strength algorithm is selected. You can select multiple authentication algorithms and reorder them by priority.
   * **Perfect Forward Secrecy** - Enable this option to perform a new Diffie-Hellman exchange during each Phase 2 rekey so that if a key is compromised, previously encrypted traffic remains secure.
   * **Diffie-Hellman group (if PFS is enabled)** - Select the DH group to use for Phase 2 key exchange. By default, the lowest DH group is selected. You can select multiple DH groups and reorder them by priority.
   * **Key lifetime** - Select the lifetime, in seconds, for the Phase 2 tunnel.
1. Click **Create**.
1. From the **VPN connection details** page, set the **IPsec policies** field to use the wanted IPsec policy.

   To ensure successful IKE/IPsec negotiation, configure both peers with at least one matching algorithm in each category (authentication, encryption, and PF groups). Aligning these settings across peers helps avoid connection failures.
   {: tip}

## Creating an IPsec policy from the CLI
{: #vpn-using-cli-create-ipsec-policy}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create an IPsec policy from the CLI, enter the following command:

```sh
ibmcloud is ipsec-policy-create IPSEC_POLICY_NAME AUTHENTICATION_ALGORITHMS ENCRYPTION_ALGORITHMS PFSGS [--key-lifetime KEY_LIFETIME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

- **IPSEC_POLICY_NAME** - The name of the IPsec policy.
- **AUTHENTICATION_ALGORITHMS** - The authentication algorithms. Must be disabled only if **ENCRYPTION_ALGORITHMS** contains combined-mode algorithms (`aes128gcm16`, `aes192gcm16`, `aes256gcm16`). One of: `disabled`, `sha256`, `sha384`, `sha512`, or a comma-separated list of authentication algorithms (`sha384,sha256,sha512`). The order of the algorithms determines their priority during negotiation.
- **ENCRYPTION_ALGORITHMS** - The encryption algorithms. One of: `aes128`, `aes128gcm16`, `aes192`, `aes192gcm16`, `aes256`, `aes256gcm16`, or a comma-separated list of encryption algorithms (`aes128,aes192,aes256`). The order of the algorithms determines their priority during negotiation.
- **PFSGS** - Perfect Forward Secrecy Groups. One of: `disabled`, `group_14`, `group_15`, `group_16`, `group_17`, `group_18`, `group_19`, `group_20`, `group_21`, `group_22`, `group_23`, `group_24`, `group_31`, or a comma-separated list of PFS groups (`group_14,group_15,group_16`). The order of the groups determines their priority during negotiation.
- **--key-lifetime value** - The key lifetime in seconds. Maximum: `86400`, Minimum: `1800`. The default value is `3600`.
- **--resource-group-id value** - The ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name value** - The name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--output value** - Specify output in JSON format.
- **-q, --quiet** - Suppress verbose output.

`md5` and `sha1` authentication algorithms, `group_2` and `group_5` DH groups, and the `triple_des` encryption algorithm were deprecated on 20 September 2022 and are no longer supported in the console.
{: deprecated}

The `AUTHENTICATION_ALGORITHMS` must be `disabled` if and only if `ENCRYPTION_ALGORITHMS` is `aes128gcm16`, `aes192gcm16`, or `aes256gcm16`.
{: note}

## Updating an IPsec policy from the CLI
{: #vpn-using-cli-update-ipsec-policy}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To update an IPsec policy from the CLI, enter the following command:

```sh
ibmcloud is ipsec-policy-update IPSEC_POLICY [--name NEW_NAME] [--authentication-algorithms AUTHENTICATION_ALGORITHMS] [--pfsgs PFSGS] [--encryption-algorithms ENCRYPTION_ALGORITHMS] [--authentication-algorithm AUTHENTICATION_ALGORITHM] [--pfs disabled | group_14 | group_15 | group_16 | group_17 | group_18 | group_19 | group_20 | group_21 | group_22 | group_23 | group_24 | group_31] [--encryption-algorithm aes128 | aes128gcm16 | aes192 | aes192gcm16 | aes256 | aes256gcm16] [--key-lifetime KEY_LIFETIME] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

- **--name** - The name of the IPsec policy.
- **--authentication-algorithms** - A comma-separated list of authentication algorithms (recommended). The order of the algorithms determines their priority during negotiation.
- **--authentication-algorithm** - The authentication algorithm (deprecated). Must be disabled only if **--encryption_algorithm** is `aes128gcm16`, `aes192gcm16`, or `aes256gcm16`. One of: `disabled`, `sha256`, `sha384`, `sha512`
- **--encryption-algorithms** - A comma-separated list of encryption algorithms (recommended). The order of the algorithms determines their priority during negotiation.
- **--encryption-algorithm** - The encryption algorithm (deprecated). One of: `aes128`, `aes128gcm16`, `aes192`, `aes192gcm16`, `aes256`, `aes256gcm16`.
- **--pfsgs** - A comma-separated list of Perfect Forward Secrecy groups. The order of the groups determines their priority during negotiation.
- **--pfs** - The Perfect Forward Secrecy group (deprecated). One of: disabled, `group_14`, `group_15`, `group_16`, `group_17`, `group_18`, `group_19`, `group_20`, `group_21`, `group_22`, `group_23`, `group_24`, `group_31`.

   Do not mix singular and array-based properties for the same algorithm category in a single command.
   {: note}

### Command examples
{: #command-examples-vpn-create-ipsec-policy}

The singular values for `authentication algorithms`, `pfsgs`, and `encryption algorithms`, to create IPsec policy from the CLI are deprecated. Use multiple comma-separated values instead.
{: deprecated}

- Create an IPsec policy by using comma-separated authentication algorithms (SHA 512 and SHA 256), encryption algorithms (AES 256, AES 192, and AES 128), and perfect forward secrecy groups (group_14 and group_15):

   ```sh
   ibmcloud is ipsec-policy-create my-ipsec-policy sha512,sha256 aes256,aes192,aes128 group_14,group_15
   ```
   {: pre}

- Create an IPsec policy by using single authentication algorithm (SHA 256), single encryption algorithm (AES 128), and DH Group 14:

   ```sh
   ibmcloud is ipsec-policy-create my-ipsec-policy sha256 aes128 group_14
   ```
   {: pre}

- Create an IPsec policy with the same parameters and a 3600-seconds lifetime:

   ```sh
   ibmcloud is ipsec-policy-create my-ipsec-policy sha256 aes128 group_14 --key-lifetime 3600
   ```
   {: pre}

- Create an IPsec policy with the same parameters and a resource group ID:

   ```sh
   ibmcloud is ipsec-policy-create my-ipsec-policy sha256 aes128 group_14 --resource-group-id fee82deba12e4c0fb69c3b09d1f12345 --output JSON
   ```
   {: pre}

- Update an IPsec policy to change the name, authentication algorithms to SHA 512 and SHA 256, encryption algorithms to AES 256, AES 192, and perfect forward secrecy groups to group_15 and group_16:

   ```sh
   ibmcloud is ipsec-policy-update my-ipsec-policy --name new-ipsec-policy --authentication-algorithms sha512,sha256 --encryption-algorithms aes256,aes192 --pfsgs group_15,group_16 --output JSON
   ```
   {: pre}

- Update the IPsec policy with GCM encryption (authentication disabled):

   ```sh
   ibmcloud is ipsec-policy-update $ipsec_policy \
      --authentication-algorithms disabled \
      --encryption-algorithms aes128gcm16,aes256gcm16
   ```
   {: pre}

   The `authentication_algorithms` must be set to `disabled` if and only if the `encryption_algorithms` list contains any GCM-based algorithms (`aes128gcm16`, `aes192gcm16`, or `aes256gcm16`). GCM algorithms provide both encryption and built-in data integrity (authentication) as part of a single operation, so a separate authentication algorithm (such as `sha256` or `sha512`) must not be specified.
   {: note}

## Creating an IPsec policy with the API
{: #vpn-using-api-create-ipsec-policy}
{: api}

The singular properties `authentication_algorithm`, `dh_group`, `encryption_algorithm`, and `pfs` for IPsec negotiation are deprecated. Use the array-based properties to create IPsec policy. To know more about the singular and array-based algorithm properties for IPsec policy when using the API, see [Updating to multiple IKE and IPsec algorithms](/docs/vpc?topic=vpc-vpn-update-multiple-algorithms).
{: important}

To create an IPsec policy with multiple algorithms by using the array-based properties (recommended), follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

   ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
   {: pre}

1. Create the IPsec policy:

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/ipsec_policies?version=$api_version&generation=2" \
        -H "Authorization: Bearer $iam_token" \
        -d '{
           "name": "my-new-ipsec-policy",
           "authentication_algorithms": ["sha256", "sha512"],
           "encryption_algorithms": ["aes128", "aes256"],
           "pfs_groups": ["group_14", "group_15", "group_16", "group_17", "group_18"],
           "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}

Create an IPsec policy with `encryption_algorithms` set to GCM-based algorithms and `authentication_algorithms` set to `disabled`:

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/ipsec_policies?version=$api_version&generation=2" \
      -H "Authorization: Bearer $iam_token" \
      -d '{
         "name": "gcm-ipsec-policy",
         "encryption_algorithms": ["aes128gcm16","aes192gcm16","aes256gcm16"],
         "authentication_algorithms": ["disabled"],
         "pfs_groups": ["group_14","group_15","group_16","group_17","group_18"],
         "resource_group": {
           "id": "'$ResourceGroupId'"
         }
       }'
   ```
   {: codeblock}

   The `authentication_algorithms` property must be set to `disabled` if and only if the `encryption_algorithms` list contains any GCM-based algorithms (`aes128gcm16`, `aes192gcm16`, or `aes256gcm16`). GCM algorithms provide both encryption and built-in data integrity (authentication) as part of a single operation, so a separate authentication algorithm (such as `sha256` or `sha512`) must not be specified.
   {: note}

To create an IPsec policy with the API by using singular properties (deprecated), follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

   ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
   {: pre}

1. Create the IPsec policy:

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/ipsec_policies?version=$api_version&generation=2" \
        -H "Authorization: Bearer $iam_token" \
        -d '{
           "name": "my-new-ipsec-policy",
           "authentication_algorithm": "sha256",
           "encryption_algorithm": "aes128",
           "pfs": "group_14",
           "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}

## Updating an IPsec policy with the API
{: #vpn-using-api-update-ipsec-policy}
{: api}

To update an IPsec policy with the API by using the array-based properties (recommended), follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

   ```sh
   export ResourceGroupId=<your_resourcegroup_id>
   ```
   {: pre}

1. Update the IPsec policy:

   ```sh
      curl -X PATCH "$vpc_api_endpoint/v1/ipsec_policies/$ipsec_policy_id?version=$api_version&generation=2" \
        -H "Authorization: Bearer $iam_token" \
        -d '{
            "name": "my-updated-ipsec-policy",
            "authentication_algorithms": ["sha512", "sha384", "sha256"],
            "encryption_algorithms": ["aes256", "aes128"],
            "pfs_groups": ["group_18", "group_17", "group_16", "group_15", "group_14"],
         }'
   ```
   {: codeblock}

To update an IPsec policy with the API by using singular properties (deprecated), follow these steps:

   You can update IPsec policies by using either deprecated singular properties or array-based properties. Both forms are accepted, but you must not mix singular and array-based properties for the same algorithm category in a single request.
   {: note}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

   ```sh
   export ResourceGroupId=<your_resourcegroup_id>
   ```
   {: pre}

1. Update the IPsec policy:

   ```sh
      curl -X PATCH "$vpc_api_endpoint/v1/ipsec_policies/$ipsec_policy_id?version=$api_version&generation=2" \
        -H "Authorization: Bearer $iam_token" \
        -d '{
            "name": "my-updated-ipsec-policy",
            "authentication_algorithm": "sha512",
            "encryption_algorithm": "aes256",
              "pfs": "group_18",
         }'
   ```
   {: codeblock}

To view the complete set of APIs for site-to-site VPN gateways, see the [VPC API reference](/apidocs/vpc/latest#create-ipsec-policy).
{: tip}

## Next steps
{: #vpn-create-ipsec-next-steps}

After you create an IPsec policy, complete the following tasks as needed:

* [Create an IKE policy](/docs/vpc?topic=vpc-creating-ike-policy) if you decide to use a custom IKE policy instead of an auto-negotiated IKE policy.
* If you didn't create a VPN connection during VPN gateway creation, you can create one after the gateway is provisioned. For more information, see [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections).
* For a route-based VPN, select or [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table). Then, [create a route using the VPN connection type](/docs/vpc?topic=vpc-create-vpc-route).

## Related links
{: #related-links-vpn-ipsec-policy}

* [Upgrading weak cipher suites on a VPN gateway](/docs/vpc?topic=vpc-upgrading-weak-ciphers&interface=ui).
* [Changes to VPN gateway IKE and IPsec policy API](/docs/vpc?topic=vpc-vpn-hcr-introduction&interface=ui).
* [How do I check IPsec logs?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-19)
* [Why does my IPsec connection status appear to be up but the BGP state isn't established?](/vpc?topic=vpc-troubleshoot-ipsec-up-bgp-not-established)
