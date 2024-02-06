---

copyright:
  years: 2021, 2024
lastupdated: "2024-02-06"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up windows servers for using the instance metadata service
{: #imd-windows-configuration}

To access instance metadata from Windows servers, there are extra requirements to locate a default gateway and add a route.
{: shortdesc}

## Overview
{: #imd-windows-config-overview}

To use the metadata service on Windows, you set up a default route to a link local address for the metadata. To do this, you need to locate the IP address of the default gateway and then add a route to the link local address. After this initial setup, you make calls to access the instance metadata.

The information in this topic is presented as separate steps. More likely, you would set up a `cloudbaseinit` automation process that does all the steps in a single process. Examples presented are to illustrate what you need, but you can use other methods to get the default gateway and add the route.

## Step 1 - Locate the IP of the default gateway
{: #idm-windows-def-gateway}

Running as administrator, locate the IP of the default gateway. A convenient way is to use Powershell `Get-NetRoute` command.

Locate the IP of the default gateway by using the PowerShell Get-NetRoute cmdlet. This command gets the next hop for the default route, also known as the default gateway. For more information, see the Windows Powershell documentation for [Get-NetRoute](https://learn.microsoft.com/en-us/powershell/module/nettcpip/get-netroute?view=windowsserver2019-ps){: external}.

From the Windows terminal, the following example invokes the Powershell `Get-NetRoute` command to get the default IP routes and pass the routes to the SelectObject cmdlet, which then displays the NextHop property for each default route.

```powershell
C:\> powershell "Get-NetRoute -DestinationPrefix "0.0.0.0/0" | Select-Object -ExpandProperty "NextHop""
```
{: codeblock}

The first IP address that is retrieved is the default route. Place the output into a variable.

## Step 2: Add a route to the default gateway
{: #imd-windows-add-route}

The metadata service uses a link local address (169.254.169.254) to [set up access to the service](/docs/vpc?topic=vpc-imd-configure-service) and [retrieve metadata from the instance](/docs/vpc?topic=vpc-imd-get-metadata).

Set up the default route so that the link local address can get to the default gateway. From the windows or Powershell terminal, you would specify:

```powershell
C:> route -p add 169.254.169.254 MASK 255.255.255.255 $DEFAULT_GATEWAY
```
{: codeblock}

A Python automation script might contain code like this:

```python
command = 'route -p add 169.254.169.254 MASK 255.255.255.255 ()'.format(default_gateway)
```
{: codeblock}

These examples use the `route` command, but can also use the Powershell `New-NetRoute` command and pipe the route in a single command. For example, to combine steps 1 and 2 in a single command, you could specify:

```powershell
C:\> powershell "Get-NetRoute -DestinationPrefix "0.0.0.0/0" | Select-Object -ExpandProperty "NextHop" | New-NetRoute"
```
{: codeblock}

To add routes, you must run as an administrator on the Windows server.
{: note}

## Step 3: Programmatically retrieve instance metadata
{: #imd-windows-get-metadata}

After you add a route to the default gateway, you can access instance metadata by using the link local address. Construct your automation script by using the tool of your choice to transfer data over the network, such as `curl`.

To see `curl` commands to invoke the metadata service API and retrieve data, see [Retrieve instance metadata from your running virtual server instance](/docs/vpc?topic=vpc-imd-get-metadata#imd-retrieve-instance-data).

## Next steps
{: #imd-next-steps-windows}

[Use the instance metadata service](/docs/vpc?topic=vpc-imd-get-metadata).
