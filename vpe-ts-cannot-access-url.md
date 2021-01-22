---

copyright:
  years: 2018, 2020
lastupdated: "2020-10-30"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why can't I can access a service using the service's URL?
{: #troubleshoot-cannot-access-url}
{: troubleshoot}
{: support}


You should be able to access a service using the reserved IP or service's URL. If you can't access a service using the URL, you might need to force the DHCP client to renew its lease.
{: shortdesc}

Cannot use the service's URL to access the service.
{: tsSymptoms}

The DNS service was not able to change the default DNS server for the virtual server instance trying to access the service.
{: tsCauses}

To correct this issue, follow steps similar to the following Ubuntu commands:
{: tsResolve}

1. In the virtual server instance, check the last lease in `/var/lib/dhcp/dhclient.leases`. The entry for DNS servers should read option `domain-name-servers 161.26.0.7,161.26.0.8`, not `161.26.0.10,161.26.0.11`.
1. If this entry is not updated, run `/sbin/dhclient` to force the DHCP client update.
