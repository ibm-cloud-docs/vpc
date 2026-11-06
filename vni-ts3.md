---

copyright:
  years: 2025, 2026
lastupdated: "2026-11-04"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting connection tracking limits
{: #troubleshoot-vni-3}
{: troubleshoot}
{: support}

When a virtual network interface (VNIC) connection tracking table is full, the application on the virtual server fails to open new connections, and they time out. Existing connections continue to work, and after the table entry expires, a new connection can be opened.
{: shortdesc}

You cannot establish new connections through the virtual network interface. Existing connections continue to work, but new connection attempts fail or time out.
{: tsSymptoms}

The connection tracking table for the VNIC reaches its maximum concurrent connection limit. This limit is automatically set based on the effective bandwidth of the VNIC. When the table is full, new connection attempts are dropped until existing entries are closed or time out.
{: tsCauses}

To check whether a VNIC is reaching its limit and resolves the issue:
{: tsResolve}

1. **Check application and system logs** for errors such as `Connection timed out`, `ETIMEDOUT`, or `connect() failed`, and note when they occur.

1. **Count the active connections on the VNIC** that uses the VNIC's IP address:

   Linux:
   ```bash
   ss -tuan src <VNIC_IP> | tail -n +2 | wc -l
   ```
   {: pre}

   Windows:
   ```cmd
   netstat -an | findstr <VNIC_IP> | find /c /v ""
   ```
   {: pre}

1. **Analyze the connection state**. Many TCP connections in `SYN-SENT` state, or UDP requests with no reply (such as DNS lookups timing out), indicate that new connections are not completing.

1. **Compare the count with the limit** for the VNIC's effective bandwidth (see [Connection table size defaults](/docs/vpc?topic=vpc-vni-connection-table-bandwidth#connection-table-defaults)). If the count is close to the limit, take one or more of the following actions:
   * Reduce the number of concurrent connections by implementing connection pooling.
   * Implement DNS caching to reduce DNS lookup connections.
   * Optimize your application to reuse connections instead of creating new ones.
   * If you cannot change your application, consider changing to a VSI profile with higher bandwidth (which provides a larger connection tracking table limit).
