---

copyright:
  years: 2018, 2024
lastupdated: "2024-01-12"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning a reservation for VPC
{: #provisioning-reservation-vpc}

[Beta]{: tag-blue}

You can provision your reserved capacity through the UI and the CLI. You must provision your reserved capacity before you attach virtual servers.
{: shortdesc}

Reservations are available in only the Sydney region with the following virtual server profiles: bx2-2x8, bx2d-2x8, bx2-4x16, bx2d-4x16.
{: note}

## Before you begin
{: #before-you-begin-provisioning-reserved-vpc}

You can provision your reserved capacity through the {{site.data.keyword.cloud}} catalog. You must provision your reserved capacity before you can attach virtual servers.

If you are not the account administrator, your user account must include the **Manage reserved capacities** permission. For more information about updating permissions, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).

## Provisioning reserved capacity with the UI
{: #provisioning-reserved-capacity-ui-vpc}
{: ui}

In the {{site.data.keyword.cloud_notm}}, complete the following steps to provision your reserved capacity.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![the menu icon ](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Reservations**.
2. To create new reserved capacity, click **Create**.
3. From the **New reservation for VPC** page, enter or select the following information:

   | Field                   | Value               |
   | ----------------------- | ------------------- |
   | Geography               | Select the specific location for your workload. Locations are composed of regions; each region is a separate geographic area. Keep in mind that you can't select individual locations for each virtual server that you provision within this reserved capacity. Your selection is the location for all virtual server instances that you provision within this reserved capacity. |
   | Name                    | Enter a name for your reserved capacity. |
   | Resource group          | Select a resource group. |
   | Tags                    | Enter any applicable tags. |
   | Access management tags  | Enter any applicable access management tags. |
   | Reservation type        | Only available with Virtual Servers for VPC. |
   | Server quantity         | Enter the number of instances that you want to assign to this reserved capacity (limit of 200 vCPUs). |
   | Term length             | Choose either a 1 or 3-year term length. |
   | Server profile          | Select a profile for your reservation. Keep the following rules in mind when you reserve capacity.  /n You can't change profiles after your reservation is created.  /n You can't combine different profile sizes.  /n Your reserved virtual servers must all have the same size (all resources must be identical). |
   | Advanced options | Toggle **Auto renew** to **On** if you want to continue your reservation after your selected term length completes. |
   {: caption="Table 1. Reserved capacity provisioning selections" caption-side="top"}

4. Click **Create reservation**.

## Next steps
{: #next-steps-provisioning-reserved-vpc}

After your reserved capacity is provisioned and available to use, you can **Attach** an existing virtual server to your capacity. Or, you can **Create** virtual servers in your reserved capacity by using the {{site.data.keyword.cloud_notm}} console, the CLI, or the API. For more information about creating a virtual server, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).

## Attaching an existing virtual server to reserved capacity with the UI
{: #attach-virtual-server-ui-vpc}
{: ui}

You can attach existing virtual servers to your reserved capacity. Keep in mind that all existing virtual servers must be the same profile, size, and in the same location.

1. From your virtual server list, click **Attach**.

When a reservation expires, the servers that are attached to the reservation convert to on-demand pricing.
{: note}

## Provisioning reserved capacity with the CLI
{: #provisioning-reserved-capacity-cli-vpc}
{: cli}

You can reserve capacity by using the command-line interface (CLI). If you want to use user tags or access management tags to manage your resources, see [Working with tags](/docs/account?topic=account-tag&interface=cli).

{{site.data.keyword.cloud_notm}} CLI is not supported on LinuxONE (s390x processor architecture). However, you can install the CLI on another supported platform and use it with LinuxONE (s390x processor architecture) virtual server instances.
{: note}

### Before you begin
{: #before-creating-reserved-capacity-vpc-cli}
{: cli}

* Download, install, and initialize the following CLI plug-ins.
   * {{site.data.keyword.cloud_notm}} CLI
   * The infrastructure-service plug-in

   For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

* Make sure that you [created a VPC](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli).

### Gathering information to create reserved capacity by using the CLI
{: #gather-info-to-create-reserved-capacity-cli}

Ready to create reserved capacity? Before you can run the `is.reservation-create` command, you need to know the details about the capacity that you want to reserve such as the reservation type and server quantity.

Gather the following information by using the associated commands.

| Capacity details | Listing options | VPC CLI reference documentation |
| ---------------- | ----------------|---------------------------------|
| Capacity        | Amount of capacity to reserve (limit of 200 vCPUs).  |  |
| Term length             | (1 or 3-year term length) |  |
| Server profile          |  Keep the following rules in mind when you reserve capacity.  \n - You can't change profiles after your reservation is created.  \n - You can't combine different profile sizes.  \n - Your reserved virtual servers must all have the same size (all resources must be identical). |  |
| Zone               | Select the specific location for your workload. Locations are composed of regions; each region is a separate geographic area. Keep in mind that you can't select individual locations for each virtual server that you provision within this reserved capacity. Your selection is the location for all virtual server instances that you provision within this reserved capacity. |  |
| Name                    | The name for your reserved capacity. |  |
{: caption="Table 2. Required reserved capacity details for the CLI" caption-side="bottom"}

## Provisioning reserved capacity by using the CLI
{: #provision-reserved-capacity-cli-vpc}
{: cli}

You can create {{site.data.keyword.vpc_short}} reserved capacity in your region by using the command-line interface (CLI). To create a reserved capacity reservation by using the CLI, use the `ibmcloud is reservation-create` command`.

1. Provision reserved capacity by using the following command with the associated details.

```sh
ibmcloud is reservation-create --capacity NUMBER --term TIME --profile PROFILE --profile-resource-type PROFILE_TYPE --zone ZONE --name NAME
```
{: pre}

See the following example.

```sh
ibmcloud is reservation-create --capacity 10 --term one_year --profile ba2-2x8 --profile-resource-type instance_profile --zone us-east-1 --name reservation-mock-test-1
Creating reservation with name reservation-mock-test-1 under account VPCUI-DEMO as user Sreekar.B.V@ibm.com...

ID                22d3ec3b-d798-454a-ad88-fe608ae86ad6
Name              reservation-mock-test-1
CRN               crn:v1:staging:public:is:us-east:a/823bd195e9fd4f0db40ac2e1bffef3e0::reservation:22d3ec3b-d798-454a-ad88-fe608ae86ad6
Status            active
Zone              us-east-1
Lifecycle state   stable
Affinity Policy   open
Resource group    -
Profile Name      ba2-2x8
Created At        2023-07-21T10:54:46.29+05:30
Expiration        At   Policy   Term
                  -    -        one_year

Capacity          Allocated   Available   Total   Used   Status
                  10          -           10      -      -
```
{: screen}

Where the following argument and option values are used.

* `capacity` : Amount of capacity to reserve
* `term` : Either a 1 or 3-year term
* `profile`: Which profile to use for the reservation
* `profile-resource-type`: Which profile type to use
* `zone`: Which zone to create the reservation in
* `name`: The name of the reservation

### Next steps
{: #next-steps-provisioning-reserved-vpc}

After your reserved capacity is provisioned and available to use, you can **Attach** or **Create** virtual servers by using the {{site.data.keyword.cloud_notm}} console, the CLI, or the API. For more information about creating a virtual server, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).
