---

copyright:
  years: 2021, 2024
lastupdated: "2024-10-10"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Integrating an application load balancer with security groups
{: #alb-integration-with-security-groups}

The {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) allows you to attach [security groups](/docs/vpc?topic=vpc-using-security-groups) to enhance your application's security.
{: shortdesc}

Security groups are a convenient way to secure your ALB instances. With a security group attached to your load balancer, you have full control over inbound and outbound traffic to and from the load balancer's listeners and its back-end targets. This feature also makes it convenient to tighten the security posture of the load balancers' back-end targets. Instead of identifying the load balancers using their IP addresses or CIDR range, back-end targets can simply use the load balancer's security group as the source in their own security group definition. This ensures that traffic from all load balancer appliances is allowed automatically, irrespective of their IP addresses.

## Network traffic rules
{: #alb-traffic-rules}

The following tables provide best practices for inbound and outbound traffic for both public and private application load balancers.

### Public Application Load Balancer
{: #public-application-load-balancer}

#### Inbound rules
{: #lb-inbound-rules}

| Protocol | Source Type | Source | Value |
|-----------|------|------|-------|
| TCP| IP Address | 0.0.0.0/0 | `Listener port` |
{: caption="Configuration information for inbound rules of public load balancers" caption-side="bottom"}

In a typical use case, it is common to allow inbound traffic from all sources to the listener port on a public load balancer.

| Protocol | Source Type | Source | Value |
|-----------|------|------|-------|
| TCP| IP Address | 209.173.53.167/20 | `Listener port` |
{: caption="Configuration information for inbound rules of public load balancers from a specific CIDR" caption-side="bottom"}

However, if your requirements need to restrict inbound traffic, you may specify a source CIDR, such as `209.173.53.167/20`. This will allow all public IP addresses within the IP range to reach the public load balancer.

#### Outbound rules
{: #lb-outbound-rules}

| Protocol | Destination type | Destination | Value |
|-----------|------|------|-------|
| TCP | Security group |  `Back-end target` | `Back-end target port` |
| TCP | Security group |  `Back-end target` | `Health check port` |
{: caption="Configuration information for outbound rules of public load balancers" caption-side="bottom"}

Ensure that your back-end targets are in a security group and configured as the destination in the outbound rules. Using a nested security group allows your ALB to allow only outbound traffic to the back-end target and health check ports.

You can configure the outbound rule to be more permissive than shown (for example, allow all traffic to any destination). However, for security reasons, this is not recommended.
{: note}

### Private Application Load Balancer
{: #private-application-load-balancer}

#### Inbound rules
{: #lb-inbound-rules-private}

| Protocol | Source type | Source | Value |
|-----------|------|------|-------|
| TCP| IP address | `Subnet CIDR or VPC security group` | `Listener port` |
{: caption="Configuration information for inbound rules for private load balancers" caption-side="bottom"}

It is typical to limit the inbound rules for a private load balancer to your own workload. Ensure that you specify the source from a specific subnet CIDR, or a security group that the source devices belong to. Using a specific CIDR, or nested security group, is preferred; however, individual IP addresses also work.

A nested security group is an option only when clients are in the same VPC. If the clients are in a different VPC or on-premises, connected to the load balancer’s VPC through Transit Gateway or Direct Link, you must identify the clients using IP addresses or CIDRs.
{: note}

#### Outbound rules
{: #outbound-rules}

| Protocol | Destination type | Destination | Value |
|-----------|------|------|-------|
| TCP | Security group |  `Back-end target security group` | `Back-end target port` |
| TCP | Security group |  `Back-end target security group` | `Health check port`(if different from the back-end target port) |
{: caption="Configuration information for outbound rules for private load balancers" caption-side="bottom"}

Ensure that your back-end targets are in a security group and configured as the destination in the outbound rules. Using a nested security group enables your ALB to allow only outbound traffic to the back-end target and health check ports.

In addition, your back-end targets must have connectivity to the DNS resolver in order to resolve your load balancer's name. This is because load balancers are accessed through their DNS name.

## Attaching a security group during load balancer provisioning
{: #attaching-sg-alb-provisioning}

You can attach up to five security groups when creating an application load balancer. If you do not specify a security group during load balancer creation, the default security group for your VPC is used.

If you do not select at least one security group, it is recommended that you update your default security group rules to minimize disruption in load balancer traffic on newly created application load balancers.
{: important}

### Prerequisite: Configure security groups and rules
{: #prerequisite-security-groups}

Ensure that the security groups exist that you want to attach to your ALB. Also make sure that their rules are configured for load balancer traffic. If you need to create a security group, follow these steps. Alternatively, you can use [IBM Cloud VPC APIDOCS](/apidocs/vpc/latest#create-security-group) to create a security group.

To create a security group using the UI:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click > **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Security Groups** in the Network section.
1. Click **Create**.
1. Provide a unique name for your security group.
1. Select the VPC for your security group.
   The security group must be created in the same VPC as the load balancer.
1. Click **Add** to configure inbound and outbound rules that define what type of traffic is allowed to and from the security group. For each rule, complete the following information:
   * Specify a CIDR block or IP address for the permitted traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to or from all sources that are attached to the selected security group.
   * Select the protocols and ports to which the rule applies. For best practices about network rules, see [Network traffic rules](/docs/vpc?topic=vpc-alb-integration-with-security-groups#alb-traffic-rules).

   **Tips:**
   * All rules are evaluated, regardless of the order in which they're added.
   * Rules are stateful, which means that return traffic in response to allowed traffic is automatically permitted. For example, if you create a rule that allows inbound TCP traffic on port 80, that rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.

1. **Optional:** Edit the interfaces if you're planning to apply this security group to your other instances.
   Attaching security groups is performed in the load balancer section.
1. Click **Create security group** after you finish creating rules.

#### Security group example
{: #lb-security-group-example}

For example, configure the following inbound rules, which allow all traffic on port 80 for an HTTP listener (TCP port 80).

| Protocol | Source type | Source | Value |
|-----------|------|------|-------|
| TCP| Any | `0.0.0.0/0` | Port 80 |
{: caption="Example configuration information for inbound rules" caption-side="bottom"}

Then, configure outbound rules that allow TCP traffic to your back-end target:

| Protocol | Destination type | Destination | Value |
|-----------|------|------|-------|
| TCP| Any | `10.11.12.13/32` (Back-end target IP address) | 80 (Back-end target port) |
| TCP| Any | `10.11.12.14/32` (Back-end target IP address) | 80 (Back-end target health check port) |
{: caption="Example configuration information for outbound rules" caption-side="bottom"}

### Procedure: Attaching security groups during ALB creation
{: #create-alb-with-security-groups}

To attach security groups when [creating your application load balancer](/docs/vpc?topic=vpc-load-balancers&interface=ui), follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click > **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Load balancers** in the Network section.
1. Click **Create**.
1. Configure the name, VPC, type, subnet, listeners, and pools as needed.
1. Select the checkboxes of the security groups that you want to attach from the security group table.
1. Click **Create** to provision the load balancer.

Make sure your security group rules allow for load balancer traffic. Ensure your listener, pool, and health check ports are allowed in your security group.
{: note}

## Attaching and detaching security groups
{: #attaching-detaching-sg-to-alb}

To attach a security group to an existing load balancer, follow these steps:

Load balancers created prior to 25 February 2021 do not have a security group attached and allow all inbound and outbound traffic. If you attach a security group to a load balancer that does not have a security group, you cannot revert back to having no security groups. You can revert to the previous "allow all inbound and outbound traffic" behavior by attaching a security group with rules for allowing all inbound and outbound traffic. However, such a rule is inherently less secure than having a more restrictive security group in place, and is not recommended.
{: important}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click > **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Load balancers** in the Network section.
1. From the list of load balancers, select the load balancer to view its details page.
1. Click the **Attached security groups** tab to view attached security groups.
1. To attach one or more security groups, click **Attach**.
   You can select a maximum of five security groups to attach to an ALB.
1. Select the security group to attach.
1. Click **Attach**.

To detach a security group from a load balancer, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click > **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Load balancers** in the Network section.
1. From the list of load balancers, select the load balancer to view its details page.
1. Click the **Attached security groups** tab to view attached security groups.
1. To detach a security group, click the security group's Action menu ![Actions icon](../icons/action-menu-icon.svg).
1. Click **Detach**.
