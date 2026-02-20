---

copyright:
  years: 2023, 2026
lastupdated: "2026-02-20"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning a reservation for VPC
{: #provisioning-reserved-capacity-vpc}

You can provision a reservation through the UI, CLI, or API.
{: shortdesc}

## Before you begin
{: #before-you-begin-provisioning-reserved-vpc}

Make sure that your account has the required user permissions. Your account must be assigned the `Administrator` or `Editor` role for the VPC Infrastructure Services service and Reservations for VPC resource type. You need one of these user permissions to create a reservation. Extra roles are available.

For more information about managing IAM access, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).

When you create a reservation, the reservation resource is created, but it isn't provisioned or usable. The state of the reservation is `inactive`. To provision the reservation, you need to active it. When active, the state of the reservation is `active`.

## Provisioning a reservation with the UI
{: #provisioning-reserved-capacity-ui-vpc}
{: ui}

In the {{site.data.keyword.cloud_notm}}, complete the following steps to provision your reservation.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![the menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Reservations**.
2. To create a new reservation, click **Create**.
3. From the **New reservation for VPC** page, enter or select the following information:

   | Field                   | Value               |
   | ----------------------- | ------------------- |
   | Geography               | Select the specific location for your workload. Locations are composed of regions. Each region is a separate geographic area. Keep in mind that you can't select individual locations for each virtual server that you provision within this reservation. Your selection is the location for all virtual server instances that you provision within this reservation. |
   | Name                    | Enter a name for your reservation. |
   | Resource group          | Select a resource group. |
   | Tags                    | Enter any applicable tags. |
   | Access management tags  | Enter any applicable access management tags. |
   | Reservation type        | Available for [virtual servers](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc&interface=ui#reserved-virtual-servers-vpc-supported-profiles) and [bare metal servers](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc&interface=ui#reserved-bare-metal-servers-vpc-supported-profiles). |
   | Server quantity         | Enter the number of instances that you want to assign to this reservation (a limit of 200 vCPUs). |
   | Term length             | Choose either a 1 or 3-year term length. |
   | Server profile          | Select a [profile](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc#reserved-virtual-servers-vpc-supported-profiles) for your reservation. Keep the following rules in mind when you reserve capacity.  \n You can't change profiles after your reservation is created.  \n You can't combine different profile sizes.  \n Your reserved virtual servers must all have the same size (all resources must be identical). |
   | Reservation attachment policy | Choose either an _automatic_ or _restricted_ attachment policy. |
   | Advanced options | Toggle **Auto renew** to **On** if you want to continue your reservation after your selected term length completes.|
   {: caption="Reservation UI provisioning selections" caption-side="top"}

4. Click **Create a reservation**.

When a reservation expires, the servers that are attached to a reservation convert to the nondiscounted on-demand rate upon expiration or nonrenewal of the reservation term and any consumption are billed monthly.
{: note}

## Attaching an existing virtual server to a reservation with the UI
{: #attach-virtual-server-ui-vpc}
{: ui}

You can attach existing virtual servers to your reservation if the reservation has a restricted attachment policy. Keep in mind that all existing virtual servers must be the same profile, size, location, and manual attachment policy.

Virtual servers with an automatic attachment policy automatically attach to a reservation that has an automatic attachment policy.
{: note}

Use the following steps to attach an existing virtual server to a reservation.

1. From your virtual server list, click **Attach**.
2. Select the reservation to attach the server to and click **Attach**.

## Attaching an existing bare metal server to a reservation with the UI
{: #attach-bare-metal-server-ui-vpc}
{: ui}

You can attach an existing bare metal server to your reservation. Keep in mind that all existing bare metal servers that are attached to a reservation must be the same profile, size, and location and manual attachment policy.

Bare metal servers with an automatic attachment policy automatically attach to reservations with an automatic attachment policy.
{: note}

Use the following steps to acctach an existing bare metal server to a reservation that has an automatic attachment policy.

1. From your bare metal server list, click **Attach**.
2. Select the reservation to attach the server to and click **Attach**.

## Provisioning a reservation with the CLI
{: #provisioning-reserved-capacity-cli-vpc}
{: cli}

You can provision a reservation by using the command-line interface (CLI). If you want to use user tags or access management tags to manage your resources, see [Working with tags](/docs/account?topic=account-tag&interface=cli).

[Deprecated]{: tag-deprecated} {{site.data.keyword.cloud_notm}} CLI is not supported on LinuxONE (s390x processor architecture). However, you can install the CLI on another supported platform and use it with LinuxONE (s390x processor architecture) virtual server instances.
{: note}

### Before you begin
{: #before-creating-reserved-capacity-vpc-cli}
{: cli}

* Download, install, and initialize the following CLI plug-ins.
   * {{site.data.keyword.cloud_notm}} CLI
   * The infrastructure-service plug-in

   For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

* Make sure that you [created a VPC](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli).

### Gathering information to create a reservation by using the CLI
{: #gather-info-to-create-reserved-capacity-cli}

Ready to create a reservation with the CLI? Before you can run the `is.reservation-create` command, you need to know the details about the reservation that you want to reserve such as the reservation type and server quantity.

Gather the following information by using the associated commands.

| Reservation details | Listing options |
| ---------------- | ----------------|
| Capacity        | Amount of capacity to reserve (limit of 200 vCPUs). |
| Term length             | (1 or 3-year term length) |
| Server profile          |  Keep the following rules in mind when you provision a reservation.  \n - You can't change profiles after your reservation is created.  \n - You can't combine different profile sizes.  \n - Your reserved virtual servers must all have the same size (all resources must be identical). |
| Zone               | Select the specific location for your workload. Locations are composed of regions; each region is a separate geographic area. Keep in mind that you can't select individual locations for each virtual server that you provision within this reserved capacity. Your selection is the location for all virtual server instances that you provision within this reservation. |
| Name                    | The name for your reservation. |
| Reservation attachment policy | Choose whether you want an _automatic_ or _restricted_ attachment policy. |
{: caption="Required reservation details for the CLI" captin-side="bottom"}

## Provisioning a reservation by using the CLI
{: #provision-reserved-capacity-cli-vpc}
{: cli}

You can create an {{site.data.keyword.vpc_short}} Reservation in your region by using the command-line interface (CLI). To create a reservation by using the CLI, use the `ibmcloud is reservation-create` command.

1. Provision a reservation by using the following command with the associated details.

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
CRN               crn:v1:bluemix:public:is:us-east:a/823bd195e9fd4f0db40ac2e1bffef3e0::reservation:22d3ec3b-d798-454a-ad88-fe608ae86ad6
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

Where the following argument and option values are used:

* --name: New name for the reservation.
* --capacity: The capacity configuration for this reservation.
* --term: Term of the reservation. One of: one_year, three_year.
* --profile: The name of the profile to use for this reservation.
* --profile-resource-type: The resource type of the profile. One of: instance_profile.
* --zone: Name of the zone
* --affinity-policy: The affinity policy for this reservation.
* --expiration-policy: The policy to apply when the committed use term expires. One of: release, renew.
* --resource-group-id: ID of the resource group. This ID is mutually exclusive with --resource-group-name.
* --resource-group-name: Name of the resource group. This name is mutually exclusive with --resource-group-id.
* --output: Specify output format, only JSON is supported. One of: JSON.
* -q, --quiet: Suppress verbose output.

### Next steps
{: #next-step-provisioning-reserved-vpc}

After your reservation is provisioned and active, you can **Attach** or **Create** virtual servers by using the {{site.data.keyword.cloud_notm}} console, the CLI, or the API. For more information about creating a virtual server, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).

### Attaching an existing virtual server to a reservation with the CLI
{: #attach-virtual-server-cli-vpc}
{: cli}

You can attach an existing virtual server to a reservation with restricted affinity policy by using the CLI. To attach a reserved reservation by using the CLI, use the `ibmcloud is instance-update` command.

1. Attach a virtual server to a reservation by using the following command with the associated details.

```sh
ibmcloud is instance-update <instance-name> -reservation-affinity-policy manual --reservation-affinity-pool <reservation-name>
```
{: pre}

### Attaching an existing bare metal server to a reservation with the CLI
{: #attach-bare-metal-server-cli-vpc}
{: cli}

You can attach an existing bare metal server to a reservation with restricted affinity policy by using the CLI. To create a reserved reservation by using the CLI, use the `ibmcloud is bare-metal-server-update` command.

1. Attach a bare metal server to a reservation by using the following command with the associated details.

```sh
ibmcloud is bare-metal-server-update SERVER [--name NEW_NAME] [--enable-secure-boot false | true] [--tpm-mode tpm_2 | disabled] [--bandwidth BANDWIDTH] [--reservation-affinity-policy, --res-policy disabled | manual] [--reservation-affinity-pool, --res-pool RESERVATION_AFFINITY_POOL] [--output JSON] [-q, --quiet]
```
{: pre}

## Creating a reservation with the API
{: #create-reservation-api-vpc}
{: api}

You can create an {{site.data.keyword.vpc_short}} reservation in your region by using the application programming interface (API). To create a reservation by using the API, use [Create a reservation](/apidocs/vpc/latest#create-reservation).

Specify a `Post /reservations` request create a new reservation. See the following example.

```sh
curl -X POST "$vpc_api_endpoint/v1/reservations?version=2024-01-27&generation=2" -H "Authorization: Bearer $iam_token" -d '{
      "capacity": {
        "total": 10
      },
      "committed_use": {
        "expiration_policy": "renew",
        "term": "one_year"
      },
      "name": "my-reservation",
      "profile": {
        "name": "bx2-4x16",
        "resource_type": "instance_profile"
      },
      "zone": {
        "name": "us-south-1"
      }
    }'
```
{: pre}


## Next steps
{: #next-step-provisioning-reserved}

After your reservation is provisioned and active, you can attach an existing server to your reservation. Or, you can create servers in your reservation by using the IBM Cloud console, the CLI, or the API. For more information about creating server, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers) and [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).

## Creating a reservation with Terraform
{: #create-reservation-terraform-vpc}
{: terraform}

You can create a reservation by using Terraform.

### Before you begin
{: #byb-create-reservation-terraform}
{: terraform}

Make sure that you set up [Terraform for VPC](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest){: external}

### Creating a reservation by using Terraform
{: #creating-reservation-by-using-terraform}

Create a reservation by using one of the following examples. For more information, see the Terraform documentation on [ibm_is_reservation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_reservation).

Run the following Terraform commands to create a reservation.

Reservations are regional-specific that are based on the endpoint. By default, the target region is _us-south_. If your VPC is in another region other than _us-south_, make sure that you target the correct region in the provider `provider.tf`.
{: note}

   * Set the target region.

   ```terraform
   provider "ibm" {
     region = "us-south"
   }
   ```
   {: codeblock}

   * Create a reservation.

   ```terraform
   resource "ibm_is_reservation" "example" {
  capacity {
    total = 5
  }
  committed_use {
    term = "one_year"
  }
  profile {
    name          = "ba2-2x8"
    resource_type = "instance_profile"
  }
  zone = "us-east-3"
  name = "reservation-terraform-1"
}
   ```
{: codeblock}
