---

copyright:
  years: 2022, 2024
lastupdated: "2024-05-09"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Private Path network load balancer
{: #ppnlb-ui-creating-private-path-network-load-balancer}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

You can use a Private Path network load balancer (NLB) only with a Private Path service. Create the Private Path NLB from the [Load Balancers for VPC page](/vpc-ext/provision/loadBalancer){: external} or as a part of the Private Path service provisioning process.

A Private Path service allows service providers to enable and manage private connectivity for consumers of their hosted service. This Private Path service requires a Private Path NLB to establish a secure connection with each consumer's Virtual Private Endpoint (VPE) gateway. For more information, see [About Private Path services](/docs/vpc?topic=vpc-private-path-service-intro){: external}.
{: important}

Private Path network load balancers support the port range feature. Currently, all VPEs connected to a Private Path network load balancer can connect to any port in the range.
{: note}

To learn more, see [Setting public network load balancer port ranges](/docs/vpc?topic=vpc-nlb-port-ranges&locale=en&interface=ui).

## Before you begin
{: #ppnlb-prerequisites}

Review the following list to ensure that your Private Path network load balancer (NLB) is properly configured:

- If you don't have a VPC, create a VPC in the region that you want to create your Private Path NLB. Use the same VPC region for the Private Path NLB and Private Path service.
- Recommendation: Create your virtual server instances prior to creating a Private Path NLB to ensure it is fully operational.
- Ensure there is at least one subnet in your selected VPC.

You can create a Private Path NLB while provisioning a Private Path service.
{: note}

## Creating a Private Path network load balancer in the UI
{: #ppnlb-ui}
{: ui}

To create and configure {{site.data.keyword.nlb_full}} in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** icon ![Menu icon](../icons/icon_hamburger.svg), then click **VPC Infrastructure > Load balancers**.
1. Click **Create +** in the upper right of the page.
1. For Load balancer type, select the **Network Load Balancer (NLB)** tile.
1. In the Location section, edit the following fields, if necessary.
   * **Geography**: The geography where you want to create the load balancer.
   * **Region**: The region where you want to create the load balancer.
1. In the Details section, complete the following information:
   * **Name**: Enter a name for the load balancer, such as `private-path-load-balancer`.
   * **Resource group**: Select a resource group for the load balancer.
   * **Tags**: (Optional) Add tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Access management tags**: (Optional) Add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Virtual private cloud**: Select the VPC where you want to deploy the load balancer.
   * **Subnet**: Select a subnet.
   * **Type**: Select **Private path**.
1. In the Back-end pools section, click **Create pool +**, specify the following information, then click **Create**. You can create one or more pools.
   * Type a unique name for the pool, such as `private-path-pool`.
   * Select a protocol for your instances in this pool. The protocol of the pool must match the protocol of its associated listener. For example, if the listener is TCP, the protocol of the pool must be TCP.
   * Select the method, which is the load-balancing algorithm, from the following options.
      * **Round robin** - Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
      * **Weighted round robin** - Forward requests to each instance in proportion to its assigned weight. For example, if you have instances A, B, and C, and their weights are set to 60, 60 and 30, then instances A and B receive an equal number of connections, and instance C receives half as many connections.
   * Choose session stickiness and select **None**.
   * Enter the following health check options:
     * **Health check path** - Health check path is applicable only if you select HTTP as the health check protocol. The health check path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (`/`).
     * **Health protocol** - The protocol used by the load balancer to send health check messages to the instances in the pool.
     * **Health port (optional)** - The port on which to send health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
     * **Interval (sec)** - The interval in seconds between two consecutive health check attempts.
     * **Timeout (sec)** - The maximum amount of time the system waits for a response from a health check request.
     * **Maximum retries** - The maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

       Although the load balancer stops sending connections to unhealthy instances, the load balancer continues monitoring the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

     If instances in the pool are unhealthy and you believe that your application is working correctly, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
     {: tip}

1. You can attach virtual server instances to your back-end pool now, or after you create your Private Path NLB. Click **Attach server +** on the table row of your back-end pool, specify the following information, then click **Attach**.

   * **Subnet**: Choose a subnet.
   * From the list of servers, select the servers that you want to attach to the back-end pool. Ensure that you specify valid values for each server port.

      You can attach up to 50 virtual server instances to a back-end pool.
      {: note}

1. In the Front-end listeners section, click **Create listener +**, specify the following information, then click **Create**. You can create one or more listeners.
   * **Default back-end pool** - The default back-end pool to which this listener forwards traffic.
   * **Protocol** - The protocol to use for receiving incoming requests (**TCP**).
   * **Listener port** - The listening port on which requests are received.
   * **Back-end pool** - The default back-end pool to which this listener forwards traffic.

1. Review the order summary, then click **Create** to complete your order.

## Creating a Private Path network load balancer from the CLI
{: #ppnlb-cli-creating-network-load-balancer}
{: cli}

Beta participants must export a feature flag to use the CLI. Contact your IBM Support representative to obtain.
{: attention}

The following example illustrates creating a Private Path NLB with the CLI. In this example, the Private Path NLB is in front of one VPC virtual server instance (ID `0716_6acdd058-4607-4463-af08-d4999d983945`) running a TCP server that listens on port `9090`. The load balancer has a front-end listener, which allows secure access to the TCP server.

You must export the feature flag for Private Path service and Private Path network load balancer related commands to run successfully.
{: important}

To export the feature flag, enter the following commands:

```sh
export IBMCLOUD_IS_FEATURE_PRIVATE_PATH_SERVICE_GATEWAY=true
export IBMCLOUD_IS_FEATURE_PP_NLB_SUPPORT=true
```
{: pre}

To create a Private Path NLB from the CLI, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli){: external}.

1. Log in to your account with the CLI. After you enter the password, the system prompts which account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Create a Private Path NLB:

   ```sh
   ibmcloud is load-balancer-create ppnlb-test private-path --subnet 0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c --family network
   ```
   {: pre}

   Sample output:

   ```sh
   Creating load balancer ppnlb-test in resource group under account IBM Cloud Network Services as user test@ibm.com...

    ID                 r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
    Name               ppnlb-test
    CRN                crn:v1:public:is:us-south-1:a/6266f0fbb7df487d8438b9b31d24cd96::load-balancer:r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
    Family             Network
    Host name          99b5ab45-us-south.lb.test.appdomain.cloud
    Subnets            ID                                          Name
                       0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c   ppnlb

    Public IPs
    Private IPs
    Provision status   create_pending
    Operating status   offline
    Is public          false
    Listeners
    Pools              ID   Name

    Resource group     ID                                 Name
                       3021f90279574ce287dd5fba82c08899   Default

    Created            2020-08-27T14:34:34.732-05:00
   ```
   {: screen}


1. Create a pool:

   ```sh
   ibmcloud is load-balancer-pool-create nlb-pool r006-99b5ab45-6357-42db-8b32-5d2c8aa62776  weighted_round_robin tcp 10
   ```
   {: pre}

   Sample output:

   ```sh
   Creating pool nlb-pool of load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776  under account IBM Cloud Network Services as user test@ibm.com...

   ID                         r006-3b66d605-6aa5-4166-9f66-b16054da3cb0
   Name                       nlb-pool
   Protocol                   tcp
   Algorithm                  weighted_round_robin
   Instance group             ID   Name
                              -    -

   Health monitor             Type   Port   Health monitor URL   Delay   Retries   Timeout
                              http   8080   /                    10      2         5

   Session persistence type   source_ip
   Members
   Provision status           active
   Created                    2020-08-27T14:45:42.038-05:00
   ```
   {: screen}

1. Create a member:

   ```sh
   ibmcloud is load-balancer-pool-member-create r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 9090 0716_6acdd058-4607-4463-af08-d4999d983945 --weight 70
   ```
   {: pre}

   Sample output:

   ```sh
   Creating member of pool r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 under account IBM Cloud Network Services as user test@ibm.com...

   ID                 r006-61f8b000-a90d-4abe-909e-c507dffec565
   Port               9090
   Target             0716_6acdd058-4607-4463-af08-d4999d983945
   Weight             70
   Health             unknown
   Created            2020-08-27T14:59:55.446-05:00
   Provision status   create_pending
   ```
   {: screen}

1. Create a listener:

   ```sh
   ibmcloud is load-balancer-listener-create r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 7070 tcp --default-pool r006-3b66d605-6aa5-4166-9f66-b16054da3cb0
   ```
   {: pre}

   Sample output:

   ```sh
   Creating listener of load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...

   ID                     r006-2847a948-f9b6-4fc1-91c6-f1c49dac3eba
   Certificate instance   -
   Connection limit       -
   Port                   7070
   Protocol               tcp
   Default pool           r006-3b66d605-6aa5-4166-9f66-b16054da3cb0
   Provision status       create_pending
   Created                2020-08-27T15:16:08.643-05:00
   ```
   {: screen}

1. Get details about your load balancer:

   ```sh
   ibmcloud is load-balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
   ```
   {: pre}

   Sample output:

   ```sh
   Getting load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...

   ID                 r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
   Name               nlb-test
   CRN                crn:v1:public:is:us-south-1:a/6266f0fbb7df487d8438b9b31d24cd96::load-balancer:r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
   Family             Network
   Host name          99b5ab45-us-south.lb.test.appdomain.cloud
   Subnets            ID                                          Name
                      0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c   nlb

   Public IPs         150.238.50.78, 150.238.54.95
   Private IPs        10.240.0.58, 10.240.0.59
   Provision status   active
   Operating status   online
   Is public          false
   Listeners          r006-2847a948-f9b6-4fc1-91c6-f1c49dac3eba
   Pools              ID                                          Name
                      r006-3b66d605-6aa5-4166-9f66-b16054da3cb0   ppnlb-pool

   Resource group     ID                                 Name
                      3021f90279574ce287dd5fba82c08899   Default

   Created            2020-08-27T14:34:34.732-05:00
   ```
   {: screen}

## Creating a network load balancer with the API
{: #ppnlb-api-creating-network-load-balancer}
{: api}

The following example illustrates creating a Private Path network load balancer with the API. The NLB detailed here is in front of two VPC virtual server instances (`192.168.100.5` and `192.168.100.6`) running a web application that listens on port `80`. It has a front-end listener, which allows secure access to the web application by using HTTPS.

This example skips the [prerequisite steps](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli){: external} for using the API to provision a VPC, subnets, and instances.
{: note}

To create a Private Path network load balancer with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli){: external}.

1. Store the following values in variables to be used in the API command:

   * `subnetId` - First, get your subnet and then populate the variable:

    ```bash
    export subnetId=<your_subnet_id>
    ```
    {: pre}

   * `vsiId` - Second, get your VSI that is in the same VPC as subnet's

    ```bash
    export vsiId=<your_VSI_id>
    ```
    {: pre}

1. Create a Private Path network load balancer with a listener, pool, and attached server instances (pool members):

   ```bash
   curl -H "Authorization: $iam_token" -X POST
   "$vpc_api_endpoint/v1/load_balancers?version=$api_version&generation=2" \
       -d '{
        "is_public": false,
        "is_private_path": true,
        "name": "my-load-balancer",
        "listeners": [
            {
                "default_pool": {
                    "name": "my-pool"
                },
                "port_min": 80,
                "port_max": 80,
                "protocol": "tcp"
            }
        ],
        "subnets": [
            {
                "id" : "$subnetId"
            }
        ],
        "pools": [
            {
                "algorithm": "round_robin",
                "health_monitor": {
                    "delay": 2,
                    "max_retries": 1,
                    "timeout": 1,
                    "port": 80,
                    "type": "tcp"
                },
                "name": my-pool",
                "protocol": "tcp",
                "members" : [
                    {
                        "port" : 80,
                        "target" : {"id" : "$vsiId"}
                    }
                ]
            }
        ],
        "profile": {
            "name": "network-private-path"
        }
    }'
   ```
   {: codeblock}

   Sample output:

   ```sh
        {
            "availability": "region",
            "created_at": "2023-09-22T21:49:26.000Z",
            "crn": "crn:v1:bluemix:public:is:au-syd:a/b21af5a9874242b7851e780943d595a9::load-balancer:r026-d-86b51a9e-a058-4317-beed-8ee581a1f413",
            "hostname": ".invalid",
            "href": "https://au-syd.iaas.cloud.ibm.com/v1/load_balancers/r026-d-86b51a9e-a058-4317-beed-8ee581a1f413",
            "id": "r026-d-86b51a9e-a058-4317-beed-8ee581a1f413",
            "instance_groups_supported": false,
            "is_private_path": true,
            "is_public": false,
            "listeners": [
                {
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/load_balancers/r026-d-86b51a9e-a058-4317-beed-8ee581a1f413/listeners/r026-00b18a00-d860-4f4d-bb84-e29ffc2d8e47",
                    "id": "r026-00b18a00-d860-4f4d-bb84-e29ffc2d8e47"
                }
            ],
            "logging": {
                "datapath": {
                    "active": false
                }
            },
            "name": "my-load-balancer",
            "operating_status": "offline",
            "pools": [
                {
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/load_balancers/r026-d-86b51a9e-a058-4317-beed-8ee581a1f413/pools/r026-f25e3f26-3aa6-4aef-9108-1587d3ac6679",
                    "id": "r026-f25e3f26-3aa6-4aef-9108-1587d3ac6679",
                    "name": "my-pool"
                }
            ],
            "private_ips": [
                {
                    "address": "10.245.0.4",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02/reserved_ips/02h7-e21d477e-3e71-47fc-aec4-9703766cf652",
                    "id": "02h7-e21d477e-3e71-47fc-aec4-9703766cf652",
                    "name": "nugget-ounce-stability-vocation",
                    "resource_type": "subnet_reserved_ip"
                },
                {
                    "address": "10.245.0.5",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02/reserved_ips/02h7-1910c529-dadd-4960-aba5-3c4604f99f56",
                    "id": "02h7-1910c529-dadd-4960-aba5-3c4604f99f56",
                    "name": "impostors-synopses-uncivic-livable",
                    "resource_type": "subnet_reserved_ip"
                },
                {
                    "address": "10.245.0.6",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02/reserved_ips/02h7-a7e55150-5b02-426f-b1dc-aa136effc6ce",
                    "id": "02h7-a7e55150-5b02-426f-b1dc-aa136effc6ce",
                    "name": "unmade-garnish-preview-defame",
                    "resource_type": "subnet_reserved_ip"
                },
                {
                    "address": "10.245.0.7",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02/reserved_ips/02h7-ae009701-8301-420c-862e-a9ec1f79214c",
                    "id": "02h7-ae009701-8301-420c-862e-a9ec1f79214c",
                    "name": "slacking-scuttle-stonework-reptile",
                    "resource_type": "subnet_reserved_ip"
                }
            ],
            "profile": {
                "family": "network",
                "href": "https://au-syd.iaas.cloud.ibm.com/v1/load_balancer/profiles/network-private-path",
                "name": "network-private-path"
            },
            "provisioning_status": "create_pending",
            "public_ips": [],
            "resource_group": {
                "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/297468d87ca047c5be28368e52b055ce",
                "id": "297468d87ca047c5be28368e52b055ce",
                "name": "Default"
            },
            "resource_type": "load_balancer",
            "route_mode": false,
            "security_groups": [],
            "security_groups_supported": false,
            "source_ip_session_persistence_supported": false,
            "subnets": [
                {
                    "crn": "crn:v1:bluemix:public:is:au-syd-1:a/b21af5a9874242b7851e780943d595a9::subnet:02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02",
                    "id": "02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02",
                    "name": "my-subnet",
                    "resource_type": "subnet"
                }
            ],
            "udp_supported": false
        }
   ```
   {: screen}

   Save the ID of the load balancer to use in the next steps. For example, save it in the variable `lbid`.

   ```bash
   lbid=0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727
   ```
   {: pre}

1. Get details about the Private Path load balancer:

   ```bash
    curl -H "Authorization: $iam_token" -X GET "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
   ```
   {: codeblock}

   Allow some time for provisioning. When the load balancer is ready, it is set to `online` and `active` status, as shown in the following sample output:

   ```sh
        {
            "availability": "region",
            "created_at": "2023-09-22T21:49:26.000Z",
            "crn": "crn:v1:bluemix:public:is:au-syd:a/b21af5a9874242b7851e780943d595a9::load-balancer:r026-d-86b51a9e-a058-4317-beed-8ee581a1f413",
            "hostname": ".invalid",
            "href": "https://au-syd.iaas.cloud.ibm.com/v1/load_balancers/r026-d-86b51a9e-a058-4317-beed-8ee581a1f413",
            "id": "r026-d-86b51a9e-a058-4317-beed-8ee581a1f413",
            "instance_groups_supported": false,
            "is_private_path": true,
            "is_public": false,
            "listeners": [
                {
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/load_balancers/r026-d-86b51a9e-a058-4317-beed-8ee581a1f413/listeners/r026-00b18a00-d860-4f4d-bb84-e29ffc2d8e47",
                    "id": "r026-00b18a00-d860-4f4d-bb84-e29ffc2d8e47"
                }
            ],
            "logging": {
                "datapath": {
                    "active": false
                }
            },
            "name": "my-load-balancer",
            "operating_status": "only",
            "pools": [
                {
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/load_balancers/r026-d-86b51a9e-a058-4317-beed-8ee581a1f413/pools/r026-f25e3f26-3aa6-4aef-9108-1587d3ac6679",
                    "id": "r026-f25e3f26-3aa6-4aef-9108-1587d3ac6679",
                    "name": "my-pool"
                }
            ],
            "private_ips": [
                {
                    "address": "10.245.0.4",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02/reserved_ips/02h7-e21d477e-3e71-47fc-aec4-9703766cf652",
                    "id": "02h7-e21d477e-3e71-47fc-aec4-9703766cf652",
                    "name": "nugget-ounce-stability-vocation",
                    "resource_type": "subnet_reserved_ip"
                },
                {
                    "address": "10.245.0.5",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02/reserved_ips/02h7-1910c529-dadd-4960-aba5-3c4604f99f56",
                    "id": "02h7-1910c529-dadd-4960-aba5-3c4604f99f56",
                    "name": "impostors-synopses-uncivic-livable",
                    "resource_type": "subnet_reserved_ip"
                },
                {
                    "address": "10.245.0.6",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02/reserved_ips/02h7-a7e55150-5b02-426f-b1dc-aa136effc6ce",
                    "id": "02h7-a7e55150-5b02-426f-b1dc-aa136effc6ce",
                    "name": "unmade-garnish-preview-defame",
                    "resource_type": "subnet_reserved_ip"
                },
                {
                    "address": "10.245.0.7",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02/reserved_ips/02h7-ae009701-8301-420c-862e-a9ec1f79214c",
                    "id": "02h7-ae009701-8301-420c-862e-a9ec1f79214c",
                    "name": "slacking-scuttle-stonework-reptile",
                    "resource_type": "subnet_reserved_ip"
                }
            ],
            "profile": {
                "family": "network",
                "href": "https://au-syd.iaas.cloud.ibm.com/v1/load_balancer/profiles/network-private-path",
                "name": "network-private-path"
            },
            "provisioning_status": "active",
            "public_ips": [],
            "resource_group": {
                "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/297468d87ca047c5be28368e52b055ce",
                "id": "297468d87ca047c5be28368e52b055ce",
                "name": "Default"
            },
            "resource_type": "load_balancer",
            "route_mode": false,
            "security_groups": [],
            "security_groups_supported": false,
            "source_ip_session_persistence_supported": false,
            "subnets": [
                {
                    "crn": "crn:v1:bluemix:public:is:au-syd-1:a/b21af5a9874242b7851e780943d595a9::subnet:02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02",
                    "href": "https://au-syd.iaas.cloud.ibm.com/v1/subnets/02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02",
                    "id": "02h7-ee9114e7-28d4-4416-bbfa-f274dbd49f02",
                    "name": "my-subnet",
                    "resource_type": "subnet"
                }
            ],
            "udp_supported": false
        }
   ```
   {: codeblock}

