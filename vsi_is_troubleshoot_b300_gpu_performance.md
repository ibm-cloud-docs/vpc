---

copyright:
  years: 2026
lastupdated: "2026-04-29"

keywords: nvidia b300, gpu, performance, clock speed, degraded performance

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my virtual server with NVIDIA B300 GPUs exhibiting degraded performance or running at high speeds for some GPUs?
{: #troubleshoot-b300-gpu-locked-clock}
{: troubleshoot}

In some instances, one or more NVIDIA B300 GPU clock speeds are locked, which can cause degraded performance or higher power usage on affected GPUs.
{: shortdesc}

NVIDIA HGX B300 accelerated virtual server profiles are available for select customers. Create a [support case](/docs/account?topic=account-open-case&interface=ui) if you are interested in purchasing and using this offering.
{: preview}

When you run the `nvidia-smi` command to check GPU status, you see that some GPUs are running at significantly higher clock speeds and power usage than others, even when no workload is running.
{: tsSymptoms}

Run the following command to check GPU status:

```sh
nvidia-smi --query-gpu=index,pstate,power.draw,clocks.current.graphics,clocks.current.sm,temperature.gpu --format=csv index, pstate, power.draw [W], clocks.current.graphics [MHz], clocks.current.sm [MHz], temperature.gpu
```
{: pre}

The following example shows the output without a workload running. GPU 6 and GPU 7 are running at higher speeds (2017 MHz) and higher power usage (187-188 W) compared to the other GPUs (120 MHz and 133-138 W):

```text
0, P0, 134.45 W, 120 MHz, 120 MHz, 37
1, P0, 136.28 W, 120 MHz, 120 MHz, 37
2, P0, 135.65 W, 120 MHz, 120 MHz, 31
3, P0, 133.02 W, 120 MHz, 120 MHz, 31
4, P0, 136.44 W, 120 MHz, 120 MHz, 39
5, P0, 138.55 W, 120 MHz, 120 MHz, 39
6, P0, 187.59 W, 2017 MHz, 2017 MHz, 34
7, P0, 188.10 W, 2017 MHz, 2017 MHz, 34
```
{: screen}

This issue is due to an NVIDIA bug for the B300 GPU. The issue can occur when the GPU goes into an idle state and the clock speed becomes locked.
{: tsCauses}

To unlock the GPU clocks and restore normal performance, reset the GPUs by using the `nvidia-smi` command.
{: tsResolve}

1. Reset the GPUs by running the following command:

   ```sh
   nvidia-smi -r
   ```
   {: pre}

2. Verify that all GPUs are now running at similar low power and low clock speeds by running the following command:

   ```sh
   nvidia-smi --query-gpu=index,pstate,power.draw,clocks.current.graphics,clocks.current.sm,temperature.gpu --format=csv index, pstate, power.draw [W], clocks.current.graphics [MHz], clocks.current.sm [MHz], temperature.gpu
   ```
   {: pre}

   After the reset, all GPUs should show similar clock speeds and power usage levels.
