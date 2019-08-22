---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: cli, reference, commands

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# CLI reference
{: #cli-reference}

This document provides a reference of the command line interface (CLI) commands available in the Virtual Private Cloud (VPC) early access release.
{:shortdesc}

This document is organized into the following sections:
* [Network commands](#network)
* [Compute commands](#compute)
* [Regions and zones commands](#geography)
* [Storage CLI commands](#storage)

## Prerequisites
{: #cli-ref-prereqs}

1. Install the [IBM Cloud CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/cli){: new_window}.

2. Install or update the VPC plugin.

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: codeblock}

  To update:

  ```
  ibmcloud plugin update
  ```
  {: codeblock}

  To view installed plugins and versions:

  ```
  ibmcloud plugin list
  ```
  {: codeblock}

3. Log in to IBM Cloud. 

  If you have a federated account:
  ```
  ibmcloud login -a api.ng.bluemix.net -sso
  ```
  {: codeblock}

  Otherwise, use the folowing command:
  ```
  ibmcloud login -a api.ng.bluemix.net
  ```
  {: codeblock}

4. Make sure the CLI plugin targets generation 2 of the VPC infrastructure service. To view the current target generation, run the following command:

  ```
  ibmcloud is target
  ```
  {: codeblock}

  If the plugin is not currently targeting generation 2, run the following command: 

  ```
  ibmcloud is target --gen 2
  ```
  {: codeblock}

## Network commands
{: #network}

This section provides information about the CLI commands available for  network functionality.

## Floating IPs
{: #floating-ips-cli-ref}


### `ibmcloud is floating-ip`
{: #floating-ips-cli}

View details of a floating IP.

`ibmcloud is floating-ip FLOATING_IP_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is floating-ip-release`
{: #floating-ip}

Release a floating IP.

`ibmcloud is floating-ip-release FLOATING_IP_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is floating-ip-reserve`
{: #floating-ip-reserve}

Reserve a floating IP.

`ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE | --nic-id NIC_ID) [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**options**

- `--zone`: Name of the targetzone. This option is mutually exclusive with --nic-id.
- `--nic-id`: ID of the target network interface. This option is mutually exclusive with --zone.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is floating-ip-update`
{: #floating-ip-update}

Update a floating ip.

`ibmcloud is floating-ip-update FLOATING_IP_ID [--name NEW_NAME] [--nic-id NIC_ID] [--json]`

**options**

- `--name`: New name of the floating ip.
- `--nic-id`: ID of the network interface to associate.
- `--json`: Format output in JSON.

---

### `ibmcloud is floating-ips`
{: #floating-ip-list}

List all floating ips.

`ibmcloud is floating-ips [--json]`

**options**

- `--json`: Format output in JSON.

---
## Public gateways
{: #public-gateways}

### `ibmcloud is public-gateway`
{: #public-gateway-details}

View details of a public gateway.

`ibmcloud is public-gateway GATEWAY_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is public-gateway-create`
{: #public-gateway-create}

Create a public gateway.

`ibmcloud is public-gateway-create GATEWAY_NAME VPC_ID ZONE_NAME [--floating-ip-id IP_ID | --floating-ip-address IP_ADDRESS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**options**

- `--floating-ip-id`: ID of the floating IP bound to the public gateway.
- `--floating-ip-address`: IP address of the floating IP bound to the public gateway.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is public-gateway-delete`
{: #public-gateway-delete}

Delete a public gateway.

`ibmcloud is public-gateway-delete GATEWAY_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is public-gateway-update`
{: #public-gateway-update}

Update a public gateway.

`ibmcloud is public-gateway-update GATEWAY_ID --name NEW_NAME [--json]`

**options**

- `--name`: New name of the public gateway.
- `--json`: Format output in JSON.

---

### `ibmcloud is public-gateways`
{: #public-gateway-list-all}

List all public gateways.

`ibmcloud is public-gateways [--json]`

**options**

- `--json`: Format output in JSON.

---
## Security groups
{: #security-groups-cli-ref}

### `ibmcloud is security-group`
{: #security-groups-cli-details}

View details of a security group.

`ibmcloud is security-group GROUP_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-create`
{: #security-group-create}

Create a security group.

`ibmcloud is security-group-create GROUP_NAME VPC_ID [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**options**

- `--resource-group-id`: ID of the resource group. This option is exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---


### `ibmcloud is security-group-delete`
{: #security-group-delete}

Delete a security group.

`ibmcloud is security-group-delete GROUP_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is security-group-network-interface`
{: #security-group-network-interface-details}

View details of a network interface of a security group.

`ibmcloud is security-group-network-interface GROUP_ID NIC_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-network-interface-add`
{: #security-group-network-interface-add}

Add a network interface to a security group.

`ibmcloud is security-group-network-interface-add GROUP_ID NIC_ID [--json]`

**options**

- `GROUP_ID`: ID of the security group.
- `NIC_ID`: ID of the network interface.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-network-interface-remove`
{: #security-group-network-interface-remove}

Remove a network interface from a security group.

`ibmcloud is security-group-network-interface-remove GROUP_ID NIC_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is security-group-network-interfaces`
{: #security-group-network-interfaces}

List all network interfaces of a security group.

`ibmcloud is security-group-network-interfaces GROUP_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-rule`
{: #security-group-rule}

View details of a security group rule.

`ibmcloud is security-group-rule GROUP_ID RULE_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-rule-add`
{: #security-group-rule-add}

Add a rule to a security group.

`ibmcloud is security-group-rule-add GROUP_ID DIRECTION PROTOCOL [--remote REMOTE_ADDRESS | CIDR_BLOCK | SECURITY_GROUP_ID] [--icmp-code ICMP_CODE] [--icmp-type ICMP_TYPE] [--port-max PORT_MAX] [--port-min PORT_MIN] [--json]`

**options**

- `--remote`: The set of network interfaces from which this rule allows traffic, Can be specified as either an REMOTE_ADDRESS, CIDR_BLOCK and SECURITY_GROUP_ID.
- `--icmp-type`: ICMP traffic type to allow. Valid values from 0 to 254. This option is specified only when protocol is set to icmp. If unspecified, all types are allowed.
- `--icmp-code`: ICMP traffic code to allow. Valid values from 0 to 255. This option is specified only when protocol is set to icmp. If unspecified, all codes are allowed.
- `--port-min`: Minimum port number. Valid values are from 1 to 65535. This option is specified only when protocol is set to tcp or udp (default: 1).
- `--port-max`: Maximum port number. Valid values are from 1 to 65535. This option is specified only when protocol is set to tcp or udp (default: 65535).
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-rule-delete`
{: #security-group-rule-delete}

Delete a rule from a security group.

`ibmcloud is security-group-rule-delete GROUP_ID RULE_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is security-group-rule-update`
{: #security-group-rule-update}

Update a rule of a security group.

`ibmcloud is security-group-rule-update GROUP_ID RULE_ID [--direction DIRECTION] [--remote REMOTE_ADDRESS | CIDR_BLOCK | SECURITY_GROUP_ID] [--icmp-code ICMP_CODE] [--icmp-type ICMP_TYPE] [--port-max PORT_MAX] [--port-min PORT_MIN] [--json]`

**options**

- `--direction`: Direction of traffic to enforce. Valid values are inbound or outbound.
- `--remote`: The set of network interfaces from which this rule allows traffic, Can be specified as either an REMOTE_ADDRESS, CIDR_BLOCK and SECURITY_GROUP_ID.
- `--icmp-type`: ICMP traffic type to allow. Valid values from 0 to 254. This option is specified only when protocol is set to icmp.
- `--icmp-code`: ICMP traffic code to allow. Valid values from 0 to 255. This option is specified only when protocol is set to icmp.
- `--port-min`: Minimum port number. Valid values are from 1 to 65535. This option is specified only when protocol is set to tcp or udp.
- `--port-max`: Maximum port number. Valid values are from 1 to 65535. This option is specified only when protocol is set to tcp or udp.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-rules`
{: #security-group-rules}

List all rules of a security group.

`ibmcloud is security-group-rules GROUP_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-update`
{: #security-group-update}

Update a security group.

`ibmcloud is security-group-update GROUP_ID [--name NEW_NAME] [--json]`

**options**

- `--name`: New name of the security group.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-groups`
{: #security-groups-cli}

List all security groups.

`ibmcloud is security-groups [--json]`

**options**

- `--json`: Format output in JSON.

---

## Subnets
{: #subnet-cli-ref}

### `ibmcloud is subnet`
{: #subnet-details}

View details of a subnet.

`ibmcloud is subnet SUBNET_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is subnet-create`
{: #subnet-create}

Create a subnet.

`ibmcloud is subnet-create SUBNET_NAME VPC_ID (--ipv4-cidr-block CIDR_BLOCK | (--ipv4-address-count ADDR_COUNT --zone ZONE_NAME)) [--public-gateway-id PUBLIC_GATEWAY] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**options**

- `--ipv4-cidr-block`: the IPv4 range of the subnet, this is exclusive with --ipv4-address-count.
- `--ipv4-address-count`: The total number of IPv4 addresses required, must be a power of 2 and minimum value is 8. This option is mutually exclusive with --ipv4-cidr-block.
- `--zone`: Name of the zone.
- `--public-gateway-id`: The ID of the public gateway.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is subnet-delete`
{: #subnet-delete}

Delete a subnet.

`ibmcloud is subnet-delete SUBNET_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is subnet-update`
{: #subnet-update}

Update a subnet.

`ibmcloud is subnet-update SUBNET_ID  [--name NEW_NAME] [--public-gateway-id PUBLIC_GATEWAY_ID] [--json]`

**options**

- `--name`: New name of the subnet.
- `--public-gateway-id`: ID of the public gateway.
- `--json`: Format output in JSON.

---

### `ibmcloud is subnets`
{: #subnets-list}

List all subnets.

`ibmcloud is subnets [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is subnet-public-gateway`
{: #subnet-public-gateway}

View details of public gateway attached to the subnet.

`ibmcloud is subnet-public-gateway SUBNET_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is subnet-public-gateway-detach`
{: #subnet-public-gateway-detach}

Detach the public gateway from a subnet.

`ibmcloud is subnet-public-gateway-detach SUBNET_ID`

---

## VPCs
{: #vpcs-cli-ref}

### `ibmcloud is vpc`
{: #vpc-details}

View details of a VPC.

`ibmcloud is vpc VPC_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-address-prefix`
{: #vpc-address-prefix}

View details of a VPC address prefix.

`ibmcloud is vpc-address-prefix VPC_ID PREFIX_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-address-prefix-create`
{: #vpc-address-prefix-create}


Create an address prefix.

`ibmcloud is vpc-address-prefix-create PREFIX_NAME VPC_ID ZONE_NAME CIDR [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-address-prefix-delete`
{: #vpc-address-prefix-delete}

Delete an address prefix.

`ibmcloud is vpc-address-prefix-delete VPC_ID PREFIX_ID [--force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is vpc-address-prefix-update`
{: #vpc-address-prefix-update}

Update an address prefix.

`ibmcloud is vpc-address-prefix-update VPC_ID PREFIX_ID [--name NEW_NAME] [--json]`

**options**

- `--name`: New name of the address prefix.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-address-prefixes`
{: #vpc-address-prefixes}

List all address prefixes.

`ibmcloud is vpc-address-prefixes VPC_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-create`
{: #vpc-create}

Create a VPC.

`ibmcloud is vpc-create VPC_NAME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**options**

- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-default-security-group`
{: #vpc-default-security-group}

View details of the default security group of a VPC.

`ibmcloud is vpc-default-security-group VPC_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-delete`
{: #vpc-delete}

Delete a VPC.

`ibmcloud is vpc-delete VPC_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is vpc-route`
{: #vpc-route}

View details of a route.

`ibmcloud is vpc-route VPC_ID ROUTE_ID [--json]`

**Options**

- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-route-create`
{: #vpc-route-create}

Create a route.

`ibmcloud is vpc-route-create ROUTE_NAME VPC_ID --zone ZONE_NAME --destination DESTINATION_CIDR --next-hop-ip NEXT_HOP_IP [--json]`

**Options**

- `--zone`: Name of the zone.
- `--destination`: The destination CIDR of the route.
- `--next-hop-ip`: The IP address of the next hop to which to route packets.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-route-delete`
{: #vpc-route-delete}

Delete a route.

`ibmcloud is vpc-route-delete VPC_ID ROUTE_ID [--force]`

**Options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is vpc-route-update`
{: #vpc-route-update}

Update a route.

`ibmcloud is vpc-route-update VPC_ID ROUTE_ID --name NEW_NAME [--json]`

**Options**

- `--name`: New name of the route.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-routes`
{: #vpc-routes}

List all routes.

`ibmcloud is vpc-routes VPC_ID [--json]`

**Options**

- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-update`
{: #vpc-update}

Update a VPC.

`ibmcloud is vpc-update VPC_ID [--name NEW_NAME] [--json]`

**options**

- `--name`: New name of the VPC.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpcs`
{: #vpc-list}

List all VPCs.

`ibmcloud is vpcs [--json]`

**options**

- `--json`: Format output in JSON.

---

## Compute commands
{: #compute}

This section provides information about the CLI commands available for compute functionality.

## Images
{: #compute-images}

### `ibmcloud is image`
{: #image-details}

View details of an image.

`ibmcloud is image IMAGE_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is images`
{: #image-list-region}

List all images in the region.

`ibmcloud is images [--json]`

**options**

- `--json`: Format output in JSON.

---

## Instances
{: #instances}

### `ibmcloud is instance`
{: #instance-details}

View details of a virtual server instance.

`ibmcloud is instance INSTANCE_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instances`
{: #instances-list}

List all virtual virtual server instances.

`ibmcloud is instances [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-create`
-{: #instance-create}

-Create a virtual server instance.

`ibmcloud is instance-create INSTANCE_NAME VPC_ID ZONE_NAME PROFILE_NAME SUBNET_ID --image-id IMAGE_ID [--boot-volume BOOT_VOLUME_JSON | @BOOT_VOLUME_JSON_FILE] [--volume-attach VOLUME_ATTACH_JSON | @VOLUME_ATTACH_JSON_FILE] [--key-ids IDS] [--user-data DATA] [--network-interface NETWORK_INTERFACE_JSON | @NETWORK_INTERFACE_JSON_FILE] [--security-group-ids SECURITY_GROUP_IDS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**options**

- `--boot-volume`: BOOT_VOLUME_JSON|@BOOT_VOLUME_JSON_FILE, boot volume attachment in json or json file.
- `--volume-attach`: VOLUME_ATTACH_JSON|@VOLUME_ATTACH_JSON_FILE, volume attachment in json or json file.
- `--image-id`: ID of the image.
- `--security-group-ids`: Comma separated security group IDs for primary network interface.
- `--key-ids`: Comma separated IDs of SSH keys.
- `--user-data`: data|@data-file. User data to transfer to the virtual server instance.
- `--network-interface`: NETWORK_INTERFACE_JSON|@NETWORK_INTERFACE_JSON_FILE. The  network interface attachment in json or json file. For information about the required format of NETWORK_INTERFACE_JSON, see primary_network_interface in the [API reference](https://{DomainName}/apidocs/vpc#create-an-instance){: new_window}.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-delete`
{: #instance-delete}

Delete a virtual server instance.

`ibmcloud is instance-delete INSTANCE_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-initialization-values`
{: #instance-initialization-values}

View initialization details of a server instance.

`ibmcloud is instance-initialization-values INSTANCE_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface`
{: #instance-network-interface}

View details of a network interface of a virtual server instance.

`ibmcloud is instance-network-interface INSTANCE_ID NIC_ID [--json]`

**options**

- `--json`: Format output in JSON.

--- 

### `ibmcloud is instance-network-interface-create`
{: #instance-network-interface-create}


#### Create a network interface for a server instance.
{: #instance-network-interface-create-for-server}

`ibmcloud is instance-network-interface-create NIC_NAME INSTANCE_ID SUBNET_ID [--ipv4 IPV4_ADDRESS] [--security-group-id SECURITY_GROUP_ID1 --security-group-id SECURITY_GROUP_ID2 ...] [--json]`

**options**

- `--ipv4`: Primary IPv4 address.
- `--security-group-id`: ID of the security group.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface-delete`
{: #instance-network-interface-delete}

Remove a network interface from a server instance.

`ibmcloud is instance-network-interface-delete INSTANCE_ID NIC_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-network-interface-floating-ip`
{: #instance-network-interface-floating-ip}

View details of a floating IP associated with a network interface.

`ibmcloud is instance-network-interface-floating-ip INSTANCE_ID NIC_ID FLOATING_IP_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface-floating-ip-add`
{: #instance-network-interface-floating-ip-add}

Associate a floating IP with a network interface.

`ibmcloud is instance-network-interface-floating-ip-add INSTANCE_ID NIC_ID FLOATING_IP_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface-floating-ip-remove`
{: #instance-network-interface-floating-remove}

Disassociate a floating IP from a network interface.

`ibmcloud is instance-network-interface-floating-ip-remove INSTANCE_ID NIC_ID FLOATING_IP_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-network-interface-floating-ips`
{: #instance-network-interface-floating-ips}

List all floating IPs associated with a network interface.

`ibmcloud is instance-network-interface-floating-ips INSTANCE_ID NIC_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface-update`
{: #instance-network-interface-update}

Update a network interface of a virtual server instance.

`ibmcloud is instance-network-interface-update INSTANCE_ID NIC_ID [--name NEW_NAME] [--json]`

**options**

- `--name`: New name of NIC.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interfaces`
{: #instance-network-interface-list}

List all network interfaces of a virtual server instance.

`ibmcloud is instance-network-interfaces INSTANCE_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-profile`
{: #instance-profile}

View details of a virtual server instance profile.

`ibmcloud is instance-profile PROFILE_NAME [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-profiles`
{: #instance-profiles}

List all virtual server instance profiles in the region.

`ibmcloud is instance-profiles [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-reboot`
{: #instance-reboot}

Restart the operating system of a virtual server instance.

`ibmcloud is instance-reboot INSTANCE_ID [--no-wait] [-f, --force] [--json]`

**options**

- `--no-wait`: Execute the action immediately and cancel any queued actions.
- `--json`: Format output in JSON.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-start`
{: #instance-start}

Start a virtual server instance.

`ibmcloud is instance-start INSTANCE_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-stop`
{: #instance-stop}

Stop a virtual server instance.

`ibmcloud is instance-stop INSTANCE_ID [--no-wait] [-f, --force] [--json]`

**options**

- `--no-wait`: Execute the action immediately and cancel any queued actions.
- `--json`: Format output in JSON.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-update`
{: #instance-update}

Update a virtual server instance.

`ibmcloud is instance-update INSTANCE_ID [--name NEW_NAME]  [--json]`

**options**

- `--name`: New name of the virtual server instance.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-volume-attachment`
{: #instance-volume-attachment}

View details of a volume attachment.

`ibmcloud is instance-volume-attachment INSTANCE_ID VOLUME_ATTACHMENT_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-volume-attachments`
{: #instance-volume-attachments}

List all volume attachments to an instance.

`ibmcloud is instance-volume-attachments INSTANCE_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-volume-attachment-add`
{: #instance-volume-attachment-add}

Create a volume attachment, connecting a volume to an instance.

`ibmcloud is instance-volume-attachment-add NAME INSTANCE_ID VOLUME_ID [--auto-delete true | false] [--json]`

**options**

- `--auto-delete`: The attached volume will be deleted when deleting the instance, default is false.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-volume-attachment-detach`
{: #instance-volume-attachment-detach}

Delete a volume attachment, detaching a volume from an instance.

`ibmcloud is instance-volume-attachment-detach INSTANCE_ID VOLUME_ATTACHMENT_ID [-f, --force]`

**options**

- `--force`, `-f`: Force the operation without confirmation.

---

### `ibmcloud is instance-volume-attachment-update`
{: #instance-volume-attachment-update}

Update a volume attachment.

`ibmcloud is instance-volume-attachment-update INSTANCE_ID VOLUME_ATTACHMENT_ID [--name NEW_NAME] [--auto-delete true | false] [--json]`

**options**

- `--name`: New name of the volume.
- `--auto-delete`: The attached volume will be deleted when deleting the instance, default is false.
- `--json`: Format output in JSON.

---

## Keys
{: #keys}

### `ibmcloud is key`
{: #key-details}

View details of a key.

`ibmcloud is key KEY_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is key-create`
{: #key-create}

Import an RSA public key.

`ibmcloud is key-create KEY_NAME (KEY | @KEY_FILE) [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**options**

- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is key-delete`
{: #key-delete}

Delete a key.

`ibmcloud is key-delete KEY_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is key-update`
{: #key-update}

Update the name of a key.

`ibmcloud is key-update KEY_ID --name NEW_NAME [--json]`

**options**

- `--name`: New name for the key.
- `--json`: Format output in JSON.

---

### `ibmcloud is keys`
{: #key-list}

List all keys.

`ibmcloud is keys [--json]`

**options**

- `--json`: Format output in JSON.

---

## Regions and zones commands
{: #geography}

This section provides information about the CLI commands available for working with regions and zones.

## Regions
{: #regions}

### `ibmcloud is region`
{: #region-details}

View details of a region.

`ibmcloud is region REGION_NAME [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is regions`
{: #region-list}

List all regions.

`ibmcloud is regions [--json]`

**options**

- `--json`: Format output in JSON.

---

## Zones
{: #zones}

### `ibmcloud is zone`
{: #zone-details}

View details of a zone in the target region.

`ibmcloud is zone ZONE_NAME [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is zones`
{: #zone-list}

List all zones in the target region.

`ibmcloud is zones [--json]`

**options**

- `--json`: Format output in JSON.

---
## Storage CLI commands
{: #storage}

### `ibmcloud is volume`
{: #volume-details}

View details of a volume.

`ibmcloud is volume VOLUME_ID [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is volumes`
{: #volume-all-details}

View details of all volumes.

**options**

`ibmcloud is volumes [--json]`

---

### `ibmcloud is volume-create`
{: #volume-create}

Create a volume.

`ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**options**

- `--capacity`: Capacity of the volume in GB, from 10 to 2000. Default to 100.
- `--iops`: Input/Output Operations Per Second for the volume.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is volume-delete`
{: #volume-delete}

Delete a volume.

`ibmcloud is volume-delete VOLUME_ID [-f, --force]`

**options**

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is volume-profile`
{: #volume-profile}

View details of a volume profile. 

`ibmcloud is volume-profile PROFILE_NAME [--json]`

Volume profile names are 10iops-tier, 5iops-tier, general-purpose, and custom.

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is volume-profiles`
{: #volume-profiles}

List all volume profiles.

`ibmcloud is volume-profiles [--json]`

**options**

- `--json`: Format output in JSON.

---

### `ibmcloud is volume-update`
{: #volume-update}

Update a volume.

`ibmcloud is volume-update VOLUME_ID [--name NEW_NAME] [--json]`

**options**

- `--name`: New name of the volume.
- `--json`: Format output in JSON.

---
