---

copyright:
  years: 2019
lastupdated: "2019-12-09"

keywords: vsi, virtual server instance, power, performance

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}

# Changing SMT settings for POWER-based instances
{: #vsi_is_power_perf}

To improve overall efficiency, you might want to change the simultaneous multi-threading (SMT) value for your POWER-based instance. When you create an instance, the default SMT value is set to 2 for the boot images provided.  
{:shortdesc}

## Viewing the current SMT setting
{: #view_smt_setting}

To view the current SMT setting, run the following pp64_cpu command:

```
$ sudo ppc64_cpu --smt 
```
{:codeblock} 

```
SMT=2
```
{:screen}

## Changing the SMT setting
{: #change_smt_setting}

To change the SMT value, run the following `ppc64_cpu –smt=<N>` command, where 'N' is the number of virtual CPUs to enable:
 
```
$ sudo ppc64_cpu --smt=4 
```
{:codeblock}

 ```
[root@power-centos ~]# ppc64_cpu --smt
 ```
 {:codeblock}

```
SMT=4
 ```
{:screen}  

```
$ sudo lscpu 
```
{:codeblock}

```
Architecture: ppc64le
Byte Order: Little Endian
CPU(s): 4O
n-line CPU(s) list: 0-3
Thread(s) per core: 4
```
{:screen}

The instance is dynamically switched to SMT-4 mode. If you run the frequency utility of the ppc64_cpu command, the utility picks two of the vCPUs to report on, at random. Whereas for SMT-2 mode, the utility always returns cpu 0 and 1. The following example shows the frequence utility command and sample output.

```
# sudo ppc64_cpu --frequency 
```
{:codeblock}

```
min:3.723 GHz (cpu 0)
max:3.751 GHz (cpu 3)
avg:3.744 GHz
```
{:screen}

## Considerations
{: #smt_considerations}

* The ppc64_cpu utility is part of the “powerpc_utils”package for Linux distributions. The package should be part of the pre-built boot images available. If for some reason ppc64_cpu is not found, you can install the powerpc_utils package.
* If you run an older version of the powerpc_utils package, the SMT setting does not persist when you restart the instance. An example of an older version is “powerpc-utils-1.3.4-10.el7.ppc64le”. In this case,you must run the “ppc64_cpu –smt=4” command manually after you restart or place it in a boot script. 
* The only valid values for the SMT setting are integers 1 - 4.  

### Choosing an SMT setting value

Generally 2 or 4 threads are good choices. Four threads have more parallelization, but with a bit more overhead. Some workloads have their own guidance. For example, for TensorFlow, see the "AC922 with NVIDIA Tesla V100" section in [WML CE tuning recommendations](https://www.ibm.com/support/knowledgecenter/en/SS5SF7_1.6.1/navigation/wmlce_tuning.htm){: external}. The POWER virtual server backend is AC922 with optional NVIDIA Tesla V100 GPUs.
