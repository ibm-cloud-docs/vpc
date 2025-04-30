---

copyright:
  years: 2023, 2025
lastupdated: "2025-04-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Private Path service
{: #private-path-service-about}

As a service provider, you are responsible for managing your consumer account IDs. Currently, tracking or validating account IDs is not supported. For more information, see [Responsibilities for managing consumer account IDs](/docs/vpc?topic=vpc-pps-consumer-account-id-responsibilities&interface=ui).
{: attention}

Private Path services for VPC enable service providers to create and manage private connectivity for hosted IBM Cloud and third-party services and applications. You can create a Private Path service by using the console, CLI, API, or Terraform.
{: shortdesc}

## Before you begin
{: #pps-prerequisites-byb-vpc}

Before you create a Private Path service, review the following prerequisites:

* Review [Private Path service limitations](/docs/vpc?topic=vpc-ppsg-limitations&interface=ui) for known limitations.
* Make sure that you have a VPC and at least one subnet in the selected VPC. [Learn more](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console)
* Create a Private Path network load balancer. You can create your load balancer while provisioning your Private Path service, or you can use the [Load balancer for VPC](/infrastructure/provision/loadBalancer) console.

   You must use the same VPC region for both your load balancer and Private Path service.
   {: important}

* You must choose a DNS FQDN for your service for consumer use. This domain is configured in a consumer's private DNS, but you are required to prove ownership of the FQDN in public DNS. This requires you to take steps with your DNS provider. For more information, see [Registering and verifying ownership of service endpoints (FQDNs)](/docs/vpc?topic=vpc-private-path-service-about&interface=ui#pps-domain-register-verify). 

   You can opt out of this requirement if you are willing to use one of limited predefined set of domains listed in [Registering and verifying ownership of service endpoints (FQDNs)](/docs/private-path?topic=private-path-private-path-service-about&interface=ui#pps-domain-register-verify).
   {: note}

You can create an {{site.data.keyword.cloud}} Private Path service using the console, CLI, API, or Terraform.

## Creating a Private Path service in the console
{: #pps-ui-creating-private-path-service}
{: ui}

To create a Private Path service with the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation Menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure > Network > Private Path services**.
1. Click **Create**.
1. Review the checklist for important information.
1. In the Location section, ensure that the following fields are correct. If not, click the Edit icon ![Edit icon](images/edit.png) to update.
    * **Geography**: The general area where you want to create the Private Path service.
    * **Region**: The region where you want to create the Private Path service.
1. In the Details section, provide the following information:
    * **Name**: Enter a unique identifier for the Private Path service, such as `my-privatepath-service`.
    * **Resource group**: Select a resource group, if necessary.
    * **Tags**: Optionally, add any relevant tags to help group your Private Path services.
    * **Access management tags**: Optionally, add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
    * **Virtual private cloud**: Select the VPC where you want the Private Path service created.
1. In the Private Path network load balancer section, select a Private Path NLB for the Private Path service, or click **Create** to create one. To create a Private Path NLB, follow these steps:

   Click **Next** to go to the next step, or use the navigation menu on the left to go back to a specific section.
   {: note}

   1. In the Define details section, provide the following information:
       * **Name**: Enter a unique identifier for the Private Path NLB, such as `my-privatepath-service`.
       * **Resource group**: Select a resource group for the Private Path NLB.
       * **Tags**: Optionally, add any relevant tags to help group your Private Path NLBs.
       * **Access management tags**: Optionally, add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial). {: #private-path-nlb-steps}
       * **Subnet**: Select the subnet where you want the Private Path NLB created.
   1. Optionally, in the Create back-end pool section:
       * **Name**: Enter a unique identifier for the Private Path NLB, such as `my-ppnlb`.
       * Select the method, which is the load-balancing algorithm. The follow options are shown.
         * **Round robin** - Forwards requests to each instance in turn. All instances receive approximately an equal number of client connections.
         * **Weighted round robin** - Forwards requests to each instance in proportion to its assigned weight. For example, if you have instances A, B, and C, and their weights are set to 60, 60 and 30, then instances A and B receive an equal number of connections, and instance C receives half as many connections.

         In the Health check section:
         * **Health check path** - The health check path is applicable only if you select HTTP as the health check protocol. The health check path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (/).
         * **Health protocol** - The protocol used by the load balancer to send health check messages to the instances in the pool.
         * **Health port** - The port on which to the load balancer sends health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
         * **Interval** - The interval in seconds between two consecutive health check attempts. By default, health checks are sent every 5 seconds.
         * **Timeout** - The maximum amount of time the system waits for a response from a health check request. By default, the load balancer waits 2 seconds for a response.
         * **Max retries** - The maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

         Although the load balancer stops sending connections to unhealthy instances, it continues monitoring the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

        If instances in the pool are unhealthy and you believe that your application is working correctly, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
     {: tip}

       * Click **Save**. Repeat this step if you want to create another back-end pool.
   1. Optionally, in the Attach members section, specify the following information, then click **Attach**:
       * **Back-end pool**: Select the back-end pool where you want to attach servers.
       * **Subnet**: Search for specific subnets in the table, and check the box beside the subnets that you want to attach. In the Port column, enter a port number for each subnet you select.
       * **Member type**: Add virtual server instances, or an application load balancer as a member. For virtual server instances, attach each type individually.

          If you attach an ALB as a member target to a Private Path NLB pool, no other members can be added to that pool.
          {: note}

   1. Optionally, in the Add front-end listeners section, select the back-end pool where you want to attach your front-end listener. Then, select the listener port value and click **Save**. Repeat this step if you want to create another front-end listener.
   1. In the Review section, confirm that the information you submitted is correct. Review the order summary, then click **Create**.

      It takes a few minutes for your Private Path NLB to be created. When the load balancer is created, its status changes from **Creating** to **Active** in the table.
      {: note}

1. In the Service endpoint section, click **Create**. Provide a name for the service endpoint where you want to connect your Private Path service. Then, validate ownership of the FQDN domain name and click **Add**. For more information, see [Register and verify ownership of service endpoints (FQDNs)](/docs/vpc?topic=vpc-private-path-service-about&interface=ui#pps-domain-register-verify).
1. Select to enable or disable zonal affinity for the service endpoints. When zonal affinity is enabled, the endpoint maintains persistence to the zone after the connection is created.
1. In the Account policies section:
   * The default policy is set to review and triage each incoming connection request. You can change the default policy to permit or deny all requests without review.
   * To establish account policies that are different than the default policy, click **Create**. Provide the account ID of the account for which you want to set up a policy. For the Account policy option, select Review, Permit, or Deny.

   Individual account policies take precedence over the default policy.
   {: tip}

1. Review the summary page, then click **Create** to order your Private Path service.

   When provisioning completes, the Private Path service status indicates `Stable` in the Private Path services for VPC table.

## Creating a Private Path service from the CLI
{: #pps-cli-creating-private-path-service}
{: cli}

The following example shows how to use the CLI to create a Private Path service.

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a Private Path service from the CLI, follow these steps:

1. Enter the following command:

```sh
ibmcloud is private-path-service-gateway-create
    [--load balancer LOAD_BALANCER]
    [--service-endpoints SERVICE_ENDPOINTS]
    [--default-access-policy | deny | permit | review]
    [--name NAME]
    [--zonal-affinity | true | false]
    [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`--load-balancer`
:   Indicates ID or name of load balancer for this Private Path service.

`--service-endpoints`
:   Indicates the fully qualified domain names for this Private Path service. Any uppercase letters will be converted to lowercase.

`--default-access-policy`
:   Indicates the policy to use for bindings from accounts without an explicit account policy. One of: `deny`, `permit`, `review`. (default: `deny`).

`--name`
:   Indicates the name for this Private Path service.

`--zonal-affinity`
:   indicates whether this Private Path service has zonal affinity. One of: `false`, `true`.

`--output`
:   specify output format, only JSON is supported. One of: `JSON`.

`-q, --quiet`
:   suppress verbose output.

### Command examples
{: #cli-cmd-examples-creating-private-path-service}
{: cli}

- Create a policy-based Private Path service with a permit policy and zonal affinity:
   `ibmcloud is private-path-service-gateway-create --load-balancer my-cli-nlb --service-endpoints cli.domain.com --default-access-policy permit --name cli-ppsg-1 --zonal-affinity true`

- Create a policy-based Private Path service with a deny policy and zonal affinity:
   `ibmcloud is private-path-service-gateway-create --load-balancer r006-d-439744e1-81d7-43fb-95d5-2356774240bb --service-endpoints clidemo.domain.com --default-access-policy deny --name cli-ppsg-2 --zonal-affinity true`

## Creating a Private Path service with the API
{: #pps-api-creating-private-path-service}
{: api}

To create a Private Path service with the API, follow these steps:


1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Store the following values in variables to be used in the API command:

   * `loadBalancerId` - First, get your load balancer and then populate the variable:

      ```sh
      export loadBalancerId=<your_loadbalancer_id>
      ```
      {: pre}


1. When all variables are initiated, to create a private path service:

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/private_path_service_gateways?version=$api_version&generation=2" \
      -d {
        "default_access_policy": "review",
        "load_balancer": {
          "id": "$loadBalancerId"
        },
        "name": "my-ppsg",
        "service_endpoints": ["example.com"],
        "zonal_affinity": false
      }'
      ```
      {: codeblock}

## Creating a Private Path service with Terraform
{: #creating-private-path-service-terraform}
{: terraform}

The following example provisions a Private Path network with Terraform:

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

## Registering and verifying ownership of service endpoints (FQDNs)
{: #pps-domain-register-verify}

When creating a Private Path service, you are required to prove that you own the _service-endpoints_ (DNS FQDNs) you provide. This is done to prevent DNS hijacking and FQDN conflicts. Ownership is verified by creating a TXT record for each FQDN (service endpoint) with specific contents. Create the TXT records in a public DNS. The public DNS is only consulted when the Private Path service is created. After a Private Path service is created, only a private DNS is used in the data path  (never a public DNS).

The required `TXT` record must start with a `ibm-domain-verification=` prefix. Validation is successful if the prefix is followed by a value that matches the `SHA-256` hash of the account ID associated with the user creating the Private Path service. An example TXT record to add:
`ibm-domain-verification=252cfc164d3600a79007f25312a6a924288cfc6dbcaeec838ca9048cde664acb`

The details of how to go about adding a TXT record to your FQDN differs depending on the public DNS service you use. It is recommended that you consult your DNS service provider for details.

If multiple service endpoints are specified for a Private Path service, the ownership validation must be successful for all of them.

The following is a list of top-level domains that you can use to bypass domain name ownership validation:
- `.intranet`
- `.internal`
- `.private`
- `.corp`
- `.home`
- `.lan`

Wildcard (&#42;) domains are supported. For example, a Private Path service with `"service_endpoints": ["*.service.com"]` includes all of its subdomains, such as `api1.service.com` and `api2.service.com`.

The DNS ownership validation is successful when the wildcard domain contains the valid `TXT` record. In this example, to pass the validation, you can add the valid `TXT` record to `service.com`.

## Next steps
{: #pps-next-steps}

1. [Verify connectivity to your Private Path service](/docs/vpc?topic=vpc-pps-verify&interface=ui)
1. [Publish your Private Path service](/docs/vpc?topic=vpc-pps-activating&interface=ui)
1. [Communicate connection information to consumers](/docs/vpc?topic=vpc-pps-ui-communicate&interface=ui)
1. [Review connection requests](/docs/vpc?topic=vpc-pps-ui-reviewing&interface=ui) and [Create account policies](/docs/vpc?topic=vpc-pps-create-account-policy&interface=ui)
