{{site.data.keyword.attribute-definition-list}}

You can use the UI, CLI, API, and Terraform to manage the lifecycle of your custom images with three statuses. You can move the image back and forth through all the statuses and set dates to automatically change an image status. All status changes are tracked as an {{site.data.keyword.atracker_full_notm}} event. You can filter your list of images based on status to aid in clean-up or tracking of your images. For more information about making status changes, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui).

| Image status | Description |
| -------------- | -------------- |
| `available` | The most current version of the operating system image is `available`. When a new version of an operating system is made `available`, the older version image of that guest operating system changes to `deprecated`. No stock operating system that reaches EOS has an `available` image. |
| `deprecated` | You can use the `deprected` status to indicate the image can be used, but it is not recommended. For example, with a stock image, when a new version of an operating system is made `available`, the older version image of that guest operating system changes to `deprecated`. You can still create virtual server instances or bare metal servers with these images.|
| `obsolete` | You can use the `obsolete` status to indicate images must not be used, such as an operating system image that has reached EOS. You can't create virtual server instances or bare metal servers with these images. If you try to use an obsolete image to create an instance, you receive a message that states that you can't use the image to create an instance. This status allows a reversible disabling of an image before you delete the image. |
{: caption="Image lifecycle status" caption-side="bottom"}

Any image that is in `deprecated` or `obsolete` status is still billed. If you don't want to be billed for the image, you must delete it.
{: note}
