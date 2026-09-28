---

copyright:
  years: 2025, 2026
lastupdated: "2026-09-28"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting connection tracking limits
{: #troubleshoot-vni-3}
{: troubleshoot}
{: support}

When a VNIC's connection tracking table is full, the application on the virtual server fails to open some of the new connections, and they time out. Existing connections will keep working, and after it expires, a new connection can be opened.
{: shortdesc}

You cannot establish new connections through the virtual network interface. Existing connections continue to work, but new connection attempts fail or time out.
{: tsSymptoms}


The connection tracking table for the VNIC has reached its maximum concurrent connection limit. This limit is automatically set based on the effective bandwidth of the VNIC. When the table is full, new connection attempts are dropped until existing entries are terminated or time out.
{: tsCauses}

To check whether a VNIC is reaching its limit:
{: tsResolve}

Check your application and system logs for errors such as Connection timed out, ETIMEDOUT or connect() failed, and note when they occur.

Count the active connections on the VNIC, using the VNIC's IP address:

Linux: `ss -tuan src <VNIC_IP> | tail -n +2 | wc -l`

Windows: `netstat -an | findstr <VNIC_IP> | find /c /v ""`

Many TCP connections in SYN-SENT, or UDP requests with no reply (such as DNS lookups timing out), are signs that new connections are not completing.

Compare the count with the limit for the VNIC's effective bandwidth in Table 1. If it's close, reduce the number of concurrent connections, for example with connection pooling or DNS caching. If you can't change your application, consider changing your VSI profile according to the table above.
