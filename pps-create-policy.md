---

copyright:
  years: 2024
lastupdated: "2024-11-12"

keywords: private path

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an account policy
{: #pps-create-account-policy}

As a service provider, you are responsible for managing your consumer account IDs. Currently, tracking or validating account IDs is not supported. For more information, see [Responsibilities for managing consumer account IDs](/docs/vpc?topic=vpc-pps-consumer-account-id-responsibilities&interface=ui).
{: attention}

You can create a policy for a specific account ID. This is advantageous when you want a different action for an account than what is set for the default policy. For example, if you set the default policy to **Review all requests**, but you want to automatically **Permit** requests from ID `Lauren`, you can create an account policy to bypass triaging requests from that ID.
{: shortdesc}

You can create an account policy to review, accept, or reject connection requests using the UI, CLI, API, or Terraform.

## Creating an account policy in the UI
{: #pps-ui-create-account-policy}
{: ui}

To create a Private Path service policy in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation Menu** ![Menu icon](images/menu_icon.png), then click **Infrastructure > Network > Private Path services**.
1. Click the name of the Private Path service where you want to add an account policy.
1. On the Private Path service's Details page, click the Account policies tab.
1. In the Account policies table, click **Create policy**.
1. In the Create policy window that appears, enter the following information:

    * Account ID - Enter the account ID for which you are creating an account policy.
    * Policy name - Optionally, enter a unique identifier to name your policy.
    * Request policy - Select whether to automatically **Review**, **Permit**, or **Deny** all incoming connection requests from the specified account ID.
1. Click **Create**. Your new account policy appears as a row in the Account policies table.

## Creating an account policy from the CLI
{: #pps-cli-create-account-policy}
{: cli}

The following example shows how create an account policy from the CLI.

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create an account policy from the CLI, follow these steps:

1. Enter the following command:

```sh
ibmcloud is private-path-service-gateway-account-policy-create PRIVATE_PATH_SERVICE_GATEWAY --account-id ACCOUNT_ID
    [--access-policy deny | permit | review]
    [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`PRIVATE_PATH_SERVICE_GATEWAY`
:   Inicates ID or name of the Private Path service.

`--account-id`
:   Indicates ID of the account for this access policy.

`--access-policy`
:   Indicates the access policy for the account. One of: deny, permit, review.

`--output`
:   Specifies output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppresses verbose output.

### Command examples
{: #cli-cmd-examples-create-account-policy}
{: cli}

- Create a deny policy:
   `ibmcloud is private-path-service-gateway-account-policy-create r006-f5926633-81d0-429e-bcf8-91151ade00bf --account-id e13b4574db1743b1b7897bebca551db1 --access-policy deny`

- Create a review policy:
   `ibmcloud is private-path-service-gateway-account-policy-create cli-ppsg-0 --account-id 2d1bace7b46e4815a81e52c6ffeba5cf --access-policy review`

## Creating an account policy with the API
{: #pps-api-create-account-policy}
{: api}

To create an account policy with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup){: external}.
1. Store the following values in variables to be used in the API command:

   * `ppsgId` - Get your Private Path service and then populate the variable:

      ```sh
      export ppsgId=<your_ppsg_id>
      ```
      {: pre}

   * `accountId` - Get consumer account ID and then populate the variable:

      ```sh
      export accountId=<consumer_account_id>
      ```
      {: pre}

The following example shows how to create an account policy with the API.

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways/$ppsgId/account_policies?version=$api_version&generation=2" \
      -d {
        "access_policy": "review",
        "account": {
          "id": "$accountId"
        }
      }'
      ```
      {: codeblock}

## Creating an account policy with Terraform
{: #pps-create-account-policy-terraform}
{: terraform}

The following example updates or deletes an account policy's access to a Private Path network by using Terraform:

```terraform
resource "ibm_is_private_path_service_gateway_account_policy" "ppsgAccountPolicy" {
    private_path_service_gateway = ibm_is_private_path_service_gateway.ppsg.id
    access_policy = "permit"    ## modified to deny
    account = "7f75c7b025e54bc5635f754b2f888665"
}
```
{: codeblock}

## Related links
{: #pps-create-policies-related-links}

- [About account policies](/docs/vpc?topic=vpc-pps-about-account-policies&interface=ui)
- [Updating and deleting an account policy](/docs/vpc?topic=vpc-pps-update-account&interface=ui)
