---

copyright:
  years:  2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring Virtual Network Interfaces
{: #vnimonitoring}

You can use IBM Cloud Activity Tracker to monitor virtual network interfaces from your Log Analysis instance.


## Monitoring Virtual Network Interfaces by using IBM Cloud Activity Tracker
{: #vnimonitoring_at}

Operations on virtual network interfaces generate Activity Tracker events named `is.virtual-network-interface.virtual-network-interface.*`. Complete the following steps to review virtual network interfaces by using Activity Tracker:

1. In the IBM Cloud console, go to the **Menu icon** ![Navigation Menu](/images/menu_icon.png) > **Observability** > **Activity Tracker**.
2. Click **Open dashboard** on the dashboard that you use to monitor trusted profiles.
3. Use the search field to view virtual network interfaces events.

One API operation may generate more than one Activity Tracker event. For example, when you create a virtual network interface with an exsting reserved IP, it will generate:
- `is.virtual-network-interface.virtual-network-interface.create` for the virtual network interface creation
- `is.virtual-network-interface.virtual-network-interface.attach` for the virtual network interface being attached to each reserved IP
- `is.subnet.reserved-ip.attach` for each reserved IP being attached to the virtual network interface
- `is.virtual-network-interface.virtual-network-interface.attach` for the virtual network interface being attached to each security group
- `is.security-group.security-group.attach` for each security group being attached to the virtual network interface

You can use the request id to search for all Activity Tracker events for one API operation. The request id can be found by:
- The `X-Request-Id` in the API request or response header.
- The `requestId` in each Activity Tracker event's `requestData`.
