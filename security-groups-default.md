---

copyright:
  years: 2018, 2025
lastupdated: "2025-12-19"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating a VPC's default security group rules
{: #updating-the-default-security-group}

The default security group is similar to any other security group, with the exception that it cannot be deleted.
{: shortdesc}

When you create a VPC, the system creates a default security group for the VPC, with two rules:

* A rule named `inbound-icmp-tcp-udp-from-this-security-group` to allow inbound ICMP, TCP, and UDP traffic from any member of this security group (that is, all other resources that are attached to this security group)
* A rule named `outbound-icmp-tcp-udp` to allow outbound ICMP, TCP, and UDP traffic to any destination

Rules to allow inbound pinging and SSH from resources outside the security group are not automatically added to the default security group.

You can modify the rules of the default security group by using the console, CLI, or API.

If you edit the rules of the default security group, those edited rules then apply to all current and future servers in the group.
{: important}

## Updating the default security group in the console
{: #example-modifying-the-default-sg-rules-using-ui}
{: ui}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external}.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Security groups**.
1. In the Security groups list, click the name of the default security group that you want to update.
1. On the default security group's details page, click the **Rules** tab.
1. Modify the rules as necessary:
      - To edit a rule, click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions"), click **Edit**, make your changes, then click **Save**.
      - To delete a rule, click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions"), click **Delete**, then click **Delete** again.
      - To create a rule, click **Create**.

      For each rule, specify the following information:

      * Select the protocols and ports to which the rule applies. For more information about using ICMP, TCP, and UDP protocols in your ACL rules, see [Understanding internet communication protocols](/docs/vpc?topic=vpc-understanding-icp#understanding-icp).
      * Specify a CIDR block or IP address for the permitted traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to or from all instances that are attached to the selected security group.

      **Tips:**
      * All rules are evaluated, regardless of the order in which they're added.
      * The rules are stateful, which means that return traffic in response to allowed traffic is automatically permitted. For example, you created a rule that allows inbound TCP traffic on port 80. That rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.
      * For Windows images, make sure that the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).

1. _Optional:_ To view interfaces that are attached to the security group, click the **Attached resources** tab and review the Attached interfaces section.
1. When you finish creating rules, click the **Security groups** breadcrumb to return to your list of Security groups for VPC.


## Updating the default security group from the CLI
{: #example-modifying-the-default-security-group-rules-using-the-cli}
{: cli}

Complete the following steps to update the default security group by using the CLI:

1. Log in to {{site.data.keyword.vpc_full}}.

   If you have a federated account:

   ```sh
   ibmcloud login -sso
   ```
   {: pre}

   Otherwise, use this command:

   ```sh
   ibmcloud login
   ```
   {: pre}

2. Get the default security group ID and details for the VPC.

   Run the following command to list all VPCs:

   ```sh
   ibmcloud is vpcs
   ```
   {: pre}

   The default security group name is shown under the column `Default Security Group`. Note the name of it so that you can find the `ID` when you list the security groups (next).

   Now list all the security groups:

   ```sh
   ibmcloud is security-groups
   ```
   {: pre}

   Save the security group ID (for the default security group) in a variable so that you can use it later. For example, use the variable name `sg`:

   ```sh
   sg=0738-2d364f0a-a870-42c3-a554-000001162469
   ```
   {: pre}

   To get details about the security group, run the following command:

   ```sh
   ibmcloud is security-group GROUP
   ```
   {: pre}

   For example, run the following command to get details about the security group with the security group ID you saved as a variable:

    ```sh
   ibmcloud is security-group $sg
   ```
   {: pre}

   Alternatively, you can insert the actual ID value in place of the variable `$sg`.

3. Update the default security group to add rules that allow SSH and PING.

   Disabling SSH connections prohibits the license registration for Red Hat Enterprise Linux. It can result in provisioning failures.
   {: important}

   To add rules in your default security group, run the following command:

   ```sh
   ibmcloud is security-group-rule-add GROUP DIRECTION PROTOCOL [--port-min PORT_MIN] [--port-max PORT_MAX]
   ibmcloud is security-group-rule-add GROUP DIRECTION PROTOCOL [--icmp-type ICMP_TYPE [--icmp-code ICMP_CODE]]
   ```
   {: codeblock}

   For example, run the following command to add rules that allow SSH and PING rules to the security group with the ID you set as your variable:

   ```sh
   ibmcloud is security-group-rule-add $sg inbound tcp --port-min 22 --port-max 22
   ibmcloud is security-group-rule-add $sg inbound icmp --icmp-type 8 --icmp-code 0
   ```
   {: codeblock}

Adding and removing security group rules is an asynchronous operation. It usually takes 1 - 30 seconds for the change to go into effect.
{: note}

## Updating the default security group with the API
{: #example-modifying-the-default-security-group-rules-using-the-api}
{: api}

Complete the following steps to update the default security group by using the API:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the correct variables.

1. Get the default security group ID and details for the VPC.

   Run the following command to list all VPCs:

   ```sh
   curl -sX GET -H "Authorization:$iam_token" "$vpc_api_endpoint/v1/vpcs?generation=2&version=2022-09-13"
   ```
   {: pre}

   Locate `default_security_group` in the API response to see the details of the default security group. Note the name of the default security group so that you can find the `id` when you list the security groups (next). Note the default security group `id`, and save it in a variable so that you can use it later. For example, use the variable name `id`:

   ```sh
   "sg" = "r006-a937009e-5da5-4e7b-9072-d44e1095327b"
   ```
   {: pre}

1. Update the default security group to add rules that allow SSH and PING.

Disabling SSH connections prohibits the license registration for Red Hat Enterprise Linux. It can result in provisioning failures.
{: important}

Run the following two commands to add default security group rules that allow SSH and PING for the security group with the security group `id` you set as the variable `sg`:

   ```sh
   curl -sX POST  -H "Authorization:$iam_token" "$vpc_api_endpoint/v1/security_groups/$sg/rules?generation=2&version=$api_version"  -d '{"direction":"inbound","protocol":"tcp","port_min":22, "port_max":22}'
   ```
   {: pre}

   ```sh
   curl -sX POST  -H "Authorization:$iam_token" "$vpc_api_endpoint/v1/security_groups/$sg/rules?generation=2&version=$api_version"  -d '{"direction":"inbound","protocol":"icmp","code":0, "type":9}'
   ```
   {: pre}

Adding and removing security group rules is an asynchronous operation. It usually takes 1 - 30 seconds for the change to go into effect.
{: note}
