---

copyright:
  years: 2021, 2024
lastupdated: "2024-02-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# x86-64 bare metal server profiles
{: #bare-metal-servers-profile}

When you create a bare metal server, you can select from a profile family that best fits your needs. A profile provides a different combination of hardware configurations that include the number of vCPUs, amount of RAM, and local storage size. The attributes define the size and capabilities of the bare metal server that you provision.
{: shortdesc}

## About profile families
{: #profile-familiy}

Profiles are grouped by the *vCPUs:Memory* ratio across all the VPC compute offerings. You can choose from the following profile families:

| Family | vCPU:Memory ratio | Description |
|-----|-----|-----|
| Balanced | 1:4 | Best for midsize databases and common cloud applications with moderate traffic. |
| Compute | 1:2 | Best for CPU-intensive demands with moderate to high web traffic, such as production batch processing and front-end web servers. |
| Memory | 1:8 or 1:6 | Best for memory intensive workloads, such as large caching workloads, large database applications, or in-memory analytics workloads. |
| Very High Memory | 1:16 | Best for running small to medium in-memory databases and OLAP workloads, such as SAP BW/4 HANA. |
{: caption="Table 1. Profile families" caption-side="bottom"}

Very High Memory profiles are available for customers with special approval. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Profile configurations
{: #bare-metal-servers-profile-list}

See the following table to see the available profile configurations.

| Name | vCPU | Memory (GiB) | Local storage | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| bx2-metal-96x384 | 96 | 384 | 960 GB | 100 |
| bx2d-metal-96x384  | 96 | 384  | 960 GB  \n 25.6 TB secondary storage (allocation of 8 x 3200) | 100 |
| bx2-metal-192x768 | 192 | 768 | 960 GB | 100 |
| bx2d-metal-192x768 | 192 | 768 | 960 GB  \n 51.2 TB secondary storage (allocation of 16 x 3200) | 100 |
{: caption="Table 2. Profile families" caption-side='top"}
{: #bare-metal-balanced-profiles}
{: tab-title="Balanced profile"}
{: tab-group="profile-configurations"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| cx2-metal-96x192 | 96 | 192 | 960 GB | 100 |
| cx2d-metal-96x192 | 96 | 192 | 960 GB  \n 25.6 TB secondary storage (allocation of 8 x 3200) | 100 |
{: caption="Table 2. Profile families" caption-side='top"}
{: #bare-metal-compute-profiles}
{: tab-title="Compute profile"}
{: tab-group="profile-configurations"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| mx2-metal-96x768 | 96 | 768 | 960 GB | 100 |
| mx2d-metal-96x768 | 96 | 768 | 960 GB  \n 25.6 TB secondary storage (allocation of 8 x 3200) | 100 |
{: caption="Table 2. Profile families" caption-side='top"}
{: #bare-metal-memory-profiles-mx2d}
{: tab-title="Memory profile"}
{: tab-group="profile-configurations"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| vx2d-metal-96x1536 | 96 | 1536 | 960 GB   \n 25.6 TB secondary storage (allocation of 8 x 3200) | 100 |
{: caption="Table 2. Profile families" caption-side='top"}
{: #bare-metal-memory-profiles-vx2d}
{: tab-title="Very High Memory profile"}
{: tab-group="profile-configurations"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

### Understanding the naming rule of the profiles
{: #profile-naming-rule-bm-x86-64}

The following information describes the naming rule of the profiles.

* *b* represents *Balanced* family profile - *c* represents the *Compute* family profile - *m* represents the *Memory* family profile - *v* represents the *Very High Memory* family profile.
* *x* represents the *x86_64* CPU architecture.
* *2* represents the current generation of processors (Cascade Lake).
* *d* represents support for *NVMe U.2* SSDs.
* "metal" denotes that the profile is a bare metal server.
* The last position that contains numbers shows amount of vCPUs and the amount of memory (GB). For example, *192x768* means that this profile has 192 vCPUs and 768 GiB of memory.

Using “bx2d-metal-192x768” as an example, it's a *Balanced* bare metal profile with *192 vCPUs and 768 GiB memory*, has Cascade Lake processors, and NVMe U.2 SSDs.

Bare metal profiles are dedicated servers that provide physical cores. vCPU measurements are used in profile naming only. vCPU to physical cores are a 2:1 ratio (e.g 192 vCPU = 96 physical cores).

## Profile availability by region
{: #bare-metal-profile-availability-by-region}

See the following table to see what bare metal profiles are available by region.

| Profile |  us-south-1 | us-south-2 | us-south-3 | us-east-2 |
| ------- | ----------- | ---------- | ---------- | --------- |
| cx2-metal-96x192   | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| cx2d-metal-96x192  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| bx2-metal-96x384   | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| bx2d-metal-96x384  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| mx2-metal-96x768   | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| mx2d-metal-96x768  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  ![Checkmark icon](../icons/checkmark-icon.svg)|  |
| bx2-metal-192x768  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2d-metal-192x768 | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| vx2d-metal-96x1536 | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
{: caption="Table 3. Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-americas}
{: tab-title="Americas"}
{: tab-group="profile-regions"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Profile | eu-de-1 | eu-de-2 |
| ------- | ----------- | ---------- |
| cx2-metal-96x192    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx2d-metal-96x192   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2-metal-96x384    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2d-metal-96x384   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx2-metal-96x768    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx2d-metal-96x768   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2-metal-192x768   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2d-metal-192x768  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| vx2d-metal-96x1536 |  ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Table 3. Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-europe}
{: tab-title="Europe"}
{: tab-group="profile-regions"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

## Viewing profile configurations
{: #view-bare-metal-servers-profile}

You can view available profile configurations by using the UI, [CLI](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=cli#view-bare-metal-servers-profile-cli), or the [API](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=api#view-bare-metal-servers-profile-api).

## Using the UI to view profiles
{: #view-bare-metal-servers-profile-ui}
{: ui}

Use the following steps to view available bare metal profiles by using the UI.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.
2. From the **Bare metal servers for VPC** page, click **Create**.
3. On the **New bare metal server for VPC** page, you can view and select profiles under **Profile**.

## Using the CLI to view profiles
{: #view-bare-metal-servers-profile-cli}
{: cli}

Use the following command to list all the bare metal server profiles that are available in a region:

```text
ibmcloud is bare-metal-server-profiles [--output JSON] [-q, --quiet]
```
{: pre}

## Using the API to view profiles
{: #view-bare-metal-servers-profile-api}
{: api}

List all bare metal server profiles that are available in a region by running the following API request:

```text
curl -X GET \
"$vpc_api_endpoint/v1/bare_metal_server/profiles?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

## Next Steps
{: #bare-metal-servers-profile-next-step}

After you choose a profile, you can [create bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).
