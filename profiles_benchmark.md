---
copyright:
  years: 2022, 2026
lastupdated: "2026-02-05"

keywords: benchmark, benchmark scores

subcollection: vpc


---
{{site.data.keyword.attribute-definition-list}}

# Linux and Windows benchmark scores
{: #profiles_benchmark}

Linux&reg; and Windows&reg; benchmark scores are measured on high-performance virtual servers for {{site.data.keyword.vpc_full}} (VPC) by using [CoreMark](https://www.eembc.org/coremark/faq.php). CoreMark is an industry standard benchmark that measures the performance of CPUs on physical machines and virtual servers that are offered by Infrastructure-as-a-Service (IaaS) providers like {{site.data.keyword.vpc_short}}.
{: shortdesc}

While the current benchmark results are for Compute instance profiles, we expect the CoreMark scores to be similar for Balanced and Memory instance profiles with the same CPU count.

## Compute x2 profiles
{: #compute_virutal_server_profiles_benchmark}

Compute x2 profiles offers core to RAM ratio of 1 vCPU to 2 GiB. Compute cx2 profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers.

You can provision virtual servers on Compute hosts with Broadwell, Skylake, Cascade Lake and Sapphire Rapid processor types. The benchmark tests are run on all CPU types. The following tables show the benchmark scores for Linux and Windows virtual server Compute instance profiles.

| Profile | CPU | CPU MHz | vCPUs | Score | Standard Deviation (%) | Sample Count |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| cx2-2x4 | Broadwell | 2095.170 | 2 | 24690.74 | 3.76 | 2160 |
| cx2-4x8 | Broadwell | 2095.148 | 4 | 49126.78 | 2.08 | 1560 |
| cx2-8x16 | Broadwell | 2095.148 | 8 | 99210.04 | 1.47 | 1070 |
| cx2-16x32 | Broadwell | 2095.148 | 16 | 197633.25 | 1.38 | 735 |
| cx2-32x64 | Broadwell | 2095.148 | 32 | 389354.87 | 1.01 | 2600 |
| cx2-48x96 | Broadwell | 2095.148 | 48 | 579164.89 | 1.81 | 3150 |
| cx2-2x4 | Skylake | 2593.910 | 2 | 33762.35 | 3.05 | 1180 |
| cx2-4x8 | Skylake | 2593.910 | 4 | 64204.93 | 1.86 | 1010 |
| cx2-8x16 | Skylake | 2593.910 | 8 | 124993.94 | 1.86 | 1280 |
| cx2-16x32 | Skylake | 2593.910 | 16 | 252193.52 | 1.43 | 1020 |
| cx2-32x64 | Skylake | 2593.942 | 32 | 501800.80 | 1.88 | 829 |
| cx2-48x96 | Skylake | 2593.910 | 48 | 733627.36 | 1.53 | 1430 |
| cx2-2x4 | Cascade Lake | 2593.906 | 2 | 32965.71 | 6.12 | 660 |
| cx2-4x8 | Cascade Lake | 2593.906 | 4 | 66823.80 | 2.88 | 1450 |
| cx2-8x16 | Cascade Lake | 2593.910 | 8 | 130840.32 | 2.99 | 1590 |
| cx2-16x32 | Cascade Lake | 2593.910 | 16 | 260456.10 | 2.70 | 1820 |
| cx2-32x64 | Cascade Lake | 2593.938 | 32 | 491829.84 | 2.61 | 2270 |
| cx2-48x96 | Cascade Lake | 2593.948 | 48 | 740440.18 | 2.35 | 1720 |
{: class="simple-tab-table"}
{: caption="Linux benchmark for x2 Compute profile family" caption-side="bottom"}
{: #linux-x2-instance-profiles}
{: tab-title="x2 Linux"}
{: tab-group="x2-instance-profiles-benchmark"}

| Profile | CPU | CPU MHz | vCPUs | Score | Standard Deviation (%) | Sample Count |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| cx2-2x4 | Broadwell | 2095 | 2 | 24856.04 | 1.13 | 2980 |
| cx2-4x8 | Broadwell | 2095 | 4 | 49812.16 | 1.18 | 1980 |
| cx2-8x16 | Broadwell | 2095 | 8 | 99004.92 | 1.74 | 1619 |
| cx2-16x32 | Broadwell | 2095 | 16 | 198064.93 | 1.35 | 1190 |
| cx2-32x64 | Broadwell | 2095 | 32 | 390969.81 | 0.72 | 868 |
| cx2-48x96 | Broadwell | 2096 | 48 | 583921.43 | 0.67 | 1110 |
| cx2-2x4 | Skylake | 2594 | 2 | 33117.14 | 4.66 | 707 |
| cx2-4x8 | Skylake | 2594 | 4 | 64123.32 | 2.38 | 890 |
| cx2-8x16 | Skylake | 2594 | 8 | 125528.91 | 2.81 | 1080 |
| cx2-16x32 | Skylake | 2594 | 16 | 250483.46 | 2.40 | 1000 |
| cx2-32x64 | Skylake | 2594 | 32 | 487394.15 | 1.91 | 870 |
| cx2-48x96 | Skylake | 2594 | 48 | 725383.25 | 2.14 | 610 |
| cx2-2x4 | Cascade Lake | 2594 | 2 | 34456.40 | 1.43 | 476 |
| cx2-4x8 | Cascade Lake | 2594 | 4 | 65935.61 | 1.36 | 1290 |
| cx2-8x16 | Cascade Lake | 2594 | 8 | 128147.69 | 3.14 | 1430 |
| cx2-16x32 | Cascade Lake | 2594 | 16 | 254473.98 | 2.48 | 1630 |
| cx2-32x64 | Cascade Lake | 2594 | 32 | 495187.22 | 2.49 | 2000 |
| cx2-48x96 | Cascade Lake | 2594 | 48 | 735378.48 | 1.75 | 1990 |
{: class="simple-tab-table"}
{: caption="Windows benchmark for x2 Compute profile family" caption-side="bottom"}
{: #windows-x2-instance-profiles}
{: tab-title="x2 Windows"}
{: tab-group="x2-instance-profiles-benchmark"}

## How IBM Cloud measures performance
{: #how_ibm_cloud_measures_performance}

The CoreMark benchmark is created in [PerforKitBenchmarker](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker), an open source cloud benchmarking framework with [ibmcloud provider](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/tree/master/perfkitbenchmarker/providers/ibmcloud), which is contributed by {{site.data.keyword.cloud}} engineers.

For the cx2 profile Linux measurements, virtual servers are created with the Red Hat version 8.4 image in the {{site.data.keyword.cloud}} public catalog and for Windows measurements, servers are created with the Windows Server 2019 image.
