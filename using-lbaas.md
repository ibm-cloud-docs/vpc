---


copyright:
  years: 2018, 2020
lastupdated: "2020-02-14"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, layer-7

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Using load balancers 
{: #load-balancers}

Use the {{site.data.keyword.cloud}} load balancer for VPC service to distribute traffic among multiple server instances within the same region of your VPC.
{:shortdesc}

## Beta participants
{: #beta-participants}
14 February 2020: This service is now generally available. You must change to a standard plan to continue using instances that you created during the beta. Any instances that continue to use the beta plan for this service beyond 30 days notice of general availability will be subject to deletion.

## Types of load balancers
{: #types-load-balancer}

You can create a public or private load balancer. Table 1 shows a summary comparison of features.

| Feature | Public Load Balancer | Private Load Balancer |
|--------|-------|-------|
| Accessible on internet? |  Yes, with a fully qualified domain name (FQDN) | No, internal clients only, on same region and VPC |
| Accepts all traffic? | Yes | No, RFC 1918 traffic only |
| How is domain name registered? | Public IP addresses | Private IP addresses |
{: caption="Table 1. Comparison of public and private load balancers" caption-side="top"}

### Public load balancer
{: #public-load-balancer}

A public load balancer service instance is assigned a publicly accessible FQDN, which you must use for access to your applications that are hosted behind the load balancer. This domain name can be registered with one or more public IP addresses.

Over time, the number and value of these public IP addresses might change due to maintenance and scaling activities. The back-end virtual server instances (VSIs) hosting your application must run in the same region, and under the same VPC.

Use the assigned FQDN to send traffic to the public load balancer to avoid connectivity problems to your applications during system maintenance or scaling down activities.
{: important}

### Private load balancer
{: #private-load-balancer}

The private load balancer is accessible only to internal clients on your private subnets, within the same region and VPC. The private load balancer accepts traffic only from [RFC1918](https://tools.ietf.org/html/rfc1918){: external} address spaces.

Similar to a public load balancer, your private load balancer service instance is assigned a fully qualified domain name (FQDN). However, this domain name is registered with one or more private IP addresses.

{{site.data.keyword.cloud_notm}} operations might change the number and value of your assigned private IP addresses over time, based on maintenance and scaling activities. The back-end virtual server instances (VSIs) hosting your application must run in the same region, and under the same VPC.

Use the assigned FQDN to send traffic to the private load balancer to avoid connectivity problems to your applications during system maintenance or scaling down activities.
{: important}

## Front-end listeners and back-end pools
{: #front-end-listeners-and-back-end-pools}

You can define up to 10 front-end listeners (application ports) and map them to back-end pools on the back-end application servers. The fully qualified domain name (FQDN) assigned to your load balancer and the front-end listener ports are exposed to the public internet. Incoming user requests are received on these ports.

The supported front-end listener protocols are:
* HTTP
* HTTPS
* TCP

The supported back-end pool protocols are:
* HTTP
* TCP

Incoming HTTPS traffic is terminated at the load balancer to allow for plain-text HTTP communication with the back-end server.

You can attach up to 50 virtual server instances to a back-end pool. Traffic is sent to each instance on its specified data port. This data port doesn't need to be the same as the front-end listener port.

Ports 56500 - 56520 can't be used as front-end listener ports because they are reserved for management purposes.
{: important}

## Layer-7 load balancing
{: #layer-7-load-balancing}

Both public and private load balancers support Layer-7 load balancing. Data traffic is distributed based on configured policies and rules. A _policy_ defines the action to take, which means how the traffic is distributed, when the incoming request matches the rules that are associated with the policy.

### Policies
{: #layer-7-policy}

You can define policies for HTTP and HTTPS listeners. For each policy, you define one or more rules. The policy is applied only when all its rules are matched.

You can attach more than one policy to a listener. In general, a policy with the lowest priority is evaluated first. Each policy must have a different priority.

If the incoming request does not match the rules for any of the policies, the request is redirected to the default pool of the listener.

The following actions are supported for a Layer-7 policy:

* **Reject:** The request is denied with a 403 response.
* **Redirect:** The request is redirected to a configured URL and response code.
* **Forward:** The request is sent to a specific back-end pool.

Policies set to `reject` are evaluated first, regardless of their priority. Next, policies set to `redirect` are evaluated. Finally, policies set to `forward` are evaluated.

Within each action category, the policies are evaluated in ascending order of priority (lowest priority to highest priority). Table 2 defines these policy priorities.

### Policy properties
{: #layer-7-policy-properties}

Property  | Description
------------- | -------------
Name | The name of the policy. The name must be unique within the listener.
Action | The action to take when all policy rules match. The acceptable values are `reject`, `redirect`, and `forward`. A policy for the `reject` action is always evaluated first, regardless of its priority. Policies with `redirect` actions are evaluated next, followed by policies with `forward` actions.
Priority | Policies are evaluated based on ascending order of priority.
URL | The URL to which the request is redirected, if the action is set to `redirect`.
HTTP Status Code | Status code of the response returned by the load balancer when the action is set to `redirect`. The acceptable values are: 301, 302, 303, 307 or 308.
Target | The back-end pool of virtual server instances to which the request is forwarded, if the action is set to `forward`.
{: caption="Table 2. Description of policy properties" caption-side="top"}

### Rules
{: #layer-7-rules}

A Layer-7 rule defines how a request should be matched. Three types of rules are supported, described in Table 3.

Type      |  Description
----------| -----------------------
`hostname` | The request matches the specified hostname, such as `api.my_company.com`.
`header`    | The request matches an HTTP header field, such as `Cookie`.
`path`      | The request matches a path in the URL, such as `/index.html`.
{: caption="Table 3. Layer-7 rules" caption-side="top"}

To match a request, a `condition` statement must be defined in a rule. Three conditions are supported, described in Table 4.

Condition |  Type of evaluation
----------------|---------------------
`contains`        |  Verify whether the extracted field contains the specified string.
`equals`        |  Verify whether the extracted field is identical to the specified string.
`matches_regex`           |  Match the extracted field with the specified regular expression.
{: caption="Table 4. Condition statements defined in a rule" caption-side="top"}

### Rule properties
{: #layer-7-rule-properties}

Table 5 describes Layer-7 policy rule properties.

Property  | Description
------------- | -------------
Type | Specifies the type of rule. The acceptable values are `hostname`, `header`, or `path`.
Condition | Specifies the condition with which a rule is evaluated. Condition can be: `contains`, `equals`, or `matches_regex`.
Field | Specifies the HTTP header field name. This field is applicable only to the `header` rule type. For example, to match a cookie in the HTTP header, the field can be set to `Cookie`.
Value |  The value to be matched.
{: caption="Table 5. Descriptions of rule properties" caption-side="top"}

The following characters are not allowed for `field` or `value` if the rule type is `header`: `"(),/:;<=>?@[\]{}'`.
{: tip}

## Load balancing methods
{: #load-balancing-methods}

Three load balancing methods are available for distributing traffic across the back-end application servers:

* **Round-robin:** Round-robin is the default load balancing method. With this method, the load balancer forwards incoming client connections in round-robin fashion to the back-end servers. As a result, all back-end servers receive roughly an equal number of client connections.

* **Weighted Round-robin:** With this method, the load balancer forwards incoming client connections to the back-end servers in proportion to the weight assigned to these servers. Each server is assigned a default weight of 50, which can be customized to any value in the range 0 - 100.

  For example, if application servers A, B and C have the weights 60, 60, and 30, then servers A and B receive an equal number of connections, while server C receives half that number of connections.

  Setting a server weight to '0' means that no new connections are forwarded to that server, but any existing traffic continues to flow. Using a weight of '0' can help bring down a server gracefully and remove it from service rotation. 
  {: tip}
  
  The server weight values are applicable only with the weighted round-robin method. They are ignored with round-robin and least-connections load balancing methods.

* **Least connections:** With this method, the back-end server instance that serves the least number of connections at a given time receives the next client connection.

## Horizontal scaling
{: #horizontal-scaling}

The load balancer adjusts its capacity automatically according to the load. When this adjustment occurs, you may see a change in the number of IP addresses associated with the load balancer's DNS name.

## Health checks
{: #health-checks}

Health check definitions are mandatory for back-end pools. Health checks can be configured on back-end ports or on a separate health check port, based on the application. 

The load balancer conducts periodic health checks to monitor the health of the back-end ports, and it forwards client traffic to them accordingly. If a back-end server port is found to be unhealthy, no new connections are forwarded to it. The load balancer continues to monitor health of unhealthy ports, and it resumes their use if they become healthy again, which means that they successfully pass two consecutive health check attempts.

The health checks for HTTP and TCP ports are conducted as follows:

* **HTTP:** An `HTTP GET` request against a pre-specified URL is sent to the back-end server port. The server port is marked healthy upon receiving a `200 OK` response. The default `GET` health path is "/" through the UI, and it can be customized.

* **TCP:** The load balancer attempts to open a TCP connection with the back-end server on the specified TCP port. The server port is marked healthy if the connection attempt is successful, and the connection is closed.

By default, health checks are sent every five seconds on the same port on which traffic is sent to the instance. By default, the load balancer waits two seconds for a response to the health check, and an instance is no longer considered healthy after two failed health checks.

## SSL offloading and required authorizations
{: #ssl-offloading-and-required-authorizations}

For all incoming HTTPS connections, the load balancer service terminates the SSL connection and establishes a plain-text HTTP communication with the back-end server instance. With this technique, CPU-intensive SSL handshakes and encryption or decryption tasks are shifted away from the back-end server instances, allowing them to use all their CPU cycles for processing application traffic.

An SSL certificate is required for the load balancer to perform SSL offloading tasks. You can manage the SSL certificates through [IBM Certificate Manager](/docs/services/certificate-manager?topic=certificate-manager-getting-started).

To give a load balancer access to your SSL certificate, you must enable **service-to-service authorization**, which grants your load balancer service instance access to your certificate manager instance. For more information, see [Granting access between services](/docs/iam?topic=iam-serviceauth#create-auth). Make sure to choose **VPC Infrastructure** as the source service, **Load Balancer for VPC** as the resource type, **Certificate Manager** as the target service, and assign the **Writer** service access role.

If the required authorization is removed, errors might occur for your load balancer.
{: important}

## MZR support
{: #mzr-support}

Load Balancer for VPC supports Multi-Zone-Regions (MZRs). High availability and redundancy can be achieved by deploying a load balancer with subnets from different zones. When subnets from multiple zones are used to provision a load balancer, the load balancer appliances get deployed to multiple zones. 

## Advanced traffic management
{: #advanced-traffic-management}

The following advanced traffic management features are available.

## Max connections
{: #max-connections}

Use the ‘max connections’ configuration to limit the maximum number of concurrent connections for a given front-end virtual port. By default, no limit is specified. The maximum possible concurrent connections for a given front-end virtual port or system-wide across all front-end virtual ports is `15000`.

## Session persistence
{: #session-persistence}

The load balancer supports session persistence based upon the source IP of the connection. As an example, if you have `source IP` type session persistence enabled for port 80 (HTTP), then subsequent HTTP connection attempts from the same source IP client are persistent on the same back-end server. This feature is available for all three supported protocols (HTTP, HTTPS and TCP).

## HTTP keep alive
{: #http-keep-alive}

The load balancer supports `HTTP keep alive` as long as it is enabled on both the client and back-end servers. The load balancer attempts to re-use the server-side HTTP connections to increase connection efficiency and reduce latency.

## Connection timeouts
{: #connection-timeouts}

The following timeout values are used by the load balancer. You cannot customize these values at this time.

| Name | Description | Timeout |                                                                              
| ------------------------------------------ | --------------------------------------------------- | ------------------- |
| Server-side connection attempt    | The maximum time window that the load balancer can use to establish TCP connection with the back-end server. If the connection attempt is unsuccessful, the load balancer tries the next available server, according to the load-balancing method configured. | 5 seconds   |
| Client-side idle connection  | The maximum idle time, after which the load balancer brings down the client-side connection, if the client has failed to close its connection properly.| 50 seconds  |
| Server-side idle connection | The maximum idle time (with back-end protocol configuration of TCP), after which the load balancer closes the server-side connection. With the back-end protocol configuration of HTTP, if the load balancer fails to receive a response to its HTTP request within the idle timeout window, it will return an error message to the end-client.                                | 50 seconds |
{: caption="Table 1. Load Balancer Timeout Values" caption-side="top"}

## Preserving end-client IP address
{: #preserving-end-client-ip-address}

Load Balancer for VPC works as a reverse proxy, which terminates incoming traffic from the client. The load balancer establishes a separate connection to the back-end server instance, using its own IP address. For HTTP connections with the backend servers (against front-end HTTP or HTTPS connections), the load balancer preserves the original client IP address by including it inside the `X-Forwarded-For HTTP` header. For TCP connections, the original client IP information is not preserved.

## Identity and access management
{: #identity-and-access-management-iam}

You can configure access policies for a **Load Balancer for VPC** instance. For more information about managing user access policies, see [Managing access to resources](/docs/services/iam?topic=iam-iammanidaccser#resourceaccess).

### Configuring resource group access policies for users
{: #configuring-resource-group-access-policies-for-users}

To create a load balancer, you need access to a resource group. The user who is creating a load balancer must have proper access to the resource group provided, or to the default resource group, if none is provided.

1. Go to **Manage > Account > Users** to see a list of the users with access to your IBM Cloud account.
2. Select the name of the user to whom you want to assign an access policy. If the user is not shown, click **Invite users** to add the user to your IBM Cloud account.
3. Select **Assign access**.
4. Select **Assign access within a resource group**.
5. From the **Resource group** list, select a resource group.
6. From the **Assign access to a resource group** list, select an access role.
7. Select **Assign** to assign the resource group access policy to the user.

### Configuring resource access policies for users
{: #configuring-resource-access-policies-for-users}

Table 6 provides information for configuring resource access policies for users.

| Platform Access Role | Load Balancer Action |
|-------------|-----|
| Administrator | Create/View/Edit/Delete load balancer |
| Editor | Create/View/Edit/Delete load balancer |
| Viewer | View load balancer |
{: caption="Table 6. Platform-access roles and load balancer actions" caption-side="top"}

1. Go to **Manage > Account > Users**.
2. Select the name of the user to whom you want to assign an access policy. If the user is not shown, click **Invite users** to add the user to your IBM Cloud account.
3. Select **Assign access**.
4. Select **Assign access to resources**.
5. From the **Services** list, select **VPC Infrastructure**.
6. From the **Resource type** list, select **Load Balancer for VPC**.
7. From the **Load Balancer ID** list, select a Load Balancer instance ID, or use the default value, **All load balancers**.
8. Assign a platform access role to the user.
9. Select **Assign** to assign the access policy to the user.

## Activity tracker integration
{: #activity-tracker-integration}

The load balancer service is integrated with IBM Cloud Activity Tracker with LogDNA, which records events in a manner compliant with the CADF standard, as triggered by user-initiated activities that change the state of the service in the cloud.

For a list of actions that are recorded as auditing events on the load balancer service instances, see [Activity tracker with LogDNA events](/docs/vpc?topic=vpc-at-events#events-load-balancers).

All auditing events are recorded to IBM Cloud Activity Tracker with LogDNA in the `us-south` region, regardless in which region the load balancer service is provisioned.

To view events, you must provision an IBM Cloud Activity Tracker with LogDNA instance in the`us-south` region under your account. Users in your account must have an IAM policy that grants the **Viewer** platform access role and **Reader** service access role on the IBM Cloud Activity Tracker with LogDNA instance.

For more information about granting access, see [Granting user permissions to a user or service ID](/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-iam_view_events).

IBM Cloud account users can monitor account-level operations that are performed on the load balancer service.
{: tip}

To provision an IBM Cloud Activity Tracker with LogDNA instance:
1. Log in to the IBM Cloud console. [Log in to IBM Cloud console](https://{DomainName}/){: external}.
2. Click the ![Menu icon](../../icons/icon_hamburger.svg) and select **Observability > Activity Tracker**.
3. Click **Create Instance**.
4. Define a service name.
5. Select `us-south` as the region and choose the resource group
6. Choose a plan other than `lite` if you have a paid account.
7. Click **Create**.

For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

Here is the sample activity tracker message for a **Create Listener** operation:
```bash
{
    "logSourceCRN": "crn:v1:staging:public:is:eu-gb:a/<ACCOUNT_ID>::load-balancer:4633518f-8aac-48a1-a694-d15ee6bd70e3",
    "meta": {
        "serviceProviderName": "is-load-balancer",
        "serviceProviderProjectId": "48a7a7b7-6642-4aa1-8af9-c1be4ef82050",
        "serviceProviderRegion": "ng",
        "userAccountIds": [
            <ACCOUNT_ID>
        ]
    },
    "payload": {
        "action": "is.load-balancer.load-balancer.listener.create",
        "eventTime": "2019-05-30T18:42:48.96+0000",
        "eventType": "activity",
        "id": "e4ee1906d01a35efe8bd8303ce0a734e",
        "initiator": {
            "credential": {
                "type": "token"
            },
            "host": {
                "address": <CLIENT_IP>,
                "agent": "python-requests/2.19.1"
            },
            "id": <USER-ID>,
            "name": <USER_ID>,
            "project_id": <ACCOUNT_ID>,
            "typeURI": "service/security/account/user"
        },
        "message": "is.load-balancer: create listener 4633518f-8aac-48a1-a694-d15ee6bd70e3 success",
        "observer": {
            "id": "activity-tracker.cloud.ibm.com",
            "name": "ActivityTracker",
            "typeURI": "security/edge/activity-tracker"
        },
        "outcome": "success",
        "reason": {
            "reasonCode": 201,
            "reasonType": "https://www.iana.org/assignments/http-status-codes/http-status-codes.xml"
        },
        "requestData": "{\"headers\":{\"RayID\":\"4df2d9911a3ac2bd\"},\"extraData\":{\"resourceName\":\"4633518f-8aac-48a1-a694-d15ee6bd70e3\"}}",
        "requestPath": "/v1/load_balancers/4633518f-8aac-48a1-a694-d15ee6bd70e3/listeners",
        "severity": "normal",
        "target": {
            "host": {
                "address": <API_END_POINT>
            },
            "id": "crn:v1:staging:public:is:eu-gb:a/<ACCOUNT_ID>::load-balancer:4633518f-8aac-48a1-a694-d15ee6bd70e3",
            "name": "4633518f-8aac-48a1-a694-d15ee6bd70e3",
            "typeURI": "/v1/load_balancers/4633518f-8aac-48a1-a694-d15ee6bd70e3/listeners"
        },
        "typeURI": "http://schemas.dmtf.org/cloud/audit/1.0/event"
    },
    "saveServiceCopy": true
}
```
## Configuring ACLs for use with load balancers
{: #acls-lb}

If you use access control lists (ACLs) to block traffic on the subnets in which the load balancer is deployed, make sure that ACL rules are in place to allow incoming traffic to the configured listener ports and the following management ports:
- Inbound on TCP port 56501
- Outbound on TCP port 53, 443, 10514, 8834


## APIs available
{: #lbaas-apis-available}

To make API calls, you must use a REST client. For example, you can use a `curl` command to retrieve all existing load balancers:

```
curl -X GET "$api_endpoint/v1/load_balancers?version=$api_version&generation=2" -H "Authorization: $iam_token"
```
{: pre}

Use the APIs described in Table 7 for load balancers in your VPC environment. For more information, see the [VPC API reference](https://{DomainName}/apidocs/vpc#list-all-load-balancers).

| Description | API |
|-------------|-----|
| Create and provision a load balancer | POST /load_balancers |
| Retrieve all load balancers | GET /load_balancers |
| Retrieve a load balancer | GET /load_balancers/{id} |
| Delete a load balancer | DELETE /load_balancers/{id} |
| Update a load balancer | PATCH /load_balancers/{id} |
| Create a listener | POST /load_balancers/{id}/listeners |
| Retrieve all listeners of the load balancer | GET /load_balancers/{id}/listeners |
| Retrieve a listener | GET /load_balancers/{id}/listeners/{listener_id} |
| Delete a listener | DELETE /load_balancers/{id}/listeners/{listener_id} |
| Update a listener | PATCH /load_balancers/{id}/listeners/{listener_id} |
| Create a pool | POST /load_balancers/{id}/pools |
| Retrieve all pools of the load balancer | GET /load_balancers/{id}/pools |
| Retrieve a pool | GET /load_balancers/{id}/pools/{pool_id} |
| Delete a pool | DELETE /load_balancers/{id}/pools/{pool_id} |
| Update a pool | PATCH /load_balancers/{id}/pools/{pool_id} |
| Add a virtual server instance to the pool  | POST /load_balancers/{id}/pools/{pool_id}/members |
| Retrieve all members of the pool | GET /load_balancers/{id}/pools/{pool_id}/members |
| Retrieve a member |GET /load_balancers/{id}/pools/{pool_id}/members/{member_id} |
| Delete a member from the pool | DELETE /load_balancers/{id}/pools/{pool_id}/members/{member_id} |
| Update a member | PATCH /load_balancers/{id}/pools/{pool_id}/members/{member_id} |
| Update members of the pool | PUT /load_balancers/{id}/pools/{pool_id}/members |
| Retrieve statistics of a load balancer | GET /load_balancers/{id}/statistics |
| Retrieve all policies of the listener |  GET /load_balancers/{id}/listeners/{listener_id}/policies
| Create a policy for the listener | POST /load_balancers/{id}/listeners/{listener_id}/policies
| Delete a policy from the listener | DELETE /load_balancers/{id}/listeners/{listener_id}/policies/{id}
| Retrieve a policy of the listener | GET /load_balancers/{id}/listeners/{listener_id}/policies/{id}
| Update a policy of the listener | PATCH /load_balancers/{id}/listeners/{listener_id}/policies/{id}
| Retrieve all the rules associated with a policy | GET /load_balancers/{id}/listeners/{listener_id}/policies/{policy_id}/rules
| Create a rule for the policy | POST /load_balancers/{id}/listeners/{listener_id}/policies/{policy_id}/rules
| Delete a rule from the policy | DELETE /load_balancers/{id}/listeners/{listener_id}/policies/{policy_id}/rules/{rule_id}
| Retrieve a rule from the policy | GET /load_balancers/{id}/listeners/{listener_id}/policies/{policy_id}/rules/{rule_id}
| Update a rule of the policy | PATCH /load_balancers/{id}/listeners/{listener_id}/policies/{policy_id}/rules/{rule_id}
{: caption="Table 7. APIs for load balancers" caption-side="top"}

## Load balancer API example
{: #load-balancer-example}

In the following example, you use the API to create a load balancer in front of two VPC virtual server instances (`192.168.100.5` and `192.168.100.6`) running a web application that listens on port `80`. The load balancer has a front-end listener, which allows secure access to the web application by using HTTPS. You can use the API to get details of the load balancer after it is created, and to delete the load balancer.

The example steps that follow skip the prerequisite steps of using the [IBM Cloud UI](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console), [CLI](/docs/vpc?topic=vpc-creating-a-vpc-using-cli), or [API](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis) to provision a VPC, subnets, and instances.

You can also create a load balancer using the UI or CLI. For instructions, see [Creating a load balancer using the IBM Cloud console](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#load-balancer) or the [CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
{: tip}

### Step 1. Create a load balancer with a listener, pool, and attached server instances (pool members)

```bash
curl -H "Authorization: $iam_token" -X POST
"$api_endpoint/v1/load_balancers?version=$api_version&generation=2" \
    -d '{
        "name": "example-balancer",
        "is_public": true,
        "listeners": [
            {
                "certificate_instance": {
                    "crn": "crn:v1:staging:public:cloudcerts:us-south:a/123456:b8877ea4-b8eg-467e-912a-da1eb7f031cg:certificate:43219c4c97d013fb2a95b21dddde1234"
                },
                "port": 443,
                "protocol": "https",
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
                    "type": "http",
                    "url_path": "/"
                },
                "name": "example-pool",
                "protocol": "http",
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

```
lbid=0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727
```
You can deploy load balancer appliances to multiple subnets. To achieve higher availability and redundancy, deploy the load balancer to subnets in different zones.
{: tip}

### Step 2. Get details about the load balancer

```
curl -H "Authorization: $iam_token" -X GET "$api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
```
{: pre}

Allow some time for provisioning. When the load balancer is ready, it is set to `online` and `active` status, as shown in the following sample output:

Sample output:

```bash
{
  "id": "0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
  "crn": "crn:v1:staging:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
  "name": "example-balancer",
  "created_at": "2018-07-13T22:22:24.489Z",
  "hostname": "dd754295-e9e0-4c9d-bf6c-58fbc59e5727.lb.appdomain.cloud",
  "is_public": true,
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

### Step 3. Delete the load balancer

```bash
curl -H "Authorization: $iam_token" -X DELETE "$api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
```
{: pre}

## Layer-7 examples: Create policy and rules
{: #layer-7-examples-create-policy-and-rules}

The following examples show how policies and rules are created and associated with a listener.

### Example 1: Create an HTTPS listener with redirect policies

```
bash
curl -H "Authorization: $iam_token" -X POST
"$api_endpoint/v1/load_balancers/$lbId/listeners" \
    -d '{
            "certificate_instance": {
                "crn": "crn:v1:staging:public:cloudcerts:us-south:a/1111111111111111111111111111:22222222-3333-4444-5555-666666666666:certificate:77777777777777777777777777777777"
            },
            "connection_limit": 2000,
            "port": 443,
            "protocol": "https",
            "policies": [
                {
                    "name": "hostname_header",
                    "action": "redirect",
                    "priority": 1,
                    "target": {
                        "url": "https://www.examples.com/",
                        "http_status_code": 307
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "header",
                            "field": "aheader",
                            "value": "avalue"
                        },
                        {
                            "condition": "equals",
                            "type": "hostname",
                            "value": "abc.com"
                        }
                    ]
                },
                {
                    "name": "header_cookie",
                    "action": "redirect",
                    "priority": 5,
                    "target": {
                        "url": "https://www.mycookies.com/",
                        "http_status_code": 302
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "header",
                            "field": "aheader",
                            "value": "avalue"
                        },
                        {
                            "condition": "equals",
                            "type": "header",
                            "field": "cookie",
                            "value": "flavor=oatmeal"
                        }
                    ]
                },
                {
                    "name": "path_hostname",
                    "action": "redirect",
                    "priority": 10,
                    "target": {
                        "url": "https://www.myexamples.com/",
                        "http_status_code": 301
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "hostname",
                            "value": "abc"
                        },
                        {
                            "condition": "equals",
                            "value": "/test",
                            "type": "path"
                          }
                    ]
                }
            ]
        }'
```
{: codeblock}

### Example 2: Create policies to forward requests to pools and associate it with an existing listener
```
bash
curl -H "Authorization: $iam_token" -X POST
"$api_endpoint/v1/load_balancers/$lbId/listeners/$listenerId/policies" \
    -d '{
            "policies": [
                {
                    "action": "forward",
                    "priority": 1,
                    "target": {
                        "id": "7df616da-4dd6-43d3-881d-801ae29e29fe"
                    },
                    "rules": [
                        {
                            "condition": "equals",
                            "type": "header",
                            "field": "cookie",
                            "value": "flavor=oatmeal"
                        }
                    ]
                },
                {
                    "action": "forward",
                    "priority": 5,
                    "target": {
                        "id": "0738-8061c411-0d50-4c79-b475-102666796434"
                    },
                    "rules": [
                        {
                            "condition": "contains",
                            "type": "header",
                            "field": "aheader",
                            "value": "avalue"
                        }
                    ]
                },
                {
                    "action": "forward",
                    "priority": 10,
                    "target": {
                        "id": "0738-62914e09-3928-4d89-b7f7-1bb7a6d7fe85"
                    },
                    "rules": [
                        {
                            "condition": "matches_regex",
                            "type": "hostname",
                            "value": "abc[a-z]*.com"
                        }
                    ]
                },
                {
                    "action": "forward",
                    "priority": 6,
                    "target": {
                        "id": "0738-62914e09-3928-4d89-b7f7-1bb7a6d7fe85"
                    },
                    "rules": [
                        {
                            "condition": "equals",
                            "type": "path",
                            "value": "/test/testtest"
                        }
                    ]
                }
            ]
        }'
```
{: codeblock}

## FAQs
{: #load-balancer-faqs}

This section contains answers to some frequently asked questions about the Load Balancer for VPC service.

### Can I use a different DNS name for my load balancer?
{: #can-i-use-a-different-dns-name-for-my-load-balancer}
{: faq}

The auto-assigned DNS name for the load balancer is not customizable. However, you can add a CNAME (Canonical Name) record that points your preferred DNS name to the auto-assigned load balancer DNS name. For example, your load balancer in `us-south` has ID `dd754295-e9e0-4c9d-bf6c-58fbc59e5727`, and the auto-assigned load balancer DNS name is `dd754295-us-south.lb.appdomain.cloud`. Your preferred DNS name is `www.myapp.com`. You can add a CNAME record (through the DNS provider that you use to manage `myapp.com`) pointing `www.myapp.com` to the load balancer DNS name `dd754295-us-south.lb.appdomain.cloud`.

### What's the maximum number of front-end listeners I can define with my load balancer?
{: #what-s-the-maximum-number-of-front-end-listeners-i-can-define-with-my-load-balancer}
{: faq}

10

### What's the maximum number of virtual server instances I can attach to my back-end pool?
{: #what-s-the-maximum-number-of-server-instances-i-can-attach-to-my-back-end-pool}
{: faq}

50

### Is the load balancer horizontally scalable?
{: #is-the-load-balancer-horizontally-scalable}
{: faq}

Yes. The load balancer automatically adjusts its capacity based on the load. When horizontal scaling takes place, the number of IP addresses associated with the load balancer's DNS changes.

### What should I do if I'm using ACLs on the subnets that are used to deploy the load balancer?
{: #what-should-i-do-if-i-am-using-acls-on-the-subnets-that-are-used-to-deploy-the-load-balancer}

Make sure that the proper ACL rules are in place to allow incoming traffic for configured listener ports and management ports. Traffic between the load balancer and back-end instances should also be allowed.

For detailed information on the ACLs configuration required, refer to [Configuring ACLs for use with load balancers](#acls-lb).

### Why am I receiving an error message: `certificate instance not found`?
{: #error-certificate-instance-not-found}
{: faq}

* The certificate instance CRN may not be valid.
* You may not have granted **service-to-service authorization**. See [SSL offloading](#ssl-offloading-and-required-authorizations).

### Why am I receiving a `401 Unauthorized Error` code?
{: #401-unauthorized-error}
{: faq}

Check the following access policies for your user:
* The access policy for the load balancer resource type
* The access policy for the resource group
* If `HTTPS` listeners are used, also check the service-to-service authorization for the Certificate Manager instance.

### Why is my load balancer in `maintenance_pending` state?
{: #maintenance-pending}
{: faq}

The load balancer is in `maintenance_pending` state during various maintenance activities such as:
* Horizontal scaling activities
* Recovery activities
* Rolling upgrade to address vulnerabilities and apply security patches

### Why do I need to choose multiple subnets during provisioning?
{: #why-do-I-need-to-choose-multiple-subnets-during-provisioning}
{: faq}

Load Balancer for VPC is Multi-Zone-Region (MZR) ready. Load balancer appliances are deployed to the subnets you selected. To achieve higher availability and redundancy, deploy the load balancer to subnets in different zones.

### Do I need additional IPs in the subnet for load balancer operations?
{: #do-I-need-additional-ips-in-subnet-for-load-balancer-operations}
{: faq}

Additional IPs in the subnet will be needed for horizontal scaling and maintenance operations.

### Why is back-end member health under my pool `unknown`?
{: #back-end-member-health-unknown}
{: faq}

* The pool is not associated with any listeners
* Configuration changes might have been made to the pool or its associated listener

### Why is back-end member health under my pool `faulted` ?
{: #back-end-member-health-failing}
{: faq}

Verify the following configurations:  
* Does the port of the configured back-end protocol match the port the application is listening on?
* Does the configured health-check have the correct protocol (TCP or HTTP), port, and URL (for HTTP) information? For HTTP, ensure your application responds with 200 OK for the configured health check URL.
* Is the back-end server a virtual server with an enabled security group? If so, ensure that the security group rules allow traffic between the load balancer and the virtual server.

Refer to the [Health Checks](#health-checks) section for additional information.
 
### Which TLS version is supported with SSL offload?
{: #which-tls-version-is-supported-with-ssl-offload}
{: faq}

Load Balancer for VPC supports TLS 1.2 with SSL termination.

The following ciphers are supported (listed in order of precedence):
* ECDHE-RSA-AES256-GCM-SHA384
* ECDHE-RSA-AES256-SHA384
* AES256-GCM-SHA384
* AES256-SHA256
* ECDHE-RSA-AES128-GCM-SHA256
* ECDHE-RSA-AES128-SHA256
* AES128-GCM-SHA256
* AES128-SHA256

### What are the default settings and allowed values for health check parameters?
{: #what-are-the-default-settings-and-allowed-values-for-health-check-parameters}
{: faq}

* Health check interval: Default is 5 seconds, and the range is 2 - 60 seconds.
* Health check response timeout: Default is 2 seconds, and the range is 1 - 59 seconds.
* Maximum retry attempts: Default is two retry attempts, and the range is 1-10 retries.

The health check response timeout value must be less than the health check interval value.
{:tip}

### Are the load balancer IP addresses fixed?
{: #are-the-load-balancer-ip-addresses-fixed}
{: faq}

Load balancer IP addresses are not guaranteed to be fixed, due to the built-in elasticity of the service. During horizontal scaling, you will see changes in the available IPs associated with the FQDN of your load balancer.

Use FQDN, rather than cached IP addresses.
{:important}

### Does the load balancer support Layer-7 switching?
{: #does-the-load-balancer-support-layer-7-switching}
{: faq}

Yes, the load balancer supports Layer-7 switching.

### Why does HTTPS listener creation or update tell me that my certificate is invalid?
{: #why-does-https-listener-creation-or-update-tell-me-that-my-certificate-is-invalid}
{: faq}

Check for these possibilities:

* The provided certificate CRN may not be valid.
* The certificate instance in Certificate Manager may not have an associated private key.
