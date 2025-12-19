---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-18"

keywords: windows serial console, connect to windows console, connect to windows serial console, serial console, connect to serial console


subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to the serial console with Windows
{: #connect-to-serial-console-with-windows}

Serial console isn't enabled by default on Windows&reg;. You need to enable it by using the following script.

```sh
#ps1_sysnative
bcdedit /ems on
bcdedit /emssettings EMSPORT:2 EMSBAUDRATE:115200
Restart-Computer
```
{: codeblock}

The restart command works only if the windows `cloudbase-init` service allows for restarts. If not, then you need to manually restart.
{: important}

For more information about enabling serial console for Windows, see the following links.

* [Boot Parameters to Enable EMS Redirection](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/boot-parameters-to-enable-ems-redirection){: external}
* [Using the Serial Console on Windows IaaS VMs](https://techcommunity.microsoft.com/blog/itopstalkblog/using-the-serial-console-on-windows-iaas-vms/2272295)
