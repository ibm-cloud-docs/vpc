---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-02"

keywords: vsi, virtual server instance, power, performance

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}

# Changing SMT settings for POWER-based instances
{: #vsi_is_power_perf}

To improve overall efficiency, you might want to change the simultaneous multi-threading (SMT) value for your POWER-based instance. When you create an instance, the default SMT value is set to 2 for the boot images provided.  
{:shortdesc}

The IBM Cloud Virtual Servers for VPC on POWER service is deprecated. As of 02 June 2020, you cannot provision new instances. Any instance that is still provisioned as of 02 September 2020 will be deleted. For more information, see the [End of Service Announcement for Virtual Servers for VPC on POWER](https://www.ibm.com/cloud/blog/announcements/end-of-service-announcement-for-virtual-servers-for-vpc-on-power).
{:deprecated}

## Viewing the current SMT setting
{: #view_smt_setting}

To view the current SMT setting, run the following pp64_cpu command:

```
sudo ppc64_cpu --smt 
```
{:pre} 

```
SMT=2
```
{:screen}

## Changing the SMT setting
{: #change_smt_setting}

To change the SMT value, run the following `ppc64_cpu –smt=<N>` command, where 'N' is the number of virtual CPUs to enable:
 
```
sudo ppc64_cpu --smt=4 
```
{:pre}

 ```
[root@power-centos ~]# ppc64_cpu --smt
 ```
{:pre}

```
SMT=4
 ```
{:screen}  

```
sudo lscpu 
```
{:pre}

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
sudo ppc64_cpu --frequency 
```
{:pre}

```
min:3.723 GHz (cpu 0)
max:3.751 GHz (cpu 3)
avg:3.744 GHz
```
{:screen}

## Considerations
{: #smt_considerations}

* The ppc64_cpu utility is part of the “powerpc_utils”package for Linux distributions. The package should be part of the pre-built boot images available. If for some reason ppc64_cpu is not found, you can install the powerpc_utils package.
* If you run an older version of the powerpc_utils package, the SMT setting does not persist when you restart the instance. An example of an older version is “powerpc-utils-1.3.4-10.el7.ppc64le”. In this case,you must run the “ppc64_cpu –smt=4” command manually after you restart or place it in a boot script. If you are using the latest version of the powerpc_utils package and still see issues with the SMT value persisting across a restart, you can run “smtstate --save” after changing the SMT value.
* The only valid values for the SMT setting are integers 1 - 4.  

### Choosing an SMT setting value

Generally 2 or 4 threads are good choices. Four threads have more parallelization, but with a bit more overhead. Some workloads have their own guidance. For example, for TensorFlow, see the "AC922 with NVIDIA Tesla V100" section in [WML CE tuning recommendations](https://www.ibm.com/support/knowledgecenter/en/SS5SF7_1.6.1/navigation/wmlce_tuning.htm){: external}. The POWER virtual server backend is AC922 with optional NVIDIA Tesla V100 GPUs.

