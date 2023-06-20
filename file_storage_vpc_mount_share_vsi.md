---

copyright:
  years: 2021, 2023
lastupdated: "2023-06-20"

keywords: file share, file storage, mount helper, mount target, mount path, secure connection, NFS, API

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Mounting a file share on a virtual server instance with the API tutorial
{: #file-storage-mount-vsi-tutorial-api}

This tutorial describes how to mount a file share with security group access mode to a specific virtual server instance in the VPC.
{: shortdesc}

## Before you begin
{: #fs-mount-vsi-prereqs}

This tutorial requires the following prerequisites:

* An IBM Cloud [VPC](/docs/vpc?topic=vpc-getting-started). 
* An IBM Cloud [SSH key](/docs/vpc?topic=vpc-ssh-keys).
* An IBM Cloud [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers).

You will create these in the tutorial steps. Use the links for more information.

## Objectives
{: #fs-mount-vsi-objective}

* Create a VPC, new security group rule, subnet, SSH key, create and start a virtual server instance.
* Mount a file share on a specific virtual server instance.

## Create a virtual server instance on which to mount the file share
{: #fs-create-vsi}

### Step 1: Create a VPC
{: #fs-tutorial-create-vpc}

Make a `POST /vpcs` call to dreate a VPC. If you have an existing VPC, you can use that one. Copy the VPC ID and the ID for the default security group.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/vpcs?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{ 
    "address_prefix_management": "auto",
    "name": "vsi-test-vpc"
    }'
```
{: pre}

### Step 2: Update security group rules
{: #fs-tutorial-update-sec-group}

Make a `POST /security_groups/{security-group-id}` call and create a new rule.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/security_groups/{security-group-id}/rules?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{ 
    "direction": "inbound",
    "ip_version": "ipv4",
    "protocol": "tcp",
    "port_min": 22,
    "port_max": 22,
    "remote": {
        "cidr_block": "0.0.0.0/0"
        }
    }'
```
{: pre}

### Step 3: Create a subnet
{: #fs-tutorial-create-subnet}

Make a `POST /subnets` call to dreate a subnet. If you have an existing subnet, you can optionally use that one.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/subnets?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{ 
    "zone": {
        "name": "us-south-1"
    },
    "ip_version": "ipv4",
    "name": "vsi-subnet1",
    "ipv4_cidr_block": "10.240.5.0/24",
    "vpc": {
        "id": "{vpc-id}"
        }
    }`
```
{: pre}

### Step 4: Select an image
{: #fs-tutorial-select-image}

Make a `GET /images` call to list available images for the virtual server instance. 

```curl
curl -X GET \
"$vpc_api_endpoint/v1/images?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'
```
{: pre}

### Step 5: Create an SSH key
{: #fs-tutorial-create-ssh-key}

Make a `POST /keys` call to create an SSH key to enable us to ssh into the instance.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/keys?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{ 
    "name": "vsi-test-key",
    "public_key": "{ssh_pubkey}"
    }'
```
{: pre}

### Step 6: Create and start up the instance
{: #fs-tutorial-start-instance}

Make a `POST /instances` call to create the instance. You must create the instance in the same VPC where you created the subnet.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "name": "dev-vsi-1",
    "keys": [
        {
        "id": "{ssh-key-id}"
        }
    ],
    "primary_network_interface": {
        "name": "eth0",
        "subnet": {
        "id": "{subnet-id}"
        }
    },
    "profile": {
        "name": "cx2-2x4"
    },
    "vpc": {
        "id": "{vpc-id}"
    },
    "zone": {
        "name": "us-south-1"
    },
    "image": {
        "id": "{image-id}"
    }
    }'
```
{: pre}

In the response, copy the network interfaces ID. For example, 

```json
"network_interfaces": [
    {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/779847dc-3448-4434-9706-0cb2b8151d91/network_interfaces/7ca88dfb-8962-469d-b1de-1dd56f4c3275",
    "id": "7ca88dfb-8962-469d-b1de-1dd56f4c3275",
    "name": "my-network-interface",
  .
  .
  .
```
{: codeblock}

### Step 7: Create a floating IP
{: #fs-tutorial-create-floating-ip}

Make a `POST /floating_ips` call to create a floating IP.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/floating_ips?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
  "name": "vsi-floating-ip",
  "target": {
    "id": "{vsi-nic-id}"
  }
}'
```
{: pre}

In the response, copy the `address` IP:

```json
  "address": "192.0.2.2",
  "created_at": "2019-01-28T12:08:05Z",
  "crn": "crn:[...]",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/floating_ips/da0cded3-53a3-4d4a-9809-8c59b50d2b80",
  "id": "da0cded3-53a3-4d4a-9809-8c59b50d2b80",
  .
  .
  .
```
{: codeblock}

## Mounting the file share on the instance
{: #fs-mount-share-vsi}

### Step 1: SSH into the virtual server instance
{: #fs-ssh-instance}

With the virtual server instance in a running state, SSH into the instance. 

```sh
ssh root@<floating_ip> -i <path to your ssh key>
```
{: pre}

The user login 'root' is disabled by default in Fedora Core OS. The user login 'core' can be used to log in to Fedora Core OS instances.
{: note}

### Step 2: Update the instance and install NFS
{: #fs-update-instance-nfs}

Update the instance so that the file share can be mounted.

```sh
apt update && apt upgrade -y && apt install nfs-common -y && reboot
```
{: pre}

### Step 3: Create a mount directory for the file share
{: #fs-create-mount-directory}

```sh
mkdir -p /mnt/nfs
```
{: pre}

### Step 4: Mount the file share on the instance
{: #fs-mount-share-instance}

```sh
mount -t nfs4 -o sec=sys,nfsvers=4.1 <mount-target-path> /mnt/nfs
```
{: pre}

Get the mount target path by making a `GET /shares/{share-id}/targets/{target-id}`.

```curl
curl -X GET \
"$vpc_api_endpoint/v1/shares/{share_id}/targets/{target_id}?version=2023-05-06&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'
```
{: pre}

In the response, the `mount_path` looks similar to this example:

```sh
fsf-dal1099b-fz.adn.networklayer.com:/nxg_s_voll_mz7121_2901c23b_f3d7_4a03_a055_0817ad204bc6
```
{: codeblock}

### Step 5: Verify the file share has been mounted
{: #fs-verify-mount}

Run `df -h`. The command returns the mount path. For example:

```sh
fsf-dal1099b-fz.adn.networklayer.com:/nxg_s_voll_mz7121_2901c23b_f3d7_4a03_a055_0817ad204bc6   24G  320K   24G
```
{: codeblock}

## Related content
{: #fs-mount-share-rel-content}

* [Enable encryption in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit).
