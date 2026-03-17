---

copyright:
  years: 2026
lastupdated: "2026-03-17"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Why can't I use the root user to connect using SSH with a Linux image
{: #troubleshoot-linux-image-connect-ssh}
{: troubleshoot}
{: support}

I can't connect to the virtual server instance using the root user account when I use a Linux image.
{: shortdesc}


I receive the error `root@: Permission denied (publickey,gssapi-keyex,gssapi-with-mic)` when trying to connect to the virtual server instance using the root user account.
{: tsSymptoms}

The root SSH login is disabled for all IBM-provided Linux images. Root SSH access is disabled for security reasons. The system-generated default user account is typically associated with managing access and operations. The default user can be used to log into the virtual server instance using SSH. The SSH keys are configured for the default user in the image. The name of the default user account depends on the operating system image being used.
{: tsCauses}

Use the default user account associated with your Linux operating system.
{: tsResolve}

Use the default user provided for Linux operating systems. You can elevate privileges with sudo if necessary. Go to [Determining the default user account](/docs/vpc?topic=vpc-vsi_is_connecting_linux#determining-default-user-account) and review the table. The table lists each Linux operating system and its corresponding default user account. Use this default user account to log into the virtual server instance.
