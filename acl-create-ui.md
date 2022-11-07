---

copyright:
  years: 2019, 2022

lastupdated: "2022-11-07"

keywords:  

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating network ACLs 
{: #acl-create-ui}

## Using the UI
{: #configuring-the-acl} 

You can configure the ACL to limit inbound and outbound traffic to the subnet. By default, all traffic is allowed.

Each subnet can be attached to only one ACL. However, each ACL can be attached to multiple subnets.

To configure the ACL:

1. In the navigation pane, click **Network > Subnets**.
1. Click the subnet that you created.
1. In the **Subnet details** area, click the name of the ACL.
1. Click **Add rule** to configure inbound and outbound rules that define what traffic is allowed in or out of the subnet. For each rule, specify the following information:
   * Select whether to allow or deny the specified traffic.
   * Select the protocol to which the rule applies.  
   * For the source and destination of the rule, specify the IP range and ports for which the rule applies. For example, if you want all inbound traffic to be allowed to the IP range 192.168.0.0/24 in your subnet, specify **Any** as the source and 192.168.0.0/24 as the destination. But if you want to allow inbound traffic only from 169.168.0.0/24 to your entire subnet, specify 169.168.0.0/24 as the source and **Any** as the destination for the rule.
   * Specify the rule's priority. Rules with lower numbers are evaluated first and override rules with higher numbers. For example, if a rule with priority 2 allows HTTP traffic and a rule with priority 5 denies all traffic, HTTP traffic is still allowed.  
1. When you finish creating rules, click the **All access control lists** breadcrumb at the beginning of the page.

### Example ACL
{: #ACL-example}

For example, you can configure the following inbound rules:

* Allow HTTP traffic from the internet
* Allow all inbound traffic from the subnet 10.10.20.0/24
* Deny all other inbound traffic  

| Priority| Allow/Deny | Protocol | Source | Destination |
|--------------|-----------|------|------|------|
| 1 | Allow | TCP | Any IP, ports 80 - 80 |Any IP, any port|
| 2 | Allow | ALL | 10.10.20.0/24, any port |Any IP, any port|
| 3 | Deny| ALL | Any IP, any port |Any IP, any port|
{: caption="Table 1. Information for configuring inbound rules" caption-side="top"}

Then, configure the following outbound rules:

* Allow HTTP traffic to the internet
* Allow all outbound traffic to the subnet 10.10.20.0/24
* Deny all other outbound traffic  

| Priority| Allow/Deny | Protocol | Source | Destination |
|--------------|-----------|------|------|------|
| 1 | Allow | TCP | Any IP, any port |Any IP, ports 80 - 80  |
| 2 | Allow | ALL | Any IP, any port | 10.10.20.0/24, any port |
| 3 | Deny| ALL | Any IP, any port |Any IP, any port|
{: caption="Table 2. Information for configuring outbound rules" caption-side="top"}
