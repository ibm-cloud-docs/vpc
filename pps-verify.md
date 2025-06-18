---

copyright:
  years: 2024, 2025
lastupdated: "2023-1-16"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Verifying connectivity to a Private Path service
{: #pps-verify}

After you create a Private Path service, the service's status is `Stable`. At this time, it's a good idea to test the connection to your service by creating a VPE gateway with the cloud resource name (CRN) associated with your Private Path service.
{: shortdesc}

To verify that the Private Path service is fully functional before publishing it for consumer use, you must use the same account to create the VPE gateway as the account used to create the Private Path service. After you publish your Private Path service, any account can be used to create the VPE gateway.
{: important}

You can verify connectivity to a Private Path service by using SSH to log into a virtual server instance running in the VPC containing the endpoint gateway. Then, initiate traffic to the VPE service endpoint or private IP.

## Verifying connectivity to a Private Path service in the console
{: #pps-ui-verify-private-path-service}
{: ui}

To verify connectivity to a Private Path service from the IBM Cloud console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Private Path services**.
1. Locate your new Private Path in the table and click the name of the service to show its Details page.
1. Copy the CRN to your clipboard.
1. Click **Infrastructure**, then click **Virtual private endpoint gateways** in the Network section.
1. Create a VPE gateway to connect to your Private Path service using your Private Path CRN. For instructions, see [Creating a VPE gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway).
1. Navigate back to the Private Path services for VPC list page and click the name of your Private Path service in the table.
1. In the Connections section:

   * If your default policy is set to **Permit all requests**, your request shows in the **Permitted** view.
   * If your default policy is set to **Review all requests**, your request shows in the **Requests to review** view. Permit your connection request.

1. Connect to your service.

## Verifying connectivity to a Private Path service from the CLI
{: #pps-cli-verify-private-path-service}
{: cli}

The following example shows how to use the CLI to verify connectivity to a Private Path service.

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To verify connectivity to a Private Path service from the CLI, follow these steps:

1. Create a VPE gateway to connect to your Private Path service using your Private Path CRN. For instructions, see [Creating a VPE gateway from the CLI](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=cli).
1. Connect to your service.

## Verifying connectivity to a Private Path service with the API
{: #pps-api-verify-private-path-service}
{: api}

To verify connectivity to a Private Path service with the API, follow these steps:

1. Follow [these instructions](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=api)to create a VPE with `TargetCrn` specified with your Private Path service CRN.
1. Ensure that at least one of your load balancer's members health is shown as `ok`.
1. From a VSI in the same VPE's VPC, initiate a request to the VPE's `private IP` or `service_endpoint` and expect to get a reply. For example, SSH into a VSI in the same VPE's VPC with image `ibm-ubuntu-18-04-6-minimal-s390x-3`. Then run this command:

```sh
  export ip=<VPE-private-ip>
  export port=<load-balancer-listener-port>
  wget http://$ip:$port
```
{: codeblock}

## Verifying connectivity to a Private Path service with Terraform
{: #verifying-private-path-service-terraform}
{: terraform}

The following example verifies connectivity to a Private Path network by using Terraform:

```terraform
resource "ibm_is_virtual_endpoint_gateway" "endpoint_gateway" {
    name = "my-example-egw"
    target {
        crn = ibm_is_private_path_service_gateway.ppsg.crn
        resource_type = "private_path_service_gateway"
    }
    vpc = ibm_is_vpc.vpc.id
}
```
{: codeblock}

For documentation about Terraform resources, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_private_path_service_gateway).{: external}

## Next steps
{: #pps-next-step-after-verify}

1. [Publish your Private Path service](/docs/vpc?topic=vpc-pps-activating&interface=ui)
1. [Communicate connection information to consumers](/docs/vpc?topic=vpc-pps-ui-communicate&interface=ui)
1. [Review connection requests](/docs/vpc?topic=vpc-pps-ui-reviewing&interface=ui) and [Create account policies](/docs/vpc?topic=vpc-pps-create-account-policy&interface=ui)
