{{site.data.keyword.attribute-definition-list}}

You can use the UI, CLI, API, and Terraform to manage the lifecycle of your custom images with three statuses. You can move the image back and forth through all the statuses and set dates to automatically change an image status. All status changes are tracked as an {{site.data.keyword.atracker_full_notm}} event. You can filter your list of images based on status to aid in clean-up or tracking of your images. For more information about making status changes, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui).

| Image status | Description |
| -------------- | -------------- |
| `available` | The most current version of the operating system image is `available`. When a new version of an operating system is made `available`, the older version image of that guest operating system changes to `deprecated`. No stock operating system that reaches EOS has an `available` image. |
| `deprecated` | Older versions of stock operating systems are `deprecated`. Also, any stock operating system that reached EOS is also `deprecated`. You can still provision an instance with these images. The UI doesn't display any `deprecated` images when you create an instance. These images are still visible in the CLI and API.|
| `obsolete` | Images that are EOS as defined by the vendor are `obsolete`. You can't provision instances with these images. If you try to use an obsolete image to create an instance, you receive a message that states that you can't use the image to create an instance. This status allows a reversible disabling of an image before you delete the image. |
{: caption="Image lifecycle status" caption-side="bottom"}

Any image that is in `deprecated` or `obsolete` status is still billed. If you don't want to be billed for the image, you must delete it.
{: note}
