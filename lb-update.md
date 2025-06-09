---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-09"

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
1. Select the **Navigation Menu** ![Navigation Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Select the Region of your load balancer.
1. Select the load balancer that you want to update.
1. Select **Back-end pools** if you need to edit a pool or virtual server instance parameters.
1. Select **Front-end listeners** if you need to edit listener parameters.
1. After you're done editing, select **Save** to save your changes.

The **Active** button on the upper left of your screen now shows as **Updating**. When **Updating** changes back to **Active**, the update is done and the new changes are applied.

## Updating an application load balancer from the CLI
{: #alb-updating-cli}
{: cli}

The following example shows how to use the CLI to update your ALB pool to use the algorithm `least_connections` and the port of the member:

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

## Updating an application load balancer with the API
{: #alb-updating-frontend-listener-port-api}
{: api}

The following example illustrates how to use the API to update the front-end listener port of an application load balancer. For example, if the front-end listener port was set to 80 and you want to update the port value to 90.

To update an application load balancer with the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Use the following example to get the listener ID that you need for the update:

   Save the ID of the load balancer

   ```bash
   export lbid="0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727"
   ```
   {: pre}

   ```bash
   curl -H "Authorization: $iam_token" -X GET
   "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
   ```
   {: codeblock}

   Sample output:

   ```sh
   {
       "created_at": "2020-08-24T23:36:22.990359Z",
       "crn": "crn:v1:bluemix:public:is:eu-gb-3:a/be636a7a6e4d4b6296bedf669ce8f88::load-balancer:r018-808fedde-2650-46cc-9cd1-4b828b92970a",
       "hostname": "808fedde-eu-gb.lb.appdomain.cloud",
       "href": "https://eu-gb.iaas.cloud.ibm.com/v1/load_balancers/r018-808fedde-2650-46cc-9cd1-4b828b92970a",
       "id": "r018-808fedde-2650-46cc-9cd1-4b828b92970a",
       "is_public": true,
       "listeners": [
           {
               "href": "https://eu-gb.iaas.cloud.ibm.com/v1/load_balancers/r018-808fedde-2650-46cc-9cd1-4b828b92970a/listeners/r018-3811d7ad-3bbe-4cb4-82de-8608f767866a",
               "id": "r018-3811d7ad-3bbe-4cb4-82de-8608f767866a"
           }
       ],
       "name": "test-alb",
       "operating_status": "online",
       "pools": [
           {
               "href": "https://eu-gb.iaas.cloud.ibm.com/v1/load_balancers/r018-808fedde-2650-46cc-9cd1-4b828b92970a/pools/r018-df11657f-63cf-49b5-af0f-6eeab32b3559",
               "id": "r018-df11657f-63cf-49b5-af0f-6eeab32b3559",
              "name": "pool1"
           },
           {
               "href": "https://eu-gb.iaas.cloud.ibm.com/v1/load_balancers/r018-808fedde-2650-46cc-9cd1-4b828b92970a/pools/r018-733df056-2a46-4b5a-8889-9a787e60871d",
               "id": "r018-733df056-2a46-4b5a-8889-9a787e60871d",
               "name": "pool2"
           }
       ],
       "private_ips": [
           {
               "address": "10.242.128.5"
           },
           {
               "address": "10.242.128.6"
           }
       ],
       "profile": {
           "family": "Network",
           "href": "https://eu-gb.iaas.cloud.ibm.com/v1/load_balancer/profiles/network-fixed",
           "name": "network-fixed"
       },
       "provisioning_status": "active",
       "public_ips": [
           {
               "address": "158.176.168.61"
           },
           {
               "address": "158.176.172.132"
           }
       ],
       "resource_group": {
           "href": "https://resource-controller.cloud.ibm.com/v1/resource_groups/42c4f51adc3147b4b4049ad9826c30a1",
           "id": "42c4f51adc3147b4b4049ad9826c30a1",
           "name": "Default"
       },
       "resource_type": "load_balancer",
       "subnets": [
           {
               "href": "https://eu-gb.iaas.cloud.ibm.com/v1/subnets/07a7-37b4dcfc-841e-4d4a-9f9f-9e45ffbd0285",
               "id": "07a7-37b4dcfc-841e-4d4a-9f9f-9e45ffbd0285",
               "name": "alb1"
           }
       ]
   }
   ```
   {: screen}

1. Save the listener ID that you want to update from the previous step. For example, save it in the variable `listenerid`.

   ```sh
   export listenerid="r018-3811d7ad-3bbe-4cb4-82de-8608f767866a"
   ```
   {: pre}

1. Update the listener port of the load balancer:

   ```bash
   curl -H "Authorization: $iam_token" -X PATCH
   "$vpc_api_endpoint/v1/load_balancers/$lbid/listeners/$listenerid?version=$api_version&generation=2" \
       -d '{"port": 200}'
   ```
   {: codeblock}

   Sample output:

   ```sh
   {
       "created_at": "2020-08-24T23:36:23.723008Z",
       "default_pool": {
           "href": "https://eu-gb.iaas.cloud.ibm.com/v1/load_balancers/r018-808fedde-2650-46cc-9cd1-4b828b92970a/pools/r018-df11657f-63cf-49b5-af0f-6eeab32b3559",
           "id": "r018-df11657f-63cf-49b5-af0f-6eeab32b3559",
           "name": "pool1"
       },
       "href": "https://eu-gb.iaas.cloud.ibm.com/v1/load_balancers/r018-808fedde-2650-46cc-9cd1-4b828b92970a/listeners/r018-3811d7ad-3bbe-4cb4-82de-8608f767866a",
       "id": "r018-3811d7ad-3bbe-4cb4-82de-8608f767866a",
       "port": 200,
       "protocol": "tcp",
       "provisioning_status": "update_pending"
   }
   ```
   {: screen}
