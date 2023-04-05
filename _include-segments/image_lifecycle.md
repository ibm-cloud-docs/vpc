

{{site.data.keyword.attribute-definition-list}}

You can use the UI, CLI, API, and Terraform to manage the lifecycle of your custom images with 3 statuses. You can move the image back and forth through all the statuses and set up dates to automatically change the image status. All status changes are tracked as an Activity Tracker event. You can filter your list of images based on status to aid in clean-up or tracking of your images. For more information on making the status changes, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui).

| Image status | Description |
| -------------- | -------------- | 
| `available` | The image can be used to create an instance. |
| `deprecated` | The image can be used to create an instance, but you receive a warning. Using the `deprecated` status can discourage use of the image before changing the status to `obsolete`.| 
| `obsolete` | The image can't be used to create an instance. If you try to use an obsolete image to create an instance, you will receive a message stating that the image can't be used to create an instance. This status allows a reversible disabling of an image before you delete the image. | 
{: caption="Table 1. Image lifecycle status" caption-side="bottom"}

Any image that is in `deprecated` or `obsolete` status is still billed. If you don't want to be billed for the image, you must delete it.
{: note}
