---

copyright:
  years: 2020, 2022
lastupdated: "2022-08-31"

keywords:

subcollection: vpc

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:term: .term}

# Managing security and compliance with VPC Infrastructure Services
{: #manage-security-compliance}

{{site.data.keyword.vpc_full}} is integrated with the {{site.data.keyword.compliance_short}} to help you manage security and compliance for your organization.
{: shortdesc}

With the {{site.data.keyword.compliance_short}}, you can monitor controls and goals that pertain to {{site.data.keyword.vpc_short}} infrastructure.


## Monitoring security and compliance posture with VPC
{: #monitor-vpc}

As a security or compliance focal, you can use the {{site.data.keyword.vpc_short}} [goals](x2117978){: term} to help ensure that your organization is adhering to the external and internal standards for your industry. By using the {{site.data.keyword.compliance_short}} to validate the resource configurations in your account against a [profile](x2034950){: term}, you can identify potential issues as they arise.

All of the goals for {{site.data.keyword.vpc_short}} are added to the {{site.data.keyword.cloud_notm}} Control Library profile but can also be mapped to other profiles.
{: note}

To start monitoring your resources, check out [Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-getting-started).

### Available goals for VPC
{: #vpc-available-goals}

* Check whether Virtual Private Cloud (VPC) is configured with public gateways that are provisionable only within permitted zones
* Check whether Virtual Private Cloud (VPC) has no subnet with public gateway attached
* Check whether Virtual Private Cloud (VPC) network access control lists don't allow egress from 0.0.0.0/0 to any port
* Check whether Virtual Private Cloud (VPC) network access control lists don't allow ingress from 0.0.0.0/0 to any port
* Check whether Virtual Private Cloud (VPC) has no attached public gateways
* Check whether Virtual Private Cloud (VPC) has no public gateways that are attached at the time of provisioning
* Check whether Virtual Private Cloud (VPC) classic access is disabled
* Check whether Virtual Private Cloud (VPC) network access control lists don't allow ingress from 0.0.0.0/0 to port 3389
* Check whether Virtual Private Cloud (VPC) network access control lists don't allow ingress from 0.0.0.0/0 to port 22
* Check whether Virtual Private Cloud (VPC) security groups have no outbound ports open to the internet (0.0.0.0/0)
* Check whether Virtual Private Cloud (VPC) security groups have no inbound ports open to the internet (0.0.0.0/0)
* Check whether Virtual Private Cloud (VPC) has TLS v1.2 set for all inbound traffic through a load balancer
* Check whether Virtual Private Cloud (VPC) has no rules in the default security group
* Check whether Virtual Private Cloud (VPC) security groups have no inbound rules that specify source IP 0.0.0.0/0 to RDP ports 3389
* Check whether Virtual Private Cloud (VPC) security groups have no inbound rules that specify source IP 0.0.0.0/0 to SSH port 22
* Check whether Security Groups for VPC doesn't allow PING for the default security group
* Check whether Security Groups for VPC doesn't allow SSH for the default security group
* Check whether Security Groups for VPC contains no outbound rules in security groups that specify source IP 8.8.8.8/32 to DNS port 53

### Available goals for VPC load balancers
{: #lb-available-goals}

* Check whether Application Load Balancer for VPC is configured with at least one VPC security group
* Check whether Application Load Balancer for VPC is attached with an Auto Scale for VPC instance group that is provided with health check
* Check whether Application Load Balancer for VPC has subnet identifiers of the workload that are identifiable by the Auto Scale for VPC instance group definition
* Check whether Application Load Balancer for VPC has application port of the workload that is identifiable by the Auto Scale for VPC instance group definition
* Check whether Application Load Balancer for VPC uses HTTPS (SSL and TLS) instead of HTTP
* Check whether Application Load Balancer for VPC is configured to convert HTTP client requests to HTTPS
* Check whether Application Load Balancer for VPC pool uses the HTTPS protocol for HTTPS listeners
* Check whether Application Load Balancer for VPC has a health check protocol that is either HTTP or HTTPS
* Check whether Application Load Balancer for VPC has health check configured when created
* Check whether Application Load Balancer for VPC listener is configured with default pool
* Check whether Application Load Balancer for VPC is configured with multiple members in the pool
* Check whether Application Load Balancer for VPC public access is disabled

### Available goals for VPN gateways
{: #vpn-available-goals}

* Check whether VPN for VPC has a Dead Peer Detection policy that is set to `restart`
* Check whether VPN for VPC authentication is configured with a strong pre-shared key with at least 24 alphanumeric characters
* Check whether VPN for VPC has an IPsec policy that does not have Perfect Forward Secrecy (PFS) disabled
* Check whether VPN for VPC has IPsec policy authentication that is set to `sha256`
* Check whether VPN for VPC has IPsec policy encryption that is not set to `triple_des`
* Check whether VPN for VPC has a Diffie-Hellman group set to at least group #
* Check whether VPN for VPC has Internet Key Exchange (IKE) policy authentication that is set to `sha256`
* Check whether VPN for VPC has Internet Key Exchange (IKE) policy encryption that is not set to `triple_des`

### Available goals for Block Storage
{: #block-storage-available-goals}

* Check whether Block Storage for VPC is enabled with IBM Activity Tracker
* Check whether Block Storage for VPC is enabled with customer-managed encryption and Keep Your Own Key (KYOK)
* Check whether Block Storage for VPC is enabled with customer-managed encryption and Bring Your Own Key (BYOK)
* Check whether Block Storage for VPC is encrypted with customer-managed keys

### Available goals for Snapshots
{: #snapshots-available-goals}

* Check whether Snapshots for VPC is encrypted with customer-managed keys

### Available goals for File Storage
{: #shares-available-goals}

* Check whether File Storage for VPC is enabled with customer-managed encryption and Bring Your Own Key (BYOK).
* Check whether File Storage for VPC is in a resource group other than _Default_.
### Available goals for Virtual Servers
{: #virtual-servers-available-goals}

* Check whether Virtual Servers for VPC instances are identifiable by the workload they are running based on the Auto Scale for VPC instance group definition
* Check whether Virtual Servers for VPC is provisioned from customer-defined list of images
* Check whether Virtual Servers for VPC is provisioned from an encrypted image
* Check whether Virtual Servers for VPC data volumes are enabled with customer-managed encryption.  Supported key managment services are HPCS - Keep Your Own Key (KYOK), and Key Protect for Bring Your Own Key (BYOK) 
* Check whether Virtual Servers for VPC boot volumes are enabled with customer-managed encryption.  Supported key managment services are HPCS - Keep Your Own Key (KYOK), and Key Protect for Bring Your Own Key (BYOK)
* Check whether Virtual Servers for VPC resource group other than "Default" is selected
* Check whether Virtual Servers for VPC instance has all interfaces with IP-spoofing disabled
* Check whether Virtual Servers for VPC instance doesn't have a floating IP
* Check whether Virtual Servers for VPC instance has the minimum # interfaces
* Check whether all network interfaces of a virtual server instance have at least one Virtual Private Cloud (VPC) security group attached
* Check whether all virtual server instances have at least one Virtual Private Cloud (VPC) security group attached

### Available goals for Images
{: #images-available-goals}

* Check whether Images for VPC instance is encrypted with customer-managed key

### Available goals for Flow Logs
{: #flow-logs-available-goals}

* Check whether Flow Logs for VPC are enabled


## Governing VPC resource configuration
{: #govern-vpc}

As a security or compliance focal, you can use the {{site.data.keyword.compliance_short}} to define configuration rules for the VPC resources that you create.

[Config rules](#x3084914){: term} are used to enforce the configuration standards that you want to implement across your accounts. To learn more about the data that you can use to create a rule for VPC resources, review the following table.

To learn more about constructing config rules, check out [Formatting rules and templates](/docs/security-compliance?topic=security-compliance-formatting-rules-templates).

| VPC Service | Resource kind | Property | Operator type | Value | Description |
|-------------|---------------|----------|---------------|-------|-------------|
| Auto Scale | *instance* | *membership_count* | Numeric | *["1"]* | *A number that indicates the total number of instances in the instance group.* |
| Block Storage | *instance* | *user_managed_encryption* | Boolean | - | *A Boolean that indicates whether customer managed key encryption is enabled.* |
| Image | *instance* | *encryption* | String | *["user_managed", "none"]* | *A string that indicates whether customer managed key encryption is enabled.* |
| Load Balancer | *instance* | *profile_family* | String| *["application", "network"]* | *A list of strings that match load balancer profile family name from load balancer profile family.* |
| Load Balancer | *instance* | *load_balancer_type* | String| *["private", "public"]* | *A list of strings that indicate what type of  load balancer can be provisioned.* |
| Virtual Server | *instance* | *floating_ips_allowed* | boolean | *["is_true", "is_false"]* | *A boolean indicating whether or not floating IPs can be associated with the network interfaces of the instance.* |
| Virtual Server | *instance* | *metadata_service_enabled* | boolean | *["is_true", "is_false"]* | *A boolean indicating whether or not the metadata service can be enabled for the instance.* |
{: caption="Table 1. Rule properties for VPC" caption-side="bottom"}

### Example: Governing load balancer resource configuration
{: #govern-lb}

The following example is a rule configuration with only private access enabled:

```
{
  "target": {
    "service_name": "is.load-balancer",
    "resource_kind": "instance",
    "additional_target_attributes": []
  },
  "required_config": {
    "description": "Public access disabled",
    "and": [
      {
        "property": "load_balancer_type",
        "operator": "string_equals",
        "value": "private"
      }
    ]
  }
}
```
{: codeblock}

The following example is a rule configuration that prevents users from creating public type application load balancers:

```
{
  "target": {
    "service_name": "is.load-balancer",
    "resource_kind": "instance",
    "additional_target_attributes": []
  },
  "required_config": {
    "description": "Public access disabled for application load balancer",
    "or": [
      {
        "property": "profile_family",
        "operator": "string_equals",
        "value": "network"
      },
      {
        "and": [
          {
            "property": "profile_family",
            "operator": "string_equals",
            "value": "application"
          },
          {
            "property": "load_balancer_type",
            "operator": "string_equals",
            "value": "private"
          }
        ]
      }
    ]
  }
}
```
{: codeblock}

Make sure that *enforcement_actions* is set with *action: disallow* to apply the rule and meet the compliance criteria. For more information, see [Formatting rules and templates](/docs/security-compliance?topic=security-compliance-formatting-rules-templates).
{: note}

### Checking results for load balancers
{: #results-lb}

As a security or compliance focal, you can use the {{site.data.keyword.compliance_short}} to monitor compliance results for the {{site.data.keyword.alb_full}} and {{site.data.keyword.nlb_full}} instances under your account against the configuration rules.

The evaluation results are only available for a limited time and are updated once a day. It is recommended that reports are downloaded and organized to maintain a history of compliance for audit purposes. For more information about reporting results, see [View evaluation results](https://cloud.ibm.com/security-compliance/compliance-posture/rules).
{: note}
