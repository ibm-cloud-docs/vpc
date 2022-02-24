---

copyright:
  years: 2021, 2022
lastupdated: "2022-02-24"

keywords: metadata, virtual private cloud, instance, virtual server

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Summary of data returned by the instance metadata service
{: #imd-metadata-summary}

Use the instance metadata service to access information about the instance, SSH keys, and placement groups. This topic provides a summary of this data by URI path and metadata key, and describes the metadata.
{: shortdesc}

The API response for each metadata type is presented in JSON format containing metadata key:value pairs.

## Summary of metadata for instances
{: #imd-instance-summary}

Use the information in the Table 1 to understand the type of metadata returned for an instance. To access all metadata, see [Use the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).

| Instance URI path | Metadata key | Description of the metadata |
|-------------------|--------------|-----------------------------|
| /instance | availability_policy | The availability policy for this virtual server instance. |
| /instance	| bandwidth | The total bandwidth in megabits per second that are shared across the virtual server instance's network interfaces and block storage volumes. |
| /instance | boot_volume_attachment | Boot volume attachment, with volume name, CRN, and ID. |
| /instance	| created_at | The date and time that the virtual server instance was created. |
| /instance	| crn | The Cloud Resource Name for this virtual server instance. |
| /instance	| dedicated_host | Collection of the dedicated host references which includes the CRN, deleted hosts, ID, name, and resource_type. |
| /instance	| disks	| A list of the instance's disks. |
| /instance	| gpu | The virtual server instance GPU configuration |
| /instance	| id | The unique identifier for this virtual server instance. |
| /instance	| image	| The image from which the virtual server instance was provisioned. For example, a Debian Linux stock image. |
| /instance	| lifecycle_reasons | The reasons for the current lifecycle_state. |
| /instance	| lifecycle_state | The lifecycle state of the virtual server instance. |
| /instance	| memory | The amount of memory, truncated to whole gibibytes. |
| /instance	| metadata_service | Indicates whether the metadata service endpoint is available to the virtual server instance. (Enabled status is true or false.) |
| /instance	| name | The user-defined name for this virtual server instance (and default system hostname). |
| /instance	| network_interfaces | A list of the virtual server instance's network interfaces, including the primary network interface. |
| /instance	| placement_target | The placement restrictions for the virtual server instance. |
| /instance	| primary_network_interface	| The instance's primary network interface. |
| /instance	| profile |	The globally unique name for this virtual server instance profile. For example, a bx2-2x8, balanced profile. |
| /instance	| startable | Indicates whether the state of the virtual server instance permits a start request. |
| /instance	| status | The status of the instance: deleting, failed, paused, pausing, pending, restarting, resuming, running, starting, stopped, or stopping. |
| /instance	| status_reasons | The reasons for the current status. |
| /instance	| total_network_bandwidth | The amount of bandwidth in megabits per second that is allocated exclusively to instance network interfaces. |
| /instance	| total_volume_bandwidth | The amount of bandwidth in megabits per second allocated exclusively to the instance's block storage volumes. Incrreasing this value results in a corresponding decrease to total_network_bandwidth. |
| /instance	| vcpu | The number of VCPUs assigned by way of a structure that includes count and architecture properties. |
| /instance	| volume_attachments | A list of the virtual server instance's volume attachments. |
| /instance | boot_volume_attachment | Instance's boot volume attachment. |
| /instance	| vpc |	The VPC in which the virtual server instance resides. |
| /instance	| zone | The globally unique name for the zone. |
| /instance_initialization | default_trusted_profile | The default trusted profile configuration specified at virtual server instance creation. |
| /instance_initialization | keys |	A list of references to public SSH keys used at instance initialization. |
| /instance_initialization | password |The administrator password at initialization, encrypted using `encryption_key`, and returned base64-encoded. |
| /instance_initialization | user_data | User data that will be made available when setting up a new virtual server instance. |
| /instance/network_interfaces | network_interfaces	| A list of network interfaces. See /instance/network_interfaces/{id} for details. |
| /instance/network_interfaces/{id}	| allow_ip_spoofing | Indicates whether source IP spoofing is allowed on this interface. If false, source IP spoofing is prevented on this interface. If true, source IP spoofing is allowed on this interface. |
| /instance/network_interfaces/{id}	| created_at | The date and time that the network interface was created. |
| /instance/network_interfaces/{id}	| floating_ips | Array of references to floating IPs associated with the network interface. |
| /instance/network_interfaces/{id}	| id | The unique identifier for the network interface. |
| /instance/network_interfaces/{id}	| name |The user-defined name for the network interface. |
| /instance/network_interfaces/{id}	| port_speed | The network interface port speed in Mbps. |
| /instance/network_interfaces/{id}	| primary_ipv4_address | The primary IPv4 address. |
| /instance/network_interfaces/{id}	| resource_type	| The resource type. |
| /instance/network_interfaces/{id}	| security_groups |	A list of security groups. |
| /instance/network_interfaces/{id}	| status | The status of the network interface. |
| /instance/network_interfaces/{id}	| subnet | The subnet associated with the instance. |
| /instance/network_interfaces/{id}	| type | The type of this network interface as it relates to an instance. |
| /instance/volume_attachments/{id} | bandwidth | The maximum bandwidth in megabits per second for the volume when attached to this instance. This may be lower than the volume bandwidth depending on the configuration of the instance. |
| /instance/volume_attachments/{id}	| created_at | The date and time that the volume was attached. |
| /instance/volume_attachments/{id}	| delete_volume_on_instance_delete | If set to `true`, when deleting the instance, the volume is also deleted. |
| /instance/volume_attachments/{id}	| device | Information about how the volume is exposed to the instance operating system. |
| /instance/volume_attachments/{id}	| id | The unique identifier for the volume attachment. |
| /instance/volume_attachments/{id}	| name | The user-defined name for the volume attachment. |
| /instance/volume_attachments/{id}	| status | The status of the volume attachment: attached, attaching, deleting, or detaching. |
| /instance/volume_attachments/{id}	| type | The type of volume attachment, boot or data. |
| /instance/volume_attachments/{id}	| volume | The attached volume, which contains the CRN, ID, name, and `deleted` value, when the volume is deleted. |
{: caption="Table 1. Metadata for an instance" caption-side="bottom"}

## Summary of metadata for SSH keys
{: #imd-sshkeys-summary}

Use the information in the Table 2 to understand the type of metadata returned for SSH keys. 

| SSH Key URI path | Metadata key | Description of the metadata |
|------------------|--------------|-----------------------------|
| /keys	| keys | A collection of keys. See keys/{id} for details. |
| /keys/{id} | created_at |	The date and time that the key was created. |
| /keys/{id} | crn | The Cloud Resource Name for this key. |
| /keys/{id} | fingerprint | The fingerprint for this key. The value is returned base64-encoded and prefixed with the hash algorithm (always SHA256). |
| /keys/{id} | id | The unique identifier for the key. |
| /keys/{id} | length | The length of this key, in bits. |
| /keys/{id} | name	| The unique user-defined name for this key. If unspecified, the name will be a hyphenated list of randomly selected words (for example, "elderly-mountain-troup-opponent".) |
public_key | The public SSH key. |
| /keys/{id} | type | The crypto-system used by this key. |
{: caption="Table 2. Metadata for SSH keys" caption-side="bottom"}


## Summary of metadata for placement groups
{: #imd-placement-group-summary}

Use the information in the Table 3 to understand the type of metadata returned for placement groups. 

| Placement group URI path | Metadata key | Description of the metadata |
|--------------------------|--------------|-----------------------------|
| /placement_groups | placement_groups | A collection of placement groups. See placement_groups/{id} for details. |
| /placement_groups/{id} | created_at |	The date and time that the placement group was created. |
| /placement_groups/{id} | crn | The Cloud Resource Name for this placement group. |
| /placement_groups/{id} | id |	The unique identifier for this placement group. |
| /placement_groups/{id} | lifecycle_state | The lifecycle state of the placement group. |
| /placement_groups/{id} | name | The user-defined name for the placement group. |
| /placement_groups/{id} | resource_type | The resource type. |
| /placement_groups/{id} | strategy | The strategy for this placement group. |
{: caption="Table 3. Metadata for placement groups" caption-side="bottom"}

## Next steps
{: #imd-summary-next-steps}

[Retrieve data using the metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
