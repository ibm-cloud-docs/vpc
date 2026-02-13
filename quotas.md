---

copyright:
  years: 2018, 2026
lastupdated: "2026-02-13"

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

To increase a quota for a particular resource, [contact support](/unifiedsupport/cases/form){: external}.
{: note}

### Compute resources
{: #vsi-quotas}

| Resource | Quota |
| ------- | ------ |
| vCPU | 200 per region  |
| RAM | 5600 GB per region |
| Bare metal servers | 25 per account  |
| Instance storage | 18 TB per region |
| SSH keys | 200 per region |
| GPU | 16 per region |
| Dedicated host groups | 100 per region |
| Storage optimized (ox2) instance storage | 96 TB per region |
{: caption="Quotas for virtual server instances" caption-side="bottom"}

When you provision virtual server instances dedicated hosts, the vCPU, RAM, and solid-state drives, which are associated with the instances count toward the vCPU, RAM, and instance storage quotas per region. Instances that are provisioned on dedicated hosts do not count against the vCPU quota. For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

Reservations for virtual servers count against the vCPU, RAM, instance storage, Storage optimized (ox2) instance storage, and GPU quotas.

Bare metal servers use physical cores and don't count toward your vCPU quota.
{: note}

### VPCs
{: #vpc-quotas}

|   Resource     | Quota |
| ------- | ------ |
| Virtual private clouds | 10 per region|
| Subnets | 100 per VPC |
| Address prefixes | 25 per VPC |
| Service IPs | 1 per zone per VPC |
{: caption="Quotas for the VPC service" caption-side="bottom"}


### Access control lists
{: #acl-quotas}

| Resource | Quota |
|--------|-----|
| ACLs | 100 per VPC |
| Rules | 200 per ACL |
{: caption="Quotas for access control lists" caption-side="bottom"}

### Application load balancers
{: #alb-quotas}

| Resource | Quota |
|--------|-----|
| Load balancers | 50 per region |
| Listeners | 10 per load balancer |
| Pools | 10 per load balancer |
| Members | 50 per pool |
| Policies | 10 per listener |
| Rules | 10 per policy |
| Subnets | 15 per load balancer |
{: caption="Quotas for load balancers" caption-side="bottom"}

### Cluster networks
{: #cluster-networks-quotas}

| Resource | Quota |
|--------|-----|
| Maximum number of cluster networks per account per region | 5 |
| Maximum number of cluster network subnets per cluster network | 32 |
| Maximum number of cluster network subnet reserved IP addresses per account | 5000 |
{: caption="Quotas for cluster networks" caption-side="bottom"}

### Floating IPs
{: #fips-quotas}

| Resource | Quota |
|--------|-----|
| Floating IP addresses | 40 per zone |
{: caption="Quotas for floating IPs" caption-side="bottom"}

### Network load balancers
{: #nlb-quotas}

| Resource | Quota |
|--------|-----|
| Load balancers | 50 per region |
| Listeners | 10 per load balancer |
| Pools | 10 per load balancer |
| Members | 50 per pool |
| Policies | N/A |
| Rules | N/A |
| Subnets | 1 per load balancer |
{: caption="Quotas for load balancers" caption-side="bottom"}

### Private Path network load balancers
{: #ppnlb-quotas}

| Resource | Quota |
|--------|-----|
| Load balancers | 50 per region |
| Listeners | 10 per load balancer |
| Pools | 10 per load balancer |
| Members | 50 per pool (if pool member targets an an ALB, only a single member is allowed) |
{: caption="Quotas for Private Path load balancers" caption-side="bottom"}

### Public address ranges
{: #par-quotas}

| Resource | Quota |
|--------|-----|
| Maximum number of public address ranges per account per region | 5 |
| Prefix size | `/32` to `/28` |
{: caption="Quotas for public address ranges" caption-side="bottom"}

### Reserved IP addresses
{: #reserved-ip-quotas}

| Resource | Quota |
|--------|-----|
| Reserved IP addresses | 20,000 per region |
{: caption="Quotas for reserved IP addresses" caption-side="bottom"}

### Routing tables and routes
{: #routing-tables-routes-quotas}

| Resource | Quota |
|--------|-----|
| Routing tables per VPC | 50 |
| Routes per routing table | 200 |
| Advertise routes per VPC | 30|
{: caption="Quotas for routing tables and routes" caption-side="bottom"}

Each route has a destination property, which includes a prefix length (`/24` in `10.2.0.0/24`). The number of unique prefix lengths that are supported per custom routing table is 14. Multiple routes with the same prefix count as only one unique prefix.
{: note}



### Security groups
{: #security-group-quotas}

| Resource | Quota |
|--------|-----|
| Security groups | 100 per VPC |
| Rules | 250 per security group |
| [Targets](/docs/vpc?topic=vpc-using-security-groups#about-security-group-targets) | 1000 per security group |
{: caption="Quotas for security groups" caption-side="bottom"}

### Virtual network interfaces
{: #virtual-network-interfaces-quotas}

| Resource | Quota |
|--------|-----|
| IP addresses | 8 per virtual network interface |
{: caption="Quotas for virtual network interfaces" caption-side="bottom"}

### VPN gateways (site-to-site)
{: #vpn-quotas}

| Resource | Quota | Supports Policy Mode | Supports Route Mode|
|--------|--------|----------|----------|
| VPN gateways| 9 per region, 3 per zone | Yes | Yes[^fn1]|
| VPN connections | 10 per VPN gateway | Yes | Yes |
| IKE policies | 20 per region | Yes| Yes |
| IPsec policies | 20 per region | Yes | Yes |
| Peer and local subnets | 50 across all connections per gateway, 15 per connection | Yes | No |
| User defined advertised routes | 10 per VPN gateway | No | Yes[^fn2] |
{: caption="Quotas for the site-to-site VPN gateway service" caption-side="bottom"}

[^fn1]: A single VPC supports a maximum of one route-mode VPN per zone.
[^fn2]: This resource and quota is applicable only to dynamic route-based VPN connection.

### VPN servers (client-to-site)
{: #vpn-server-quotas}

| Resource | Quota |
|--------|-----|
| Active connections per VPN server | 2000 |
| Active VPN servers | 10 per region |
| Active routes per VPN server | 50 |
| Number of certificate revocations lists | 20000 |
| Number of VPN servers in a security group | 10 |
| Number of auth clients per second per VPN server | 10 |
{: caption="Quotas for the client-to-site VPN server service" caption-side="bottom"}

### Block Storage volumes and snapshots
{: #block-storage-quotas}

| Resource | Quota |
|----------|-------|
| Boot and secondary volumes | 300 total VPC volumes per region. |
| Snapshots and backup snapshots | Up to 750 snapshots per first-generation storage volume in a region. \n Up to 512 snapshots per second-generation storage volume in a region.|
{: caption="Quotas for Block Storage volumes and snapshots" caption-side="bottom"}

### File shares and snapshots
{: #file-storage-quotas}

| Resource | Quota |
|----------|-------|
| File shares | 300 file shares per account, across all VPCs |
| Mount targets | 256 per file share per account per zone. |
| Accessor share bindings | A file share can have a maximum of 100 accessor bindings.|
| Snapshots and backup snapshots | Zonal file shares can have up to 750 snapshots per share in a region. \n In the current release of Regional file shares, you can create up to 30 snapshots per share in a region. |
| Snapshots and backup snapshots | The total snapshot size that is allocated to a Zonal file share can't exceed 8 times the size of the share. \n In the current release of Regional file shares, there are no limitations on snapshot size allocation.|
{: caption="Quotas for file shares" caption-side="bottom"}

### Backup policies and plans
{: #backup-quotas}

| Resource | Quota |
|----------|-------|
| Backup policies |  You can create up to 10 backup policies per region. This quota can't be increased. |
| File shares | The cumulative size of all backups for a share can't exceed 100 TB. |
| Block volumes | The cumulative size of all backups for a first-generation storage volume can't exceed 10 TB. \n You can create backup snapshots of second-generation storage volumes up to 32 TB. |
| Retention period | You can keep your backup snapshots for up to 1000 days. |
{: caption="Quotas for file shares" caption-side="bottom"}

### Placement groups
{: #placement-group-quotas}

| Resource | Quota |
|--------|-----|
| Placement groups | 100 placement groups per region |
| Instances (*host_spread*) | 12 instances per placement group per region with `host_spread` placement group strategy. |
| Instances (*power_spread*) | 4 instances per placement group per region with `power_spread` placement group strategy. |
{: caption="Quotas for placement groups" caption-side="bottom"}

The quotas for placement groups are preset and can't be adjusted.
{: note}

## Service limits
{: #service-limits-for-vpc-services}

The following table displays current VPC service limits. Unlike quotas, these limits can't be adjusted.

| Resource | Limit |
|--------|-----|
| VPCs with classic access | 1 per region|
| Network interfaces | 5, 10, or 15 (depending on the size of the instance) per instance | 
| PCI network interfaces for bare metal servers | 8 per bare metal server |
| Public address ranges | 10 per VPC per zone |
| Public gateways | 1 per zone per VPC |
| Secondary volumes per instance | Up to 12 secondary volumes |
| Security groups | 5 per [target](/docs/vpc?topic=vpc-using-security-groups#about-security-group-targets) |
| Image export jobs | 5 active jobs per image, 10 total per image, 20 active jobs per account, per region|
| Instance groups for auto scale and more | 200 per account |
| Instance group memberships  | 1000 per instance group|
{: caption="Limits for VPC resources" caption-side="bottom"}
