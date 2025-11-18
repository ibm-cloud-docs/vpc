---

copyright:
  years: 2022, 2025
lastupdated: "2025-11-18"

keywords: file share, customer-managed encryption, encryption, byok, KMS, Key Protect, Hyper Protect Crypto Services,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating file shares with customer-managed encryption
{: #file-storage-byok-encryption}

By default, {{site.data.keyword.filestorage_vpc_short}} shares are encrypted with IBM-managed encryption. You can also create an envelop-encryption for your file shares by using one of the supported key management services to create or import your own root keys. You can't change the encryption type after the file share is created.
{: shortdesc}

For more information, see [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

## Before you begin
{: #custom-managed-vol-prereqs-file}

To create file shares with customer-managed encryption, you must have your own customer root key. You can provision a key management service (KMS), and create or import your customer root key (CRK). You can choose between [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-getting-started-tutorial) and [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started). Then, [create a service-to-service authorization](/docs/vpc?topic=vpc-file-s2s-auth) between {{site.data.keyword.filestorage_vpc_short}} and the KMS instance that you created.

It's also possible to use a customer root key from another account. In {{site.data.keyword.cloud_notm}}, the KMS can be either located in the same or in another account as the service that is using an encryption key. This deployment pattern allows enterprises to centrally manage encryption keys for all corporate accounts. For more information, see [Encryption key management](/docs/solution-tutorials?topic=solution-tutorials-resource-sharing#resource-sharing-security-kms).

Configure all required [service-to-service authorizations](/docs/vpc?topic=vpc-file-s2s-auth&interface=ui#file-s2s-auth-encryption-ui){: ui}[service-to-service authorizations](/docs/vpc?topic=vpc-file-s2s-auth&interface=cli#file-s2s-auth-encryption-cli){: cli}[service-to-service authorizations](/docs/vpc?topic=vpc-file-s2s-auth&interface=api#file-s2s-auth-encryption-api){: api}[service-to-service authorizations](/docs/vpc?topic=vpc-file-s2s-auth&interface=terraform#file-s2s-auth-encryption-terraform){: terraform} between File Storage for VPC (source service) and the KMS instance (target service) that holds the customer root key. If you're provisioning volumes with a CRK of another account, ask that account's administrator to set up the authorization and to share the CRN of the root key.
{: requirement}

## Creating file shares with customer-managed encryption in the console
{: #fs-byok-encryption-ui}
{: ui}

Follow this procedure to specify customer-managed encryption when you create a file share.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu icon** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.

1. Click **Create**.

1. Enter the information that is described in the Table 1.

   | Field | Value |
   |-------|-------|
   | **Availability** | If you're a customer with special access to preview the new regional file share offering, you can choose between Regional and Single zone data availability. You can't change this property after the share is created. \n If you're account is not allow-listed, this field does not appear. Select the location for your zonal file share. |
   | **Location** | If you chose Single zone availability, select the geography, region, and zone for the new file share, for example, North America, Dallas (us-south), us-south-2. If you chose regional availability, select the region. For example, Dallas (us-south). |
   | **Details** | |
   | Name  | Choose a meaningful name for your file share. The share name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name later if you want. |
   | Resource Group | Specify a [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups). Resource groups help organize your account resources for access control and billing purposes. |
   | Tags | Tags are used to organize, track, and even manage access to your file share resources. You can tag related resources and view them throughout your account by filtering by tags from your resource list. User tags are visible account-wide. Avoid including sensitive data in the tag name. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | Access-management tags| You can apply flexible access policies on your file shares with access-management tags. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial). |
   | Profile | The profile is auto-populated based on your data availability selection. For more information, see [file Storage profiles](/docs/vpc?topic=vpc-file-storage-profiles). \n - If you chose regional availability, your file share uses the `rfs` profile. Select the size and bandwidth for your file share. You can increase the capacity later, and you can also adjust the bandwidth as needed. \n - If you chose Single zone availability, your file share uses the `dp2` profile. Select the size and IOPS for your file share. You can increase the capacity later, and you can also adjust the IOPS as needed.|
   | Mount target access mode  | Select how you want to manage access to this file share: |
   |  | Security group: Access to the file share is based on [security group](/docs/vpc?topic=vpc-using-security-groups#sg-getting-started) rules. This option can be used to restrict access to specific virtual server instances. You can also use this option if you want to mount the file share to a virtual server instance in another zone. This option is recommended as you have more control over who can access the data that is stored on the file share. When you choose this type of access, you can also specify the allowed transit encryption modes. |
   |  | Virtual private cloud: Access to the file share is granted to any virtual server instance in the same region. Cross-zone mounting and encryption in transit are not supported. This option is only available for zonal file shares. |
   | Allowed transit encryption modes| As the share owner, you can specify how you want clients within your account and authorized accounts to connect to your file share. You can select *none* if you do not want them to use encryption in transit. If you want them to use encryption in transit, select *ipsec* for your zonal files or *stunnel* for your regional files. |
   {: caption="Values for creating a file share and mount target." caption-side="bottom"}

1. The creation of [mount targets](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets) is optional. You can skip this step if you do not want to create a mount target now. However, you need one to mount your file share on a compute host. A file share can have multiple mount targets so you can access it from multiple VPCs. You can create one mount target per VPC per file share. To create it, click **Create**.  

   - If you selected security group as the access mode, enter the information as described in the Table 2. This action creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected Security group. This mount target supports encryption-in-transit and cross-zone mounting.
      1. Provide a mount target name. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want.
      2. Select an available VPC. The list includes only those VPCs with a subnet in the selected location. The location selection is inherited from the file share (for example, us-south-2).
      3. A default virtual network interface is generated. You can customize it by clicking the Edit icon ![Edit icon](/images/edit.png). You can change the name or subnet if you have multiple subnets in the zone.
      4. Click **Next**.
      5. **Encryption in transit** is disabled by default for zonal shares, and it is enabled by default for regional shares. Click the toggle to change the preset value. For more information about this feature, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit). 
      6. Then, click **Next**. 
      7. Review your selection, and either click **Back** to return and update your choices or click **Create**.

   - If you selected VPC as the access mode, provide a name for the mount target and select a VPC from the list. This mount target can be used to mount the file share on any virtual server instance of the selected VPC in the same zone as the file share. Cross-zone mounting is not supported.

1. Update the fields in the **Encryption at rest** section. 
   1. Select the encryption type. By default, all file shares are encrypted by IBM-managed keys. You can also choose to create an envelop-encryption for your shares with your own keys. If you want to use your own keys, select one of the [key management services](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok): {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}.
   1. Specify the key by locating it by the instance or by its CRN.
      - If you chose to locate by instance, choose an instance from the Encryption service instance menu. Then, select the Key name from the list.
      - If you chose to locate by CRN, enter the CRN value. Use this option when you want to use a CRN of a key from another account, as you can't see that key in the instance selector.

1. When all the required information is entered, click **Create file share**. You return to the {{site.data.keyword.filestorage_vpc_short}} page, where a message indicates that the file share is provisioning. When the transaction completes, the share status changes to **Active**.

If you created your {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance by using a private endpoint, root keys that were created by using that instance are not shown in the console. You must use the CLI or API to access and use those root keys.
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
      ibmcloud resource service-instances
      ```
      {: pre}

      ```sh
      Retrieving instances with type service_instance in all resource groups in all locations under account Test Account as test.user@ibm.com... 
      OK
      Name             Location   State    Type               Resource Group ID
      KeyProtect-ki    us-south   active   service_instance   db8e8d865a83e0aae03f25a492c5b39e
      schematics       us-south   active   service_instance   db8e8d865a83e0aae03f25a492c5b39e
      ```
      {: screen}

   1. Use the `ibmcloud resource service-instance` command to get the instance ID. The ID is the last string in the CRN after the account number.
      ```sh
      ibmcloud resource service-instance KeyProtect-ki -location us-south --id
      ```
      {: pre}

      ```sh
      Retrieving service instance KeyProtect-ki in all resource groups under account Test Account as test.user@ibm.com...
      crn:v1:bluemix:public:kms:us-south:a/a1234567:: 22e573bd-c02c-4d7f-81e2-2aa867da176d
      ```
      {: screen}

   1. Use the ID in the `ibmcloud kp keys` command to retrieve the key information.
      ```sh
      ibmcloud kp keys -c --instance-id 22e573bd-c02c-4d7f-81e2-2aa867da176d
      ```
      {: pre}

      ```sh
      Targeting endpoint: https://qa.us-south.kms.test.cloud.ibm.com
      Retrieving keys...
      OK
      Key ID                                 Key Name      CRN   
      2fb8d675-bde3-4780-b127-3d0b413631c1   my-file-key   crn:v1:bluemix:public:kms:us-south:a/a1234567:22e573bd-c02c-4d7f-81e2-2aa867da176d:key:2fb8d675-bde3-4780-b127-3d0b413631c1
      ```
      {: screen}

   If you plan to use the encryption key of another account, the previous steps must be performed on the other account. You can't list the resources of another account even if you are authorized to use them.
   {: note}

1. If you are a customer with special access to preview the regional file share profile, you can use the `rfs` profile to create a file share. To be able to create and manage a regional file share from the CLI, set the appropriate environmental variable with the following command.

    ```sh
    export IBMCLOUD_IS_FEATURE_SHARE_DENALI_REGIONAL_AVAILABILITY=true
    ```
    {: pre}
    
    The CLI returns the properties for "Allowed Access Protocols", "Availability Mode", "Bandwidth", and "Storage Generation" only when this environmental variable is set to "true".

1. Specify the `ibmcloud is share-create` command with the `--encryption-key` option to create a file share with customer-managed encryption. The `encryption_key` option must be followed by a valid CRN for the root key in the key management service. If you want to enable encryption in transit, too, specify that in the mount target JSON. The security groups that you associate with a mount target must allow inbound access for the TCP protocol on the NFS port from all servers where you want to mount the share.

   - The following example creates a file share with customer-managed encryption, security group access mode, and a mount target with a virtual network interface. Encryption in transit is not enabled.

       ```sh
      ibmcloud is share-create --name my-encrypted-file-share --zone us-south-2 --profile dp2 --size 500 --iops 2000  --user-tags env:dev --encryption-key crn:v1:bluemix:public:kms:us-south:a/a1234567:key:2fb8d675-bde3-4780-b127-3d0b413631c1 --mount-targets '[{"name":"my-new-mount-target","virtual_network_interface": {"name":"my-vni-2","subnet": {"id":"r006-298acd6c-e71e-4204-a04f-fe4a4dd89805"},"security_groups":[{"id":"r006-7f369ca2-ca49-4053-b007-5cab79b9873b"}]}}]'
      ```
      {: pre}

      ```json  
      Creating file share my-encrypted-file-share under account Test Account as user test.user@ibm.com...
                      
      ID                           r006-d44298fe-aced-4f55-a690-8a3830e9fd90   
      Name                         my-encrypted-file-share   
      CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-d44298fe-aced-4f55-a690-8a3830e9fd90   
      Lifecycle state              pending   
      Access control mode          security_group  
      Accessor binding role        none  
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
      Snapshot count               10
      Snapshot size                10 
      Source snapshot              -        
      ```
      {: screen}

      ```sh
      $ ibmcloud is share-mount-targets my-encrypted-file-share 
      Listing share mount target of my-encrypted-file-share in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
      ```
      {: pre}

      ```json
      ID                                          Name                  VPC      Lifecycle state   Transit Encryption   
      r006-00432317-436e-4940-ab7d-8b26c186b00f   my-new-mount-target   my-vpc   stable            none 
      ```
      {: screen}

   - The following example creates a file share with customer-managed encryption, security group access mode, and a mount target with a virtual network interface, and encryption-in-transit enabled.

      ```sh
      ibmcloud is share-create --name my-encrypted-eit-file-share --zone us-south-2 --profile dp2 --size 500 --iops 2000  --user-tags env:dev --encryption_key crn:v1:bluemix:public::kms:us-south:a/a1234567:key:2fb8d675-bde3-4780-b127-3d0b413631c1 --mount-targets '[{"name":"my-new-mount-target","transit_encryption": "user_managed","virtual_network_interface": {"name":"my-vni-3","subnet": {"id":"r006-298acd6c-e71e-4204-a04f-fe4a4dd89805"},"security_groups":[{"id":"r006-7f369ca2-ca49-4053-b007-5cab79b9873b"}]}}]'
      ```
      {: pre}

      ```json
      Creating file share my-encrypted-eit-file-share under account Test Account as user test.user@ibm.com...
                                
      ID                           r006-f6bf049e-f46c-4160-b548-4a36d27256ac   
      Name                         my-encrypted-eit-file-share   
      CRN                          crn:v1:bluemix:public::is:us-south-2:a/a1234567::share:r006-f6bf049e-f46c-4160-b548-4a36d27256ac   
      Lifecycle state              pending   
      Access control mode          security_group   
      Accessor binding role        none 
      Zone                         us-south-2   
      Profile                      dp2   
      Size(GB)                     500   
      IOPS                         2000   
      User Tags                    env:dev   
      Encryption                   user_managed   
      Mount Targets                ID                                          Name      
                                   r006-e6bd52b8-c656-4ba6-8749-1bb41bfa2c3c   my-new-mount-target      
                                
      Resource group               ID                                 Name      
                                   db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
      Created                      2023-10-20T03:05:38+00:00   
      Encryption key               crn:v1:bluemix:public:kms:us-south:a/a1234567-c02c-4d7f-81e2-2aa867da176d:key:2fb8d675-bde3-4780-b127-3d0b413631c1   
      Replication role             none   
      Replication status           none   
      Replication status reasons   Status code   Status message      
                                   -             -      

      Snapshot count               0
      Snapshot size                0       
      Source snapshot              -                       
      ```
      {: screen}

      ```sh
      ibmcloud is share-mount-targets my-encrypted-eit-file-share
      ```
      {: pre}

      ```json
      Listing share mount target of my-encrypted-eit-file-share in all resource groups and region us-south under account Test Account as user test.user@ibm.com...

      ID                                          Name                  VPC      Lifecycle state   Transit Encryption   
      r006-e6bd52b8-c656-4ba6-8749-1bb41bfa2c3c   my-new-mount-target   my-vpc   stable            user_managed  
      ```
      {: screen}

   - The following example creates a regional file share with customer-managed encryption, security group access mode, and encryption-in-transit enabled.

      ```sh
      ibmcloud is share-create --name my-regional-file-share --profile rfs --size 40 --bandwidth 800 --atem stunnel,none --encryption-key crn:v1:bluemix:public::kms:us-south:a/a1234567:key:2fb8d675-bde3-4780-b127-3d0b413631c1
      ```
      {: pre}

      ```json  
      Creating file share my-file-share1 under account Test Account as user test.user@ibm.com...
                                      
      ID                                 r006-9ae55188-610e-4cf9-9350-d0b675026ff8   
      Name                               my-regional-file-share
      CRN                                crn:v1:bluemix:public:is:us-south:a/a1234567::share:r006-9ae55188-610e-4cf9-9350-d0b675026ff8   
      Lifecycle state                    pending   
      Access control mode                security_group   
      Accessor binding role              none   
      Allowed transit encryption modes   stunnel,none   
      Zone                               -   
      Profile                            rfs   
      Size(GB)                           40   
      IOPS                               35000   
      Encryption                         user_managed   
      Mount Targets                      ID                          Name      
                                         No mounted targets found.      
                                      
      Resource group                     ID                                 Name      
                                         11caaa983d9c4beb82690daab08717e9   Default      
                                      
      Created                            2025-09-22T21:17:23+05:30
      Encryption key                     crn:v1:bluemix:public:kms:us-south:a/a1234567-c02c-4d7f-81e2-2aa867da176d:key:2fb8d675-bde3-4780-b127-3d0b413631c1      

      Replication role                   none   
      Replication status                 none   
      Replication status reasons         Status code   Status message      
                                         -             -      
                                      
      Snapshot count                     0   
      Snapshot size                      0   
      Source snapshot                    -   
      Allowed Access Protocols           nsf4   
      Availability Mode                  regional   
      Bandwidth(Mbps)                    800   
      Storage Generation                 2
      ```
      {: screen}

For more information about the command options, see [`ibmcloud is share-create`](/docs/vpc?topic=vpc-vpc-reference#share-create).

## Creating file shares with customer-managed encryption with the API
{: #fs-byok-api}
{: api}

You can create file shares with customer-managed encryption by calling the [Virtual Private Cloud (VPC) API](/apidocs/vpc).

Make a `POST /shares` request and specify the `encryption_key` parameter to identify your customer root key (CRK). It is shown in the example as `crn:[...key:...]`. 

You must provide the `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc/latest#api-generation-parameter).
{: requirement}

The following example creates a zonal file share with a mount target, and specifies the CRN of the root key for customer-managed encryption.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2024-11-05&generation=2" -H "Authorization: $iam_token" \
-d '{
     "name": "my-encrypted-share",
     "mount_targets": [
         {
           "name": "docs-mount-1",
           "virtual_network_interface": {
             "name": "my-virtual-network-interface-1",
             "allow_ip_spoofing": false,
             "auto_delete": true,
             "enable_infrastructure_nat": true,
             "primary_ip": {"auto_delete": true},
             "subnet": {"id": "0727-267015ac-7b12-4f62-bda9-52fcb9483fc4"},
             "ips": [],
             "security_groups": [{"id": "r006-bf9475c2-6846-4c39-b392-587643b2e2f8"}],
             "protocol_state_filtering_mode": "auto"
          },
          "transit_encryption": "none"
         }
       ],
     "profile": {"name": "dp2"},
     "size": 100,
     "zone": {"name": "us-south-2"},
     "iops": 3000,
     "allowed_transit_encryption_modes": ["none","user_managed"],
     "encryption_key": {"crn": "crn:v1:bluemix:public:kms:us-south:a/a1234567-c02c-4d7f-81e2-2aa867da176d:key:2fb8d675-bde3-4780-b127-3d0b413631c1"},
     "resource_group": {"id": "db00a952a88945a987b7be1980fdae8e"},
     "access_control_mode": "security_group"
   }'
```
{: pre}

The following example creates a regional file share without a mount target, and specifies the CRN of the root key for customer-managed encryption.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2025-09-02&generation=2" -H "Authorization: $iam_token" \
-d '{
     "name": "my-encrypted-regional-share",
     "profile": {"name": "rfs"},
     "size": 1000,
     "bandwidth": 800,
     "allowed_transit_encryption_modes": ["none","stunnel"],
     "encryption_key": {"crn": "crn:v1:bluemix:public:kms:us-south:a/a1234567-c02c-4d7f-81e2-2aa867da176d:key:2fb8d675-bde3-4780-b127-3d0b413631c1"},
     "resource_group": {"id": "db00a952a88945a987b7be1980fdae8e"},
     "access_control_mode": "security_group"
   }'
```
{: pre}

You can also specify the CRN of a root key from a different account in the `POST /shares` call. If you want to do that, contact the other account's administrator to ensure that the service-to-service authorizations are in place and to get the CRN of the encryption key.

## Creating file shares with customer-managed encryption with Terraform
{: #fs-byok-terraform}
{: terraform}

To create a file share, use the `ibm_is_share` resource. The following example creates a zonal file share with 800 GiB capacity and the `dp2` performance profile. The file share is encrypted by using a key that is identified by its CRN. The example also specifies a new mount target with a virtual network interface.

```terraform
resource "ibm_is_share" "share4" {
   zone                = "us-south-2"
   size                = "800"
   iops                = "3000"
   name                = "my-share4"
   profile             = "dp2"
   encryption_key      = "crn:v1:bluemix:public:kms:us-south:a/a1234567:key:2fb8d675-bde3-4780-b127-3d0b413631c1"
   access_control_mode = "security_group"
   mount_target {
       name = "target"
       security_groups = [<security_group_ids>]
       virtual_network_interface {
         primary_ip {
           address     = "10.240.64.5"
           auto_delete = true
           name        = "my-example-pip"
         }
       }
   }
}
```
{: screen}

The following example creates a regional file share with 1000 GiB capacity and the `rfs` performance profile. The file share is encrypted by using a key that is identified by its CRN.

```terraform
resource "ibm_is_share" "regional-share" {
   size                = "1000"
   name                = "my-regional-share"
   profile             = "rfs"
   bandwidth           = "800"
   encryption_key      = "crn:v1:bluemix:public:kms:us-south:a/a1234567:key:2fb8d675-bde3-4780-b127-3d0b413631c1"
   access_control_mode = "security_group"
   }
```
{: screen}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

## Next steps
{: #next-step-fs-byok}

- Use the [IBM Cloud File Share Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to mount your encrypted file share to an authorized Compute instance.

- Manage the root keys that are protecting your file share by [rotating](/docs/vpc?topic=vpc-vpc-key-rotation), [disabling](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-disable-root-keys), or [deleting](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-delete-root-keys) keys.

- Consider setting up replication for your share. For more information, see [About file share replication](/docs/vpc?topic=vpc-file-storage-replication).

- Learn about [Sharing and mounting a file share from another account](/docs/vpc?topic=vpc-file-storage-accessor-create&interface=ui).
