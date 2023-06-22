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

A virtual network interface is a logical abstraction of a network interface in a subnet. A virtual network interface has an IP address in a subnet and at least one security group. 
{: shortdesc}

A virtual network interface can be attached to a target resource. Currently, virtual network interfaces support only file share mount targets. Support for other resources is in development. 

You can view virtual network interfaces independently of other VPC resources.

## Getting started with virtual network interfaces
{: #vni-getting-started}

Virtual network interfaces must be attached to a file share mount target. You can expect this basic workflow:

1. Create a [file share mount](/docs/vpc?topic=vpc-file-storage-create&interface=ui).
1. Create a virtual network interface.
1. Attach the virtual network interface to the file share mount target.

## Using security groups with virtual network interfaces
{: #vni-security-groups}

You can add and remove a virtual network interface from a security group. 

### Adding and removing a virtual network interface to a security group with the API
{: #add-vni-api}
{: api}

To add a virtual network interface to a security group using the API, see [Add a target to a security group](/apidocs/vpc/latest#create-security-group-target-binding).

To remove a virtual network interface from a security group using the API, see [Remove a target to a security group](/apidocs/vpc/latest#delete-security-group-target-binding).
