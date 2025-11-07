---

copyright:
  years: 2022, 2025
lastupdated: "2025-11-07"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Private Path network load balancer
{: #ppnlb-ui-creating-private-path-network-load-balancer}

You can use a Private Path network load balancer (NLB) only with a Private Path service. Create the Private Path NLB from the [Load Balancers for VPC page](/infrastructure/provision/loadBalancer) or as part of the Private Path service provisioning process. You can create a Private Path network load balancer by using the console, CLI, API, or Terraform.
{: shortdesc}

Private Path allows service providers to enable and manage private connectivity for the consumers of their hosted service. The Private Path service requires a Private Path NLB to establish a secure connection with each consumer's Virtual Private Endpoint (VPE) gateway. For more information, see [About Private Path services](/docs/vpc?topic=vpc-private-path-service-intro).

Private Path NLBs support the port range feature. Currently, all VPEs connected to a Private Path NLB can connect to any port in the range. For more information, see [Setting public network load balancer port ranges](/docs/vpc?topic=vpc-nlb-port-ranges&interface=ui).
{: note}

## Before you begin
{: #ppnlb-prerequisites}

Review the following requirements to ensure that your Private Path NLB is properly configured:

- If you don't have a VPC, create one in the same region that you want to create your Private Path NLB. Use the same VPC region for the Private Path NLB and Private Path service.
- Create your virtual server instances before creating a Private Path NLB. This ensures that it is fully operational.
- Make sure you have at least one subnet in your selected VPC.

You can create a Private Path network load balancer by using the console, CLI, API, or Terraform.

## Creating a Private Path network load balancer in the console
{: #ppnlb-ui}
{: ui}

To create and configure {{site.data.keyword.nlb_full}} in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Click **Create** in the upper right of the page.
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
   * **Type**: Select **Private Path**.
1. In the Back-end pools section, click **Create pool**, specify the following information, then click **Create**. You can create one or more pools.
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

1. You can attach members to your back-end pool now, or after you create your Private Path NLB. Click **Attach** on the table row of your back-end pool. Specify the following information, then click **Attach**.

   * **Member type**: Add virtual server instances, reserved IPs, or an application load balancer as a member. For virtual server instances, attach each type individually. A reserved IP member may be bound to a bare metal server, primary or secondary interface of a virtual server instance, or a virtual network interface.

      If you attach an ALB as a member target to a Private Path NLB pool, no other members can be added to that pool.
      {: note}

   * **Subnet**: Choose a subnet.
   * From the list of servers, select the servers that you want to attach to the back-end pool. Ensure that you specify valid values for each server port.

      Although you can attach more than one virtual server instance to a back-end pool, your Private Path load balancer provides regional availability and is resilient to zone failure even if a single subnet is selected.
      {: note}

    You do not need to create multiple Private Path load balancers or specify more than a single subnet to ensure resiliency to zone failure. Your subnet selection only impacts the IP-addresses that are associated with the load balancer. For more information on how many virtual server instances you can attach to a back-end pool, see [Quotas and service limits for Private Path network load balancers](/docs/vpc?topic=vpc-quotas#ppnlb-quotas).

   You can edit attached members by clicking the Edit icon ![Edit icon](images/edit.png) in the row of the member you want to edit in the back-end pools table. You can also delete attached members by selecting the minus icon in the row of the member you want to delete.
   {: tip}

1. In the Front-end listeners section, click **Create**, specify the following information, then click **Create**. You can create one or more listeners.
   * **Default back-end pool** - The default back-end pool to which this listener forwards traffic.
   * **Listener protocol** - The protocol to use for receiving incoming requests (**TCP**).
   * **Listener port** - The listening port on which requests are received.

1. Review the order summary, then click **Create** to complete your order.

## Creating a Private Path network load balancer from the CLI
{: #ppnlb-cli-creating-network-load-balancer}
{: cli}

The following example demonstrates creating a Private Path NLB from the CLI. In this example, the Private Path NLB is in front of one VPC virtual server instance (ID `0716_6acdd058-4607-4463-af08-d4999d983945`) running a TCP server that listens on port `9090`. The load balancer has a front-end listener, which allows secure access to the TCP server.

To create a Private Path NLB by from the CLI, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account with the CLI. After you enter the password, the system prompts you to choose which account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Create a Private Path NLB:

   ```sh
   ibmcloud is load-balancer-create ppnlb-test private-path --subnet cli-subnet-1 --family network
   ```
   {: pre}

   Sample output:

   ```sh
   Creating load balancer ppnlb-test in resource group under account IBM Cloud Network Services as user test@ibm.com...

    ID
    Name               ppnlb-test
    CRN
    Family             Network
    Host name
    Subnets            ID                                          Name
                                                                   cli-subnet-2

    Public IPs
    Reserved IPs         ID                                          Address        Subnet

    Provision status   create_pending
    Operating status   offline
    Is public          false
    Is private path    true
    Listeners
    Pools              ID                                 Name
                                                          my-pool
    Resource group     ID                                 Name
                                                          Default

    Created            2025-01-10T15:07:53+05:30
    Availability                 region
    Instance Group Supported     false
    SourceIP Session Supported   false
    Security groups supported    false
    UDP Supported                false
    Access mode                  private_path
    ```
    {: screen}



1. Create a pool:

   ```sh
   ibmcloud is load-balancer-pool-create my-pool pp-nlb-test round_robin tcp 20 2 5 http
   ```
   {: pre}

   Sample output:

   ```sh
   Creating pool my-pool of load balancer my-pool under account IBM Cloud Network Services as user test@ibm.com...

   ID
   Name                       my-pool
   Protocol                   tcp
   Algorithm                  round_robin
   Instance group             ID   Name
                              -    -
   Proxy protocol        disabled
   Health monitor             Type   Port   Health monitor URL   Delay   Retries   Timeout
                              http   -      /                    20      2         5

   Session persistence type   Type   Cookie name
                              -      -

   Members
   Provision status           create_pending
   Created                    2025-01-10T20:44:57+05:30
   ```
   {: screen}

1. Create a member:

   ```sh
   ibmcloud is load-balancer-pool-member-create test-ppnlb-1 test 3000 my-target
   ```
   {: pre}

   A user can create a Private Path NLB targeting a virtual server instance, reserved IP, or an application load balancer (ALB). Create a member with `my-target` as `my-instance` or `my-alb`.
   {: note}

   Sample output:

   ```sh
   Creating member of pool test under account IBM Cloud Network Services as user test@ibm.com...

   ID                 test
   Port               3000
   Target             10.240.66.14
   Weight             50
   Health             unknown
   Created            0001-01-01T05:53:28+05:53
   Provision status   create_pending
   ```
   {: screen}


1. Create a listener:

   ```sh
   ibmcloud is load-balancer-listener-create ppnlb-test --port-min 443 --protocol tcp
   ```
   {: pre}

   Sample output:

   ```sh
   Creating listener of load balancer ppnlb-test under account IBM Cloud Network Services as user test@ibm.com...

   ID
   Certificate instance      -
   Connection limit          -
   Ports                     443
   Idle connection timeout   -
   Protocol                  tcp
   Default pool              -
   Accept proxy protocol     false
   Provision status          create_pending
   Created                   2025-01-10T21:02:37+05:30
   ```
   {: screen}

1. Get details about your load balancer:

   ```sh
   ibmcloud is load-balancer ppnlb-test
   ```
   {: pre}

   Sample output:

   ```sh
   Getting load balancer ppnlb-test under account IBM Cloud Network Services as user test@ibm.com...

   ID
   Name               ppnlb-test
   CRN
   Family             Network
   Host name
   Subnets            ID                                          Name
                                                                  nlb

   Public IPs
   Reserved IPs       ID                                          Address        Subnet
   Provision status   active
   Operating status   online
   Is public          false
   Is private path    true
   Listeners
   Pools              ID                                          Name
                                                                  my-pool

   Resource group     ID                                 Name
                                                         Default

   Created                      2025-01-10T15:07:53+05:30
   Availability                 region
   Instance Group Supported     false
   SourceIP Session Supported   false
   Security groups supported    false
   UDP Supported                false
   Access mode                  private_path
   ```
   {: screen}

## Creating a network load balancer with the API
{: #ppnlb-api-creating-network-load-balancer}
{: api}

The following example illustrates creating a Private Path NLB with the API. The NLB detailed here is in front of two VPC virtual server instances (`192.168.100.5` and `192.168.100.6`) running a web application that listens on port `80`. It has a front-end listener, which allows secure access to the web application by using HTTPS.

This example skips the [prerequisite steps](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli) for using the API to provision a VPC, subnets, and instances.
{: note}

To create a Private Path NLB with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Store the following values in variables to be used in the API command:

   * `subnetId` - First, get your subnet and then populate the variable:

    ```bash
    export subnetId=<your_subnet_id>
    ```
    {: pre}

   * `targetId` - Second, get the virtual server instance, reserved IP, or application load balancer (ALB) that is in the same VPC as the subnet's:

    ```bash
    export targetId=<your_target_id>
    ```
    {: pre}

1. Create a Private Path NLB with a listener, pool, and attached server instances (pool members):

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
                        "target" : {"id" : "$targetId"}
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

   Allow time for provisioning. When the load balancer is ready, it is set to `online` and `active` status, as shown in the following sample output:

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

## Creating a Private Path network load balancer with Terraform
{: #creating-private-path-network-load-balancer-terraform}
{: terraform}

The following example creates a Private Path network load balancer with Terraform:

1. Create a Private Path NLB:

    ```terraform
    resource "ibm_is_lb" "example_ppnlb" {
      name    = "example-ppnlb"
      subnets = [ibm_is_subnet.example_subnet.id]
      profile = "network-private-path"
      type    = "private_path"
    }
    ```
    {: codeblock}

1. Optionally, create a pool for your Private Path NLB:

    ```terraform
    resource "ibm_is_lb_pool" "example_pool" {
      name            = "example-pool"
      lb              = [ibm_is_lb.example_ppnlb.id]
      algorithm       = "round_robin"
      protocol        = "tcp"
      health_delay    = 2
      health_retries  = 2
      health_timeout  = 1
      health_type     = "tcp"
    }
    ```
    {: codeblock}

1. Optionally, target an ALB or a virtual server instance ID as a pool member for your Private Path NLB:

    ```terraform
    resource "ibm_is_lb" "example_alb" {
      name    = "example-alb"
      subnets = [ibm_is_subnet.example_subnet.id]
    }
    ```
    {: codeblock}

    ```terraform
    resource "ibm_is_lb_pool_member" "example_member" {
      lb        = [ibm_is_lb.example_ppnlb.id]
      pool      = element(split("/", ibm_is_lb_pool.example_pool.id), 1)
      port      = 8080
      weight    = 20
      target_id = [ibm_is_lb.example_alb.id]
    }
    ```
    {: codeblock}

1. Optionally, target a reserved IP as a pool member for your Private Path NLB:


    ```terraform
    resource "ibm_is_subnet_reserved_ip" "example" {
    subnet = ibm_is_subnet.example.id
    }
    ```
    {: codeblock}

    ```terraform
    resource "ibm_is_lb_pool_member" "example" {
      lb        = ibm_is_lb.example_ppnlb.id
      pool      = element(split("/", ibm_is_lb_pool.example.id), 1)
      port      = 8080
      weight    = 20
      target_id = ibm_is_subnet_reserved_ip.example.id
    }
    ```
    {: codeblock}

For documentation about Terraform resources, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_private_path_service_gateway).{: external}
