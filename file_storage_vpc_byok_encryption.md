---

copyright:
  years: 2022, 2023
lastupdated: "2023-10-20"

keywords: file share, customer-managed encryption, encryption, byok, KMS, Key Protect, Hyper Protect Crypto Services,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating file shares with customer-managed encryption
{: #file-storage-vpc-encryption}

By default, {{site.data.keyword.filestorage_vpc_short}} shares are encrypted with IBM-managed encryption. You can also create an envelop-encryption for your file shares by using one of the supported key management services to create or import your own root keys. You can't change the encryption type after the file share is created.
{: shortdesc}

For more information, see [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

## Before you begin
{: #custom-managed-vol-prereqs-file}

To create file shares with customer-managed encryption, you must first provision a key management service, and create or import your customer root key (CRK). You must also [create a service-to-service authorization](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-volumes-prereqs) between {{site.data.keyword.filestorage_vpc_short}} and the key management service. When you complete these prerequisites, you can start creating file shares that use customer-managed encryption.

For more information, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs).

## Creating file shares with customer-managed encryption in the UI
{: #fs-byok-encryption-ui}
{: ui}

Follow this procedure to specify customer-managed encryption when you create a file share.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure ![VPC icon](../../icons/vpc.svg) > Storage > File Shares**.

1. Click **Create**.

1. Enter the information that is described in the Table 1.

   | Field | Value |
   |-------|-------|
   | Location | Choose the geography, the region, and the zone where you want to create the file share. |
   | Name  | Choose a meaningful name for your file share. The share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name later if you want. |
   | Resource Group | Specify a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups). Resource groups help organize your account resources for access control and billing purposes. |
   | Tags | Tags are used to organize, track, and even manage access to your file share resources. You can tag related resources and view them throughout your account by filtering by tags from your resource list. User tags are visible account-wide. Avoid including sensitive data in the tag name. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | Access-management tags| You can apply flexible access policies on your file shares with access-management tags. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial). |
   | Profile | New file shares are created with the dp2 profile. Select the size and IOPS for your file share. For more information, see [file Storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). |
   | Mount target access mode |  Select one: \n - Security group: Access to the file share is based on security group rules within a subnet. This option can be used to restrict access to specific virtual server instances. \n - Virtual private cloud: Access to the file share is granted to any virtual server instance in the same VPC. |
   {: caption="Table 1. Values for creating a file share and mount target." caption-side="bottom"}

1. The creation of mount targets is optional. You can skip this step if you do not want to create a mount target now. Otherwise, click **Create**. You can create one mount target per VPC per file share. 

   - If you selected security group as the access mode, enter the information as described in the Table 2. This action creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected Security group.

     | Field | Value |
     |-------|-------|
     | **Details** | |
     | Mount target name | Specify a mount target name. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. |
     | Zone | Zone is inherited from the file share (for example, Dallas 2). |
     | VPC | Select an available VPC. The list includes only those VPCs with a subnet in the selected zone. |
     | Subnet | Select a subnet from the list. |
     | **Reserved IP address** | Required for the mount target. The IP address cannot be changed afterward. However, you can delete the mount target and create another one with a different IP address. |
     | Reserving method | You can have the file service select an IP address for you. The reserved IP becomes visible after the mount target is created. Or, specify your own IP. |
     | Auto-release | Releases the IP address when you delete the mount target. Enabled by default. |
     | **Security groups** | The security group for the VPC is selected by default, or select from the list. |
     | **Encryption in transit** | Disabled by default, click the toggle to enable. For more information about this feature, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit). |
     {: caption="Table 2. Values for creating a mount target." caption-side="top"}

   - If you selected VPC as the access mode, provide a name for the mount target and select the VPC where the file share is to be used in.

1. Update the fields in the **Encryption** section.By default, all file shares are encrypted by IBM-managed keys. You can also choose to create an envelop-encryption for your shares with your own keys. If you want to use your own keys, select one of the [key management services](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok).

   | Field | Value |
   | ----- | ----- |
   | Encryption | To use customer-managed encryption, select either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. The key management service (KMS) instance includes the root key that is imported to or created in that KMS instance. |
   | Encryption service instance | If you provisioned multiple KMS instances in your account, select the one that includes the root key that you want to use for customer-managed encryption. |
   | Key name | Select the root key within the KMS instance that you want to use for encrypting the share. |
   | Key ID | The field shows the key ID that is associated with the data encryption key that you selected. |
   {: caption="Table 3. Values for customer-managed encryption for file shares." caption-side="bottom"}

1. When all the required information is entered, click **Create file share**. You return to the {{site.data.keyword.filestorage_vpc_short}} page, where a message indicates that the file share is provisioning. When the transaction completes, the share status changes to **Active**.

If you created your {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance by using a private endpoint, root keys that were created by using that instance are not shown in the UI. You must use the CLI or API to access and use those root keys.
{: important}

## Creating file shares with customer-managed encryption from the CLI
{: #fs-byok-cli}
{: cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Gather the information that you need for provisioning a share, such as a unique name, location, the capacity, and performance characteristics that your file share must have. If you're creating a mount target with a [virtual network interface](/docs/vpc?topic=vpc-vni-about), use the appropriate CLI commands to list the available [subnets](/docs/vpc?topic=vpc-vpc-reference#subnets-list), [reserved IP addresses in a subnet](/docs/vpc?topic=vpc-vpc-reference#subnet-reserved-ips-list), and [security groups](/docs/vpc?topic=vpc-vpc-reference#security-groups-list). For more information, see [Gathering information from the CLI](/docs/vpc?topic=vpc-file-storage-create&interface=cli#fs-vpc-getinfo-cli).

1. For the encryption, retrieve the ID of your key management service and the CRN of the root key in that service. 
   1. List the available KMS instances with the `ibmcloud resource service-instances` command.
      ```sh
      $ ibmcloud resource service-instances
      Retrieving instances with type service_instance in all resource groups in all locations under account Test Account as test.user@ibm.com... 
      OK
      Name             Location   State    Type               Resource Group ID
      KeyProtect-ki    us-south   active   service_instance   db8e8d865a83e0aae03f25a492c5b39e
      schematics       us-south   active   service_instance   db8e8d865a83e0aae03f25a492c5b39e
      ```
      {: screen}

   1. Use the `ibmcloud resource service-instance` command to get the instance ID. The ID is the last string in the CRN after the account number.
      ```sh
      $ ibmcloud resource service-instance KeyProtect-ki -location us-south --id
      Retrieving service instance KeyProtect-ki in all resource groups under account Test Account as test.user@ibm.com...
      crn:v1:bluemix:public:kms:us-south:a/a1234567:: 22e573bd-c02c-4d7f-81e2-2aa867da176d
      ```
      {: screen}

   1. Use the ID in the `ibmcloud kp keys` command to retrieve the key information.
      ```sh
      $ ibmcloud kp keys -c --instance-id 22e573bd-c02c-4d7f-81e2-2aa867da176d
      Targeting endpoint: https://qa.us-south.kms.test.cloud.ibm.com
      Retrieving keys...
      OK
      Key ID                                 Key Name      CRN   
      2fb8d675-bde3-4780-b127-3d0b413631c1   my-file-key   crn:v1:bluemix:public:kms:us-south:a/a1234567:22e573bd-c02c-4d7f-81e2-2aa867da176d:key:2fb8d675-bde3-4780-b127-3d0b413631c1
      ```
      {: screen}

      For more information, see [Step 1 - Obtain service instance and root key information](/docs/vpc?topic=vpc-creating-instances-byok#byok-cli-setup-prereqs).

1. Specify the `ibmcloud is share-create` command with the `--encryption-key` option to create a file share with customer-managed encryption. The `encryption_key` option must be followed by a valid CRN for the root key in the key management service. If you want to enable encryption in transit, too, specify that in the mount target JSON.

   - The following example creates a file share with customer-managed encryption, security group access mode, and a mount target with a virtual network interface. Encryption in transit is not enabled.
      ```sh
      $ ibmcloud is share-create --name my-encrypted-file-share --zone us-south-2 --profile dp2 --size 500 --iops 2000  --user-tags env:dev --encryption_key crn:v1:bluemix:public:kms:us-south:a/a1234567:key:2fb8d675-bde3-4780-b127-3d0b413631c1 --mount-targets '[{"name":"my-new-mount-target","virtual_network_interface": {"name":"my-vni-2","subnet": {"id":"0726-298acd6c-e71e-4204-a04f-fe4a4dd89805"},"security_groups":[{"id":"r134-7f369ca2-ca49-4053-b007-5cab79b9873b"}]}}]'
      Creating file share my-encrypted-file-share under account Test Account as user test.user@ibm.com...
                                
      ID                           r006-d44298fe-aced-4f55-a690-8a3830e9fd90   
      Name                         my-encrypted-file-share   
      CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-d44298fe-aced-4f55-a690-8a3830e9fd90   
      Lifecycle state              pending   
      Access control mode          security_group   
      Zone                         us-south-2   
      Profile                      dp2   
      Size(GB)                     500   
      IOPS                         2000   
      User Tags                    env:dev   
      Encryption                   user_managed   
      Mount Targets                ID                                          Name      
                                   r006-00432317-436e-4940-ab7d-8b26c186b00f   my-new-mount-target      
                                
      Resource group               ID                                 Name      
                                   db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
      Created                      2023-10-19T21:16:27+00:00   
      Encryption key               crn:v1:bluemix:public:kms:us-south:a/a1234567:key:2fb8d675-bde3-4780-b127-3d0b413631c1   
      Replication role             none   
      Replication status           none   
      Replication status reasons   Status code   Status message      
                                   -             -  
      ```
      {: screen}

      ```sh
      $ ibmcloud is share-mount-targets my-encrypted-file-share 
      Listing share mount target of my-encrypted-file-share in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
      ID                                          Name                  VPC      Lifecycle state   Transit Encryption   
      r134-00432317-436e-4940-ab7d-8b26c186b00f   my-new-mount-target   my-vpc   stable            none 
      ```
      {: screen}

   - The following example creates a file share with customer-managed encryption, security group access mode, and a mount target with a virtual network interface, and encryption-in-transit enabled.
      ```sh
      $ ibmcloud is share-create --name my-encrypted-eit-file-share --zone us-south-2 --profile dp2 --size 500 --iops 2000  --user-tags env:dev --encryption_key crn:v1:bluemix:public::kms:us-south:a/a123456:key:2fb8d675-bde3-4780-b127-3d0b413631c1 --mount-targets '[{"name":"my-new-mount-target","transit_encryption": "user_managed","virtual_network_interface": {"name":"my-vni-3","subnet": {"id":"0726-298acd6c-e71e-4204-a04f-fe4a4dd89805"},"security_groups":[{"id":"r134-7f369ca2-ca49-4053-b007-5cab79b9873b"}]}}]'
      Creating file share my-encrypted-eit-file-share under account Test Account as user test.user@ibm.com...
                                
      ID                           r134-f6bf049e-f46c-4160-b548-4a36d27256ac   
      Name                         my-encrypted-eit-file-share   
      CRN                          crn:v1:bluemix:public::is:us-south-2:a/a1234567::share:r134-f6bf049e-f46c-4160-b548-4a36d27256ac   
      Lifecycle state              pending   
      Access control mode          security_group   
      Zone                         us-south-2   
      Profile                      dp2   
      Size(GB)                     500   
      IOPS                         2000   
      User Tags                    env:dev   
      Encryption                   user_managed   
      Mount Targets                ID                                          Name      
                                   r134-e6bd52b8-c656-4ba6-8749-1bb41bfa2c3c   my-new-mount-target      
                                
      Resource group               ID                                 Name      
                                   db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
      Created                      2023-10-20T03:05:38+00:00   
      Encryption key               crn:v1:bluemix:public:kms:us-south:a/a1234567-c02c-4d7f-81e2-2aa867da176d:key:2fb8d675-bde3-4780-b127-3d0b413631c1   
      Replication role             none   
      Replication status           none   
      Replication status reasons   Status code   Status message      
                                   -             -      
      ```
      {: screen}

      ```sh
      $ ibmcloud is share-mount-targets my-encrypted-eit-file-share 
      Listing share mount target of my-encrypted-eit-file-share in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
      ID                                          Name                  VPC      Lifecycle state   Transit Encryption   
      r134-e6bd52b8-c656-4ba6-8749-1bb41bfa2c3c   my-new-mount-target   my-vpc   stable            user_managed  
      ```
      {: screen}

For more information about the command options, see [`ibmcloud is share-create`](/docs/vpc?topic=vpc-vpc-reference#share-create).

## Creating customer-managed encryption file shares with the API
{: #fs-byok-api}
{: api}

You can create file shares with customer-managed encryption by calling the [Virtual Private Cloud (VPC) API](/apidocs/vpc).

Make a `POST /shares` request and specify the `encryption_key` parameter to identify your customer root key (CRK). It is shown in the example as `crn:[...key:...]`.

You can also specify the CRN of a root key from a different account in the `POST /shares` call. For more information, see [About cross-account key access and use](/docs/vpc?topic=vpc-vpc-byok-cross-acct-key&interface=ui#byok-cross-acct-about).
{: note}

You must provide the `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc#api-generation-parameter).
{: requirement}

The following example creates a file share with a mount target, and specifies the CRN of the root key for customer-managed encryption.

   ```sh
   curl -X POST \
   "$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2" \
   -H "Authorization: $iam_token" \
   -d '{
        "encryption_key": {"crn":"crn:[...key:...]"},
        "iops": 1000,
        "name": "my-encrypted-share",
        "profile": {"name": "tier-5iops"},
        "resource_group": {"id": "678523bcbe2b4eada913d32640909956"},
        "size": 100,
        "mount_targets": [
          {
            "name": "my-share-target",
            "subnet": {"id": "7ec86020-1c6e-4889-b3f0-a15f2e50f87e"}
          }
        ],
        "zone": {"name": "us-south-2"}
        }'
   ```
   {: codeblock}

A successful response looks like the following example.

   ```json
   {
     "created_at": "2023-08-08T22:58:49.000Z",
     "crn": "crn:[...]",
     "encryption": "customer_managed",
     "encryption_key": {"crn": "crn:[...key:...]"},
     "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/a0c07083-f411-446c-9316-7b08d6448c86",
     "id": "a0c07083-f411-446c-9316-7b08d6448c86",
     "iops": 1000,
     "lifecycle_state": "pending",
     "name": "my-encrypted-share",
     "profile": {
       "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/tier-5iops",
       "name": "tier-5iops",
       "resource_type": "share_profile"
     },
     "resource_group": {
       "crn": "crn:[...]",
       "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
       "id": "678523bcbe2b4eada913d32640909956",
       "name": "Default"
     },
     "resource_type": "share",
     "size": 100,
     "mount_targets": [
       {
         "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/a0c07083-f411-446c-9316-7b08d6448c86/mount_targets/   1b5571cb-536d-48d0-8452-81c05c6f7b80",
         "id": "1b5571cb-536d-48d0-8452-81c05c6f7b80",
         "name": "my-share-target",
         "resource_type": "share_target",
         "vpc": {
           "crn": "crn:[...]",
           "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/12bb28fc-856d-4902-813b-dc065d1ed084",
           "id": "12bb28fc-856d-4902-813b-dc065d1ed084",
           "name": "my-vpc",
           "resource_type": "vpc"
         }
       }
     ],
     "zone": {
       "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
       "name": "us-south-2"
     }
   }
   ```
   {: screen}

## Next steps
{: #next-step-fs-byok}

- Manage the root keys that are protecting your file share by [rotating](/docs/vpc?topic=vpc-vpc-key-rotation), [disabling](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-disable-root-keys), or [deleting](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-delete-root-keys) keys.

- Use the [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to mount your encrypted file share to an authorized Compute instance.
