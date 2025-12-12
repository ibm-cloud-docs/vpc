---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About network ACLs
{: #using-acls}

You can use an access control list (ACL) to control all incoming and outgoing traffic in {{site.data.keyword.vpc_full}}. An ACL is a built-in, virtual firewall, similar to a security group. In contrast to security groups, ACL rules control traffic to and from the _subnets_, rather than to and from the _instances_.

For a comparison of the characteristics of security groups and ACLs, see the [comparison table](/docs/vpc?topic=vpc-security-in-your-vpc#compare-security-groups-and-access-control-lists).
{: tip}

The example that is presented in this document shows how to create network ACLs in your VPC by using the CLI. For more information about how to set up ACLs in the {{site.data.keyword.cloud_notm}} console, see [Configuring ACLs in the console](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#configuring-the-acl-ct).

## Working with ACLs and ACL rules
{: #working-with-acls-and-acl-rules}

To make your ACLs effective, create rules that determine how to handle your inbound and outbound network traffic. You can create multiple inbound and outbound rules. For more information about rules quotas, see [Quotas](/docs/vpc?topic=vpc-quotas#acl-quotas).

* With inbound rules, you can allow or deny traffic from a source IP range, with specified protocols and ports.
* With outbound rules, you can allow or deny traffic to a destination IP range, with specified protocols and ports.
* ACL rules are prioritized and considered in sequence. Higher priority rules are evaluated first and override lower priority rules.
* Inbound rules are separated from outbound rules.
* If no rules are specified, then **implicit deny** is the default behavior.

For more information about using ICMP, TCP, UDP, and all other protocols in your ACL rules, see [Understanding internet communication protocols](/docs/vpc?topic=vpc-understanding-icp#understanding-icp).

## Limitations
{: #limitations-acl}

* Currently, ESP protocol packets are supported only on instances with Gen2 profiles. On instances with other profiles, and on all bare metal servers, inbound and outbound ESP packets are always dropped. Although network ACL rules can be configured for ESP traffic with the VPC API, such rules affect only instances with Gen2 profiles. ESP protocol is not supported on other types of profiles that are attached to the same network as a Gen2 profile. The ESP protocol not displayed as a choice in the IBM Cloud console to avoid confusion between instance profile generations. For more information, see [Profile generation](/docs/vpc?topic=vpc-profiles&interface=ui#profiles-generation).

## Updating a VPC's default ACL rules
{: #updating-the-default-acl}

The default ACL is similar to any other ACL, with the exception that it cannot be deleted.
{: shortdesc}

When you create a VPC, the system creates a default ACL for the VPC with two rules:

* A rule named `allow-inbound` to allow inbound ICMP, TCP, and UDP traffic from any source
* A rule named `allow-outbound` to allow outbound ICMP, TCP, and UDP traffic to any destination

You can modify the rules of the default ACL by using the console, CLI, or API.

If you edit the rules of the default ACL, those edited rules then apply to all current and future subnets that are attached to the ACL.
{: important}

### Attaching an ACL to a subnet
{: #attaching-an-acl-to-a-subnet}

When you create a new subnet, you can specify an ACL to attach. If you don't specify an ACL, the VPC's default network ACL is attached.

Every subnet has exactly one ACL attached. You can replace a subnet's ACL with a different ACL.

## ACL example
{: #acl-demo-example}

In the example that follows, you create two ACLs and associate them with two subnets by using the command-line interface (CLI). Figure 1 shows what the scenario looks like.

![Figure showing an example ACL scenario](images/vpc-acls.svg){: caption="ACL with two subnets" caption-side="bottom"}

As Figure 1 illustrates, you have two web servers that deal with requests from the internet and two back-end servers that you want to hide from the public. In this example, you place the servers into two separate subnets, `10.10.10.0/24` and `10.10.20.0/24`, and you need to allow the web servers to exchange data with the back-end servers. Also, you want to allow limited outbound traffic from the back-end servers.

### Example rules
{: #acl-example-rules}

The example rules that follow show how to set up the ACL rules for the basic scenario.

As a best practice, give fine-grained rules a higher priority than coarse-grained rules. For example, you have a rule that blocks all traffic from the subnet `10.10.30.0/24`. If it is assigned a higher priority, any fine-grained rules with lower priority that allow traffic from `10.10.30.5` are never applied.
{: note}

For two-way protocols such as TCP, it is essential to define both inbound and outbound rules. The outbound rule allows the initial TCP SYN connection request, while the corresponding inbound rule is required to accept the TCP SYN ACK response. Without both rules, the communication can't be established.
{: note}

#### ACL-1 example rules
{: #acl1-example-rules}

| Direction| Allow/Deny | Source IP | Source Port | Destination IP | Destination Port | Protocol | Description|
|--------------|-----------|------|------|------|------------------|-------|----------|
| Inbound | Allow | `10.10.20.0/24`| Any | `10.10.10.0/24` | Any | Any| Allow all inbound traffic from the subnet `10.10.20.0/24` where the back-end servers are placed|
| Inbound | Allow | `0.0.0.0/0` | Any | `10.10.10.0/24` | 80 | TCP | Allow HTTP traffic from the internet to the web server|
| Inbound | Allow | `0.0.0.0/0` | Any | `10.10.10.0/24` | 443 | TCP | Allow HTTPS traffic from the internet to the web server|
| Inbound | Allow | `0.0.0.0/0` | 80 | `10.10.10.0/24` | Any | TCP | Allow HTTP return traffic from the internet to the web server|
| Inbound | Allow | `0.0.0.0/0` | 443 | `10.10.10.0/24` | Any | TCP | Allow HTTPS return traffic from the internet to the web server|
| Inbound | Deny| `0.0.0.0/0`| Any | `0.0.0.0/0`| Any | Any | Deny all other traffic inbound|
| Outbound | Allow | `10.10.10.0/24` | Any | `0.0.0.0/0`| 80 | TCP | Allow HTTP traffic from the web server to the internet|
| Outbound | Allow | `10.10.10.0/24` | Any |`0.0.0.0/0` | 443 | TCP | Allow HTTPS traffic from the web server to the internet|
| Outbound | Allow | `10.10.10.0/24` | 80 | `0.0.0.0/0`| Any | TCP | Allow HTTP return traffic from the web server to the internet|
| Outbound | Allow | `10.10.10.0/24` | 443 |`0.0.0.0/0` | Any | TCP | Allow HTTPS return traffic traffic from the web server to the internet|
| Outbound | Allow | `10.10.10.0/24` | Any |`10.10.20.0/24` | Any | Any | Allow all outbound traffic to the subnet `10.10.20.0/24` where the back-end servers are placed|
| Outbound | Deny | `0.0.0.0/0` | Any | `0.0.0.0/0`| Any | Any | Deny all other traffic outbound |
{: caption="Example rules for ACL-1" caption-side="bottom"}

#### ACL-2 example rules
{: #acl2-example-rules}

| Direction| Allow/Deny | Source IP | Source Port | Destination IP | Destination Port | Protocol | Description|
|--------------|-----------|------|------|------|------------------|-------|----------|
| Inbound | Allow| `10.10.10.0/24` | Any |`10.10.20.0/24`| Any | Any | Allow all inbound traffic from the subnet `10.10.10.0/24` where the web servers are placed|
| Inbound | Deny | `0.0.0.0/0` | Any | `0.0.0.0/0` | Any | Any | Deny all other traffic inbound|
| Inbound | Deny | `0.0.0.0/0` | 80 | `10.0.20.0/24` | Any | Any | Allow HTTP return traffic from the internet|
| Inbound | Deny | `0.0.0.0/0` | 443 | `10.0.20.0/24` | Any | Any | Allow HTTPS return traffic from the internet|
| Outbound | Allow | `10.10.20.0/24` | Any | `0.0.0.0/0` | 80 | TCP | Allow HTTP traffic to the internet |
| Outbound | Allow | `10.10.20.0/24` | Any | `0.0.0.0/0` | 443 | TCP | Allow HTTPS traffic to the internet |
| Outbound | Allow | `10.10.20.0/24` | Any | `10.10.10.0/24` | Any | Any | Allow all outbound traffic to the subnet `10.10.10.0/24` where the web servers are placed|
| Outbound | Deny | `0.0.0.0/0`| Any | `0.0.0.0/0`| Any | Any | Deny all other traffic outbound |
{: caption="Example rules for ACL-2" caption-side="bottom"}

This example illustrates general cases only. In your scenarios, you might want to have more granular control over the traffic:

* You might have a network administrator who needs access to the `10.10.10.0/24` subnet from a remote network for operation purposes. In that case, you need to allow SSH traffic from the internet to this subnet.
* You might want to narrow down the protocol scope that you allow between your two subnets.

### Example steps
{: #acl-example-steps}

The following example steps skip the prerequisite steps of using the CLI to create a VPC, which must be done first. For more information, see [Using the CLI to create VPC resources](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli).

#### Step 1. Create the ACLs
{: #step-1-create-the-acls}

Use the following CLI commands to create two ACLs, named `my_web_subnet_acl` and `my_backend_subnet_acl`:

```sh
ibmcloud is network-acl-create my-web-subnet-acl $vpc_id --source-acl-id $old_acl_id
ibmcloud is network-acl-create my-backend-subnet-acl $vpc_id --source-acl-id $old_acl_id
```
{: codeblock}

The response includes the newly created ACL IDs. Save the IDs of both ACLs to be used in later commands. You can use variables that are named `webacl` and `bkacl`, like this:

```sh
webacl="0738-ba9e785a-3e10-418a-811c-56cfe5669676"
bkacl="0738-a4e28308-8ee7-46ab-8108-9f881f22bdbf"
```
{: codeblock}

#### Step 2. Retrieve the default ACL rules
{: #step-2-retrieve-the-default-acl-rules}

Before you add rules, retrieve the default inbound and outbound ACL rules so that you can insert new rules before them.

```sh
ibmcloud is network-acl-rules $webacl
ibmcloud is network-acl-rules $bkacl
```
{: codeblock}

The response shows the default inbound and outbound rules that allow all IPv4 traffic in all protocols.

```sh
Getting rules of network acl ba9e785a-3e10-418a-811c-56cfe5669676 under account Demo Account as user demouser...

inbound
ID                                     Name                                                          Action   IPv*   Protocol   Source      Destination   Created
0738-e2b30627-1a1d-447b-859f-ac9431986b6f   allow-all-inbound-rule-2d86bc3f-58e4-436a-8c1a-9b0a710556d6   allow    ipv4   all        0.0.0.0/0   0.0.0.0/0     2 months ago

outbound
ID                                     Name                                                         Action   IPv*   Protocol   Source      Destination   Created
0738-173a3492-0544-472e-91c0-7828cbcb62d4   allow-all-outbound-rule-2d86bc3f-58e4-436a-8c1a-9b0a710556d6   allow    ipv4   all        0.0.0.0/0   0.0.0.0/0     2 months ago
```
{: screen}

Save the IDs of both ACL rules as variables so you can use the values in later commands. For example, you can save the IDs in variables `inrule` and `outrule`:

```sh
inrule="0738-e2b30627-1a1d-447b-859f-ac9431986b6f"
outrule="0738-173a3492-0544-472e-91c0-7828cbcb62d4"
```
{: screen}

#### Step 3. Add new ACL rules as described
{: #step-3-add-new-acl-rules-as-decribed}

In this example, first add inbound rules and then add outbound rules.

Insert new inbound rules before the default inbound rule.

```sh
ibmcloud is network-acl-rule-add my_web_acl_rule200 $webacl deny inbound all 0.0.0.0/0 0.0.0.0/0 \
--before-rule-id $in-rule
```
{: pre}

At each step, save the ID of the ACL rule in a variable so the ID can be used in later commands. For example, you can use the variable `acl200`:

```sh
acl200="0738-90930627-1a1d-447b-859f-ac9431986b6f"
```
{: pre}

Now add the rule to `acl200`:

```sh
ibmcloud is network-acl-rule-add my_web_acl_rule100 $webacl allow inbound all 10.10.20.0/24 0.0.0.0/0 \
--before-rule-id $acl200
```
{: pre}

Add more rules until your ACL setup is complete, saving each ID as a variable.

```sh
acl100="0738-78340627-1a1d-447b-859f-ac9431986b6f"
ibmcloud is network-acl-rule-add my_web_acl_rule20 $webacl allow inbound tcp 0.0.0.0/0 0.0.0.0/0 \
--source-port-min 443 --source-port-max 443 --before-rule-id $acl100
acl20="32450627-1a1d-447b-859f-ac9431986b6f"
ibmcloud is network-acl-rule-add my_web_acl_rule10 $webacl allow inbound tcp 0.0.0.0/0 0.0.0.0/0 \
--source-port-max 80 --source-port-min 80 --before-rule-id $acl20
```
{: codeblock}

Insert new outbound rules before the default outbound rule.

```sh
ibmcloud is network-acl-rule-add my_web_acl_rule200e $webacl deny outbound all 0.0.0.0/0 0.0.0.0/0 \
--before-rule-id $outrule
acl200e="11110627-1a1d-447b-859f-ac9431986b6f"
ibmcloud is network-acl-rule-add my_web_acl_rule100e $webacl allow outbound all 0.0.0.0/0 10.10.20.0/24 \
--before-rule-id $acl200e
acl100e="22220627-1a1d-447b-859f-ac9431986b6f"
ibmcloud is network-acl-rule-add my_web_acl_rule20e $webacl allow outbound tcp 0.0.0.0/0 0.0.0.0/0 \
--source-port-max 443 --source-port-min 443 --before-rule-id $acl100e
acl20e="33330627-1a1d-447b-859f-ac9431986b6f"
ibmcloud is network-acl-rule-add my_web_acl_rule10e $webacl allow outbound tcp 0.0.0.0/0 0.0.0.0/0 \
--source-port-max 80 --source-port-min 80 --before-rule-id $acl20e
```
{: codeblock}

#### Step 4. Create the two subnets with the newly created ACL
{: #step-4-create-the-two-subnets-with-the-newly-created-acl}

Create two subnets so that each of your ACLs is associated with one of the new subnets.

```sh
ibmcloud is subnet-create my-web-subnet $vpc_id $zone --ipv4_cidr_block 10.10.10.0/24 \
 --network-acl-id $webacl
ibmcloud is subnet-create my-backend-subnet$vpc_id $zone --ipv4_cidr_block 10.10.20.0/24 \
--network-acl-id $bkacl
```
{: codeblock}
{: pre}

## Command list cheat sheet
{: #acl-cli-command-list-cheat-sheet}

To show a complete list of the available VPC CLI commands for ACLs:

```sh
ibmcloud is network-acls
```
{: pre}

To see your ACL and its metadata, including rules:

```sh
ibmcloud is network-acl $webacl
```
{: pre}

To list all ACL rules:

```sh
ibmcloud is network-acl-rules $webacl
```
{: pre}

## Example inbound `ping` rule
{: #acl-example-inbound-ping-rule}

To add an ACL rule, here's an example command for adding a `ping` inbound rule before the default inbound rule:

Syntax:

```sh
ibmcloud is network-acl-rule-add ACL ACTION DIRECTION PROTOCOL SOURCE DESTINATION [--name NAME] [--icmp-type ICMP_TYPE] [--icmp-code ICMP_CODE] [--source-port-min PORT_MIN] [--source-port-max PORT_MAX] [--destination-port-min PORT_MIN] [--destination-port-max PORT_MAX] [--before-rule-id RULE_ID] [--output JSON] [-q, --quiet]
```
{: pre}

Example:

```sh
ibmcloud is network-acl-rule-add 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 allow inbound icmp 10.2.2.2 10.2.2.3 --icmp-type 8 --icmp-code 0
```
{: pre}

## Related links
{: #acl-related-links}

These links provide additional information about {{site.data.keyword.cloud_notm}} ACLs for VPC.

* [Network ACL CLI reference](/docs/vpc?topic=vpc-vpc-reference#network-acls)
* [Network ACL API reference](/apidocs/vpc/latest#list-network-acls)
* [Network ACL required permissions](/docs/account?topic=account-iam-service-roles-actions#is.network-acl-roles)
* [Network ACL activity tracking events](/docs/vpc?topic=vpc-at_events#events-network-acl)
* [Network ACL quotas](/docs/vpc?topic=vpc-quotas#acl-quotas)
