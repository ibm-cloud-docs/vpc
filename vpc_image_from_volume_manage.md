---

copyright:
  years: 2021, 2024
lastupdated: "2024-01-30"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing image from a volume
{: #image-from-volume-vpc-manage}

Images from volumes have their own lifecycles that you can manage.
{: shortdesc}

<!-- these bullets need an intro-->

* IAM permissions for creating an image from volume.
* Make an immediate or schedule a lifecycle status change.
* Cancel an image queued for creation.
* Delete an image from volume.

## IAM permissions for creating an image from volume
{: #ifv-iam}

IAM enables you to securely authenticate users for platform services and control access to resources consistently across {{site.data.keyword.cloud}}. The following table shows the access requirements for creating an image from a volume.

| Operation | Action | API call | IAM access roles |
|-----------|--------|----------|------------------|
| Create an instance | is.instance.instance.create | `POST/instances` | Administrator, Editor |
| List all instances | is.instance.instance.list | `GET/instances` | Administrator, Editor, Operator, Viewer |
| Create an image from an instance boot volume | is.image.image.create | `POST/images` | Administrator, Editor |
| List all images | is.image.image.list | `GET/images` | Administrator, Editor, Operator, Viewer |
| Delete image | is.image.image.delete | `DELETE/images` | Administrator, Editor |
| Schedule deprecation | is.image.image.deprecate | `POST/images` with `deprecation_at` specified | Administrator, Editor |
| Schedule obsolescence | is.image.image.obsolete | `POST/images` with `obsolescence_at` specified | Administrator, Editor |
{: caption="Table 2. IAM user permissions" caption-side="bottom"}

## Image from volume lifecycle states
{: #ifv-states}

The following table describes the lifecycle states for custom images that are created from a volume.

| Status | Explanation |
|-----------------|-------------|
| Available | Image is available when you provision an instance. |
| Pending | The image is being created. If successful, the image is available. Otherwise, creation failed. |
| Failed | Image creation failed. |
| Deleting | The image is being deleted. |
| Deprecated | You can use the image to create an instance. Using the `deprecated` status can discourage use of the image before the status changes to `obsolete`. |
| Obsolete | You can't use the image to a create an instance. If you try to use an obsolete image to create an instance, you receive a message that the image can't be used to create an instance. This status allows a reversible disabling of an image before you delete the image. |
{: caption="Table 3. Image lifecycle states" caption-side="bottom"}

## Scheduling an image from volume lifecycle status by using the UI
{: #ifv-schedule-ilm-status-change-ui}
{: ui}

You can schedule either a single image lifecycle status change or schedule the status changes for the entire lifecycle of the image.

Use the following steps to schedule a single status change:
<!-- see the -include-segments folder to update this information which is shared with vsi_is_manage_images.md -->
{{_include-segments/image_single_status_lifecycle_schedule.md}}

Use the following steps to schedule the entire lifecycle of the image:
{{_include-segments/image_complete_lifecycle_status.md}}

## Changing an image from volume lifecycle status by using the CLI
{: #ifv-schedule-ilm-status-change-cli}
{: cli}

You can change the lifecycle status of an {{site.data.keyword.vpc_short}} image from volume by using the command-line interface (CLI). You can make an immediate status change by using the **ibmcloud is image-deprecate** or **ibmcloud is image-obsolete** commands. You can also schedule these status changes for a future date and time by using the **ibmcloud is image-update** command. Specify the name or ID of the custom image with the `IMAGE` variable.

<!-- see the -include-segments folder to update this information which is shared with vsi_is_manage_images.md -->
{{_include-segments/ilm-change-image-lifecycle-status-cli.md}}

## Removing previously scheduled image from volume lifecycle statuses by using the CLI
{: #ifv-reset-ilm-status-change-cli}
{: cli}

<!-- see the -include-segments folder to update this information which is shared with vsi_is_manage_images.md -->
{{_include-segments/ilm-reset-image-lifecycle-status-cli.md}}

## Changing an image from volume lifecycle status by using the API
{: #ifv-schedule-ilm-status-change-api}
{: api}

You can change the lifecycle status of a {{site.data.keyword.vpc_short}} image from volume by using the application programming interface (API). You can make an immediate status change or schedule a status change to happen later.

<!-- see the -include-segments folder to update this information which is shared with vsi_is_manage_images.md -->
{{_include-segments/ilm-change-image-lifecycle-status-api.md}}

## Removing previously scheduled image from volume lifecycle statuses by using the API
{: #ifv-reset-ilm-status-change-api}
{: api}

<!-- see the -include-segments folder to update this information which is shared with vsi_is_manage_images.md -->
{{_include-segments/ilm-reset-image-lifecycle-status-api.md}}

## Performance considerations
{: #ifv-performance}

Several factors can impact how quickly your image from volume is queued and created, such as the size of the image and number of jobs in the queue. If an image is taking longer than you want to wait, you can cancel the job.

### Performance during image creation
{: #ifv-image-created}

When you create the image, the API `status_reasons` parameter indicates `image_request_queued`, with a message _"The image request is accepted and waiting for system resources to become available"_. When you see this message, it might take a while for the job to start. The following table provides some guidelines that are based on image size. Check for an `in_progress` status to see whether the image is being created. Again, depending on the size of the image, it might take a while before the image is available.

| Image size | Estimated time until image is available |
|------------|--------------------------------|
| 5 GB |  5 min |
| 10 GB | 10 min |
| 25 GB |  25 min |
| 50 GB |  50 min |
| 75 GB | 1 hr 15 min |
| 100 GB | 1 hr 40 min |
{: caption="Table 1. Estimated wait time for image creation" caption-side="bottom"}

The time for the job to start is about 30 seconds. This time does not reflect traffic in the queue, which can increase the estimate. Most jobs start within 5 - 10 minutes. If it takes longer, [cancel the image creation](#ifv-delete-queued).
{: note}

### Cancelling an image that is queued for creation
{: #ifv-delete-queued}

You can cancel the image creation by deleting the image if the underlying job didn't start yet. For images in a _pending_ state, the `GET/images` API call returns an `image_request_queued` reason code. You can delete the pending image with `DELETE/image{image_id}`. Alternatively, you can use the [UI](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui#ifv-delete-ui) or [CLI](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=cli#ifv-delete-cli) to delete the image that is in a pending state.

## Deleting an image from volume by using the UI
{: #ifv-delete-ui}
{: ui}

Delete an image from a volume from the list of custom images. You can delete images in that are [while they are being created](#ifv-delete-queued) (_pending_ state) and available images.

1. Go to the list of custom images. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
2. On the **Custom images** tab, locate the image that you want to delete. From the overflow menu (ellipsis), select **Delete**.

## Deleting an image from volume by using the CLI
{: #ifv-delete-cli}
{: cli}

1. Locate the image from volume in the list of images.

   ```sh
   ibmcloud is images
   ```
   {: pre}

2. Delete the image by ID.

   ```sh
   ibmcloud is image-delete IMAGE_ID
   ```
   {: pre}

## Deleting an image from volume by using the API
{: #ifv-delete-api}
{: api}

Specify a `DELETE/images` call and specify the ID of the image that was created from the boot volume.

1. Locate the ID of the image from volume:

    ```sh
    GET/images
    ```
    {: pre}

2. Delete the image by ID:

    ```sh
    DELETE/image/{image_id}
    ```
    {: pre}


## Next steps
{: #ifv-manage-next-steps}

Preview the end-to-end process for creating an image from volume, then you can use the image when you create a new virtual server instance. For more information, see the [Image from volume tutorial](/docs/vpc?topic=vpc-creating-and-using-an-image-from-volume).
