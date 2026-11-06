---

copyright:
  years: 2026, 2026
lastupdated: "2026-11-06"

keywords: file share, file storage, mount, mount command fails, NFS version, NFS 4.1, AIX, unsupported operating system

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I mount a file share on my server?
{: #troubleshooting-file-storage-nfs-version}
{: troubleshoot}
{: support}

Resolve file share mount failures that occur because the operating system of the client doesn't support the required NFS version.
{: shortdesc}

When you run the `mount` command on a server, the file share does not mount. The error message might not mention the NFS version. For example, on an AIX server, the mount command returns a message about the host database or a route to the remote host, even though the network path is available:
{: tsSymptoms}

```sh
mount -v nfs -o vers=4 <mount-target-IP>:/<mount-path> /<local-directory>
mount: 1831-009 <mount-target-IP> not in hosts database
mount: 1831-008 giving up on:
<mount-target-IP>:/<mount-path>
A route to the remote host is not available.
```
{: screen}

The same file share can be mounted successfully from a Linux server in the same VPC.

{{site.data.keyword.filestorage_vpc_short}} requires NFS version 4.1 or higher. Some operating systems, such as AIX, support only NFS version 4.0 or earlier. For example, see [NFS version 4 support](https://www.ibm.com/docs/en/aix/7.3.0?topic=system-nfs-version-4-support){: external} in the AIX documentation. These operating systems can't mount {{site.data.keyword.filestorage_vpc_short}} file shares, and the error messages that they return don't always identify the NFS version as the cause.
{: tsCauses}

Verify that the operating system of your server supports NFS version 4.1 or higher.
{: tsResolve}

1. Check the documentation of your operating system to find the NFS versions that it supports. If the highest supported NFS version is earlier than 4.1, the operating system can't mount the file share.
2. If you use AIX, the error messages that are described in [mount error messages](https://www.ibm.com/docs/en/aix/7.3.0?topic=troubleshooting-mount-error-messages){: external} in the AIX documentation, such as `not in hosts database`, point to hostname resolution or network problems. If the hostname and routing checks pass, also check whether your AIX level supports NFS version 4.1 or higher.
3. If your operating system does not support NFS version 4.1 or higher, mount the file share from a server that runs a [supported operating system](/docs/vpc?topic=vpc-file-storage-vpc-faqs#faq-fs-os).
4. If your operating system does support NFS version 4.1 or higher, verify that the mount command specifies a supported version, such as `vers=4.1`. For more information, see the mounting instructions for [Red Hat](/docs/vpc?topic=vpc-file-storage-mount-RHEL), [CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos), or [Ubuntu](/docs/vpc?topic=vpc-file-storage-mount-ubuntu).

For more information about the NFS version requirement, see [NFS version](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-nfs-version). If you still can't mount the file share, or your operating system is not listed in the documentation, contact the support team for your operating system or [IBM support](/docs/support?topic=support-open-case).
