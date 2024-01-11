---

copyright:
  years: 2021, 2024
lastupdated: "2024-01-10"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About reserved IPs
{: #about-reserved-ips}

The reserved IPs capability on VPC allows you to reserve IP addresses for use on your resources. You can specify a particular address or allow the system to select any available address. You can also make a new IP reservation with or without a target with which to bind the address.
{: shortdesc}

## Before you begin
{: #rip-before-you-begin}

Reserved IPs are a sub-resource of subnets. Identity and Access Management (IAM) does not currently have support for sub-resources, so reserved IPs "inherit" permissions from the subnet.

To see the IAM permissions required for reserved IPs, see [Subnets calls](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls#subnets-authorizations-required-for-api-and-cli-calls).

VPC does not support fragmented IP packets. Fragmented packets are dropped at the edge.
{: note}

## Creating reserved IPs in the UI
{: #using-ui}
{: ui}

To create a reserved IP, follow these steps:

1. From the IBM Cloud menu, select **VPC Infrastructure** > **Subnets**.
1. Select an existing subnet, or create a new one.
1. Click the Reserved IPs tab.
1. Click **Create +**.

The following sections describe working with reserved IP addresses with different attributes.

### Creating an unassociated reserved IP
{: #create-reserved-ip-no-vpe}

To create an unassociated reserved IP, follow these steps:

1. Enter a name for your reserved IP. Be sure to use lowercase alphanumeric characters with no spaces for the name.
2. Select whether you want to have the system choose a reserved IP for you automatically, choose from a list of existing reserved IPs, or enter one yourself.
3. Select an address, if you require a specific IP address (optional).
4. Click **Reserve IP**.

### Deleting a reserved IP
{: #ui-delete-reserved-ip}

To delete (release) a reserved IP using the UI, navigate to  **Subnets > Reserved IPs** and click the actions menu ![actions menu](images/overflow.png) next to the reserved IP that you want to delete. Select **Release**.

<!--### Unbinding a reserved IP
{: #unbinding-reserved-ip}

<!--If a reserved IP has the auto-delete option enabled, unbinding the reserved IP deletes the reserved IP with no further action needed.
When the auto-delete option is disabled, unbinding causes the reserved IP to become unbound, but it remains associated with the subnet. You might need to refresh the UI to view the changes.-->

## Working with reserved IPs from the CLI
{: #cli-reserved-ip}
{: cli}

You can use the CLI to create, update, and delete reserved IP addresses.

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

### Creating a reserved IP from the CLI
{: #cli-create-reserved-ip}

To reserve a VPC private IP address using the CLI, run the following command:

```sh
ibmcloud is subnet-reserved-ip-create SUBNET [--vpc VPC] [--name NAME] [--auto-delete true | false] [--target TARGET] [--output JSON] [-q, --quiet]
```
{: pre}

Where:
- **SUBNET** is the ID of the subnet.
- **--vpc** is the ID or name of the VPC. This option is only required if you want to specify the unique resource by name inside this VPC.
- **--name** is the user-defined name for this reserved IP. Names must be unique within the subnet that the reserved IP resides in. Names beginning with `ibm-` are reserved for provider-owned resources.
- **--address** is the IP address to reserve, which must not already be reserved on the subnet. If not specified, an available address on the subnet is automatically selected.
- **--auto-delete** determines how the auto-delete feature is set. If set to `true`, this reserved IP automatically deletes when the target is deleted. One of: `true`, `false` (default: `true`).
- **--target** is the target of the reserved IP.
- **--output** specifies the output format; only JSON is supported.
- **-q, --quiet** suppresses verbose output.

### Updating a reserved IP from the CLI
{: #cli-update-reserved-ip}

To update a reserved IP from the CLI, run the following command:

```sh
ibmcloud is subnet-reserved-ip-update SUBNET RESERVED_IP [--vpc VPC] [--name NEW_NAME] [--auto-delete true | false] [--output JSON] [-q, --quiet]
```
{: pre}

Where:
- **SUBNET** is the ID of the subnet.
- **RESERVED_IP** is the ID of the reserved IP.
- **--vpc** is the ID or name of the VPC. It is only required if you want to specify the unique resource by name inside this VPC.
- **--name** is the new name of the reserved IP.
- **--auto-delete** determines how the auto-delete feature is set. If set to `true`, this reserved IP automatically deletes when the target is deleted. One of: `true`, `false` (default: `true`).
- **--output** specifies the output format; only JSON is supported.
- **-q, --quiet** suppresses verbose output.

### Deleting a reserved IP from the CLI
{: #cli-delete-reserved-ip}

To delete a reserved IP from the CLI, the reserved IP must be unbound. After unbinding the reserved IP, run the following command to delete it:

```sh
ibmcloud is subnet-reserved-ip-delete SUBNET (RESERVED_IP1 RESERVED_IP2 ...) [--vpc VPC] [-f, --force] [--output JSON] [-q, --quiet]
```
{: pre}

Where:
- **SUBNET** is the ID of the subnet.
- **RESERVED_IP1 RESERVED_IP2** is the IDs of the reserved IPs. You can specify one or more.
- **--vpc** is the ID or name of the VPC. It is only required if you want to specify the unique resource by name inside this VPC.
- **--force, -f** forces the operation without confirmation.
- **--output** specifies the output format; only JSON is supported.
- **-q, --quiet** suppresses verbose output.

## Using the API
{: #api-reserved-ip}
{: api}

To create a reserved IP with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. [Create a subnet](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#create-a-subnet-cli).
1. Store the subnet ID in a variable to be used in the API command:

   ```sh
   export SubnetId=<your_subnet_id>
   ```
   {: codeblock}

1.  Create a reserved IP, and optionally specify and address in the `address` property:

   ```sh
   curl -X POST -sH "Authorization:${iam_token}" \
   "$vpc_api_endpoint/v1/subnets/$SubnetId/reserved_ips?version=$api_version&generation=2" \
   -d '{"name": "test-reserved-ip", "address": "10.240.0.15"}' | jq
   ```
   {: codeblock}

### Deleting a reserved IP
{: #api-delete-rip}

Use the following API to delete a reserved IP:

```sh
curl -X DELETE -sH "Authorization:${iam_token}" \
"$vpc_api_endpoint/v1/subnets/$SubnetId/reserved_ips/$ReservedIpId?version=$api_version&generation=2"
```
{: codeblock}

- The API does not allow you to delete a reserved IP that is bound to a primary network interface. A virtual server instance must have a primary network interface, and all network interfaces must have a reserved IP.
- A reserved IP that is bound to a secondary network interface can be bound to a variety of resources, including but not limited to:
    - Network interfaces
    - Virtual Private Endpoints (VPEs)
    - Load balancers
    - VPN gateways
    - IKS workers
    - DNS Services instances
- Unbinding a reserved IP deletes the reserved IP if the auto-delete property is set to `true`.

#### When auto-delete is disabled
{: #autodelete-disabled}

If you create a reserved IP without binding it, auto-delete is disabled by default. A reserved IP that is unbound cannot have auto-delete enabled.

When the auto-delete option is off, unbinding leaves the reserved IP in place, but doesn't delete the reserved IP.

#### When auto-delete is enabled
{: #autodelete-enabled}

If you create a virtual server instance without specifying a reserved IP, the system creates a reserved IP for you and binds it to the primary network interface. In this scenario, the auto-delete feature is enabled by default.

When the auto-delete option is on, unbinding the reserved IP from its target results in the deletion of the reserved IP.

It might take several minutes for the reserved IP to be deleted.
{: note}
