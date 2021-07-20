---

copyright:
  years: 2021
lastupdated: "2021-05-19"

keywords: bare metal servers, profile, baremetal profile

subcollection: vpc

---

{:beta: .beta}
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

# Bare Metal Servers for VPC (beta) profiles - draft
{: #bare-metal-servers-profile}

When you create a bare metal server, you can select a profile based on your specific need. Each profile provides a different combination of hardware configurations that include number of vCPU, amount of RAM, and size of local storage. 

Bare Metal Servers for VPC (beta) currently offers two profiles from the "Balanced" profile family. 
{: note}

## About profile families
{: #profile-familiy}

Profiles are grouped by the "vCPUs:Memory" ratio across all the VPC compute offerings. You can choose from three profile families:

| Families | "vCPU:Memory" ratio |
|---------|---------|
| Balanced | 1:4 |
| Compute | 1:2 |
| Memory | 1:8 or 1:6 |
{: caption="Table 1. Profile families" caption-side="top"}

For more information about profile families, see [Instance Profiles](/docs/vpc?topic=vpc-profiles).

Profiles of bare metal servers fall into the "Balanced" profile family as their "vCPU : Memory" ratios is "1:4".

## Profiles of Bare Metal for VPC
{: #bare-metal-servers-profile-list}

See table 2 for the configuration of each profile.

| Name | vCPU | Memory (GiB) | Local storage | Total Network Bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| bx2-metal-192x768 | 192 | 768 | 0.96 TB SATA M.2 mirrored drive * 1 | 100 |
| bx2d-metal-192x768 | 192 | 768 | 0.96 TB SATA M.2 mirrored drive \* 1<br><br>3.2 TB U.2 NVMe SSDs \* 16 | 100 |
{: caption="Table 2. Bare Metal Servers for VPC profiles" caption-side="top"}

## Understanding the naming rules for profiles
{: #profile-naming-rule}

You can know the configuration of a profile through its name.

* *b* represents a profile of the Balanced family.
* *x* represents the CPU architecture is x86_64
* *2* represents this profile has the current generation of processors (Cascade Lake).
* *d* represents this profile has NVMe U.2 SSDs.
* The field between the two dashes is "metal" for bare metal servers.
* The field after the second dash contains information on the number of vCPU and the size of memory (GB), for example, "192x768" means that this profile has 192 vCPU and a memory of 768 GiB.

Take “bx2d-metal-192x768” as an example, you can learn from this profile name that it is a bare metal profile with 192 vCPU and 768 GiB memory. This profile has the Cascade Lake processors. It provides NVMe U.2 SSDs for storage.

## Viewing profile configurations
{: #view-bare-metal-servers-profile}

You can view available profile configurations by using the IBM Cloud console, REST API, or the IBM Cloud Command Line Interface (CLI).

### Using the IBM Cloud console to view profiles
{: #view-bare-metal-servers-profile-ui}

**(Need to verify on the UI later)**

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), navigate to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**.
2. From the **Bare metal servers for VPC** page, click **Create**.
3. On the **New bare metal server for VPC** page, you can view and select profiles under **Profile**.

### Using REST API to view profiles
{: #view-bare-metal-servers-profile-api}

List all the bare metal server profiles available in a region by running the following API request:

```
curl -X GET \
"$vpc_api_endpoint/v1/bare_metal_server/profiles?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{:pre}

### Using the CLI to view profiles
{: #view-bare-metal-servers-profile-cli}

List all the bare metal server profiles available in a region by running the following command:

```
ibmcloud is bare-metal-server-profiles [--output JSON] [-q, --quiet]
```
{:pre}

## Next Steps
{: #bare-metal-servers-profile-next-step}

After you choose a profile, it's time to [create bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).

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

