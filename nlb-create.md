---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-24"

keywords: network load balancer, public, listener, pool, round-robin

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a public or private network load balancer
{: #nlb-ui-creating-network-load-balancer}

You can create an {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) with the UI, CLI, or API.

## Before you begin
{: #before-you-begin-nlb-public-private}

To configure either a public or private NLB, make sure that you have met the following prerequisites:

* If you don't have a VPC, create a VPC in the region that you want to create your NLB.
* Create a subnet in the preferred zone in your VPC.

The NLB service may add rules to custom routing tables to ensure service availability for some failure conditions. As a result, if the client is outside the zone and/or VPC of the NLB, you must add an ingress custom routing table to the VPC where the NLB resides with the proper traffic source selected.
{: important}

For Private NLB, depending on the location of the clients, you must ensure that ingress routing tables exist (as described in Table 1).

| Client location | Routing table type | Traffic source |
|----|----|----|
| On-prem | Ingress | Direct Link |
| Another VPC or classic infrastructure | Ingress | Transit Gateway |
| Another availability zone of the same VPC | Ingress | VPC zone |
{: caption="Table 1: Traffic sources that require ingress custom routing tables." caption-side="bottom"}

For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
{: note}

## Creating a network load balancer in the UI
{: #nlb-ui}
{: ui}

To create and configure a network load balancer in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click **Infrastructure > Network > Load balancers**.
1. Click **Create +** in the upper right of the page.
1. For Load balancer type, select the Network Load Balancer (NLB) tile.
1. For Location, edit the following fields, if necessary.
   * **Geography** - Indicates the geography where you want the load balancer created.
   * **Region** - Indicates the region where you want the load balancer created.
1. For Details, complete the following information:
   * **Name** - Enter a name for the load balancer, such as `my-load-balancer`.
   * **Resource group** - Select a resource group for the load balancer.
   * **Tags** - (Optional) Add tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Access management tags** - (Optional) Add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Virtual private cloud** - Select your VPC.
   * For the type. select either **Public** or **Private**.

      **For Private type only**, you can enable routing mode, which is used to deploy highly available virtual network functions (VNFs). For use cases and end-to-end instructions, see [About HA virtual network function deployments](/docs/vpc?topic=vpc-about-vnf-ha).
      {: note}

   * For the DNS type, select either **Public** or **Private**. Private DNS zones are resolvable only on IBM Cloud, and only from explicitly permitted networks in an account or with cross-account access.

      **For Private type only**, click **Bind+** to enter your DNS instance and zone information, then click **Bind**.
      {: note}

   * Select the subnets in which to create your load balancer. To maximize the availability of your application, select subnets in different zones.
1. For Back-end pools, click **Create pool** and specify the following information to create a back-end pool. When you are done, click **Create**.

   You can create one or more pools.
   {: note}

   * Type a name for the pool, such as `my-pool`.
   * Select a protocol for your instances in this pool. The protocol of the pool must match the protocol of its associated listener. For example, if the listener is TCP, the protocol of the pool must be TCP. Or, if the listener is UDP, the protocol of the pool must also be UDP.
   * Choose session stickiness and select **None** or **Source IP**. If you choose **Source IP**, all requests during your session are sent to the same instance.
   * Select the method, which is the load-balancing algorithm. The following options are shown:
      * **Round robin** - Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
      * **Weighted round robin** - Forward requests to each instance in proportion to its assigned weight. For example, if you have instances A, B, and C, and their weights are set to 60, 60, and 30, then instances A and B receive an equal number of connections, and instance C receives half as many connections.
      * **Least connections** - Forward requests to the instance with the least number of connections at the current time.
   * For Health check, the following options are shown:
     * **Health check path** - A health check path is applicable only if you select HTTP as the health check protocol. The health check path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (/).
     * **Health protocol** - The protocol used by the load balancer to send health check messages to the instances in the pool.
     * **Health port** - The port on which the load balancer sends health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
     * **Interval** - The interval in seconds between two consecutive health check attempts. By default, health checks are sent every 5 seconds.
     * **Timeout (sec)** - The maximum amount of time the system waits for a response from a health check request. By default, the load balancer waits 2 seconds for a response.
     * **Max retries** - The maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

      You can attach server instances after you create your back-end pool.
      {: note}

    

     Although the load balancer stops sending connections to unhealthy instances, the load balancer continues monitoring the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

     If instances in the pool are unhealthy and you believe that your application is running fine, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
     {: tip}

1. Click **Create listener** and specify the following information:
   * **Default back-end pool** - The default back-end pool to which this listener forwards traffic.
   * **Protocol** - The protocol to use for receiving incoming requests.
   * **Listener port** - The listening port on which requests are received. Choices are:
      * **Single listener port** - The port that the load balancer receives your inbound customer traffic.
      * **Listener port range** - The range of ports where the load balancer receives your inbound customer traffic.

   Then, click **Create**. You can create one or more listeners.
1. For Security groups, select the security groups that you want to attach to your load balancer, or click **Create** to create a new security group to attach to your NLB.

   Ensure that the security group allows for load-balancing traffic (listener, targets, and health check ports). If you do not specify a security group, the default security group from your VPC attaches instead.
   {: note}

1. Click **Add to Estimate** to review the pricing for your load balancer and create it.

## Creating a network load balancer from the CLI
{: #nlb-cli-creating-network-load-balancer}
{: cli}

The following example illustrates using the CLI to create a {{site.data.keyword.nlb_full}} (NLB). In this example, it is in front of one VPC virtual server instance (ID `0716_6acdd058-4607-4463-af08-d4999d983945`) running a TCP server that listens on port `9090`. The load balancer has a front-end listener, which allows secure access to the TCP server.

To create a network load balancer with the CLI, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account from the CLI. After you enter the password, the system prompts which account and region that you want to use.

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Create a load balancer.

   ```sh
   ibmcloud is load-balancer-create nlb-test public --subnet 0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c --family network
   ```
   {: pre}

   Sample output:

   ```sh
   Creating load balancer nlb-test in resource group under account IBM Cloud Network Services as user test@ibm.com...

    ID                 r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
    Name               nlb-test
    CRN                crn:v1:public:is:us-south-1:a/123456::load-balancer:r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
    Family             Network
    Host name          99b5ab45-us-south.lb.test.appdomain.cloud
    Subnets            ID                                          Name
                       0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c   nlb
    Security Groups    ID                                          Name
                       0581-a1336811-39dc-5aff-a0e6-6489a43ca62v   nlb1

    Public IPs
    Private IPs
    Provision status   create_pending
    Operating status   offline
    Is public          true
    Listeners
    Pools              ID   Name

    Resource group     ID                                 Name
                       3021f90279574ce287dd5fba82c08899   Default

    Created            2020-08-27T14:34:34.732-05:00
   ```
   {: screen}

   Create a private network load balancer.

   ```sh
   ibmcloud is load-balancer-create nlb-test private --subnet 07a7-37b4dcfc-841e-4d4a-9f9f-9e45ffbd0285 --family network
   ```
   {: pre}

   Sample output:

   ```sh
   Creating load balancer nlb-test in resource group  under account CNS Development Account - netsvs as user test@us.ibm.com...

   ID                 r018-8a994baa-21ba-428c-ac3f-e3fd91fa92c9
   Name               nlb-test
   CRN                crn:v1:bluemix:public:is:eu-gb-3:a/123456::load-balancer:r018-8a994baa-21ba-428c-ac3f-e3fd91fa92c9
   Family             Network
   Host name          8a994baa-eu-gb.lb.appdomain.cloud
   Subnets            ID                                          Name
                      07a7-37b4dcfc-841e-4d4a-9f9f-9e45ffbd0285   nlb1
   Security Groups    ID                                          Name
                      0581-a1336811-39dc-5aff-a0e6-6489a43ca626

   Public IPs
   Private IPs
   Provision status   create_pending
   Operating status   offline
   Is public          false
   Listeners
   Pools              ID   Name

   Resource group     ID                                 Name
                      42c4f51adc3147b4b4049ad9826c30a1   Default

   Created            2021-03-22T11:34:57.34-05:00
   ```
   {: screen}



1. Create a member.

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

1. Create a listener.

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

1. Get details about your load balancer.

   ```sh
   ibmcloud is load-balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
   ```
   {: pre}

   Sample output:

   ```sh
   Getting load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...

   ID                 r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
   Name               nlb-test
   CRN                crn:v1:public:is:us-south-1:a/123456::load-balancer:r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
   Family             Network
   Host name          99b5ab45-us-south.lb.test.appdomain.cloud
   Subnets            ID                                          Name
                      0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c   nlb

   Public IPs         150.238.50.78, 150.238.54.95
   Private IPs        10.240.0.58, 10.240.0.59
   Provision status   active
   Operating status   online
   Is public          true
   Listeners          r006-2847a948-f9b6-4fc1-91c6-f1c49dac3eba
   Pools              ID                                          Name
                      r006-3b66d605-6aa5-4166-9f66-b16054da3cb0   nlb-pool

   Resource group     ID                                 Name
                      3021f90279574ce287dd5fba82c08899   Default

   Created            2020-08-27T14:34:34.732-05:00
   ```
   {: screen}

## Creating a network load balancer with the API
{: #nlb-api-creating-network-load-balancer}
{: api}

The following example illustrates creating a network load balancer in front of two VPC virtual server instances (`192.168.100.5` and `192.168.100.6`) running a web application that listens on port `80`. The load balancer has a front-end listener, which allows secure access to the web application by using HTTPS.

The example skips the [prerequisite steps](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=api) for using the API to provision a VPC, subnets, and instances.
{: note}

To create a network load balancer with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

1. Store the following values in variables to be used in the API command.

   * `ResourceGroupId` - First, get your resource group and then populate the variable.

    ```bash
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

1. Create a load balancer with a listener, pool, and attached server instances (pool members).

   ```bash
   curl -H "Authorization: $iam_token" -X POST
   "$vpc_api_endpoint/v1/load_balancers?version=$api_version&generation=2" \
       -d '{
           "name": "example-balancer",
           "is_public": true,
           "profile": {
               "name": "network-fixed"
           },
           "listeners": [
               {
                   "certificate_instance": {
                     "crn": "crn:v1:bluemix:public:cloudcerts:us-south:a/123456:b8877ea4-b8eg-467e-912a-da1eb7f031cg:certificate:43219c4c97d013fb2a95b21dddde1234"
                   },
                   "port": 443,
                   "protocol": "tcp",
                   "default_pool": {
                       "name": "example-pool"
                   }
               }
           ],
           "pools": [
               {
                   "algorithm": "round_robin",
                   "health_monitor": {
                       "delay": 5,
                       "max_retries": 2,
                       "timeout": 2,
                       "type": "tcp",
                       "url_path": "/"
                   },
                   "name": "example-pool",
                   "protocol": "tcp",
                   "session_persistence": {
                       "cookie_name": "string",
                       "type": "source_ip"
                   },
                   "members": [
                       {
                           "port": 80,
                           "target": {
                               "address": "192.168.100.5"
                           },
                           "weight": 50
                       },
                       {
                           "port": 80,
                           "target": {
                               "address": "192.168.100.6"
                           },
                           "weight": 50
                       }
                   ]
               }
           ],
           "subnets": [
               {
                   "id": "7ec87131-1c7e-4990-b4f0-a26f2e61f98e"
               }
           ]
           }'
   ```
   {: codeblock}

   Sample output:

   ```json
   {
       "created_at": "2018-07-12T23:17:07.5985381Z",
       "crn": "crn:v1:bluemix:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
       "hostname": "ac34687d.lb.appdomain.cloud",
       "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
       "id": "0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
       "is_public": true,
       "profile": {
           "name": "network-fixed",
           "family": "network"
       },
       "listeners": [
           {
               "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
               "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/listeners/70294e14-4e61-11e8-bcf4-0242ac110004"
           }
       ],
       "name": "example-balancer",
       "operating_status": "offline",
       "pools": [
           {
               "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
               "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/pools/70294e14-4e61-11e8-bcf4-0242ac110004",
               "name": "example-pool"
           }
       ],
       "provisioning_status": "create_pending",
       "resource_group": {
           "id": "56969d60-43e9-465c-883c-b9f7363e78e8"
       },
       "subnets": [
           {
               "id": "0738-7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
               "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
               "name": "example-subnet"
           }
       ]
   }
   ```
   {: codeblock}

   Save the ID of the load balancer to use in the next steps. For example, you can save it in the variable `lbid`.

   ```bash
   lbid=0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727
   ```
   {: pre}

1. Get details about the load balancer.

   ```bash
    curl -H "Authorization: $iam_token" -X GET "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
   ```
   {: codeblock}

   Allow some time for provisioning. When the load balancer is ready, it is set to `online` and `active` status, as shown in the following sample output:

   ```json
   {
    "id": "0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "crn": "crn:v1:bluemix:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "name": "example-balancer",
    "created_at": "2018-07-13T22:22:24.489Z",
    "hostname": "dd754295-e9e0-4c9d-bf6c-58fbc59e5727.lb.appdomain.cloud",
    "is_public": true,
    "profile": {
          "name": "network-fixed",
          "family": "network"
    },
    "listeners": [
      {
        "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
         "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/listeners/70294e14-4e61-11e8-bcf4-0242ac110004"
      }
    ],
    "operating_status": "online",
    "pools": [
      {
        "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/pools/70294e14-4e61-11e8-bcf4-0242ac110004",
        "name": "example-pool"
      }
    ],
    "private_ips": [
      {
        "address": "192.168.10.5"
      },
      {
        "address": "192.168.10.6"
      }
    ],
    "provisioning_status": "active",
    "public_ips": [
      {
          "address": "169.11.111.115"
      },
      {
          "address": "169.11.111.116"
      }
    ],
    "resource_group": {
      "id": "0738-56969d60-43e9-465c-883c-b9f7363e78e8"
    },
    "subnets": [
      {
        "id": "0738-7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
        "name": "example-subnet"
      }
    ]
   ```
   {: codeblock}
