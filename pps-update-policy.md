---

copyright:
  years: 2024
lastupdated: "2024-02-08"

keywords: private path

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating and deleting an account policy
{: #pps-update-account}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

As a service provider, you are responsible for managing your consumer account IDs. Currently, tracking or validating account IDs is not supported. For more information, see [Responsibilities for managing consumer account IDs](/docs/vpc?topic=vpc-pps-consumer-account-id-responsibilities&interface=ui).
{: attention}

You can update or delete a Private Path service account policy at any time using the UI, CLI, or API.
{: shortdesc}

## Updating and deleting an account policy in the UI
{: #pps-ui-update-account-policy}
{: ui}

To update an account policy in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![navigation menu](../icons/icon_hamburger.svg), then click **VPC Infrastructure**.
1. Click **Private Path services** in the Network section.
1. Click the name of the Private Path service that includes the account policy that you want to update.
1. On the Private Path service's Details page, click the Account policies tab.
1. Locate the account with the policy that you want to update, then click the Edit ![Edit icon](/images/edit.png) or Delete ![Delete icon](/images/delete.png) icon.

   In the Edit policy panel, you can update the policy name and the request policy (**Review**, **Permit**, or **Deny**).

1. Click **Save** to save your changes.

## Updating and deleting an account policy from the CLI
{: #pps-cli-update-account-policy}
{: cli}

The following examples show how to use the CLI to update and delete an account policy.

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli){: external}.

You must export the feature flag for Private Path service and Private Path network load balancer related commands to run successfully.
{: important}

To export the feature flag, enter the following commands:

```sh
export IBMCLOUD_IS_FEATURE_PRIVATE_PATH_SERVICE_GATEWAY=true
export IBMCLOUD_IS_FEATURE_PP_NLB_SUPPORT=true
```
{: pre}

### Updating an account policy from the CLI
{: #pps-cli-howto-update-account-policy}
{: cli}

To update an account policy from the CLI, follow these steps:

1. Enter the following command:

```sh
 ibmcloud is private-path-service-gateway-account-policy-update PRIVATE_PATH_SERVICE_GATEWAY ACCOUNT_POLICY [--access-policy deny | permit | review]
[--output JSON] [-q, --quiet]
```
{: pre}

Where:

`PRIVATE_PATH_SERVICE_GATEWAY`
:   Indicates the ID or name of the Private Path service.

`ACCOUNT_POLICY`
:   Indicates the ID of the account policy for the Private Path service.

`access-policy`
:   Indicates the access policy for the account. One of: deny, permit, review.

`--output`
:   Specify the output format, only JSON is supported. One of: JSON.

`-q, --quiet`
:   Suppress verbose output.

### Deleting an account policy from the CLI
{: #pps-cli-delete-account-policy}
{: cli}

To delete one or more account policies from the CLI, follow these steps:

1. Enter the following command:

```sh
ibmcloud is private-path-service-gateway-account-policy-delete PRIVATE_PATH_SERVICE_GATEWAY (ACCOUNT_POLICY1 ACCOUNT_POLICY2 ...)
    [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

Where:

`PRIVATE_PATH_SERVICE_GATEWAY`
:   Identifies ID or name of the Private Path service.

`ACCOUNT_POLICY1`
:   Identifies ID of the account policy for the Private Path service.

`ACCOUNT_POLICY2`
:   Identifies ID of the account policy for the Private Path service.

`--output`
:   Specifies output format, only JSON is supported. One of: `JSON`.

`--force, -f`
:   Forces the operation without confirmation.

`-q, --quiet`
:   Suppresses verbose output.

### Command examples
{: #cli-cmd-examples-update-delete-account-policy}
{: cli}

- Update to a permit policy:
   `ibmcloud is ppsg-apu r006-f5926633-81d0-429e-bcf8-91151ade00bf 2d1bace7b46e4815a81e52c6ffeba5cf`

- Update to a review policy:
   `ibmcloud is ppsg-apu cli-ppsg-0 e13b4574db1743b1b7897bebca551db1 --access-policy review`

- Delete an account policy:
   `ibmcloud is ppsg-apd r006-2e671f14-19fc-4089-9ad3-973176711259 efe5afc483594adaa8325e2b4d1290df`

- Delete an account policy:
   `ibmcloud is private-path-service-gateway-account-policy-delete cli-ppsg-0 2d1bace7b46e4815a81e52c6ffeba5cf e13b4574db1743b1b7897bebca551db1`

## Updating an account policy with the API
{: #pps-api-update-account-policy}
{: api}

To update an account policy with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup){: external}.
1. Store the following values in variables to be used in the API command:

   * `ppsgId` - Get your Private Path service and then populate the variable:

      ```sh
      export ppsgId=<your_ppsg_id>
      ```
      {: pre}

   * `accountPolicyId` - Get your account policy ID and then populate the variable:

      ```sh
      export accountPolicyId=<your_account_policy_id>
      ```
      {: pre}

The following example shows how to use the API to update an account policy.

      ```sh
      curl -X PATCH -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways/$ppsgId/account_policies/$accountPolicyId?version=$api_version&generation=2" \
      -d {
        "access_policy": "deny"
      }
      '
      ```
      {: codeblock}

The following example shows how to use the API to delete an account policy.

      ```sh
      curl -X DELETE -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways/$ppsgId/account_policies/$accountPolicyId?version=$api_version&generation=2"
      ```
      {: codeblock}

## Related links
{: #pps-update-policies-related-links}

- [About account policies](/docs/vpc?topic=vpc-pps-about-account-policies&interface=ui)
- [Creating an account policy](/docs/vpc?topic=vpc-pps-create-account-policy&interface=ui)
