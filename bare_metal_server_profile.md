---

copyright:
  years: 2021, 2026
lastupdated: "2026-02-26"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# x86-64 bare metal server profiles
{: #bare-metal-servers-profile}

When you create a bare metal server, you can select from a profile family that best fits your needs. A profile provides a different combination of hardware configurations that include the number of vCPUs, amount of RAM, bandwidth speed, and local storage size. The attributes define the size and capabilities of the bare metal server that you provision.
{: shortdesc}

Sapphire Rapids (x3 and x3d) profiles are only available in Dallas (us-south) and Frankfurt (eu-de).
{: preview}

## About profile families
{: #profile-familiy}

Profiles are grouped by the _vCPUs:Memory_ ratio across all the VPC compute offerings. You can choose from the following profile families:

| Family | vCPU:Memory ratio | Description |
|-----|-----|-----|
| Balanced | 1:4 | Best for midsize databases and common cloud applications with moderate traffic. |
| Compute | 1:2 | Best for CPU-intensive demands with moderate to high web traffic, such as production batch processing and front-end web servers. |
| Memory | 1:8 or 1:6 | Best for memory intensive workloads, such as large caching workloads, large database applications, or in-memory analytics workloads. |
| Very High Memory | 1:16 | Best for running small to medium in-memory databases and OLAP workloads, such as SAP BW/4 HANA. |
{: caption="Profile families" caption-side="bottom"}

Very High Memory profiles for Gen2 (x2) are available for customers with special approval. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Profile configurations
{: #bare-metal-servers-profile-list}

Multiple profile generations are available with Intel Cascade Lake processors (x2) or Intel Sapphire Rapids processors (x3). See the following tables to see the available profile configurations.

| Name | vCPU | Memory (GiB) | Local storage (GB) | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| bx2-metal-96x384 | 96 | 384 | 960 | 100 |
| bx2d-metal-96x384  | 96 | 384  | 960  \n 25.6 TB secondary storage (allocation of 8 x 3200) | 100 |
{: caption="Profile families for x2 and x2d" caption-side='top"}
{: #bare-metal-balanced-profiles-x2}
{: tab-title="Balanced profiles for x2 and x2d"}
{: tab-group="profile-configurations-x2"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage (GB) | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| cx2-metal-96x192 | 96 | 192 | 960 | 100 |
| cx2d-metal-96x192 | 96 | 192 | 2x960 SSDs in RAID1    \n 25.6 TB secondary storage (allocation of 8 x 3200) | 100 |
{: caption="Profile families for x2 and x2d" caption-side='top"}
{: #bare-metal-compute-profiles-x2}
{: tab-title="Compute profiles for x2 and x2d"}
{: tab-group="profile-configurations-x2"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage (GB) | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| mx2-metal-96x768 | 96 | 768 | 960 | 100 |
| mx2d-metal-96x768 | 96 | 768 | 2x960 SSDs in RAID1    \n 25.6 TB secondary storage (allocation of 8 x 3200) | 100 |
{: caption="Profile families for x2 and x2d" caption-side='top"}
{: #bare-metal-memory-profiles-mx2d}
{: tab-title="Memory profiles for x2 and x2d"}
{: tab-group="profile-configurations-x2"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage (GB) | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| vx2d-metal-96x1536 | 96 | 1536 | 2x960 SSDs in RAID1   \n 25.6 TB secondary storage (allocation of 8 x 3200) | 100 |
{: caption="Profile families for x2 and x2d" caption-side='top"}
{: #bare-metal-memory-profiles-vx2d}
{: tab-title="Very High Memory profile for x2d"}
{: tab-group="profile-configurations-x2"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage (GB) | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| bx3-metal-48x256 | 48 | 256 | 480 | 200 |
| bx3d-metal-48x256 | 48 | 256 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
| bx3-metal-64x256 | 64 | 256 | 480 | 200 |
| bx3d-metal-64x256 | 64 | 256 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
| bx3d-metal-192x1024 | 192 | 1024 | 2x480 SSDs in RAID1  \n 61.44 TB secondary storage (allocation of 8 x 7680) | 200 |
{: caption="Profile families for x3 and x3d" caption-side='top"}
{: #bare-metal-balanced-profiles-x3}
{: tab-title="Balanced profiles for x3 and x3d"}
{: tab-group="profile-configurations-x3"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage (GB) | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| cx3-metal-48x128 | 48 | 128 | 480 | 200 |
| cx3d-metal-48x128 | 48 | 128 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
| cx3-metal-64x128 | 64 | 128 | 480 | 200 |
| cx3d-metal-64x128 | 64 | 128 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
{: caption="Profile families for x3 and x3d" caption-side='top"}
{: #bare-metal-compute-profiles-x3}
{: tab-title="Compute profiles for x3 and x3d"}
{: tab-group="profile-configurations-x3"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage (GB) | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| mx3-metal-16x128 | 16 | 128 | 480 | 200 |
| mx3d-metal-16x128 | 16 | 128 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
| mx3-metal-48x512 | 48 | 512 | 480 | 200 |
| mx3d-metal-48x512 | 48 | 512 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
| mx3-metal-64x512 | 64 | 512 | 480 | 200 |
| mx3d-metal-64x512 | 64 | 512 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
| mx3d-metal-96x1024 | 96 | 1024 | 480, 8x7600 | 200 |
| mx3d-metal-128x1024 | 128 | 1024 | 2x480 SSDs in RAID1  \n 61.44 TB secondary storage (allocation of 8 x 7680) | 200 |
{: caption="Profile families for x3 and x3d" caption-side='top"}
{: #bare-metal-memory-profiles-mx3d}
{: tab-title="Memory profiles for x3 and x3d"}
{: tab-group="profile-configurations-x3"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage (GB)  | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| vx3-metal-16x256 | 16 | 256 | 480	| 200 |
| vx3d-metal-16x256 | 16 | 256 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
{: caption="Profile families for x3 and x3d" caption-side='top"}
{: #bare-metal-memory-profiles-vx3d}
{: tab-title="Very High Memory profiles for for x3 and x3d"}
{: tab-group="profile-configurations-x3"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GiB) | Local storage (GB) | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| ux3-metal-16x512 | 16 | 512 | 480 | 200 |
| ux3d-metal-16x512 | 16 | 512 | 2x480 SSDs in RAID1  \n 30.72 TB secondary storage (allocation of 4 x 7680) | 200 |
{: caption="Profile families for x3 and x3d" caption-side="top"}
{: #bare-metal-uhmemory-profiles-x3}
{: tab-title="Ultra High Memory profiles for x3 and x3d"}
{: tab-group="profile-configurations-x3"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

### Understanding the naming rule of the profiles
{: #profile-naming-rule-bm-x86-64}

The following information describes the naming rule of the profiles.

* _b_ represents _Balanced_ family profile - _c_ represents the _Compute_ family profile - _m_ represents the _Memory_ family profile - _v_ represents the _Very High Memory_ family profile.
* _x_ represents the _x86_64_ CPU architecture.
* _2_ represents previous generation Intel Cascade Lake processors.
* _3_ represents current generation Intel Sapphire Rapids processors.
* _d_ represents support for additional local storage.
* "metal" denotes that the profile is a bare metal server.
* The last position that contains numbers shows the amount of vCPUs and the amount of memory (GB). For example, _96x384_ means that this profile has 96 vCPUs and 384 GiB of memory.

Using “bx2d-metal-96x384” as an example, it's a _Balanced_ bare metal profile with _96 vCPUs and 384 GiB memory_, has Cascade Lake processors, and NVMe U.2 SSDs.

Bare metal profiles are dedicated servers that provide physical cores. vCPU measurements are used in profile naming only. vCPU to physical cores are a 2:1 ratio (for example, 96 vCPU = 48 physical cores).

## Generation 3 (x3) bare metal profiles availability by region
{: #bare-metal-profile-availability-by-region-gen3}

See the following table to see what Generation 3 (x3) bare metal profiles are available by region.

| Profile |  us-south-dal10-a | us-south-dal12-a | us-south-dal13-a |
| ------- | ----------- | ---------- | ---------- |
| mx3-metal-16x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx3-metal-48x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx3-metal-64x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-16x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx3d-metal-48x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx3d-metal-64x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| vx3-metal-16x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3-metal-48x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3-metal-64x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| vx3d-metal-16x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3d-metal-48x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| ux3-metal-16x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3-metal-48x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3-metal-64x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| ux3d-metal-16x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-48x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-64x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-96x1024  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-128x1024   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3d-metal-192x1024   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-americas-gen3}
{: tab-title="Americas"}
{: tab-group="profile-regions-gen3"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Profile |  eu-de-fra02-a | eu-de-fra04-a  | eu-de-fra05-a  |
| ------- | ----------- | ---------- | ---------- |
| mx3-metal-16x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx3-metal-48x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx3-metal-64x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-16x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx3d-metal-48x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx3d-metal-64x128   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| vx3-metal-16x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3-metal-48x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3-metal-64x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| vx3d-metal-16x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3d-metal-48x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3d-metal-64x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| ux3-metal-16x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3-metal-48x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3-metal-64x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| ux3d-metal-16x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-48x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-64x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-96x1024  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-128x1024   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3d-metal-192x1024   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-europe-gen3}
{: tab-title="Europe"}
{: tab-group="profile-regions-gen3"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}


| Profile |  in-che-che02-a | in-che-che02-b  | in-che-che02-c  |  in-mum-mum02-a | in-mum-mum03-a | in-mum-mum05-a |
| ------- | ----------- | ---------- | ---------- |----------- | ---------- | ---------- |
| mx3-metal-16x128   |   |   |   |   |   |   |
| cx3-metal-48x128   |   |   |   |   |   |   |
| cx3-metal-64x128   |   |   |   |   |   |   |
| mx3d-metal-16x128   |   |   |   |   |   |   |
| cx3d-metal-48x128   |   |   |   |   |   |   |
| cx3d-metal-64x128   |   |   |   |   |   |   |
| vx3-metal-16x256   |   |   |   |   |   |   |
| bx3-metal-48x256   |   |   |   |   |   |   |
| bx3-metal-64x256   |   |   |   |   |   |   |
| vx3d-metal-16x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3d-metal-48x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx3d-metal-64x256   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| ux3-metal-16x512   |   |   |   |   |   |   |
| mx3-metal-48x512   |   |   |   |   |   |   |
| mx3-metal-64x512   |   |   |   |   |   |   |
| ux3d-metal-16x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-48x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-64x512   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx3d-metal-96x1024  |   |   |   |   |   |   |
| mx3d-metal-128x1024   |   |   |   |   |   |   |
| bx3d-metal-192x1024   |   |   |   |   |   |   |
{: caption="Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-ap-gen3}
{: tab-title="Asia Pacific"}
{: tab-group="profile-regions-gen3"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}


## Generation 2 (x2) bare metal profiles availability by region
{: #bare-metal-profile-availability-by-region}

See the following table to see what Generation 2 (x2) bare metal profiles are available by region.

| Profile |  us-south-dal10-a | us-south-dal12-a | us-south-dal13-a | us-east-wdc04-a | us-east-wdc06-a | ca-tor-tor04-a | ca-tor-tor05-a |
| ------- | ----------- | ---------- | ---------- | --------- | -------- | -------- | -------- |
| cx2-metal-96x192   | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| cx2d-metal-96x192  | ![Checkmark icon](../icons/checkmark-icon.svg) | | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| bx2-metal-96x384   | ![Checkmark icon](../icons/checkmark-icon.svg) |  ![Checkmark icon](../icons/checkmark-icon.svg) |  ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |
| bx2d-metal-96x384  | ![Checkmark icon](../icons/checkmark-icon.svg) |  ![Checkmark icon](../icons/checkmark-icon.svg) |  ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| mx2-metal-96x768   | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx2d-metal-96x768  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| vx2d-metal-96x1536 | | ![Checkmark icon](../icons/checkmark-icon.svg) | |
{: caption="Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-americas}
{: tab-title="Americas"}
{: tab-group="profile-regions"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Profile | eu-de-fra02-a | eu-de-fra04-a | eu-es-mad02-a | eu-es-mad05-a | eu-gb-lon04-a |
| ------- | ----------- | ---------- | ---------- | ---------- | ---------- |
| cx2-metal-96x192    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx2d-metal-96x192   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2-metal-96x384    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2d-metal-96x384   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx2-metal-96x768    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx2d-metal-96x768   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | | | ![Checkmark icon](../icons/checkmark-icon.svg) |
| vx2d-metal-96x1536 |  ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-europe}
{: tab-title="Europe"}
{: tab-group="profile-regions"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Profile | jp-tok-tok04-a | jp-tok-tok05-a |
| ------- | ----------- | ----------- |
| cx2-metal-96x192    |  |  |
| cx2d-metal-96x192   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2-metal-96x384    | |  |
| bx2d-metal-96x384   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx2-metal-96x768    |  |  |
| mx2d-metal-96x768   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-asiapacific}
{: tab-title="Asia Pacific"}
{: tab-group="profile-regions"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

## Viewing profile configurations
{: #view-bare-metal-servers-profile}

You can view available profile configurations by using the [UI](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui#view-bare-metal-servers-profile-ui ), [CLI](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=cli#view-bare-metal-servers-profile-cli), or the [API](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=api#view-bare-metal-servers-profile-api).

## Using the UI to view profiles
{: #view-bare-metal-servers-profile-ui}
{: ui}

Use the following steps to view available bare metal profiles by using the UI.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.
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
