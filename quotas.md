---

copyright:
  years: 2018, 2022

lastupdated: "2022-06-06"

keywords:

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Quotas and service limits
{: #quotas}

The following information shows quotas and limits for {{site.data.keyword.vpc_full}} and the resources available within it.
{: shortdesc}

## Quotas
{: #vpcquotas}

The following tables show the quotas for various VPC resources.

To increase a quota for a particular resource, [contact support](https://{DomainName}/unifiedsupport/cases/form){: external}.
{: note}

### Compute resources
{: #vsi-quotas}

|   Resource     | Quota |
| ------- | ------ |
| vCPU |  200 per region  |
| RAM | 5600 GB per region |
| Bare metal servers | 25 per account |
| Instance storage | 18 TB per region |
| Floating IP addresses | 40 per zone |
| SSH keys | 200 per account |
| GPU | 16 per account |
| Dedicated host groups | 100 per region | 
{: caption="Table 1. Quotas for virtual server instances" caption-side="bottom"}

When you provision virtual server instances and dedicated hosts, the vCPU associated with these resources counts toward the vCPU quotas per region. Instances that are provisioned on dedicated hosts do not count against the vCPU quota.

When you provision virtual server instances, the RAM and instance storage, along with vCPU, that is associated with the instances count toward the vCPU, RAM, and instance storage quotas per region.

Bare metal servers use physical cores and don't count toward your vCPU quota.
{: note} 

### VPCs
{: #vpc-quotas}

|   Resource     | Quota |
| ------- | ------ |
| Virtual private clouds | 10 per region|
| Subnets | 15 per VPC |
| Address prefixes | 25 per VPC |
{: caption="Table 2. Quotas for the VPC service" caption-side="bottom"}

### Access control lists
{: #acl-quotas}

|Resource|Quota|
|--------|-----|
|ACLs| 25 per VPC |
|Rules| 100 per ACL |
{: caption="Table 3. Quotas for access control lists" caption-side="bottom"}

### Security groups
{: #security-group-quotas}

|Resource|Quota|
|--------|-----|
|Security groups|50 per VPC|
|Rules|50 per security group|
|Network interfaces|1000 per security group|
{: caption="Table 4. Quotas for security groups" caption-side="bottom"}

### VPN gateways (site-to-site)
{: #vpn-quotas}

|Resource|Quota|
|--------|-----|
| VPN gateways| 9 per region, 3 per zone |
| VPN connections | 10 per VPN gateway |
| IKE policies | 20 per region |
| IPSec policies | 20 per region |
| Peer subnets | 50 across all connections of a VPN gateway, 15 per individual VPN connection |
| Local subnets | 50 across all connections of a VPN gateway, 15 per individual VPN connection |
| Route-based VPN gateway | 1 per zone per VPC |
{: caption="Table 5. Quotas for the site-to-site VPN gateway service" caption-side="bottom"}

### VPN servers (client-to-site) BETA
{: #vpn-server-quotas}

|Resource|Quota|
|--------|-----|
| Active connections per VPN server | 2000 |
| Active VPN servers | 10 per region |
| Active routes per VPN server | 50 |
| Number of certificate revocations lists | 20,000 |
| Number of security groups attached on a VPN server | 5 | 
| Number of VPN servers in a security group | 10 |
| Number of auth clients per second per VPN server | 10 | 
{: caption="Table 6. Quotas for the client-to-site VPN server service" caption-side="bottom"} 

### Load balancers
{: #load-balancer-quotas}

|Resource|Quota|
|--------|-----|
| Load balancers | 50 per region |
| Listeners | 10 per load balancer |
| Pools | 10 per load balancer |
| Members | 50 per pool |
| Policies | 10 per listener |
| Rules | 10 per policy |
| Security Groups | 5 per load balancer |
{: caption="Table 7. Quotas for load balancers" caption-side="bottom"}

### Routing tables and routes
{: #routing-tables-routes-quotas}

|Resource|Quota|
|--------|-----|
| Routing tables per VPC | Default limit: 50  \n Maximum limit: 200 |
| Routes per routing table | Default limit: 200  \n Maximum limit: 400 |
{: caption="Table 8. Quotas for routing tables and routes" caption-side="bottom"}

Each route has a destination property, which includes a prefix length (`/24` in `10.2.0.0/24`). The number of unique prefix lengths that are supported per custom routing table is 14. Multiple routes with the same prefix count as only one unique prefix.
{: note}

### Block storage volumes and snapshots
{: #block-storage-quotas}

|Resource|Quota|
|--------|-----|
| Boot and secondary volumes | 300 total VPC volumes per account in a region |
| Snapshots and backup snapshots | 100 per volume in a region |
{: caption="Table 9. Quotas for block storage volumes and snapshots" caption-side="bottom"}

### File shares
{: #file-storage-quotas}

|Resource|Quota|
|--------|-----|
| File shares | 300 total file shares per account in a region |
{: caption="Table 10. Quotas for file shares" caption-side="bottom"}

### Placement groups
{: #placement-group-quotas}

|Resource|Quota|
|--------|-----|
| Placement groups | 100 placement groups per account in a region |
| Instances | 12 instances per placement group per region with host_spread placement group strategy. |
| Instances | 4 instances per placement group per region with power_spread placement group strategy. |
{: caption="Table 11. Quotas for placement groups" caption-side="bottom"}

The quotas for placement groups are set and can't be adjusted.
{: note}

## Service limits
{: #service-limits-for-vpc-services}

The following table displays current VPC service limits. Unlike quotas, these limits can't be adjusted.

|Resource|Limit|
|--------|-----|
| VPCs with classic access | 1 per region|
| Network interfaces | 5 per instance |
| PCI network interfaces for bare metal servers | 8 per bare metal server |
| Public Gateways | 1 per zone per VPC |
| Security groups | 5 per network interface (NIC) on a virtual server instance |
| Remote rules for security groups |5 per security group|
| Secondary volumes per instance | Up to 12 secondary volumes |
| Instance groups for auto scale and more | 200 per account|
| Instance group memberships  | 1000 per instance group|
{: caption="Table 12. Limits for VPC resources" caption-side="bottom"}
