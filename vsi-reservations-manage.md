---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-11"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing a reservation for VPC
{: #managing-reserved-capacity-vpc}

After you provision your reservation, you can manage the reservation with the following actions.
{: shortdesc}

| Action | Description |
| --- | --- |
| List all reservations | List all reservations in the region. |
| Create a reservation | Create a reservation in the region. |
| Delete a reservation | Delete an `inactive` reservation. You can't delete an `active` reservation. This action can't be reversed. |
| Retrieve a reservation | Retrieve a reservation as specified by its identifier in the URL. |
| Update a reservation | Update the reservation with new information. |
| Activate a reservation | Activate an `inactive` reservation. |
{: caption="Actions for managing reservations for virtual servers" caption-side="bottom"}

## Before you begin
{: #before-you-begin-managing-reserved-vpc}

You must provision your reservation before you attach virtual servers. For more information, see [Provisioning reservations for virtual servers](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).

If you are not the account administrator, your user account must include the **Manage reserved capacities** permission. For more information about updating permissions, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).

## Managing a reservation with the UI
{: #managing-reservation-ui-vpc}
{: ui}

You can use the UI to manage your reservation and the serversnthat are attached.

### Attaching a virtual server instance to your reservation with the UI
{: #attach-vsi-reservation-ui-vpc}
{: ui}

You can attach a virtual server to a reservation.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![the menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Reservations**.
1. Select a reservation in the Reservation details page.
1. From the virtual servers list, click **Actions** > **Attach**.
1. Select the server that you want to attach to the reservation and click **Attach**.

### Detaching a server from reservation with the UI
{: #removing-adding-server-reservation-ui-vpc}
{: ui}

You can detach a virtual server from your reservation.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![the menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Reservations**.
1. From your virtual server list or in the Reservation details page, click the server that you want to detach and then click **Actions** > **Detach**.
1. To confirm, click **Detach**.

If the instance is stopped and started, even though pointing to an expired reservation, it is allowed to start and is billed at pay-go prices.
{: note}

### Special considerations for deleting a reservation
{: #special-considerations-deleting-reservation-vpc}

You can delete an `inactive` reservation and you can patch the following fields.

   * affinity_policy
   * capacity.total
   * committed_use.expiration_policy
   * committed_use.term
   * name
   * profile

If you want to change any other fields, you need to delete the reservation and re-create it.

However, after a reservation is activated and is `active`, many aspects of the reservation are permanent for the lifetime of the reservation. Make sure that you review the reservation before you activate it to make sure that the configuration is correct. The only fields that can be patched for an `active` reservation are `name` and `committed_use.expiration_policy`.

### Deleting a reservation with the UI
{: #deleting-reservation-ui-vpc}
{: ui}

You can delete a reservation.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![the menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Reservations**.
1. From your Reservations list, click **Actions** > **Delete reservation**.
1. To confirm, click **Delete**.

## Managing a reservation with the CLI
{: #managing-reservation-cli-vpc}
{: cli}

You can manage a reservation by using the CLI.

### Activate a reservation
{: #activate-reservation-cli-vpc}
{: cli}

You can activate a reservation by using the command-line interface (CLI). To activate a specific reservation by using the CLI, use the `ibmcloud is reservation-activate` command. You must specify the name or ID of the specific reservation by using the `RESERVATION_NAME` for the specific reservation you want to get.

```sh
ibmcloud is reservation-activate
```

See the following example.

```sh
ibmcloud is reservation-activate b4735ce6-f83d-45d1-b41b-4d47451c8396
Activating reservation b4735ce6-f83d-45d1-b41b-4d47451c8396 under account VPCUI-DEMO as user Sreekar.B.V@ibm.com...
Successfully activated reservation.
```
{: screen}

Where the following argument and option values are used

* RESERVATION: ID or name of the reservation.
* --output: Specify output format, only JSON is supported. One of: JSON.
* -q, --quiet: Suppress verbose output

### Getting an instance with reservation
{: #create-reservation-cli-vpc}
{: cli}

You can get instance with reservation in your region by using the command-line interface (CLI). To get a specific instance reservation by using the CLI, use the `ibmcloud is instance` command. You must specify the name or ID of the specific reservation by using the `RESERVATION_NAME` for the specific reservation you want to get. Use the `NEW_NAME` variable in the `--name` option to rename the reservation.

```sh
ibmcloud is instance RESERVATION_NAME
```
{: pre}

See the following example.

```sh
ibmcloud is reservation aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa
Getting reservation aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa under account VPCUI-DEMO as user Sreekar.B.V@ibm.com...

ID                aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa
Name              aaa-default-reservation-2
CRN               crn:v1:bluemix:public:is:jp-tok:a/823bd195e9fd4f0db40ac2e1bffef3e0::reservation:aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa
Status            active
Zone              us-east-1
Lifecycle state   stable
Affinity Policy   open
Resource group    -
Profile Name      mx2-2x16
Created At        2023-06-05T10:41:51.401+05:30
Expiration        At                         Policy   Term
                  2026-08-28T05:11:51.401Z   renew    three_year

Capacity          Allocated   Available   Total   Used   Status
                  8           8           8       0      -
```
{: screen}

### Deleting a reservation
{: #delete-reservation-cli-vpc}
{: cli}

You can delete {{site.data.keyword.vpc_short}} a reservation in your region by using the command-line interface (CLI). To delete a reservation by using the CLI, use the `ibmcloud is reservation-delete` command. You must specify the name or ID of the specific reservation by using the `RESERVATION_NAME` for the specific reservation you want to delete.

```sh
ibmcloud is reservation-delete RESERVATION_NAME
```
{: pre}

See the following example.

```sh
ibmcloud is reservation-delete e9e537f0-59f5-4759-b06e-86bb5cd4068e
This will delete reservation e9e537f0-59f5-4759-b06e-86bb5cd4068e and cannot be undone. Continue [y/N] ?> y
Deleting reservation e9e537f0-59f5-4759-b06e-86bb5cd4068e under account VPCUI-DEMO as user Sreekar.B.V@ibm.com...
OK
Reservation e9e537f0-59f5-4759-b06e-86bb5cd4068e is deleted.
```
{: screen}

Where the following argument and option values are used

* RESERVATION1: ID or name of the reservation.
* RESERVATION2: ID or name of the reservation.
* --force, -f: Force the operation without confirmation
* --output: Specify output format, only JSON is supported. One of: JSON.
* -q, --quiet: Suppress verbose output.

### Listing all reservations
{: #list-all-reservation-cli-vpc}
{: cli}

You can list all {{site.data.keyword.vpc_short}} reservations in your region by using the command-line interface (CLI). To list all reservations by using the CLI, use the `ibmcloud is reservation` command.

```sh
ibmcloud is reservation
```
{: pre}

See the following example.

```sh
ibmcloud is instances
Listing instances in all resource groups and region us-south under account VPCUI-DEV as user Sreekar.B.V@ibm.com...
ID                                          Name               Status    Reserved IP     Floating IP   Profile   Image                             VPC    Zone         Resource group   Reservation Name
0735_73b1e3e5-10a2-44b0-a113-c8d6cd8c42e7   cli-in-test-1      pending   0.0.0.0         -             cx2-2x4   ibm-centos-7-9-minimal-amd64-11   test   us-south-3   Default          reservation-cli-test-1
0735_4e245881-c9c8-49f3-a21b-d890a13835bb   i4                 running   10.240.128.30   -             cx2-2x4   ibm-centos-7-9-minimal-amd64-11   test   us-south-3   Default          reservation-cli-test-3
0735_83748ac9-dffc-4ec0-9914-27c9b7e4a5f3   test-unattached2   running   10.240.128.20   -             cx2-2x4   ibm-centos-7-9-minimal-amd64-11   test   us-south-3   Default          -
```
{: screen}

### Retrieving a reservation
{: #retrieve-reservation-cli-vpc}
{: cli}

You can retrieve a specific {{site.data.keyword.vpc_short}} reservation in your region by using the command-line interface (CLI). To retrieve a specific reservation by using the CLI, use the `ibmcloud is reservation` command. You must specify the name or ID of the specific reservation by using the `RESERVATION_NAME` for the specific reservation you want to retrieve.

```sh
ibmcloud is reservation RESERVATION_NAME
```
{: pre}

See the following example.

```sh
ibmcloud is reservation aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa
Getting reservation aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa under account VPCUI-DEMO as user Sreekar.B.V@ibm.com...

ID                aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa
Name              aaa-default-reservation-2
CRN               crn:v1:bluemix:public:is:jp-tok:a/823bd195e9fd4f0db40ac2e1bffef3e0::reservation:aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa
Status            active
Zone              us-east-1
Lifecycle state   stable
Affinity Policy   open
Resource group    -
Profile Name      mx2-2x16
Created At        2023-06-05T10:41:51.401+05:30
Expiration        At                         Policy   Term
                  2026-08-28T05:11:51.401Z   renew    three_year

Capacity          Allocated   Available   Total   Used   Status
                  8           8           8       0      -
```
{: screen}

### Updating a reservation
{: #update-reservation-cli-vpc}
{: cli}

You can update a specific {{site.data.keyword.vpc_short}} reservation in your region by using the command-line interface (CLI). To update a specific reservation by using the CLI, use the `ibmcloud is reservation-update` command. You must specify the name or ID of the specific reservation by using the `RESERVATION_NAME` for the specific reservation you want to update. Use the `NEW_NAME` variable in the `--name` option to rename the reservation.

```sh
ibmcloud is reservation-update RESERVATION_NAME [--name NEW_NAME]
```
{: pre}

See the following example.

```sh
ibmcloud is reservation-update aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa --name aaa-default-reservation-2-renamed
Updating reservation aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa under account VPCUI-DEMO as user Sreekar.B.V@ibm.com...

ID                aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa
Name              aaa-default-reservation-2-renamed
CRN               crn:v1:bluemix:public:is:us-east:a/823bd195e9fd4f0db40ac2e1bffef3e0::reservation:aaaaaaaa-aaaa-default-reservation2-aaaaaaaaaaaa
Status            active
Zone              us-east-1
Lifecycle state   stable
Affinity Policy   open
Resource group    -
Profile Name      mx2-2x16
Created At        2023-06-14T10:58:17.92+05:30
Expiration        At                         Policy    Term
                  2026-08-28T05:28:17.920Z   release   three_year

Capacity          Allocated   Available   Total   Used   Status
                  5           -           5       -      -
```
{: screen}

Where the following argument and option values are used

* --name: New name for the reservation.
* --capacity: The capacity configuration for this reservation.
* --term: Term of the reservation. One of: one_year, three_year.
* --profile: The name of the profile to use for this reservation.
* --profile-resource-type: The resource type of the profile. One of: instance_profile.
* --expiration-policy: The policy that applies when the committed use term expires. One of: release, renew.
* --output: Specify output format, only JSON is supported. One of: JSON.
* -q, --quiet: Suppress verbose output.

### Listing instance profiles with reservation terms
{: #list-instance-profiles-with-reservation-terms-cli-vpc}

You can list instance profiles with reservation terms in your region by using the command-line interface (CLI). To list instance profiles with reservation terms by using the CLI, use the `ibmcloud is instance-profiles` command.

```sh
ibmcloud is instance-profiles
```
{: pre}

See the following example.

```sh
ibmcloud is instance-profiles
Listing instance profiles in region us-south under account VPCUI-DEV as user Sreekar.B.V@ibm.com...
Name               vCPU Manufacturer   Architecture   Family             vCPUs   Memory(GiB)   Bandwidth(Mbps)   Volume bandwidth(Mbps)   GPUs   Storage(GB)   Min NIC Count   Max NIC Count   Status     Reservation Terms
bx2-2x8            intel               amd64          balanced           2       8             4000              1000                     -      -             1               5               current    -
bx2-2x8-sriov      intel               amd64          balanced           2       8             4000              1000                     -      -             1               5               current    -
bx2a-2x8           amd                 amd64          balanced           2       8             2000              500                      -      -             1               5               current    -
bx2d-2x8           intel               amd64          balanced           2       8             4000              1000                     -      1x75          1               5               previous   one_year,three_year
bx3-2x10           intel               amd64          balanced           2       10            4000              1000                     -      -             1               5               current    -
bx3d-2x10          intel               amd64          balanced           2       10            4000              1000                     -      1x65          1               5               current    one_year,three_year
```
{: screen}

### Listing instances with a reservation
{: #list-instances-with-reservation-cli-vpc}

You can list instances with reservation terms in your region by using the command-line interface (CLI). To list instances with a reservation terms by using the CLI, use the `ibmcloud is instances` command. You must specify the name or ID of the specific instance by using the `INSTANCE_NAME` for the specific instances that you want to list.

```sh
ibmcloud is instances
```
{: pre}

See the following example.

```sh
ibmcloud is instance instance-cli-test-patch-reservation-1
Getting instance instance-cli-test-patch-reservation-1 under account VPCUI-DEV as user Sreekar.B.V@ibm.com...

ID                                    0735_78c1b310-6dc3-45d4-9c01-0335dc526135
Name                                  instance-cli-test-patch-reservation-1
CRN                                   crn:v1:bluemix:public:is:us-south-3:a/efe5afc483594adaa8325e2b4d1290df::instance:0735_78c1b310-6dc3-45d4-9c01-0335dc526135
Status                                running
Availability policy on host failure   restart
Startable                             true
Profile                               cx2-2x4
Architecture                          amd64
vCPU Manufacturer                     intel
vCPUs                                 2
Memory(GiB)                           4
Bandwidth(Mbps)                       4000
Volume bandwidth(Mbps)                1000
Network bandwidth(Mbps)               3000
Lifecycle Reasons                     Code   Message
                                      -      -

Lifecycle State                       stable
Metadata service                      Enabled   Protocol   Response hop limit
                                      false     http       1

Image                                 ID                                          Name
                                      r006-f47cc24c-e020-4db5-ad96-1e5be8b5853b   ibm-centos-7-9-minimal-amd64-11

Numa Count                            1
VPC                                   ID                                          Name
                                      r006-832aa717-816d-4351-8c44-65399dbfe069   test

Zone                                  us-south-3
Resource group                        ID                                 Name
                                      11caaa983d9c4beb82690daab08717e9   Default

Created                               2023-10-18T18:39:48+05:30
Network Interfaces                    Interface   Name      ID                                          Subnet           Subnet ID                                   Floating IP   Security Groups              Allow source IP spoofing   Reserved IP
                                      Primary     primary   0735-a51439ba-9a96-4f17-867c-89cffcd76c31   sn-20231006-01   0735-3edc7506-bece-4ee6-9c5c-916c750dab57   -             dream-grove-spleen-tinwork   false                      10.240.128.18

Boot volume                           ID                                          Name                              Attachment ID                               Attachment name
                                      r006-4ca2ad61-98c8-4182-a095-82cbd5db2f1f   dynastic-thyself-hunter-compile   0735-7cd56e60-025d-4dda-ab17-003457d2df30   sterilize-handlebar-transpire-blender

Reservation Affinity Policy           manual
Reservation Affinity Pool             ID                                          Name                     CRN                                                                                                                             Resource type
                                      0735-4d504e68-b22b-41ed-9a13-6df324301a9f   reservation-cli-test-1   crn:v1:bluemix:public:is:us-south-3:a/efe5afc483594adaa8325e2b4d1290df::reservation:0735-4d504e68-b22b-41ed-9a13-6df324301a9f   reservation

Reservation                           ID                                          Name                     CRN                                                                                                                             Resource type
                                      0735-4d504e68-b22b-41ed-9a13-6df324301a9f   reservation-cli-test-1   crn:v1:bluemix:public:is:us-south-3:a/efe5afc483594adaa8325e2b4d1290df::reservation:0735-4d504e68-b22b-41ed-9a13-6df324301a9f   reservation

Health State                          ok
```
{: screen}

### Getting instance profiles with reservation terms
{: #get-reservation-profile-with-reservation-terms-cli-vpc}

You can get instance profiles with reservation terms in your region by using the command-line interface (CLI). To get instance profiles with reservation terms by using the CLI, use the `ibmcloud is instance-profile` command.

```sh
ibmcloud is instance-profile
```
{: pre}

See the following example.

```sh
ibmcloud is instance-profiles
Listing instance profiles in region us-south under account VPCUI-DEV as user Sreekar.B.V@ibm.com...
Name               vCPU Manufacturer   Architecture   Family             vCPUs   Memory(GiB)   Bandwidth(Mbps)   Volume bandwidth(Mbps)   GPUs   Storage(GB)   Min NIC Count   Max NIC Count   Status     Reservation Terms
bx2-2x8            intel               amd64          balanced           2       8             4000              1000                     -      -             1               5               current    -
bx2-2x8-sriov      intel               amd64          balanced           2       8             4000              1000                     -      -             1               5               current    -
bx2a-2x8           amd                 amd64          balanced           2       8             2000              500                      -      -             1               5               current    -
bx2d-2x8           intel               amd64          balanced           2       8             4000              1000                     -      1x75          1               5               previous   one_year,three_year
bx3-2x10           intel               amd64          balanced           2       10            4000              1000                     -      -             1               5               current    -
bx3d-2x10          intel               amd64          balanced           2       10            4000              1000                     -      1x65          1               5               current    one_year,three_year
```
{: screen}

## Managing reservation with the API
{: #managing-reservation-api-vpc}
{: api}

To manage a reservation by using the application programming interface (API), you need an IAM role that includes the following actions. For more information, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).

   - is.reservation.reservation.list
   - is.reservation.reservation.read
   - is.reservation.reservation.create
   - is.reservation.reservation.delete
   - is.reservation.reservation.read
   - is.reservation.reservation.update

### Listing all reservations
{: #list-reservation-api-vpc}
{: api}

You can list all of the {{site.data.keyword.vpc_short}} reservations in your region by using the application programming interface (API). To list all reservations by using the API, use [List all images](/apidocs/vpc/latest#list-reservations).

Specify a `GET /reservations` request to list all of the reservations. See the following example.

```sh
curl -X GET "$vpc_api_endpoint/v1/reservations?version=2024-01-27&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

The reservations are sorted first by their `create_at` property value. The newest reservations are listed first. If reservations have identical `created_at` property values, they are then sorted by their `name` property values. The list of reservations defaults to 50 reservations, but you can increase this amount up to 100.

You can filter the list further with the following property values.

* `name`
* `resource_group.id`
* `zone.name`

### Deleting a reservation
{: #delete-reservation-api-vpc}
{: api}

You can delete an {{site.data.keyword.vpc_short}} reservation in your region by using the application programming interface (API). To delete a reservation by using the API, use [Delete a reservation](/apidocs/vpc/latest#delete-reservation).

Specify a `DELETE /reservation` request to delete a reservation. See the following example.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/reservations/$id?version=2024-01-27&generation=2" - H "Authorization: Bearer $iam_token"
```
{: pre}

### Retrieving a reservation
{: #retrieve-reservation-api-vpc}
{: api}

You can retrieve a specific {{site.data.keyword.vpc_short}} reservation in your region by using the application programming interface (API). To retrieve a specific reservation by using the API, use [Retrieve a reservation](/apidocs/vpc/latest#get-reservation).

Specify a `GET /reservation/{id}` request to retrieve a specific reservation where `id` is the reservation identifier of the reservation you are retrieving. The identifier is in the URL. See the following example.

```sh
curl -X GET "$vpc_api_endpoint/v1/reservations/$id?version=2024-01-27&generation=2" - H "Authorization: Bearer $iam_token"
```
{: pre}

### Updating a reservation
{: #update-reservation-api-vpc}
{: api}

You update a specific {{site.data.keyword.vpc_short}} reservation in your region by using the application programming interface (API). To update a specific reservation by using the API, use [Update a reservation](/apidocs/vpc/latest#update-reservation).

Specify a `PATCH /reservations/{id}` request to update a specific reservation where `id` is the reservation identifier of the reservation you are retrieving. The identifier is in the URL. See the following example.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/reservations/$id?version=2024-01-27&generation=2" -H "Authorization: Bearer $iam_token" -d '{
      "committed_use": {
        "expiration_policy": "release"
      }
    }'
```
{: pre}

### Activating a reservation
{: #activate-reservation-api-vpc}
{: api}

You can activate a specific {{site.data.keyword.vpc_short}} reservation in your region by using the application programming interface (API). To activate a specific reservation by using the API, use [Activate a reservation](/apidocs/vpc/latest#activate-reservation).

Specify a `POST /reservations/{id}/activate` request to update a specific reservation where `id` is the reservation identifier of the reservation you are retrieving. The identifier is in the URL. See the following example.

```sh
curl -X POST "$vpc_api_endpoint/v1/reservations/$id/activate?version=2024-01-27&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}
