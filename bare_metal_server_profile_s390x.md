---

copyright:
  years: 2022
lastupdated: "2022-09-19"

keywords: bare metal server profile, profile, LinuxONE bare metal profiles, viewing profile, view profiles

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

# LinuxONE Bare Metal server profiles
{: #linuxone-bare-metal-servers-profile}

When you create a LinuxONE bare metal server, you can select from a profile family that best fits your needs. A profile provides a different combination of hardware configurations that include number of CPU cores, amount of RAM, and size of local storage. The attributes define the size and capabilities of the bare metal server that is provisioned.
{: shortdesc}


LinuxONE bare metal server profiles are in the "Memory" profile family because their "vCPU : Memory" ratios is larger than "1:16". For more information about profile families, see [Instance Profiles](/docs/vpc?topic=vpc-profiles).

## Profiles configurations
{: #bare-metal-servers-profile-list}

See table 2 for the configurations of each profile.

| Name | CPU Cores | vCPU |Memory (GiB) | SAN storage | Total Network Bandwidth (Gbps) | number of supported interfaces |
|---------|---------|---------|---------|---------|---------|------|
| mz2d-metal-2x64 | 1 | 2 | 64 | 100 GB FCP boot storage on IBM FlashSystem 9200 <br><br>512 GB FCP Data storage | 2 | 1 |
| mz2d-metal-16x512 | 8 | 16 | 512 | 100 GB FCP boot storage on IBM FlashSystem 9200 <br><br>4096 GB FCP Data storage | 10 | 2 |
{: caption="Table 2. LinuxONE Bare Metal servers profile" caption-side="bottom"}

**Note:** Each CPU core of the LinuxONE Bare Metal server has 2 Simultaneous Multithreading (SMT) threads for increasing the efficiency of CPUs to deliver more throughput per processor core.
{: note}

## Understanding the naming rule of the profiles
{: #profile-naming-rule}

The following information describes the naming rule of the profiles.

* *m* represents a *Memory* family profile.
* *x* represents the *x86_64* and *z* represents the *s390x* CPU architecture.
* *2* represents this profile has the current generation of processors (LinuxONE mainframe).
* *d* represents FCP storage based on SAN provided by IBM FlashSystem 9200.
* The field between the two dashes is "metal" for bare metal servers.
* The field after the second dash contains information on the number of CPU cores and the size of memory (GB), for example, "4x256" means that this profile has 4 CPU cores with 2 SMT threads and a memory of 256 GB.

Using “mz2d-metal-8x256 as an example, you can know that it's a *Memory* bare metal profile with *4 CPU cores (2 SMT threads in each CPU core) and 256 GiB memory*. This profile has the s390x architecture processors and FCP storage based on SAN.

## Viewing profile configurations
{: #view-bare-metal-servers-profile}

You can view available profile configurations by using the UI, [CLI](#view-bare-metal-servers-profile-cli), or the [API](#view-bare-metal-servers-profile-api).

## Using the UI to view profiles
{: #view-bare-metal-servers-profile-ui}
{: ui}

Use the following steps to view available bare metal profiles by using the UI.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**.
2. From the **Bare metal servers for VPC** page, click **Create**.
3. On the **New bare metal server for VPC** page, you can view and select profiles under **Profile**.

## Using the CLI to view profiles
{: #view-bare-metal-servers-profile-cli}
{: cli}

Use the following command to list all the bare metal server profiles that are available in a region:

```sh
ibmcloud is bare-metal-server-profiles [--output JSON] [-q, --quiet]
```
{: pre}

## Using the API to view profiles
{: #view-bare-metal-servers-profile-api}
{: api}

List all bare metal server profiles available in a region by running the following API request:

```sh
curl -X GET \
"$vpc_api_endpoint/v1/bare_metal_server/profiles?version=2022-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

## Next Steps
{: #bare-metal-servers-profile-next-step}

After you choose a profile, you can [create bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).
