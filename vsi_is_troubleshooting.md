---

copyright:
  years: 2018, 2020
lastupdated: "2019-12-18"

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Troubleshooting virtual server instances for VPC
{: #troubleshooting-your-virtual-servers-for-vpc}

If you experience difficulties with your {{site.data.keyword.vsi_is_full}} instances, review the following possible causes.

## Permissions not set up in IBM Cloud
{: #troubleshooting-permissions-in-ibm-cloud}

Before you can create {{site.data.keyword.vsi_is_short}}, you need the correct permissions to be set up in {{site.data.keyword.cloud_notm}} console. If your permissions are not correct, you can create a server and it shows `Pending` status, which quickly turns into `Failed` status. Make sure that the administrator of the account assigns you the correct [permissions](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources#managing-user-permissions-for-vpc-resources).

## Servers are all in Unknown status
{: #troubleshooting-servers-unknown-status}

Most likely you do not have adequate permissions to view the server status. Make sure that you have the correct [permissions](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources#managing-user-permissions-for-vpc-resources).

The Unknown status also might be caused by an expired IMS token. Run `bx sl init` again and rebuild the `ims_subject` with the new token. Make sure that you are passing in the `X-Subject-Token:$ims_subject` parameter in the request header.

## Error: 409 Conflict when you create an instance action
{: #troubleshooting-conflict-create-instance}

You can't create certain instance actions if the status of your instance is in conflict with another action. For example, if your instance status is `stopped`, and you try to create a `reboot` action, the system returns a 409 error.

| Status      | Action     | Conflict |
| ----------- | ---------- | -------- |
| Running     | start      | yes      |
| Stopped     | Any action other than start  | yes      |
| Not running | pause      | yes      |
| Not running | reboot     | yes      |
| Not paused  | resume     | yes      |
| Paused      | Any action other than resume | yes      |

## Instance not responding to `instance-reboot` request
{: #troubleshooting-instance-not-responding}

If your instance is not responding to an `instance-reboot` request, you can try an `instance-reset` request. The `instance-reboot` request sends an OS-reboot request to the instance, while an `instance-reset` request performs a hard reset of the VSI instance. You can think of the difference as typing "ctrl-alt-delete" on your computer's keyboard versus pressing the reset or power button. Remember that the `instance-reset` request takes longer to complete than the `instance-reboot` request.

## Why can't I add my SSH key?
{: #troubleshooting-cant-add-ssh-key}

If you try to add an SSH key to your account and get an error that the key can't be parsed, ensure there are no line breaks in the string. An SSH key is a continuous string of characters; sometimes line breaks are introduced when the SSH key is copied from a terminal. To avoid this issue, first paste your SSH key into a text editor and remove any line breaks. Then, copy the SSH key from the text editor and paste it into the VPC UI, CLI, or API.

## How do I reregister a RHEL VSI? 
{: #troubleshooting-reregister-RHEL-VSI}

If you see this error message: 
```
This system is not registered with an entitlement server.
```
{:pre}

Your REHL VSI has been unregistered from the capsule server. To resolve this issue, run the reregister-ng-rhel-vsi.sh script to reregister the vsi. 

```
#!/bin/bash
##
## =============================================================================
## IBM Confidential
## © Copyright IBM Corp. 2020
##
## The source code for this program is not published or otherwise divested of
## its trade secrets, irrespective of what has been deposited with the
## U.S. Copyright Office.
## =============================================================================
##
#
# Description: Reregister an RHEL VSI to its respective capsule server
#

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

argumentsFound=false
FILE_DIR=/var/lib/cloud/instance/scripts/vendor
file=$(grep -src -r -w 'REDHAT_CAPSULE_SERVER\|OS_INSTALL_CODE' $FILE_DIR | awk -F: '$2 == 6 {print $1}')
echo "Processing $file..."
if [ -f  "$file" ]; then
    capsule="$(grep "REDHAT_CAPSULE_SERVER=" $file | cut -d\" -f2)"
    organization="$(grep "OS_REDHAT_ORG_NAME=" $file | cut -d\" -f2)"
    activationKey="$(grep "ACTIVATION_KEYS=" $file | cut -d\" -f2)"
    profileName="$(grep "PROFILENAME=" $file | cut -d\" -f2)"
    if [ ! -z "$capsule" ] && [ ! -z "$organization" ] && [ ! -z "activationKey" ] && [ ! -z "profileName" ]; then
        argumentsFound=true
    fi
fi

if [ "$argumentsFound" = false ]; then
    if [ -z "$4" ]; then
        echo Please provide capsule hostname, organization, activation key and profile name
        exit
    fi

    capsule="$(echo $1 | cut -d. -f1).adn.networklayer.com"
    organization=$2
    activationKey=$3
    profileName=$4
fi
echo "Cleaning metadata..."
yum clean all

echo "Unregistering system..."
subscription-manager unregister
subscription-manager clean

echo "Removing any existing katello-ca RPMs..."
rpm -qa | grep katello-ca | xargs rpm -e

echo "Installing consumer RPM..."
rpm -Uvh http://${capsule}/pub/katello-ca-consumer-latest.noarch.rpm

subscription-manager config --server.hostname=${capsule}
subscription-manager config --rhsm.baseurl=https://${capsule}/pulp/repos

if [ -f /etc/rhsm/facts/katello.facts ]; then
    mv /etc/rhsm/facts/katello.facts /etc/rhsm/facts/katello.facts.bak.$(date +%s)
fi
echo '{"network.hostname-override":"'${profileName}'"}' > /etc/rhsm/facts/katello.facts

echo "Registering system..."
subscription-manager register --org="${organization}" --activationkey="${activationKey}" --force
```
{:screen}


To run the script:
1. chmod +x reregister-ng-rhel-vsi.sh 
2. ./reregister-ng-rhel-vsi.sh

If the script fails, provide the following parameters: 
*	capsule hostname
*	organization
*	activation key 
*	profile name
