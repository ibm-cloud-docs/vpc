---

copyright:
  years: 2026
lastupdated: "2026-06-26"

keywords: VPN, IKE policy, IPsec policy, multiple algorithms, migration, deprecated, authentication algorithms, encryption algorithms, dh groups

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to multiple IKE and IPsec algorithms
{: #vpn-update-multiple-algorithms}

VPN for VPC supports configuring multiple algorithms for IKE and IPsec policies. Using multiple algorithms can improve compatibility, flexibility, and security.
{: shortdesc}

Use the following instructions to migrate from deprecated, singular algorithm properties to multiple  algorithm properties.

## Before you begin
{: #vpn-update-algorithms-prereqs}

Review the following information before you update your VPN policies.

### Deprecated singular algorithm properties
{: #deprecated-singular-algorithm-properties}

As of 29 May 2026, the VPN IKE and IPsec singular algorithm properties are deprecated. Existing VPN connections configured with singular properties continue to function without changes. However, it is recommended to update to array-based algorithm properties as soon as possible.
{: deprecated}

| IKE singular property (deprecated) | IKE multiple property (recommended)|
|------------------------------------|------------------------------------|
| `authentication_algorithm`         | `authentication_algorithms`        |
| `encryption_algorithm`             | `encryption_algorithms`            |
| `dh_group`                         | `dh_groups`                        |
{: caption="Singular and array-based algorithm properties for IKE policies" caption-side="bottom"}

| IPsec singular property (deprecated) | IPsec multiple property (recommended)|
|--------------------------------------|--------------------------------------|
| `authentication_algorithm`           | `authentication_algorithms`          |
| `encryption_algorithm`               | `encryption_algorithms`              |
| `pfs`                                | `pfs_groups`                         |
{: caption="Singular and array-based algorithm properties for IPsec policies" caption-side="bottom"}

Use multiple algorithms when your VPN environment requires compatibility with peers that support different cryptographic algorithms or when you want to prioritize stronger encryption while maintaining backward compatibility.
{: tip}

### Important considerations
{: #important-considerations}

* VPN connections that use custom IKE or IPsec policies with deprecated singular algorithm properties might experience negotiation failures or interoperability issues.
* New IKE and IPsec policies can be created only by using array-based algorithm properties.
* When you update an existing IKE or IPsec policy, it might temporarily disconnect the VPN tunnel while the connection is re-established.
* If your disaster recovery procedures, automation, scripts, API integrations, or CLI workflows reference deprecated singular properties, update them accordingly.
* Review your current IKE and IPsec policies to identify the algorithms in use.
* Verify that the peer VPN gateway supports the algorithms that you plan to configure on IBM Cloud.
* Configure matching algorithms on the peer VPN gateway for IKE and IPsec negotiation before updating the IBM Cloud VPN gateway.
* Plan policy updates during a maintenance window to minimize service disruption.

### Understanding update behavior in the API
{: #vpn-update-algorithms-behavior}
{: api}

The API automatically synchronizes singular and array-based algorithm properties. Review the following behavior and restrictions before updating your policies.

IKE version compatibility
:   Array-based algorithm properties are supported only with IKEv2. If you use IKEv1, you must configure a single algorithm for each category using singular properties.

   - Use IKEv2 with array-based properties:

   ```json
   {
      "ike_version": 2,
      "authentication_algorithms": ["sha512", "sha256"],
      "encryption_algorithms": ["aes256", "aes128"]
   }
   ```
   {: codeblock}

   - Use IKEv1 with singular properties:

   ```json
   {
      "ike_version": 1,
      "authentication_algorithm": "sha256",
      "encryption_algorithm": "aes128"
   }
   ```
   {: codeblock}

   IKEv1 supports only one algorithm per category. VPN connections that use IKEv1 don't support array-based IKE or IPsec algorithm properties. Use IKEv2 whenever possible.
   {: note}

Automatic synchronization
:   When you update array-based properties (`authentication_algorithms`, `encryption_algorithms`, `dh_groups`, `pfs_groups`), the corresponding singular properties (`authentication_algorithm`, `encryption_algorithm`, `dh_group`, `pfs`) are updated automatically. Similarly, updating singular properties automatically updates the related array-based properties. For examples, see:

   * [IKE policy update examples](/docs/vpc?topic=vpc-vpn-update-multiple-algorithms&interface=api#vpn-ike-policy-patch-examples)
   * [IPsec policy update examples](/docs/vpc?topic=vpc-vpn-update-multiple-algorithms&interface=api#vpn-ipsec-policy-patch-examples)

Read-only indicators
:   When multiple algorithms are configured, the singular properties (`authentication_algorithm`, `encryption_algorithm`) return read-only indicator values in API responses. For example:

   * `authentication_algorithm` returns `"multiple"`
   * `encryption_algorithm` returns `"multiple"`
   * `dh_group` returns `65535`

   These values are returned only in responses and can't be submitted in a `PATCH` request.

   ```json
   {
      "authentication_algorithms": ["sha256", "sha384", "sha512"],
      "authentication_algorithm": "multiple",
      "dh_groups": [14, 15, 16],
      "dh_group": 65535,
      "encryption_algorithms": ["aes128", "aes256"],
      "encryption_algorithm": "multiple"
   }
   ```
   {: codeblock}

   If only one algorithm is configured in an array-based property, the singular property returns the configured value instead of `"multiple"`.

   ```json
   {
      "authentication_algorithms": ["sha256"],
      "authentication_algorithm": "sha256",
      "dh_groups": [14],
      "dh_group": 14,
      "encryption_algorithms": ["aes128"],
      "encryption_algorithm": "aes128"
   }
   ```
   {: codeblock}

Property mixing restriction
:   Do not mix singular and array-based properties for the same algorithm category in a single request. Choose one approach per request.

   - Correct example: This example includes the algorithm properties for IKE, where only array-based properties are used for all categories:

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/ike_policies/$ike_policy_id?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
        "authentication_algorithms": ["sha256", "sha384", "sha512"],
        "dh_groups": [14, 15, 16],
        "encryption_algorithms": ["aes128", "aes256"]
      }'
   ```
   {: codeblock}

   - Incorrect example: This example includes the algorithm properties for IKE, where both singular and array-based properties are mixed for the same category:

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/ike_policies/$ike_policy_id?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
        "authentication_algorithm": "sha512",
        "authentication_algorithms": ["sha256", "sha384", "sha512"],
        "dh_groups": [14, 15, 16],
        "encryption_algorithms": ["aes128", "aes256"]
      }'
   ```
   {: codeblock}

GCM algorithm requirements and restrictions
:   If the `encryption_algorithms` property contains GCM-based algorithms (`aes128gcm16`, `aes192gcm16`, or `aes256gcm16`), the `authentication_algorithms` property must be set to `["disabled"]`.

   - Correct example: Shows IPsec algorithm properties when the `encryption_algorithms` array includes GCM-based algorithms:

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/ipsec_policies/$ipsec_policy_id?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
        "authentication_algorithms": ["disabled"],
        "encryption_algorithms": ["aes256gcm16", "aes192gcm16"],
        "pfs_groups": ["group_14", "group_15", "group_16"]
      }'
   ```
    {: codeblock}

   - Incorrect example: Shows algorithm properties for IPsec where the `encryption_algorithms` array includes GCM-based algorithms:

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/ipsec_policies/$ipsec_policy_id?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
        "authentication_algorithms": ["sha256"],
        "encryption_algorithms": ["aes256gcm16", "aes192gcm16"],
        "pfs_groups": ["group_14", "group_15", "group_16"]
      }'
   ```
   {: codeblock}

   GCM algorithms provide encryption and built-in authentication as part of a single operation. Do not configure separate authentication algorithms such as `sha256` or `sha512` with GCM encryption algorithms.

   Do not mix GCM and non-GCM encryption algorithms in the same policy. Mixing them can create configuration conflicts and negotiation failures.
   {: note}

:   When `authentication_algorithms` is set to `["disabled"]`, you must also update `authentication_algorithms` when changing from GCM to non-GCM encryption algorithms.

   - Correct example: Updates both the `encryption_algorithms` and `authentication_algorithms` properties:

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/ipsec_policies/$ipsec_policy_id?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
        "authentication_algorithms": ["sha512", "sha256"],
        "encryption_algorithms": ["aes256", "aes128"],
        "pfs_groups": ["group_14", "group_15", "group_16"]
      }'
   ```
   {: codeblock}

   - Incorrect example: Shows a case where only the `encryption_algorithms` property is updated with non-GCM algorithms (`aes128`, `aes192`, `aes256`):

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/ipsec_policies/$ipsec_policy_id?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
        "authentication_algorithms": ["disabled"],
        "encryption_algorithms": ["aes256", "aes128"],
        "pfs_groups": ["group_14", "group_15", "group_16"]
      }'
   ```
   {: codeblock}

Algorithm validation rules
:   Ensure that all algorithm values in your request are valid, unique, and not empty. Use only supported algorithm names, avoid duplicates, and provide at least one value in each array. Invalid, duplicate, or empty entries can result in a validation error.

   - Correct example: Updates the algorithm values, where supported algorithms and values are used for all categories:

   ```sh
   {
      "authentication_algorithms": ["sha512", "sha256"],
      "dh_groups": [14, 15],
      "encryption_algorithms": ["aes256", "aes192", "aes128"]
   }
   ```
   {: codeblock}

   - Incorrect example: Updates the algorithm values, where duplicate values are used in the same algorithm category:

   ```sh
   {
      "authentication_algorithms": ["sha512", "sha256"],
      "dh_groups": [14, 15],
      "encryption_algorithms": ["aes256", "aes256", "aes128"]
   }
   ```
   {: codeblock}

## Updating IKE policies with the API
{: #update-ike-policies-api}
{: api}

Follow these steps to update your IKE policies from singular to multiple algorithm.

Before you begin, make sure to [set up your API environment](/docs/vpc?topic=vpc-set-up-environment&interface=api#cli-prerequisites-setup).

To update an IKE policy with the API, follow these steps:

1. Store the IKE policy ID in a variable, for example:

    ```sh
    export ike_policy_id=<your_ike_policy_id>
    ```
    {: codeblock}

    To find the IKE policy ID, use the [list IKE policies](/docs/apis/vpc/latest#list-ike-policies) command.

1. Update the IKE policy to use multiple algorithms. Replace singular properties with the corresponding array-based properties.

   `authentication_algorithms` - Array of authentication algorithms. Options: `sha256`, `sha384`, `sha512`.

   `dh_groups` - Array of Diffie-Hellman groups. Options: `14`, `15`, `16`, `17`, `18`, `19`, `20`, `21`, `22`, `23`, `24`, `31`.

   `encryption_algorithms` - Array of encryption algorithms. Options: `aes128`, `aes192`, `aes256`.

    ```sh
    curl -X PATCH "$vpc_api_endpoint/v1/ike_policies/$ike_policy_id?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
        "authentication_algorithms": ["sha256", "sha384", "sha512"],
        "dh_groups": [14, 15, 16],
        "encryption_algorithms": ["aes128", "aes256"]
      }'
    ```
    {: codeblock}

### IKE policy update examples
{: #vpn-ike-policy-patch-examples}

When you update the array‑based properties `encryption_algorithms`, `authentication_algorithms`, and `dh_groups` in the IKE policy, the corresponding singular properties `encryption_algorithm`, `authentication_algorithm`, and `dh_group` are automatically updated. Similarly, when you update any of the singular properties, the associated array‑based properties are also automatically updated.

The following example shows how `PATCH` requests affect both singular and array properties:

#### Example 1: Single to multiple algorithms
{: #vpn-ike-policy-patch-example-1}

- This example shows the patching behavior from single to multiple algorithms. The current state shows the single‑value property `encryption_algorithm` set to `aes128`, and the array‑based property `encryption_algorithms` also containing only `aes128`.

   ```json
      {
      "authentication_algorithm": "sha256",
      "authentication_algorithms": [
         "sha256"
      ],
      "created_at": "2025-03-09T01:40:25.782663Z",
      "dh_group": 14,
      "dh_groups": [
         14
      ],
      "encryption_algorithm": "aes128",
      "encryption_algorithms": [
         "aes128"
      ],
      "id": "r006-e98f46a3-1e4e-4195-b4e5-b8155192689d",
      "ike_version": 2,
      "key_lifetime": 28800,
      "name": "my-ike-policy",
      "negotiation_mode": "main",
      "resource_group": {
         "id": "fee82deba12e4c0fb69c3b09d1f12345",
         "name": "Default"
      },
      "resource_type": "ike_policy"
      }
   ```
   {: codeblock}

- In the `PATCH` request, the array-based property `encryption_algorithms` is updated so that it now specifies both `aes128` and `aes256`.

   ```json
      {
      "encryption_algorithms": [
         "aes128",
         "aes256"
      ]
      }
   ```
   {: codeblock}

- A successful response looks like the following example. In this response, the singular property `encryption_algorithm` returns a read-only value `"multiple"`, indicating that multiple algorithms are now configured. This field can't be modified directly through a `PATCH` request. The array‑based property `encryption_algorithms` is updated to include both `aes128` and `aes256`.

   ```json
      {
      "authentication_algorithm": "sha256",
      "authentication_algorithms": [
         "sha256"
      ],
      "created_at": "2025-03-09T01:40:25.782663Z",
      "dh_group": 14,
      "dh_groups": [
         14
      ],
      "encryption_algorithm": "multiple",
      "encryption_algorithms": [
         "aes128",
         "aes256"
      ],
      "id": "r006-e98f46a3-1e4e-4195-b4e5-b8155192689d",
      "ike_version": 2,
      "key_lifetime": 28800,
      "name": "my-ike-policy",
      "negotiation_mode": "main",
      "resource_group": {
         "id": "fee82deba12e4c0fb69c3b09d1f12345",
         "name": "Default"
      },
      "resource_type": "ike_policy"
      }
   ```
   {: codeblock}

#### Example 2: Multiple algorithms to single
{: #vpn-ike-policy-patch-example-2}

- This example shows the patching behavior from multiple to a single algorithm. The current state shows the single‑value property `encryption_algorithm` set to `"multiple"`, and the array‑based property `encryption_algorithms` containing `aes128` and `aes256`.

   ```json
      {
      "authentication_algorithm": "sha256",
      "authentication_algorithms": [
         "sha256"
      ],
      "created_at": "2025-03-09T01:40:25.782663Z",
      "dh_group": 14,
      "dh_groups": [
         14
      ],
      "encryption_algorithm": "multiple",
      "encryption_algorithms": [
         "aes128",
         "aes256"
      ],
      "id": "r006-e98f46a3-1e4e-4195-b4e5-b8155192689d",
      "ike_version": 2,
      "key_lifetime": 28800,
      "name": "my-ike-policy",
      "negotiation_mode": "main",
      "resource_group": {
         "id": "fee82deba12e4c0fb69c3b09d1f12345",
         "name": "Default"
      },
      "resource_type": "ike_policy"
      }
   ```
   {: codeblock}

- In the `PATCH` request, the array-based property `encryption_algorithms` is updated so that it now specifies only `aes256`.

   ```json
      {
      "encryption_algorithms": [
         "aes256"
      ]
      }
   ```
   {: codeblock}

- A successful response looks like the following example. In this response, the single‑value property `encryption_algorithm` is automatically set to `aes256`, indicating that only a single algorithm is configured. The array‑based property `encryption_algorithms` is updated to include `aes256`.

   ```json
      {
      "authentication_algorithm": "sha256",
      "authentication_algorithms": [
         "sha256"
      ],
      "created_at": "2025-03-09T01:40:25.782663Z",
      "dh_group": 14,
      "dh_groups": [
         14
      ],
      "encryption_algorithm": "aes256",
      "encryption_algorithms": [
         "aes256"
      ],
      "id": "r006-e98f46a3-1e4e-4195-b4e5-b8155192689d",
      "ike_version": 2,
      "key_lifetime": 28800,
      "name": "my-ike-policy",
      "negotiation_mode": "main",
      "resource_group": {
         "id": "fee82deba12e4c0fb69c3b09d1f12345",
         "name": "Default"
      },
      "resource_type": "ike_policy"
      }
   ```
   {: codeblock}

## Updating IPsec policies with the API
{: #update-ipsec-policies-api}
{: api}

Follow these steps to update your IPsec policies from singular to array-based algorithm properties.

Before you begin, make sure to [set up your API environment](/docs/vpc?topic=vpc-set-up-environment&interface=api#cli-prerequisites-setup).

To update an IPsec policy with the API, follow these steps:

1. Store the IPsec policy ID in a variable, for example:

    ```sh
    export ipsec_policy_id=<your_ipsec_policy_id>
    ```
    {: codeblock}

    To find the IPsec policy ID, use the [list IPsec policies](/docs/apis/vpc/latest#list-ipsec-policies) command.

1. Update the IPsec policy to use multiple algorithms. Replace singular properties with the corresponding array-based properties.

   `authentication_algorithms` - Array of authentication algorithms. Options: `sha256`, `sha384`, `sha512`, `disabled`.

   `encryption_algorithms` - Array of encryption algorithms. Options: `aes128`, `aes192`, `aes256`, `aes128gcm16`, `aes192gcm16`, `aes256gcm16`.

   `pfs_groups` - Array of Perfect Forward Secrecy groups. Options: `disabled`, `group_14`, `group_15`, `group_16`, `group_17`, `group_18`, `group_19`, `group_20`, `group_21`, `group_22`, `group_23`, `group_24`, `group_31`.

    ```sh
    curl -X PATCH "$vpc_api_endpoint/v1/ipsec_policies/$ipsec_policy_id?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
        "authentication_algorithms": ["sha256", "sha384", "sha512"],
        "encryption_algorithms": ["aes128", "aes256"],
        "pfs_groups": ["group_14", "group_15", "group_16"]
      }'
    ```
    {: codeblock}

### IPsec policy update examples
{: #vpn-ipsec-policy-patch-examples}
{: api}

When you update the array‑based properties `encryption_algorithms`, `authentication_algorithms`, and `pfs_groups` in an IPsec policy, the corresponding single‑value properties `encryption_algorithm`, `authentication_algorithm`, and `pfs` are automatically updated. Similarly, when you update any of the single‑value properties, the associated array‑based properties are also automatically updated.

The following example shows how `PATCH` requests affect both singular and array properties:

#### Example 1: Single to multiple algorithms
{: #vpn-ipsec-policy-patch-example-1}

- This example shows the patching behavior from single to multiple algorithms. The current state shows the single‑value property `encryption_algorithm` set to `aes128`, and the array‑based property `encryption_algorithms` also containing only `aes128`.

   ```json
      {
      "authentication_algorithm": "sha256",
      "authentication_algorithms": [
         "sha256"
      ],
      "connections": [],
      "created_at": "2025-03-09T01:46:00.785105Z",
      "encapsulation_mode": "tunnel",
      "encryption_algorithm": "aes128",
      "encryption_algorithms": [
         "aes128"
      ],
      "id": "r006-51eae621-dbbc-4c47-b623-b57a43c19876",
      "key_lifetime": 3600,
      "name": "my-ipsec-policy",
      "pfs": "group_14",
      "pfs_groups": [
         "group_14"
      ],
      "resource_group": {
         "id": "fee82deba12e4c0fb69c3b09d1f12345",
         "name": "Default"
      },
      "resource_type": "ipsec_policy",
      "transform_protocol": "esp"
      }
   ```
   {: codeblock}

- In the `PATCH` request, the array-based property `encryption_algorithms` is updated so that it now specifies both `aes128` and `aes256`.

   ```json
      {
      "encryption_algorithms": [
         "aes128",
         "aes256"
      ]
      }
   ```
   {: codeblock}

- A successful response looks like the following example. In this response, the single‑value property `encryption_algorithm` is automatically set to the read‑only value `"multiple"`, indicating that multiple algorithms are now configured. This field can't be modified directly through a `PATCH` request. The array‑based property `encryption_algorithms` now includes both `aes128` and `aes256`.

   ```json
      {
      "authentication_algorithm": "sha256",
      "authentication_algorithms": [
         "sha256"
      ],
      "connections": [],
      "created_at": "2025-03-09T01:46:00.785105Z",
      "encapsulation_mode": "tunnel",
      "encryption_algorithm": "multiple",
      "encryption_algorithms": [
         "aes128",
         "aes256"
      ],
      "id": "r006-51eae621-dbbc-4c47-b623-b57a43c19876",
      "key_lifetime": 3600,
      "name": "my-ipsec-policy",
      "pfs": "group_14",
      "pfs_groups": [
         "group_14"
      ],
      "resource_group": {
         "id": "fee82deba12e4c0fb69c3b09d1f12345",
         "name": "Default"
      },
      "resource_type": "ipsec_policy",
      "transform_protocol": "esp"
      }
   ```
   {: codeblock}

#### Example 2: Multiple algorithms to single
{: #vpn-ipsec-policy-patch-example-2}

- This example shows the patching behavior from multiple to a single algorithm. The current state shows the single‑value property `encryption_algorithm` set to `"multiple"`, and the array‑based property `encryption_algorithms` containing `aes128` and `aes256`.

   ```json
      {
      "authentication_algorithm": "sha256",
      "authentication_algorithms": [
         "sha256"
      ],
      "connections": [],
      "created_at": "2025-03-09T01:46:00.785105Z",
      "encapsulation_mode": "tunnel",
      "encryption_algorithm": "multiple",
      "encryption_algorithms": [
         "aes128",
         "aes256"
      ],
      "id": "r006-51eae621-dbbc-4c47-b623-b57a43c19876",
      "key_lifetime": 3600,
      "name": "my-ipsec-policy",
      "pfs": "group_14",
      "pfs_groups": [
         "group_14"
      ],
      "resource_group": {
         "id": "fee82deba12e4c0fb69c3b09d1f12345",
         "name": "Default"
      },
      "resource_type": "ipsec_policy",
      "transform_protocol": "esp"
      }
   ```
   {: codeblock}

- In the `PATCH` request, the array-based property `encryption_algorithms` is updated so that it now specifies only `aes256`.

   ```json
      {
      "encryption_algorithms": [
         "aes256"
      ]
      }
   ```
   {: codeblock}

- A successful response looks like the following example. In this response, the single‑value property `encryption_algorithm` is automatically set to `aes256`, indicating that only a single algorithm is configured. The array‑based property `encryption_algorithms` is updated to include `aes256`.

  ```json
      {
      "authentication_algorithm": "sha256",
      "authentication_algorithms": [
         "sha256"
      ],
      "connections": [],
      "created_at": "2025-03-09T01:46:00.785105Z",
      "encapsulation_mode": "tunnel",
      "encryption_algorithm": "aes256",
      "encryption_algorithms": [
         "aes256"
      ],
      "id": "r006-51eae621-dbbc-4c47-b623-b57a43c19876",
      "key_lifetime": 3600,
      "name": "my-ipsec-policy",
      "pfs": "group_14",
      "pfs_groups": [
         "group_14"
      ],
      "resource_group": {
         "id": "fee82deba12e4c0fb69c3b09d1f12345",
         "name": "Default"
      },
      "resource_type": "ipsec_policy",
      "transform_protocol": "esp"
      }
   ```
   {: codeblock}

## Related links
{: #related-links-vpn-multiple-algorithms}

* [Creating an IKE policy](/docs/vpc?topic=vpc-creating-ike-policy)
* [Creating an IPsec policy](/docs/vpc?topic=vpc-creating-ipsec-policy)
* [How are encryption algorithms chosen for IKE and IPsec in a site-to-site VPN connection?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-40)
* [How do I check IPsec logs?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-41)
