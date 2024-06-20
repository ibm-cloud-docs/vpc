---

copyright:
  years: 2020, 2024
lastupdated: "2024-06-20"

keywords: vpe, acls, virtual private endpoints, endpoint gateways

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring ACLs and security groups for use with endpoint gateways
{: #configure-acls-sgs-endpoint-gateways}

You can configure access control lists (ACLs) and security groups for use with endpoint gateways.

## Configuring ACLs for use with endpoint gateways
{: #vpe-configuring-acls}

You can use ACLs to control the traffic in your Virtual Private Endpoint (VPE) for VPC deployment. Use ACLs to filter traffic going to and from the VPE.
{: shortdesc}

Ideally, you should configure the ACLs before taking an action on a virtual private endpoint, such as creating or updating. For instructions, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls#using-acls).

The ACL is on the subnet that holds the reserved IP, which is bound to the endpoint gateway that creates the VPE. You require the same rule on each subnet that has an endpoint connected to the VPE. For example, suppose that you have three reserved IPs (one IP in each zone) connected to the endpoint gateway. You can:

* Use the same ACL on each of the three subnets.

OR

* If you have different ACLs on the three different subnets, make sure to set up a rule on each of ACLs for the same VPE.

## Attaching security groups to endpoint gateways
{: #attaching-sqs-endpoint-gateways}

Attach security groups to endpoint gateways to improve the security of their applications. You can then manage these security groups and tighten their security rules for inbound traffic towards your endpoint gateways.
{: shortdesc}

You can attach security groups when creating an endpoint gateway. After the endpoint gateway is created, you can modify the security groups attached to the endpoint gateway (for example, apply security group rules to an endpoint gateway's data paths).
{: note}

Note the following:

* When you create an endpoint gateway without specifying a security group, the VPC default security group is attached to the endpoint gateway.
* You can specify a maximum of five security groups when creating an endpoint gateway.
* To attach an existing security group to an endpoint gateway, add the endpoint gateway to the security group's array of targets.
* To detach a security group from an endpoint gateway, delete the endpoint gateway from the security group's array of targets.

If you plan to use default security groups for new endpoint gateways, review your default security group rules. If necessary, edit the rules to accommodate your endpoint gateway traffic.
{: tip}

### Security group example
{: #vpe-security-group-example}

For example, configure the following inbound rule, which allows all traffic on port 80 for an HTTP listener (TCP port 80).

| Protocol | Source type | Source | Value |
|-----------|------|------|-------|
| TCP | Any | `0.0.0.0/0` | Port 80 |
{: caption="Table 1. Example configuration information for inbound rules" caption-side="bottom"}
