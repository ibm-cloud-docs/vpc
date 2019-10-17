---

copyright:
  years: 2018, 2019
lastupdated: "2019-10-17"

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
* [Compute commands](#compute-clis)
* [Regions and zones commands](#geography)
* [Storage CLI commands](#storage)

## Prerequisites
{: #cli-ref-prereqs}

1. Install the [IBM Cloud CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/cli){: new_window}.

2. Install or update the VPC plug-in.

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  To update:

  ```
  ibmcloud plugin update
  ```
  {: pre}

  To view installed plug-ins and versions:

  ```
  ibmcloud plugin list
  ```
  {: pre}

4. Make sure the CLI plug-in targets generation 2 of the VPC infrastructure service. To view the current target generation, run the following command:

  ```
  ibmcloud is target
  ```
  {: pre}

  If the plug-in is not currently targeting generation 2, run the following command: 

  ```
  ibmcloud is target --gen 2
  ```
  {: pre}

## Network commands
{: #network}

This section provides information about CLI commands for network functionality.

## Floating IPs
{: #floating-ips-cli-ref}


### `ibmcloud is floating-ip`
{: #floating-ips-cli}

View details of a floating IP.

`ibmcloud is floating-ip FLOATING_IP [--json]`

#### Command options
{: #command-options-floating-ips-cli}

- `FLOATING_IP`: ID of the floating IP.

- `--json`: Format output in JSON.

---

### `ibmcloud is floating-ip-release`
{: #floating-ip}

Release a floating IP.

`ibmcloud is floating-ip-release FLOATING_IP [-f, --force]`

#### Command options
{: #command-options-floating-ip}

- `FLOATING_IP`: ID of the floating IP.

- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is floating-ip-reserve`
{: #floating-ip-reserve}

Reserve a floating IP.

`ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE_NAME | --nic-id NIC_ID) [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

#### Command options
{: #command-options-floating-ip-reserve}

- `FLOATING_IP_NAME`: Name of the floating IP.
- `--zone`: Name of the targetzone. This option is mutually exclusive with --nic-id.
- `--nic-id`: ID of the target network interface. This option is mutually exclusive with --zone.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is floating-ip-update`
{: #floating-ip-update}

Update a floating ip.

`ibmcloud is floating-ip-update FLOATING_IP [--name NEW_NAME] [--nic-id NIC_ID] [--json]`


#### Command options
{: #command-options-floating-ip-update}

- `FLOATING_IP`: ID of the floating IP.
- `--name`: New name of the floating ip.
- `--nic-id`: ID of the network interface to associate.
- `--json`: Format output in JSON.

---

### `ibmcloud is floating-ips`
{: #floating-ip-list}

List all floating ips.

`ibmcloud is floating-ips [--json]`

#### Command options
{: #command-options-floating-ip-list}

- `--json`: Format output in JSON.

---
## Public gateways
{: #public-gateways}

### `ibmcloud is public-gateway`
{: #public-gateway-details}

View details of a public gateway.

`ibmcloud is public-gateway GATEWAY [--json]`

#### Command options
{: #command-options-public-gateway-details}

- `GATEWAY`: ID of the public gateway.
- `--json`: Format output in JSON.

---

### `ibmcloud is public-gateway-create`
{: #public-gateway-create}

Create a public gateway.

`ibmcloud is public-gateway-create GATEWAY_NAME VPC ZONE_NAME [--floating-ip-id IP_ID | --floating-ip-address IP_ADDRESS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

#### Command options
{: #command-options-public-gateway-create}

- `GATEWAY_NAME`: Name of the public gateway.
- `VPC`: ID of the VPC.
- `ZONE_NAME`: Name of the zone.
- `--floating-ip-id`: ID of the floating IP bound to the public gateway.
- `--floating-ip-address`: IP address of the floating IP bound to the public gateway.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is public-gateway-delete`
{: #public-gateway-delete}

Delete a public gateway.

`ibmcloud is public-gateway-delete GATEWAY [-f, --force]`

#### Command options
{: #command-options-public-gateway-delete}

- `GATEWAY`: ID of the public gateway.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is public-gateway-update`
{: #public-gateway-update}

Update a public gateway.

`ibmcloud is public-gateway-update GATEWAY --name NEW_NAME [--json]`

#### Command options
{: #command-options-public-gateway-update}

- `GATEWAY`: ID of the public gateway.
- `--name`: New name of the public gateway.
- `--json`: Format output in JSON.

---

### `ibmcloud is public-gateways`
{: #public-gateway-list-all}

List all public gateways.

`ibmcloud is public-gateways [--json]`

#### Command options
{: #command-options-public-gateway-list-all}

- `--json`: Format output in JSON.

---
## Security groups
{: #security-groups-cli-ref}

### `ibmcloud is security-group`
{: #security-groups-cli-details}

View details of a security group.

`ibmcloud is security-group GROUP [--json]`

#### Command options
{: #command-options-security-groups-cli-details}

- `GROUP`: ID of the security group.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-create`
{: #security-group-create}

Create a security group.

`ibmcloud is security-group-create GROUP_NAME VPC [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

#### Command options
{: #command-options-security-group-create}

- `GROUP_NAME`: Name of the subnet.
- `VPC`: ID of the VPC.
- `--resource-group-id`: ID of the resource group. This option is exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---


### `ibmcloud is security-group-delete`
{: #security-group-delete}

Delete a security group.

`ibmcloud is security-group-delete GROUP [-f, --force]`

#### Command options
{: #command-options-security-group-delete}

- `GROUP`: ID of the security group.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is security-group-network-interface`
{: #security-group-network-interface-details}

View details of a network interface of a security group.

`ibmcloud is security-group-network-interface GROUP NIC [--json]`

#### Command options
{: #command-options-security-group-network-interface-details}

- `GROUP`: ID of the security group.
- `NIC`: ID of the network interface.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-network-interface-add`
{: #security-group-network-interface-add}

Add a network interface to a security group.

`ibmcloud is security-group-network-interface-add GROUP NIC [--json]`

#### Command options
{: #command-options-security-group-network-interface-add}

- `GROUP`: ID of the security group.
- `NIC`: ID of the network interface.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-network-interface-remove`
{: #security-group-network-interface-remove}

Remove a network interface from a security group.

`ibmcloud is security-group-network-interface-remove GROUP_ID NIC_ID [-f, --force]`

#### Command options
{: #command-options-security-group-network-interface-remove}

- `GROUP`: ID of the security group.
- `NIC`: ID of the network interface.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is security-group-network-interfaces`
{: #security-group-network-interfaces}

List all network interfaces of a security group.

`ibmcloud is security-group-network-interfaces GROUP [--json]`

#### Command options
{: #command-options-security-group-network-interfaces}

- `GROUP`: ID of the security group.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-rule`
{: #security-group-rule}

View details of a security group rule.

`ibmcloud is security-group-rule GROUP RULE_ID [--json]`

#### Command options
{: #command-options-security-group-rule}

- `GROUP`: ID of the security group.
- `RULE_ID`: ID of the rule.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-rule-add`
{: #security-group-rule-add}

Add a rule to a security group.

`ibmcloud is security-group-rule-add GROUP DIRECTION PROTOCOL [--remote REMOTE_ADDRESS | CIDR_BLOCK | SECURITY_GROUP_ID] [--icmp-type ICMP_TYPE [--icmp-code ICMP_CODE]] [--port-max PORT_MAX --port-min PORT_MIN] [--json]`

#### Command options
{: #command-options-security-group-rule-add}

- `GROUP`: ID of the security group.
- `DIRECTION`: Direction of traffic to enforce. Enumeration type: `inbound` or `outbound`.
- `PROTOCOL`: Protocol to enforce. Enumeration type: `all`, `icmp`, `tcp`, or `udp`.
- `--remote`: The set of network interfaces from which this rule allows traffic, Can be specified as either an REMOTE_ADDRESS, CIDR_BLOCK and SECURITY_GROUP_ID.
- `--icmp-type`: ICMP traffic type to allow. Valid values are 0 - 254. This option is specified only when protocol is set to icmp. If unspecified, all types are allowed.
- `--icmp-code`: ICMP traffic code to allow. Valid values are 0 - 255. This option is specified only when protocol is set to icmp. If unspecified, all codes are allowed.
- `--port-min`: Minimum port number. Valid values are 1 - 65535. This option is specified only when protocol is set to tcp or udp (default: 1).
- `--port-max`: Maximum port number. Valid values are 1 - 65535. This option is specified only when protocol is set to tcp or udp (default: 65535).
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-rule-delete`
{: #security-group-rule-delete}

Delete a rule from a security group.

`ibmcloud is security-group-rule-delete GROUP RULE_ID [-f, --force]`

#### Command options
{: #command-options-security-group-rule-delete}

- `GROUP`: ID of the security group.
- `RULE_ID`: ID of the rule.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is security-group-rule-update`
{: #security-group-rule-update}

Update a rule of a security group.

`ibmcloud is security-group-rule-update GROUP RULE_ID [--direction DIRECTION] [--remote REMOTE_ADDRESS | CIDR_BLOCK | SECURITY_GROUP_ID] [--icmp-type ICMP_TYPE [--icmp-code ICMP_CODE]] [--port-max PORT_MAX --port-min PORT_MIN] [--json]`

#### Command options
{: #command-options-security-group-rule-update}

- `GROUP`: ID of the security group.
- `RULE_ID`: ID of the rule.
- `--direction`: Direction of traffic to enforce. Valid values are inbound or outbound.
- `--remote`: The set of network interfaces from which this rule allows traffic, Can be specified as either an REMOTE_ADDRESS, CIDR_BLOCK and SECURITY_GROUP_ID.
- `--icmp-type`: ICMP traffic type to allow. Valid values are 0 - 254. This option is specified only when protocol is set to icmp.
- `--icmp-code`: ICMP traffic code to allow. Valid values are 0 to 255. This option is specified only when protocol is set to icmp.
- `--port-min`: Minimum port number. Valid values are 1 - 65535. This option is specified only when protocol is set to tcp or udp.
- `--port-max`: Maximum port number. Valid values are 1 - 65535. This option is specified only when protocol is set to tcp or udp.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-rules`
{: #security-group-rules}

List all rules of a security group.

`ibmcloud is security-group-rules GROUP [--json]`

#### Command options
{: #command-options-security-group-rules}

- `GROUP`: ID of the security group.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-group-update`
{: #security-group-update}

Update a security group.

`ibmcloud is security-group-update GROUP [--name NEW_NAME] [--json]`

#### Command options
{: #command-options-security-group-update}

- `GROUP`: ID of the security group.
- `--name`: New name of the security group.
- `--json`: Format output in JSON.

---

### `ibmcloud is security-groups`
{: #security-groups-cli}

List all security groups.

`ibmcloud is security-groups [--json]`

#### Command options
{: #command-options-security-groups-cli}

- `--json`: Format output in JSON.

---

## Subnets
{: #subnet-cli-ref}

### `ibmcloud is subnet`
{: #subnet-details}

View details of a subnet.

`ibmcloud is subnet SUBNET [--json]`

#### Command options
{: #command-options-subnet-details}

- `SUBNET`: ID of the subnet.
- `--json`: Format output in JSON.

---

### `ibmcloud is subnet-create`
{: #subnet-create}

Create a subnet.

`ibmcloud is subnet-create SUBNET_NAME VPC (--ipv4-cidr-block CIDR_BLOCK | (--ipv4-address-count ADDR_COUNT --zone ZONE_NAME)) [--network-acl-id NETWORK_ACL_ID] [--public-gateway-id PUBLIC_GATEWAY_ID] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`


#### Command options
{: #command-options-subnet-create}

- `SUBNET_NAME`: Name of the subnet.
- `VPC`: ID of the VPC.
- `--ipv4-cidr-block`: the IPv4 range of the subnet. This is exclusive with --ipv4-address-count.
- `--ipv4-address-count`: The total number of IPv4 addresses required. Must be a power of 2 and minimum value is 8. This option is mutually exclusive with --ipv4-cidr-block.
- `--zone`: Name of the zone.
- `--public-gateway-id`: The ID of the public gateway.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is subnet-delete`
{: #subnet-delete}

Delete a subnet.

`ibmcloud is subnet-delete SUBNET [-f, --force]`

#### Command options
{: #command-options-subnet-delete}

- `SUBNET`: ID of the subnet.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is subnet-update`
{: #subnet-update}

Update a subnet.

`ibmcloud is subnet-update SUBNET [--name NEW_NAME] [--public-gateway-id PUBLIC_GATEWAY_ID] [--json]`


#### Command options
{: #command-options-subnet-update}

- `SUBNET`: ID of the subnet.
- `--name`: New name of the subnet.
- `--public-gateway-id`: ID of the public gateway.
- `--json`: Format output in JSON.

---

### `ibmcloud is subnets`
{: #subnets-list}

List all subnets.

`ibmcloud is subnets [--json]`

#### Command options
{: #command-options-subnets-list}

- `--json`: Format output in JSON.

---

### `ibmcloud is subnet-public-gateway`
{: #subnet-public-gateway}

View details of public gateway that is attached to the subnet.

`ibmcloud is subnet-public-gateway SUBNET [--json]`

#### Command options
{: #command-options-subnet-public-gateway}

- `SUBNET`: ID of the subnet.
- `--json`: Format output in JSON.

---

### `ibmcloud is subnet-public-gateway-detach`
{: #subnet-public-gateway-detach}

Detach the public gateway from a subnet.

`ibmcloud is subnet-public-gateway-detach SUBNET`

### Command options

- `SUBNET`: ID of the subnet.

---

## VPCs
{: #vpcs-cli-ref}

### `ibmcloud is vpc`
{: #vpc-details}

View details of a VPC.

`ibmcloud is vpc VPC [--json]`

#### Command options
{: #command-options-vpc-details}

- `VPC`: ID of the VPC.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-address-prefix`
{: #vpc-address-prefix}

View details of a VPC address prefix.

`ibmcloud is vpc-address-prefix VPC PREFIX [--json]`

#### Command options
{: #command-options-vpc-address-prefix}

- `VPC`: ID of the VPC.
- `PREFIX`: ID of the VPC address prefix.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-address-prefix-create`
{: #vpc-address-prefix-create}


Create an address prefix.

`ibmcloud is vpc-address-prefix-create PREFIX_NAME VPC ZONE_NAME CIDR [--json]`

#### Command options
{: #command-options-vpc-address-prefix-create}

- `PREFIX_NAME`: Name of the address prefix.
- `VPC`: ID of the VPC.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-address-prefix-delete`
{: #vpc-address-prefix-delete}

Delete an address prefix.

`ibmcloud is vpc-address-prefix-delete VPC PREFIX [--force]`

#### Command options
{: #command-options-vpc-address-prefix-delete}

- `VPC`: ID of the VPC.
- `PREFIX`: ID of the VPC address prefix.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is vpc-address-prefix-update`
{: #vpc-address-prefix-update}

Update an address prefix.

`ibmcloud is vpc-address-prefix-update VPC PREFIX [--name NEW_NAME] [--json]`

#### Command options
{: #command-options-vpc-address-prefix-update}

- `VPC`: ID of the VPC.
- `PREFIX`: ID of the VPC address prefix.
- `--name`: New name of the address prefix.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-address-prefixes`
{: #vpc-address-prefixes}

List all address prefixes.

`ibmcloud is vpc-address-prefixes VPC [--json]`

#### Command options
{: #command-options-vpc-address-prefixes}

- `VPC`: ID of the VPC.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-create`
{: #vpc-create}

Create a VPC.

`ibmcloud is vpc-create VPC_NAME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

#### Command options
{: #command-options-vpc-create}

- `VPC_NAME`: Name of the VPC
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-default-security-group`
{: #vpc-default-security-group}

View details of the default security group of a VPC.

`ibmcloud is vpc-default-security-group VPC [--json]`

#### Command options
{: #command-options-vpc-default-security-group}

- `VPC`: ID of the VPC.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpc-delete`
{: #vpc-delete}

Delete a VPC.

`ibmcloud is vpc-delete VPC [-f, --force]`

#### Command options
{: #command-options-vpc-delete}

- `VPC`: ID of the VPC.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is vpc-update`
{: #vpc-update}

Update a VPC.

`ibmcloud is vpc-update VPC [--name NEW_NAME] [--json]`

#### Command options
{: #command-options-vpc-update}

- `VPC`: ID of the VPC.
- `--name`: New name of the VPC.
- `--json`: Format output in JSON.

---

### `ibmcloud is vpcs`
{: #vpc-list}

List all VPCs.

`ibmcloud is vpcs [--json]`

#### Command options
{: #command-options-vpc-list}

- `--json`: Format output in JSON.

---

## Compute commands
{: #compute-clis}

This section provides information about CLI commands for compute functionality.

## Images
{: #compute-images}

### `ibmcloud is operating-systems`
{: #operating-systems}

List all operating systems.

`ibmcloud is operating-systems [--json]`

#### Command options
{: #command-options-compute-images}

- `--json`: Format output in JSON.

---

### `ibmcloud is operating-system`
{: #operating-system}

View details of an operating system.

`ibmcloud is operating-system OPERATING_SYSTEM_NAME [--json]`

#### Command options
{: #command-options-operating-system}

- `OPERATING_SYSTEM_NAME`: Name of the operating system
- `--json`: Format output in JSON.

---

### `ibmcloud is image`
{: #image-details}

View details of an image.

`ibmcloud is image IMAGE [--json]`

#### Command options
{: #command-options-image-details}

- `IMAGE`: ID of the image.
- `--json`: Format output in JSON.

---

### `ibmcloud is images`
{: #image-list-region}

List all images in the region.

`ibmcloud is images [--json]`

#### Command options
{: #command-options-image-list-region}

- `--json`: Format output in JSON.

---

### `ibmcloud is image-create`
{: #image-create}

Create an image.

`ibmcloud is image-create IMAGE_NAME --file IMAGE_FILE_LOCATION --os-name OPERATING_SYSTEM_NAME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

#### Command options
{: #command-options-image-create}

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

`ibmcloud is image-update IMAGE [--name NEW_NAME] [--json]`

#### Command options
{: #command-options-image-update}

- `IMAGE`: ID of the image.
- `--name`: New name of the image.
- `--json`: Format output in JSON.

---

### `ibmcloud is image-delete`
{: #image-delete}

Delete an image.

`ibmcloud is image-delete IMAGE [-f, --force]`

#### Command options
{: #command-options-image-delete}

- `IMAGE`: ID of the image.
- `--force, -f`: Force the operation without confirmation.

---

## Instances
{: #instances}

### `ibmcloud is instance`
{: #instance-details}

View details of a virtual server instance.

`ibmcloud is instance INSTANCE [--json]`

#### Command options
{: #command-options-instance-details}

- `INSTANCE`: ID of the instance.
- `--json`: Format output in JSON.

---

### `ibmcloud is instances`
{: #instances-list}

List all virtual server instances.

`ibmcloud is instances [--json]`

#### Command options
{: #command-options-instances-list}

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-create`
-{: #instance-create}

-Create a virtual server instance.

`ibmcloud is instance-create INSTANCE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET --image-id IMAGE_ID [--boot-volume BOOT_VOLUME_JSON | @BOOT_VOLUME_JSON_FILE] [--volume-attach VOLUME_ATTACH_JSON | @VOLUME_ATTACH_JSON_FILE] [--key-ids IDS] [--user-data DATA] [--network-interface NETWORK_INTERFACE_JSON | @NETWORK_INTERFACE_JSON_FILE] [--security-group-ids SECURITY_GROUP_IDS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

#### Command options
{: #command-options-instance-create}

- `INSTANCE_NAME`: Name of the server instance.
- `VPC`: ID of the VPC.
- `ZONE_NAME`: Name of the zone.
- `PROFILE_NAME`: Name of the profile.
- `SUBNET`: ID of the subnet.
- `--image-id`: ID of the image.
- `--boot-volume`: BOOT_VOLUME_JSON|@BOOT_VOLUME_JSON_FILE, boot volume attachment in JSON or JSON file.
- `--volume-attach`: VOLUME_ATTACH_JSON|@VOLUME_ATTACH_JSON_FILE, volume attachment in JSON or JSON file.
- `--security-group-ids`: Comma separated security group IDs for primary network interface.
- `--key-ids`: Comma-separated IDs of SSH keys.
- `--user-data`: data|@data-file. User data to transfer to the virtual server instance.
- `--network-interface`: NETWORK_INTERFACE_JSON|@NETWORK_INTERFACE_JSON_FILE. The network interface attachment in JSON or JSON file. For more information about the required format of NETWORK_INTERFACE_JSON, see primary_network_interface in the [API reference](https://{DomainName}/apidocs/vpc#create-an-instance){: new_window}.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-delete`
{: #instance-delete}

Delete a virtual server instance.

`ibmcloud is instance-delete INSTANCE [-f, --force]`

#### Command options
{: #command-options-instance-delete}

- `INSTANCE`: ID of the instance.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-initialization-values`
{: #instance-initialization-values}

View initialization details of a server instance.

`ibmcloud is instance-initialization-values INSTANCE [--private-key (KEY | @KEY_FILE)] [--json]`

#### Command options
{: #command-options-instance-initialization-values}

- `INSTANCE`: ID of the instance.
- `--private-key`: key|@key-file. The private key in PEM format to decrypt Windows administrator default password.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface`
{: #instance-network-interface}

View details of a network interface of a virtual server instance.

`ibmcloud is instance-network-interface INSTANCE NIC [--json]`

#### Command options
{: #command-options-instance-network-interface}

- `INSTANCE`: ID of the instance.
- `NIC`: ID of the network interface.
- `--json`: Format output in JSON.

--- 

### `ibmcloud is instance-network-interface-create`
{: #instance-network-interface-create}


#### Create a network interface for a server instance.
{: #instance-network-interface-create-for-server}

`ibmcloud is instance-network-interface-create NIC_NAME INSTANCE SUBNET [--ipv4 IPV4_ADDRESS] [--security-group-id SECURITY_GROUP_ID1 --security-group-id SECURITY_GROUP_ID2 ...] [--json]`

#### Command options
{: #command-options-instance-network-interface-create}

- `NIC_NAME`: Name of the network interface.
- `INSTANCE`: ID of the instance.
- `SUBNET`: ID of the subnet.
- `--ipv4`: Primary IPv4 address.
- `--security-group-id`: ID of the security group.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface-delete`
{: #instance-network-interface-delete}

Remove a network interface from a server instance.

`ibmcloud is instance-network-interface-delete INSTANCE NIC [-f, --force]`

#### Command options
{: #command-options-instance-network-interface-delete}

- `INSTANCE`: ID of the instance.
- `NIC`: ID of the network interface.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-network-interface-floating-ip`
{: #instance-network-interface-floating-ip}

View details of a floating IP associated with a network interface.

`ibmcloud is instance-network-interface-floating-ip INSTANCE NIC FLOATING [--json]`

#### Command options
{: #command-options-instance-network-interface-floating-ip}

- `INSTANCE`: ID of the instance.
- `NIC`: ID of the network interface.
- `FLOATING_IP`: ID of the floating IP.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface-floating-ip-add`
{: #instance-network-interface-floating-ip-add}

Associate a floating IP with a network interface.

`ibmcloud is instance-network-interface-floating-ip-add INSTANCE NIC FLOATING_IP [--json]`

#### Command options
{: #command-options-instance-network-interface-floating-ip-add}

- `INSTANCE`: ID of the instance.
- `NIC`: ID of the network interface.
- `FLOATING_IP`: ID of the floating IP.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface-floating-ip-remove`
{: #instance-network-interface-floating-remove}

Disassociate a floating IP from a network interface.

`ibmcloud is instance-network-interface-floating-ip-remove INSTANCE NIC FLOATING [-f, --force]`

#### Command options
{: #command-options-instance-network-interface-floating-remove}

- `INSTANCE`: ID of the instance.
- `NIC`: ID of the network interface.
- `FLOATING_IP`: ID of the floating IP.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-network-interface-floating-ips`
{: #instance-network-interface-floating-ips}

List all floating IPs associated with a network interface.

`ibmcloud is instance-network-interface-floating-ips INSTANCE NIC [--json]`

#### Command options
{: #command-options-instance-network-interface-floating-ips}

- `INSTANCE`: ID of the instance.
- `NIC`: ID of the network interface.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interface-update`
{: #instance-network-interface-update}

Update a network interface of a virtual server instance.

`ibmcloud is instance-network-interface-update INSTANCE NIC [--name NEW_NAME] [--json]`

#### Command options
{: #command-options-#instance-network-interface-update}

- `INSTANCE`: ID of the instance.
- `NIC`: ID of the network interface.
- `--name`: New name of NIC.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-network-interfaces`
{: #instance-network-interface-list}

List all network interfaces of a virtual server instance.

`ibmcloud is instance-network-interfaces INSTANCE [--json]`

#### Command options
{: #command-options-instance-network-interface-list}

- `INSTANCE`: ID of the instance.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-profile`
{: #instance-profile}

View details of a virtual server instance profile.

`ibmcloud is instance-profile PROFILE_NAME [--json]`

#### Command options
{: #command-options-instance-profile}

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-profiles`
{: #instance-profiles}

List all virtual server instance profiles in the region.

`ibmcloud is instance-profiles [--json]`

#### Command options
{: #command-options-instance-profiles}

- `--json`: Format output in JSON.

---

### `ibmcloud is instance-reboot`
{: #instance-reboot}

Restart the operating system of a virtual server instance.

`ibmcloud is instance-reboot INSTANCE [--no-wait] [-f, --force] [--json]`

#### Command options
{: #command-options-instance-reboot}

- `INSTANCE`: ID of the instance.
- `--no-wait`: Execute the action immediately and cancel any queued actions.
- `--json`: Format output in JSON.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-start`
{: #instance-start}

Start a virtual server instance.

`ibmcloud is instance-start INSTANCE [--json]`

#### Command options
{: #command-options-instance-start}

- `INSTANCE`: ID of the instance.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-stop`
{: #instance-stop}

Stop a virtual server instance.

`ibmcloud is instance-stop INSTANCE [--no-wait]  [-f, --force] [--json]`

#### Command options
{: #command-options-instance-stop}

- `INSTANCE`: ID of the instance.
- `--no-wait`: Execute the action immediately and cancel any queued actions.
- `--json`: Format output in JSON.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is instance-update`
{: #instance-update}

Update a virtual server instance.

`ibmcloud is instance-update INSTANCE [--name NEW_NAME] [--json]`

#### Command options
{: #command-options-instance-update}

- `INSTANCE`: ID of the instance.
- `--name`: New name of the virtual server instance.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-volume-attachment`
{: #instance-volume-attachment}

View details of a volume attachment.

`ibmcloud is instance-volume-attachment INSTANCE VOLUME_ATTACHMENT [--json]`

#### Command options
{: #command-options-instance-volume-attachment}

- `INSTANCE`: ID of the instance.
- `VOLUME_ATTACHMENT`: ID of the volume attachment.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-volume-attachments`
{: #instance-volume-attachments}

List all volume attachments to an instance.

`ibmcloud is instance-volume-attachments INSTANCE [--json]`

#### Command options
{: #command-options-instance-volume-attachments}

- `INSTANCE`: ID of the instance.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-volume-attachment-add`
{: #instance-volume-attachment-add}

Create a volume attachment, connecting a volume to an instance.

`ibmcloud is instance-volume-attachment-add NAME INSTANCE VOLUME [--auto-delete true | false] [--json]`

#### Command options
{: #command-options-instance-volume-attachment-add}

- `NAME`: Name of the volume attachment.
- `INSTANCE`: ID of the instance.
- `VOLUME`: ID of the volume.
- `--auto-delete`: The attached volume is deleted when the instance is deleted. The default is false.
- `--json`: Format output in JSON.

---

### `ibmcloud is instance-volume-attachment-detach`
{: #instance-volume-attachment-detach}

Delete a volume attachment, detaching a volume from an instance.

`ibmcloud is instance-volume-attachment-detach INSTANCE VOLUME_ATTACHMENT [-f, --force]`

#### Command options
{: #command-options-instance-volume-attachment-detach}

- `INSTANCE`: ID of the instance.
- `VOLUME_ATTACHMENT`: ID of the volume attachment.
- `--force`, `-f`: Force the operation without confirmation.

---

### `ibmcloud is instance-volume-attachment-update`
{: #instance-volume-attachment-update}

Update a volume attachment.

`ibmcloud is instance-volume-attachment-update INSTANCE VOLUME_ATTACHMENT [--name NEW_NAME] [--auto-delete true | false] [--json]`

#### Command options
{: #command-options-instance-volume-attachment-update}

- `INSTANCE`: ID of the instance.
- `VOLUME_ATTACHMENT`: ID of the volume attachment.
- `--name`: New name of the volume.
- `--auto-delete`: The attached volume will be deleted when deleting the instance, default is false.
- `--json`: Format output in JSON.

---

## Keys
{: #keys}

### `ibmcloud is key`
{: #key-details}

View details of a key.

`ibmcloud is key KEY [--json]`

#### Command options
{: #command-options-key-details}

- `KEY`: ID of the key.
- `--json`: Format output in JSON.

---

### `ibmcloud is key-create`
{: #key-create}

Import an RSA public key.

`ibmcloud is key-create KEY_NAME (KEY | @KEY_FILE) [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

#### Command options
{: #command-options-key-create}

- `KEY_NAME`: Name of the key.
- `KEY`: key|@key-file. The public SSH key to be imported into the system.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is key-delete`
{: #key-delete}

Delete a key.

`ibmcloud is key-delete KEY [-f, --force]`

#### Command options
{: #command-options-key-delete}

- `KEY`: ID of the key.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is key-update`
{: #key-update}

Update the name of a key.

`ibmcloud is key-update KEY --name NEW_NAME [--json]`

#### Command options
{: #command-options-key-update}

- `KEY`: ID of the key.
- `--name`: New name for the key.
- `--json`: Format output in JSON.

---

### `ibmcloud is keys`
{: #key-list}

List all keys.

`ibmcloud is keys [--json]`

#### Command options
{: #command-options-key-list}

- `--json`: Format output in JSON.

---

## Regions and zones commands
{: #geography}

This section provides information about CLI commands for working with regions and zones.

## Regions
{: #regions}

### `ibmcloud is region`
{: #region-details}

View details of a region.

`ibmcloud is region REGION_NAME [--json]`

#### Command options
{: #command-options-region-details}

- `--json`: Format output in JSON.

---

### `ibmcloud is regions`
{: #region-list}

List all regions.

`ibmcloud is regions [--json]`

#### Command options
{: #command-options-region-list}

- `--json`: Format output in JSON.

---

## Zones
{: #zones}

### `ibmcloud is zone`
{: #zone-details}

View details of a zone in the target region.

`ibmcloud is zone ZONE_NAME [--json]`

#### Command options
{: #command-options-zone-details}

- `--json`: Format output in JSON.

---

### `ibmcloud is zones`
{: #zone-list}

List all zones in the target region.

`ibmcloud is zones [--json]`

#### Command options
{: #command-options-zone-list}

- `--json`: Format output in JSON.

---

## Storage CLI commands
{: #storage}

### `ibmcloud is volumes`
{: #volume-all-details}

View details of all volumes.

#### Command options
{: #command-options-volume-all-details}

`ibmcloud is volumes [--json]`

---

### `ibmcloud is volume`
{: #volume-details}

View details of a volume.

`ibmcloud is volume VOLUME [--json]`

#### Command options
{: #command-options-volume-details}

- `VOLUME`: ID of the volume.
- `--json`: Format output in JSON.

---

### `ibmcloud is volume-create`
{: #volume-create}

Create a volume.

`ibmcloud is volume-create VOLUME_NAME PROFILE_NAME ZONE_NAME [--capacity CAPACITY] [--iops IOPS] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--json]`

#### Command options
{: #command-options-volume-create}

- `VOLUME_NAME`: Name of the volume you specify.
- `PROFILE_NAME`: Name of the volume profile, for example, general-purpose.
- `ZONE_NAME`: Name of zone.  For example, us-south-1.
- `--capacity`: Capacity of the volume in GB, 10 - 2000. Default to 100.
- `--iops`: Input/Output Operations Per Second for the volume.
- `--resource-group-id`: ID of the resource group. This option is mutually exclusive with --resource-group-name.
- `--resource-group-name`: Name of the resource group. This option is mutually exclusive with --resource-group-id.
- `--json`: Format output in JSON.

---

### `ibmcloud is volume-delete`
{: #volume-delete}

Delete a volume.

`ibmcloud is volume-delete VOLUME [-f, --force]`

#### Command options
{: #command-options-volume-delete}

- `VOLUME`: ID of the volume.
- `--force, -f`: Force the operation without confirmation.

---

### `ibmcloud is volume-profiles`
{: #volume-profiles}

List all volume profiles.

`ibmcloud is volume-profiles [--json]`

#### Command options
{: #command-options-volume-profiles}

- `--json`: Format output in JSON.

---

### `ibmcloud is volume-profile`
{: #volume-profile}

View details of a volume profile. 

`ibmcloud is volume-profile PROFILE_NAME [--json]`

Volume profile names are 10iops-tier, 5iops-tier, general-purpose, and custom.

#### Command options
{: #command-options-volume-profile}

- `--json`: Format output in JSON.

---

### `ibmcloud is volume-update`
{: #volume-update}

Update a volume.

`ibmcloud is volume-update VOLUME [--name NEW_NAME] [--json]`

#### Command options
{: #command-options-volume-update}

- `VOLUME`: ID of the volume.
- `--name`: New name of the volume.
- `--json`: Format output in JSON.

---
