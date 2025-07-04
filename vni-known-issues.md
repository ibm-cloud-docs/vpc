---

copyright:
  years: 2023, 2025
lastupdated: "2025-06-27"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for virtual network interfaces
{: #vni-known-issues}

The following known limitations apply to this virtual network interface release:
{: shortdesc}

* With infrastructure NAT enabled for a virtual network interface, no more than one floating IP can target the virtual network interface.
* When a floating IP is attached to a virtual network interface, network address translation (NAT) is performed between the floating IP’s public address and the virtual network interface’s primary IP address.
* The `protocol_state_filtering_mode` property cannot be `disabled` when the target is a file share mount target.
* Creating an instance group from an instance template that has a virtual network interface with `allow_ip_spoofing` set to `true` is not yet supported.
* Not all virtual network interface targets support all virtual network interface policies. The following list shows the current incompatibilities.
    * Virtual server instances do not support disabling infrastructure NAT.
    * The `allow_ip_spoofing` property cannot be `true` when the target is a file share mount target.
    * The `enable_infrastructure_nat` property cannot be `false` when the target is an instance network attachment or a file share mount target.
    * Secondary IP addresses are not allowed when the target is a file share mount target.
    * A floating IP cannot target a virtual network interface that is attached to a file share mount target.
    * A flow log collector cannot target a virtual network interface that is attached to a bare metal server network attachment or a file share mount target.
    * Select customers have access to virtual server instances with vNICs that are SR-IOV enabled. Flow log collectors cannot target instance network attachments (nor child network interfaces) that are SR-IOV enabled.
