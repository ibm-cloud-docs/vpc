---

copyright:
  years: 2021
lastupdated: "2021-08-23"

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
{:beta: .beta}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Summary of data returned by the metadata service (Beta)
{: #imd-metadata-summary}

Using the instance metadata service, you can access information about the instance, SSH keys, and placement groups. This topic provides a summary of this data by URI path, metadata key, and describes the metadata.
{:shortdesc}

The API response for each metadata type is presented in JSON format containing metadata key:value pairs.

This service is available only to accounts with special approval to preview this beta feature.
{:beta}

## Summary of metadata for instances
{: #imd-instance-summary}

Use the information in the Table 1 to understand the type of metadata returned for an instance. To access all metadata, see [Use the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).

| Instance URI path | Metadata key | Description of the metadata |
|-------------------|--------------|-----------------------------|
| /instance | boot_volume_attachment | Boot volume attachment, with volume name, crn, and ID. |
| /instance	| created_at | The date and time that the virtual server instance was created. |
| /instance	| crn| The Cloud Resource Name for this virtual server instance. |
| /instance	| dedicated_host |Collection of the dedicated host references which includes the crn, deleted hosts, ID, name, and resource_type. |
| /instance	| disks	| A list of the instance's disks. |
| /instance	| gpu | The virtual server instance GPU configuration |
| /instance	| id | The unique identifier for this virtual server instance. |
| /instance	| image	| The image from which the virtual server instance was provisioned. For example, a Debian Linux stock image. |
| /instance	| memory | The amount of memory, truncated to whole gibibytes. |
| /instance	| name | The user-defined name for this virtual server instance (and default system hostname). |
| /instance	| network_interfaces | A list of the virtual server instance's network interfaces, including the primary network interface. |
| /instance	| primary_network_interface	| The instance's primary network interface. |
| /instance	| profile |	The globally unique name for this virtual server instance profile. For example, a bx2-2x8, balanced profile. |
| /instance	| vcpu | The number of VCPUs assigned by way of a structure that includes count and architecture properties. |
| /instance	| volume_attachments | A list of the virtual server instance's volume attachments, including the boot volume attachment. |
| /instance	| vpc |	The VPC in which the virtual server instance resides. |
| /instance	| zone | The globally unique name for the zone. |
| /instance_initialization | keys |	A list of references to public SSH keys used at instance initialization. |
| /instance_initialization | password |The administrator password at initialization, encrypted using `encryption_key`, and returned base64-encoded. |
| /instance_initialization | user_data | User data that will be made available when setting up a new virtual server instance. |
| /instance/network_interfaces | network_interfaces	| A list of network interfaces. See /instance/network_interfaces/{id} for details. |
| /instance/network_interfaces/{id}	| created_at | The date and time that the network interface was created. |
| /instance/network_interfaces/{id}	| floating_ips | Array of references to floating IPs associated with the network interface. |
| /instance/network_interfaces/{id}	| id | The unique identifier for the network interface. |
| /instance/network_interfaces/{id}	| name |The user-defined name for the network interface. |
| /instance/network_interfaces/{id}	| primary_ipv4_address | The primary IPv4 address. |
| /instance/network_interfaces/{id}	| resource_type	| The resource type. |
| /instance/network_interfaces/{id}	| security_groups |	A list of security groups. |
| /instance/network_interfaces/{id}	| subnet | The subnet associated with the instance. |
| /instance/network_interfaces/{id}	| type | The type of this network interface as it relates to an instance. |
| /instance/volume_attachments	| volume_attachments | A list of volume attachments. See /instance/volume_attachments/{id} for details. |
| /instance/volume_attachments/{id}	| created_at | The date and time that the volume was attached. |
| /instance/volume_attachments/{id}	| delete_volume_on_instance_delete | If set to `true`, when deleting the instance the volume is also deleted. |
| /instance/volume_attachments/{id}	| device | Information about how the volume is exposed to the instance operating system. |
| /instance/volume_attachments/{id}	| id | The unique identifier for the volume attachment. |
| /instance/volume_attachments/{id}	| name | The user-defined name for the volume attachment. |
| /instance/volume_attachments/{id}	| type | The type of volume attachment, boot or data. |
| /instance/volume_attachments/{id}	| volume | The attached volume, which contains the CRN, ID, name, and `deleted` value, when the volume is deleted. |
{: caption="Table 1. Metadata for an instance" caption-side="top"}

## Summary of metadata for SSH keys
{: #imd-sshkeys-summary}

Use the information in the Table 2 to understand the type of metadata returned for SSH keys. 

| SSH Key URI path | Metadata key | Description of the metadata |
|------------------|--------------|-----------------------------|
| /keys	| first	| A link to the first page of resources. |
| /keys	| keys | A collection of keys. See keys/{id} for details. |
| /keys	| limit	| The maximum number of resources that can be returned by the request. |
| /keys	| next	| A link to the next page of resources. |
| /keys	| total_count |	The total number of resources across all pages. |
| /keys/{id} | created_at |	The date and time that the key was created. |
| /keys/{id} | crn | The Cloud Resource Name for this key. |
| /keys/{id} | fingerprint | The fingerprint for this key. The value is returned base64-encoded and prefixed with the hash algorithm (always SHA256). |
| /keys/{id} | id | The unique identifier for the key. |
| /keys/{id} | length | The length of this key, in bits. |
| /keys/{id} | name	| The unique user-defined name for this key. If unspecified, the name will be a hyphenated list of randomly-selected words (for example, "elderly-mountain-troup-opponent".) |
| /keys/{id} | public_key | The public SSH key. |
{: caption="Table 2. Metadata for SSH keys" caption-side="top"}


## Summary of metadata for placement groups
{: #imd-placement-group-summary}

Use the information in the Table 3 to understand the type of metadata returned for placement groups. 

| Placement group URI path | Metadata key | Description of the metadata |
|--------------------------|--------------|-----------------------------|
| /placement_groups | first | A link to the first page of resources. |
| /placement_groups | limit | The maximum number of resources that can be returned by the request. |
| /placement_groups  |	next | A link to the next page of resources, present for all pages but last page. |
| /placement_groups | placement_groups | A collection of placement groups. See placement_groups/{id} for details. |
| /placement_groups  | total_count | The total number of resources across all pages. |
| /placement_groups/{id} | created_at |	The date and time that the placement group was created. |
| /placement_groups/{id} | crn | The Cloud Resource Name for this placement group. |
| /placement_groups/{id} | id |	The unique identifier for this placement group. |
| /placement_groups/{id} | lifecycle_state | The lifecycle state of the placement group. |
| /placement_groups/{id} | name | The user-defined name for the placement group. |
| /placement_groups/{id} | resource_type | The resource type. |
| /placement_groups/{id} | strategy | The strategy for this placement group. |
{: caption="Table 3. Metadata for placement groups" caption-side="top"}

## Next steps
{: #imd-summary-next-steps}

[Retrieve data using the metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
