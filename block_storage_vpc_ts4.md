---

copyright:
  years: 2019, 2025
lastupdated: "2025-08-06"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I delete the expanded volume when its associated virtual server instance is deleted?
{: #troubleshoot-topic-4}
{: troubleshoot}

When you attempt to delete a virtual server instance with an attached volume that is being resized, the volume remains in an _updating_ state and can't be deleted.
{: tsSymptoms}

When initiated the deletion of your virtual server instance either manually or by auto-delete, the volume was being resized. The running operation can't complete, so the status of the volume is shown as _updating_ and the volume isn't deleted with the instance.
{: tsCauses}

A volume must be in an _available_ state for operations such as attach, detach, delete. When you are expanding a volume, wait for the volume resize to complete before you initiate any other operations. If you try to delete a volume that's resizing, the volume remains in an _updating_ state and is not deleted with the instance. To delete the volume, reattach the volume to a different instance, and make sure that the resizing is complete (volume status becomes _available_), and then delete the volume.
{: tsResolve}
