---

copyright:
  years: 2025, 2025
lastupdated: "2025-07-31"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adding allowed-use expressions to custom images
{: #custom-image-allowed-use-expressions}

Use an allowed-use expression with your custom image to define the capabilities and restrictions of an image and help you find compatible image and profile combinations during server creation.
{: shortdesc}

You can use allowed-use expressions to configure which profiles, images, and server settings can be used together when you provision a virtual server instance or bare metal server. When you make selections to create a virtual server instance or bare metal server, if you use a custom image with defined allowed-use expressions, you can then determine which profiles are compatible with those expressions.

During the creation of a virtual server instance or a bare metal server with custom images that have an allowed-use expression, the information that is provided in the allowed-use properties is then evaluated against a potential virtual server instance or bare metal server properties to determine whether that custom image can be used to create the virtual server instance or bare metal server.

With the UI, CLI, and API, you can define allowed-use expressions for any custom image, image from volume, detached boot volume, or snapshots of boot volumes.  {{site.data.keyword.vpc_full}} sets the allowed-use expressions for all stock images, but you can define them for any custom image you created. The default value is the allowed-use expressions evaluates to true, meaning that the custom image is allowed to be used with all potential server provisions. You must define or edit the allowed-use expressions if you don't want the custom image to be used in specific situations.

For custom images and images shared with a private catalog, you can define the allowed-use expressions when creating the image in {{site.data.keyword.vpc_short}}. For images from volumes, detached boot volumes, and snapshots of boot volumes, the allowed-use expressions are inherited from the source by default. However, you can override these expressions when creating the resource. For all images, boot volumes, and snapshots of boot volumes, the allowed-use expressions can be updated after creation.

For more information on x86 virtual server profiles and to get the information to define the allowed-use expressions you want to use for your custom image, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui). For more information on x86-64 bare metal server profiles, see [x86-64 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui).

## IAM authority required to define allowed-use expressions
{: #iam-allowed-use-expression}

You must have one or more of the following authorizations to be able to define allowed-use expressions, depending on what type of resource you are working with.

- `is.image.image.manage-allowed-use` for custom images, images that are managed by a private catalog, and image from volume
- `is.volume.volume.manage-allowed-use` for boot volumes
- `is.snapshot.snapshot.manage-allowed-use` for snapshots of boot volumes

## Defining allowed-use expressions by using the UI
{: #custom-image-allowed-use-expressions-ui}
{: ui}

When you are creating an image by using the UI, you can add the allowed-use expression details in the `Advanced Options` area. If you are viewing an existing image, the image details include `Virtual server expression` and `Bare metal server expression`. Click edit on one of these fields to open the allowed-use expression text box.

The allowed-use expression section is a text box where you can add the expressions as detailed in the following table. See the following tables for the allowed-use expression details for virtual server instances and bare metal servers.

| Allowed-use expression | Expression variable | Type |
|----------|---------|---------|
| Virtual server expression | `gpu.count`  \n Indicates the number of GPUs assigned to the virtual server instance. | Integer |
| - | `gpu.manufacturer`  \n Indicates the GPU manufacturer. | Case-sensitive string |
| -| `gpu.memory`  \n Indicates the overall amount of GPU memory in GiB. | Integer |
| -| `gpu.model`  \n Indicates the GPU model. | Case-sensitive string |
| - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the virtual server instance. | Boolean |
{: caption="Virtual server instance UI allowed-use expressions" caption-side="bottom"}
{: #UI-allowed-use-instance}
{: tab-title="Virtual server instance UI allowed-use expressions"}
{: tab-group="UI allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Virtual server instance UI allowed-use expression options"}

| Allowed-use expression | Expression variable | Type |
|----------|---------|---------|
| Bare metal server expression |`enable_secure_boot`  \n  Indicates whether secure boot is enabled for the bare metal server. | Boolean |
{: caption="Bare metal server UI allowed-use expressions" caption-side="bottom"}
{: #UI-allowed-use-bare-metal}
{: tab-title="Bare metal server UI allowed-use expressions"}
{: tab-group="UI allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Bare metal server UI allowed-use expression options"}

The following example is an allowed-use expression for a virtual server instance for GPU count that is greater than one with Nvidia as the GPU manufacturer. Secure boot is required.

```json
gpu.count > 1 && gpu.manufacturer == 'nvidia' && enable_secure_boot
```
{: codeblock}

For more information about Common Expression Language, which is used to create the allowed-use expression, see [Google's CEL Language Definition reference](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external}

## Defining allowed-use expressions by using the CLI
{: #custom-image-allowed-use-expressions-cli}
{: cli}

See the following tables for the allowed-use expression details for virtual server instances and bare metal servers.

| Allowed-use option | Expression variable | Type |
|----------|---------|---------|
| `--allowed-use-instance` |  `gpu.count`  \n Indicates the number of GPUs assigned to the virtual server instance. | Integer |
| - | `gpu.manufacturer`  \n Indicates the GPU manufacturer. | Case-sensitive string |
| -| `gpu.memory`  \n Indicates the overall amount of GPU memory in GiB. | Integer |
| -| `gpu.model`  \n Indicates the GPU model. | Case-sensitive string |
| - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the virtual server instance. | Boolean |
{: caption="Virtual server instance CLI allowed-use expressions" caption-side="bottom"}
{: #CLI-instance}
{: tab-title="Virtual server instance CLI allowed-use expressions"}
{: tab-group="CLI allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Virtual server instance CLI allowed-use expression options"}

| Allowed-use option | Expression variable | Type |
|----------|---------|---------|
| `--allowed-use-bare-metal-server` | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the bare metal server. | Boolean |
{: caption="Bare metal server CLI allowed-use expressions" caption-side="bottom"}
{: #CLI-bare-metal}
{: tab-title="Bare metal server CLI allowed-use expressions"}
{: tab-group="CLI allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Bare metal server CLI allowed-use expression options"}

The following code example shows how to use the allowed-use expression options when you create a new custom image by using the CLI. The example sets up the image to require secure boot enablement for a bare metal server, and to allow any provision of a virtual server instance.

```sh
ibmcloud is image-create my-ubuntu-20-amd64 --file cos://us-south/custom-image-vpc-bucket/customImage-0.qcow2 --os-name ubuntu-20-04-amd64 --allowed-use-bare-metal-server "enable_secure_boot==true" --allowed-use-instance true
```
{: pre}

The following code example shows how to use the allowed-use expression options when you update an existing custom image by using the CLI. The example is updating an image to require secure boot enablement for provisions of both a bare metal server and a virtual server instance.

```sh
ibmcloud is image-update r006-c9da1575-20cd-4412-9269-8ed08d3ac278 --allowed-use-bare-metal-server enable_secure_boot==true --allowed-use-instance enable_secure_boot==true
```
{: pre}

 For more information about the CLI allowed-use expression options and variables, see the [VPC CLI reference: Images](/docs/vpc?topic=vpc-vpc-reference#compute-images). For more information regarding Common Expression Language, which is used to create the allowed-use expression, see [Google's CEL Language Definition reference](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external}

## Defining allowed-use expressions by using the API
{: #custom-image-allowed-use-expressions-api}
{: api}

See the following tables for the allowed-use expression details for virtual server instances and bare metal servers.

| Allowed-use property | Allowed-use subproperty | Expression variable | Type |
|----------|---------|---------|---------|
| `allowed_use` | `instance` | `gpu.count`  \n Indicates the number of GPUs assigned to the virtual server instance. | Integer |
| - | - | `gpu.manufacturer`  \n Indicates the GPU manufacturer. | Case-sensitive string |
| -| - | `gpu.memory`  \n Indicates the overall amount of GPU memory in GiB. | Integer |
| -| - | `gpu.model`  \n Indicates the GPU model. | Case-sensitive string |
| - | - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the virtual server instance. | Boolean |
{: caption="Virtual server instance API allowed-use expressions" caption-side="bottom"}
{: #api-instance}
{: tab-title="Virtual server instance API allowed-use expressions"}
{: tab-group="API allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Virtual server instance API allowed-use expression options"}

| Allowed-use property | Allowed-use sub-property | Expression variable | Type |
|----------|---------|---------| ---------|
| `allowed_use` | `bare_metal_server` | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the bare metal server. | Boolean |
{: caption="Bare metal server API allowed-use expressions" caption-side="bottom"}
{: #api-bare-metal}
{: tab-title="Bare metal servers API allowed-use expressions"}
{: tab-group="API allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Bare metal server API allowed-use expression options"}

To create a new custom image with allowed-use expressions, use the `POST /images` API command. The following example creates a new custom image with an allowed-use expression for a virtual server instance for GPU count that is greater than one with Nvidia as the GPU manufacturer. Secure boot is required. The bare metal server allowed-use expression is set to false which prevents the image from being used to provision a bare metal server.

```sh
curl -X POST "$vpc_api_endpoint/v1/images?version=$today&generation=2" -H "Authorization: Bearer $iam_token" -d '{
      "name": "my-image",
      "file": {
        "href": "cos://us-south/my-bucket/my-image.qcow2"
      },
      "operating_system": {
        "name": "debian-9-amd64"
      },
      "allowed_use": {
         "instance": "gpu.count > 1 && gpu.manufacturer == \"nvidia\" && enable_secure_boot",
         "bare_metal_server": "false"
      }
    }'
```
{: pre}

To update an existing custom image with an allowed-use expression, use the `PATCH /images` API command. The following example updates an existing custom image with an allowed-use expression for a virtual server instance for GPU count that is greater than one with Nvidia as the GPU manufacturer. Secure boot is required. The bare metal server allowed-use expression is set to false which prevents the image from being used to provision a bare metal server.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/images/$image_id?version=$today&generation=2" -H 'Content-Type: application/json' -H "Authorization: Bearer $iam_token"  -d '{
"allowed_use": {
  "instance": "gpu.count > 1 && gpu.manufacturer == \"nvidia\" && enable_secure_boot",
  "bare_metal_server": "false"
  }
}'
```
{: pre}

For more details on the API `allowed_use` property and sub-properties, see the [Virtual Private Cloud API: Create image](/apidocs/vpc-scoped#create-image) and [Virtual Private Cloud API: Update image](/apidocs/vpc-scoped#update-image). For more information regarding Common Expression Language, which is used to create the allowed-use expression, see [Google's CEL Language Definition reference](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external}

## Defining allowed-use expressions by using Terraform
{: #custom-image-allowed-use-expressions-terraform}
{: terraform}

See the following tables for the allowed-use expression details for virtual server instances and bare metal servers.

| Allowed-use attribute | Allowed-use subattribute | Expression variable | Type |
|----------|---------|---------|---------|
| `allowed_use` | `instance` | `gpu.count`  \n Indicates the number of GPUs assigned to the virtual server instance. | Integer |
| - | - | `gpu.manufacturer`  \n Indicates the GPU manufacturer. | Case-sensitive string |
| -| - | `gpu.memory`  \n Indicates the overall amount of GPU memory in GiB. | Integer |
| -| - | `gpu.model`  \n Indicates the GPU model. | Case-sensitive string |
| - | - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the virtual server instance. | Boolean |
{: caption="Virtual server instance Terraform allowed-use expressions" caption-side="bottom"}
{: #terraform-instance}
{: tab-title="Virtual server instance Terraform allowed-use expressions"}
{: tab-group="Terraform allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Virtual server instance Terraform allowed-use expression options"}

| Allowed-use attribute | Allowed-use subattribute | Expression variable | Type |
|----------|---------|---------|---------|
| `allowed_use` | `bare_metal_server` | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the virtual server instance. | Boolean |
{: caption="Bare metal server Terraform allowed-use expressions" caption-side="bottom"}
{: #terraform-bare-metal}
{: tab-title="Bare metal servers Terraform allowed-use expressions"}
{: tab-group="Terraform allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Bare metal server Terraform allowed-use expression options"}

To create a new custom image with allowed-use expressions or update an existing image, update the `ibm_is_image` resource. The following example creates a new custom image with an allowed-use expression for a virtual server instance for GPU count that is greater than one with Nvidia as the GPU manufacturer. Secure boot is required. The bare metal server allowed-use expression is set to false which prevents the image from being used to provision a bare metal server.

```sh
resource "ibm_is_image" "example" {
  name               = "my-image"
  href               = "cos://us-south/buckettesttest/livecd.ubuntu-cpc.azure.vhd"
  operating_system   = "ubuntu-16-04-amd64"
  allowed_use{
    api_version       = "2025-04-03"
    bare_metal_server = “enable_secure_boot == true”
    instance = “gpu.count > 0 && enable_secure_boot == true”
  }
}
```
{: codeblock}

For more information regarding Common Expression Language, which is used to create the allowed-use expressions, see [Google's CEL Language Definition reference](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external}</
