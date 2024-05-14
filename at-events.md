---

copyright:
  years: 2019, 2024

lastupdated: "2024-05-15"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Activity Tracker events
{: #at-events}

As a security officer, auditor, or manager, you can use the {{site.data.keyword.at_full}} service to track how users and applications interact with {{site.data.keyword.cloud}} Virtual Private Cloud (VPC).
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started).

## Network resources
{: #events-network}

The following tables list the actions that are related to network resources and the generation of events.

### ACL events
{: #events-network-acl}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| network-acl  | is.network-acl.network-acl.create   | Network ACL was created  |
| network-acl  | is.network-acl.network-acl.update   | Network ACL was updated  |
| network-acl  | is.network-acl.network-acl.delete   | Network ACL was deleted  |
| network-acl  | is.network-acl.network-acl.read | One or more network ACL was retrieved |
| network-acl  | is.network-acl.rule.create  | Rule was added to Network ACL  |
| network-acl  | is.network-acl.rule.update  | Network ACL Rule was updated   |
| network-acl  | is.network-acl.rule.delete  | Rule was removed from Network ACL  |
| network-acl  | is.network-acl.rule.read | One or more network ACL rules was retrieved |
{: caption="Table 1. Actions that generate events for Network ACL" caption-side="bottom"}

### Custom route events
{: #events-custom-routes}

The following table lists the actions that are related to VPC routing tables and routes.

| Resource | Action | Description |
|---|---|---|
| routing table | is.vpc.routing-table.create  | Routing table was created |
| routing table | is.vpc.routing-table.update  | Routing table was updated |
| routing table | is.vpc.routing-table.delete | Routing table was deleted |
| routing table | is.vpc.routing-table.read | Routing table was retrieved (get and list) |
| route | is.vpc.routing-table_route.create | Routing table route was created |
| route | is.vpc.routing-table_route.update | Routing table route was updated |
| route | is.vpc.routing-table_route.delete | Routing table route was deleted |
| route | is.vpc.routing-table_route.read | Routing table route was retrieved (get and list) |
| subnet | is.subnet.routing-table.read | Subnets attached to a routing table were retrieved |
| subnet | is.subnet.routing-table.attach | Routing table was attached to a subnet |
{: caption="Table 2. Actions that generate events for VPC routing tables and routes" caption-side="bottom"}

### DNS resolution binding events
{: #events-dns-resolution-bindings}

The following table lists the actions that are related to DNS resolution bindings.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| DNS resolution binding | `is.vpc.dns-resolution-binding.create` | Creates a DNS resolution binding. Generates events for both hub VPC and DNS-shared VPCs. |
| DNS resolution binding | `is.vpc.dns-resolution-binding.update` | Updates the name of a DNS resolution binding. |
| DNS resolution binding | `is.vpc.dns-resolution-binding.delete` | Deletes a DNS resolution binding. Generate events for both hub VPC and DNS-shared VPCs. |
| DNS resolution binding | `is.vpc.dns-resolution-binding.read` | Gets details of a DNS resolution binding. |
| DNS resolution binding | `is.vpc.dns-resolution-binding.list` | Lists DNS resolution bindings. |
{: caption="Table 3. Actions that generate events for DNS resolution bindings" caption-side="bottom"}

Although the DNS resolution binding is created or deleted by the DNS-shared VPC authorized user, the system is actually creating or deleting the DNS resolution binding objects for both the hub VPC and DNS-shared VPCs. Therefore, the system also generates the Activity Tracker event for both hub and DNS-shared VPCs.
{: note}

Existing events include a new attribute:

`is.vpc.vpc.{create,update}`
:    Includes a `dns` property and its object value in request data.

`is.vpc.vpc.{read,list}`
:    Includes a `dns` property and its object value in response data.

`is.endpoint-gateway.endpoint-gateway.{create,update}`
:    Includes an `allow_dns_resolution_binding property` and its boolean value in both request and response data.

`is.endpoint-gateway.endpoint-gateway.{list,read}`
:    Includes an `allow_dns_resolution_binding property` and its boolean value in response data.

### Floating IP events
{: #events-network-floatingIP}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| floating-ip  | is.floating-ip.floating-ip.create   | Floating IP was created  |
| floating-ip  | is.floating-ip.floating-ip.update   | Floating IP was updated  |
| floating-ip  | is.floating-ip.floating-ip.delete   | Floating IP was deleted  |
| floating-ip  | is.floating-ip.floating-ip.read | One or more floating IP was retrieved |
{: caption="Table 4. Actions that generate events for Floating IP" caption-side="bottom"}

### Flow log events
{: #events-flow-logs}

The following table lists the actions that are related to flow logs and the generation of events.

| Resource | Action | Description |
|---|---|---|
| flow-log-collector | is.flow-log-collector.flow-log-collector.create | Flow log collector was created |
| flow-log-collector | is.flow-log-collector.flow-log-collector.delete | Flow log collector was deleted |
| flow-log-collector | is.flow-log-collector.flow-log-collector.read | Flow log collector was read |
| flow-log-collector | is.flow-log-collector.flow-log-collector.update | Flow log collector was updated |
{: caption="Table 5. Actions that generate events for flow log collectors" caption-side="bottom"}

### Load balancer events
{: #events-load-balancers}

The following table lists the actions that are related to load balancers and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| Load balancer |  is.load-balancer.load-balancer.create | Load balancer was created |
| Load balancer |  is.load-balancer.load-balancer.update | Load balancer was updated |
| Load balancer |  is.load-balancer.load-balancer.delete | Load balancer was deleted |
| Load balancer |  is.load-balancer.load-balancer.read | Load balancer was read |
| Listener |  is.load-balancer.load-balancer-listener.create | Listener was created |
| Listener |  is.load-balancer.load-balancer-listener.update | Listener was updated |
| Listener |  is.load-balancer.load-balancer-listener.delete | Listener was deleted |
| Listener |  is.load-balancer.load-balancer-listener.read | Listener was read |
| Pool |  is.load-balancer.load-balancer-pool.create | Pool was created |
| Pool |  is.load-balancer.load-balancer-pool.update | Pool was updated |
| Pool |  is.load-balancer.load-balancer-pool.delete | Pool was deleted |
| Pool |  is.load-balancer.load-balancer-pool.read | Pool was read |
| Member |  is.load-balancer.load-balancer-pool-member.create | Member was created |
| Member |  is.load-balancer.load-balancer-pool-member.update | Member was updated |
| Member |  is.load-balancer.load-balancer-pool-member.delete | Member was deleted |
| Member |  is.load-balancer.load-balancer-pool-member.read | Member was read |
| Policy |  is.load-balancer.load-balancer-listener-policy.create | Policy was created |
| Policy |  is.load-balancer.load-balancer-listener-policy.update | Policy was updated |
| Policy |  is.load-balancer.load-balancer-listener-policy.delete | Policy was deleted |
| Policy |  is.load-balancer.load-balancer-listener-policy.read | Policy was read |
| Rule |  is.load-balancer.load-balancer-listener-policy-rule.create | Rule was created |
| Rule |  is.load-balancer.load-balancer-listener-policy-rule.update | Rule was updated |
| Rule |  is.load-balancer.load-balancer-listener-policy-rule.delete | Rule was deleted |
| Rule |  is.load-balancer.load-balancer-listener-policy-rule.read | Rule was read |
{: caption="Table 6. Actions that generate events for load balancers" caption-side="bottom"}

### Private Path service events
{: #events-private-path-service-events}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| private-path-service-gateway | is.private-path-service-gateway.private-path-service-gateway.read | One or more Private Path service gateway was retrieved |
| private-path-service-gateway | is.private-path-service-gateway.private-path-service-gateway.list | Private Path service gateway was listed |
| private-path-service-gateway | is.private-path-service-gateway.private-path-service-gateway.update | Private Path service gateway was updated |
| private-path-service-gateway | is.private-path-service-gateway.private-path-service-gateway.create  /n is.private-path-service-gateway.load-balancer.attach  /n is.load-balancer.private-path-service-gateway.attach | Private Path service gateway was created |
| private-path-service-gateway | is.private-path-service-gateway.private-path-service-gateway.delete  /n is.private-path-service-gateway.load-balancer.detach  /n is.load-balancer.private-path-service-gateway.detach | Private Path service gateway was deleted |
| private-path-service-gateway | is.private-path-service-gateway.private-path-service-gateway.publish | Private Path service gateway was published |
| private-path-service-gateway | is.private-path-service-gateway.load-balancer.detach  /n is.is.load-balancer.private-path-service-gateway.detach | Private Path service gateway load balancer was deleted |
| private-path-service-gateway | is.private-path-service-gateway.load-balancer.attach  /n is.load-balancer.private-path-service-gateway.attach | Private Path service gateway load balancer was attached |
| private-path-service-gateway | is.private-path-service-gateway.private-path-service-gateway.revoke-account  /n is.private-path-service-gateway.endpoint-gateway-binding.deny | Private Path service gateway account was revoked |
| private-path-service-gateway | is.private-path-service-gateway.endpoint-gateway-binding.read | Private Path service gateway consumer connection request was retrieved |
| private-path-service-gateway | is.private-path-service-gateway.endpoint-gateway-binding.list | Private Path service gateway consumer connection requests were listed |
| private-path-service-gateway | is.private-path-service-gateway.endpoint-gateway-binding.permit | Private Path service gateway consumer connection request was permitted |
| private-path-service-gateway | is.private-path-service-gateway.endpoint-gateway-binding.deny | Private Path service gateway consumer connection request was denied |
| private-path-service-gateway | is.private-path-service-gateway.endpoint-gateway-binding.create | Private Path service gateway consumer connection has been created |
| private-path-service-gateway | is.private-path-service-gateway.endpoint-gateway-binding.read | Private Path service gateway consumer connection request has been retrieved |
| private-path-service-gateway | is.private-path-service-gateway.endpoint-gateway-binding.delete | Private Path service gateway consumer connection has been deleted |
| private-path-service-gateway | is.private-path-service-gateway.account-policy.read | Private Path service gateway account policy has been retrieved |
| private-path-service-gateway | is.private-path-service-gateway.account-policy.list | Private Path service gateway account policy has been listed |
| private-path-service-gateway | is.private-path-service-gateway.account-policy.create | Private Path service gateway account policy has been created |
| private-path-service-gateway | is.private-path-service-gateway.account-policy.delete | Private Path service gateway account policy has been deleted |
| private-path-service-gateway | is.private-path-service-gateway.account-policy.update | Private Path service gateway account policy has been updated |
{: caption="Table 7. Actions that generate events for Private Path services" caption-side="bottom"}

### Public gateway events
{: #events-network-public-gateway}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| public-gateway | is.public-gateway.public-gateway.create   | Public Gateway was created   |
| public-gateway | is.public-gateway.public-gateway.update   | Public Gateway was updated   |
| public-gateway | is.public-gateway.public-gateway.delete   | Public Gateway was deleted   |
| public-gateway | is.public-gateway.public-gateway.read | One or more public gateways were retrieved  |
{: caption="Table 8. Actions that generate events for Public Gateway" caption-side="bottom"}

### Reserved IPs
{: #events-reserved-ips}

| Resource | Action  | Description  |
|:-------------|:-----------------------------------|:-------------|
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.create is.endpoint-gateway.endpoint-gateway.attach is.subnet.reserved-ip.create is.subnet.reserved-ip.attach is.security-group.security-group.attach | Endpoint gateway was created |
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.detach is.subnet.reserved-ip.detach is.subnet.reserved-ip.delete | Reserved IP was unbound from an endpoint gateway |
| endpoint-gateway | is.subnet.reserved-ip.read | Reserved IP bound to an endpoint gateway was retrieved |
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.attach is.subnet.reserved-ip.attach | Reserved IP was bound to an endpoint gateway |
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.delete is.subnet.reserved-ip.detach is.endpoint-gateway.endpoint-gateway.detach is.subnet.reserved-ip.delete is.security-group.security-group.detach | Endpoint gateway was deleted |
| instance | is.instance.instance.create is.subnet.reserved-ip.create is.subnet.reserved-ip.attach is.instance.network-interface.attach | Instance was created |
| instance | is.instance.instance.delete is.subnet.reserved-ip.detach is.subnet.reserved-ip.delete is.instance.network-interface.detach | Instance was deleted |
| instance | is.instance.network-interface.create is.subnet.reserved-ip.create is.instance.network-interface.attach is.subnet.reserved-ip.attach | Network interface was created on an instance |
| instance | is.instance.network-interface.delete is.subnet.reserved-ip.detach is.subnet.reserved-ip.delete is.instance.network-interface.detach | Network interface was deleted |
| instance | is.subnet.reserved-ip.read | Reserved IPs bound to a network interface were listed |
| instance | is.subnet.reserved-ip.read | Bound reserved IP was retrieved |
| subnet | is.subnet.reserved-ip.read | Reserved IPs in a subnet were listed |
| subnet | is.subnet.reserved-ip.create is.subnet.subnet.update is.endpoint-gateway.endpoint-gateway.attach is.subnet.reserved-ip.attach | IP in a subnet was reserved |
| subnet | is.subnet.subnet.update is.subnet.reserved-ip.delete | Reserved IP was released |
| subnet | is.subnet.reserved-ip.read  | Reserved IP was retrieved |
| subnet | is.subnet.reserved-ip.update | Reserved IP was updated |
{: caption="Table 9. Actions that generate events for Reserved IP addresses" caption-side="bottom"}

### Security group events
{: #events-network-security-group}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| security-group | is.security-group.security-group.create   | Security Group was created   |
| security-group | is.security-group.security-group.delete   | Security Group was deleted   |
| security-group | is.security-group.security-group.update   | Security Group was updated   |
| security-group | is.security-group.security-group.read | One or more security groups were retrieved |
| security-group | is.security-group.security-group-rule.create  | Rule was added to Security Group  |
| security-group | is.security-group.security-group-rule.delete  | Rule was removed from Security Group  |
| security-group | is.security-group.security-group-rule.update  | Security Group Rule was updated  |
| security-group | is.security-group.security-group-rule.read | One or more security group rules was retrieved |
| security-group | is.security-group.security-group-interface.attach | Interface was attached to Security Group   |
| security-group | is.security-group.security-group-interface.detach | Interface was removed from Security Group   |
| security-group | is.security-group.security-group-interface.read | One or more security group interfaces was retrieved |
{: caption="Table 10. Actions that generate events for Security Group" caption-side="bottom"}

### Subnet events
{: #events-network-subnet}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| subnet   | is.subnet.subnet.create   | Subnet was created   |
| subnet   | is.subnet.subnet.update   | Subnet was updated   |
| subnet   | is.subnet.subnet.delete   | Subnet was deleted   |
| subnet   | is.subnet.subnet.read | One or more subnets was retrieved |
| subnet   | is.subnet.network-acl.update  | Subnet's Network ACL was replaced   |
| subnet   | is.subnet.public-gateway.attach  | Public Gateway was attached to Subnet  |
| subnet   | is.subnet.public-gateway.detach  | Public Gateway was detached from Subnet  |
| subnet   | is.subnet.public-gateway.read | A subnet public-gateway attachment was retrieved |
{: caption="Table 11. Actions that generate events for Subnet" caption-side="bottom"}

### Virtual network interface events
{: #events-vni}

The following table lists the actions that are related to virtual network interfaces and the generation of events.

| Resource | Action | Description |
|---|---|---|
| virtual-network-interface | is.virtual-network-interface.virtual-network-interface.create | Virtual network interface was created |
| virtual-network-interface | is.virtual-network-interface.virtual-network-interface.attach | Virtual network interface was attached |
| virtual-network-interface | is.virtual-network-interface.virtual-network-interface.detach | Virtual network interface was detached |
| virtual-network-interface | is.virtual-network-interface.virtual-network-interface.update | Virtual network interface was updated |
| virtual-network-interface | is.virtual-network-interface.virtual-network-interface.delete | Virtual network interface was deleted |
{: caption="Table 12. Actions that generate events for virtual network interfaces" caption-side="bottom"}

### Virtual network interface target resource events
{: #events-vni-target-resource-events}

The following table lists the actions that are related to the target resources of virtual network interfaces and the generation of events.

| Resource | Action | Description |
|---|---|---|
| virtual-network-interface | is.bare-metal-server.network-attachment.attach | Virtual network interface was attached to a bare metal server |
| virtual-network-interface | is.bare-metal-server.network-attachment.detach | Virtual network interface was detached from a bare metal server|
| virtual-network-interface | is.share.mount-target.attach | Virtual network interface was attached to a file share mount target |
| virtual-network-interface | is.share.mount-target.detach | Virtual network interface was detached from a file share mount target|
| virtual-network-interface | is.instance.network-attachment.attach | Virtual network interface was attached to a virtual server instance|
| virtual-network-interface | is.instance.network-attachment.detach | Virtual network interface was detached from a virtual server instance |
{: caption="Table 13. Actions that generate events for targets of virtual network interfaces" caption-side="bottom"}

### Virtual private endpoint events
{: #events-vpe}

The following table lists the actions that are related to virtual private endpoints and the generation of events.

| Resource | Action | Description |
|---|---|---|
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.create | Endpoint gateway was created |
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.delete | Endpoint gateway was deleted |
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.update | Endpoint gateway was updated |
{: caption="Table 14. Actions that generate events for virtual private endpoints" caption-side="bottom"}

### VPC events
{: #events-network-vpc}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| vpc  | is.vpc.vpc.create   | VPC was created  |
| vpc  | is.vpc.vpc.update   | VPC was updated  |
| vpc  | is.vpc.vpc.delete   | VPC was deleted  |
| vpc  | is.vpc.vpc.read  | One or more VPC was retrieved |
| vpc  | is.vpc.address-prefix.create  | Address Prefix was added to VPC  |
| vpc  | is.vpc.address-prefix.update  | VPC Address Prefix was updated   |
| vpc  | is.vpc.address-prefix.delete  | Address Prefix was removed from VPC  |
| vpc  | is.vpc.address-prefix.read | One or more address prefixes were retrieved |
| vpc  | is.vpc.vpc-route.create   | Route was added to VPC   |
| vpc  | is.vpc.vpc-route.update   | VPC Route was updated  |
| vpc  | is.vpc.vpc-route.delete   | Route was removed from VPC   |
{: caption="Table 15. Actions that generate events for VPC" caption-side="bottom"}

### VPN gateway events
{: #events-vpns}

The following table lists the actions that are related to site-to-site VPN gateways and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| vpn  | is.vpn.vpn-gateway.create   | VPN gateway was created |
| vpn  | is.vpn.vpn-gateway.delete   | VPN gateway was deleted |
| vpn  | is.vpn.vpn-gateway.update   | VPN gateway was updated |
| vpn  | is.vpn.vpn-gateway.read   | VPN gateway was retrieved |
| vpn  | is.vpn.vpn-gateway.list   | VPN gateways were listed |
| vpn  | is.vpn.vpn-connection.create   | VPN connection was created on VPN gateway |
| vpn  | is.vpn.vpn-connection.delete   | VPN connection was deleted from VPN gateway |
| vpn  | is.vpn.vpn-connection.update   | VPN connection was updated on VPN gateway |
| vpn  | is.vpn.vpn-connection.read   | VPN connection was retrieved from VPN gateway |
| vpn  | is.vpn.vpn-connection.list   | VPN gateway connections were listed |
| vpn  | is.vpn.vpn-connection_local-cidr.create | Local subnet was created on VPN gateway connection |
| vpn  | is.vpn.vpn-connection_local-cidr.delete | Local subnet was deleted from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_local-cidr.read   | Local subnet was retrieved from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_local-cidr.list   | Local subnets were listed from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_peer-cidr.create  | Peer subnet was created on VPN gateway connection |
| vpn  | is.vpn.vpn-connection_peer-cidr.delete  | Peer subnet was deleted from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_peer-cidr.read  | Peer subnet was retrieved from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_peer-cidr.list  | Peer subnets were listed from VPN gateway connection |
| vpn  | is.vpn.ike-policy.create   | IKE policy was created |
| vpn  | is.vpn.ike-policy.delete   | IKE policy was deleted |
| vpn  | is.vpn.ike-policy.update   | IKE policy was updated |
| vpn  | is.vpn.ike-policy.read   | IKE policy was retrieved |
| vpn  | is.vpn.ike-policy.list   | IKE policies were listed |
| vpn  | is.vpn.ipsec-policy.create   | IPsec policy was created |
| vpn  | is.vpn.ipsec-policy.delete   | IPsec policy was deleted |
| vpn  | is.vpn.ipsec-policy.update   | IPsec policy was updated |
| vpn  | is.vpn.ipsec-policy.read   | IPsec policy was retrieved |
| vpn  | is.vpn.ipsec-policy.list   | IPsec policies were listed |
{: caption="Table 16. Actions that generate events for site-to-site VPN gateways" caption-side="bottom"}

### VPN server events
{: #events-vpn-server}

The following table lists the actions that are related to client-to-site VPN servers and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| vpn  | is.vpn-server.vpn-server.create   | VPN server was created |
| vpn  | is.vpn-server.vpn-server.delete   | VPN server was deleted |
| vpn  | is.vpn-server.vpn-server.update   | VPN server was updated |
| vpn  | is.vpn-server.vpn-server.read   | VPN server was retrieved |
| vpn  | is.vpn-server.vpn-server.list   | VPN servers were listed |
| vpn  | is.vpn-server.vpn-server-configuration.read | VPN server configuration was downloaded |
| vpn  | is.vpn-server.vpn-server-client.create | VPN client was connected to a VPN server |
| vpn  | is.vpn-server.vpn-server-client.delete | VPN client was disconnected and deleted from a VPN server |
| vpn  | is.vpn-server.vpn-server-client.read   | VPN client was retrieved from a VPN server |
| vpn  | is.vpn-server.vpn-server-client.list   | VPN server clients were listed |
| vpn  | is.vpn-server.vpn-server-client.disconnect | VPN client was disconnected from a VPN server, and deleted according to the VPN server's auto-deletion policy |
| vpn  | is.vpn-server.vpn-server-route.create  | VPN server route was created |
| vpn  | is.vpn-server.vpn-server-route.delete   | VPN server route was deleted |
| vpn  | is.vpn-server.vpn-server-route.update  | VPN server route was updated |
| vpn  | is.vpn-server.vpn-server-route.read  | VPN server route was retrieved |
| vpn  | is.vpn-server.vpn-server-route.list  | VPN server routes were listed |
{: caption="Table 17. Actions that generate events for client-to-site VPN servers" caption-side="bottom"}

In client certificate authentication mode or client certificate and User ID/passcode authentication mode, authentication failure due to an invalid client certificate will not generate an activity track event.
{: note}

## Compute resources
{: #events-compute}

The following tables list the actions that are related to compute resources and the generation of events.

### Instance events
{: #events-compute-instance}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| instance   | is.instance.instance.create   | - Instance was created \n - Includes the state of the `allow_ip_spoofing` parameter, which disables source/destination checks for network interfaces that are created on the virtual server. When set to `false`, IP spoofing is not allowed on the interface.  |
| instance   | is.instance.instance.delete   | Instance was deleted   |
| instance   | is.instance.instance.update   | Instance was updated   |
| instance   | is.instance.instance.read | One or more instances was retrieved |
| instance   | is.instance.action.create   | Instance action was created  |
| instance   | is.instance.action.delete   | Pending instance action was deleted  |
| instance   | is.instance.instance.start  | Instance was started     |
| instance   | is.instance.instance.stop   | Instance was stopped     |
| instance   | is.instance.console-access-token.create   | A console access token was created   |
| instance   | is.instance.console.read   | Connection to the console was retrieved   |
| instance   | is.instance.instance.volume-stop | Instance was stopped because a volume was suspended when its encryption key was deleted  |
| instance   | is.instance.instance-template.create   | Instance template was created  |
| instance   | is.instance.instance-template.delete   | Instance template was deleted  |
| instance   | is.instance.instance-template.update   | Instance template was updated     |
| instance   | is.instance.instance-template.read     | Instance template was retrieved     |
| instance   | is.instance.interface.attach  | Floating IP was associated to instance network interface  |
| instance   | is.instance.interface.detach  | Floating IP was disassociated from instance network interface |
| instance   | is.instance.volume-attachment.create   | Instance volume attachment was created  |
| instance   | is.instance.volume-attachment.delete   | Instance volume attachment was deleted  |
| instance   | is.instance.volume-attachment.update   | Instance volume attachment was updated  |
| instance   | is.instance.volume-attachment.read | One or more instance volume attachments was retrieved |
| instance   | is.instance.network-interface.create   | - Instance network interface was created (Instance was attached to a subnet) \n - Includes the state of the `allow_ip_spoofing` parameter, which disables source/destination checks for network interfaces that are created on the virtual server. When set to `false`, IP spoofing is not allowed on the interface.  |
| instance   | is.instance.network-interface.update   | - Instance network interface was updated (Instance was attached to a subnet) \n - Includes the state of the `allow_ip_spoofing` parameter, which disables source/destination checks for network interfaces that are created on the virtual server. When set to `false`, IP spoofing is not allowed on the interface. |
| instance   | is.instance.network-interface.delete   | Instance network interface was deleted (Instance was detached from a subnet)  |
| instance | is.instance.network-interface.read | One or more instance network interfaces was retrieved |
| instance | is.instance.disk.read | One or more instance storage disks was retrieved |
| instance | is.instance.disk.update | Instance storage disk name was updated |
| instance | is.instance.disk.wipe | Instance storage disk was wiped clean |
| instance | is.instance.gpu.wipe | Memory was wiped on the GPU for the Instance |
{: caption="Table 18. Actions that generate events for Instance" caption-side="bottom"}

### Instance Metadata service events
{: #events-metadata}

The instance metadata service events are undergoing changes and should not be used for automation. They are useful, however, for audit purposes.
{: note}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| Instance Metadata | is.metadata.jwt-token.create | An instance identity token was created for accessing the Metadata service |
| Instance Metadata | is.metadata.computeresource-token.request | Synchronizes the metadata service and IAM activity tracker events when the requested instance identity token is created. |
| Instance Metadata | is.metadata.instance.initialization | Initialization information was retrieved for the calling instance  |
| Instance Metadata | is.metadata.instance.read | Metadata information was retrieved for the calling instance  |
| Instance Metadata | is.metadata.instance-key.get | A public SSH key was retrieved for the calling instance |
| Instance Metadata | is.metadata.instance-key.list | Public SSH keys were listed for the calling instance  |
| Instance Metadata | is.metadata.instance-network-interface.get | Network interface information was retrieved for the calling instance |
| Instance Metadata | is.metadata.instance-network-interface.list | Network interfaces were listed for the calling instance  |
| Instance Metadata | is.metadata.instance-placement-group.get | Placement group information was retrieved for the calling instance |
| Instance Metadata | is.metadata.instance-placement-group.list | Placement group information was listed for the calling instance |
| Instance Metadata | is.metadata.instance-volume-attachment.get | A volume attachment was retrieved for the calling instance |
| Instance Metadata | is.metadata.instance-volume-attachment.list | Volume attachments were listed for the calling instance  |
{: caption="Table 19. Actions that generate events for the Instance Metadata service" caption-side="bottom"}

### Bare metal server events
{: #events-compute-bm}

Some fields for Bare Metal Servers for VPC AT events will be changing between the Beta and Limited Available (LA) releases. For more information, see [Analyzing events](/docs/vpc?topic=vpc-at-events#at_events_iam_analyze).
{: note}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| Bare Metal Server | is.bare-metal-server.bare-metal-server.read | One or more bare metal servers was retrieved |
| Bare Metal Server | is.bare-metal-server.bare-metal-server.create | Bare metal server was created |
| Bare Metal Server | is.bare-metal-server.bare-metal-server.update | Bare metal server was updated |
| Bare Metal Server | is.bare-metal-server.bare-metal-server.delete | Bare metal server was deleted |
| Bare Metal Server | is.bare-metal-server.bare-metal-server.restart | Bare metal server was restarted |
| Bare Metal Server | is.bare-metal-server.bare-metal-server.start | Bare metal server was started |
| Bare Metal Server | is.bare-metal-server.bare-metal-server.stop | Bare metal server was stopped |
| Bare Metal Server | is.bare-metal-server.console.read | Bare metal server console connection was retrieved |
| Bare Metal Server | is.bare-metal-server.console-access-token.create | Bare metal server console access token was created |
| Bare Metal Server | is.bare-metal-server.disk.read | One or more disks on a bare metal server was retrieved |
| Bare Metal Server | is.bare-metal-server.disk.update | Disk on a bare metal server was updated |
| Bare Metal Server | is.bare-metal-server.initialization.read | Bare metal server's initialization data was retrieved |
| Bare Metal Server | is.bare-metal-server.network-interface.read | One or more network interfaces on a bare metal server was retrieved |
| Bare Metal Server | is.bare-metal-server.network-interface.create | Network interface was created on a bare metal server |
| Bare Metal Server | is.bare-metal-server.network-interface.update | Network interface on a bare metal server was updated |
| Bare Metal Server | is.bare-metal-server.network-interface.delete | Network interface on a bare metal server was deleted |
| Bare Metal Server | is.bare-metal-server.network-interface-floating-ip.read | One or more floating IPs associated with a network interface was retrieved |
| Bare Metal Server | is.bare-metal-server.network-interface-floating-ip.attach | Floating IP was associated with a network interface |
| Bare Metal Server | is.bare-metal-server.network-interface-floating-ip.detach | Floating IP was disassociated from a network interface |
{: caption="Table 20. Actions that generate events for Bare Metal Server" caption-side="bottom"}

### Key events
{: #events-compute-key}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| key  | is.key.key.create   | Key was created  |
| key  | is.key.key.delete   | Key was deleted  |
| key  | is.key.key.update   | Key was updated  |
| key | is.key.key.read | One or more keys was retrieved |
{: caption="Table 21. Actions that generate events for Key" caption-side="bottom"}

### Dedicated host events
{: #events-compute-dedicated-host}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| dedicated-host | is.dedicated-host.dedicated-host.create | Dedicated host was created |
| dedicated-host | is.dedicated-host.dedicated-host.update | Dedicated host or host disk was updated |
| dedicated-host | is.dedicated-host.dedicated-host.delete  |Dedicated host was deleted |
| dedicated-host | is.dedicated-host.dedicated-host.read | One or more dedicated hosts or host disks were retrieved |
{: caption="Table 22. Actions that generate events for Dedicated Host" caption-side="bottom"}

### Dedicated host group events
{: #events-compute-dedicated-host-group}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| dedicated-host-group  | is.dedicated-host.dedicated-host-group.create | Dedicated host group was created |
| dedicated-host-group  | is.dedicated-host.dedicated-host-group.update | Dedicated host group was updated |
| dedicated-host-group  | is.dedicated-host.dedicated-host-group.delete | Dedicated host group was deleted |
| dedicated-host-group  | is.dedicated-host.dedicated-host-group.read | One or more dedicated host groups was retrieved |
{: caption="Table 23. Actions that generate events for Dedicated Host Group" caption-side="bottom"}

### Instance group events
{: #events-compute-instance-group}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| instance-group  | is.instance-group.instance-group.create   | Instance group was created  |
| instance-group  | is.instance-group.instance-group.delete   | Instance group was deleted  |
| instance-group  | is.instance-group.instance-group.update   | Instance group was updated  |
| instance-group  | is.instance-group.instance-group.read | One or more instance groups were retrieved |
| instance-group  | is.instance-group.manager.create   | Instance group manager was created |
| instance-group  | is.instance-group.manager.delete   | Instance group manager was deleted  |
| instance-group  | is.instance-group.manager.update   | Instance group manager was updated  |
| instance-group  | is.instance-group.manager.read     | One or more instance group managers were retrieved |
| instance-group  | is.instance-group.manager_policy.create   | Instance group policy was created  |
| instance-group  | is.instance-group.manager_policy.delete   | Instance group policy was deleted  |
| instance-group  | is.instance-group.manager_policy.update   | Instance group policy was updated  |
| instance-group  | is.instance-group.manager_policy.read     | One or more instance group policies were retrieved |
| instance-group  | is.instance-group.load-balancer.create    | Instance group load balancer was created |
| instance-group  | is.instance-group.load-balancer.delete    | Instance group load balancer was deleted  |
| instance-group  | is.instance-group.load-balancer.update    | Instance group load balancer was updated  |
| instance-group  | is.instance-group.load-balancer.detach    | Instance group load balancer was detached  |
| instance-group  | is.instance-group.load-balancer.read      | One or more instance group load balancers were retrieved |
| instance-group  | is.instance-group.membership.create   | Instance group membership was created  |
| instance-group  | is.instance-group.membership.delete   | Instance group membership was deleted  |
| instance-group  | is.instance-group.membership.update   | Instance group membership was updated  |
| instance-group  | is.instance-group.membership.read     | Instance group membership was retrieved |
| instance-group  | is.instance-group.instance.create     | Instance group instance was created |
{: caption="Table 24. Actions that generate events for Instance Group" caption-side="bottom"}

### Image resources
{: #events-images}

The following table lists the actions that are related to image resources and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| image  | is.image.image.create   | Image was created |
| image  | is.image.image.delete   | Image was deleted |
| image  | is.image.image.update   | Image was updated |
| image  | is.image.image.read     | Image was retrieved |
| image  | is.image.image.export   | Image was exported |
| image  | is.image.image-export-job.create   | Image export job was created |
| image  | is.image.image-export-job.delete   | Image export job was deleted |
| image  | is.image.image-export-job.update   | Image export job was updated |
| image  | is.image.image-export-job.read     | Image export job was retrieved |
| image  | is.image.image-export-job.list     | Image export jobs were listed |
{: caption="Table 25. Actions that generate events for image resources" caption-side="bottom"}

For the image update event, if you are rotating the root key for an image, the CRN for the old key and new key remains the same. The ID for the new key that is rotated in is indicated in the `kmsKeyRefID` field of the image.
{: note}

### Placement group resources
{: #events-placement-group}

The following table lists the actions that are related to placement group resources and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| placement_group | is.placement-group.placement-group.create | Placement group was created |
| placement_group | is.placement-group.placement-group.delete | Placement group was deleted |
| placement_group | is.placement-group.placement-group.update | Placement group was updated |
| instance | is.instance.instance.create | Instance was created and includes a placement group reference |
| instance | is.instance.instance.update | Instance was updated and includes updates to the placement group reference |
{: caption="Table 26. Actions that generate events for placement group resources" caption-side="bottom"}

## Storage resources
{: #events-storage}

### Block Storage events
{: #events-block-storage}

The following table lists the actions that are related to volume resources and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| volume  | is.volume.volume.create  | Volume was created  |
| volume  | is.volume.volume.update  | Volume was updated  |
| volume  | is.volume.volume.delete  | Volume was deleted  |
| volume  | is.volume.volume.read    | One or more volumes were retrieved  |
| volume | is.volume.volume.operate | Volume ID was specified |
{: caption="Table 27. Actions that generate events for Block Storage resources" caption-side="bottom"}

An event does not contain a volume name if no information is available at the time of the event. For example, when you make a request to create a volume but do not provide a volume name, the information is not available and does not appear in the event.
{: note}

### Snapshots events
{: #events-snapshots}

The following table lists the actions that are related to snapshots resources and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| snapshot  | is.snapshot.snapshot.create  | Snapshot creation process started |
| snapshot  | is.snapshot.snapshot.capture    | Volume data was captured  |
| snapshot  | is.snapshot.snapshot.update  | Snapshot was updated  |
| snapshot  | is.snapshot.snapshot.delete  | Snapshot was deleted  |
| snapshot  | is.snapshot.snapshot.read    | One or more snapshots were retrieved  |
| snapshot  | is.snapshot.snapshot.restore | Volume was restored from a snapshot |
| snapshot  | is.snapshot.snapshot.operate | Source snapshot ID was specified |
{: caption="Table 28. Actions that generate events for snapshot resources" caption-side="bottom"}

### Consistency group events
{: #events-consistency-group}

The following table lists the actions that are related to snapshot consistency group resources and the generation of events.

| Resource |  Action | Description  |
|---|---|---|
| snapshot-consistency-group | is.snapshot-consistency-group.snapshot-consistency-group.create   | snapshot-consistency-group was created  |
| snapshot-consistency-group | is.snapshot-consistency-group.snapshot-consistency-group.update   | snapshot-consistency-group was updated  |
| snapshot-consistency-group | is.snapshot-consistency-group.snapshot-consistency-group.delete   | snapshot-consistency-group was deleted  |
| snapshot-consistency-group | is.snapshot-consistency-group.snapshot-consistency-group.read     | One or more snapshot-consistency-group were retrieved |
{: caption="Table 29. Actions that generate events for snapshot consistency group resources" caption-side="bottom"}

### File Storage events
{: #events-file-storage}

The following table lists the actions that are related to file share resources and the generation of events.

| Resource | Action                | Description             |
|:---------|:----------------------|:------------------------|
| shares  | is.share.share.create  | File share was created  |
| shares  | is.share.share.read    | One or more file shares were retrieved  |
| shares  | is.share.share.update  | File share was updated  |
| shares  | is.share.share.delete  | File share was deleted  |
| shares  | is.share.share.schedule.modification | The replication schedule was modified. |
| shares  | is.share.replica.read  | The replica share was read |
| shares  | is.share.source.read   | The source share was read |
| shares  | is.share.source.update | Updated share source |
| shares  | is.share.share.init    | Replication Initialization status |
| shares  | is.share.share.failover| Replication Failover status |
| shares  | is.share.share.split   | Replication Split status |
| share mount targets | is.share.mount-target.create| Mount target for a file share was created  |
| share mount targets | is.share.mount-target.read  | One mount target for a file share was retrieved  |
| share mount targets | is.share.mount-target.list  | List of all mount targets for a file share was retrieved  |
| share mount targets | is.share.mount-target.update| Mount target for a file share was modified  |
| share mount targets | is.share.mount-target.delete| Mount target for a file share was deleted  |
{: caption="Table 30. Actions that generate events for file storage resources" caption-side="bottom"}

### Backup service events
{: #events-backup-service}

The following table lists the actions that are related to the VPC Backup service resources and the generation of events.

| Resource       | Action                                    | Description  |
|:---------------|:------------------------------------------|:-----------------------|
| backup-policy  | is.backup-policy.backup-policy.create     | Backup policy was created |
| backup-policy  | is.backup-policy.backup-policy.update     | Backup policy was updated  |
| backup-policy  | is.backup-policy.backup-policy.delete     | Backup policy was deleted  |
| backup-policy  | is.backup-policy.backup-policy.list       | One or more backup policies were retrieved  |
| backup-policy  | is.backup-policy.backup-policy.read       | Backup policy was retrieved  |
| backup-policy  | is.backup-policy.backup-plan.create       | Backup plan was created  |
| backup-policy  | is.backup-policy.backup-plan.delete       | Backup plan was deleted  |
| backup-policy  | is.backup-policy.backup-plan.read         | One or more backup plans were retrieved  |
| backup-policy  | is.backup-policy.backup-job.read          | One or more backup jobs were retrieved  |
| backup-policy  | is.backup-policy.backup-policy-job.create | This event is triggered if an Enterprise-level backup policy fails to create backups in one or more child accounts due to missing service-to-service authorizations.  |
{: caption="Table 31. Actions that generate events for VPC Backup service resources" caption-side="bottom"}

## Supported locations
{: #at-supported-locations}

{{site.data.keyword.at_full}} support is available for the Dallas location.

## Viewing events
{: #at_ui}

Events that are generated by VPC resources are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Launching the UI through the IBM Cloud UI](/docs/activity-tracker?topic=activity-tracker-launch#launch_cloud_ui).

## Analyzing events
{: #at_events_iam_analyze}

Refer to the following information when you are analyzing events:

- Activity Tracker actions are set to `read` for both the GET and LIST calls, for example, `is.instance.instance.read`. However, for the LIST calls, the `target.name` field is set to `*`, whereas for the GET calls, it is set to the name of the resource.

- When a user triggers an instance action change, an Activity Tracker log is generated. The action is specified to one of the following valid action types: `start`, `stop`, and `reboot`.

- The `target` field contains information about the top-level resources, for example, VPC, subnets, and instances (virtual machine). For subresources, such as volume-attachments and security-group rules, their unique identifier can be found from the `responseData.responseURI` field. The mapping to the parent resources is reflected by the URI.

- For events that are related to volume-attachments, the ID of the volume that was attached can be found indirectly by getting the ID of the volume-attachment from the `target.responseURI` field (which also contains the instance ID), and by using that ID to query the corresponding `GET /instance/{instance_id}/volume-attachment/{volume-attachment-id}` API. The volume ID can be found from the response of the GET API call.

- For events that are related to Load Balancer requests, consider the following information:

    You can get more data in the `requestData.requestPath` field. You can find details about a subresource such as a listener ID or rule ID.

    When you create resources, you can get IDs from the `responseData.responseContent` field.

    For update actions, the original value is not included. You can do a read action to get the value before you run an update on the resource.

- The following fields for Bare Metal Servers for VPC AT events are changing between the Beta and LA releases:

   1) The `severity` field in Beta events is not consistent with general practice within VPC and is corrected in the LA release.

   2) The `target resourceGroupID` field in Beta events does not include the CRN prefix. This is corrected in the LA release.

   3) Some Beta events include array data. If the array is too large, the event isn't recorded. For LA, these events display an array element count rather than the actual data.

- Each time you open a console for a virtual server instance or bare metal server, two API calls are issued: the first one generates a console access token, the second one uses the generated token to open the console websocket.

   This API call generates two Activity Tracker events: `is.instance.console-access-token.create` and `is.instance.console.read`, or `is.bare-metal-server.console-access-token.create` and `is.bare-metal-server.console.read`.

   You cannot use `request id` to correlate console Activity Tracker events as each call generates a unique `request id`. Instead, you can use `initiator id` to correlate console events that are generated from API calls issued by the same user.

- You can find the detailed information and fields included in the requestData and responseData for the Bare Metal Servers for VPC AT events in the [API documentation](/apidocs/vpc/latest).
