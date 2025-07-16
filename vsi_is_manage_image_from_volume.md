---

copyright:
  years: 2021, 2025
lastupdated: "2024-06-12"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing images from a volume
{: #image-from-volume-vpc-manage}

Custom images that are created from a volume are independent from the original volume. You can update the image and delete it as needed. You can delete the original volume and the custom image persists. You can schedule the deprecation and making the image obsolete in the console, from the CLI, or the API.
{: shortdesc}

Allowed-use expressions: You can use an allowed-use expression with your custom image to define the capabilities and restrictions of an image and help you find compatible image and profile combinations during server creation. The allowed-use expressions default value for any custom images that are created from a volume is the allowed-use expressions from the original source volume. However, you can edit these allowed-use expressions on the custom image. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).
{: note}

## Scheduling a lifecycle status change for a custom image by using the UI
{: #ifv-schedule-ilm-status-change-ui}
{: ui}

You can schedule either a single image lifecycle status change or schedule the status changes for the entire lifecycle of the image.

Use the following steps to schedule a single status change:

{{_include-segments/image_single_status_lifecycle_schedule.md}}

Use the following steps to schedule the entire lifecycle of the image:
{{_include-segments/image_complete_lifecycle_status.md}}

## Changing a lifecycle status of a custom image by using the CLI
{: #ifv-schedule-ilm-status-change-cli}
{: cli}

You can change the lifecycle status of an {{site.data.keyword.vpc_short}} image that was created from a volume from the command-line interface (CLI). You can make an immediate status change by using the `ibmcloud is image-deprecate` or `ibmcloud is image-obsolete` commands. You can also schedule these status changes for a future date and time by using the `ibmcloud is image-update` command. Specify the name or ID of the custom image with the `IMAGE` variable.


{{_include-segments/ilm-change-image-lifecycle-status-cli.md}}

## Removing previously scheduled lifecycle status change by using the CLI
{: #ifv-reset-ilm-status-change-cli}
{: cli}


{{_include-segments/ilm-reset-image-lifecycle-status-cli.md}}

## Changing the lifecycle status of a custom image by using the API
{: #ifv-schedule-ilm-status-change-api}
{: api}

You can change the lifecycle status of an {{site.data.keyword.vpc_short}} image that was created from volume with the application programming interface (API). You can make an immediate status change or schedule a status change to happen later.


{{_include-segments/ilm-change-image-lifecycle-status-api.md}}

## Removing previously scheduled lifecycle status change by using the API
{: #ifv-reset-ilm-status-change-api}
{: api}


{{_include-segments/ilm-reset-image-lifecycle-status-api.md}}

## Performance considerations
{: #ifv-performance}

Several factors can impact how quickly your image from volume is queued and created, such as the size of the image and number of jobs in the queue. If an image is taking longer than you want to wait, you can cancel the job.

### Performance during image creation
{: #ifv-image-created}

When you create the image, the API `status_reasons` parameter indicates `image_request_queued`, with a message _"The image request is accepted and waiting for system resources to become available"_. When you see this message, it might take a while for the job to start. The following table provides some guidelines that are based on image size. Check for an `in_progress` status to see whether the image is being created. Again, depending on the size of the image, it might take a while before the image is available.

| Image size | Estimated time until image is available |
|------------|-----------------------------------------|
| 5 GB |  5 min |
| 10 GB | 10 min |
| 25 GB |  25 min |
| 50 GB |  50 min |
| 75 GB | 1 hr 15 min |
| 100 GB | 1 hr 40 min |
{: caption="Estimated wait time for image creation" caption-side="bottom"}

The time for the job to start is about 30 seconds. This time does not reflect traffic in the queue, which can increase the estimate. Most jobs start within 5 - 10 minutes. If it takes longer, [cancel the image creation](#ifv-delete-queued).
{: note}

### Cancelling an image that is queued for creation
{: #ifv-delete-queued}

You can cancel the image creation by deleting the image if the underlying job didn't start yet. For images in a _pending_ state, the `GET /images` API call returns an `image_request_queued` reason code. You can delete the pending image with a `DELETE /image{image_id}` request. Alternatively, you can use the [UI](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui#ifv-delete-ui) or [CLI](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=cli#ifv-delete-cli) to delete the image that is in a pending state.

## Deleting a custom image by using the UI
{: #ifv-delete-ui}
{: ui}

You can delete an image that was created from a volume from the list of custom images. You can delete images that are in pending state (while they are being created) and in available state.

1. Go to the list of custom images. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **Navigation Menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure ![VPC icon](../../icons/vpc.svg) > Compute > Images**.
2. On the **Custom images** tab, locate the image that you want to delete. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Delete**.

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

Make a `DELETE /images` request, and specify the ID of the image that was created from a boot volume.

1. Locate the ID of the image from volume:

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/images?version=2024-06-12&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

2. Delete the image by ID:

   ```sh
   curl -X DELETE "$vpc_api_endpoint/v1/images/$image_id?version=2024-06-12&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}


## Next steps
{: #ifv-manage-next-steps}

Preview the end-to-end process for creating an image from volume, then you can use the image when you create a new virtual server instance. For more information, see the [Image from volume tutorial](/docs/vpc?topic=vpc-creating-and-using-an-image-from-volume).
