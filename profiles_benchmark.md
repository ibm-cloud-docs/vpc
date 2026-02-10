---
copyright:
  years: 2022, 2026
lastupdated: "2026-02-10"

keywords: benchmark, benchmark scores

subcollection: vpc


---
{{site.data.keyword.attribute-definition-list}}

# Linux and Windows benchmark scores
{: #profiles_benchmark}

Linux&reg; and Windows&reg; benchmark scores are measured on virtual servers for {{site.data.keyword.vpc_full}} (VPC) by using [CoreMark](https://www.eembc.org/coremark/faq.php). CoreMark is an industry standard benchmark that measures the performance of CPUs on physical machines and virtual servers that are offered by Infrastructure-as-a-Service (IaaS) providers like {{site.data.keyword.vpc_short}}.
{: shortdesc}

While the current benchmark results are for Compute instance profiles, we expect the CoreMark scores to be similar for Balanced and Memory instance profiles with the same CPU count.

## Compute x2 profiles
{: #compute_virtual_server_profiles_benchmark}

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

## x3 profiles for Compute, Balanced, and Memory
{: #profiles-benchmark-x3-compute-balanced-memory}

Compute cx3 profiles offers core to RAM ratio of 1 vCPU to 2.5GiB. Compute cx3 profiles are best for moderate to high web traffic workloads.

Balanced bx3 profiles offers core to RAM ratio of 1vCPU to 5GiB. Balanced bx3 profiles offer a core to RAM ratio that is best for midsize databases and common cloud applications with moderate traffic.

Memory mx3 profiles offer a core to RAM ratio of 1vCPU to 10 Gib . Memory mx3 profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads.

You can provision virtual servers on compute, balanced, and memory hosts with Broadwell, Skylake, Cascade Lake and Sapphire Rapid processor types. The benchmark tests are run on all CPU types.

The following tables show the benchmark scores for Linux virtual server compute, balanced, and memory instance profiles.

| Profile | CPU | CPU MHz | vCPUs | Score | Standard Deviation (%) | Sample Count |
| --------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| bx3d-2x10 | Sapphire Rapids | 2100 | 2 | 40757.82 | 0.85 | 736 |
| bx3d-4x20 | Sapphire Rapids | 2100 | 4 | 76553.71 | 0.93 | 790 |
| bx3d-8x40 | Sapphire Rapids | 2100 | 8 | 154691.04 | 1.32 | 754 |
| bx3d-16x80 | Sapphire Rapids | 2100 | 16 | 304402.63 | 1.85 | 769 |
| bx3d-24x120 | Sapphire Rapids | 2100 | 24 | 456695.00 | 1.74 | 738 |
| bx3d-32x160 | Sapphire Rapids | 2100 | 32 | 567898.13 | 2.46 | 736 |
| bx3d-48x240 | Sapphire Rapids | 2100 | 48 | 849300.53 | 2.50 | 780 |
| bx3d-64x320 | Sapphire Rapids | 2100 | 64 | 1118520.02 | 3.65 | 719 |
| bx3d-96x480 | Sapphire Rapids | 2100 | 96 | 1639131.55 | 3.75 | 847 |
| bx3d-128x640 | Sapphire Rapids | 2100 | 128 | 2133338.37 | 4.51 | 759 |
| bx3d-176x880 | Sapphire Rapids | 2100 | 176 | 2830073.61 | 4.03 | 823 |
{: class="simple-tab-table"}
{: caption="Linux benchmark for x3 Balanced profile family" caption-side="bottom"}
{: #linux-x3-balanced-instance-profiles}
{: tab-title="bx3d"}
{: tab-group="Linux-x3-instance-profiles-benchmark"}

| Profile | CPU | CPU MHz | vCPUs | Score | Standard Deviation (%) | Sample Count |
| --------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| cx3d-2x5 | Sapphire Rapids | 2100 | 2 | 40723.11 | 1.06 | 759 |
| cx3d-4x10 | Sapphire Rapids | 2100 | 4 | 76488.99 | 1.12 | 719 |
| cx3d-8x20 | Sapphire Rapids | 2100 | 8 | 154816.84 | 1.09 | 682 |
| cx3d-16x40 | Sapphire Rapids | 2100 | 16 | 305429.92 | 1.01 | 767 |
| cx3d-24x60 | Sapphire Rapids | 2100 | 24 | 457094.26 | 1.37 | 760 |
| cx3d-32x80 | Sapphire Rapids | 2100 | 32 | 567182.09 | 2.54 | 802 |
| cx3d-48x120 | Sapphire Rapids | 2100 | 48 | 848676.26 | 2.64 | 785 |
| cx3d-64x160 | Sapphire Rapids | 2100 | 64 | 1121316 | 2.90 | 713 |
| cx3d-96x240 | Sapphire Rapids | 2100 | 96 | 1631223.28 | 3.96 | 740 |
| cx3d-128x320 | Sapphire Rapids | 2100 | 128 | 2135730.15 | 4.19 | 771 |
| cx3d-176x440 | Sapphire Rapids | 2100 | 176 | 2822074.45 | 4.29 | 796 |
{: class="simple-tab-table"}
{: caption="Linux benchmark for x3 Compute profile family" caption-side="bottom"}
{: #linux-x3-compute-instance-profiles}
{: tab-title="cx3d"}
{: tab-group="Linux-x3-instance-profiles-benchmark"}

| Profile | CPU | CPU MHz | vCPUs | Score | Standard Deviation (%) | Sample Count |
| --------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| mx3d-2x20 | Sapphire Rapids | 2100 | 2 | 40937.92 | 0.47 | 776 |
| mx3d-4x40 | Sapphire Rapids | 2100 | 4 | 76841.26 | 0.80 | 790 |
| mx3d-8x80 | Sapphire Rapids | 2100 | 8 | 155284.53 | 1.04 | 764 |
| mx3d-16x160 | Sapphire Rapids | 2100 | 16 | 306141.72 | 1.28 | 665 |
| mx3d-24x240 | Sapphire Rapids | 2100 | 24 | 458639.79 | 1.15 | 728 |
| mx3d-32x320 | Sapphire Rapids | 2100 | 32 | 569303.73 | 2.61 | 762 |
| mx3d-48x480 | Sapphire Rapids | 2100 | 48 | 851847.96 | 2.53 | 793 |
| mx3d-64x640 | Sapphire Rapids | 2100 | 64 | 1126683.96 | 2.82 | 756 |
| mx3d-96x960 | Sapphire Rapids | 2100 | 96 | 1634841.27 | 3.89 | 847 |
| mx3d-128x1280 | Sapphire Rapids | 2100 | 128 | 2132420.17 | 4.40 | 819 |
| mx3d-176x1760 | Sapphire Rapids | 2100 | 176 | 2808204.56 | 4.48 | 750 |
{: class="simple-tab-table"}
{: caption="Linux benchmark for x3 Memory profile family" caption-side="bottom"}
{: #linux-x3-memory-instance-profiles}
{: tab-title="mx3d"}
{: tab-group="Linux-x3-instance-profiles-benchmark"}

The following tables show the benchmark scores for Windows virtual server compute, balanced, and memory instance profiles.

| Profile | CPU | CPU MHz | vCPUs | Score | Standard Deviation (%) | Sample Count |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| bx3d-2x10 | Sapphire Rapids | 2100 | 2 | 36847.34 | 8.46 | 786 |
| bx3d-4x20 | Sapphire Rapids | 2100 | 4 | 72858.34 | 5.18 | 809 |
| bx3d-8x40 | Sapphire Rapids | 2100 | 8 | 144418.55 | 3.62 | 742 |
| bx3d-16x80 | Sapphire Rapids | 2100 | 16 | 288975.14 | 2.45 | 797 |
| bx3d-24x120 | Sapphire Rapids | 2100 | 24 | 430227.53 | 3.52 | 718 |
| bx3d-32x160 | Sapphire Rapids | 2100 | 32 | 549444.75 | 3.77 | 782 |
| bx3d-48x240 | Sapphire Rapids | 2100 | 48 | 831245.26 | 2.32 | 767 |
| bx3d-64x320 | Sapphire Rapids | 2100 | 64 | 1108089.19 | 2.67 | 798 |
| bx3d-96x480 | Sapphire Rapids | 2100 | 96 | 1683495.42 | 1.99 | 773 |
| bx3d-128x640 | Sapphire Rapids | 2100 | 128 | 2220283.24 | 2.76 | 782 |
| bx3d-176x880 | Sapphire Rapids | 2100 | 176 | 2917796.68 | 3.18 | 683 |
{: class="simple-tab-table"}
{: caption="Windows benchmark for x3 Balanced profile family" caption-side="bottom"}
{: #windows-x3-balanced-instance-profiles}
{: tab-title="bx3d"}
{: tab-group="x3-instance-profiles-benchmark"}

| Profile | CPU | CPU MHz | vCPUs | Score | Standard Deviation (%) | Sample Count |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| cx3d-2x5 | Sapphire Rapids | 2100 | 2 | 36847.34 | 8.46 | 743 |
| cx3d-4x10 | Sapphire Rapids | 2100 | 4 | 73315.4 | 4.79 | 820 |
| cx3d-8x20 | Sapphire Rapids | 2100 | 8 | 144660.78 | 3.59 | 718 |
| cx3d-16x40 | Sapphire Rapids | 2100 | 16 | 288182.98 | 2.80 | 760 |
| cx3d-24x60 | Sapphire Rapids | 2100 | 24 | 426398.8 | 4.28 | 741 |
| cx3d-32x80 | Sapphire Rapids | 2100 | 32 | 553045.24 | 2.72 | 696 |
| cx3d-48x120 | Sapphire Rapids | 2100 | 48 | 826408.76 | 3.14 | 724 |
| cx3d-64x160 | Sapphire Rapids | 2100 | 64 | 1116247.34 | 1.43 | 751 |
| cx3d-96x240 | Sapphire Rapids | 2100 | 96 | 1672736.52 | 2.81 | 696 |
| cx3d-128x320 | Sapphire Rapids | 2100 | 128 | 2232073.18 | 2.09 | 724
| cx3d-176x440 | Sapphire Rapids | 2100 | 176 | 2957538.32 | 2.14 | 821 |
{: class="simple-tab-table"}
{: caption="Windows benchmark for x3 Compute profile family" caption-side="bottom"}
{: #windows-x3-compute-instance-profiles}
{: tab-title="cx3d"}
{: tab-group="x3-instance-profiles-benchmark"}

| Profile | CPU | CPU MHz | vCPUs | Score | Standard Deviation (%) | Sample Count |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| mx3d-2x20 | Sapphire Rapids | 2100 | 2 | 36979.58 | 8.38 | 685 |
| mx3d-4x40 | Sapphire Rapids | 2100 | 4 | 73476.33 | 4.86 | 765 |
| mx3d-8x80 | Sapphire Rapids | 2100 | 8 | 143968.23 | 4.24 | 733 |
| mx3d-16x160 | Sapphire Rapids | 2100 | 16 | 288211.8 | 3.00 | 723 |
| mx3d-24x240| Sapphire Rapids | 2100 | 24 | 432492.62 | 2.74 | 790 |
| mx3d-32x320 | Sapphire Rapids | 2100 | 32 | 551855.71 | 3.16 | 799 |
| mx3d-48x480 | Sapphire Rapids | 2100 | 48 | 833851.25 | 1.76 | 754 |
| mx3d-64x640 | Sapphire Rapids | 2100 | 64 | 1112305.82 | 2.00 | 701 |
| mx3d-96x960 | Sapphire Rapids | 2100 | 96 | 1646519.69 | 3.96 | 836 |
| mx3d-128x1280 | Sapphire Rapids | 2100 | 128 | 2158757.43 | 4.19 | 800 |
| mx3d-176x1760 | Sapphire Rapids | 2100 | 176 | 2797826.33 | 4.69 | 840 |
{: class="simple-tab-table"}
{: caption="Windows benchmark for x3 Memory profile family" caption-side="bottom"}
{: #windows-x3-memory-instance-profiles}
{: tab-title="mx3d"}
{: tab-group="x3-instance-profiles-benchmark"}

## How IBM Cloud measures performance
{: #how_ibm_cloud_measures_performance}

The CoreMark benchmark is created in [PerforKitBenchmarker](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker), an open source cloud benchmarking framework with [ibmcloud provider](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/tree/master/perfkitbenchmarker/providers/ibmcloud), which is contributed by {{site.data.keyword.cloud}} engineers.

For the cx2 profile Linux measurements, virtual servers are created with the Red Hat version 8.4 image in the {{site.data.keyword.cloud}} public catalog and for Windows measurements, servers are created with the Windows Server 2019 image.

For the bx3, cx3, and mx3 profiles Linux measurements, virtual servers are created with the Red Hat version 9.0 image in the {{site.data.keyword.cloud}} public catalog and for Windows measurements, servers are created with the Windows Server 2022 image.
