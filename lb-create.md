---

copyright:
  years: 2021
lastupdated: "2021-02-07"

keywords: application load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc network

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

# Creating an {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}}
{: #load-balancer}

You can create an {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) to distribute inbound traffic across multiple instances.
{: shortdesc}

## Creating an application load balancer using the UI
{: #lb-ui-creating-network-load-balancer}
{: ui}

To create an ALB:

1. In the navigation pane, click **Network > Load balancers**.
1. On the Load balancers page, click **New load balancer** and specify the following information.
    * **Name**: Enter a name for the load balancer, such as `my-load-balancer`.
    * **Virtual private cloud**: Select your VPC.
    * **Resource group**: Select a resource group for the load balancer.
    * **Tags**: (Optional) Enter tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
    * **Type**: Select the load balancer type.
      * A public load balancer has a public IP address, which means that it can route requests from clients over the internet.
      * A private load balancer has a private IP address, which means that it is accessible only to internal clients on your private subnets, within the same region and VPC.
    * **Region**: Indicates the region in which the load balancer will be created; that is, the region selected for your VPC.
    * **Subnets**: Select the subnets in which to create your load balancer. To maximize the availability of your application, select subnets in different zones.
    * **Security groups**: Select the security groups that you want to attach to your load balancer.
      Ensure that the security group allows for load balancing traffic (listener, backend, and health check ports). If you do not specify a security group, the default security group from your VPC attaches instead.
      {: note}
1. Click **New pool** and specify the following information to create a back-end pool. You can create one or more pools.
    * **Name**: Enter a name for the pool, such as `my-pool`.
    * **Protocol**: Select the protocol for your instances in this pool. The protocol of the pool must match the protocol of its associated listener. For example, if an HTTPS or HTTP protocol is selected for the listener, the protocol of the pool must be HTTP. Similarly, if the listener protocol is TCP, the protocol of the pool must be TCP.
    * **Method**: Select how you want the load balancer to distribute traffic across the instances in the pool:
      * **Round robin:** Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
      * **Weighted round robin:** Forward requests to each instance in proportion to its assigned weight. For example, you have instances A, B, and C, and their weights are set to 60, 60 and 30. Instances A and B receive an equal number of connections, and instance C receives half as many connections.
      * **Least connections:** Forward requests to the instance with the least number of connections at the current time.
    * **Session stickiness**: Select whether all requests during a user's session are sent to the same instance.
    * **Health checks**: Configure how the load balancer checks the health of the instances.
      * **Health check path**: Health path is applicable only if HTTP is selected as the health check protocol. The health path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (`/`).
      * **Health protocol**: The protocol used by the load balancer to send health check messages to the instances in the pool.
      * **Health port**: The port on which to send health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
      * **Interval**: Interval in seconds between two consecutive health check attempts. By default, health checks are sent every 5 seconds.
      * **Timeout**: Maximum amount of time the system waits for a response from a health check request. By default, the load balancer waits 2 seconds for a response.
      * **Max retries**: Maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

        Although the load balancer stops sending connections to unhealthy instances, the load balancer continues monitoring the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

      If instances in the pool are unhealthy and you believe that your application is running fine, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
      {: tip}

1. Click **Create**.
1. Next to the entry for the new pool, click **Attach** in the **Instances** column to add an instance to the pool. Click **Add** to add more instances to the pool. Specify the following information for each instance:
   * Select one or more subnets from which to select an instance.
   * Select an instance. If an instance has multiple interfaces, make sure that you select the correct IP address.
   * Specify the port on which traffic is sent to the instance.
   * If your pool uses the **Weighted round robin** method, assign a weight for each instance.

      Assigning '0' weight to an instance means that no new connections are forwarded to that instance, but any existing traffic continues to flow while the current connection is active. Using a weight of '0' can help bring down an instance gracefully and remove it from service rotation.
      {: tip}

1. Click **New listener** and specify the following information to create a listener. You can create one or more listeners.
   * **Protocol**: The protocol to use for receiving incoming requests.
   * **Port**: The listening port on which requests are received.
   * **Back-end pool**: The default back-end pool to which this listener forwards traffic.
   * **Max connections** (optional): Maximum number of concurrent connections the listener allows.
   * **SSL certificate**: If HTTPS is the selected protocol for this listener, you must select an SSL certificate. Make sure that the load balancer is authorized to access the SSL certificate. For more information, see [Before you begin](#before).
1. Click **Create**.
1. After you finish creating pools and listeners, click **Create load balancer**.
1. To view details of an existing load balancer, click the name of your load balancer on the **Load balancers** page.
1. If you want to redirect, forward, or reject particular incoming traffic for an HTTP or HTTPS front-end listener based on certain criteria, configure layer 7 policies.
   *  After the status of the load balancer changes to **Active**, click **Front-end listeners** in the navigation and click the value in the **Policies** column for the listener you created.
   *  On the Policies page, click **Add policy** and specify the following information to create a policy. You can create multiple policies.
        * **Name**: Enter a name for the policy, such as `my-policy`. The name must be unique within the listener.
        * **Action**: The action to take when all the rules for the policy match. You can reject a request with a 403 response, redirect the request to a configured URL and response code, or forward the request to a specific back-end pool. If an incoming request does not match the rules for any policies, the request is forwarded to the default back-end pool of the listener.
        * **Priority**: Within each action type, policies are evaluated in ascending order of priority. Policies to reject traffic are always evaluated first, regardless of their priority. Policies to redirect traffic are evaluated next, followed by policies to forward traffic.
        * **Redirect**: The URL to which the request is redirected, if the action is set to **Redirect**.
        * **Status Code**: The status code of the response returned by the load balancer, if the action is set to **Redirect**.
        * **Forward**: The back-end pool of virtual server instances to which the request is forwarded, if the action is set to **Forward to pool**.
   * On the Policies page, click **Add rule** for your policy. If rules exist for the policy, click the value in the **Rules** column to add more rules.
   * In the Rules window, click **Add rule** and specify the following information to create a rule. If you create multiple rules for a policy, the policy is applied only when all its rules are matched.
        * **Condition**: Specifies the condition with which a rule is evaluated.
        * **Type**: The type of information to be evaluated by the rule: the name of the host from which the request originated, an HTTP header field, or a path in the URL.
        * **Value**: The value to be matched.
        * **Key**: The name of the HTTP header field to evaluate, if the rule type is **Header**. For example, to match a cookie in the HTTP header, enter **Cookie** for the key.

## Creating an application load balancer using the CLI
{: #nlb-cli-creating-network-load-balancer}
{: cli}

The following example illustrates using the CLI to create an {{site.data.keyword.alb_full}} (ALB). In this example, it is in front of one VPC virtual server instance (id `0716_6acdd058-4607-4463-af08-d4999d983945`) running a TCP server that listens on port 9090. The load balancer has a front-end listener, which allows secure access to the TCP server.

To create an application load balancer by using the CLI, perform the following procedure:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

2. Use the terminal to log in to your account using the CLI. After you enter the password, the system prompts which account and region you want to use:

    ```
    ibmcloud login --sso
    ```
    {: pre}

3. Create a load balancer:

  ```
  ibmcloud is load-balancer-create alb-test public --subnet 0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c --family network
  ```
  {: pre}

  Sample output:

  ```
  Creating load balancer nlb-test in resource group under account IBM Cloud Network Services as user test@ibm.com...

    ID                 r134-99b5ab45-6357-42db-8b32-5d2c8aa62776
    Name               alb-test
    CRN                crn:v1:public:is:us-south-1:a/6266f0fbb7df487d8438b9b31d24cd96::load-balancer:r134-99b5ab45-6357-42db-8b32-5d2c8aa62776
    Family             Network
    Host name          99b5ab45-us-south.lb.test.appdomain.cloud
    Subnets            ID                                          Name
                       0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c   nlb

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

4. Create a pool:

  ```
  ibmcloud is load-balancer-pool-create alb-pool r134-99b5ab45-6357-42db-8b32-5d2c8aa62776  weighted_round_robin tcp 10
  ```
  {: pre}

  Sample output:

  ```
  Creating pool nlb-pool of load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776  under account IBM Cloud Network Services as user test@ibm.com...

  ID                         r134-3b66d605-6aa5-4166-9f66-b16054da3cb0
  Name                       alb-pool
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

5. Create a member:

  ```
  ibmcloud is load-balancer-pool-member-create r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 r134-3b66d605-6aa5-4166-9f66-b16054da3cb0 9090 0716_6acdd058-4607-4463-af08-d4999d983945 --weight 70
  ```
  {: pre}

  Sample output:
  ```
  Creating member of pool r134-3b66d605-6aa5-4166-9f66-b16054da3cb0 under account IBM Cloud Network Services as user test@ibm.com...

  ID                 r134-61f8b000-a90d-4abe-909e-c507dffec565
  Port               9090
  Target             0716_6acdd058-4607-4463-af08-d4999d983945
  Weight             70
  Health             unknown
  Created            2020-08-27T14:59:55.446-05:00
  Provision status   create_pending
  ```
  {: screen}

6. Create a listener:

  ```
  ibmcloud is load-balancer-listener-create r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 7070 tcp --default-pool r134-3b66d605-6aa5-4166-9f66-b16054da3cb0
  ```
  {: pre}

  Sample output:
  ```
  Creating listener of load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...

  ID                     r134-2847a948-f9b6-4fc1-91c6-f1c49dac3eba
  Certificate instance   -
  Connection limit       -
  Port                   7070
  Protocol               tcp
  Default pool           r134-3b66d605-6aa5-4166-9f66-b16054da3cb0
  Provision status       create_pending
  Created                2020-08-27T15:16:08.643-05:00
  ```
  {: screen}

7. Get details about your load balancer:

  ```
  ibmcloud is load-balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776
  ```
  {: pre}

  Sample output:

  ```
  Getting load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...

  ID                 r134-99b5ab45-6357-42db-8b32-5d2c8aa62776
  Name               nlb-test
  CRN                crn:v1:public:is:us-south-1:a/6266f0fbb7df487d8438b9b31d24cd96::load-balancer:r134-99b5ab45-6357-42db-8b32-5d2c8aa62776
  Family             Network
  Host name          99b5ab45-us-south.lb.test.appdomain.cloud
  Subnets            ID                                          Name
                     0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c   nlb

  Public IPs         150.238.50.78, 150.238.54.95
  Private IPs        10.240.0.58, 10.240.0.59
  Provision status   active
  Operating status   online
  Is public          true
  Listeners          r134-2847a948-f9b6-4fc1-91c6-f1c49dac3eba
  Pools              ID                                          Name
                     r134-3b66d605-6aa5-4166-9f66-b16054da3cb0   nlb-pool

  Resource group     ID                                 Name
                     3021f90279574ce287dd5fba82c08899   Default

  Created            2020-08-27T14:34:34.732-05:00
  ```
  {: screen}

## Creating an application load balancer using the API
{: #nlb-api-creating-network-load-balancer}
{: api}

The following example illustrates using the API to create an applicatrion load balancer in front of two VPC virtual server instances (`192.168.100.5` and `192.168.100.6`) running a web application that listens on port 80. The load balancer has a front-end listener, which allows secure access to the web application by using HTTPS.

The example skips the [prerequisite steps](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis) for using the API to provision a VPC, subnets, and instances.
{: note}

To create an application load balancer by using the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

2. Store the following values in variables to be used in the API command:

   * `ResourceGroupId` - First, get your resource group and then populate the variable:

    ```bash
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

3. Create a load balancer with a listener, pool, and attached server instances (pool members)

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
                      "crn": "crn:v1:staging:public:cloudcerts:us-south:a/123456:b8877ea4-b8eg-467e-912a-da1eb7f031cg:certificate:43219c4c97d013fb2a95b21dddde1234"
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

  ```
  {
      "created_at": "2018-07-12T23:17:07.5985381Z",
      "crn": "crn:v1:staging:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
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
  {: screen}

  Save the ID of the load balancer to use in the next steps. For example, save it in the variable `lbid`.

  ```bash
  lbid=0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727
  ```
  {: pre}

4. Get details about the load balancer

  ```bash
  curl -H "Authorization: $iam_token" -X GET "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
  ```
  {: codeblock}

  Allow some time for provisioning. When the load balancer is ready, it is set to `online` and `active` status, as shown in the following sample output:

  ```
  {
    "id": "0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "crn": "crn:v1:staging:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
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
  }
  ```
  {: screen}
