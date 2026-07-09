---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, Linux SysRq, serial console, unresponsive, kernel panic, SysRq key, kdump

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How can I use the Linux SysRq key to troubleshoot an unresponsive virtual server instance from the serial console?
{: #linux-vsi-serial-console}
{: troubleshoot}
{: support}

Your Linux virtual server instance is unresponsive and you cannot access it through SSH or direct access.
{: shortdesc}

An unresponsive Linux virtual server instance can occur for various reasons, such as system crashes, deadlock situations, or kernel-related issues. When the virtual server is unresponsive, standard troubleshooting methods such as SSH or direct access might not work. In these cases, the serial console remains accessible, which enables administrators to interact with the server by using the Linux System Request (SysRq) key.
{: tsSymptoms}

System failures, kernel panics, or unresponsive processes can cause a Linux server to become unavailable through regular processes. These issues might arise from software bugs, hardware faults, or misconfigured system parameters. The SysRq key provides low-level access to the Linux kernel and bypasses higher-level processes that are frozen or unresponsive. This mechanism is built into the Linux kernel as a failsafe to help users perform critical system actions, such as rebooting the server or dumping diagnostic information, even when other input methods are unavailable.
{: tsCauses}

To troubleshoot and resolve the issue by using the SysRq key from the serial console, complete the following steps:
{: tsResolve}

1. Access the virtual server instance by connecting to a serial console. For more information, see [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console&interface=ui).

1. Enable SysRq commands. If the SysRq commands are already enabled, you can skip this step. To enable the SysRq commands, set the `kernel.sysrq` parameter.

   ```sh
   echo 1 > /proc/sys/kernel/sysrq
   ```
   {: pre}

1. Send a SysRq command from the serial console. The following example starts a kernel crash dump (kdump).

   To trigger a crash dump, make sure that the `crashkernel` is configured on the Linux virtual server instance. For instructions, see the [Kernel crash dump mechanism](https://ubuntu.com/server/docs/how-to/software/kernel-crash-dump/){: external}.
   {: note}

   1. Press the **Enter** key.
   1. Press the `~` (tilde) key.

      For AZERTY keyboards: Press the **Alt Gr** key + the **2** key to type the `~` key.
      {: note}

   1. Press `B` (uppercase B key).
   1. Press `c` (lowercase c), which starts a kernel crash dump (kdump).

   Note the following when you use SysRq commands:

   - Press and release each key individually. Do not hold any of the keys down.
   - Complete the entire sequence within five seconds, otherwise the SysRq commands are not detected on the serial console.
   - When you type this sequence, the characters do not appear on the serial console. The system treats them as break sequence characters. Normal console typing resumes after the sequence is completed or if the sequence times out or encounters an invalid character.
   - If the sequence times out (takes longer than five seconds) or an invalid break sequence character is pressed, restart the process from step 1.

   Example:

   ```screen
   ENTER + ~ + B + c
   ```
   {: pre}

1. Analyze the diagnostic output. The previous SysRq kernel crash dump generates diagnostic data that you can analyze. By sending relevant SysRq commands, you can collect valuable diagnostic data for troubleshooting and regaining control of the system.

For a list of SysRq command keys, press **Enter** followed by `~Bh` (tilde, uppercase B, lowercase h). For more details, see [Linux Magic System Request Key Hacks - What are the 'command' keys?](https://docs.kernel.org/admin-guide/sysrq.html){: external}
{: note}
