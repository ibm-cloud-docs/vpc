---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-30"

keywords: vpc, early access, known issues, bugs

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Known issues
{: #known-issues}

Known issues might change during the early access release, so feel free to check back from time to time.
{:shortdesc}


## API-1144: Virtual server instances must be stopped before they can be deleted
{: #API-1144}
- **Symptom:** The VSI cannot be deleted.
- **Workaround:** Stop the instance before attempting to delete it.


## COM-1507: Invalid default repository setting in Ubuntu and Debian VSIs
{: #COM-1507}
- **Symptom:** The `apt update` command fails on a VSI created on a Debian stock image.
- **Workaround:** Update /etc/apt/sources.list, replace 'mirrors.service.networklayer.com' with 'mirrors.adn.networklayer.com' 
- **Fix:** Update the default source.list to use mirrors.adn.networklayer.com. Create APT stanza in cloud-init to overwrite FQDN hard coded in Linux images.

## NET-1114: Security group rules > generation=2 tested in webUI
{: #NET-1114}
- **Symptom:** Deleting or removing a rule takes several hours.
- **Workaround:** Change or delete an InBound or OutBound security group rule. Stop and then start the instance in the VPC. The instance (and only that instance) takes on the properties of the security group rule change.


## RIOS-129: Inconsistent image names between VPC and VPC on Classic
{: #RIOS-129}
- **Symptom:** The names of stock images are different in VPC and VPC on Classic. Expected behavior: The names should be consistent across VPC and VPC on Classic unified images.
- **Fix:** The image service is being rolled out in VPC on Classic and will be in VPC prior to GA. At that point, unified images will have exact name matches between VPC on Classic and VPC as the same image service will be running in both environments.	

## COM-1612: Default CentOS repository setting is invalid
{: #COM-1612}
- **Symptom:** If you create a VSI on the stocked CentOS image, log in to the VSI and run the command “yum update", the command will fail. 
- **Cause:**
The default repository configuration file points to mirrors.softlayer.local, which is invalid. 

<!-- ```
# cat /etc/yum.repos.d/CentOS-Base.repo 
base] 
name=CentOS-$releasever - Base 
baseurl=http://mirrors.softlayer.local/centos/$releasever/os/$basearch/ 
#mirrorlist=http://#mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os 
gpgcheck=1 
gpgkey=http://mirrors.softlayer.local/centos/RPM-GPG-KEY-CentOS-7 
... 
```
{:codeblock} -->

<pre class="codeblock"><code class="hljs"># cat /etc/yum.repos.d/CentOS-Base.repo 
base] 
name=CentOS-$releasever - Base 
baseurl=http://mirrors.softlayer.local/centos/$releasever/os/$basearch/ 
#mirrorlist=http://#mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os 
gpgcheck=1 
gpgkey=http://mirrors.softlayer.local/centos/RPM-GPG-KEY-CentOS-7 
... 
</code></pre>

- **Workaround:** Update /etc/yum.repos.d/CentOS-Base.repo. Replace "mirrors.softlayer.local" with "mirrors.adn.networklayer.com”. 
- **Fix:** CentOS-Base.repo will be corrected. 
