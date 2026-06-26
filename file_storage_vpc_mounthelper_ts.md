---

copyright:
  years: 2023, 2026
lastupdated: "2026-06-26"

keywords: file share, Mount Helper, troubleshoot, troubleshooting, IPsec, stunnel, logs

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting Mount Helper
{: #fs-mount-helper-ts}
{: troubleshoot}

Use these troubleshooting tips to resolve common issues with Mount Helper installation and file share mounting. Check logs, verify IPsec connections, and restart services as needed.
{: shortdesc}

## Checking installation logs
{: #fs-mount-helper-ts-install}

If the IPsec installation fails, check the mount-helper installation script logs that are displayed on the standard output.

## Checking IPsec service logs
{: #fs-mount-helper-ts-ipsec-service}

If the IPsec is installed properly but failed to start the IPsec service, check the `charon` logs. The `charon` logs can be found in one of the following log files based on OS image: `/var/log/syslog` or `/var/log/messages`.

## Monitoring IPsec connection negotiation
{: #fs-mount-helper-ts-ipsec-monitor}

You can run the following `swanctl` command on another terminal to check the IPsec connection negotiation logs before you attempt the mount command.

```sh
swanctl -T
```
{: pre}

## Starting or disconnecting IPsec connections manually
{: #fs-mount-helper-ts-ipsec-manual}

To manually start or disconnect the IPsec connection, use one of the following commands.

```sh
ipsec up <connection-name>
ipsec down <connection-name>
```
{: codeblock}

If IPsec command is not available, use the following `swanctl` command.

```sh
swanctl -i --ike <connection-name>
swanctl -i --child <connection-name>
swanctl -t --ike <connection-name>
```
{: codeblock}

## Listing active IPsec connections
{: #fs-mount-helper-ts-ipsec-list}

To list active connections with IPsec encryption, use one of the following commands.

```sh
ipsec status
```
{: pre}

Or

```sh
swanctl -l
```
{: pre}

## Restarting the strongSwan service
{: #fs-mount-helper-ts-strongswan-restart}

Command to restart the strongSwan service.

```sh
systemctl restart strongswan
```
{: pre}

## Checking Mount Helper logs
{: #fs-mount-helper-ts-logs}

Location of the mount helper log.

```sh
/opt/ibm/mount-ibmshare/mount-ibmshare.log
```
{: pre}

## Checking stunnel logs
{: #fs-mount-helper-ts-stunnel-logs}

Location of the stunnel logs:

```sh
/var/log/stunnel/ibmshare_[MOUNT_PATH].log
```
{: pre}

## Next steps
{: #fs-mount-helper-ts-next-steps}

If you continue to experience issues after trying these troubleshooting steps:
- Review the [Mount Helper requirements](/docs/vpc?topic=vpc-fs-mount-helper-about#fs-mount-helper-requirements) to ensure your environment is properly configured
- Check the [Mount Helper installation steps](/docs/vpc?topic=vpc-fs-mount-helper-install) to verify the utility was installed correctly
- Review the [mounting procedures](/docs/vpc?topic=vpc-fs-mount-helper-mount) to ensure you're using the correct commands
