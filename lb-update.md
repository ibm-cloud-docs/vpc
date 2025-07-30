---

copyright:
  years: 2020, 2025
lastupdated: "2025-07-30"

keywords: application load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc network, update

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating an application load balancer
{: #alb-updating}

You can update an {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) with the console, CLI, or API.

## Updating an application load balancer in the console
{: #alb-updating-ui}
{: ui}

To update an ALB in the {{site.data.keyword.cloud_notm}} console, perform the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Select the Region of your load balancer.
1. Select the load balancer that you want to update.
1. Select **Back-end pools** if you need to edit a pool or virtual server instance parameters.
1. Select **Front-end listeners** if you need to edit listener parameters. To update the Client/Server timeout values, select **Edit** from the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") in the row of the Listener that you want to edit. You can select a timeout value between 50 seconds and 2 hours (7200 seconds). This value is for both the client and server. If you need more than 2 hours, you can [open a support case](/docs/account?topic=account-open-case&interface=ui) and provide a business requirement for the timeout value required.
1. After you're done editing, select **Save** to save your changes.

The **Active** button on the upper left of your window now shows as **Updating**. When **Updating** changes back to **Active**, the update is done and the new changes are applied.

## Updating an application load balancer from the CLI
{: #alb-updating-cli}
{: cli}

There are multiple options to update your ALB from the CLI. For options, see [VPC CLI reference for load balancers](/docs/vpc?topic=vpc-vpc-reference#lb-anchor).

The following example shows how to use the CLI to update your ALB pool to use the algorithm `least_connections` and the port of the member:

Sample command:

```sh
ibmcloud is load-balancer-pool-update r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 --algorithm least_connections
```
{: pre}

Sample output:

```sh
Updating pool r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 of load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...

ID                         r006-3b66d605-6aa5-4166-9f66-b16054da3cb0
Name                       alb-pool
Protocol                   tcp
Algorithm                  least_connections
Instance group             ID   Name
                           -    -

Health monitor             Type   Port   Health monitor URL   Delay   Retries   Timeout
                           http   8080   /                    10      2         5

Session persistence type   source_ip
Members                    r006-61f8b000-a90d-4abe-909e-c507dffec565
Provision status           update_pending
Created                    2020-08-27T14:45:42.038-05:00
```
{: screen}

Sample command:

```sh
ibmcloud is load-balancer-pool-member-update r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 r006-61f8b000-a90d-4abe-909e-c507dffec565 --port 6060
```
{: pre}

Sample output:

```sh
Updating member r006-61f8b000-a90d-4abe-909e-c507dffec565 of load balancer pool r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 under account IBM Cloud Network Services as user test@ibm.com...

ID                 r006-61f8b000-a90d-4abe-909e-c507dffec565
Port               6060
Target             0716_6acdd058-4607-4463-af08-d4999d983945
Weight             70
Health             unknown
Created            2020-08-27T14:59:55.446-05:00
Provision status   update_pending
```
{: screen}

### Updating an application load balancer timeout value from the CLI
{: #alb-timeout-value-updating-cli}
{: cli}

The following example shows how to use the CLI to update your ALB timeout value:

1. Get the listener ID:

```sh
ibmcloud is load-balancer LOAD_BALANCER_ID
```
{: pre}

1. Verify the current listener details:

```sh
ibmcloud is load-balancer-listener [--load-balancer LOAD_BALANCER_ID] [--listener LISTENER_ID] [--output JSON]
```
{: pre}

1. Update the timeout value:

```sh
ibmcloud is load-balancer-listener-update [--load-balancer LOAD_BALANCER_ID] [--listener LISTENER_ID] [--idle-connection-timeout VALUE_IN_SECONDS]
```
{: pre}

Where:

`--load-balancer`
:   ID or name of the load balancer.

`--listener`
:   ID or name of the listener.

`--output`
:   Output format, only `JSON` is supported.

`-idle-connection-timeout`
:   Idle connection timeout value in seconds.

Sample command:

```sh
ibmcloud is load-balancer-listener-update r042-ce351658-c236-4c99-8711-1414c5a6a99c r042-a00f2544-a1d3-4f9b-af31-4c34990a01f0 --idle-connection-timeout 90
```
{: pre}

Sample output:

```sh
Updating listener r042-a00f2544-a1d3-4f9b-af31-4c34990a01f0 of load balancer r042-ce351658-c236-4c99-8711-1414c5a6a99c under account SoftLayer Internal - Network Operations as user labhatia@in.ibm.com...

ID                        r042-a00f2544-a1d3-4f9b-af31-4c34990a01f0
Certificate instance      -
Connection limit          -
Ports                     22
Idle connection timeout   90
Protocol                  http
Default pool              -
Accept proxy protocol     false
Provision status          update_pending
Created                   2025-04-15T14:11:06+05:30
```
{: screen}

## Updating an application load balancer with the API
{: #alb-updating-frontend-listener-port-api}
{: api}

The following example illustrates how to use the API to update the front-end listener port of an application load balancer. For example, if the front-end listener port was set to 80 and you want to update the port value to 90.

To update an application load balancer with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Use the following example to get the listener ID that you need for the update:

   Save the ID of the load balancer:

   ```sh
   export lbid="r042-ce351658-c236-4c99-8711-1414c5a6a99c"
   ```
   {: pre}

   Install "jq" to view the output from the following command in clean JSON format.
   {: note}

   ```sh
   curl -H "Authorization: $iam_token" -X GET
   "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2" | jq
   ```
   {: codeblock}

   Sample output:

   ```sh
   {
    "access_mode": "private",
    "attached_load_balancer_pool_members": [],
    "availability": "subnet",
    "created_at": "2024-12-23T06:29:04Z",
    "crn": "crn:v1:bluemix:public:is:br-sao:a/5a83dca0e9c3408788bac5da136a9e76::load-balancer:r042-ce351658-c236-4c99-8711-1414c5a6a99c",
    "dns": {
        "instance": {
        "crn": "crn:v1:bluemix:public:dns-svcs:global:a/74589ab5050034ede18fa510153e91f8:a2b96ed8-cea2-4db0-b60a-c02d99586d4f::"
        },
        "zone": {
        "id": "a9240f0c-da77-4c55-be84-1d56c549a4de"
        }
    },
    "failsafe_policy_actions": [
        "fail",
        "drop",
        "forward"
    ],
    "hostname": "ce351658-br-sao.test.com",
    "href": "https://br-sao.iaas.cloud.ibm.com/v1/load_balancers/r042-ce351658-c236-4c99-8711-1414c5a6a99c",
    "id": "r042-ce351658-c236-4c99-8711-1414c5a6a99c",
    "instance_groups_supported": true,
    "is_private_path": false,
    "is_public": false,
    "listeners": [
        {
        "href": "https://br-sao.iaas.cloud.ibm.com/v1/load_balancers/r042-ce351658-c236-4c99-8711-1414c5a6a99c/listeners/r042-a00f2544-a1d3-4f9b-af31-4c34990a01f0",
        "id": "r042-a00f2544-a1d3-4f9b-af31-4c34990a01f0"
        }
    ],
    "logging": {
        "datapath": {
        "active": true
        }
    },
    "name": "test-alb",
    "operating_status": "online",
    "pools": [
        {
        "href": "https://br-sao.iaas.cloud.ibm.com/v1/load_balancers/r042-ce351658-c236-4c99-8711-1414c5a6a99c/pools/r042-40fe93a2-16f4-467d-b0e4-70c7b093e137",
        "id": "r042-40fe93a2-16f4-467d-b0e4-70c7b093e137",
        "name": "pooltest"
        }
    ],
    "private_ips": [
        {
        "address": "192.168.40.12",
        "href": "https://br-sao.iaas.cloud.ibm.com/v1/subnets/02v7-3520cb95-db1a-4787-9bde-f9cb8e811685/reserved_ips/02v7-be9dd47d-3899-4f67-8f7d-0a38ee73a376",
        "id": "02v7-be9dd47d-3899-4f67-8f7d-0a38ee73a376",
        "name": "author-sabbatical-zookeeper-paddleboat",
        "resource_type": "subnet_reserved_ip"
        },
        {
        "address": "192.168.40.13",
        "href": "https://br-sao.iaas.cloud.ibm.com/v1/subnets/02v7-3520cb95-db1a-4787-9bde-f9cb8e811685/reserved_ips/02v7-4a439c6a-15ed-43c0-910c-345d3af5f0c0",
        "id": "02v7-4a439c6a-15ed-43c0-910c-345d3af5f0c0",
        "name": "thinning-visitor-prance-chaps",
        "resource_type": "subnet_reserved_ip"
        }
    ],
    "profile": {
        "family": "application",
        "href": "https://br-sao.iaas.cloud.ibm.com/v1/load_balancer/profiles/dynamic",
        "name": "dynamic"
    },
    "provisioning_status": "active",
    "public_ips": [],
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/5710acc3545d425db62cb37037d17245",
        "id": "5710acc3545d425db62cb37037d17245",
        "name": "Default"
    },
    "resource_type": "load_balancer",
    "route_mode": false,
    "security_groups": [
        {
        "crn": "crn:v1:bluemix:public:is:br-sao:a/5a83dca0e9c3408788bac5da136a9e76::security-group:r042-f9a9b05b-9f19-4e65-a39c-8e5ccd1b0303",
        "href": "https://br-sao.iaas.cloud.ibm.com/v1/security_groups/r042-f9a9b05b-9f19-4e65-a39c-8e5ccd1b0303",
        "id": "r042-f9a9b05b-9f19-4e65-a39c-8e5ccd1b0303",
        "name": "resent-wrongness-erasure-flaky"
        }
    ],
    "security_groups_supported": true,
    "source_ip_session_persistence_supported": true,
    "subnets": [
        {
        "crn": "crn:v1:bluemix:public:is:br-sao-3:a/5a83dca0e9c3408788bac5da136a9e76::subnet:02v7-3520cb95-db1a-4787-9bde-f9cb8e811685",
        "href": "https://br-sao.iaas.cloud.ibm.com/v1/subnets/02v7-3520cb95-db1a-4787-9bde-f9cb8e811685",
        "id": "02v7-3520cb95-db1a-4787-9bde-f9cb8e811685",
        "name": "subnet-test-zone3",
        "resource_type": "subnet"
        }
    ],
    "udp_supported": false
   }

   ```
   {: screen}

    1. Save the listener ID that you want to update from the previous step. For example, save it in the variable `listenerid`.

    ```sh
    export listenerid="r042-a00f2544-a1d3-4f9b-af31-4c34990a01f0"
    ```
    {: pre}

    1. Update the listener port and client/server timeout values of the load balancer:

    ```sh
    curl -H "Authorization: $iam_token" -X PATCH
    "$vpc_api_endpoint/v1/load_balancers/$lbid/listeners/$listenerid?version=$api_version&generation=2" \
        -d '{"idle_connection_timeout": 50 , "port": 8080}' | jq
    ```
    {: codeblock}

    Sample output:

    ```text
    {
        "id": "r042-a00f2544-a1d3-4f9b-af31-4c34990a01f0",
        "href": "https://br-sao.iaas.cloud.ibm.com/v1/load_balancers/r042-ce351658-c236-4c99-8711-1414c5a6a99c/listeners/r042-a00f2544-a1d3-4f9b-af31-4c34990a01f0",
        "protocol": "http",
        "port": 8080,
        "port_min": 8080,
        "port_max": 8080,
        "provisioning_status": "update_pending",
        "created_at": "2025-04-15T08:41:06Z",
        "accept_proxy_protocol": false,
        "idle_connection_timeout": 50
    }
   ```
   {: screen}

You can set a timeout value between 50 and 7200 seconds. If you need more than 2 hours (7200 seconds), you can [open a support case](/docs/account?topic=account-open-case&interface=ui).
{: note}
