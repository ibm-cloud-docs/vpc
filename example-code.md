---

copyright:
  years: 2018, 2020
lastupdated: "2020-02-04"

keywords: create, VPC, API, IAM, token, permissions, endpoint, region, zone, profile, status, subnet, gateway, floating IP, delete, resource, provision

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}

# Using the REST APIs to create VPC resources
{: #creating-a-vpc-using-the-rest-apis}

You can create and configure an {{site.data.keyword.vpc_full}} resources using the REST APIs.
{:shortdesc} 

To create and configure your virtual private cloud (VPC) and other attached resources, perform the steps in the sections that follow, in this order:

1. Create a VPC and subnet to define the network.
1. If you want to allow all resources in the subnet to communicate with the public internet, attach a public gateway.
1. Create a virtual server instance. By default, a 100 GB boot volume is attached to the instance.
1. If you want more storage, create a block storage volume and attach it to your instance.
1. To define the inbound and outbound traffic that's allowed for the instance, configure its security group.
1. If you want your instance to be reachable from the internet, reserve and associate a floating IP address.

## Before you begin
{: #before-api-tutorial}

Define variables for the IAM token, API endpoint, and API version. For instructions, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

A good way to learn more about the API is to click **Get sample API call** on the provisioning pages in {{site.data.keyword.cloud_notm}} console. You can view the correct sequence of API requests and better understand actions and their dependencies.
{: tip}

## Create a VPC
{: #create-vpc-api-tutorial}

Create an {{site.data.keyword.vpc_short}} called `my-vpc`.

```bash
curl -X POST "$api_endpoint/v1/vpcs?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

You must send the `generation` parameter with every API request to specify which generation to use. For generation 2 virtual server instances, specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API](https://{DomainName}/apidocs/vpc#api-generation-parameter) 
{: important}

For the rest of the calls, you'll need to know the ID of the newly created VPC. Save the ID in a variable, for example:

```
vpc="0738-59de4046-3434-4d87-bb29-0c99c428c96e"
```
{: pre}

To verify that the variable was saved, run `echo $vpc` and make sure the response is not empty. 

The previous example does not create a VPC with classic access. If you require the VPC to have access to your classic resources, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure). You can only enable a VPC for classic access while creating it. In addition, you can only have one classic access VPC in your account at any time.
{: important}

## Create a subnet
{: #create-subnet-api-tutorial}

Before you create a subnet, select the zone and address prefix in which to create it. To list the address prefixes for each zone in your VPC, run the following command:

```bash
curl -X GET "$api_endpoint/v1/vpcs/$vpc/address_prefixes?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" 
```
{: pre}

Let's pick the default address prefix for the us-south-3 zone. From the response, note the CIDR block of the address prefix. When you create a subnet, you must specify an IP range that's within one of the address prefixes of the selected zone. 

A subnet cannot be resized after it is created. 
{: important}

```bash
curl -X POST "$api_endpoint/v1/subnets?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-subnet",
        "ipv4_cidr_block": "10.0.1.0/24",
        "zone": { "name": "us-south-3" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Save the ID of the subnet in a variable.

```bash
subnet="0738-35fb0489-7105-41b9-99de-033fae723006"
```
{: pre}

To provision resources in your subnet, the subnet must be in the `Ready` status. Query the subnet resource and make sure that the status is `Ready` before you continue. If the status is `failed`, contact [support](/docs/get-support?topic=get-support-getting-customer-support) with the details. You can attempt to continue by trying to provision another subnet.

```bash
curl -X GET "$api_endpoint/v1/subnets/$subnet?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

## Attach a public gateway
{: #attach-public-gateway-api-tutorial}

Attach a public gateway to the subnet if you want to allow all attached resources to communicate with the public internet. 

Create a public gateway for the zone:

```bash
curl -X POST "$api_endpoint/v1/public_gateways?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Save the ID of the public gateway in a variable.

```bash
gateway="0738-ad0cded3-53a3-4d4a-9809-8c59b50d2b80"
```
{: pre}

Attach the public gateway to your subnet.

```bash
curl -X PUT "$api_endpoint/v1/subnets/$subnet/public_gateway?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

Only one public gateway per zone is allowed in a VPC, but that public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the 'ibmcloud is public-gateways` command and look for the particular VPC and Zone values.
{: tip}

## Add an SSH key
{: #add-ssh-key-api-tutorial}

Add your public SSH key to your {{site.data.keyword.cloud_notm}} account. This key is specified when you create the instance, and it's needed later to log in to the instance. You can use one key to provision multiple instances.

```bash
curl -X POST "$api_endpoint/v1/keys?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-key",
        "public_key": "ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQB/nAmOjTmezNUDKYvEeIRf2YnwM9/uUG1d0BYsc8/tRtx+RGi7N2lUbp728MXGwdnL9od4cItzky/zVdLZE2cycOa18xBK9cOWmcKS0A8FYBxEQWJ/q9YVUgZbFKfYGaGQxsER+A0w/fX8ALuk78ktP31K69LcQgxIsl7rNzxsoOQKJ/CIxOGMMxczYTiEoLvQhapFQMs3FL96didKr/QbrfB1WT6s3838SEaXfgZvLef1YB2xmfhbT9OXFE3FXvh2UPBfN+ffE7iiayQf/2XR+8j4N4bW30DiPtOQLGUrH1y5X/rpNZNlWW2+jGIxqZtgWg7lTy3mXy5x836Sj/6L"
      }'
```
{: pre}

Save the ID of the SSH key in a variable, for example:

```bash
key="0738-35fb0489-7105-41b9-8764-033fae723006"
```
{: pre}

## Select a profile and image for your instance
{: #select-profile-and-image}

Call the APIs to list all profiles and images available for your instance, and choose a combination. The following command lists the profiles available.

```
curl -X GET "$api_endpoint/v1/instance/profiles?version=$api_version&generation=2" \
  -H "Authorization:$iam_token"
```
{: pre}

The following command lists the images available.

```
curl -X GET "$api_endpoint/v1/images?version=$api_version&generation=2" \
  -H "Authorization:$iam_token"
```
{: pre}

Save the name of the profile and the ID of the image in variables, which will be used later to provision an instance. For example:

```bash
profile_name="b2-2x8"
image_id="0738-660198a6-52c6-21cd-7b57-e37917cef586"
```
{: pre}


## Create an instance
{: #create-instance-api-tutorial}

Create an instance in the newly created subnet. Pass in your public SSH key so that you can log in after the instance is provisioned.

```bash
curl -X POST "$api_endpoint/v1/instances?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-instance",
        "zone": {
          "name": "us-south-3"
        },
        "vpc": {
          "id": "'$vpc'"
        },
        "primary_network_interface": {
          "subnet": {
            "id": "'$subnet'"
          }
        },
        "keys":[{"id": "'$key'"}],
        "profile": {
          "name": "'$profile_name'"
         },
        "image": {
          "id": "'$image_id'"
         }
        }'
```
{: pre}

Save the ID of the instance in a variable, for example:

```
instance="0738-35fb0489-7105-41b9-99de-033fae723006"
```
{: pre}

The status of the instance is `stopped` when it's first created. Before you can proceed, the instance needs to move to the `running` status, which takes a few minutes. Query the status of the instance and make sure it is `running`.

```bash
curl -X GET "$api_endpoint/v1/instances/$instance?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

Save the ID of the primary network interface of the instance returned in the `GET/instance API` call, for example:  

```bash
network_interface="0738-7710e766-dd6e-41ef-9d36-06f7adbef33d"
```
{: pre}

You can't get the ID of the primary network interface until you query the specific instance.
{: important}

## Create and attach a block storage volume
{: #create-and-attach-storage-api-tutorial}

You can create a block storage volume and attach it to your virtual server instance if you want more storage. Create a block storage volume with a request similar to this example, and specify a volume name, zone, and profile.

To see a list of volume profiles, provide this request:

```bash
curl -X GET "$api_endpoint/v1/volumes/profiles?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
```
{: pre}

Profiles can be general-purpose (3 IOPS/GB), 5iops-tier, 10-iops-tier, and custom. See [About Block Storage for VPC](/docs/vpc?topic=vpc-block-storage-about#capacity-performance)
for information about volume capacity and IOPS ranges based on the volume profile you select.

```
curl -X POST "$api_endpoint/v1/volumes?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-volume",
        "iops": 1000,
        "capacity": 100,
        "zone": {
            "name": "us-south-3"
            },
        "profile": {
            "name": "custom"
            }
      }'
```
{: pre}

Save the ID of the volume in a variable, for example:

```
volume_id="0738-640774d7-2adc-4609-add9-6dfd96167a8f"
```
{: pre}

The status of the volume is pending when it first is created. Before you can proceed, the volume needs to move to available status, which takes a few minutes. To check the status of the volume, run this command:

```bash
curl -X GET "$api_endpoint/v1/volumes/$volume_id?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}

Create a volume attachment to attach the new volume to the virtual server instance. Use the instance ID variable that you created earlier in the request. Use the volume ID, CRN of the volume, or URL to specify the volume in the data parameter. This example uses the variable previously created for the volume ID.

```bash
curl -X POST "$api_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-volume-attachment",
        "volume": {
            "id": "'$volume_id'"
            }
      }'
```
{: pre}

## Add rules to the default security group
{: #add-sg-rule-api-tutorial}

You can configure the security group to define the inbound and outbound traffic that is allowed for the instance. For example, you can add a rule to allow SSH traffic.

Find the security group for the VPC:

```
curl -X GET "$api_endpoint/v1/vpcs/$vpc/default_security_group?version=$api_version&generation=2" \
  -H "Authorization:$iam_token"
```
{: pre}

Save the ID of the security group in a variable, for example:

```
sg=0738-2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Now create a rule to allow inbound SSH traffic so that you'll be able to connect to the instance:

```
curl -X POST "$api_endpoint/v1/security_groups/$sg/rules?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

For Windows images, make sure the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
{: tip}

## Create a floating IP address
{: #create-floating-ip-api-tutorial}

Create a floating IP address if you want your instance to be reachable from the internet. Use the instance's primary network interface as the target for the floating IP address.

```bash
curl -X POST "$api_endpoint/v1/floating_ips?version=$api_version&generation=2" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-floatingip",
        "target": {
            "id":"'$network_interface'"
        }
      }
'
```
{: pre}


Save the ID of the floating IP address in a variable, for example.

```bash
floating_ip="0738-35fb0489-7105-41b9-99de-033fae723006"
```
{: pre}


## Log in to your instance 
{: #log-in-to-instance-api-tutorial}

To connect to the instance, use the floating IP address you created. To get the floating IP address, run the following command:

```
curl -X GET "$api_endpoint/v1/floating_ips/$floating_ip?version=$api_version&generation=2" \
  -H "Authorization:$iam_token"
```
{: pre}

On Linux, use the `address` of the floating IP to connect to the instance with SSH:

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

To connect to a Windows image, log in using its decrypted password. For instructions, see [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
{: tip}

## Monitoring your instance
{: #monitoring-your-instance-api-tutorial} 

You can monitor the CPU, volume, memory, and network usage of your instance over time in the {{site.data.keyword.cloud_notm}} console. Because the monitoring data is stored in {{site.data.keyword.monitoringlong_notm}}, you must be authenticated to an instance of the Monitoring service in your account. For more information, see [Monitoring your instances](/docs/vpc?topic=vpc-monitoring).

## (Optional): Delete the resources
{: #delete-resources-api-tutorial}

You can delete resources, however, a resource can't be deleted if it's required by other resources.
For example, a VPC can't be deleted if it contains instances, subnets, or public gateways. For instructions on deleting a VPC and all its resources, see [Deleting a VPC using the REST APIs](/docs/vpc?topic=vpc-deleting-using-api).

## Congratulations!
{: #congratulations-api-tutorial}

You've successfully created and configured your VPC by using the REST APIs. To try out more API commands, see the [Virtual Private Cloud API](https://{DomainName}/apidocs/vpc).
