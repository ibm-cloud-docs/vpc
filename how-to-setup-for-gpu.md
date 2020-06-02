---

copyright:
  years: 2020
lastupdated: "2020-06-02"

keywords: image, stock image, custom image, power, virtual server, GPU, profiles

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:deprecated: .deprecated}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Setting up the GPU driver for a POWER-based instance
{: #setup-gpus}

The IBM Cloud Virtual Servers for VPC on POWER service is deprecated. As of 02 June 2020, you cannot provision new instances. Any instance that is still provisioned as of 02 September 2020 will be deleted. For more information, see the [End of Service Announcement for Virtual Servers for VPC on POWER](https://www.ibm.com/cloud/blog/announcements/end-of-service-announcement-for-virtual-servers-for-vpc-on-power).
{: deprecated}

When you provision IBM Cloud Virtual Servers for VPC on POWER, you have the option of specifying profiles with NVIDIA Tesla V100 GPUs to accelerate your cognitive workloads. Due to a current licensing restriction, the NVIDIA GPU driver does not come preinstalled on the stock images. However, you can easily install the NVIDIA driver on your Ubuntu 18.04 POWER virtual server instance with the following procedure.

Currently, the recommended driver level is the most recent update available on the 418 version stream. Today that is 418.116.0. The minimum recommended level will change in the future, so be sure to keep current as time goes on. Also, check the supported NVIDA driver levels for the GPU software products you intend to use. 
{: important}

The NVIDIA CUDA Toolkit provides sample utilities and other developer tools that some find useful in validating or developing for their GPUs. CUDA includes a version of the NVIDA GPU driver, so if you plan on using CUDA, then see [Installing CUDA](#installing-cuda) to install it first. Then, return here to update the GPU driver if not at the recommended level.
{: important}


## To install or update the NVIDIA GPU driver, follow these steps:
{: #install-gpu-driver}

1. Check whether the driver is already installed.
   ```
   sudo cat /proc/driver/nvidia/version
   ```
   {: pre}

   This command reports the kernel driver version (and gcc compiler level) if it is installed. Continue with the following steps to install it or update it.

2. Make sure that the prerequisites are met.
   Generally, the prerequisites are gcc and related make utilities. The following command example installs a set of utilities together:
   ```
   sudo apt update; sudo apt install build-essential
   ```
   {: pre}
   
   If you update a driver to a new level in the same major release stream, then you must ensure that no processes are using the GPU. To remove any processes on a kernel module, you can use the following command to stop the persistence daemon:

   ```
   sudo systemctl stop nvidia-persistenced
   ```
   {: pre}
   
   Information on the driver releases, including prerequisites, can be found starting on the [NVIDIA Driver Documentation](https://docs.nvidia.com/datacenter/tesla/index.html){: external}. The stock images come with the linux-headers already installed. Also, you can use three driver installation methods: Run file, local package manager, and remote package manager. The same method must always be followed when you install or update the GPU driver or CUDA. It is recommended to follow the steps in the NVIDIA documentation to consistently use the package-manager-with-local-debian method.
   {: tip}

3. Accept the license and download the driver file.
   Go to the NVIDIA driver page to select the proper hardware and software combination for your environment. It is important to use a validated driver with your cloud instance. Use the following link to access the validated level on the 418 release stream for ubuntu 18.04 on Power Architecture:

   [https://www.nvidia.com/Download/driverResults.aspx/155289](https://www.nvidia.com/Download/driverResults.aspx/155289){: external}

   If you select the individual drop-downs, you must specify Product Type 'Tesla', Product Series 'V-Series', Product 'Tesla V100', CUDA Toolkit '10.1'. You might need to select "Show All Operating Systems" to choose the specific Linux distribution (for example, 'Linux POWER LE Ubunutu 18.04'). Otherwise, if you choose 'Linux 64-bit POWER LE', you get a run file instead of a Debian file.
   {: tip}
   
   Click the download link and review the license terms for the driver. Agree to the terms and download the Debian file.
   Alternatively, after you review the NVIDIA provided information, you can implicitly agree to the terms and conditions. Then, download the Debian file from the console session of your virtual server instance if it has public gateway that is configured for internet access:
   
   ```
   sudo wget http://us.download.nvidia.com/tesla/418.116.00/nvidia-driver-local-repo-ubuntu1804-418.116.00_1.0-1_ppc64el.deb
   ```
   {: pre}

4. On your VSI, update package management with the Debian information.
   The following commands assume Ubuntu 18.04 and driver release 418.116.
   ```
   sudo dpkg -i nvidia-driver-local-repo-ubuntu1804-418.116.00_1.0-1_ppc64el.deb
   sudo apt-key add /var/nvidia-driver-local-repo-418.116.00/7fa2af80.pub
   sudo apt-get update -y
   ```
   {: pre}

5. Install the GPU driver.
   ```
   sudo apt-get install nvidia-driver-418
   ```
   {: pre}
   
   The preceding command assumes that the nvidia-driver-local-repo updated with the most recent driver package files for the 418 major release.

6. Restart your instance for the new driver settings to take effect.
   ```
   sudo reboot
   ```
   {: pre}
   
After you restart your instance, run the `nvidia-smi` command to load the kernel modules and report status. The following console output is an example:

```
$ nvidia-smi
Thu Jan 23 14:39:27 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.116.00   Driver Version: 418.116.00   CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla V100-SXM2...  Off  | 00000002:00:01.0 Off |                    0 |
| N/A   34C    P0    53W / 300W |      0MiB / 32480MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  Tesla V100-SXM2...  Off  | 00000002:00:02.0 Off |                    0 |
| N/A   38C    P0    53W / 300W |      0MiB / 32480MiB |      4%      Default |
+-------------------------------+----------------------+----------------------+
...

$ sudo modinfo nvidia | grep ^version
version:        418.116.00
```
{: screen}

The output from the command that reports `CUDA Version: 10.1` does not necessarily mean that the CUDA toolkit is installed. It is reporting only the release stream of CUDA that is compatible with the driver. For more information, see the following section for CUDA-specific information.
{:tip}


## Installing CUDA
{: #installing-cuda}

CUDA is a parallel computing platform and programming model that is developed by NVIDIA. CUDA provides a driver API, a runtime API, and popular programming language extensions to use GPU compute capabilities in your software. It does not come preinstalled on the IBM Cloud stock images for VPC. It might not be required depending on your workload. However, the CUDA toolkit has sample utilities that demonstrate benchmarking and various other utilities to show capabilities and programming examples. If you choose to install CUDA, review the following information.

If the NVIDIA GPU driver is already installed, then you need to install the CUDA level that matches your installed NVIDIA driver version. You can follow step 1 in the previous section to determine whether the NVIDIA driver is installed. Also, the `nvidia-smi` command reports the GPU driver, if installed, and the corresponding CUDA release supported.

If you install a CUDA release for a different GPU driver release, then the NVIDIA GPU driver can inadvertently update or backlevel to an unsupported release for your cognitive workloads. For example, `CUDA Toolkit 10.1 update2` contains Linux GPU driver level 418.87, not 418.116.
{: important}

To install the CUDA Toolkit, follow these steps.

1. Visit the following URL to see the available CUDA toolkit releases:

   [CUDA Toolkit releases](https://developer.nvidia.com/cuda-toolkit-archive){: external}

   A typical scenario on IBM Cloud VPC would be to install CUDA 10.1 update2, and then update the GPU driver to 418.116 at the end of this sequence (step 6).

2. Select the release level link, then choose Operating System, Architecture, Distribution, Version, and Installer type of `deb (local)`. These selections show you a set of installation instruction steps to follow from the command line under `Base Installer`.

3. Run all of the commands that are provided on the NVIDIA page.

4. Restart your instance so that any new driver modules configure properly.

5. Confirm the installation by running the following to output the CUDA compilation tools release information:

   ```
   export PATH=$PATH:/usr/local/cuda/bin
   $ nvcc -V
   ```
   {: pre}

6. Update the GPU driver if needed by following the [GPU driver installation instructions](#install-gpu-driver). This allows you to update to the most recent driver level that may not yet be packaged inside a CUDA Toolkit update.


## Testing the CUDA Toolkit
{: #testing-cuda}

To try the sample utilities, follow [NVIDIA's instructions](https://docs.nvidia.com/cuda/cuda-samples/index.html#getting-started-with-cuda-samples){: external}.

The following console output shows an example of navigating to the location of one of the toolkit samples, building it, and running it:

```
$ find / -name bandwidthTest 2> /dev/null
/usr/local/cuda-10.1/samples/1_Utilities/bandwidthTest
$ cd /usr/local/cuda-10.1/samples/1_Utilities/bandwidthTest
$ sudo make
$ ./bandwidthTest
[CUDA Bandwidth Test] - Starting...
Running on...

 Device 0: Tesla V100-SXM2-32GB
 Quick Mode
 ...
 Result = PASS
```
{: screen}

