---

copyright:
  years: 2021,2022

lastupdated: "2022-10-11"

keywords: troubleshooting LinuxONE bare metal servers, hardware issues, troubleshoot s390x bare metal server

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting s390x bare metal servers
{: #troubleshoot_s390x_bare_metal}

s390x Bare Metal Servers for VPC is available for customers with special approval to preview this service in the Washington DC (us-east) and São Paulo (br-sao) regions. 
{: preview}

The following sections cover common difficulties that you might encounter, and offers some helpful tips.

## Bare metal servers have hardware issues
{: #hardware_issues}

When hardware issues occur, you can request support from IBM by creating a support case.

Go to the [IBM Cloud&reg; Support Center](https://cloud.ibm.com/unifiedsupport/cases/form){: external} to create a case. For more information about creating a case, see [Support cases](/docs/vpc?topic=vpc-getting-help#support-tickets).

After the IBM operations team receives the case, the bare metal server is put in maintenance mode.

You need to power off the server before so it can go into the **Maintenance** state.
{: note}

You cannot start a bare metal server that is in the **Maintenance** state. The serial console is also disabled. You can't connect to the system over the network.

During maintenance, IBM also has no access to your network and workloads. The data on the disks isn't examined. But it is recommended that you use software encryption for added security.

You can delete the bare metal server that is in the **Maintenance** state.
{: note}

When the issues are fixed, the state returns to **Stopped**.
