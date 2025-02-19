<staging>
---

copyright:
  years: 2022, 2025
lastupdated: "2025-02-19"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adding allowed-use expressions to custom images
{: #custom-image-allowed-use-expressions}

Use an allowed-use expression with your custom image to manage profile and operating system compatabilities.
{: shortdesc}

You can use allowed-use expressions to define your preferred profiles, images, and server settings that can be used when provisioning a virtual server instance and bare metal server. When you provision a virtual server instance or a bare metal server with custom images that have an allowed-use expression, the information provided in the allowed-use properties is then evaluated against a potential virtual server instance or bare metal server properties to determine whether that custom image can be used to create the virtual server instance or bare metal server.

With the UI, CLI, and API, you can define allowed-use expressions for any custom image, image from volume, boot volume, or snapshots. {{site.data.keyword.vsi_is_full}} sets the allowed-use expression for all stock images, but you can define them for any custom image you created. The default value is the allowed-use expressions evaluates to true, meaning that the custom image is allowed to be used with all potential server provisions. You must define or edit the allowed-use expression if you don't want the custom image to be used in specific situations.

For custom images and images shared with a private catalog, you can define the allowed-use expressions when you create the image in {{site.data.keyword.vsi_is_short}}. For image from volume, boot volumes, and snapshots, the source already defines the allowed-use expression. For all images, the allowed-use expression can be updated.

For more information on x86 profiles and to get the information to define the allowed-use expression you want to use for your custom image, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui).

## IAM authority required to define allowed-use expressions
{: #iam-allowed-use-expression}

You must have one or more of the following authorizations to be able to define allowed-use expressions, depending on what types of images you use.

- `is.image.image.manage-allowed-use` for custom images, images managed by a private catalog, and image from volume
- `is.volume.volume-managed-allowed-use` for boot volumes
- `is.snapshot.snapshot.manage-allowed-use` for snapshot images

## Defining allowed-use expressions considerations by using the UI
{: #custom-image-allowed-use-expressions-ui}
{: ui}

Within the UI, you can add or edit the allowed-use expression details in the `Advanced Settings` area. The allowed-use expression section is a text box where you add the expressions as detailed in the following table. If you are viewing an existing image, the image details includes `Virtual server expression` and `Bare metal server expression`. Click edit on one of these fields to open the allowed-use expression text box.

See the following tables for the allowed-use expression details for virtual server instances and bare metal servers.

| Allowed-use expression | Expression option | Expression | Type |
|----------|---------|---------|---------|
| Virtual server expression | `gpu.count`  \n Indicates the number of GPUs assigned to the virtual server instance. | Integer |
| - | `gpu.manufacturer`  \n Indicates the GPU manufacturer. | String |
| -| `gpu.memory`  \n Indicates the overall amount of GPU memory in GiB. | Integer |
| -| `gpu.model`  \n Indicates the GPU model. | String |
| - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the virtual server instance. | Boolean |
{: caption="Virtual server instance UI allowed-use expressions" caption-side="bottom"}
{: #UI-allowed-use-instance}
{: tab-title="Virtual server instance UI allowed-use expressions"}
{: tab-group="UI allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Virtual server instance UI allowed-use expression options"}

| Allowed-use expression | Expression option | Expression | Type |
|----------|---------|---------| ---------|
| Bare metal server expression |`enable_secure_boot`  \n  Indicates whether secure boot is enabled for the bare metal server. | Boolean |
{: caption="Bare metal server UI allowed-use expressions" caption-side="bottom"}
{: #UI-allowed-use-bare-metal}
{: tab-title="Bare metal server UI allowed-use expressions"}
{: tab-group="UI allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Bare metal server UI allowed-use expression options"}

The following example is allowed-use expression for a virtual server instance for GPU count that is greater than one with Nvidia as the GPU manufacturer. Secure boot is required.

```sh
gpu.count > 1 && gpu.manufacturer == 'nvidia' && enable_secure_boot
```
{: pre}

For more information regarding Common Expression Language, which is used to create the allowed-use expression, see [Google's CEL Language Definition reference](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external}

## Defining allowed-use expressions by using the CLI
{: #custom-image-allowed-use-expressions-cli}
{: cli}

See the following tables for the allowed-use expression details for virtual server instances and bare metal servers.

| Allowed-use expression | Expression option | Expression | Type |
|----------|---------|---------|---------|
| `--allowed-use-instance` | `instance` | `gpu.count`  \n Indicates the number of GPUs assigned to the virtual server instance. | Integer |
| - | - | `gpu.manufacturer`  \n Indicates the GPU manufacturer. | String |
| -| - | `gpu.memory`  \n Indicates the overall amount of GPU memory in GiB. | Integer |
| -| - | `gpu.model`  \n Indicates the GPU model. | String |
| - | - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the virtual server instance. | Boolean |
{: caption="Virtual server instance CLI allowed-use expressions" caption-side="bottom"}
{: #CLI-instance}
{: tab-title="Virtual server instance CLI allowed-use expressions"}
{: tab-group="CLI allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Virtual server instance CLI allowed-use expression options"}

| Allowed-use expression | Expression option | Expression | Type |
|----------|---------|---------|---------|
| `--allowed-use-bare-metal-server` | `bare_metal_server` | - |
| - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the bare metal server. | - | Boolean |
{: caption="Bare metal server CLI allowed-use expressions" caption-side="bottom"}
{: #CLI-bare-metal}
{: tab-title="Bare metal server CLI allowed-use expressions"}
{: tab-group="CLI allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Bare metal server CLI allowed-use expression options"}

The following code example shows how to use the allowed-use expression options when creating a new custom image using the CLI. The example sets up the image to require secure boot enablement for a bare metal server, and to allow any provision of a virtual server instance.

```sh
ibmcloud is image-create my-ubuntu-20-amd64 --file cos://us-south/custom-image-vpc-bucket/customImage-0.qcow2 --os-name ubuntu-20-04-amd64 --allowed-use-bare-metal-server "enable_secure_boot==true" --allowed-use-instance true
```
{: pre}

The following code example shows how to use the allowed-use expression options when updating an existing custom image using the CLI. The example is updating an image to require secure boot enablement for provisions of both a bare metal server and a virtual server instance.

```sh
ibmcloud is image-update r134-c9da1575-20cd-4412-9269-8ed08d3ac278 --allowed-use-bare-metal-server enable_secure_boot==true --allowed-use-instance enable_secure_boot==true
```
{: pre}

 For more information about the CLI allowed-use expression options and variables, see the [VPC CLI reference: Images](/docs/vpc?topic=vpc-vpc-reference#compute-images).

## Defining allowed-use expressions by using the API
{: #custom-image-allowed-use-expressions-api}
{: api}

See the following tables for the allowed-use expression details for virtual server instances and bare metal servers.

| Allowed-use expression | Expression option | Expression | Type |
|----------|---------|---------|---------|
| `allowed_use` | `instance` | `gpu.count`  \n Indicates the number of GPUs assigned to the virtual server instance. | Integer |
| - | - | `gpu.manufacturer`  \n Indicates the GPU manufacturer. | String |
| -| - | `gpu.memory`  \n Indicates the overall amount of GPU memory in GiB. | Integer |
| -| - | `gpu.model`  \n Indicates the GPU model. | String |
| - | - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the virtual server instance. | Boolean |
{: caption="Virtual server instance API allowed-use expressions" caption-side="bottom"}
{: #api-instance}
{: tab-title="Virtual server instance API allowed-use expressions"}
{: tab-group="API allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Virtual server instance API allowed-use expression options"}

| Allowed-use expression | Expression option | Expression | Type |
|----------|---------|---------| ---------|
| `allowed_use` | `bare_metal_server` | - |
| - | `enable_secure_boot`  \n  Indicates whether secure boot is enabled for the bare metal server. | - | Boolean |
{: caption="Bare metal server API allowed-use expressions" caption-side="bottom"}
{: #api-bare-metal}
{: tab-title="Bare metal servers API allowed-use expressions"}
{: tab-group="API allowed-use expressions"}
{: class="simple-tab-table"}
{: summary="Bare metal server API allowed-use expression options"}

To create a new custom image with an allow use expression, use the `POST /images` API command. The following example creates a new custom image with an allowed-use expression for a virtual server instance for GPU count that is greater than one with Nvidia as the GPU manufacturer. Secure boot is required.

```sh
curl -X POST "$vpc_api_endpoint/v1/images?version=$tomorrow&generation=2&maturity=development" -H "Authorization: Bearer $iam_token" -d '{
      "name": "my-image",
      "file": {
        "href": "cos://us-south/my-bucket/my-image.qcow2"
      },
      "operating_system": {
        "name": "debian-9-amd64"
      }
      "allowed_use": {
         "instance": "gpu.count > 1 && gpu.manufacturer == 'nvidia' && enable_secure_boot",
         "bare_metal_server": "false"
      }
    }'
```
{: pre}

To update an existing custom image with an allow use expression, use the `PATCH /images` API command. The following example updates an existing custom image with an allowed-use expression for a virtual server instance for GPU count that is greater than one with Nvidia as the GPU manufacturer. Secure boot is required.

```sh
{
"allowed_use": {
  "instance": "gpu.count > 1 && gpu.manufacturer == 'nvidia' && enable_secure_boot",
  "bare_metal_server": "false"
}
```
{: pre}

For more details on the API allowed-use expression property, sub-properties, and expressions, see the [Virtual Private Cloud API: Create image](/apidocs/vpc-scoped#create-image) and [Virtual Private Cloud API: Update image](/apidocs/vpc-scoped#update-image).
</staging>
