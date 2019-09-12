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

### `ibmcloud is operating-systems`
{: #operating-systems}

List all operating systems.

`ibmcloud is operating-systems [--json]`

**Options**

- `--json`: Format output in JSON.

---

### `ibmcloud is operating-system`
{: #operating-system}

View details of an operaging system.

`ibmcloud is operating-system OPERATING_SYSTEM_NAME [--json]`

**Options**

- `OPERATING_SYSTEM_NAME`: Name of the operating system
- `--json`: Format output in JSON.

---

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

### `ibmcloud is image-create`
{: #image-create}

Create an image.

`ibmcloud is image-create IMAGE_NAME --file IMAGE_FILE_LOCATION --os-name OPERATING_SYSTEM_NAME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**Options**

- `IMAGE_NAME`: Name of the new image.
- `--file`: The Cloud Object Store (COS) location of the image file, for example: 'cos://us-south/custom-image-vpc-bucket/customImage-0.vhd'.
- `--os-name`: The name of the operating system for this image.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is image-update`
{: #image-update}

Update an image.

`ibmcloud is image-delete IMAGE_ID [--name NEW_NAME] [--json]`

**Options**

- `IMAGE_ID`: ID of the image.
- `--name`: New name of the image.
- `--json`: Format output in JSON.

---

### `ibmcloud is image-delete`
{: #image-delete}

Delete an image.

`ibmcloud is image-delete IMAGE_ID [-f, --force]`

**Options**

- `IMAGE_ID`: ID of the image.
- `--force, -f`: Force the operation without confirmation.

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

## VPN Commands
{: #vpn}

This section gives details about the CLI commands available for working with VPN, including IKE and IPsec policies.

## IKE Policies

### `ibmcloud is ike-policies`
{: #ike-policies-anchor}

List all IKE policies.

`ibmcloud is ike-policies [--json]`

**Options**

- `--json`: Format output in JSON.

---

### `ibmcloud is ike-policy`
{: #ike-policy}

View details of an IKE policy.

`ibmcloud is ike-policy IKE_POLICY_ID [--json]`

**Options**

- `IKE_POLICY_ID`: ID of the IKE policy.
- `--json`: Format output in JSON.

---

### `ibmcloud is ike-policy-create`
{: #ike-policy-create}

Create an IKE policy.

`ibmcloud is ike-policy-create IKE_POLICY_NAME AUTHENTICATION_ALGORITHM DH_GROUP ENCRYPTION_ALGORITHM IKE_VERSION [--key-lifetime KEY_LIFETIME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**Options**
- `IKE_POLICY_NAME`: Name of the IKE policy.
- `AUTHENTICATION_ALGORITHM`: The authentication algorithm. Enumeration type: `md5`, `sha1`, `sha256`.
- `DH_GROUP`: The Diffie-Hellman group. Enumeration type: `2`, `5`, `14`.
- `ENCRYPTION_ALGORITHM`: The encryption algorithm. Enumeration type: `3des`, `aes128`, `aes256`.
- `IKE_VERSION`: The IKE protocol version. Enumeration type: `1`, `2`.
- `--key-lifetime`: The key lifetime in seconds. Maximum: 86400, Minimum: 1800 (default: 28800).
- `--resource-group-id`: ID of the resource group. This option is exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is ike-policy-delete`
{: #ike-policy-delete}

Delete an IKE policy.

`ibmcloud is ike-policy-delete IKE_POLICY_ID [-f, --force]`

**Options**

- `IKE_POLICY_ID`: ID of the IKE policy.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is ike-policy-update`
{: #ike-policy-update}

Update an IKE policy.

`ibmcloud is ike-policy-update IKE_POLICY_ID [--authentication_algorithm md5 | sha1 | sha256] [--dh-group 2 | 5 | 14] [--encryption-algorithm 3des | aes128 | aes256] [--ike-version 1 | 2] [--key-lifetime KEY_LIFETIME] [--name NEW_NAME] [--json]`

**Options**
- `IKE_POLICY_ID` ID of the IKE policy.
- `--name`: New name of the IKE policy.
- `--authentication-algorithm`: The authentication algorithm. Enumeration type: `md5`, `sha1`, `sha256`.
- `--dh-group`: The Diffie-Hellman group. Enumeration type: `2`, `5`, `14`.
- `--encryption-algorithm`: The encryption algorithm. Enumeration type: `3des`, `aes128`, `aes256`.
- `--ike-version`: The IKE protocol version. Enumeration type: `1`, `2`.
- `--key-lifetime`: The key lifetime in seconds. Maximum: 86400, Minimum: 1800.
- `--json`: Format output in JSON.

---

### `ibmcloud is ike-policy-connections`
{: #ike-policy-connections}

List all connections that use the IKE policy.

`ibmcloud is ike-policy-connections IKE_POLICY_ID [--json]`

**Options**
- `IKE_POLICY_ID`: ID of the IKE policy.
- `--json`: Format output in JSON.


## IPsec Policies

### `ibmcloud is ipsec-policies`
{: #ipsec-policies-anchor}

List all IPsec policies.

`ibmcloud is ipsec-policies [--json]`

**Options**

- `--json`: Format output in JSON.

---

### `ibmcloud is ipsec-policy`
{: #ipsec-policy}

View details of an IPsec policy.

`ibmcloud is ipsec-policy IPSEC_POLICY_ID [--json]`

**Options**
- `IPSEC_POLICY_ID`: ID of the IPsec policy.
- `--json`: Format output in JSON.

---

### `ibmcloud is ipsec-policy-create`
{: #ipsec-policy-create}

Create an IPsec policy.

`ibmcloud is ipsec-policy-create IPSEC_POLICY_NAME AUTHENTICATION_ALGORITHM ENCRYPTION_ALGORITHM PFS [--key-lifetime KEY_LIFETIME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**Options**
- `IPSEC_POLICY_NAME`: Name of the IPsec policy.
- `AUTHENTICATION_ALGORITHM`: The authentication algorithm. Enumeration type: `md5`, `sha1`, `sha256`.
- `ENCRYPTION_ALGORITHM`: The encryption algorithm. Enumeration type: `3des`, `aes128`, `aes256`.
- `PFS`: Perfect Forward Secrecy. Enumeration type: disabled, `group_2`, `group_5`, `group_14`.
- `--key-lifetime`: The key lifetime in seconds. Maximum: 86400, Minimum: 1800 (default: 3600).
- `--resource-group-id`: ID of the resource group. This option is exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is ipsec-policy-delete`
{: #ipsec-policy-delete}

Delete an IPsec policy.

`ibmcloud is ipsec-policy-delete IPSEC_POLICY_ID [-f, --force]`

**Options**

- `IPSEC_POLICY_NAME`: Name of the IPsec policy.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is ipsec-policy-update`
{: #ipsec-policy-update}

Update an IPsec policy.

`ibmcloud is ipsec-policy-update IPSEC_POLICY_ID [--authentication_algorithm md5 | sha1 | sha256] [--pfs disabled | group_2 | group_5 | group_14] [--encryption-algorithm 3des | aes128 | aes256] [--key-lifetime KEY_LIFETIME] [--name NEW_NAME] [--json]`

**Options**
- `IPSEC_POLICY_ID`: ID of the IPsec policy.
- `--name`: New name of the IPsec policy.
- `--authentication-algorithm`: The authentication algorithm. Enumeration type: `md5`, `sha1`, `sha256`.
- `--pfs`: Perfect Forward Secrecy. Enumeration type: `disabled`, `group_2`, `group_5`, `group_14`.
- `--encryption-algorithm`: The encryption algorithm. Enumeration type: `3des`, `aes128`, `aes256`.
- `--key-lifetime`: The key lifetime in seconds. Maximum: 86400, Minimum: 1800.
- `--json`: Format output in JSON.

---

### `ibmcloud is ipsec-policy-connections`
{: #ipsec-policy-connections}

List all connections that use the IPsec policy.

`ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID [--json]`

**Options**
- `IPSEC_POLICY_NAME`: Name of the IPsec policy.
- `--json`: Format output in JSON.


## VPN Gateway Commands

This section contains commands available for working with VPN for VPC gateways.

### `ibmcloud is vpn-gateways`
{: #vpn-gateways}


List all VPN gateways.

`ibmcloud is vpn-gateways [--json]`

**Options**

- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway`
{: #vpn-gateway}

View details of a VPN gateway.

`ibmcloud is vpn-gateway VPN_GATEWAY_ID [--json]`

**Options**
- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway-create`
{: #vpn-gateway-create}

Create a VPN gateway.

`ibmcloud is vpn-gateway-create VPN_GATEWAY_NAME SUBNET_ID [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**Options**
- `VPN_GATEWAY_NAME`: Name of the VPN gateway.
- `SUBNET_ID`: The unique identifier for this subnet.
- `--resource-group-id`: ID of the resource group. This option is exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway-delete`
{: #vpn-gateway-delete}

Delete a VPN gateway.

`ibmcloud is vpn-gateway-delete VPN_GATEWAY_ID [-f, --force]`

**Options**

- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is vpn-gateway-update`
{: #vpn-gateway-update}

Update a VPN gateway.

`ibmcloud is vpn-gateway-update VPN_GATEWAY_ID [--name NEW_NAME] [--json]`

**Options**
- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `--name`: New name of the VPN gateway.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway-connections`
{: #vpn-gateway-connections}

List all VPN gateway connections.

`ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID [--json]`

**Options**
- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway-connection-create`
{: #vpn-gateway-connection-create}

Create a VPN gateway connection.

`ibmcloud is vpn-gateway-connection-create CONNECTION_NAME VPN_GATEWAY_ID PEER_ADDRESS PRESHARED_KEY [--admin-state-up true | false] [--dead-peer-detection-action ACTION] [--dead-peer-detection-interval INTERVAL] [--dead-peer-detection-timeout TIMEOUT] [--ike-policy IKE_POLICY_ID] [--ipsec-policy IPSEC_POLICY_ID]  [--local-cidr CIDR1 --local-cidr CIDR2 ...] [--peer-cidr CIDR1 --peer-cidr CIDR2 ...] [--json]`

**Options**
- `CONNECTION_NAME`: Name of the connection.
- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `PEER_ADDRESS`: The IP address of the peer VPN gateway.
- `PRESHARED_KEY`: The preshared key.
- `--admin-state-up`: If set to `false`, the VPN gateway connection is shut down. default is `false`.
- `--dead-peer-detection-action`: Dead Peer Detection actions. Enumeration type: `restart`, `clear`, `hold`, `none`.
- `--dead-peer-detection-interval`: Dead Peer Detection interval in seconds (default: 30).
- `--dead-peer-detection-timeout`: Dead Peer Detection timeout in seconds (default: 120).
- `--ike-policy`: ID of the IKE policy.
- `--ipsec-policy`: ID of the IPsec policy.
- `--local-cidr`: Local CIDR for this resource. This parameter can be specified multiple times to provision multiple local CIDRs.
- `--peer-cidr`: Peer CIDRs for this resource. This parameter can be specified multiple times to provision multiple peer CIDRs.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway-connection`
{: #vpn-gateway-connection}

View details of a VPN gateway connection.

`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID [--json]`

**Options**
- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `CONNECTION_ID`: ID of the connection.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway-connection-delete`
{: #vpn-gateway-connection-delete}

Delete a VPN gateway connection.

`ibmcloud is vpn-gateway-connection-delete VPN_GATEWAY_ID CONNECTION_ID [-f, --force]`

**Options**

- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `CONNECTION_ID`: ID of the connection.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is vpn-gateway-connection-update`
{: #vpn-gateway-connection-update}

Update a VPN gateway connection.

`ibmcloud is vpn-gateway-connection-update VPN_GATEWAY_ID CONNECTION_ID [--admin-state-up true | false] [--dead-peer-detection-action ACTION] [--dead-peer-detection-interval INTERVAL] [--dead-peer-detection-timeout TIMEOUT] [--ike-policy IKE_POLICY_ID] [--ipsec-policy IPSEC_POLICY_ID] [--peer-address ADDRESS] [--psk PSK] [--name NEW_NAME] [--json]`

**Options**
- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `CONNECTION_ID`: ID of the connection.
- `--admin-state-up`: If set to `false`, the VPN gateway connection is shut down. default is `false`.
- `--dead-peer-detection-action`: Dead Peer Detection actions. Enumeration type: `restart`, `clear`, `hold`, `none`.
- `--dead-peer-detection-interval`: Dead Peer Detection interval in seconds.
- `--dead-peer-detection-timeout`: Dead Peer Detection timeout in seconds.
- `--ike-policy`: ID of the IKE policy.
- `--ipsec-policy`: ID of the IPsec policy.
- `--peer-address`: The IP address of the peer VPN gateway.
- `--psk`: The preshared key.
- `--name`: New name of the connection.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway-connection-local-cidr-delete`
{: #vpn-gateway-connection-local-cidr-delete}

Remove a local CIDR from the VPN gateway connection.

`ibmcloud is vpn-gateway-connection-local-cidr-delete VPN_GATEWAY_ID CONNECTION_ID PREFIX_ADDRESS PREFIX_LENGTH [-f, --force]`

**Options**

- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `CONNECTION_ID`: ID of the connection.
- `PREFIX_ADDRESS`: The prefix address part of the CIDR.
- `PREFIX_LENGTH`: The prefix length part of the CIDR.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is vpn-gateway-connection-local-cidr-add`
{: #vpn-gateway-connection-local-cidr-add}

Add a local CIDR to a VPN gateway connection.

`ibmcloud is vpn-gateway-connection-local-cidr-add VPN_GATEWAY_ID CONNECTION_ID PREFIX_ADDRESS PREFIX_LENGTH [--json]`

**Options**

- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `CONNECTION_ID`: ID of the connection.
- `PREFIX_ADDRESS`: The prefix address part of the CIDR.
- `PREFIX_LENGTH`: The prefix length part of the CIDR.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpn-gateway-connection-peer-cidr-delete`
{: #vpn-gateway-connection-peer-cidr-delete}

Remove a peer CIDR from the VPN gateway connection.

`ibmcloud is vpn-gateway-connection-peer-cidr-delete VPN_GATEWAY_ID CONNECTION_ID PREFIX_ADDRESS PREFIX_LENGTH [-f, --force]`

**Options**

- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `CONNECTION_ID`: ID of the connection.
- `PREFIX_ADDRESS`: The prefix address part of the CIDR.
- `PREFIX_LENGTH`: The prefix length part of the CIDR.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is vpn-gateway-connection-peer-cidr-add`
{: #vpn-gateway-connection-peer-cidr-add}

Add a peer CIDR to a VPN gateway connection.

`ibmcloud is vpn-gateway-connection-peer-cidr-add VPN_GATEWAY_ID CONNECTION_ID PREFIX_ADDRESS PREFIX_LENGTH [--json]`

**Options**

- `VPN_GATEWAY_ID`: ID of the VPN gateway.
- `CONNECTION_ID`: ID of the connection.
- `PREFIX_ADDRESS`: The prefix address part of the CIDR.
- `PREFIX_LENGTH`: The prefix length part of the CIDR.
- `--json`: Format output in JSON.


## Load Balancer Commands
{: #load-balancers-anchor}

This section gives details about the CLI commands available for working with load balancers, listeners, and pools.

## Load Balancer

### `ibmcloud is load-balancers`
{: #load-balancers-cli}

List all load balancers.

`ibmcloud is load-balancers [--json]`

**Options**

- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer`
{: #load-balancer-anchor}

View details of a load balancer.

`ibmcloud is load-balancer LOAD_BALANCER_ID [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-create`
{: #load-balancer-create}

Create a load balancer.

`ibmcloud is load-balancer-create LOAD_BALANCER_NAME LOAD_BALANCER_TYPE (--subnet SUBNET_ID1 --subnet SUBNET_ID2 ...) [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

**Options**

- `LOAD_BALANCER_NAME`: Name of the load balancer.
- `LOAD_BALANCER_TYPE`: Type of the load balancer, public or private.
- `--subnet`: ID of the subnets to provision this load balancer. This parameter can be specified multiple times to provision multiple subnets.
- `--resource-group-id`: ID of the resource group. This option is exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-delete`
{: #load-balancer-delete}

Delete a load balancer.

`ibmcloud is load-balancer-delete LOAD_BALANCER_ID [-f, --force]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is load-balancer-update`
{: #load-balancer-update}

Update a load balancer.

`ibmcloud is load-balancer-update LOAD_BALANCER_ID [--name NEW_NAME] [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `--name`: New name of the Load balancer.
- `--json`: Format output in JSON.

---

## Load Balancer Listener

### `ibmcloud is load-balancer-listeners`
{: #load-balancer-listeners}

List all load balancer listeners.

`ibmcloud is load-balancer-listeners LOAD_BALANCER_ID [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener`
{: #load-balancer-listener-anchor}

View details of a load balancer listener.

`ibmcloud is load-balancer-listener LOAD_BALANCER_ID LISTENER_ID [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-create`
{: #load-balancer-listener-create}

Create a load balancer listener.

`ibmcloud is load-balancer-listener-create LOAD_BALANCER_ID PORT PROTOCOL [--certificate-instance-crn CERTIFICATE_INSTANCE_CRN] [--connection-limit LIMIT] [--default-pool DEFAULT_POOL_ID] [--policies LISTENER_POLICIES_JSON | @LISTENER_POLICIES_JSON_FILE] [--json]`


**Options**
- `LOAD_BALANCER_NAME`: Name of the load balancer.
- `LOAD_BALANCER_ID`: ID of the load balancer.
- `PORT`: The listener port number.
- `PROTOCOL`: The listener protocol. Enumeration type: `http`, `https`, `tcp`.
- `--certificate-instance-crn`: CRN of the certificate instance. Required when protocol is HTTPS.
- `--connection-limit`: The connection limit of the listener.
- `--default-pool`: ID of the default pool.
- `--policies`: LISTENER_POLICIES_JSON | @LISTENER_POLICIES_JSON_FILE, listener policies in json or json file.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-delete`
{: #load-balancer-listener-delete}

Delete a load balancer listener.

`ibmcloud is load-balancer-listener-delete LOAD_BALANCER_ID LISTENER_ID [-f, --force]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is load-balancer-listener-update`
{: #load-balancer-listener-update}

Update a load balancer listener.

`ibmcloud is load-balancer-listener-update LOAD_BALANCER_ID LISTENER_ID [--certificate-instance CERTIFICATE_INSTANCE_ID] [--connection-limit LIMIT] [--port PORT] [--protocol http | https | tcp] [--default-pool DEFAULT_POOL_ID] [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `--certificate-instance-crn`: CRN of the certificate instance.
- `--connection-limit`: The connection limit of the listener.
- `--default-pool`: ID of the default pool.
- `--protocol`: The listener protocol. Enumeration type: `http`, `https`, `tcp`.
- `--port`: The listener port.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policies`
{: #load-balancer-listener-policies}

List all load balancer policies.

`ibmcloud is load-balancer-listener-policies LOAD_BALANCER_ID LISTENER_ID [--json]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policy`
{: #load-balancer-listener-policy}

View details of load balancer listener policy.

`ibmcloud is load-balancer-listener-policy LOAD_BALANCER_ID LISTENER_ID POLICY_ID [--json]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `POLICY_ID`: ID of the policy.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policy-create`
{: #load-balancer-listener-policy-create}

Create a load balancer listener policy.

`ibmcloud is load-balancer-listener-policy-create LOAD_BALANCER_ID LISTENER_ID --priority PRIORITY --action ACTION [--name NAME] [--target-id TARGET_ID] [--target-http-status-code TARGET_HTTP_STATUS_CODE] [--target-url TARGET_URL] [--rules LISTENER_POLICY_RULES_JSON | @LISTENER_POLICY_RULES_JSON_FILE] [--json]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `--name`: The new name of the policy.
- `--target-id`: The unique identifier for this load balancer pool, specified with 'forward' action.
- `--target-http-status-code`: The http status code in the redirect response, one of [301, 302, 303, 307, 308], specified with 'redirect' action.
- `--target-url`: The redirect target URL, specified with 'redirect' action.
- `--priority`: Priority of the policy. Lower value indicates higher priority, e.g. 5, range: [1-10].
- `--action`: The policy action, one of [forward, redirect, reject].
- `--rules`: LISTENER_POLICY_RULES_JSON | @LISTENER_POLICY_RULES_JSON_FILE, listener policy rules in json or json file.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policy-delete`
{: #load-balancer-listener-policy-delete}

Delete a policy from a load balancer listener.

`ibmcloud is load-balancer-listener-policy-delete LOAD_BALANCER_ID LISTENER_ID POLICY_ID [-f, --force]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `POLICY_ID`: ID of the policy.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is load-balancer-listener-policy-update`
{: #load-balancer-listener-policy-update}

Update a policy of a load balancer listener.

`ibmcloud is load-balancer-listener-policy-update LOAD_BALANCER_ID LISTENER_ID POLICY_ID [--name NAME] [--priority PRIORITY] [--target-id TARGET_ID] [--target-http-status-code TARGET_HTTP_STATUS_CODE] [--target-url TARGET_URL] [--json]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `POLICY_ID`: ID of the policy.
- `--name`: The user-defined name for this policy. Names must be unique within the load balancer listener the policy resides in.
- `--priority`: Priority of the policy. Lower value indicates higher priority.
- `--target-id`: The unique identifier for this load balancer pool, specified with 'forward' action.
- `--target-http-status-code`: The http status code in the redirect response, one of [301, 302, 303, 307, 308], specified with 'redirect' action.
- `--target-url`: The redirect target URL, specified with 'redirect' action.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policy-rules`
{: #load-balancer-listener-policy-rules}

List all load balancer policy rules.

`ibmcloud is load-balancer-listener-policy-rules LOAD_BALANCER_ID LISTENER_ID POLICY_ID [--json]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `POLICY_ID`: ID of the policy.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policy-rule`
{: #load-balancer-listener-policy-rule}

List single load balancer policy rule.

`ibmcloud is load-balancer-listener-policy-rule LOAD_BALANCER_ID LISTENER_ID POLICY_ID RULE_ID [--json]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `POLICY_ID`: ID of the policy.
- `RULE_ID`: ID of the rule.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policy-rule-create`
{: #load-balancer-listener-policy-rule-create}

Create a load balancer listener policy rule.

`ibmcloud is load-balancer-listener-policy-rule-create LOAD_BALANCER_ID LISTENER_ID POLICY_ID --condition CONDITION --type TYPE --value VALUE [--field FIELD] [--json]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `POLICY_ID`: ID of the policy.
- `--condition`: The condition of the rule, enumeration type: contains, equals, matches_regex.
- `--type`: The type of the rule, enumeration type: header, hostname, path.
- `--`: value      Value to be matched for rule condition.
- `--field`: HTTP header field. This is only applicable to "header" rule type.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policy-rule-update`
{: #load-balancer-listener-policy-rule-update}

Update a rule of a load balancer listener policy.

`ibmcloud is load-balancer-listener-policy-rule-update LOAD_BALANCER_ID LISTENER_ID POLICY_ID RULE_ID [--condition CONDITION] [--type TYPE] [--value VALUE] [--field FIELD] [--json]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `POLICY_ID`: ID of the policy.
- `RULE_ID`: ID of the rule.
- `--condition`: The condition of the rule, enumeration type: contains, equals, matches_regex.
- `--type`: The type of the rule, enumeration type: header, hostname, path.
- `--`: value      Value to be matched for rule condition.
- `--field`: HTTP header field. This is only applicable to "header" rule type.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-listener-policy-rule-delete`
{: #load-balancer-listener-policy-rule-delete}

Delete a policy from a load balancer listener.

`ibmcloud is load-balancer-listener-policy-rule-delete LOAD_BALANCER_ID LISTENER_ID POLICY_ID RULE_ID [-f, --force]`

**options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `LISTENER_ID`: ID of the listener.
- `POLICY_ID`: ID of the policy.
- `RULE_ID`: ID of the rule.
- `--force, -f`: Force the operation without confirmation.

---

## Load Balancer Pools

### `ibmcloud is load-balancer-pools`
{: #load-balancer-pools-anchor}

List all pools of a load balancer.

`ibmcloud is load-balancer-pools LOAD_BALANCER_ID [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-pool`
{: #load-balancer-pool-anchor}

View details of a load balancer pool.

`ibmcloud is load-balancer-pool LOAD_BALANCER_ID POOL_ID [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `POOL_ID`: ID of the pool.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-pool-create`
{: #load-balancer-pool-create}

Create a load balancer pool.

`ibmcloud is load-balancer-pool-create POOL_NAME LOAD_BALANCER_ID ALGORITHM PROTOCOL HEALTH_DELAY HEALTH_RETRIES HEALTH_TIMEOUT HEALTH_TYPE [--health-monitor-url URL][--health-monitor-port PORT][--session-persistence-type TYPE] [--json]`

**Options**
- `POOL_NAME`: Name of the pool.
- `LOAD_BALANCER_ID`: ID of the load balancer.
- `ALGORITHM`: The load balancing algorithm. Enumeration type: `round_robin`, `weighted_round_robin`, least_connections.
- `PROTOCOL`: The pool protocol. Enumeration type: `http`, `tcp`.
- `HEALTH_DELAY`: The health check interval in seconds. The interval must be greater than the timeout value.
- `HEALTH_RETRIES`: The health check maximum retries.
- `HEALTH_TIMEOUT`: The health check timeout in seconds.
- `HEALTH_TYPE`: The pool protocol. Enumeration type: `http`, `tcp`.
- `--health-monitor-url`: The health check url. This option is applicable only to `http` type of HEALTH_TYPE.
- `--session-persistence-type`: The session persistence type, Enumeration type: source_ip
- `--health-monitor-port`: The health check port number.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-pool-delete`
{: #load-balancer-pool-delete}

Delete a pool from a load balancer.

`ibmcloud is load-balancer-pool-delete LOAD_BALANCER_ID POOL_ID [-f, --force]`

**Options**
- `LOAD_BALANCER_ID`: ID of the load balancer.
- `POOL_ID`: ID of the pool.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is load-balancer-pool-update`
{: #load-balancer-pool-update}

Update a pool of a load balancer.

`ibmcloud is load-balancer-pool-update LOAD_BALANCER_ID POOL_ID [--algorithm round_robin | weighted_round_robin | least_connections] [--health-delay DELAY --health-max-retries RETRIES --health-timeout TIMEOUT --health-type TYPE --health-monitor-url URL] [--health-monitor-port PORT] [--protocol http | tcp] [--session-persistence-type TYPE] [--name NEW_NAME] [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `POOL_ID`: ID of the pool.
- `--algorithm`: The load balancing algorithm. Enumeration type: `round_robin`, `weighted_round_robin`, `least_connections`.
- `--health-delay`: The health check interval in seconds. Interval must be greater than timeout value. Minimum: 2, maximum: 60.
- `--health-max-retries`: The health check max retries. Minimum: 2, maximum: 60.
- `--health-monitor-url`: The health check url. This option is applicable only to `http` type of `--health-type`.
- `--health-timeout`: The health check timeout in seconds. Minimum: 1, maximum: 59.
- `--health-type`: The pool protocol. Enumeration type: `http`, `tcp`.
- `--health-monitor-port`: The health check port number.
- `--session-persistence-type`: The session persistence type, Enumeration type: `source_ip`.
- `--session-persistence-cookie-name`: Session persistence cookie name. This option is applicable only to `--session-persistence-type`.
- `--protocol`: The pool protocol. Enumeration type: `http`, `tcp`.
- `--name`: The new name of the pool.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-pool-members`
{: #load-balancer-pool-members}

List all the members of a load balancer pool.

`ibmcloud is load-balancer-pool-members LOAD_BALANCER_ID POOL_ID [--json]`

**Options**

- `LOAD_BALANCER_ID`: ID of the load balancer.
- `POOL_ID`: ID of the pool.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-pool-member`
{: #load-balancer-pool-member}

View details of load balancer pool member.

`ibmcloud is load-balancer-pool-member LOAD_BALANCER_ID POOL_ID MEMBER_ID [--json]`

**Options**
- `LOAD_BALANCER_ID`: ID of the load balancer.
- `POOL_ID`: ID of the pool.
- `MEMBER_ID`: ID of the member.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-pool-member-create`
{: #load-balancer-pool-member-create}

Create a load balancer pool member.

`ibmcloud is load-balancer-pool-member-create LOAD_BALANCER_ID POOL_ID PORT TARGET_ADDRESS [--weight WEIGHT] [--json]`

**Options**
- `LOAD_BALANCER_ID`: ID of the load balancer.
- `POOL_ID`: ID of the pool.
- `PORT`: The port number of the application running in the server member.
- `TARGET_ADDRESS`: The IP address of the pool member.
- `--weight`: Weight of the server member. This option takes effect only when the load balancing algorithm of its belonging pool is `weighted_round_robin`.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-pool-member-update`
{: #load-balancer-pool-member-update}

Update a member of a load balancer pool.

`ibmcloud is load-balancer-pool-member-update LOAD_BALANCER_ID POOL_ID MEMBER_ID [--port PORT] [--target-address TARGET_ADDRESS] [--weight WEIGHT] [--json]`

**Options**
- `LOAD_BALANCER_ID`: ID of the load balancer.
- `POOL_ID`: ID of the pool.
- `MEMBER_ID`: ID of the member.
- `--target-address`: The IP address of the pool member.
- `--weight`: Weight of the server member. This option takes effect only when the load balancing algorithm of its belonging pool is `weighted_round_robin`.
- `--port`: The port number of the application running in the server member.
- `--json`: Format output in JSON.

---

### `ibmcloud is load-balancer-pool-member-delete`
{: #load-balancer-pool-member-delete}

Delete a member from a load balancer pool.

`ibmcloud is load-balancer-pool-member-delete LOAD_BALANCER_ID POOL_ID MEMBER_ID [-f, --force]`

**Options**
- `LOAD_BALANCER_ID`: ID of the load balancer.
- `POOL_ID`: ID of the pool.
- `MEMBER_ID`: ID of the member.
- `--force, -f`: Delete without confirmation.

---

### `ibmcloud is load-balancer-statistics`
{: #load-balancer-statistics}

List all statistics of a load balancer.

`ibmcloud is load-balancer-statistics LOAD_BALANCER_ID [--json]`

**Options**
- `LOAD_BALANCER_ID`: ID of the load balancer.
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
