---

copyright:
  years: 2021, 2025
lastupdated: "2025-07-28"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an application load balancer
{: #load-balancers}

You can create an {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) to distribute inbound traffic across multiple instances. IBM supports virtual server instances, bare metal server instances, and other devices that are reachable to the application load balancer with a device IP address, such as Power Systems™ Virtual Server instances connected over {{site.data.keyword.cloud_notm}} Direct Link.
{: shortdesc}

## Creating an application load balancer in the console
{: #lb-ui-creating-application-load-balancer}
{: ui}

To create an ALB:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. On the Load balancers page, click **Create +**.
1. For Load balancer type, select the Applicatoin Load Balancer (ALB) tile.
1. In the Location section, edit the following fields, if necessary.
   * **Geography**: Indicates the geography where you want the load balancer created.
   * **Region**: Indicates the region where you want the load balancer created.
1. In the Details section, complete the following information:
   * **Name**: Enter a name for the load balancer, such as `my-load-balancer`.
   * **Resource group**: Select a resource group for the load balancer.
   * **Tags**: (Optional) Add tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Access management tags**: (Optional) Add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * Select the **Application Load Balancer (ALB)** tile.
   * **Virtual private cloud**: Select your VPC.
   * **Type**: Select the load balancer type.
     * A public load balancer has a public IP address, which means that it can route requests from clients over the internet.
     * A private load balancer has a private IP address, which means that it is accessible only to internal clients on your private subnets, within the same region and VPC.
   * For the DNS type, select either **Public** or **Private**. Private DNS zones are resolvable only on IBM Cloud, and only from explicitly permitted networks in an account or with cross-account access.

      **For Private type only**, click Bind+ to enter your DNS instance and zone information, then click **Bind**.

   * **Subnets**: Select the subnets in which to create your load balancer. To maximize the availability of your application, select subnets in different zones.

        You cannot assign more than 15 subnets per ALB.
        {: important}

1. In the Back-end pools section, click **Create pool** and specify the following information to create a back-end pool. You can create one or more pools.
   * **Name**: Enter a name for the pool, such as `my-pool`.
   * **Protocol**: Select the protocol for your instances in this pool. The protocol of the pool must match the protocol of its associated listener. For example, if an HTTPS or HTTP protocol is selected for the listener, the protocol of the pool must be HTTP. Similarly, if the listener protocol is TCP, the protocol of the pool must be TCP.
   * **Session stickiness**: Select whether all requests during a user's session are sent to the same instance.
   * **Method**: Select how you want the load balancer to distribute traffic across the instances in the pool:
       * **Round robin:** Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
       * **Weighted round robin:** Forward requests to each instance in proportion to its assigned weight. For example, you have instances A, B, and C, and their weights are set to `60`, `60` and `30`. Instances A and B receive an equal number of connections, and instance C receives half as many connections.
       * **Least connections:** Forward requests to the instance with the least number of connections at the current time.
   * **Health check**: Configure how the load balancer checks the health of the instances.
       * **Health check path**: The health check path is applicable only if HTTP is selected as the health check protocol. The health check path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (`/`).
       * **Health protocol**: The protocol used by the load balancer to send health check messages to the instances in the pool.
       * **Health port**: The port on which to send health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
       * **Interval**: Interval in seconds between two consecutive health check attempts. By default, health checks are sent every 5 seconds.
       * **Timeout (sec)**: Maximum amount of time the system waits for a response from a health check request. By default, the load balancer waits 2 seconds for a response.
       * **Max retries**: Maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

       Although the load balancer stops sending connections to unhealthy instances, the load balancer continues monitoring the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

       If instances in the pool are unhealthy and you believe that your application is running fine, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
       {: tip}

1. Click **Create** to create the back-end pool.

   You can attach server instances after you create your back-end pool.

1. To add a server instance to the new pool, click **Attach server** in the **Server instances** column of the table.

   * To add VPC devices to your pool, such as virtual server instances and Bare Metal servers, select the **VPC devices** tab. Specify the following information for each instance:
      * Select one or more subnets from which to select an instance.
      * Select an instance. If an instance has multiple interfaces, make sure that you select the correct IP address.
      * Specify the port on which traffic is sent to the instance.
      * If your pool uses the **Weighted round robin** method, assign a weight for each instance.

         Assigning `0` weight to an instance means that no new connections are forwarded to that instance, but any existing traffic continues to flow while the current connection is active. Using a weight of `0` can help bring down an instance gracefully and remove it from service rotation.
         {: tip}

      * Click **Configure port and weight**, then specify the port on which traffic is sent to the instance.

   * To attach other server instances to your back-end pool, such as servers contained within an IBM Power Systems Virtual Server, select the **Other** tab, then click **Add more**. Specify the following information for each instance:

      * Specify a private IP address for the device.
      * Specify the port on which traffic is sent to the instance.
      * If your pool uses the **Weighted round robin** method, assign a weight for each instance.

         Assigning `0` weight to an instance means that no new connections are forwarded to that instance, but any existing traffic continues to flow while the current connection is active. Using a weight of `0` can help bring down an instance gracefully and remove it from service rotation.
         {: tip}

   * Click **Attach** to attach the server instance to your back-end pool.

1. In the Front-end listeners section, click **Create listener** and specify the following information to create a listener. You can create one or more listeners.

    * **Protocol**: The protocol to use for receiving incoming requests.
    * **Proxy protocol**: Select whether to allow the front-end listener to accept proxy protocol traffic.
    * **Port**: The listening port on which requests are received.
    * **Back-end pool**: The default back-end pool to which this listener forwards traffic.
    * **Max connections** (optional): Maximum number of concurrent connections the listener allows.
    * **IAM Authorization**: If HTTPS is the selected protocol for this listener, you must designate your IAM authorization, either by instance or your CRN.
    * **Secrets Manager**: If HTTPS is the selected protocol for this listener, you must select or create a secrets manager.
    * **SSL certificate**: If HTTPS is the selected protocol for this listener, you must select an SSL certificate. Make sure that the load balancer is authorized to access the SSL certificate.
    * **Timeout (sec)** (optional): The maximum timeout after which the load balancer closes the connection if no data has been sent or received by the time that the idle timeout period elapses. The minimum and maximum timeout values are 50 seconds and 2 hours. These values are for both the Client and Server. To increase the timeout limit to more than 2 hours, [Create a support case](/docs/account?topic=account-open-case&interface=ui) providing the business requirement for the timeout value required. 

1. Click **Create** to create the front-end listener.
1. In the Security groups section, select the security groups that you want to attach to your load balancer, or click **Create** to create a new security group to attach to your ALB.

   Ensure that the security group allows for load-balancing traffic (listener, back-end, and health check ports). If you do not specify a security group, the default security group from your VPC attaches instead.
   {: note}

1. After you finish creating pools and listeners, click **Create load balancer**.
1. To view details of an existing load balancer, click the name of your load balancer on the **Load balancers** page.
1. Optionally, you can create a backup for any of your existing pools. This allows the backup pool to manage traffic in the case of a member failure. To do so, you will need to create a failsafe policy:

   There should be at least one pool already existing in the load balancer.
   {: note}

   * After the status of your load balancer changes to **Active**, select the **Back-end pools** tab.
   * In the pools list page, click **Edit**, then specify the following information:
      * **Action**: Select **forward** in order to create a backup pool. This makes the **Target** section active.
      * **Target**: Select a pool from the list of compatible pools to create your backup pool.
1. If you want to redirect the traffic from an HTTP listener to an HTTPS listener, you can create an HTTP listener with HTTPS redirect settings.

    Layer 7 load-balancing policies overwrite settings that you define here.
    {: note}

    To do so:

    There must be an existing HTTPS listener before you create a new HTTP listener with HTTPS redirect.
    {: note}

    *  After the status of the load balancer changes to **Active**, click the **Front-end listeners** tab.
    *  In the listener list page, click **Create**, then specify the following information:
        * **Protocol**: Select your **HTTP** protocol.
        * **Port**: Choose the listening port on which requests are received.
        * **Max connections** (optional): Define the maximum number of concurrent connections that the listener allows.
        * **HTTPS redirect**: Click the toggle button to enable the HTTPS redirect configuration, then specify the following HTTPS redirect settings:
            * **HTTPS listener**: The target HTTPS listener, which the current HTTP listener incoming traffic is redirected to. Note that you will only see a list of HTTPS listeners whose `accept_proxy_proxy` value is the same as the HTTP listener.
            * **Redirect URI** (optional): The URL to which the request redirects.
            * **Status code**: The status code of the response returned by the load balancer.
1. If you want to redirect, forward, or reject particular incoming traffic for an HTTP or HTTPS front-end listener based on certain criteria, configure layer 7 policies.
    *  After the status of the load balancer changes to **Active**, click **Front-end listeners** in the navigation and click the value in the **Policies** column for the listener you created.
    *  On the Policies page, click **Add policy** and specify the following information to create a policy. You can create multiple policies.
        * **Name**: Enter a name for the policy, such as `my-policy`. The name must be unique within the listener.
        * **Action**: The action to take when all the rules for the policy match. You can reject a request with a 403 response, redirect the request to a configured URL and response code, redirect the traffic from an HTTP listener to an HTTPS listener, or forward the request to a specific back-end pool. If an incoming request does not match the rules for any policies, the request is forwarded to the default back-end pool of the listener.
        * **Priority**: Within each action type, policies are evaluated in ascending order of priority. Policies to reject traffic are always evaluated first, regardless of their priority. Policies to redirect traffic are evaluated next, followed by policies to forward traffic.
        * **Redirect**: The URL to which the request is redirected, if the action is set to **Redirect**. You must provide either a full URL or the parameters of a URI. When using a URL, all incoming traffic will redirect to this URL. When using URI parameters, values from the incoming traffic request can be retained by using the incoming values of the parameters. This includes the protocol, port, host, path, and query. The default values of URI parameters will be equal to their original incoming values. To retain the incoming values, provide them as `{protocol}`, `{port}`, `{host}`, `{path}`, and `{query}`. For example, if the host of the incoming request is `ibm.com`, then the default value will be `{host}` equal to the incoming `ibm.com` value.
        * **Status Code**: The status code of the response returned by the load balancer, if the action is set to **Redirect**.
        * **Forward**: The back-end pool of virtual server instances to which the request is forwarded, if the action is set to **Forward to pool**.
    * On the Policies page, you can also create an HTTPS redirect policy with the following configuration:
        * **Name**: Enter a name for the policy, such as `my-policy`. The name must be unique within the listener.
        * **Action**: Select the **Redirect to HTTPS** option.
        * **HTTPS listener**: The target HTTPS listener which the traffic from the current HTTP listener will redirect to. Note that you will only see a list of HTTPS listeners whose `accept_proxy_proxy` value is the same as the HTTP listener.
        * **Redirect URI** (optional): The URL to which the request redirects.
        * **Status code**: The status code of the response returned by the load balancer.
    * On the Policies page, click **Add rule** for your policy. If rules exist for the policy, click the value in the **Rules** column to add more rules.
    * In the Rules window, click **Add rule** and specify the following information to create a rule. If you create multiple rules for a policy, the policy is applied only when all its rules are matched.
        * **Condition**: Specifies the condition with which a rule is evaluated.
        * **Type**: The type of information to be evaluated by the rule: the name of the host from which the request originated, an HTTP header field, or a path in the URL.
        * **Value**: The value to be matched.
        * **Key**: The name of the HTTP header field to evaluate, if the rule type is **Header**. For example, to match a cookie in the HTTP header, enter **Cookie** for the key.

## Creating an application load balancer from the CLI
{: #lb-cli-creating-application-load-balancer}
{: cli}

The following example illustrates using the CLI to create an {{site.data.keyword.alb_full}} (ALB). In this example, it is in front of one VPC virtual server instance (ID `0716_6acdd058-4607-4463-af08-d4999d983945`) running a TCP server that listens on port 9090. The load balancer has a front-end listener, which allows secure access to the TCP server.

To create an application load balancer from the CLI, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Use the terminal to log in to your account using the CLI. After you enter the password, the system prompts which account and region you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Create a load balancer:

    ```sh
    ibmcloud is load-balancer-create alb-test public --subnet 0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c --family application
    ```
    {: pre}

    Sample output:

    ```sh
    Creating load balancer nlb-test in resource group under account IBM Cloud Network Services as user test@ibm.com...

    ID                 r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
    Name               alb-test
    CRN                crn:v1:public:is:us-south-1:a/123456::load-balancer:r006-99b5ab45-6357-42db-8b32-5d2c8aa62776
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

1. Create a pool:

    ```sh
    ibmcloud is load-balancer-pool-create alb-pool r006-99b5ab45-6357-42db-8b32-5d2c8aa62776  weighted_round_robin tcp 10  --failsafe-policy-action forward --failsafe-policy-target pool2
    ```
    {: pre}

    Sample output:

    ```sh
    Creating pool nlb-pool of load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776  under account IBM Cloud Network Services as user test@ibm.com...

    ID                         r006-3b66d605-6aa5-4166-9f66-b16054da3cb0
    Name                       alb-pool
    Protocol                   tcp
    Algorithm                  weighted_round_robin
    Instance group             ID   Name
                               -    -

    Health monitor             Type   Port   Health monitor URL   Delay   Retries   Timeout
                               http   8080   /                    10      2         5

    Failsafe policy            Action    Target ID                                   Target name   Healthy Member Threshold Count
                               forward   r006-815e16e7-8729-4d9e-9203-936a6b615ee1   pool2         0

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
    $ibmcloud is load-balancer-listener-create r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 7070 tcp --default-pool r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 -idle-connection-timeout 30
    ```
    {: pre}

    Sample output:

    ```sh
    $ibmcloud is load-balancer-listener-create r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 7070 tcp --default-pool r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 -idle-connection-timeout 30

    Sample output:
    Creating listener of load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...
    ID                      r006-2847a948-f9b6-4fc1-91c6-f1c49dac3eba
    Certificate instance    -
    Connection limit        -
    Idle connection timeout 30
    Port                    7070
    Protocol                tcp
    Default pool            r006-3b66d605-6aa5-4166-9f66-b16054da3cb0
    Provision status        create_pending
    Created                 2020-08-27T15:16:08.643-05:00
    ```
    {: screen}

    For more options, see [VPC CLI reference for load balancers](/docs/vpc?topic=vpc-vpc-reference#lb-anchor). 

1. Create a policy:

    ```sh
    ibmcloud is load-balancer-listener-policy-create 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 --action redirect --priority 2 --target-http-status-code 301 --target-url "https://{host}:443/{path}"
    ```
    {: pre}

    Sample output:

    ```sh
    Creating policy of load balancer listener 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 under account IBM Cloud Network Services as user test@ibm.com...

    ID                      r006-4847a949-f9b6-4fc1-71c6-d1c49dac3ebc
    Action                  redirect
    Priority                2
    Http status code        301
    Target Url              https://{host}:443/{path}
    Provision status        create_pending
    Created                 2024-04-23T15:16:08.643-05:00
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

## Creating an application load balancer with the API
{: #lb-api-creating-application-load-balancer}
{: api}

The following example illustrates using the API to create an application load balancer in front of two VPC virtual server instances (`192.168.100.5` and `192.168.100.6`) running a web application that listens on port `80`. The load balancer has a front-end listener, which allows secure access to the web application by using HTTPS.

The example skips the [prerequisite steps](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=api) for using the API to provision a VPC, subnets, and instances.
{: note}

To create an application load balancer with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

1. Store the following values in variables to be used in the API command:

    * `ResourceGroupId` - First, get your resource group and then populate the variable:

    ```bash
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

    You can also find your resource group ID by using IBM Cloud console. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account. Select **Manage** > **Account** > **Resource Groups**. 

1. Create a load balancer with a listener, pool, and attached server instances (pool members) with the following sample code:

    ```bash
    curl -H "Authorization: $iam_token" -X POST
    "$vpc_api_endpoint/v1/load_balancers?version=$api_version&generation=2" \
      -d @alb_create_payload.json
      Where alb_create_payload.json has the following content:
      '{
          "name": "example-balancer",
          "is_public": true,
          "listeners": [
              {
                  "certificate_instance": {
                      "crn": "crn:v1:bluemix:public:cloudcerts:us-south:a/123456:b8877ea4-b8eg-467e-912a-da1eb7f031cg:certificate:43219c4c97d013fb2a95b21dddde1234"
                  },
                  "port": 443,
                  "protocol": "tcp",
                  "idle_connection_timeout" : 80,
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

    For more options, see [VPC API reference for load balancers](https://cloud.ibm.com/apidocs/vpc/latest#list-load-balancer-profiles). 

    Sample output:

    ```sh
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
    {: screen}

    Save the ID of the load balancer to use in the next steps. For example, save it in the variable `lbid`.

      ```bash
      lbid=0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727
      ```
      {: pre}

1. Get details about the load balancer

    ```bash
    curl -H "Authorization: $iam_token" -X GET "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
    ```
    {: codeblock}

    Allow some time for provisioning. When the load balancer is ready, it is set to `online` and `active` status, as shown in the following sample output:

    ```sh
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
    }
    ```
    {: screen}
