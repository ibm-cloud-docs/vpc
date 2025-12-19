---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-19"

keywords: disable hyper-threading, vsi, virtual server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Disabling Intel Hyper-Threading Technology
{: #disabling-hyper-threading}

If your infrastructure configuration and application setup requires it, you can disable Intel Hyper-Threading Technology (Intel HT Technology) on your guest virtual machine. 
{: shortdesc}

The instructions for disabling Intel Hyper-Threading Technology on guest VMs apply only to virtual servers that are provisioned on hosts that use Intel-based processors.
{: note}

## Understanding reasons to disable Intel Hyper-Threading Technology
{: #understanding-why-to-disable-HT}

Intel Hyper-Threading Technology is a term that describes simultaneous multithreading (SMT). Hyper-Threading Technology splits each physical core into two virtual processors. Hyper-Threading Technology is like taking a wide road with a single lane and making it into two relatively narrower lanes. The two-lane highway provides better service over the single-lane road if there is slow and fast-moving traffic. Hyper-Threading Technology provides better application performance when there is File I/O, Network I/O, and other slower operations mixed with CPU-intensive operations. The performance advantage of Hyper-Threading Technology typically ranges from 0 - 30% over a single-thread mode. Some applications might also see a drop in performance.

Hyper-Threading Technology is not always desirable. In this case, you might prefer to disable it and run the cores in single-threaded mode. Three main reasons exist to consider disabling Hyper-Threading Technology. The first reason is related to software licensing. Many legacy software vendors such as electronic design automation (EDA) tool providers, require a license for each CPU, regardless of whether they use Hyper-Threading Technology or single-threaded mode. When the core is in Hyper-Threading Technology mode, you need twice as many licenses as you do in single-threaded mode, even though the application is not getting two times better performance over single-threaded mode. As a result, EDA and other practitioners disable Hyper-Threading Technology and prefer to run their software in single-threaded mode.

The second reason for disabling Hyper-Threading Technology is to avoid load balancing issues and operating system overhead that comes with Hyper-Threading Technology. High performance computing (HPC) applications typically create as many message passing interface (MPI) tasks as the number of physical cores and let the operating system manage each MPI task on a distinct physical core. When Hyper-Threading Technology is enabled, the operating system sees twice as many CPUs. While the operating system usually does a great job at mapping the MPI tasks to one of the two threads of a core, the OS occasionally might place both tasks on a physical core and keep other cores free. Having both tasks on a physical core with other cores free results in suboptimal system utilization and variable application performance. In addition, the OS kernel allocates data structures for each of the active CPUs and updates them continuously so that more kernel space is allocated in Hyper-Threading Technology mode and management overhead is alleviated. Alleviating operating system overhead by disabling Hyper-Threading Technology helps many HPC applications achieve optimal performance. 

The third reason for disabling Hyper-Threading Technology is security. Some users prefer to disable Hyper-Threading Technology to avoid any side channel attack possibilities that are associated with Hyper-Threading Technology execution. {{site.data.keyword.cloud_notm}} does not share a physical core between two customer's virtual machines so this kind of attack is not possible.

If you determine that disabling Hyper-Threading Technology is right for your configuration, you can use one of the following methods to disable it on your guest virtual machines (VMs) in {{site.data.keyword.cloud_notm}}. 

## Checking Hyper-Threading Technology status and changing it in my guest VM: Basics
{: #checking-ht-status-guest-vm}

When a virtual machine is up and running, you can check the Hyper-Threading Technology status by using the `lscpu` command on Linux operating systems. You see output similar to the following example:

```text
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              8
On-line CPU(s) list: 0-7
Thread(s) per core:  2
Core(s) per socket:  4
...
```
{: screen}

In this example, notice that the Thread(s) per core displays *2*, indicating that Hyper-Threading Technology is enabled. The VM has 4 physical cores and 8 virtual CPUs. Modern versions of the Linux kernel allow users to disable Hyper-Threading Technology on a per core basis. You can run some cores with Hyper-Threading Technology enabled and other cores with Hyper-Threading Technology disabled. To see this on a per core basis, you can use the command `lscpu --extended`. You see output similar to the following example: 

```text
CPU     NODE          SOCKET      CORE             L1d:L1i:L2:L3 ONLINE
0       0             0           0                0:0:0:0       yes
1       0             0           0                1:1:0:0       yes
2       0             0           1                2:2:1:0       yes
3       0             0           1                3:3:1:0       yes
4       0             0           2                4:4:2:0       yes
5       0             0           2                5:5:2:0       yes
6       0             0           3                6:6:3:0       yes
7       0             0           3                7:7:3:0       yes
```
{: screen}

The preceding example output shows that each core has 2 CPUs and both CPUs are online. Core 0 has siblings: *CPU 0* and *CPU 1*. To get the sibling list, you can find the data in `thread_sibling_list` in the devices file system as shown in the following example:

```sh
cat /sys/devices/system/cpu/cpu*/topology/thread_siblings_list
```
{: pre}

```text
0-1
0-1
2-3
2-3
4-5
4-5
6-7
6-7
```
{: screen}

Some kernels and CPU architectures use a comma (`,` ) instead of a hyphen (`-`) in the thread sibling list.
{: note}

You can disable Hyper-Threading Technology for a single core by writing a zero (*0*) into the online field in the device file system for the corresponding CPU. For example, to disable Hyper-Threading Technology on core 2 (CPU 5 in the previous example), run the following command:

```sh
echo 0 > /sys/devices/system/cpu/cpu5/online
```
{: pre}

This command flips CPU 5 offline, disabling Hyper-Threading Technology on core 2 but keeps the other cores in Hyper-Threading Technology mode as shown in the following example when you run the `lscpu --extended` command:

```text
CPU     NODE          SOCKET      CORE             L1d:L1i:L2:L3 ONLINE
0       0             0           0                0:0:0:0       yes
1       0             0           0                1:1:0:0       yes
2       0             0           1                2:2:1:0       yes
3       0             0           1                3:3:1:0       yes
4       0             0           2                4:4:2:0       yes
5       -             -           -                :::           no
6       0             0           3                6:6:3:0       yes
7       0             0           3                7:7:3:0       yes
```
{: screen}

You can repeat the previous process for each core to disable Hyper-Threading Technology for the VM. When the VM has many CPUs, this can be difficult. The following script is a very simple bash script to disable Hyper-Threading Technology on each core:

```sh
#!/bin/bash
     for vcpu in `cat /sys/devices/system/cpu/cpu*/topology/thread_siblings_list | cut -s -d- -f2 | cut -d- -f2 | uniq`; do
          echo 0 > /sys/devices/system/cpu/cpu$vcpu/online
     done
```
{: codeblock}



### Enabling Hyper-Threading Technology for a specific core and associated CPU
{: #enabling-ht-2-cpu}

If you want to enable Hyper-Threading Technology again, you can do so by writing a 1 into the online field for each of the CPUs. 

```sh
echo 1 > /sys/devices/system/cpu/cpu5/online
```
{: pre}

### Enabling Hyper-Threading Technology for all cores in the VM
{: #enabling-ht-all-cores}

Alternatively, you can use the following simple bash script to enable Hyper-Threading Technology for the VM.

```sh
#!/bin/bash
     for vcpu in `lscpu --extended | grep "no" | awk '{print $1}'`; do
          echo 1 > /sys/devices/system/cpu/cpu$vcpu/online
     done
```
{: codeblock}


## Enabling and disabling Hyper-Threading Technology in my guest VM: Script
{: #enabling-ht-with-script}

You can also try this [script](https://github.com/seelam/Manage_Hyperthreading.sh){: external}, created by Wyatt Gorman, for enabling and disabling Hyper-Threading Technology. When you download and run the script, you get these options:

```text
OPTIONS
-d | --disable             Disable Hyper-Threaded vCPUs
-e | --enable              Enable Hyper-Threaded vCPUs
-s | --show                Show Hyper-Threading status
-h | --help                Display this usage output
```
{: screen}

You can get the Hyper-Threading Technology status, disable Hyper-Threading Technology, and enable Hyper-Threading Technology for all the cores in the VM. However, if you need to disable Hyper-Threading Technology on a subset of the cores, follow the steps in the basics section. 

## Enabling and disabling Hyper-Threading Technology in my guest VM at boot time: User data
{: #enabling-ht-user-data}

{{site.data.keyword.cloud_notm}} provides a way to specify optional data when you create a virtual server instance to configure instances or run scripts when the instance is provisioned; this is called User Data. 

During the virtual server instance creation process, you can specify the following sample code in the User Data section of the {{site.data.keyword.cloud_notm}} console:

```sh
#cloud-config
bootcmd:
 - for cpunum in $(cat /sys/devices/system/cpu/cpu*/topology/thread_siblings_list | cut -s -d- -f2- | tr ',' '\n' | sort -un); do echo 0 > /sys/devices/system/cpu/cpu$cpunum/online; done
```
{: codeblock}

The script that you provide in the User Data field disables Hyper-Threading Technology during the instance boot process. You see messages indicating that the CPUs are disabled:

```text
dmesg |grep offline
[    9.488364] smpboot: CPU 1 is now offline
[    9.544370] smpboot: CPU 3 is now offline
[    9.572823] smpboot: CPU 5 is now offline
[    9.600633] smpboot: CPU 7 is now offline
```
{: screen}

You can use a similar approach to inject this process into Terraform.
