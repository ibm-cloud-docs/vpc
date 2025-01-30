---

copyright:
  years: 2020, 2025
lastupdated: "2025-01-29"

keywords: network load balancer, public, listener, pool, round-robin

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a private network load balancer with routing mode
{: #nlb-vnf}

Virtual Network Function (VNF) devices are virtualized network services (such as routers and firewalls) running on virtual machines. With {{site.data.keyword.vpc_short}}, you can provision VNF devices to gain better and more affordable scalability than you would by purchasing physical network devices.

Traffic destined for servers in {{site.data.keyword.vpc_short}} must be delivered to healthy VNF devices; otherwise, traffic disruption occurs. You can use network load balancers with routing mode to perform health checks and to ensure that workloads only travel through healthy VNF devices. Because of this, network load balancers with routing mode support only VNF devices as back-end targets.

For detailed examples of various ways you can deploy your NLB with routing mode, refer to [HA VNF deployments](/docs/vpc?topic=vpc-about-vnf-ha&interface=ui).
{: tip}

## Before you begin
{: #before-you-begin-nlb-route-mode-enabled}

To configure a network load balancer with routing mode enabled, make sure that you have met the following prerequisites:

* If you don't have a VPC, [create a VPC](/docs/vpc?topic=vpc-getting-started&interface=ui) in the region that you want to create your NLB.
* Create a subnet in your preferred zone in your VPC.
* To support routing mode, you must first create a service-to-service authentication policy for your NLB. To do, follow these steps:

   1. From your browser, log in to [IBM Access Management](/iam/authorizations/grant).
   1. Click **Authorizations**, then click **Create**.
   1. For the source service, select **VPC Infrastructure Services**.
      For scope access, select **Resources based on selected attributes** > **Resource Type** > **Load Balancer for VPC**.
   1. For the target service, select **VPC Infrastructure Services**.
      For scope access, select **Resources based on selected attributes** > **Resource Type** > **Virtual Private Cloud**.
   1. Select the **Editor** checkbox to give yourself Editor access, then click **Authorize**.

The following list details some important considerations about network load balancer with routing mode enabled:

* NLB route mode has two IP addresses (Active/Standby).
* NLB route mode is designed to be transparent. When a failover occurs, route mode updates all routing rules created under the same VPC with the `next_hop` of the Standby appliance IP. As a result, both IPs may be used during the lifetime of your NLB with route mode.
* In the case of a Transit VPC configuration, you may want to deploy the NLB in `vpc-hub` and configure the egress route in `vpc-spoke`. This will force data back to the NLB with route mode in `vpc-hub`. Ensure that you also add the equivalent ingress rules of the `vpc-spoke` egress rules to `vpc-hub` so that the egress rules do not need to be changed. Once the traffic reaches `vpc-hub`, the ingress routing rule will overwrite the `next_hop` and send it to the primary appliance.


## Creating a network load balancer with routing mode using the UI
{: #nlb-vnf-ui}
{: ui}

To create and configure {{site.data.keyword.nlb_full}} with routing mode with the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure** ![VPC icon](../../icons/vpc.svg)  > **Load balancers**.
1. Click **New load balancer +** in the upper right of the page.
1. In the order form, complete the following information:
   * Type a unique name for your load balancer and select a VPC.
   * Select a resource group. Use the default group, or select from the list (if defined for your account). You cannot change the resource group after you create the load balancer.
   * Select the **Network Load Balancer (NLB)** tile and the subnet where you want to deploy the load balancer.
   * Select the **Private** type.
   * Optionally, add tags.
1. Review the checklist instructions in the routing mode window, then click the button to enable it.

   You cannot modify the configuration of a routing mode NLB after it finishes provisioning.
   {: important}

1. Click **New Pool** and specify the following information to create a back-end pool.
   * Type a name for the pool, such as `my-pool`.
   * Enter a protocol for your instances in this pool. The protocol of the pool must match the protocol of its associated listener. For example, if the listener is TCP, the protocol of the pool must be TCP.
   * Select the method, which is the load-balancing algorithm. The follow options are shown:
      * **Round robin** - Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
      * **Weighted round robin** - Forward requests to each instance in proportion to its assigned weight. For example, if you have instances A, B, and C, and their weights are set to 60, 60, and 30, then instances A and B receive an equal number of connections, and instance C receives half as many connections.
      * **Least connections** - Forward requests to the instance with the least number of connections at the current time.
   * Choose session stickiness and select **None**.
   * Under the health checks, the following options are shown.
      * **Health check path** - Health check path is applicable only if you select **HTTP** as the health check protocol. The health check path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (/).
      * **Health protocol** - The protocol used by the load balancer to send health check messages to the instances in the pool.
      * **Health port** - The port on which the load balancer sends health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
      * **Interval** - The interval, in seconds, between two consecutive health check attempts. By default, health checks are sent every 5 seconds.
      * **Timeout (sec)** - The maximum amount of time the system waits for a response from a health check request. By default, the load balancer waits 2 seconds for a response.
      * **Max retries** - The maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

      Although a load balancer stops sending connections to unhealthy instances, it still continues to monitor the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

      If instances in the pool are unhealthy and you believe that your application is running correctly, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
      {: tip}

1. Click **New listener** and specify the back-end pool to which this listener forwards traffic.

   The protocol for receiving incoming requests, as well as the listening port on which requests are received, are already selected for you.
   {: note}

1. An order summary shows pricing estimates. Review the Cloud Services terms. Then, click **Create** to complete your order.

## Creating a network load balancer with routing mode by using the CLI
{: #nlb-vnf-cli}
{: cli}

To create a network load balancer with the CLI, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Create a load balancer:

   ```sh
   ibmcloud is load-balancer-create nlb-test public --subnet 0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c --family network --route-mode true
   ```
   {: pre}

   Sample output:

   ```sh
   Creating load balancer vnf under account CNS Development Account - netsvs as user ibm@us.ibm.com...

   ID                          r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512
   Name                        vnf
   CRN                         crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512
   Family                      Network
   Routing Mode Enabled        true
   Host name                   c7cadd8e-us-east.lb.appdomain.cloud
   Subnets                     ID                                          Name
                               0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024   nlb-subnet

   Public IPs
   Private IPs
   Provision status            create_pending
   Operating status            offline
   Is public                   false
   Listeners
   Pools                       ID   Name

   Resource group              ID                                 Name
                               42c4f51adc3147b4b4049ad9826c30a1   Default

   Created                     2021-09-14T17:54:24.584-05:00
   Security groups supported   false
   ```
   {: screen}

1. Wait until the NLB is in an active state, then run the following command: {: #nlb-get-private-ips-cli}

   ```sh
   ibmcloud is load-balancer r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 
   ```
   {: pre}

   Sample output:

   ```sh
   Getting load balancer r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 under account CNS Development Account - netsvs as user ibm@us.ibm.com...

   ID                          r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512
   Name                        vnf
   CRN                         crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512
   Family                      Network
   Routing Mode Enabled        true
   Host name                   c7cadd8e-us-east.lb.appdomain.cloud
   Subnets                     ID                                          Name
                               0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024   nlb-subnet

   Public IPs
   Private IPs                 10.241.64.16, 10.241.64.17
   Provision status            active
   Operating status            online
   Is public                   false
   Listeners
   Pools                       ID   Name

   Resource group              ID                                 Name
                               42c4f51adc3147b4b4049ad9826c30a1   Default

   Created                     2021-09-14T17:54:24.584-05:00
   Security groups supported   false
   ```
   {: screen}

   Take note of the first IP address listed in Private IPs. You need this when defining custom routes.
   {: tip}

1. Add a pool.

   ```sh
   ibmcloud is load-balancer-pool-create example-pool r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 round_robin tcp 20 2 5 tcp  
   ```
   {: pre}

   Sample output:

   ```sh
   Creating pool example-pool of load balancer r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 under account CNS Development Account - netsvs as user ibm@us.ibm.com

   ID                    r014-474dca7d-aece-48a3-a636-fe8c3e654a3b
   Name                  example-pool
   Protocol              tcp
   Algorithm             round_robin
   Instance group        ID   Name
                         -    -

   Proxy protocol        disabled
   Health monitor        Type   Port   Health monitor URL   Delay   Retries   Timeout
                         tcp    -                           20      2         5

   Session persistence   Type   Cookie name
                         -      -

   Members
   Provision status      active
   Created               2021-09-14T18:03:56.274-05:00
   ```
   {: screen}

1. Add a member.

   ```sh
   ibmcloud is load-balancer-pool-member-create r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 r014-474dca7d-aece-48a3-a636-fe8c3e654a3b 200 0767_5545cfb3-febc-4f09-b9ea-0aeb66074edf
   ```
   {: pre}

   Sample output:

   ```sh
   Creating member of pool r014-474dca7d-aece-48a3-a636-fe8c3e654a3b under account CNS Development Account - netsvs as user ibm@us.ibm.com...

   ID                 r014-1fdb900f-81a4-4204-839a-fcdee6c28e8a
   Port               200
   Target             0767_5545cfb3-febc-4f09-b9ea-0aeb66074edf
   Weight             50
   Health             unknown
   Created            2021-09-14T19:27:11.077-05:00
   Provision status   create_pending
   ```
   {: screen}

## Creating a network load balancer with routing mode with the API
{: #nlb-vnf-api}
{: api}

To create a network load balancer with routing mode with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Create your API request payload using the following template. Modify the name and subnet with your own values.

   ```sh
   {
      "name": "nlb-vnf",
      "is_public": false,
      "profile": {
         "name": "network-fixed"
         },
      "route_mode": true,
      "subnets": [
         {
      "id": "0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024"
         }
       ]
   }
   ```
   {: pre}

   Save this payload to a JSON file. In the following examples, the file is named `create.json`.

1. Make the request to create your NLB with the following command.
 
   The following properties must first be set in the payload:

      - `route_mode` is `true`
      - `is_public` is `false`
      - Profile name is `network_fixed`

   ```sh
   curl -s -H "Authorization: Bearer $IAM_TOKEN" -X POST -d @create.json "https://us-east.iaas.cloud.ibm.com/v1/load_balancers?version=2021-07-30&generation=2" | jq
   ```
   {: pre}

   Sample output:

   ```sh
   {
     "id": "r014-020f4f34-bb49-4699-98a7-a53384cd649d",
     "name": "nlb-vnf",
     "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d",
     "crn": "crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-020f4f34-bb49-4699-98a7-a53384cd649d",
     "is_public": false,
     "created_at": "2021-09-14T21:45:06.377862525Z",
     "hostname": "020f4f34-us-east.lb.appdomain.cloud",
     "listeners": [],
     "operating_status": "offline",
     "pools": [],
     "private_ips": [],
     "provisioning_status": "create_pending",
     "public_ips": [],
     "subnets": [
       {
         "id": "0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
         "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
         "crn": "crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::subnet:0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
         "name": "nlb-subnet"
       }
     ],
     "resource_group": {
       "id": "42c4f51adc3147b4b4049ad9826c30a1",
       "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/42c4f51adc3147b4b4049ad9826c30a1",
       "name": "Default"
     },
     "resource_type": "load_balancer",
     "logging": {
       "datapath": {
         "active": false
       }
     },
     "profile": {
       "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancer/profiles/network-fixed",
       "name": "network-fixed",
       "family": "Network"
     },
     "security_groups": [],
     "security_group_supported": false,
     "route_mode": true
   }
   ```
   {: screen}

1. After the NLB is in an active state, perform a `GET` call to fetch its status.{: #nlb-get-private-ips-api}

   ```sh
   curl -s -H "Authorization: Bearer $IAM_TOKEN" -X GET "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d?version=2021-07-30&generation=2" | jq
   ```
   {: pre}

   Sample output:

   ```sh
   {
     "id": "r014-020f4f34-bb49-4699-98a7-a53384cd649d",
     "name": "nlb-vnf",
     "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d",
     "crn": "crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-020f4f34-bb49-4699-98a7-a53384cd649d",
     "is_public": false,
     "created_at": "2021-09-14T21:45:06.377863Z",
     "hostname": "020f4f34-us-east.lb.appdomain.cloud",
     "listeners": [],
     "operating_status": "online",
     "pools": [],
     "private_ips": [
       {
          "address": "10.241.64.13"
       },
       {
          "address": "10.241.64.14"
       }
    ],
     "provisioning_status": "active",
     "public_ips": [],
     "subnets": [
        {
          "id": "0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
          "href": "https://us-east.iaas.cloud.ibm.com/v1/subnets/0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
          "crn": "crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::subnet:0767-064498f3-4df5-4fa5-b2ed-de5a3bfea024",
          "name": "nlb-subnet"
        }
     ],
     "resource_group": {
       "id": "42c4f51adc3147b4b4049ad9826c30a1",
       "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/42c4f51adc3147b4b4049ad9826c30a1",
       "name": "Default"
    },
     "resource_type": "load_balancer",
     "logging": {
        "datapath": {
        "active": false
       }
     },
     "profile": {
       "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancer/profiles/network-fixed",
       "name": "network-fixed",
       "family": "Network"
     },
     "security_groups": [],
     "security_group_supported": false,
     "route_mode": true
   }
   ```
   {: screen}

   Note the first IP address listed in `Private IPs`. You need this when defining custom routes. 
   {: tip}

1. Add a pool.

   First, create your API request payload using the following template.

   ```sh
   {
     "algorithm": "round_robin",
     "protocol": "tcp",
     "health_monitor": {
       "delay": 10,
       "max_retries": 2,
       "timeout": 5,
       "type": "tcp"
    }
   }
   ```
   {: pre}

   Then, run the following command:

   ```sh
   curl -s -H "Authorization: Bearer $IAM_TOKEN" -X POST -d @create.json "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d/pools?version=2021-07-30&generation=2" | jq
   ```
   {: pre}

   Sample output:

   ```sh
   {
     "id": "r014-0b8f4cde-4684-484b-bd3c-64f55a917328",
     "name": "flashbulb-marvelous-gainfully-skydiver",
     "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d/pools/r014-0b8f4cde-4684-484b-bd3c-64f55a917328",
     "algorithm": "round_robin",
     "health_monitor": {
     "delay": 10,
     "max_retries": 2,
       "timeout": 5,
       "type": "tcp"
     },
     "protocol": "tcp",
     "created_at": "2021-09-14T21:56:08.202217034Z",
     "provisioning_status": "active",
     "proxy_protocol": "disabled"
   }
   ```
   {: screen}

1. Wait until the NLB is in an active state, then add a listener. 

   The port range for the listener must be defined as follows:
   - `port_min` is `1`
   - `port_max` is `65535`

   Create an API request payload using the following template:

   ```sh
   {
     "default_pool": {
       "id": "r014-0b8f4cde-4684-484b-bd3c-64f55a917328"
     },
     "port_min": 1,
     "port_max": 65535,
     "protocol": "tcp"
   }
   ```
   {: pre}

   Run the following command:

   ```sh
   curl -s -H "Authorization: Bearer $IAM_TOKEN" -X POST -d @create_vnf.json "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d/listeners?version=2021-07-30&generation=2" | jq
   ```
   {: pre}

   Sample output:

   ```sh
   {
     "id": "r014-0ce87b15-9530-4624-a574-92495631f75d",
     "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d/listeners/r014-0ce87b15-9530-4624-a574-92495631f75d",
     "protocol": "tcp",
     "port": 1,
     "port_min": 1,
     "port_max": 65535,
     "default_pool": {
       "id": "r014-0b8f4cde-4684-484b-bd3c-64f55a917328",
       "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d/pools/r014-0b8f4cde-4684-484b-bd3c-64f55a917328",
       "name": "flashbulb-marvelous-gainfully-skydiver"
     },
     "provisioning_status": "create_pending",
     "created_at": "2021-09-14T22:01:13.523172702Z",
     "accept_proxy_protocol": false,
     "https_redirect": null
   }
   ```
   {: screen}

1. Add the pool member.

   First, create an API request payload using the following template:

   ```sh
   {
     "port": 90,
     "target": {
       "id": "0767_5545cfb3-febc-4f09-b9ea-0aeb66074edf"
     }
   }
   ```
   {: pre}

   Then run the following command:

   ```sh
   curl -s -H "Authorization: Bearer $IAM_TOKEN" -X POST -d @add_member.json "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d/pools/r014-0b8f4cde-4684-484b-bd3c-64f55a917328/members?version=2021-07-30&generation=2" | jq
   ```
   {: pre}

   Sample output:

   ```sh
   {
     "id": "r014-c625efe0-6f69-4ae7-a2ef-9bf33c811a2c",
     "href": "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d/pools/r014-0b8f4cde-4684-484b-bd3c-64f55a917328/members/r014-c625efe0-6f69-4ae7-a2ef-9bf33c811a2c",
     "port": 90,
     "target": {
       "id": "0767_5545cfb3-febc-4f09-b9ea-0aeb66074edf",
       "href": "https://us-east.iaas.cloud.ibm.com/v1/instances/0767_5545cfb3-febc-4f09-b9ea-0aeb66074edf",
       "crn": "crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::instance:0767_5545cfb3-febc-4f09-b9ea-0aeb66074edf"
     },
     "weight": 50,
     "health": "unknown",
     "created_at": "2021-09-14T22:07:46.898295306Z",
     "provisioning_status": "create_pending"
   }
   ```
   {: screen}

## Next steps
{: #nob-vnf-nextsteps}

After the NLB with routing mode is in an active state, follow these steps to finalize your NLB configuration:

1. Gather the following information:
   * Your subnet CIDRs (subnets where your workload VSIs reside, whick will be the IPs to use as destination IPs).
   * In the load balancer response, you will find two private IP listeds. Make note of the first one.

      You can generate this response by performing step 4 of either the [CLI](/docs/vpc?topic=vpc-nlb-vnf&interface=cli#nlb-vnf-cli) or [API](/docs/vpc?topic=vpc-nlb-vnf&interface=api) procedures.

      The first IP listed is the primary IP, and the second is the secondary, for backup purposes. Both are listed so that in the future if a failover occurs, you will know both the active and passive IPs.
      {: tip}

   * Your back-end workload subnet CIDR.

1. Configure your custom routes as follows:
   * For all traffic destined for the back-end customer workload subnet, set the next hop to the NLB private IP.
   * For all traffic destined for the client subnet, set the next hop to the NLB private IP.
  
1. VNF instances (VNIs) must reside on the same subnet with the NLB, and must be added as a member to your pool.
