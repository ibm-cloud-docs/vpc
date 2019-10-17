---

copyright:
  years: 2019

lastupdated: "2019-09-30"

keywords: activity tracker, vpc, events, logdna 

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:download: .download}


# Activity Tracker with LogDNA events
{: #at-events}

As a security officer, auditor, or manager, you can use the {{site.data.keyword.at_full}} service to track how users and applications interact with {{site.data.keyword.cloud}} Virtual Private Cloud (VPC).
{:shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).


## List of events: Network resources
{: #events-network}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| vpc  | is.vpc.vpc.create   | VPC was created.  |
| vpc  | is.vpc.vpc.update   | VPC was updated.  |
| vpc  | is.vpc.vpc.delete   | VPC was deleted.  |
| vpc  | is.vpc.address-prefix.create  | Address Prefix was added to VPC.  |
| vpc  | is.vpc.address-prefix.update  | VPC Address Prefix was updated.   |
| vpc  | is.vpc.address-prefix.delete  | Address Prefix was removed from VPC.  |
| vpc  | is.vpc.vpc-route.create   | Route was added to VPC.   |
| vpc  | is.vpc.vpc-route.update   | VPC Route was updated.  |
| vpc  | is.vpc.vpc-route.delete   | Route was removed from VPC.   |
| floating-ip  | is.floating-ip.floating-ip.delete   | Floating IP was created.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | Floating IP was updated.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | Floating IP was deleted.  |
| public-gateway | is.public-gateway.public-gateway.create   | Public Gateway was created.   |
| public-gateway | is.public-gateway.public-gateway.update   | Public Gateway was updated.   |
| public-gateway | is.public-gateway.public-gateway.delete   | Public Gateway was deleted.   |
| security-group | is.security-group.security-group.create   | Security Group was created.   |
| security-group | is.security-group.security-group.delete   | Security Group was deleted.   |
| security-group | is.security-group.security-group.update   | Security Group was updated.   |
| security-group | is.security-group.security-group-rule.create  | Rule was added to Security Group.  |
| security-group | is.security-group.security-group-rule.delete  | Rule was removed from Security Group.  |
| security-group | is.security-group.security-group-rule.update  | Security Group Rule was updated.  |
| security-group | is.security-group.security-group-interface.attach | Interface was attached to Security Group.   |
| security-group | is.security-group.security-group-interface.detach | Interface was removed from Security Group.   |
| subnet   | is.subnet.subnet.create   | Subnet was created.   |
| subnet   | is.subnet.subnet.update   | Subnet was updated.   |
| subnet   | is.subnet.subnet.delete   | Subnet was deleted.   |
| subnet   | is.subnet.public-gateway.operate  | Public Gateway was attached to Subnet.  |
| subnet   | is.subnet.public-gateway.operate  | Public Gateway was detached from Subnet.  |
{: caption="Table 1. Actions that generate events for network resources" caption-side="top"}

## List of events: Compute resources
{: #events-compute}


| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| instance   | is.instance.instance.create   | Instance was created.   |
| instance   | is.instance.instance.delete   | Instance was deleted.   |
| instance   | is.instance.instance.update   | Instance was updated.   |
| instance   | is.instance.action.create   | Instance action was created.  |
| instance   | is.instance.action.delete   | Pending instance action was deleted.  |
| instance   | is.instance.network_interface.floating_ip.create  | Floating ip was associated to instance network interface  |
| instance   | is.instance.network_interface.floating_ip.delete  | Floating ip was Disassociated from instance network interface |
| instance   | is.instance.volume_attachement.create   | Instance volume attachment was created  |
| instance   | is.instance.volume_attachement.delete   | Instance volume attachment was deleted  |
| instance   | is.instance.volume_attachement.update   | Instance volume attachment was updated  |
| key  | is.key.key.create   | key was created.  |
| key  | is.key.key.delete   | key was deleted.  |
| key  | is.key.key.update   | key was updated.  |
{: caption="Table 2. Actions that generate events for compute resources" caption-side="top"}

## List of events: Image resources
{: #events-images}

The following table lists the actions related to image resources and the generation of events.

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| image  | is.image.image.create   | Image was created |
| image  | is.image.image.delete   | Image was deleted |
| image  | is.image.image.update   | Image was updated  |
{: caption="Table 3. Actions that generate events for image resources" caption-side="top"}

## List of events: Storage resources
{: #events-storage}

| Resource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| volume  | is.volume.volume.create  |  Volume was created.  |
| volume  | is.volume.volume.update  | Volume was updated.  |
| volume  | is.volume.volume.delete  | Volume was deleted.  |
{: caption="Table 4. Actions that generate events for storage resources" caption-side="top"}

## Supported locations
{: #at-supported-locations}

{{site.data.keyword.at_full}} support is currently available for the Dallas location. 

## Viewing events
{: #at_ui}

Events that are generated by VPC resources are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Launching the web UI through the IBM Cloud UI](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).