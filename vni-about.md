---

copyright:
  years: 2023
lastupdated: "2023-06-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# About virtual network interfaces
{: #vni-about}

The beta release is provided for IBM internal users. This information is not published for external customers. 
{: beta}

A virtual network interface is a logical abstraction of a network interface in a subnet. A virtual network interface has an IP address in a subnet and at least one security group. 
{: shortdesc}

A virtual network interface can be attached to a target resource. Currently, virtual network interfaces support only file share mount targets. Support for other resources is in development. 

You can view virtual network interfaces independently of other VPC resources.

## Getting started with virtual network interfaces
{: #vni-getting-started}

Virtual network interfaces must be attached to a file share mount target. When [creating a file share mount](/docs/vpc?topic=vpc-file-storage-create&interface=ui), specify a subnet or a reserved IP to use for creating a virtual network interface for the file share mount.

When the file share mount is deleted, it's virtual network interface will be automatically deleted.

## Using security groups with virtual network interfaces
{: #vni-security-groups}

You can add and remove a virtual network interface from a security group. 

### Adding and removing a virtual network interface to a security group with the API
{: #add-vni-api}
{: api}

To add a virtual network interface to a security group using the API, see [Add a target to a security group](/apidocs/vpc/latest#create-security-group-target-binding).

To remove a virtual network interface from a security group using the API, see [Remove a target to a security group](/apidocs/vpc/latest#delete-security-group-target-binding).
