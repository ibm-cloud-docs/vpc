---

copyright:
  years: 2021
lastupdated: "2021-05-27"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:note: .note}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing image from a volume
{: #image-from-volume-vpc-manage}

Images from volumes have their own lifecycle that you can manage. 
{: shortdesc}

* Delete an image from volume.
* Cancel an image queued for creation.
* IAM permissions for creating an image from volume.

## Deleting an image from volume by using the UI
{: #ifv-delete-ui}
{: ui}

Delete an image from a volume from the list of custom images. You can delete images in the [process of being created](#ifv-delete-queued) (_pending_ state) and available images.

1. Go to the list of custom images. In the [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.

2. Locate the image that you want to delete. From the overflow menu (ellipsis), select **Delete**.


## Deleting an image from volume by using the CLI
{: #ifv-delete-cli}
{: cli}

1. Locate the image from volume in the list of images.

```
$ ibmcloud is images
```
{: pre}

2. Delete the image by ID.

```
$ ibmcloud is image-delete IMAGE_ID
```
{: pre}

## Deleting an image from volume by using the API
{: #ifv-delete-api}
{: api}

Specify a `DELETE/images` call and specify the ID of the image that was created from the boot volume.

1. Locate the ID of the image from volume:

    ```
    GET/images 
    ```
    {: pre}

2. Delete the image by ID:

    ```
    DELETE/image/{image_id} 
    ```
    {: pre}

## Performance considerations
{: #ifv-performance}

Several factors that can impact how quickly your image from volume is queued and created, such as size of the image and number of jobs in the queue. If an image is taking longer than you want to wait, you can cancel the job.

### Performance during image creation
{: #ifv-image-created}

When you create the image, the API `status_reasons` parameter indicates `image_request_queued`, with a message "The image request is accepted and waiting for system resources to become available". When you see this message, it might take a while for the job to start. The following table provides some guidelines that are based on image size. Check back and look for an `in_progress` status to see whether the image is being created. Again, depending on the size of the image, it might take a while before the image is available.

| Image size | Time until image is available |
|------------|--------------------------------|
| 5 GB |  5 min |
| 10 GB | 10 min |
| 25 GB |  25 min |
| 50 GB |  50 min |
| 75 GB | 1 hr 15 min |
| 100 GB | 1 hr 40 min |
{: caption="Table 1. Estimated wait time for image creation" caption-side="top"}

The time for the job to start is about 30 seconds. This time does not reflect traffic in the queue, which can increase the estimate. Most jobs start within 5 - 10 minutes. If it's taking longer, [cancel the image from being created](#ifv-delete-queued).
{: note}


### Cancelling an image that is queued for creation
{: #ifv-delete-queued}

You can cancel the image creation by deleting the image if the underlying job didn't start yet. For images in a _pending_ state, the `GET/images` API call returns an `image_request_queued` reason code. You can delete the pending image with `DELETE/image{image_id}`. Alternatively, use the [UI](#ifv-delete-ui) or [CLI](#ifv-delete-cli) to delete the image in a pending state.

## IAM permissions for creating an image from volume
{: #ifv-iam}

IAM enables you to securely authenticate users for platform services and control access to resources consistently across IBM Cloud. The following table shows access requirements for creating an image from a volume.

| Operation | Action | API call | IAM access roles |
|-----------|--------|----------|------------------|
| Create an instance | is.instance.instance.create | `POST/instances` | Administrator, Editor |
| List all instances | is.instance.instance.list | `GET/instances` | Administrator, Editor, Operator, Viewer |
| Create an image from an instance boot volume | is.image.image.create | `POST/images` | Administrator, Editor |
| List all images | is.image.image.list | `GET/images` | Administrator, Editor, Operator, Viewer |
| Delete image | is.image.image.delete | `DELETE/images` | Administrator, Editor |
{: caption="Table 2. IAM user permissions" caption-side="top"}

## Image from volume lifecycle states
{: #ifv-states}

The following table describes the lifecycle states for custom images that are created from a volume.

| Status | Explanation |
|-----------------|-------------|
| Available | Image is available when you provision an instance. |
| Pending | The image is being created; if successful, it is available. Otherwise, failed. |
| Failed | Image creation failed. |
| Deleting | The image is being deleted. |
{: caption="Table 3. Image lifecycle states" caption-side="top"}

## Next Steps
{: #ifv-manage-next-steps}

Preview the end-to-end process for creating an image from volume, then by using the image when you create a new virtual server instance. See the [Image from volume tutorial](/docs/vpc?topic=vpc-creating-and-using-an-image-from-volume).

