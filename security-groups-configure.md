---

copyright:
  years: 2018, 2020

lastupdated: "2020-10-12"

keywords:  

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Configuring the security groups for your VSI
{: #configuring-the-security-group}
{: step}

You can configure security groups to define the inbound and outbound traffic that they allow for your instance. For example, after you configure ACL rules for the subnet based on your company's security policies, you can further restrict traffic for specific instances depending on their workloads.

To configure your security group:

1. In the navigation pane, click **Compute > Virtual server instances**.
1. Click your instance to view its details.
1. In the **Network interfaces** section, click the security group.
1. Click **Add rule** to configure inbound and outbound rules that define what type of traffic is allowed to and from the instance. For each rule, specify the following information:  
   * Define a CIDR block or IP address for the permitted traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to or from all instances that are attached to the selected security group.   
   * Select the protocols and ports to which the rule applies.    

   **Tips:**  
  * All rules are evaluated, regardless of the order in which they're added.
  * Rules are stateful, which means that return traffic in response to allowed traffic is automatically permitted. For example, if you create a rule that allows inbound TCP traffic on port 80, that rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.
  * For Windows images, make sure that the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
1. _Optional:_ To view interfaces that are attached to the security group, click **Attached interfaces** in the navigation pane.
1. When you finish creating rules, click the **All security groups for VPC** breadcrumb at the beginning of the page.

## Example security group
{: #example-security-group}

For example, you can configure the following inbound rules:

 * Allow all SSH traffic (TCP port 22)
 * Allow all ping traffic (ICMP type 8)

| Protocol | Source Type | Source | Value |
|-----------|------|------|------|------------------|-------|
| TCP| Any | 0.0.0.0/0 | Ports 22-22
| ICMP | Any | 0.0.0.0/0 | Type: 8, Code: Any|
{: caption="Table 3. Configuration information for inbound rules" caption-side="top"}

Then, configure outbound rules that allow all TCP traffic:

| Protocol | Destination Type | Source | Value |
|-----------|------|------|------|------------------|-------|
| TCP| Any | 0.0.0.0/0 | Any port|
{: caption="Table 4. Configuration information for outbound rules" caption-side="top"}
