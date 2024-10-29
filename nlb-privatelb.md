---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-29"

keywords: network load balancer, air gap, private load balancer, floating ip

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Enabling private load balancer enforcement
{: #private-nlb-enforcement}

Private load balancer enforcement prevents public load balancers from being created. Without public load balancers, only noninternet clients, or clients from within your network environment, can access your load balancers. When enabled, a restriction is placed on your account to prevent the creation of floating IP addresses on all network load balancers.

To implement private load balancer enforcement, open an [IBM Support case](/docs/account?topic=account-using-avatar) and reference your need to alter your account to restrict the creation of floating IP addresses. After IBM processes the change, you will no longer be able to create public load balancers.

Private load balancer enforcement applies for all regions when enabled.
{: note}
