---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, RHEL, reregister, entitlement server, capsule server, RHEL registration, subscription manager

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How do I reregister an RHEL virtual server instance??
{: #troubleshooting-reregister-RHEL-VSI}
{: troubleshoot}
{: support}

You receive an error stating that your Red Hat Enterprise Linux virtual server instance is not registered with an entitlement server.
{: shortdesc}

You see the `This system is not registered with an entitlement server.` error message on your Red Hat Enterprise Linux virtual server instance.
{: tsSymptoms}

Your Red Hat Enterprise Linux virtual server instance was unregistered from the capsule server. When you provision an RHEL virtual server or Red Hat OpenShift cluster, the RHEL registration is only successful if the IP addresses for the Red Hat capsule server are included in the Access Control List. If the virtual servers or worker nodes cannot communicate with the capsule servers, the virtual servers enter a failed state. However, you can still SSH into the virtual server to troubleshoot the licensing issue.
{: tsCauses}

Use the following steps:
{: tsResolve}

1. Create an empty file in `/tmp` folder of your server: `touch reregister-ng-rhel-vsi.sh`. Copy and Paste the code in the following section [Script to reregister an RHEL virtual server instance](#script-reregister-RHEL-VSI).
1. Edit the script and add to the file: `vi reregister-ng-rhel-vsi.sh`.
1. Run the script: `chmod +x reregister-ng-rhel-vsi.sh` and `./reregister-ng-rhel-vsi.sh`.
1. If the script fails, provide the following parameters to support:
    *	capsule hostname
    *	organization
    *	activation key
    *	profile name

Example script to reregister a RHEL virtual server instance

```sh
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
# Description: Reregister an RHEL virtual server instance to its respective capsule server
#
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
argumentsFound=false
FILE_DIR=/var/lib/cloud/instance/scripts/vendor
file=$(grep -src -r -w 'REDHAT_CAPSULE_SERVER\|OS_INSTALL_CODE' $FILE_DIR | awk -F: '$2 != 0 {print $1}')
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
{: screen}
