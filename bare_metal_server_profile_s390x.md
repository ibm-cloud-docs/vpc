---

copyright:
  years: 2022
lastupdated: "2022-10-11"

keywords: LinuxONE bare metal profiles, s390x bare metal profiles

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# s390x bare metal server profiles
{: #s390x-bare-metal-servers-profile}

s390x Bare Metal Servers for VPC is available for customers with special approval to preview this service in the Washington DC (us-east) and São Paulo (br-sao) regions. 
{: preview}

When you create an s390x bare metal server, you can select an s390x bare metal server profile that best fits your needs. A profile provides a different combination of hardware configurations that include number of CPU cores, amount of RAM, and size of local storage. The attributes define the size and capabilities of the bare metal server that is provisioned.
{: shortdesc}

The s390x bare metal server profiles are in the "Memory" profile family because their "vCPU : Memory" ratio is larger than "1:16". For more information about profile families, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles).

## Profile configurations
{: #bare-metal-servers-profile-list}

See table 1 for the configurations of each profile. Each CPU core of the s390x bare metal server has two Simultaneous Multithreading (SMT) threads for increased CPU efficiency to deliver more throughput per processor core.

| Name | CPU cores | vCPU |Memory (GiB) | SAN storage | Total network bandwidth (Gbps) | Number of supported interfaces |
|---------|---------|---------|---------|---------|---------|------|
| mz2d-metal-2x64 | 1 | 2 | 64 | 100 GB FCP boot storage on IBM FlashSystem 9200  \n  \n 1024 GB FCP Data storage | 2 | 1 |
| mz2d-metal-16x512 | 8 | 16 | 512 | 100 GB FCP boot storage on IBM FlashSystem 9200  \n  \n 4096 GB FCP Data storage | 10 | 2 |
{: caption="Table 1. s390x bare metal server profiles" caption-side="bottom"}

## Understanding the naming rule of profiles
{: #profile-naming-rule}

The following information describes the naming rule of the profiles.

* *m* represents a *Memory* family profile.
* *x* represents the *x86_64* and *z* represents the *s390x* CPU architecture.
* *2* represents the current generation of processors.
* *d* represents FCP storage on SAN provided by IBM FlashSystem 9200.
* The field between the two dashes is "metal" for bare metal servers.
* The field after the second dash contains information on the number of CPU cores and the size of memory (GB). For example, "4x256" means that this profile has four CPU cores with two SMT threads and 256 GB of memory.

Use `mz2d-metal-8x256` as an example. You can know that it's a *Memory* bare metal profile with *four CPU cores (two SMT threads in each CPU core) and 256 GiB memory*. This profile has the s390x architecture processors and FCP storage based on SAN.

## Viewing profile configurations
{: #view-bare-metal-servers-profile}

You can view available profile configurations by using the UI, [CLI](#view-bare-metal-servers-profile-cli), or the [API](#view-bare-metal-servers-profile-api).

## Using the UI to view profiles
{: #view-bare-metal-servers-profile-ui}
{: ui}

Use the following steps to view available bare metal profiles by using the UI.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC infrastructure > Compute > Bare metal servers**.
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

## Next steps
{: #bare-metal-servers-profile-next-step}

After you choose a profile, you can [create a bare metal server for VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).
