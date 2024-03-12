---

copyright:
  years: 2024
lastupdated: "2024-02-14"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Integrating a network load balancer with security groups
{: #nlb-integration-with-security-groups}

The {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) allows you to attach [security groups](/docs/vpc?topic=vpc-using-security-groups) to enhance your application's security.
{: shortdesc}

Security groups are a convenient way to secure your NLB instances. With a security group attached to your load balancer, you have full control over inbound and outbound traffic to and from the load balancer's listeners and its targets. Instead of identifying the load balancers using their IP addresses or CIDR range, targets can simply use the load balancer's security group as the source in their own security group definition. This ensures that traffic from all load balancer appliances is automatically allowed, irrespective of their IP addresses.

If a network load balancer has no associated security groups, then the security group rules will allow all traffic and the filtration will be done at the VSI level (according to the `iptables` rules).

## Network traffic rules
{: #nlb-traffic rules}

The following tables provide best practices for inbound and outbound traffic for both public and private network load balancers.

### Public Network Load Balancer
{: #public-network-load-balancer}

#### Inbound rules
{: #nlb-inbound-rules}

It is common to allow inbound traffic from all sources to the listener port on a public load balancer. For example:

| Protocol | Source Type | Source | Value |
|-----------|------|------|-------|
| TCP| IP Address | `0.0.0.0/0` | `Listener port` |
{: caption="Table 1. Configuration information for inbound rules of public load balancers" caption-side="bottom"}

However, if you need to restrict inbound traffic, you can specify a source CIDR, such as `209.173.53.167/20`. This allows all public IP addresses within the IP range to reach the public load balancer. For example:

| Protocol | Source Type | Source | Value |
|-----------|------|------|-------|
| TCP| IP Address | `209.173.53.167/20` | `Listener port` |
{: caption="Table 2. Configuration information for inbound rules of public load balancers from a specific CIDR" caption-side="bottom"}

#### Outbound rules
{: #nlb-outbound-rules}

Ensure that your targets are in a security group and configured as the destination in the outbound rules. Using a nested security group allows your NLB to allow only outbound traffic to the target and health check ports.

| Protocol | Destination type | Destination | Value |
|-----------|------|------|-------|
| TCP | Security group |  `target` | `target port` |
| TCP | Security group |  `target` | `Health check port` |
{: caption="Table 3. Configuration information for outbound rules of load balancers" caption-side="bottom"}

You can configure the outbound rules to be more permissive than shown (for example, you can allow all traffic to any destination). However, this is not recommended for security reasons.
{: note}

### Private Network Load Balancer
{: #private-network-load-balancer}

#### Inbound rules
{: #nlb-inbound-rules-private}

| Protocol | Source type | Source | Value |
|-----------|------|------|-------|
| TCP| IP address | `Subnet CIDR or VPC security group` | `Listener port` |
{: caption="Table 4. Configuration information for inbound rules for private load balancers" caption-side="bottom"}

It's a common practice to limit the inbound rules for a private load balancer to your own workload. Ensure that you specify the source from a specific subnet CIDR, or a security group that the source devices belong to. Using a specific CIDR, or nested security group, is preferred; however, individual IP addresses also work.

A nested security group is an option only when clients are in the same VPC. If the clients are in a different VPC (or on-prem and connected to the load balancer’s VPC through Transit Gateway or Direct Link) you must identify the clients using IP addresses or CIDRs.
{: note}

#### Outbound rules
{: #nlb-outbound-rules-private}

| Protocol | Destination type | Destination | Value |
|-----------|------|------|-------|
| TCP | Security group |  `Target security group` | `Target port` |
| TCP | Security group |  `Target security group` | `Health check port`(if different from the target port) |
{: caption="Table 5. Configuration information for outbound rules for private load balancers" caption-side="bottom"}

Ensure that your targets are in a security group and configured as the destination in your outbound rules. Using a nested security group enables your NLB to allow only outbound traffic to the target and health check ports. Using a nested security group enables your NLB to allow only outbound traffic to the target and health check ports.

In addition, your targets must have connectivity to the DNS resolver in order to resolve your load balancer's name. This is because load balancers are accessed through their DNS name.

## Attaching a security group during load balancer provisioning
{: #attaching-sg-nlb-provisioning}

You can attach up to five security groups when creating a network load balancer. If you do not specify a security group during load balancer creation, the default security group for your VPC is used.

If you do not select at least one security group, you should update your default security group rules to minimize traffic disruption on newly created network load balancers.
{: important}

### Prerequisite: Configure security groups and rules
{: #nlb-prerequisite-security-groups}

Ensure that you've created the security groups that you want to attach to your NLB. Also make sure that their rules are configured for load balancer traffic. If you need to create a security group, perform the following procedure.

Alternatively, you can use the [IBM Cloud VPC API](/apidocs/vpc/latest#create-security-group) to create a security group.
{: tip}

To create a security group using the UI:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click > **VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Security Groups** in the Network section.
1. Click **Create**.
1. Provide a unique name for your security group.
1. Select the VPC for your security group.
   The security group must be created in the same VPC as the load balancer.
1. Click **Add** to configure inbound and outbound rules that define what type of traffic is allowed to and from the security group. For each rule, complete the following information:
   * Specify a CIDR block or IP address for the permitted traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to or from all sources that are attached to the selected security group.
   * Select the protocols and ports to which the rule applies. For best practices about network rules, see [Network traffic rules](/docs/vpc?topic=vpc-alb-integration-with-security-groups#alb-traffic-rules).

   **Tips:**
   * All rules are evaluated, regardless of the order in which they are added.
   * Rules are stateful, which means that return traffic in response to allowed traffic is automatically permitted. For example, if you create a rule that allows inbound TCP traffic on port 80, that rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.

1. **Optional:** Edit the interfaces if you're planning to apply this security group to your other instances.
   Attaching security groups is performed in the load balancer section.
1. Click **Create security group** after you finish creating rules.

#### Security group example
{: #nlb-security-group-example}

For example, configure the following inbound rules, which allow all traffic on port 80 for a TCP listener (TCP port 80).

| Protocol | Source type | Source | Value |
|-----------|------|------|-------|
| TCP| Any | `0.0.0.0/0` | Port 80 |
{: caption="Table 6. Example configuration information for inbound rules" caption-side="bottom"}

Then, configure outbound rules that allow TCP traffic to your target:

| Protocol | Destination type | Destination | Value |
|-----------|------|------|-------|
| TCP| Any | `10.11.12.13/32` (Target IP address) | `80` (Target port) |
| TCP| Any | `10.11.12.14/32` (Target IP address) | `80` (Target health check port) |
{: caption="Table 7. Example configuration information for outbound rules" caption-side="bottom"}

### Procedure: Attaching security groups during NLB creation
{: #create-nlb-with-security-groups}

To attach security groups when [creating your network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer&interface=ui), follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click > **VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Load balancers** in the Network section.
1. Click **Create**.
1. Configure the name, VPC, type, subnet, listeners, and pools as needed.
1. Select the check boxes of the security groups that you want to attach from the security group table.
1. Click **Create** to provision the load balancer.

Make sure your security group rules allow for load balancer traffic. Ensure your listener, pool, and health check ports are allowed in your security group.
{: note}

## Attaching and detaching security groups
{: #attaching-detaching-sg-to-nlb}

To attach a security group to an existing load balancer, follow these steps:

Load balancers created prior to `07 August 2023` do not have a security group attached and allow all inbound and outbound traffic. If you attach a security group to a load balancer that does not have one, you can never remove it. You can revert to the previous "allow all inbound and outbound traffic" behavior by attaching a security group with rules for allowing all inbound and outbound traffic. However, this rule is inherently insecure and is not recommended.
{: important}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click > **VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Load balancers** in the Network section.
1. From the list of load balancers, select the load balancer to view its details page.
1. Click the **Attached security groups** tab to view attached security groups.
1. To attach one or more security groups, click **Attach**.
   You can select a maximum of five security groups to attach to your NLB.
1. Select the security group to attach.
1. Click **Attach**.

To detach a security group from a load balancer, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click > **VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Load balancers** in the Network section.
1. From the list of load balancers, select the load balancer to view its details page.
1. Click the **Attached security groups** tab to view attached security groups.
1. To detach a security group, click the security group's Action menu ![Actions icon](../icons/action-menu-icon.svg).
1. Click **Detach**.
