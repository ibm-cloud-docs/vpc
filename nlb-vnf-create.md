---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-23"

keywords: network load balancer, public, listener, pool, round-robin

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating a route mode Network Load Balancer for VPC
{: #nlb-vnf}

Virtual Network Function (VNF) devices are virtualized network services (such as routers and firewalls) running on virtual machines. With {{site.data.keyword.vpc_short}}, you can provision VNF devices to gain better and more affordable scalability than you would by purchasing physical network devices.

Traffic destined for servers in {{site.data.keyword.vpc_short}} must be delivered to healthy VNF devices; otherwise, traffic disruption occurs. You can use network load balancers with route mode to perform health checks and to ensure that workloads only travel through healthy VNF devices. Because of this, network load balancers with route mode support only VNF devices as back-end targets.

## Prerequisites for NLB with route mode
{: #nlb-vnf-prereqs}

To support route mode, you must first create a service-to-service authentication policy for your NLB. To do, follow these steps:

1. From your browser, log in to [IBM Access Management](/iam/authorizations/grant).
1. Click **Authorizations**, then click **Create**.
1. For the source service, select **VPC Infrastructure Services**.
   For scope access, select **Resources based on selected attributes** > **Resource Type** > **Load Balancer for VPC**.
1. For the target service, select **VPC Infrastructure Services**.
   For scope access, select **Resources based on selected attributes** > **Resource Type** > **Virtual Private Cloud**.

## Creating a route mode network load balancer using the UI
{: #nlb-vnf-ui}
{: ui}

To create and configure {{site.data.keyword.nlb_full}} with route mode using the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external} and log in to your account.
1. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) from the upper left, then click **VPC Infrastructure > Load balancers**.
1. Click **New load balancer +** in the upper right of the page.
1. In the order form, complete the following information:
   * Type a unique name for your load balancer and select a VPC.
   * Select a resource group. Use the default group, or select from the list (if defined for your account). You cannot change the resource group after the load balancer is created.
   * Select the **Network Load Balancer (NLB)** tile and the subnet where you want to deploy the load balancer.
   * Select the **Private** type.
   * Optionally, add tags.
1. Review the checklist instructions in the route mode window, then click the button to enable it.

   You cannot modify the configuration of a route mode NLB after it finishes provisioning. 
   {: important}
1. Click **New Pool** and specify the following information to create a back-end pool. 
   * Type a name for the pool, such as `my-pool`.
   * Enter a protocol for your instances in this pool. The protocol of the pool must match the protocol of its associated listener. For example, if the listener is TCP, the protocol of the pool must be TCP.
   * Select the method, which is the load-balancing algorithm. The follow options are shown.
      * **Round robin** - Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
      * **Weighted round robin** - Forward requests to each instance in proportion to its assigned weight. For example, you have instances A, B, and C, and their weights are set to 60, 60, and 30. Instances A and B receive an equal number of connections, and instance C receives half as many connections.
      * **Least connections** - Forward requests to the instance with the least number of connections at the current time.
   * Choose session stickiness and select **None** or **Source IP**.
   * Under the health checks, the following options are shown.
      * **Health check path** - Health path is applicable only if **HTTP** is selected as the health check protocol. The health path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (/).
      * **Health protocol** - The protocol used by the load balancer to send health check messages to the instances in the pool.
      * **Health port** - The port on which to send health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
      * **Interval** - The interval, in seconds, between two consecutive health check attempts. By default, health checks are sent every 5 seconds.
      * **Timeout** - Maximum amount of time the system waits for a response from a health check request. By default, the load balancer waits 2 seconds for a response.
      * **Max retries** - Maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

      Although the load balancer stops sending connections to unhealthy instances, the load balancer continues monitoring the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

      If instances in the pool are unhealthy and you believe that your application is running fine, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
      {: tip}

1. Click **New listener** and specify the back-end pool to which this listener forwards traffic. 
      
   The protocol for receiving incoming requests, as well as the listening port on which requests are received, are already selected for you. 
   {: note}
   
1. An order summary shows pricing estimates. Review the Cloud Services terms. Then, click **Create** to complete your order.

## Creating a route mode network load balancer using the CLI
{: #nlb-vnf-cli}
{: cli}

To create a network load balancer using the CLI, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

1. Log in to your account using the CLI. After you enter the password, the system prompts which account and region that you want to use:

   ```
   ibmcloud login --sso
   ```
   {: pre}

1. Create a load balancer:

   ```
   ibmcloud is load-balancer-create nlb-test public --subnet 0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c --family network --route-mode true
   ```
   {: pre}

   Sample output:

   ```
   Creating load balancer vnf under account CNS Development Account - netsvs as user ibm@us.ibm.com...

   ID                          r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512
   Name                        vnf
   CRN                         crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512
   Family                      Network
   Route Mode Enabled          true
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

1. Wait until the NLB is in an active state, then run the following command;

   ```
   ibmcloud is load-balancer r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 
   ```
   {: pre}
   
   Sample output:
   
   ```
   Getting load balancer r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 under account CNS Development Account - netsvs as user ibm@us.ibm.com...
   
   ID                          r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512
   Name                        vnf
   CRN                         crn:v1:bluemix:public:is:us-east-2:a/be636a7a6e4d4b6296bedf669ce8f757::load-balancer:r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512
   Family                      Network
   Route Mode Enabled          true
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
   
   Take note of the first IP address listed in Private IPs. You require this when defining the custom routes. 

1. Add a pool.
   
   ```
   ibmcloud is load-balancer-pool-create example-pool r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 round_robin tcp 20 2 5 tcp  
   ```
   {: pre}
   
   Sample output:
   
   ```
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

   ```
   ibmcloud is load-balancer-pool-member-create r014-c7cadd8e-9e8b-4965-bee3-5d9ff95b3512 r014-474dca7d-aece-48a3-a636-fe8c3e654a3b 200 0767_5545cfb3-febc-4f09-b9ea-0aeb66074edf
   ```
   {: pre}

   Sample output:

   ```
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

## Creating a route mode network load balancer using the API
{: #nlb-vnf-api}
{: api}

To create a network load balancer by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Create your API request payload using the following template. Modify the name and subnet with your own values.

   ```
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
 
   The following properties must be set in the payload: 

      - `route_mode` is `true`
      - `is_public` is `false`
      - Profile name is `network_fixed`
      {: important}

   ```sh
   curl -s -H "Authorization: Bearer $IAM_TOKEN" -X POST -d @create.json "https://us-east.iaas.cloud.ibm.com/v1/load_balancers?version=2021-07-30&generation=2" | jq
   ```
   {: pre}

   Sample output:
   
   ```
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

1. After the NLB is in an active state, perform a `GET` call to fetch its status.

   ```sh
   curl -s -H "Authorization: Bearer $IAM_TOKEN" -X GET "https://us-east.iaas.cloud.ibm.com/v1/load_balancers/r014-020f4f34-bb49-4699-98a7-a53384cd649d?version=2021-07-30&generation=2" | jq
   ```
   {: pre}

   Sample output:
   
   ```
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
   
   Note the first IP address listed in `Private IPs`. You require this when you define the custom routes. 

1. Add a pool.

   First, create your API request payload using the following template. 

   ```
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

   ```
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

   ```
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
   
   ```
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

   ```
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
   
   ```
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

After the NLB with route mode enabled is in active state, follow these steps to finalize the creation of your NLB:

1. Gather the following information:
   * Your subnet CIDR.
   * In the load balancer response, you will find a list of private IPs. Make note of the first one. 
   
      You can generate this response by performing step 4 of either the [CLI](/docs/vpc?topic=vpc-nlb-vnf&interface=cli#nlb-vnf-cli) or [API](/docs/vpc?topic=vpc-nlb-vnf&interface=api) procedures.

   * Your back-end workload subnet CIDR.

1. Configure your custom routes as follows:
   * For all traffic destined for the backend customer workload subnet, set the next hop to the NLB private IP.
   * For all traffic destined for the client subnet, set the next hop to the NLB private IP.
