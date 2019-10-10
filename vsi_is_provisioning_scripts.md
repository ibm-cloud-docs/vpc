---

copyright:
  years: 2019
lastupdated: "2019-09-30"

keywords: user data, vsi, virtual server instance

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# User data
{: #user-data}
[comment]: # (linked help topic)

When you create an {{site.data.keyword.vsi_is_full}} instance, you can specify optional user data that automatically performs common configuration tasks or runs scripts.
{:shortdesc}

You can specify cloud-config data directly in the user data field, or you can include the cloud-config data in a text file and specify the file name when you create your instance. For example, if you save the cloud-config data in `userdata.blob`, specify `-user-data @userdata.blob` when you create an instance by using the CLI.

## User data examples for Linux 
{: #user-data-examples-for-linux}

The following cloud-init example shows how a Linux user can add a user and provide the user with an authorized SSH key. The **Name** field will have the public key added to `~/.ssh/authorized_keys`. 


<pre class="codeblock"><code class="hljs">#cloud-config
users:
  - name: demouser
    gecos: Demo User
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: users, admin
    ssh_import_id: None
    lock_passwd: true
    ssh_authorized_keys:
        - &lt;ssh public key&gt;
</code></pre>

The following shell script example shows how a Linux user can add an SSH key for the current user.

<pre class="codeblock"><code class="hljs">#!/bin/sh
echo &lt;sshKey&gt; &gt; ~/.ssh/authorized_keys
</code></pre>

You can paste one of these examples directly into the **User Data** field. The user data is then available to the virtual server instance during provisioning.

For more Linux user data examples and information, see [Cloud config examples ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloudinit.readthedocs.io/en/latest/topics/examples.html){:new_window}.
