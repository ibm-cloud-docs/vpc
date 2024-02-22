---

copyright:
  years: 2019, 2024
lastupdated: "2024-02-22"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing custom images
{: #managing-custom-images}

After you import a custom image to {{site.data.keyword.vpc_short}}, you can view details about the image, manage the lifecycle of the image, use the image to create a virtual server instance, export the image, and even share it with a private catalog.
{: shortdesc}

To manage an image from volume, see [Managing image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui). To use a custom image in a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).
{: note}

{{site.data.keyword.cloud}} Identity and Access Management (IAM) enables you to securely authenticate users for platform services and control access to resources consistently across {{site.data.keyword.cloud_notm}}. For information about access requirements for custom images, see the IAM roles and actions information in [Image Service for VPC](/docs/account?topic=account-iam-service-roles-actions#image-service-for-vpc).

For details about the `$vpc_api_endpoint` and `$iam_token` variables in the following examples, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](/apidocs/vpc/latest#about-vpc-api).
{: api}

## Managing custom images by using the UI
{: #custom-images-managing-ui}
{: ui}

You can manage an image by using the {{site.data.keyword.cloud_notm}} console.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
2. On the **Custom images** tab, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for a specific image and select from the available options. Encrypted custom images are identified by a lock icon after the image name. You can select from the following actions:

| Action | Description |
|--------|-------------|
| Rename | Rename the custom image. |
| Create virtual server instance | Create a new virtual server from the custom image. |
| Export | Copy the custom image to {{site.data.keyword.cos_full_notm}}. |
| Share to catalog | Share the {{site.data.keyword.vpc_short}} custom image to an existing private catalog. |
| Copy | Copy the UUID of the custom image. |
| View checksum | View the checksum for the custom image. |
| Schedule lifecycle | Opens the **Schedule image lifecycle** panel. You can make an immediate status change or schedule a statue change for a future date and time. You can schedule a single status change or schedule the complete lifecycle of the images. The image statuses are:  \n  \n * `available`: The image can be used to create an instance.  \n  \n * `deprecated`: The image is still available to use to provision and instance. Using the `deprecated` status can discourage use of the image before the status changes to `obsolete`.  \n * `obsolete`: The image is not available to use to provision an instancce.  \n  \n * Schedule complete lifecycle: You can schedule both the `deprecated` and `obsolete` status changes at the same time.  \n  \n You can move back and forth between the three statuses. Only the statuses you can change to are displayed. You can schedule status changes by by using calendar date and time or number of days. The obsolescence dates must always be after the deprecation date.|
| Delete | Delete the custom image.  \n  \n  If you want to delete a custom image in a private catalog, see [Deleting a custom image in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui#deleting-private-catalog-custom-image-vpc). |
{: caption="Table 1. Custom image actions" caption-side="bottom"}

## Listing custom images by using the CLI
{: #custom-images-list-cli}
{: cli}

You can list all of the {{site.data.keyword.vpc_short}} custom images in your region by using the command-line interface (CLI).

To list all custom images by using the CLI, use the **`ibmcloud is images`** command. Custom images are private to the account where they are created, so the `--visibility` option is `private`.

```sh
ibmcloud is images --visibility private
```
{: pre}

For more information, see [ibmcloud is images](/docs/vpc?topic=vpc-vpc-reference&interface=ui#images-list) in the VPC CLI reference page.

## Listing all images by using the API
{: #custom-images-list-api}
{: api}

You can list all of the {{site.data.keyword.vpc_short}} images in your region by using the application programming interface (API).

To list all images by using the API, use [List all images](/apidocs/vpc/latest#list-images). Custom images are private to the account where they are created, so you can set the `visibility` property to `private` to retrieve only custom images.

```sh
curl -X GET \
”$vpc_api_endpoint/v1/images?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

## Listing all images by using Terraform
{: #custom-images-list-terraform}
{: terraform}

You can list all of the {{site.data.keyword.vpc_short}} images in your region by using Terraform.

To list all images by using Terraform, use the Terraform data source command [ibm_is_images](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_images){: external}. Custom images are private to the account where they are created, so you can set the `visibility` attribute to `private` to retrieve only custom images.

```terraform
data "ibm_is_images" "images" {
  visibility = "public"
}
```
{: codeblock}

## Viewing custom image details by using the UI
{: #viewing-custom-image-details}
{: ui}

You can view the following details of a custom image:

* Name and ID
* Assigned resource group
* Size
* Location
* Creation date
* Operating system and version
* Encryption type
* Deprecation date (if set)
* Obsolescence date (if set)

You can edit the name of the custom image, adds tags for searchability, and view and copy the unique Cloud Resource Name (CRN). The SHA256 checksum value for the image is accessible.
{: tip}

To view details for a custom image, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
2. On the **Custom images** tab, click the name of a custom image to view details about that image.
3. On the **Image details** page you can edit the name of the image, add tags, and copy the CRN for the image.
4. From the **Actions** menu of the Image details page, you can create a virtual server instance from the custom image or deleting the image. For a full list of possible actions, see [Managing custom images by using the UI](/docs/vpc?topic=vpc-managing-custom-images&interface=ui#custom-images-managing-ui).

## Viewing custom image details by using the CLI
{: #custom-image-details-cli}
{: cli}

You can view the details of a custom image, such as name and ID, operating system and version, encryption status, deprecation or obsolescence dates, and assigned resource group, by using the command-line interface (CLI).

To view the details of a custom image by using the CLI, use the **`ibmcloud is image`** command. Specify the name or ID of the custom image with the `IMAGE` variable.

```sh
ibmcloud is image IMAGE
```
{: pre}

For more information, see [ibmcloud is image](/docs/vpc?topic=vpc-vpc-reference&interface=ui#image-view) in the VPC CLI reference page.

## Retrieving custom image details by using the API
{: #custom-image-retrieve-api}
{: api}

You can view the details of a custom image, such as name and ID, operating system and version, encryption status, deprecation or obsolescence dates, and assigned resource group, by using the application programming interface (API).

To retrieve the details of a specific custom image by using the API, use [Retrieve an image](/apidocs/vpc/latest#get-image). For the `$image_id` variable, specify the ID of the custom image that you want to retrieve.

```sh
curl -X GET \
”$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

## Retrieving custom image details by using Terraform
{: #custom-image-retrieve-terraform}
{: terraform}

You can view the details of a custom image, such as name and ID, operating system and version, encryption status, deprecation or obsolescence dates, and assigned resource group, by using Terraform.

To view the details of an image by using Terraform, use the Terraform data source command [ibm_is_images](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_images){: external}. For the `name` variable, specify the name of the custom image that you want to retrieve.


```terraform
data "ibm_is_images" "image_name" {
}
```
{: codeblock}

## Exporting a custom image to {{site.data.keyword.cos_full_notm}}
{: #custom-image-export-to-cos}

You can export a custom image to {{site.data.keyword.cos_full_notm}}. Later, you might want to copy the image to a new region by importing it again, or you might want to extract the image outside of VPC for use in a different location, such as on premises. Charges might be incurred for storing images in {{site.data.keyword.cos_full_notm}}. For more information, see [Billing](/docs/cloud-object-storage?topic=cloud-object-storage-billing).

### Prerequisites
{: #custom-image-export-to-cos-prereqs}

To export a custom image, you must meet the following prerequisites:

* You need an IAM role that includes the `is.image.image.export` and `is.image.image.operate` actions. For more information, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).
* You need an {{site.data.keyword.cos_full_notm}} service instance created. You must also create an authorization for the Image Service for VPC to access {{site.data.keyword.cos_full_notm}} with the Writer service access role. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import and export images](/docs/vpc?topic=vpc-object-storage-prereq).
* You need IAM access to write to the bucket where you plan to export an image. For more information, see [Assigning access to an individual bucket](/docs/cloud-object-storage?topic=cloud-object-storage-iam-bucket-permissions).

If the image that is being exported is deleted while an export job is in progress, the image status is set to `deleting`. The export job is allowed to complete before deletion of the image completes. When the image deletion is complete, its export job history is not available. You can use [Activity Tracker](/docs/vpc?topic=vpc-at-events#events-images) events for the export job to determine its final status.
{: note}

### Exporting a custom image by using the UI
{: #custom-image-export-to-cos-ui}
{: ui}

To export a custom image to {{site.data.keyword.cos_full_notm}}, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
2. On the **Custom images** tab, click the name of the custom image that you want to export.
3. On the image details page, click the **Actions** menu and select **Export**.
4. On the **Export custom image panel**, you can optionally specify a name for the export job. The name is used, along with the image name, to construct the name of the exported object in {{site.data.keyword.cos_full_notm}}. If you do not specify a name, a name is automatically generated.
5. Select the file format for exporting. Valid file formats are **QCOW2** and **VHD**. If the custom image is an encrypted image, **QCOW2** format is required.
6. Specify the {{site.data.keyword.cos_full_notm}} service instance and bucket where you want to export the custom image. Alternatively you can specify the CRN for the {{site.data.keyword.cos_full_notm}} bucket where you want to export the image.
7. Click **Export custom image**.

### Exporting a custom image by using the CLI
{: #custom-image-export-to-cos-cli}
{: cli}

You can export an {{site.data.keyword.vpc_short}} custom image to {{site.data.keyword.cos_full_notm}} by using the command-line interface (CLI).

To export a custom image by using the CLI, use the **ibmcloud is image-export-job-create** command. Specify the name or ID of the custom image to export by using the `IMAGE` variable. You can specify a name for the export job with `NEW_NAME`. You can also specify the file format for exporting the image, either `qcow2` or `vhd`. The `qcow2` format is the default file format and is the required format if the custom image is encrypted. Additionally, specify the {{site.data.keyword.cos_full_notm}} `BUCKET` where you want to export the custom image.

```sh
ibmcloud is image-export-job-create IMAGE [--name NEW_NAME] [--format qcow2 | vhd] --bucket BUCKET
```
{: pre}

The following example exports a custom image with the ID of `72251a2e-d6c5-42b4-97b0-b5f8e8d1f479`. The export job is named `my-export-job`. The file format for exporting the image is `qcow2`. The {{site.data.keyword.cos_full_notm}} bucket where the image exported is named `my-bucket`.

```sh
ibmcloud is image-export-job-create 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --name my-export-job --format qcow2 --bucket my-bucket
```
{: pre}

For more information, see [ibmcloud is image-export-job-create](/docs/vpc?topic=vpc-vpc-reference#image-export-job-create) in the VPC CLI reference page.

### Exporting a custom image with the API
{: #custom-image-export-to-cos-api}
{: api}

You can export an {{site.data.keyword.vpc_short}} custom image to {{site.data.keyword.cos_full_notm}} by using the application programming interface (API).

To export a custom image by using the API, use [Create an image export job](/apidocs/vpc/latest#create-image-export-job).

For the`$image_id` variable, specify the ID of the custom image that you want to export. You can optionally specify a meaningful `name` for the image export job. In this example, the export job name is `my-image-export`. For the `storage_bucket` `name` subproperty, specify the name of the {{site.data.keyword.cos_full_notm}} bucket where you want to export the custom image. In this example, the bucket name is `bucket-27200-lwx4cfvcue`. Alternatively, you can use the `crn` subproperty to specify the CRN of the bucket.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token" \
-d '{
  "name": "my-image-export",
  "storage_bucket": {
    "name": "bucket-27200-lwx4cfvcue"
  }
}'
```
{: codeblock}

## Viewing and managing custom image export history
{: #custom-image-export-job-history}

You can view the custom image export history to see details such as the export job name, status, target bucket in {{site.data.keyword.cos_full_notm}}, the {{site.data.keyword.cos_full_notm}} object name that is assigned, the date that the export job started, and if the export job is complete, the completion date.

You can also delete a completed image export job. Or you can delete (and cancel) an in-progress image export job if you have an IAM role that includes the `is.image.image.export` action.

When you delete an image export job, it is removed only from the export history. The exported image file is not removed from the {{site.data.keyword.cos_full_notm}} bucket. You must manage files that are stored in {{site.data.keyword.cos_full_notm}} by using the {{site.data.keyword.cos_full_notm}} interfaces.
{: important}

Finally, you can rename an image export job. Renaming the image export job does not change the name of the object that is created in {{site.data.keyword.cos_full_notm}}.

### Limits for image export jobs
{: #custom-image-export-job-limits}

The following limits exist for image export jobs:

* Five active export job requests per image
* Ten total export jobs per image
* Twenty active export jobs per account per region

### Viewing custom image export history by using the UI
{: #custom-image-export-job-history-ui}
{: ui}

To view the export history of a custom image, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
2. On the **Custom images** tab, click the name of a custom image to view details about that image.
3. On the **Image details** page, click the **Export history** tab.

#### Deleting or canceling an image export job by using the UI
{: #custom-image-export-job-delete-ui}
{: ui}

To delete a completed image export job or cancel (and delete) an in-progress image export job, complete the following steps.

1. On the image details page, click the **Export history** tab.
2. For the export job that you want to delete or cancel, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **Delete**.

#### Renaming an image export job by using the UI
{: #custom-image-export-job-rename-ui}
{: ui}

To rename an image export job, complete the following steps.

1. On the image details page, click the **Export history** tab.
2. For the export job that you want to rename, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **Rename**.


### Viewing custom image export history by using the CLI
{: #custom-image-export-job-history-cli}
{: cli}

To view the export history of a specific custom image by using the CLI, use the **ibmcloud is image-export-jobs** command. Specify the name or ID of the custom image that was exported by using the `IMAGE` variable.

```sh
ibmcloud is image-export-jobs IMAGE
```
{: pre}

The following example shows the export job history for a custom image with the ID of `72251a2e-d6c5-42b4-97b0-b5f8e8d1f479`.

```sh
ibmcloud is image-export-jobs 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479
```
{: pre}

For more information, see [ibmcloud is image-export-jobs](/docs/vpc?topic=vpc-vpc-reference#image-export-jobs-list) in the VPC CLI reference page.

### Viewing image export job details by using the CLI
{: #custom-image-export-job-details-cli}
{: cli}

To view the details of a specific image export job by using the CLI, use the **`ibmcloud is image-export-job`** command. Specify the name or ID of the custom image that was exported by using the `IMAGE` variable. Specify the name or ID of the image export job by using the `JOB` variable.

```sh
ibmcloud is image-export-job IMAGE JOB
```
{: pre}

For more information, see [ibmcloud is image-export-job](/docs/vpc?topic=vpc-vpc-reference&interface=ui#image-export-job-view) in the VPC CLI reference page.

### Deleting or canceling an image export job by using the CLI
{: #custom-image-export-job-delete-cli}
{: cli}

To delete a completed image export job or cancel (and delete) an in-progress image export job, use the **`ibmcloud is image-export-job-delete`** command. Specify the name or ID of the custom image that was exported by using the `IMAGE` variable. Specify the name or ID of the image export job by using the `JOB` variable.

```sh
ibmcloud is image-export-job-delete IMAGE JOB
```
{: pre}

For more information, see [ibmcloud is image-export-job-delete](/docs/vpc?topic=vpc-vpc-reference#image-export-job-delete) in the VPC CLI reference page.

### Renaming an image export job by using the CLI
{: #custom-image-export-job-rename-cli}
{: cli}

To rename a custom image export job by using the CLI, use the **`ibmcloud is image-export-job-update`** command. Specify the name or ID of the custom image that was exported by using the `IMAGE` variable. Specify the name or ID of the image export job by using the `JOB` variable. Specify the new name that you want to assign to the image export job by using the `NAME` variable.

```sh
ibmcloud is image-export-job-update IMAGE JOB --name NAME
```
{: pre}

For more information, see [ibmcloud is image-export-job-update](/docs/vpc?topic=vpc-vpc-reference#image-export-job-update) in the VPC CLI reference page.

### Viewing custom image export history by using the API
{: #custom-image-export-job-history-api}
{: api}

To view the export history of a specific custom image by using the API, use [List all image export jobs](/apidocs/vpc/latest#create-image-export-job).

For the`$image_id` variable, specify the ID of the custom image for which you want to display export jobs.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

### Viewing image export job details by using the API
{: #custom-image-export-job-details-api}
{: api}

To view the details of a specific image export job by using the API, use [Retrieve an image export job](/apidocs/vpc/latest#get-image-export-job).

For the`$image_id` variable, specify the ID of the custom image that was exported. For the `$export_job_id` variable, specify the ID of the image export job for which you want to retrieve details.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs/$export_job_id?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

### Deleting or canceling an image export job by using the API
{: #custom-image-export-job-delete-api}
{: api}

To delete a completed image export job or cancel (and delete) an in-progress image export job by using the API, use [Delete an image export job](/apidocs/vpc/latest#delete-image-export-job).

For the`$image_id` variable, specify the ID of the custom image for which you want to delete an export job. For the `$export_job_id` variable, specify the ID of the image export job that you want to delete.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs/$export_job_id?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

### Renaming an image export job by using the API
{: #custom-image-export-job-rename-api}
{: api}

To rename a custom image export job by using the API, use [Update an image export job](/apidocs/vpc/latest#update-image-export-job).

For the`$image_id` variable, specify the ID of the custom image for which you want to rename an export job. For the `$export_job_id` variable, specify the ID of the image export job that you want to rename. The new name to assign to the image export job in the following example is `my-export-job-patched`.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs/$export_job_id?version=2022-11-21&generation=2" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $iam_token" \
-d '{
      "name": "my-export-job-patched"
    }'
```
{: codeblock}

## Sharing a custom image to a private catalog by using the UI
{: #sharing-custom-image-private-catalog-ui}
{: ui}

You can share an existing {{site.data.keyword.vpc_short}} custom image to an existing private catalog. If you don't already have a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).

The **Share image to private catalog** action is not available for the following images:
* The image is already shared with a private catalog. An image in a private catalog can exist in only one version within one product offering in one private catalog. You must remove the image from the version it is in first before that image can be shared in a different version.
* The image is encrypted.
* The image is not an x86 image.

To share a custom image to a private catalog, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
1. On the **Custom images** tab, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **Share to catalog**.
1. On the **Share image to private catalog** page, select a **Private catalog** from the list of available private catalogs.
1. Enter a version number in **Software version**.
1. Click **Create catalog offering** to share the image to the private catalog.
1. After the image is created, click **Validate image**.

This process shares only the custom image to the private catalog. The validation process occurs within the private catalog. Clicking **Validate image** takes you to a new page and from this screen, you can start with [Step 3: Validate the virtual server image in Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui#catalog-vsivpc-review-images) to complete the validation process.
{: note}

## Sharing a custom image to a private catalog by using the CLI
{: #sharing-custom-image-private-catalog-cli}
{: cli}

You can only share a custom image to a private catalog from {{site.data.keyword.vpc_short}} by using the UI. For information on onboarding to a private catalog using the CLI, see [Onboarding software to your account](/docs/account?topic=account-create-private-catalog&interface=cli).

## Sharing a custom image to a private catalog by using the API
{: #sharing-custom-image-private-catalog-api}
{: api}

You can only share a custom image to a private catalog from {{site.data.keyword.vpc_short}} by using the UI. For information on onboarding to a private catalog using the API, see [Onboarding software to your account](/docs/account?topic=account-create-private-catalog&interface=api).

## Sharing a custom image to a private catalog by using Terraform
{: #sharing-custom-image-private-catalog-terraform}
{: terraform}

You can only share a custom image to a private catalog from {{site.data.keyword.vpc_short}} by using the UI. For information on onboarding to a private catalog using the Terraform, see [Onboarding software to your account](/docs/account?topic=account-create-private-catalog&interface=terraform).

## Schedule a custom image lifecycle status change in the UI
{: #schedule-ilm-status-change-ui}
{: ui}

You can schedule either a single image lifecycle status change or schedule the status changes for the entire lifecycle of the image. You can make an immediate status change only if you didn't schedule a future status change.

Follow these steps to schedule a single status change:
<!-- see the -include-segments folder to update this information which is shared with vpc_image_from_volume_manage.md -->
{{_include-segments/image_single_status_lifecycle_schedule.md}}

Follow these steps to schedule the entire lifecycle of the image:
{{_include-segments/image_complete_lifecycle_status.md}}

## Change the custom image lifecycle status by using the CLI
{: #schedule-ilm-status-change-cli}
{: cli}

You can change the lifecycle status of an {{site.data.keyword.vpc_short}} custom image by using the command-line interface (CLI). You can make an immediate status change by using the **ibmcloud is image-deprecate** or **ibmcloud is image-obsolete** commands. You can also schedule these status changes for a future date and time by using the **ibmcloud is image-update** command. Specify the name or ID of the custom image with the `IMAGE` variable.

<!-- see the -include-segments folder to update this information which is shared with vpc_image_from_volume_manage.md -->
{{_include-segments/ilm-change-image-lifecycle-status-cli.md}}

## Remove a previously scheduled custom image lifecycle status change by using the CLI
{: #schedule-reset-ilm-status-change-cli}
{: cli}

<!-- see the -include-segments folder to update this information which is shared with vpc_image_from_volume_manage.md -->
{{_include-segments/ilm-reset-image-lifecycle-status-cli.md}}

## Change the custom image lifecycle status by using the API
{: #schedule-ilm-status-change-API}
{: api}

You can change the lifecycle status of an {{site.data.keyword.vpc_short}} custom image by using the application programming interface (API). You can make an immediate status change or schedule a status change to happen later.

<!-- see the -include-segments folder to update this information which is shared with vpc_image_from_volume_manage.md -->
{{_include-segments/ilm-change-image-lifecycle-status-api.md}}

## Remove previously scheduled custom image lifecycle status by using the API
{: #schedule-ilm-reset-status-change-API}
{: api}

<!-- see the -include-segments folder to update this information which is shared with vpc_image_from_volume_manage.md -->
{{_include-segments/ilm-reset-image-lifecycle-status-api.md}}

## Change the custom image lifecycle status using Terraform
{: #schedule-ilm-status-change-terraform}
{: terraform}

To make an immediate status change using Terraform, use the [`ibm_is_image_deprecate`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_image_deprecate){: external} or [`ibm_is_image_obsolete`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_image_obsolete){: external} resource commands. use one of the following examples. For the `name` variable, specify the name of the custom image for the status change.

You can make an immediate status change only if a future status change is not scheduled. To remove a scheduled status change, see [Remove a scheduled custom image lifecycle status change by using Terraform](/docs/vpc?topic=vpc-managing-custom-images&interface=terraform#schedule-ilm-reset-status-change-terraform). After you remove the scheduled status change, you can make an immediate status change.
{: note}

* Change image lifecycle status to `deprecated`.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     href               = "cos://us-south/buckettesttest/livecd.ubuntu-cpc.azure.vhd"
     operating_system   = "ubuntu-16-04-amd64"
     encrypted_data_key = "eJxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx0="
     encryption_key     = "crn:v1:bluemix:public:kms:us-south:a/6xxxxxxxxxxxxxxx:xxxxxxx-xxxx-xxxx-xxxxxxx:key:dxxxxxx-fxxx-4xxx-9xxx-7xxxxxxxx"
   }
   resource "ibm_is_image_deprecate" "example" {
     image               = ibm_is_image.example.id
   }
   ```
   {: pre}

* Change the image lifecycle status to `obsolete`.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     href               = "cos://us-south/buckettesttest/livecd.ubuntu-cpc.azure.vhd"
     operating_system   = "ubuntu-16-04-amd64"
     encrypted_data_key = "eJxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx0="
     encryption_key     = "crn:v1:bluemix:public:kms:us-south:a/6xxxxxxxxxxxxxxx:xxxxxxx-xxxx-xxxx-xxxxxxx:key:dxxxxxx-fxxx-4xxx-9xxx-7xxxxxxxx"
   }
   resource "ibm_is_image_obsolete" "example" {
     image               = ibm_is_image.example.id
   }
   ```
   {: pre}

To schedule a status change, use one of the following examples.

For the `deprecation_at` or `obsolescence_at` attribute, specify a date in the ISO 8601 (`YYYY-MM-DDThh:mm:ss+hh:mm`) date and time format.

* `YYYY` is the four digit year
* `MM` is the two digit month
* `DD` is the two digit day
* `T` separates the date and time information
* `hh` is the two digit hours
* `mm` is the two digit minutes
* `+hh:mm` or `-hh:mm` is the UTC time zone

Thus, the date of 30 September 2023 at 8:00 p.m. in the North American Central Standard Time Zone (CST) would be `2023-09-30T20:00:00-06:00`

When scheduling the date and time, you can't use your current date and time. For example, if it is 8 a.m. on June 12, then the scheduled date and time must be after 8 a.m. on June 12. If you define both the `deprecation_at` and `obsolescence_at` dates and times, the `obsolescence_at` date must be after the `deprecation_at` date and time.

* Schedule a status change to `deprecated`.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     href               = "cos://us-south/buckettesttest/livecd.ubuntu-cpc.azure.vhd"
     operating_system   = "ubuntu-16-04-amd64"
     deprecation_at     = "2023-11-28T15:10:00.000Z"
   }
   ```
   {: pre}

* Schedule a status change to `obsolete`.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     href               = "cos://us-south/buckettesttest/livecd.ubuntu-cpc.azure.vhd"
     operating_system   = "ubuntu-16-04-amd64"
     obsolescence_at    = "2023-11-28T15:10:00.000Z"
   }
   ```
   {: pre}

   If you want to change the `deprecation_at` and `obsolescence_at` date and time entries, you can run these commands again. The previous dates and times are replaced with the new dates and times.

## Remove previously scheduled custom image lifecycle status using Terraform
{: #schedule-ilm-reset-status-change-terraform}
{: terraform}

To remove any scheduled status change, update the `deprecation_at` or `obsolescence_at` attributes to `null`. This attribute change removes the date and time from the image and the image is no longer scheduled for that status change.

* To change an image from `deprecated` to `available`, change the `deprecation_at` property to `null`.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     href               = "cos://us-south/buckettesttest/livecd.ubuntu-cpc.azure.vhd"
     operating_system   = "ubuntu-16-04-amd64"
     deprecation_at       = null
   }
   ```
   {: pre}

* To change an image from `obsolete` to its previous state, change `obsolescence_at` to `null`. If the image previously contained a value other than `null` for `deprecation_at`, then this attribute change removes the `obsolete` state and changes it back to `deprecated`. If the previous state was `available`, meaning the image was moved from `available` directly to `obsolete`, then this attribute change moves from `obsolete` back to `available`.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     href               = "cos://us-south/buckettesttest/livecd.ubuntu-cpc.azure.vhd"
     operating_system   = "ubuntu-16-04-amd64"
     obsolescence_at    = null
   }
   ```
   {: pre}

* To change an image that has both the `deprecation_at` or `obsolescence_at` properties set back to `available`, you must update both the `deprecation_at` or `obsolescence_at` attributes.

   ```terraform
   resource "ibm_is_image" "example" {
     name               = "example-image"
     href               = "cos://us-south/buckettesttest/livecd.ubuntu-cpc.azure.vhd"
     operating_system   = "ubuntu-16-04-amd64"
     deprecation_at     = null
     obsolescence_at    = null
   }
   ```
   {: pre}
