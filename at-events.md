---

copyright:
  years: 2019, 2020

lastupdated: "2020-12-22"

keywords: activity tracker, events, logdna

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:download: .download}

# Activity Tracker with LogDNA events
{: #at-events}

As a security officer, auditor, or manager, you can use the {{site.data.keyword.at_full}} service to track how users and applications interact with {{site.data.keyword.cloud}} Virtual Private Cloud (VPC).
{:shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started#getting-started).


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
{: caption="Table 1. Actions that generate events for Network ACL" caption-side="top"}

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

{: caption="Table 2. Actions that generate events for VPC routing tables and routes" caption-side="top"}

### Floating IP events
{: #events-network-floatingIP}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| floating-ip  | is.floating-ip.floating-ip.create   | Floating IP was created  |
| floating-ip  | is.floating-ip.floating-ip.update   | Floating IP was updated  |
| floating-ip  | is.floating-ip.floating-ip.delete   | Floating IP was deleted  |
| floating-ip  | is.floating-ip.floating-ip.read | One or more floating IP was retrieved |
{: caption="Table 3. Actions that generate events for Floating IP" caption-side="top"}

### Flow log events
{: #events-flow-logs}

The following table lists the actions that are related to flow logs and the generation of events.

| Resource | Action | Description |
|---|---|---|
| flow-log-collector | is.flow-log-collector.flow-log-collector.create | Flow log collector was created |
| flow-log-collector | is.flow-log-collector.flow-log-collector.delete | Flow log collector was deleted |
| flow-log-collector | is.flow-log-collector.flow-log-collector.read | Flow log collector was read |
| flow-log-collector | is.flow-log-collector.flow-log-collector.update | Flow log collector was updated |
{: caption="Table 4. Actions that generate events for flow log collectors" caption-side="top"}

### Load balancer events
{: #events-load-balancers}

The following table lists the actions that related to load balancers and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| Load balancer |  is.load-balancer.load-balancer.create | Load balancer was created |
| Load balancer |  is.load-balancer.load-balancer.update | Load balancer was updated |
| Load balancer |  is.load-balancer.load-balancer.delete | Load balancer was deleted |
| Listener |  is.load-balancer.load-balancer-listener.create | Listener was created |
| Listener |  is.load-balancer.load-balancer-listener.update | Listener was updated |
| Listener |  is.load-balancer.load-balancer-listener.delete | Listener was deleted |
| Pool |  is.load-balancer.load-balancer-pool.create | Pool was created |
| Pool |  is.load-balancer.load-balancer-pool.update | Pool was updated |
| Pool |  is.load-balancer.load-balancer-pool.delete | Pool was deleted |
| Member |  is.load-balancer.load-balancer-pool-member.create | Member was created |
| Member |  is.load-balancer.load-balancer-pool-member.update | Member was updated |
| Member |  is.load-balancer.load-balancer-pool-member.delete | Member was deleted |
| Policy |  is.load-balancer.load-balancer-listener-policy.create | Policy was created |
| Policy |  is.load-balancer.load-balancer-listener-policy.update | Policy was updated |
| Policy |  is.load-balancer.load-balancer-listener-policy.delete | Policy was deleted |
| Rule |  is.load-balancer.load-balancer-listener-policy-rule.create | Rule was created |
| Rule |  is.load-balancer.load-balancer-listener-policy-rule.update | Rule was updated |
| Rule |  is.load-balancer.load-balancer-listener-policy-rule.delete | Rule was deleted |
{: caption="Table 5. Actions that generate events for load balancers" caption-side="top"}



### Public gateway events
{: #events-network-public-gateway}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| public-gateway | is.public-gateway.public-gateway.create   | Public Gateway was created   |
| public-gateway | is.public-gateway.public-gateway.update   | Public Gateway was updated   |
| public-gateway | is.public-gateway.public-gateway.delete   | Public Gateway was deleted   |
| public-gateway | is.public-gateway.public-gateway.read | One or more public gateways were retrieved  |
{: caption="Table 6. Actions that generate events for Public Gateway" caption-side="top"}

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
{: caption="Table 7. Actions that generate events for Security Group" caption-side="top"}

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
{: caption="Table 8. Actions that generate events for Subnet" caption-side="top"}

### Virtual private endpoints events
{: #events-vpe}

The following table lists the actions that are related to virtual private endpoints and the generation of events.

| Resource | Action | Description |
|---|---|---|
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.create | Endpoint gateway was created |
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.delete | Endpoint gateway was deleted |
| endpoint-gateway | is.endpoint-gateway.endpoint-gateway.update | Endpoint gateway was updated |
{: caption="Table 9. Actions that generate events for virtual private endpoints" caption-side="top"}

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
{: caption="Table 10. Actions that generate events for VPC" caption-side="top"}

### VPN events
{: #events-vpns}

The following table lists the actions that are related to VPNs and the generation of events.

| Resource  | Action  | Data Event | Description  |
|:----------------|:-----------------------|:--------------|:-----------------------|
| vpn  | is.vpn.vpn-gateway.create   | | VPN gateway was created |
| vpn  | is.vpn.vpn-gateway.delete   | | VPN gateway was deleted |
| vpn  | is.vpn.vpn-gateway.update   | | VPN gateway was updated |
| vpn  | is.vpn.vpn-gateway.read   | | VPN gateway was retrieved |
| vpn  | is.vpn.vpn-gateway.list   | | VPN gateways were listed |
| vpn  | is.vpn.vpn-connection.create   | Yes | VPN connection was created on VPN gateway |
| vpn  | is.vpn.vpn-connection.delete   | Yes | VPN connection was deleted from VPN gateway |
| vpn  | is.vpn.vpn-connection.update   | Yes | VPN connection was updated on VPN gateway |
| vpn  | is.vpn.vpn-connection.read   | Yes | VPN connection was retrieved from VPN gateway |
| vpn  | is.vpn.vpn-connection.list   | | VPN gateway connections were listed |
| vpn  | is.vpn.vpn-connection_local-cidr.create   | | Local subnet was created on VPN gateway connection |
| vpn  | is.vpn.vpn-connection_local-cidr.delete   | | Local subnet was deleted from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_local-cidr.read   | | Local subnet was retrieved from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_local-cidr.list   | | Local subnets were listed from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_peer-cidr.create   | | Peer subnet was created on VPN gateway connection |
| vpn  | is.vpn.vpn-connection_peer-cidr.delete   | | Peer subnet was deleted from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_peer-cidr.read   | | Peer subnet was retrieved from VPN gateway connection |
| vpn  | is.vpn.vpn-connection_peer-cidr.list   | | Peer subnets were listed from VPN gateway connection |
| vpn  | is.vpn.ike-policy.create   | | IKE policy was created |
| vpn  | is.vpn.ike-policy.delete   | | IKE policy was deleted |
| vpn  | is.vpn.ike-policy.update   | | IKE policy was updated |
| vpn  | is.vpn.ike-policy.read   | | IKE policy was retrieved |
| vpn  | is.vpn.ike-policy.list   | | IKE policies were listed |
| vpn  | is.vpn.ipsec-policy.create   | | IPsec policy was created |
| vpn  | is.vpn.ipsec-policy.delete   | | IPsec policy was deleted |
| vpn  | is.vpn.ipsec-policy.update   | | IPsec policy was updated |
| vpn  | is.vpn.ipsec-policy.read   | | IPsec policy was retrieved |
| vpn  | is.vpn.ipsec-policy.list   | | IPsec policies were listed |
{: caption="Table 11. Actions that generate events for VPNs" caption-side="top"}

## Compute resources
{: #events-compute}

The following tables list the actions that are related to compute resources and the generation of events.

### Instance events
{: #events-compute-instance}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| instance   | is.instance.instance.create   | - Instance was created<br /> - Includes the state of the `allow_ip_spoofing` parameter, which disables source/destination checks for network interfaces that are created on the virtual server. When set to `false`, IP spoofing is not allowed on the interface.  |
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
| instance   | is.instance.network-interface_floating-ip.attach  | Floating IP was associated to instance network interface  |
| instance   | is.instance.network-interface_floating-ip.detach  | Floating IP was disassociated from instance network interface |
| instance   | is.instance.volume-attachment.create   | Instance volume attachment was created  |
| instance   | is.instance.volume-attachment.delete   | Instance volume attachment was deleted  |
| instance   | is.instance.volume-attachment.update   | Instance volume attachment was updated  |
| instance   | is.instance.volume-attachment.read | One or more instance volume attachments was retrieved |
| instance   | is.instance.network-interface.create   | - Instance network interface was created (Instance was attached to a subnet)<br /> - Includes the state of the `allow_ip_spoofing` parameter, which disables source/destination checks for network interfaces that are created on the virtual server. When set to `false`, IP spoofing is not allowed on the interface.  |
| instance   | is.instance.network-interface.update   | - Instance network interface was updated (Instance was attached to a subnet)<br /> - Includes the state of the `allow_ip_spoofing` parameter, which disables source/destination checks for network interfaces that are created on the virtual server. When set to `false`, IP spoofing is not allowed on the interface. |
| instance   | is.instance.network-interface.delete   | Instance network interface was deleted (Instance was detached from a subnet)  |
| instance | is.instance.network-interface.read | One or more instance network interfaces was retrieved |
| instance | is.instance.network-interface_floating-ip.read | One or more floating IPs under an instance network interface was retrieved |
{: caption="Table 12. Actions that generate events for Instance" caption-side="top"}

### Key events
{: #events-compute-key}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| key  | is.key.key.create   | Key was created  |
| key  | is.key.key.delete   | Key was deleted  |
| key  | is.key.key.update   | Key was updated  |
| key | is.key.key.read | One or more keys was retrieved |
{: caption="Table 13. Actions that generate events for Key" caption-side="top"}

### Dedicated host events
{: #events-compute-dedicated-host}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| dedicated-host | is.dedicated-host.dedicated-host.create | Dedicated host was created |
| dedicated-host | is.dedicated-host.dedicated-host.update | Dedicated host was updated |
| dedicated-host | is.dedicated-host.dedicated-host.delete  |Dedicated host was deleted |
| dedicated-host | is.dedicated-host.dedicated-host.read | One or more dedicated hosts were retrieved |
{: caption="Table 14. Actions that generate events for Dedicated Host" caption-side="top"}

### Dedicated host group events
{: #events-compute-dedicated-host-group}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| dedicated-host-group  | is.dedicated-host.dedicated-host-group.create | Dedicated host group was created |
| dedicated-host-group  | is.dedicated-host.dedicated-host-group.update | Dedicated host group was updated |
| dedicated-host-group  | is.dedicated-host.dedicated-host-group.delete | Dedicated host group was deleted |
| dedicated-host-group  | is.dedicated-host.dedicated-host-group.read | One or more dedicated host groups was retrieved |
{: caption="Table 15. Actions that generate events for Dedicated Host Group" caption-side="top"}

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
{: caption="Table 16. Actions that generate events for Instance Group" caption-side="top"}

### Image resources
{: #events-images}

The following table lists the actions that are related to image resources and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| image  | is.image.image.create   | Image was created |
| image  | is.image.image.delete   | Image was deleted |
| image  | is.image.image.update   | Image was updated |
{: caption="Table 17. Actions that generate events for image resources" caption-side="top"}

For the image update event, if you are rotating the root key for an image, the CRN for the old key and new key remains the same. The ID for the new key that is rotated in is indicated in the `kmsKeyRefID` field of the image.
{:note}

## Storage resources
{: #events-storage}

The following table lists the actions that related to volume resources and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| volume  | is.volume.volume.create  | Volume was created  |
| volume  | is.volume.volume.update  | Volume was updated  |
| volume  | is.volume.volume.delete  | Volume was deleted  |
| volume  | is.volume.volume.read    | One or more volumes were retrieved  |
{: caption="Table 18. Actions that generate events for storage resources" caption-side="top"}

An event does not contain a volume name if no information is available at the time of the event. For example, when you make a request to create a volume but do not provide a volume name, the information is not available and does not appear in the event.
{:note}

## Supported locations
{: #at-supported-locations}

{{site.data.keyword.at_full}} support is available for the Dallas location.

## Viewing events
{: #at_ui}

Events that are generated by VPC resources are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Launching the web UI through the IBM Cloud UI](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-launch#launch_cloud_ui).

## Analyzing events
{: #at_events_iam_analyze}

Refer to the following information when you are analyzing events:

- Activity Tracker actions are set to `read` for both the GET and LIST calls, for example, `is.instance.instance.read`. However, for the LIST calls, the `target.name` field is set to `*`, whereas for the GET calls, it is set to the name of the resource.

- When a user triggers an instance action change, an Activity Tracker log is generated. The action is specified to one of the following valid action types: `start`, `stop`, and `reboot`.

- The `target` field contains information about the top-level resources, for exmaple, VPC, subnets, and instances (virtual machine), etc. For sub-resources, such as volume-attachments, security-group rules, etc., their unique identifier can be found from the `responseData.responseURI` field. The mapping to the parent resources is reflected by the URI.

- For events that are related to volume-attachments, the ID of the volume that was attached can be found indirectly by getting the ID of the volume-attachment from the `target.responseURI` field (which also contains the instance ID), and by using that ID to query the corresponding `GET /instance/{instance_id}/volume-attachment/{volume-attachment-id}` API. The volume ID can be found from the response of the GET API call.

- For events that are related to Load Balancer requests, consider the following information:

    You can get additional data in the `requestData.requestPath` field. You can find details about a subresource such as a listener ID or rule ID.

    When you create resources, you can get IDs from the `responseData.responseContent` field.

    For update actions, the original value is not included. You can do a read action to get the value before you run an update on the resource.
