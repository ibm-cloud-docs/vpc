---

copyright:
  years: 2024
lastupdated: "2023-1-16"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Verifying connectivity to a Private Path service
{: #pps-verify}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

After you create a Private Path service, the service's status is `Stable`. At this time, it's a good idea to test the connection to your service by creating a VPE gateway with the cloud resource name (CRN) associated with your Private Path service.
{: shortdesc}

To verify that the Private Path service is fully functional before publishing it for consumer use, you must use the same account to create the VPE gateway as the account used to create the Private Path service. (After you publish your Private Path service, any account can be used to create the VPE gateway.)
{: important}

You can verify connectivity to a Private Path service by SSH into a virtual server instance running in the VPC containing the endpoint gateway. Initiate traffic to the VPE service endpoint or private IP.

## Verifying connectivity to a Private Path service in the UI
{: #pps-ui-verify-private-path-service}
{: ui}

To verify connectivity to a Private Path service from the IBM Cloud console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![Menu icon](../icons/icon_hamburger.svg), then click **VPC Infrastructure**.
1. Click **Private Path services** in the Network section.
1. Locate your new Private Path service in the table and click the name of the service to show its Details page.
1. Copy the CRN to your clipboard.
1. Click the **VPC Infrastructure** breadcrumb at the top of the page, then click **Virtual private endpoint gateways** in the Network section.
1. Create a VPE gateway to connect to your Private Path service using your Private Path CRN. For instructions, see [Creating a VPE gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway){: external}.
1. Navigate back to the Private Path services for VPC list page and click the name of your Private Path service in the table.
1. In the Connections section:

   * If your default policy is set to **Permit all requests**, your request shows in the **Permitted** view.
   * If your default policy is set to **Review all requests**, your request shows in the **Requests to review** view. Permit your connection request.
1. Connect to your service.

## Verifying connectivity to a Private Path service from the CLI
{: #pps-cli-verify-private-path-service}
{: cli}

The following example shows how to use the CLI to verify connectivity to a Private Path service.

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli){: external}.

You must first export the feature flag to use the CLI for Private Path beta release offerings.
{: important}

To export the feature flag, enter the following commands:

```sh
export IBMCLOUD_IS_FEATURE_PRIVATE_PATH_SERVICE_GATEWAY=true
export IBMCLOUD_IS_FEATURE_PP_NLB_SUPPORT=true
```
{: pre}

To verify connectivity to a Private Path service from the CLI, follow these steps:

1. Create a VPE gateway to connect to your Private Path service using your Private Path CRN. For instructions, see [Creating a VPE gateway from the CLI](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=cli){: external}.
1. Connect to your service.

## Verifying connectivity to a Private Path service with the API
{: #pps-api-verify-private-path-service}
{: api}

To verify connectivity to a Private Path service with the API, follow these steps:

1. Follow [this instruction](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=api){: external} to create a VPE with `TargetCrn` specified with your Private Path service (PPSG) CRN.
1. Ensure that at least one of your load balancer's members health is shown as `ok`. (TBD link to how to do this).
1. From a VSI in the same VPE's VPC, initiate a request to the VPE's `private IP` or `service_endpoint` and expect to get a reply. For example, ssh into a VSI in the same VPE's VPC with image `ibm-ubuntu-18-04-6-minimal-s390x-3` and run this command:

```sh
  export ip=<VPE-private-ip>
  export port=<load-balancer-listener-port>
  wget http://$ip:$port
```
{: codeblock}

The following example shows how to use the API to verify connectivity to a Private Path service.

## Next steps
{: #pps-next-step-after-verify}

1. [Publish your Private Path service](/docs/vpc?topic=vpc-pps-activating&interface=ui)
1. [Communicate connection information to consumers](/docs/vpc?topic=vpc-pps-ui-communicate&interface=ui)
1. [Review connection requests](/docs/vpc?topic=vpc-pps-ui-reviewing&interface=ui) and [Create account policies](/docs/vpc?topic=vpc-pps-create-account-policy&interface=ui)
