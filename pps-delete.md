---

copyright:
  years: 2024
lastupdated: "2024-1-16"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating and deleting a Private Path service
{: #pps-ui-updating-deleting}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

You can migrate to a newer version of Private Path service without deleting or disrupting the service you provide to your current customers. You can delete a Private Path service using the UI, CLI, or API.
{: shortdesc}

## Update a Private Path service in the UI
{: #pps-ui-update-private-path-service}
{: ui}

To update a Private Path provider service the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![Menu icon](images/menu_icon.png), then click **Infrastructure**.
1. Click **Private Path services** in the Network section.
1. In the Private Path services for VPC table, locate and click the name of the Private Path service that you want to update.
1. On the Private Path details page, click the Edit icon ![Edit icon](images/edit.png) beside the details that you want to update.

### Update the target service of a Private Path service in the UI
{: #pps-ui-update-target-private-path-service}
{: ui}

If you’re updating the actual target service without changing the load balancer, you don’t need to take any actions in Private Path service. Instead, you need to update the Private Path network load balancer.

To update the target service of a Private Path provider service the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![Menu icon](images/menu_icon.png), then click **VPC Infrastructure > Load balancers**.
1. Select the region of your load balancer.
1. Select the load balancer that you want to update.
1. Select **Back-end pools** > **Create pool +** to create a new pool of servers for your updated service.
1. Select the new options for your pool. You have the following options:

   * **Pool Name**: The name for your pool. Ideally, a name that describes the function that is performed by this pool.
   * **Protocol**: The network traffic protocol for your traffic.
   * **Method**: The load-balancing algorithm for the pool.
   * **Session stickiness**: Whether all requests during a user's session are sent to the same instance.
   * **Health check**: For more information about configuring health checks, see [Working with health checks](/docs/vpc?topic=vpc-nlb-health-checks#nlb-health-checks){: external}.
1. Make sure to configure these servers for your new provider service by setting the **Method** to Weighted round robin for your old pool and your new pool. Set the **Weight** of the new pool to a non-zero value, and the **Weight** of the old pool to a zero value. This will redirect traffic from your old server pool to your new server pool.
1. Update your existing front-end listener to finish attaching your load balancer to this new pool. On your load balancer details page, Click the **Front-end listeners** tab. In the table, select the Menu icon ![navigation menu](../icons/icon_hamburger.svg) at the end of the row of your existing listener, then select **Edit**.
1. In the menu that appears, select **Edit**. Under Default Back-end pool, type in the ID of your new pool. Select **Save**.

### Deleting a Private Path service in the UI
{: #pps-ui-deleing-private-path-service}
{: ui}

To delete a Private Path service in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. Revoke accounts associated with any active VPE gateways. To learn more, see [Updating and deleting an account policy](/docs/vpc?topic=vpc-pps-update-account&interface=ui){: external}.
1. Customer gets notified that their account is denied.
1. VPE gateway goes to `failed` state.
1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![navigation menu](../icons/icon_hamburger.svg), then click **Infrastructure**.
1. Click **Private Path services** in the Network section.
1. In the Private Path services for VPC table, locate the Private Path service that you want to delete, then click **Delete** in the Actions menu ![Actions menu](images/overflow.png).

## Updating a provider service from the CLI
{: #pps-cli-update-provider-service}
{: cli}

The following example shows how to update a Private Path provider service from the CLI.

You must first export the feature flag to use the CLI for Private Path beta release offerings.
{: important}

To export the feature flag, enter the following commands:

```sh
export IBMCLOUD_IS_FEATURE_PRIVATE_PATH_SERVICE_GATEWAY=true
export IBMCLOUD_IS_FEATURE_PP_NLB_SUPPORT=true
```
{: pre}

To update a Private Path service from the CLI, follow these steps:

1. Enter the following command:

```sh
ibmcloud is private-path-service-gateway-update PRIVATE_PATH_SERVICE_GATEWAY --name NEW_NAME [--default-access-policy deny | permit | review] [--load-balancer LOAD_BALANCER] [--published] [--zonal-affinity false | true] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`PRIVATE_PATH_SERVICE_GATEWAY`
:   Indicates ID or name of the Private Path service.

`--default-access-policy`
:   Indicates the policy to use for bindings from accounts without an explicit account policy. One of: `deny`, `permit`, `review`. Default: `deny`.

`--load-balancer`
:   Indicates ID or name of load balancer for this Private Path service.

`--published`
:   Indicates the availability of this Private Path service. If passed, value is set to `true`. A `true` value means account can request access to this Private Path service.

`--zonal-affinity`
:   Indicates whether this Private Path service has zonal affinity. One of: `false`, `true`.

`--name`
:   Indicates the new name of the Private Path service.

`--output`
:   Specifies output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   Suppresses verbose output.

### Command examples for updating a Private Path service
{: #cli-cmd-examples-updating-private-path-service}
{: cli}

- Update a Private Path service:
   `ibmcloud is private-path-service-gateway-update r006-2e671f14-19fc-4089-9ad3-973176711259 --name cli-ppsg-2 --default-access-policy deny --load-balancer r006-d-740be75d-be41-48bd-b6e4-89946ddcac4a --zonal-affinity false --published`

- Update a Private Path service:
   `ibmcloud is private-path-service-gateway-update cli-ppsg-2 --name cli-ppsg-0 --default-access-policy review --load-balancer my-cli-nlb-1 --zonal-affinity false --published`

### Deleting a Private Path service from the CLI
{: #pps-cli-deleting-private-path-service}
{: cli}

The following example shows how to use the CLI to delete a Private Path service.

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli){: external}.

You must first export the feature flag to use the CLI for Private Path beta release offerings.
{: important}

To export the feature flag, enter the following commands:

```sh
export IBMCLOUD_IS_FEATURE_PRIVATE_PATH_SERVICE_GATEWAY=true
export IBMCLOUD_IS_FEATURE_PP_NLB_SUPPORT=true
```
{: pre}

To delete a Private Path service from the CLI, follow these steps:

1. Enter the following command:

```sh
ibmcloud is private-path-service-gateway-delete (PRIVATE_PATH_SERVICE_GATEWAY1 PRIVATE_PATH_SERVICE_GATEWAY2 ...)
    [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

Where:

`PRIVATE_PATH_SERVICE_GATEWAY1`
:   Indicates ID or name of the Private Path service.

`PRIVATE_PATH_SERVICE_GATEWAY2`
:   Indicates ID or name of the Private Path service.

`--output`
:   Specifies output format, only JSON is supported. One of: `JSON`.

`force, -f`
:   Forces the operation without confirmation.

`-q, --quiet`
:   Suppresses verbose output.

### Command examples for deleting Private Path gateways
{: #cli-cmd-examples-deleting-private-path-service}
{: cli}

- Delete multiple Private Path services:
   `ibmcloud is ppsgd r006-01cd30f7-e6f2-432f-9520-76247b1fbbe1 r006-72940467-a4db-487f-b57e-b7141ac40e32`

- Delete multiple Private Path services:
   `ibmcloud is private-path-service-gateway-delete cli-ppsg-0 cli-ppsg-1`

## Updating a Private Path service with the API
{: #pps-api-update-provider-service}
{: api}

To update a Private Path provider service  with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Store the following values in variables to be used in the API command:

   * `ppsgId` - Get your Private Path service and then populate the variable:

      ```sh
      export ppsgId=<your_ppsg_id>
      ```
      {: pre}


1. When all variables are initiated, do one of the following:

   * To update a Private Path service:

      ```sh
      curl -X PATCH -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways/$ppsgId?version=$api_version&generation=2" \
      -d {
         "zonal_affinity": true
       }'
      ```
      {: codeblock}

      The above example updates the Private Path service zonal affinity to `true`.


### Deleting a Private Path service with the API
{: #pps-api-deleting-private-path-service}
{: api}

To delete a Private Path service with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Store the following values in variables to be used in the API command:

   * `ppsgId` - Get your Private Path service and then populate the variable:

      ```sh
      export ppsgId=<your_ppsg_id>
      ```
      {: pre}


1. When all variables are initiated, do one of the following:

   * To delete a Private Path service:

      ```sh
      curl -X DELETE -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways/$ppsgId?version=$api_version&generation=2"
      ```
      {: codeblock}

## Updating and deleting a Private Path service with Terraform
{: #update-delete-pps-terraform}
{: terraform}

Terraform will support this feature after it reaches General Availability (GA) and is officially released.
{: note}

Updating and Deleting a Private Path Service by using Terraform is done with the same resource.

The following example updates and delete an endpoint gateway binding to a Private Path network by using Terraform:

```terraform
resource "ibm_is_private_path_service_gateway" "ppsg" {
    default_access_policy = "deny"
    load_balancer = ibm_is_lb.ppnlb.id
    service_endpoints = ["my-service.example.com"]
    zonal_affinity = false
    name = "my-example-ppsg"
}
```
{: codeblock}
