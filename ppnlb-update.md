---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-09"

keywords: network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc network, update

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating a Private Path network load balancer
{: #ppnlb-updating}

You can update an {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) with the console, CLI, or API.
{: shortdesc}

## Updating a Private Path network load balancer in the console
{: #ppnlb-updating-ui}
{: ui}

To update a Private Path network load balancer in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login) and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. In the Load balancers for VPC table, locate and select the name of the Private Path network load balancer that you want to update.
1. On the network load balancer details page, select the **Front-end listeners** tab if you need to edit listener parameters. In the Front-end listeners table, click the Edit icon ![Edit icon](images/edit.png) beside the details that you want to update.
1. On the network load balancer details page, select the **Back-end pools** tab if you need to edit a pool or virtual server instance parameters. In the Back-end pools table, click the Edit icon ![Edit icon](images/edit.png) beside the details that you want to update.
1. If you are attaching new members, select **Attach** in the Members column of the back-end pool to which you want to add members. Specify the following information, then click **Attach**.

   * **Member type**: Add Virtual server instances, or an application load balancer as a member. If a load balancer is not available, select **Create load balancer**. For Virtual server instances, attach each type individually.

      If you attach an ALB as a member target to a Private Path NLB pool, no other members can be added to that pool.
      {: note}

   * **Subnet**: Choose a subnet.
   * From the list of servers, select the one that you want to attach to the back-end pool. Ensure that you specify valid values for each server port.

      You can attach up to 50 virtual server instances to a back-end pool. However, your Private Path load balancer provides regional availability and is resilient to zone failure even if you select a single subnet.
      {: note}

    You do not need to create multiple network load balancers or specify more than a single subnet to ensure resiliency to zone failure. Your subnet selection only impacts the IP-addresses associated with the load balancer.

1. After you're done editing, select **Save** to save your changes.

The Active button on the upper left of your screen now shows as Updating. When Updating changes back to Active, the update is done and the new changes are applied.
{: note}

## Updating a Private Path network load balancer from the CLI
{: #ppnlb-updating-cli}
{: cli}

The following example shows how to use the CLI to update your Private Path NLB pool to use the algorithm `least_connections` and the port of the member:

```sh
ibmcloud is load-balancer-pool-update r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 --algorithm least_connections
```
{: pre}

Sample output:

```sh
  Updating pool r006-3b66d605-6aa5-4166-9f66-b16054da3cb0 of load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...

  ID                         r006-3b66d605-6aa5-4166-9f66-b16054da3cb0
  Name                       nlb-pool
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

## Updating a Private Path network load balancer with the API
{: #ppnlb-updating-frontend-listener-port-api}
{: api}

The following example illustrates how to use the API to update the front-end listener port of a Private Path NLB. For example, if the front-end listener port was set to 80 and you want to update the port value to 90.

To update a Private Path NLB by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Use the following example to get the listener ID that you need for the update:

   Save the ID of the load balancer.

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
       "name": "test-nlb",
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
               "name": "nlb1"
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
