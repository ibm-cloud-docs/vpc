---

copyright:
  years: 2025, 2025
lastupdated: "2025-08-29"
keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up windows servers for the metadata service for bare metal servers
{: #metadata-windows-configuration-bare-metal}

To access bare metal server metadata from Windows servers, there are extra requirements to locate a default gateway and add a route.
{: shortdesc}

## Overview
{: #overview-bare-metal-metadata-windows-config}

To use the metadata service on Windows, you set up a default route to a link-local address for the metadata. To do this, you need to locate the IP address of the default gateway and then add a route to the link-local address. After this initial setup, you make calls to access the bare metal server metadata.

The information in this topic is presented as separate steps. More likely, you would set up a `cloudbaseinit` automation process that does all the steps in a single process. Examples presented are to illustrate what you need, but you can use other methods to get the default gateway and add the route.

## Step 1 - Locate the IP of the default gateway
{: #bare-metal-metadata-windows-def-gateway}

Running as administrator, locate the IP of the default gateway. A convenient way is to use Powershell `Get-NetRoute` command.

Locate the IP of the default gateway by using the PowerShell Get-NetRoute cmdlet. This command gets the next hop for the default route, also known as the default gateway. For more information, see the Windows Powershell documentation for [Get-NetRoute](https://learn.microsoft.com/en-us/powershell/module/nettcpip/get-netroute?view=windowsserver2019-ps){: external}.

From the Windows terminal, the following example invokes the Powershell `Get-NetRoute` command to get the default IP routes and pass the routes to the SelectObject cmdlet, which then displays the NextHop property for each default route.

```powershell
C:\> powershell "Get-NetRoute -DestinationPrefix "0.0.0.0/0" | Select-Object -ExpandProperty "NextHop""
```
{: codeblock}

The first IP address that is retrieved is the default route. Place the output into a variable.

## Step 2: Add a route to the default gateway
{: #bare-metal-metadata-windows-add-route}

The metadata service uses a link-local address (169.254.169.254) to [set up access to the service](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal) and [retrieve metadata from the bare metal server](/docs/vpc?topic=vpc-get-metadata-bare-metal).

Set up the default route so that the link-local address can get to the default gateway. From the windows or Powershell terminal, you would specify:

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

## Step 3: Programmatically retrieve bare metal server metadata
{: #windows-get-bare-metal-metadata}

After you add a route to the default gateway, you can access bare metal server metadata by using the link-local address. Construct your automation script by using the tool of your choice to transfer data over the network, such as `curl`.

To see `curl` commands to invoke the metadata service API and retrieve data, see [Retrieve bare metal server metadata from your bare metal server](/docs/vpc?topic=vpc-get-metadata-bare-metal#metadata-json-token-usemd-bare-metal).

## Next steps
{: #metadata-next-steps-windows-bare-metal}

[Use the bare metal server metadata service](/docs/vpc?topic=vpc-get-metadata-bare-metal).
