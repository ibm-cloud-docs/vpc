---

copyright:
  years: 2021, 2022
lastupdated: "2022-01-21"

keywords: bare metal server profile, profile, bare metal profiles, viewing profile, view profiles

subcollection: vpc

---

{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Profiles for Bare Metal Servers for VPC
{: #bare-metal-servers-profile}

When you create a bare metal server, you can select from a profile family that best fits your needs. A profile provides a different combination of hardware configurations that include number of vCPU, amount of RAM, and size of local storage. The attributes define the size and capabilities of the bare metal server that is provisioned. 
{: shortdesc}

## About profile families
{: #profile-familiy}

Profiles are grouped by the "vCPUs:Memory" ratio across all the VPC compute offerings. You can choose from three profile families:

| Families | "vCPU:Memory" ratio |
|---------|---------|
| Balanced | 1:4 |
| Compute | 1:2 |
| Memory | 1:8 or 1:6 |
{: caption="Table 1. Profile families" caption-side="bottom"}

For more information about profile families, see [Instance Profiles](/docs/vpc?topic=vpc-profiles).

Bare metal server profiles are in the "Balanced" profile family because their "vCPU : Memory" ratios is "1:4".

## Profiles configurations
{: #bare-metal-servers-profile-list}

See table 2 for the configurations of each profile.

| Name | vCPU | Memory (GiB) | Local storage | Total Network Bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| bx2-metal-192x768 | 192 | 768 | 0.96 TB SATA M.2 mirrored drive * 1 | 100 |
| bx2d-metal-192x768 | 192 | 768 | 0.96 TB SATA M.2 mirrored drive \* 1<br><br>3.2 TB U.2 NVMe SSDs \* 16 | 100 |
{: caption="Table 2. Bare Metal Servers for VPC profiles" caption-side="bottom"}

## Understanding the naming rule of the profiles
{: #profile-naming-rule}

The following information describes the naming rule of the profiles.

* *b* represents a *Balanced* family profile.
 * *x* represents the *x86_64* CPU architecture .
* *2* represents this profile has the current generation of processors (Cascade Lake).
* *d* represents *NVMe U.2* SSDs.
* The field between the two dashes is "metal" for bare metal servers.
* The field after the second dash contains information on the number of vCPU and the size of memory (GB), for example, "192x768" means that this profile has 192 vCPU and a memory of 768 GiB.

Using “bx2d-metal-192x768” as an example, you can know that it's a *Balanced* bare metal profile with *192 vCPU and 768 GiB memory*. This profile has the Cascade Lake processors and NVMe U.2 SSDs for storage.

Bare metal profiles are dedicated servers that provide physical cores. vCPU measurements are used in profile naming only. vCPU to physical cores are a 2:1 ratio (e.g 192 vCPU = 96 physical cores).

## Viewing profile configurations
{: #view-bare-metal-servers-profile}

You can view available profile configurations by using the UI, [CLI](#view-bare-metal-servers-profile-cli), or the [API](#view-bare-metal-servers-profile-api).

## Using the UI to view profiles
{: #view-bare-metal-servers-profile-ui}
{: ui}

Use the following steps to view available bare metal profiles by using the UI.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**.
2. From the **Bare metal servers for VPC** page, click **Create**.
3. On the **New bare metal server for VPC** page, you can view and select profiles under **Profile**.

## Using the CLI to view profiles
{: #view-bare-metal-servers-profile-cli}
{: cli}

Use the following command to list all the bare metal server profiles that are available in a region:

```
ibmcloud is bare-metal-server-profiles [--output JSON] [-q, --quiet]
```
{: pre}

## Using the API to view profiles
{: #view-bare-metal-servers-profile-api}
{: api}

List all bare metal server profiles available in a region by running the following API request:

```
curl -X GET \
"$vpc_api_endpoint/v1/bare_metal_server/profiles?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

## Next Steps
{: #bare-metal-servers-profile-next-step}

After you choose a profile, you can [create bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).

<!--
Profiles for post-GA:
| Name | ** *vCPU* ** | Memory (GiB) | Local storage | Total Network Bandwidth (Gbps) |
| bx2-metal-96x384 | 96 | 384 | 0.96 TB SATA M.2 mirrored drive * 1 | 100 |
| bx2d-metal-96x384 | 96 | 384 | 0.96 TB SATA M.2 mirrored drive * 1
3.2 TB U.2 NVMe SSDs * 8 | 100 |
| mx2-metal-32x192 | 32 | 192 | 0.96 TB SATA M.2 mirrored drives * 1 | 100 |
| mx2d-metal-32x192 |32 | 192 | 0.96 TB SATA M.2 mirrored drives * 1
3.2 TB U.2 NVMe SSDs * 8 | 100 |
| mx2-metal-48x384 | 48 | 384 | 0.96 TB SATA M.2 mirrored drive * 1 | 100 |
| mx2d-metal-48x384 | 48 | 384 | 0.96 TB SATA M.2 mirrored drive * 1
3.2 TB U.2 NVMe SSDs * 8 | 100 |
| mx2-metal-64x384 | 64| 384 | 0.96 TB SATA M.2 mirrored drive * 1 | 100 |
| mx2d-metal-64x384 | 64 | 384 | 0.96 TB SATA M.2 mirrored drive * 1
3.2 TB U.2 NVMe SSDs * 8 | 100 |
| mx2-metal-96x768 | 96 | 768 | 0.96 TB SATA M.2 mirrored drive * 1 | 100 |
| mx2d-metal-96x768 | 96 | 768 | 0.96 TB SATA M.2 mirrored drive * 1
3.2 TB U.2 NVMe SSDs * 8 | 100 |
-->

