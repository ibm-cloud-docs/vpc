---

copyright:
  years: 2019, 2020

lastupdated: "2020-04-04"

keywords:  

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating a network ACL
{: #acl-create-ui}

You can configure an ACL to limit inbound and outbound traffic to the subnet. By default, all traffic is allowed.
{: shortdesc}

Each subnet can be attached to only one ACL. However, each ACL can be attached to multiple subnets.
{: important}

Before you begin, ensure that you have created a VPC and subnet.

## Creating a network ACL using the UI
{: #configuring-the-acl}
{: ui}

To configure an ACL using the IBM Cloud console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){:external} and log in to your account.
1. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) from the upper left of the page, then click **VPC Infrastructure > Access control lists** in the Network section.
1. Click **Create** in the upper right of the page.
1. In the order form, complete the following information:
   * Type a unique name for your ACL and select a VPC.
   * Select a resource group. Use the default group, or select from the list (if defined for your account). Remember that you cannot change the resource group after the ACL is created.
   * Click **Add** to configure inbound and outbound rules that define what traffic is allowed in or out of the subnet. For each rule, specify the following information:
      * Select whether to allow or deny the specified traffic.
      * Select the protocol to which the rule applies.  
      * For the source and destination of the rule, specify the IP range and ports for which the rule applies. For example, if you want all inbound traffic to be allowed to the IP range `192.168.0.0/24` in your subnet, specify **Any** as the source and `192.168.0.0/24` as the destination. However, if you want to allow inbound traffic only from `169.168.0.0/24` to your entire subnet, specify `169.168.0.0/24` as the source and **Any** as the destination for the rule.
      * Specify the rule's priority. Rules with lower numbers are evaluated first and override rules with higher numbers. For example, if a rule with priority `2` allows HTTP traffic and a rule with priority `5` denies all traffic, HTTP traffic is still allowed.   
   * Select a subnet to attach to this ACL. Click **Attach subnets** to attach additional subnets.  

      If the subnet has an existing ACL connection, the ACL is replaced by the ACL being created.
      {: note}

## Creating a network ACL using the CLI
{: #cr-using-the-cli}
{: cli}

To create a network ACL by using the CLI, run the following command:
{: shortdesc}

```
ibmcloud is network-acl-create ACL_NAME VPC \
[--rules (RULES_JSON|@RULES_JSON_FILE) | --source-acl-id SOURCE_ACL_ID] \
[--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
[--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

* **ACL_NAME** is the name of the network ACL.
* **VPC** is the ID of the VPC.
* **--rules** are the rules for the ACL in JSON or JSON file.
* **--source-acl-id** is the ID of the network ACL to copy rules from.
* **--resource-group-id** is the ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
* **--resource-group-name** is the name of the resource group. This option is mutually exclusive with **--resource-group-id**.
* **--output** specifies output in JSON format.
* **-q, --quiet** suppresses verbose output.

For example:

- `ibmcloud is network-acl-create my-acl 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479`
- `ibmcloud is network-acl-create my-acl 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --source-acl-id 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3`

## Creating a network ACL using the API
{: #cr-using-the-api}
{: api}

To create a network ACL by using the API, follow these steps:
{: shortdesc}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` value in a variable to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    ```
    {: pre}

1.  Create a network ACL:  

   ```sh
   curl -X POST -sH "Authorization:${iam_token}" \
   "$vpc_api_endpoint/v1/vpcs/$vpc_id/routing_tables?version=$api_version&generation=2" \
   -d '{"name": "testvpc","resource_group": {"id": "'$ResourceGroupId'"}}' | jq
   ```
   {: pre}

   For example, the following network ACL is created on mzr05.

   ```sh
   curl -X POST -sH "Authorization:${iam_token}" \
   "$vpc_api_endpoint/v1/vpcs/$vpc_id/routing_tables?version=$api_version&generation=2" \
   -d  '{"name": "routing-table-3", "routes": [{"name": "route-1", "zone": {"name": "us-south-2"}, \
   "action": "deliver", "destination": "1.2.3.0/24", "next_hop": {"address": "10.0.0.1"}}, \
   {"name": "route-2", "zone": {"name": "us-south-2"}, "action": "drop", "destination": "1.2.4.0/24"}, \
   {"name": "route-3", "zone": {"name": "us-south-2"}, "action": "delegate", "destination": "1.2.5.0/24", \
   "next_hop": {"address": "0.0.0.0"}}]}' | jq
   ```
   {: pre}

## Example: Configuring rules
{: #example-configuring-rules}

For example, you can configure the following inbound rules:

* Allow HTTP traffic from the internet.
* Allow all inbound traffic from the subnet `10.10.20.0/24`.
* Deny all other inbound traffic.

| Priority | Allow/Deny | Protocol | Source | Destination |
|--------------|-----------|------|------|------|
| 1 | Allow | TCP | Any IP, ports 80 - 80 |Any IP, any port|
| 2 | Allow | ALL | 10.10.20.0/24, any port |Any IP, any port|
| 3 | Deny| ALL | Any IP, any port |Any IP, any port|
{: caption="Table 1. Information for configuring inbound rules" caption-side="top"}

Then, configure the following outbound rules:

* Allow HTTP traffic to the internet.
* Allow all outbound traffic to the subnet `10.10.20.0/24`.
* Deny all other outbound traffic.

| Priority | Allow/Deny | Protocol | Source | Destination |
|--------------|-----------|------|------|------|
| 1 | Allow | TCP | Any IP, any port |Any IP, ports 80 - 80  |
| 2 | Allow | ALL | Any IP, any port | 10.10.20.0/24, any port |
| 3 | Deny| ALL | Any IP, any port |Any IP, any port|
{: caption="Table 2. Information for configuring outbound rules" caption-side="top"}
