---

copyright:
  years: 2023, 2026
lastupdated: "2026-06-26"

keywords: file share, Mount Helper, install, installation, download, build, update, uninstall

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Installing the Mount Helper utility
{: #fs-mount-helper-install}

Install Mount Helper on your compute host to automate encrypted connections to {{site.data.keyword.filestorage_vpc_short}}. You can download the package from GitHub or build it from source code.
{: shortdesc}

## Before you begin
{: #fs-mount-helper-install-prereqs}

Before you install Mount Helper, review the [requirements and limitations](/docs/vpc?topic=vpc-fs-mount-helper-utility#fs-mount-helper-requirements) to ensure your environment is compatible.

You must connect to your compute host:
- [Connect to your virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui).
- [Connect to your bare metal server](/docs/vpc?topic=vpc-connect-to-ESXi-bare-metal-servers).
- If you want to access the file shares from IBM {{site.data.keyword.powerSys_notm}} instances, you must use a network path through a load balancer. For more information, see the following tutorial: [Accessing File Storage for VPC shares from IBM Power Virtual Server instances](/docs/sap?topic=sap-ha-nlb-rt-rfs-intro).

If you want to mount a regional file share on an IBM Power VSI, download the installation package, and follow the steps of [Installing the Mount Helper to mount regional file shares](#install-MH-for-regional).
{: note}

## Downloading the installation package
{: #download-from-github}

1. Download the Mount Helper package from GitHub.
    ```sh
    curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz
    ```
    {: codeblock}

    To establish an encrypted connection between a bare metal server and a file share, download Mount Helper version 0.2.1.
    {: important}

1. Extract the compressed file.
   ```sh
   tar -xvf mount.ibmshare-latest.tar.gz
   ```
   {: pre}

   The file contains the following items: installation and uninstallation scripts, `rpm` and `deb` packages, root CA certificates, and the configuration file.

   Closed environments: To install Mount Helper on a virtual server instance without internet connection, create or update a local repository on the VSI based on the OS. Copy the Mount Helper package along with its dependencies to the local directory.
   {: note}

## Installing the Mount Helper to mount zonal file shares
{: #install-MH-for-zonal}

1. To install the Mount Helper and all the dependencies, use the following script and specify the region where the file share is going to be mounted.
   ```sh
   ./install.sh region=us-south
   ```
   {: pre}

   The `region` argument is used to copy region-specific root CA cert. If no region is specified, then the utility copies all the root CA certs. The following table shows the values that you can use to specify the region.

   | Location            | New value | Previous Value |
   |---------------------|-------------|-------|
   | Australia - Sydney  | `au-syd`    | `syd` |
   | Brazil - Sao Paulo  | `br-sao`    | `sao` |
   | Canada - Montreal   | `ca-mon`    |       |
   | Canada - Toronto    | `ca-tor`    | `tor` |
   | Germany - Frankfurt | `eu-de`     | `fra` |
   | India - Chennai - Airtel | `in-che` |       |
   | India - Mumbai - Airtel | `in-mum`  |       |
   | Japan - Osaka       | `jp-osa`    | `osa` |
   | Japan - Tokyo       | `jp-tok`    | `tok` |
   | Spain - Madrid      | `eu-es`    | `mad` |
   | United Kingdom - London | `eu-gb` | `lon` |
   | United States - Washington, DC | `us-east`| `wdc` |
   | United States - Dallas, TX | `us-south`  | `dal` |
   {: caption="Region values that the script accepts" caption-side="bottom"}

1. Optional - Every installation image is accompanied by a file that contains the checksum value for the image file. For example, the image file ibmshare-0.0.1.tar.gz is accompanied by the ibmshare-0.0.1.tar.gz.sha256 file that contains the checksum value. To verify the integrity of the downloaded package, use the following commands.
   ```sh
   curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz.sha256
   ```
   {: pre}

   ```sh
   sha256sum -c mount.ibmshare-latest.tar.gz.sha256
   ```
   {: pre}

   A successful response shows "OK". The output looks like the following example.
   ```text
   # sha256sum -c mount.ibmshare-latest.tar.gz.sha256
   ./mount.ibmshare-latest.tar.gz: OK
   ```
   {: screen}

1. Optional - By default, a certificate lasts 1 hour, and new certificates are fetched in every 45 minutes. However, you can modify the `certificate_duration_seconds` option in the configuration file `/etc/ibmcloud/share.conf` to a different time interval. The new value must be between 5 minutes and 1 hour, and expressed in seconds.
   ```sh
   certificate_duration_seconds = 600
   ```
   {: pre}

   The valid range for `certificate_duration_seconds` value is 300 - 3600 seconds. The certificates are renewed when the current certs reach 70% of their lifetime.
   {: note}

1. Optional - If you want to renew the certs immediately with the new expiration time, then run the following command.
   ```sh
   /sbin/mount.ibmshare -RENEW_CERTIFICATE_NOW
   ```
   {: pre}

## Installing the Mount Helper to mount regional file shares
{: #install-MH-for-regional}

### Standard installation with stunnel
{: #install-MH-regional-standard}

1. To install the Mount Helper and all the dependencies, use the following script and specify the `--stunnel` option.
   ```sh
   ./install.sh --stunnel
   ```
   {: pre}

1. Optional - Every installation image is accompanied by a file that contains the checksum value for the image file. For example, the image file ibmshare-0.0.1.tar.gz is accompanied by the ibmshare-0.0.1.tar.gz.sha256 file that contains the checksum value. To verify the integrity of the downloaded package, use the following commands.
   ```sh
   curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz.sha256
   ```
   {: pre}

   ```sh
   sha256sum -c mount.ibmshare-latest.tar.gz.sha256
   ```
   {: pre}

   A successful response shows "OK". The output looks like the following example.
   ```text
   # sha256sum -c mount.ibmshare-latest.tar.gz.sha256
   ./mount.ibmshare-latest.tar.gz: OK
   ```
   {: screen}

### Installing without EPEL and StrongSwan dependencies
{: #install-MH-regional-no-epel}

If you need to mount regional file shares with encryption in transit but want to avoid installing EPEL and StrongSwan packages, you can use the `DO_NOT_INSTALL_IPSEC` environment variable. This approach installs only the stunnel components needed for regional file share mounting.

1. Install the NFS utilities package.
   ```sh
   dnf install nfs-utils
   ```
   {: pre}

1. Download the Mount Helper package from GitHub.
   ```sh
   curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz
   ```
   {: codeblock}

1. Extract the compressed file.
   ```sh
   tar -xvf mount.ibmshare-latest.tar.gz
   ```
   {: pre}

1. Set the environment variable to skip IPsec installation.
   ```sh
   export DO_NOT_INSTALL_IPSEC=true
   ```
   {: pre}

1. Install the Mount Helper with the stunnel option.
   ```sh
   ./install.sh --stunnel
   ```
   {: pre}

1. Verify that StrongSwan and EPEL were not installed.
   ```sh
   rpm -q strongswan epel-release && dnf repolist enabled | grep -i epel
   ```
   {: pre}

   A successful response shows that the packages are not installed:
   ```text
   package strongswan is not installed
   package epel-release is not installed
   ```
   {: screen}

After installation, you can mount regional file shares following the procedures in [Mounting a regional file share](/docs/vpc?topic=vpc-fs-mount-helper-mount#fs-eit-mount-share-stunnel). Traffic between the client and the file storage service is encrypted by using stunnel.

## Building the Mount Helper utility from the source code
{: #build-from-source-code}

- On Debian-based instances, run the following commands:
    ```sh
    apt-get update -y
    apt-get install git make python3 -y
    git clone https://github.com/IBM/vpc-file-storage-mount-helper.git
    cd vpc-file-storage-mount-helper
    make build-deb
    ```
    {: codeblock}

- On rpm-based instances, run the following commands:
   ```sh
   yum update -y
   dnf install git make python3 rpm-build -y
   git clone https://github.com/IBM/vpc-file-storage-mount-helper.git
   cd vpc-file-storage-mount-helper
   make build-rpm
   ```
   {: codeblock}

## Updating the Mount Helper
{: #fs-eit-mount-helper-update}

To update the installation package, run the `install.sh` script again.
```sh
./install.sh
```
{: pre}

Use the `--stunnel` option when you want to upgrade the stunnel package, too.

## Uninstalling the Mount Helper
{: #fs-eit-mount-helper-uninstall}

The following command uninstalls the utility.
```sh
./uninstall.sh
```
{: pre}

## Next steps
{: #fs-mount-helper-install-next-steps}

* [Mounting file shares with the Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-mount)
* [Troubleshooting Mount Helper](/docs/vpc?topic=vpc-fs-mount-helper-ts)
