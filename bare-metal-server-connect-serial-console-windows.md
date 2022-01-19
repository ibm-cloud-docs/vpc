---

copyright:
  years: 2022
lastupdated: "2022-01-19"

keywords: windows serial console, connect to windows console, connect to windows serial console, serial console, connect to serial console


subcollection: vpc

---

{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Connecting to the serial console with Windows
{: #connect-to-serial-console-with-windows}


Serial console isn't enabled by default on Windows&reg;. You need to enable it by using the following script. 

```
#ps1_sysnative
bcdedit /ems on
bcdedit /emssettings EMSPORT:2 EMSBAUDRATE:115200
Restart-Computer
```

The restart command works only if the windows cloudbase-init service allows for restarts. If not, then you need to manually restart.
{: important}

For more information about enabling serial console for Windows, see the following links.

* [Boot Parameters to Enable EMS Redirection](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/boot-parameters-to-enable-ems-redirection)
* [Using the Serial Console on Windows IaaS VMs](https://techcommunity.microsoft.com/t5/itops-talk-blog/using-the-serial-console-on-windows-iaas-vms/ba-p/2272295)