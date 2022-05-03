---

copyright:
  years: 2019, 2021

lastupdated: "2021-05-03"

keywords:  

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About security groups
{: #using-security-groups}

IBM Cloud Security Groups for VPC give you a convenient way to apply rules that establish filtering to each network interface of a virtual server instance, based on its IP address. When you create a security group, you configure it to create the network traffic patterns you want.
{: shortdesc}

By default, a security group denies all traffic. As rules are added to a security group, it defines the traffic that the security group permits.

Rules are _stateful_, which means that reverse traffic in response to allowed traffic is automatically permitted. For example, if you create a rule to allow inbound TCP traffic on port 80, the rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.

Security groups are scoped to a single VPC. This scoping implies that a security group can be attached _only_ to network interfaces of instances within the same VPC.

When an instance is created and no security groups are specified, the instance's primary network interface is attached to the _default_ security group of that instance's VPC. For more information, see [Updating the default security group](/docs/vpc?topic=vpc-updating-the-default-security-group#updating-the-default-security-group).

## Security groups versus network ACLs
{: #security-groups-vs-network-acls}

Security groups are tied to an instance whereas Network ACLs (NACLs) are tied to the subnet.
{: shortdesc}

NACLs are applicable at the subnet level, so any instance (for example, a virtual server instance) in the subnet with an associated NACL will follow rules of the NACL. However, that’s not the case with security groups. Security groups must be assigned explicitly to the instance. Also, unlike NACLs, a security group can be applied to multiple instances across subnets and even across zones.

![Security groups across instances and zones](/images/security-groups-across-zones.png){: caption="Security groups across instances and zones" caption-side="bottom"}

Every security group consists of a set of rules. The security group examines all of its rules before allowing any traffic to enter or leave the instance. The rules that are used to control the inbound traffic are independent of the rules that are used to control the outbound traffic. 

When a new security group is created, initially all inbound traffic is restricted and outbound traffic is allowed. Therefore, you must add rules to the group to permit incoming traffic and to apply restrictions on the outbound traffic. 

Because an instance can have multiple security groups associated with it, all the rules from each security group associated with the instance are combined together to form a single set of rules. This set of rules is used to determine whether the traffic should be denied or allowed into the instance. For every security rule that you add to the security group, you must specify the values for the following fields: 

* Direction - The direction of traffic to enforce, either inbound or outbound.
* Protocol - Indicates the prototype that this rule applies for. Values are `TCP`, `UDP`, `ICMP`, or `ALL`. 

   * If its value is `ALL`, it means that this rule applies to all protocols. Then, it's invalid to specify the port range (PortMin, PortMax). 
   * If protocol is either `TCP` or `UDP`, then the rule can also contain the port range (PortMin, PortMax). You must set either both ports, or neither. When neither is set, then traffic is allowed on all ports. For a single port, you must set both ports to the same value. 
   * When protocol is `ICMP`, you can optionally specify the `Type` property. If specified, then ICMP traffic is allowed for only the specified ICMP type. Further, if you specify `Type`, you can optionally specify the code property to allow traffic for only the specified ICMP code. 

*	Remote - Describes the set of network interfaces to which this rule allows traffic (or from which, for outbound rules). You can specify this value as either an IP address, a CIDR block, or all the identifiers of a single security group (ID, CRN, and name). If this value is omitted, a CIDR block of `0.0.0.0/0` is used to allow traffic from any source (or to any source, for outbound rules).

## Related links
{: #sg-related-links}

These links provide additional information about IBM Cloud Security Groups for VPC:

* [Security Groups CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#security-groups-cli-ref)
* [Security Groups API reference](https://{DomainName}/apidocs/vpc#list-security-groups)
* [Security Groups required permissions](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls#sg-authorizations-required-for-api-and-cli-calls)
* [Security Groups for VPC infrastructure resources for Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_security_group){: external} (VPC infrastructure > Resources)
* [Security Groups Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-network-security-group)
* [Security Groups quotas](/docs/vpc?topic=vpc-quotas#security-group-quotas)
