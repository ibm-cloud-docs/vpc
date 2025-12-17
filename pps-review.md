---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Reviewing connection requests
{: #pps-ui-reviewing}

As a service provider, you are responsible for managing your consumer account IDs. Currently, the tracking or validating of account IDs is not supported. For more information, see [Responsibilities for managing consumer account IDs](/docs/vpc?topic=vpc-pps-consumer-account-id-responsibilities&interface=ui).
{: attention}

If your default policy is set to **Review**, or if you have the account policies set to **Review**, you are notified of incoming connection requests. You can choose to **Permit**, **Deny**, or **Revoke** these requests. On the Private Path service's Details page, you can also view connection requests that were permitted, denied, or expired.
{: shortdesc}

You can choose from three options when you're working with connection requests:

* **Permit** - Permits the account's request to connect to the service. Examine the originating account ID to make sure that the connection request is legitimate. You should accept requests only from known sources. If you do not recognize this request, you can deny it or let it expire automatically.

* **Deny** - Denies the account's request to connect to the service. If you deny a request and later want to permit it, the consumer must create a new connection request by creating a Virtual Private Endpoint (VPE) gateway.

* **Revoke** - Removes all existing connections from this account ID. If an account policy exists for this account ID, its status is updated to **Deny**; if one does not exist, a **Deny** policy is created for this account ID.

If you don't want to review every connection request, you can streamline the process by changing the default policy or creating account-specific policies. For more information, see [About account policies](/docs/vpc?topic=vpc-pps-about-account-policies).
{: fast-path}

As the service provider, you can review and triage (permit, deny, or revoke) connection requests to your service by using the console, CLI, API, or Terraform.

## Reviewing connection requests in the console
{: #pps-ui-review-requests}
{: ui}

To triage incoming connection requests in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Private Path services**.
1. In the Private Path services for VPC table, click the name of a Private Path service to show its details page.
1. Scroll to the **Connections** section to review connection requests. Click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") and select an option to permit, deny, or revoke the connection request. After you select an option, you are prompted to confirm your choice, and optionally create an account policy for the account ID.

The consumer that is associated with the account ID is notified of your action. If permitted, the consumer can now access your service.

Connection requests expire after 30 days.
{: note}

## Reviewing connection requests from the CLI
{: #pps-cli-review-consumer-request}
{: cli}

The following examples show how to use the CLI to permit or deny an account-specific connection request.

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

### Permitting connection requests from the CLI
{: #pps-cli-permit-consumer-request}
{: cli}

To permit connectivity to a Private Path service from the CLI, follow these steps:

1. Enter the following command:

```sh
ibmcloud is private-path-service-gateway-endpoint-gateway-binding-permit PRIVATE_PATH_SERVICE_GATEWAY ENDPOINT_GATEWAY_BINDING
    (--set-account-policy true | false)
    [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`PRIVATE_PATH_SERVICE_GATEWAY`
:   Indicates ID or name of the Private Path service.

`ENDPOINT_GATEWAY_BINDING`
:   Indicates ID of the VPE gateway binding for the Private Path service.

`--set-account-policy`
:   Indicates whether this action becomes the access policy for any pending and future VPE gateway bindings from the same account. One of: `true`, `false`.

`--output`
:   Specifies output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppresses verbose output.


### Denying connection requests from the CLI
{: #pps-cli-deny-consumer-request}
{: cli}

To deny connectivity to a Private Path service from the CLI, follow these steps:

1. Enter the following command:

```sh
ibmcloud is private-path-service-gateway-endpoint-gateway-binding-deny PRIVATE_PATH_SERVICE_GATEWAY ENDPOINT_GATEWAY_BINDING
    (--set-account-policy true | false)
    [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`PRIVATE_PATH_SERVICE_GATEWAY`
:   ID or name of the Private Path service.

`ENDPOINT_GATEWAY_BINDING`
:   ID of the VPE gateway binding for the Private Path service.

`--set-account-policy`
:   Indicates whether this action becomes the access policy for any pending and future VPE gateway bindings from the same account. One of `true`, `false`.

`--output`
:   Specify output format, only `JSON` is supported.

`-q, --quiet`
:   Suppress verbose output.


### Command examples
{: #cli-cmd-examples-consumer-requests-private-path-service}
{: cli}

- Deny a consumer connection request:
   `ibmcloud is private-path-service-gateway-endpoint-gateway-binding-deny r006-2e671f14-19fc-4089-9ad3-973176711259 r006-17635273-0702-4a31-b406-a0014c291fbb --set-account-policy true`
   

- Deny a consumer connection request:
   `ibmcloud is ppsg-egb-deny cli-ppsg r006-f7e2b651-0d02-46e1-8811-16ea2157b547 --set-account-policy true`
   

- Permit a consumer connection request:
   `ibmcloud is ppsg-egb-permit r006-e64dab2d-8fd2-43bd-8390-229ba66e53c4 r006-0dcc2105-14a1-4501-9b43-29632e910e4b --set-account-policy true`
   

- Permit a consumer connection request:
   `ibmcloud is ppsg-egb-permit cli-ppsg r006-3a30c0b3-8c4b-4a64-bb93-3f3e17459363 --set-account-policy true`
   

## Reviewing connection requests with the API
{: #pps-api-review-consumer-request}
{: api}

To triage incoming connection requests with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Store the following values in variables to be used in the API command:

   * `ppsgId` - Get your Private Path service and then populate the variable:

      ```sh
      export ppsgId=<your_ppsg_id>
      ```
      {: pre}

   * `egwBindingId` - Get your VPE gateway binding and then populate the variable:

      ```sh
      export egwbId=<your_endpoint_gateway_binding_id>
      ```
      {: pre}

The following example shows how to use the API to permit, deny, or revoke an account-specific connection request:

1. To permit a connection request:

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways/{ppsgId}/endpoint_gateway_bindings/{egwBindingId}/permit?version=$api_version&generation=2" \
      -d {
        "set_account_policy": true
      }'
      ```
      {: codeblock}

If `set_account_policy` is set to `true`:

   * If the account has an existing access policy, that policy is updated to permit access. Otherwise, a new permit access policy is created for the account.
   * All pending VPE gateway bindings for the account are permitted.

2. To deny a connection request:

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways/{ppsgId}/endpoint_gateway_bindings/{egwBindingId}/deny?version=$api_version&generation=2" \
      -d {
        "set_account_policy": true
      }'
      ```
      {: codeblock}

If `set_account_policy` is set to `true`:

   * If the account has an existing access policy, that policy is updated to deny the request. Otherwise, a new permit access policy is created for the account.
   * All pending VPE gateway bindings for the account are denied.

3. To revoke an account:
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

## Reviewing connection requests with Terraform
{: #review-pps-consumer-request-terraform}
{: terraform}

The following example permits an endpoint gateway binding to a Private Path network by using Terraform:

```terraform
resource "ibm_is_private_path_service_gateway_endpoint_gateway_binding_operations" "policy" {
  access_policy = "permit"
  endpoint_gateway_binding = data.ibm_is_private_path_service_gateway_endpoint_gateway_bindings.ppsgEGWBindings.endpoint_gateway_bindings.0.id
  private_path_service_gateway = ibm_is_private_path_service_gateway.example.id
}
```
{: codeblock}

The following example denies an endpoint gateway binding to a Private Path network by using Terraform:

```terraform
resource "ibm_is_private_path_service_gateway_endpoint_gateway_binding_operations" "policy" {
  count = length(data.ibm_is_private_path_service_gateway_endpoint_gateway_bindings.bindings.endpoint_gateway_bindings)
  access_policy = "deny"
  endpoint_gateway_binding = data.ibm_is_private_path_service_gateway_endpoint_gateway_bindings.bindings.endpoint_gateway_bindings[count.index].id
  private_path_service_gateway = ibm_is_private_path_service_gateway.ppsg.id
}
```
{: codeblock}

For documentation about Terraform resources, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_private_path_service_gateway).{: external}

## Related links
{: #related-links-review-requests}

* [About account policies](/docs/vpc?topic=vpc-pps-about-account-policies)
* [Create an account policy](/docs/vpc?topic=vpc-pps-create-account-policy)
