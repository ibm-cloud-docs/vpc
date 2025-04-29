---

copyright:
  years: 2024, 2025
lastupdated: "2025-04-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Revoking an account's access to a Private Path service
{: #pps-ui-revoke-account}

As a service provider, you are responsible for managing your consumer account IDs. Currently, tracking or validating account IDs is not supported. For more information, see [Responsibilities for managing consumer account IDs](/docs/vpc?topic=vpc-pps-consumer-account-id-responsibilities&interface=ui).
{: attention}

You can revoke an account's access to a Private Path service using the console, CLI, API, or Terraform.
{: shortdesc}

## Revoking an account's access to a Private Path service in the console
{: #pps-ui-revoke-account-path-service}
{: ui}

To Revoke an account's access to a Private Path service in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure > Network > Private Path services**.
1. Click the name of the Private Path service that includes the account policy that you want to update.
1. On the Private Path service's Details page, click the Connections tab.
1. Locate the Account ID of the account you want to revoke, and click the Menu icon ![navigation menu](../icons/icon_hamburger.svg) at the end of the row.
1. Click **Revoke**.

## Revoking an account's access to a Private Path service from the CLI
{: #pps-cli-revoke-account-path-service}
{: cli}

The following example shows how to use the CLI to revoke an account's access to a Private Path service.

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To delete a Private Path service from the CLI, follow these steps:

1. Enter the following command:

```sh
ibmcloud is private-path-service-gateway-endpoint-gateway-binding-revoke (PRIVATE_PATH_SERVICE_GATEWAY1 PRIVATE_PATH_SERVICE_GATEWAY2 ...) --account-id ACCOUNT_ID [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`PRIVATE_PATH_SERVICE_GATEWAY1`
:   Indicates the ID or name of the Private Path service.

`PRIVATE_PATH_SERVICE_GATEWAY2`
:   Indicates the ID or name of the Private Path service.

`--account-id`
:   Indicates the ID of the account for this access policy.

`--output`
:   Specifies output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppresses verbose output.

### Command examples
{: #cli-cmd-examples-revoke-account-private-path-service}
{: cli}

- Revoke access to a Private Path service for an account:
   `ibmcloud is ppsg-ar r006-e64dab2d-8fd2-43bd-8390-229ba66e53c4 --account-id efe5afc483594adaa8325e2b4d1290df`

- Revoke access to a Private Path service for an account:
   `ibmcloud is ppsg-ar cli-ppsg --account-id efe5afc483594adaa8325e2b4d1290df`

## Deleting a Private Path service with the API
{: #pps-api-revoking-private-path-service}
{: api}

To revoke access to a Private Path service for an account, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
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

   * Run this command to revoke the account:

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways/{ppsgId}?version=$api_version&generation=2" \
      -d {
        "account": {
          "id": "$accountId"
        }
      }'
      ```
      {: codeblock}

## Revoking an account's access a Private Path service with Terraform
{: #pps-revoke-account-access-terraform}
{: terraform}

The following example revokes an account's access to a Private Path network by using Terraform:

```terraform
resource "ibm_is_private_path_service_gateway_revoke_account" "example" {
  account = "7f75c7b025e54bc5635f754b2f888665"
  private_path_service_gateway = ibm_is_private_path_service_gateway.ppsg.id
}
```
{: codeblock}
