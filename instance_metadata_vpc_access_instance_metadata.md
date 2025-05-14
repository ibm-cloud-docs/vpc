---

copyright:
  years: 2022, 2023
lastupdated: "2025-05-14"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Accessing metadata from an instance
{: #imd-access-instance-metadata}

Most often, you want to collect metadata from a running instance and use it to bootstrap another virtual server instance. Review the general procedure and step-by-step instructions for enabling the metadata service, creating an instance identity access token, and accessing the metadata.
{: shortdesc}

## General procedure to access instance metadata
{: #imd-gen-procedure}

Table 1 describes the steps that are involved in accessing instance metadata. The information provides the context for each step and links to specific information for completing the step. In an actual environment, steps 3 - 5 would more likely be started by your instance's initialization software at startup or by using cloud-init.

| Step | Context | Service Called | User Action |
|------|---------|----------------|-------------|
| 1    | IBM Cloud | VPC UI, CLI, API | The metadata service is disabled by default. You can enable the metadata service on an existing instance in the [console](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-enable-on-instance-ui), from the CLI, or with the API. Customer user data is specified on instance creation. |
| 2    | IBM Cloud | - | Sign on to the instance by using normal startup operations. |
| 3    | VPC instance | Metadata service | Run a `curl` command to call the metadata token service to [acquire an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-json-token). |
| 4    | VPC instance | Metadata service | Run a `curl` command to [call the metadata service](/docs/vpc?topic=vpc-imd-get-metadata#imd-retrieve-instance-data). The token from the previous step is passed and the metadata is returned.|
| 5    | VPC instance | - | Parse the JSON returned in the previous step to acquire the user data. |
{: caption="General procedure for accessing instance metadata" caption-side="bottom"}

## End-to-end procedure for accessing metadata from an instance
{: #imd-access-md-ex}

### Locating the running instance and enabling the metadata service in the console
{: #imd-access-md-locate-vsi-ui}
{: ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**.

1. Locate the running instance in the list. Click the name of the instance to display its details. On the Overview tab, scroll to the **Advanced configuration details**, and click the toggle to enable the metadata service.

1. If the instance has a floating IP address already (shown on the Networking tab), use that address to establish a secure connection to the server. If it does not have a floating IP address, assign one to it. For more information, see the [Next steps](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui) in the Creating virtual server instances topic.

### Locating the running instance and enabling the metadata service from the CLI
{: #imd-access-md-locate-vsi-cli}
{: cli}

1. Log in to IBM Cloud CLI.

1. Locate the running instance by using the `ibmcloud is instances` command, which lists the available instances in the region.
    ```sh
    $ ibmcloud is instances
    Listing instances in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
    ID                                          Name                    Status    Reserved IP    Floating IP      Profile    Image                                VPC                         Zone         Confidential Compute Mode   Enable Secure Boot   Resource group   Reservation Name   Cluster Network ID   Cluster Network Name   Cluster Network Attachments   
    0717_9c55d9d2-0685-4300-b475-49eb1b8c1faf   my-virtual-server-1     running   10.240.0.6     -                bx2-2x8    ibm-ubuntu-24-04-2-minimal-amd64-1   my-test-vpc                 us-south-1   disabled                    false                defaults         -                  -                    -                      -   
    0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6   my-virtual-server-2     running   10.240.64.11   169.47.94.48     bx2-2x8    ibm-ubuntu-24-04-6-minimal-amd64-3   my-test-vpc                 us-south-2   disabled                    false                defaults         -                  -                    -                      -   
    0727_d7ff31ef-75ed-42c6-b0fb-a5837a63d722   my-virtual-server-3     running   10.240.64.12   52.116.204.232   bx2-2x8    ibm-redhat-9-2-minimal-amd64-5       my-test-vpc                 us-south-2   disabled                    false                defaults         -                  -                    -                      -   
    ```
    {: screen}

    The metadata service is supported on all stock and custom images, and CPU profiles.
    {: note}

1. Run `ibmcloud is instance` command to confirm if the metadata service is enabled. The following example shows the value `false`.

    ```sh
    $ ibmcloud is instance 0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6
    Getting instance 0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6 under account Test Account as user test.user@ibm.com...
                                         
    ID                                    0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6   
    Name                                  my-virtual-server-2   
    CRN                                   crn:v1:bluemix:public:is:us-south-2:a/a1234567::instance:0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6   
    Status                                running   
    Availability policy on host failure   restart   
    Confidential Compute Mode             disabled   
    Enable Secure Boot                    false   
    Startable                             true   
    Profile                               bx2-2x8   
    Architecture                          amd64   
    vCPU Manufacturer                     intel   
    vCPUs                                 2   
    Memory(GiB)                           8   
    Bandwidth(Mbps)                       4000   
    Volume bandwidth(Mbps)                1000   
    Network bandwidth(Mbps)               3000   
    Lifecycle Reasons                     Code   Message      
                                          -      -      
                                         
    Lifecycle State                       stable   
    Metadata service                      Enabled   Protocol   Response hop limit      
                                          false     http       1      
                                         
    Image                                 ID                                          Name      
                                          r006-aa2af291-45b3-4f18-801c-8b7985e928f7   ibm-ubuntu-24-04-6-minimal-amd64-3      
                                         
    Numa Count                            1   
    VPC                                   ID                                          Name      
                                          r006-01030e3c-2663-4f7d-ac55-651929dafe37   my-test-vpc      
                                         
    Zone                                  us-south-2   
    Resource group                        ID                                 Name      
                                          6edefe513d934fdd872e78ee6a8e73ef   defaults      
                                         
    Created                               2025-03-06T19:24:02+00:00   
    Network Attachments                   Interface   Name   ID                                          Subnet              Subnet ID                                   Floating IP    VNI                                         Reserved IP      
                                          Primary     eth0   0727-a3ff4d3e-be95-4a52-8025-b55b3c3285ea   my-subnet-01   0727-f24237f5-bdf0-4b94-ab4c-167a44b8bcb5   169.47.94.48   0727-dda9244a-64b9-421b-87e9-70549a70b4c3   10.240.64.11      
                                         
    Boot volume                           ID                                          Name                                Attachment ID                               Attachment name      
                                          r006-9afd6e01-0466-4e7b-a38b-75bf39469a42   eat-client-poc-boot-1741289020000   0727-241f39a8-3573-4ebc-b512-33508eb970c9   undaunted-starved-slander-galleria      
                                             
    Reservation Affinity Policy           automatic   
    Reservation Affinity Pool             -   
    Reservation                           -   
    Health State                          ok   
    ```
    {: screen}

1. Enable the metadata service by running the `ibmcloud is instance-update` command with the option `--metadata-service true`.  

    ```sh
    $ ibmcloud is instance-update  0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6 --metadata-service true
    Updating instance 0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6 under account Test Account as user test.user@ibm.com...
                                         
    $ ibmcloud is instance 0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6
    Getting instance 0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6 under account Test Account as user test.user@ibm.com...
                                         
    ID                                    0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6   
    Name                                  my-virtual-server-2   
    CRN                                   crn:v1:bluemix:public:is:us-south-2:a/a1234567::instance:0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6   
    Status                                running   
    Availability policy on host failure   restart   
    Confidential Compute Mode             disabled   
    Enable Secure Boot                    false   
    Startable                             true   
    Profile                               bx2-2x8   
    Architecture                          amd64   
    vCPU Manufacturer                     intel   
    vCPUs                                 2   
    Memory(GiB)                           8   
    Bandwidth(Mbps)                       4000   
    Volume bandwidth(Mbps)                1000   
    Network bandwidth(Mbps)               3000   
    Lifecycle Reasons                     Code   Message      
                                          -      -      
                                         
    Lifecycle State                       stable   
    Metadata service                      Enabled   Protocol   Response hop limit      
                                          true      http       1      
                                         
    Image                                 ID                                          Name      
                                          r006-aa2af291-45b3-4f18-801c-8b7985e928f7   ibm-ubuntu-24-04-6-minimal-amd64-3      
                                         
    Numa Count                            1   
    VPC                                   ID                                          Name      
                                          r006-01030e3c-2663-4f7d-ac55-651929dafe37   my-test-vpc      
                                         
    Zone                                  us-south-2   
    Resource group                        ID                                 Name      
                                          6edefe513d934fdd872e78ee6a8e73ef   defaults      
                                         
    Created                               2025-03-06T19:24:02+00:00   
    Network Attachments                   Interface   Name   ID                                          Subnet              Subnet ID                                   Floating IP    VNI                                         Reserved IP      
                                          Primary     eth0   0727-a3ff4d3e-be95-4a52-8025-b55b3c3285ea   my-subnet-01   0727-f24237f5-bdf0-4b94-ab4c-167a44b8bcb5   169.47.94.48   0727-dda9244a-64b9-421b-87e9-70549a70b4c3   10.240.64.11      
                                         
    Boot volume                           ID                                          Name                                Attachment ID                               Attachment name      
                                          r006-9afd6e01-0466-4e7b-a38b-75bf39469a42   eat-client-poc-boot-1741289020000   0727-241f39a8-3573-4ebc-b512-33508eb970c9   undaunted-starved-slander-galleria      
                                             
    Reservation Affinity Policy           automatic   
    Reservation Affinity Pool             -   
    Reservation                           -   
    Health State                          ok
    ```
    {: screen}

1. If the instance has a floating IP address already, use that address to establish a secure connection to the server. If it does not have a floating IP address, assign one to it. For more information, see the [Next steps](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#next-step-after-creating-virtual-servers-cli) in the Creating virtual server instances topic.

### Locating the running instance and enabling the metadata service with the API
{: #imd-access-md-locate-vsi-api}
{: api}

1. Locate the running instance by [listing the available instances](/apidocs/vpc/latest#list-instances) in the region. Select the instance ID from the response.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/instances?version=2025-05-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

1. [Retrieve the information of the selected instance](/apidocs/vpc/latest#get-instance) to confirm if the metadata service is enabled.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/instances/0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6?version=2025-05-13&generation=2" -H "Authorization: Bearer $iam_token"
   ```
   {: pre}

   The API response shows `"enabled": false` in the `metadata_service` section.

   ```json
   {
    "availability_policy": {
        "host_failure": "restart",
        "host_maintenance": null
    },
    "bandwidth": 4000,
    "boot_volume_attachment": {
        "device": {
            "id": "0727-241f39a8-3573-4ebc-b512-33508eb970c9-mzpzd"
        },
        "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6/volume_attachments/0727-241f39a8-3573-4ebc-b512-33508eb970c9",
        "id": "0727-241f39a8-3573-4ebc-b512-33508eb970c9",
        "name": "undaunted-starved-slander-galleria",
        "volume": {
            "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-9afd6e01-0466-4e7b-a38b-75bf39469a42",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-9afd6e01-0466-4e7b-a38b-75bf39469a42",
            "id": "r006-9afd6e01-0466-4e7b-a38b-75bf39469a42",
            "name": "eat-client-poc-boot-1741289020000",
            "resource_type": "volume"
        }
    },
    "confidential_compute_mode": "disabled",
    "created_at": "2025-03-06T19:24:02.000Z",
    "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::instance:0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6",
    "disks": [],
    "enable_secure_boot": false,
    "health_reasons": [],
    "health_state": "ok",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6",
    "id": "0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6",
    "image": {
        "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::image:r006-aa2af291-45b3-4f18-801c-8b7985e928f7",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-aa2af291-45b3-4f18-801c-8b7985e928f7",
        "id": "r006-aa2af291-45b3-4f18-801c-8b7985e928f7",
        "name": "ibm-ubuntu-24-04-6-minimal-amd64-3",
        "resource_type": "image"
    },
    "lifecycle_reasons": [],
    "lifecycle_state": "stable",
    "memory": 8,
    "metadata_service": {
        "enabled": false,
        "protocol": "http",
        "response_hop_limit": 1
    }
    .
    .
    .
   }
   ```
   {: codeblock}

1. Enable the metadata service by making a [`PATCH /instances`](/apidocs/vpc/latest#update-instance) request for the instance.

   ```json
   curl -X PATCH "$vpc_api_endpoint/v1/instances/0727_ed12480a-40a4-41a0-98e3-6dfac8b25ad6?version=2025-05-13&generation=2" \
   -H "Authorization: Bearer $iam_token" \
   -d '{
        "metadata_service": {
           "enabled": true,
           "protocol": "http",
           "response_hop_limit": 1
           }
       }`    
   ```
   {: pre}

1. If the instance has a floating IP address already, use that address to establish a secure connection to the server. If it does not have a floating IP address, assign one to it. For more information, see [logging in to your instance](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=api#log-in-to-instance-api-tutorial).

### Establishing a secure connection to the virtual server instance
{: #imd-access-md-login-vsi}

The following example shows the command syntax to use to connect to a Linux-based server instance.

```sh
ssh -i <path to your private key file> <default-user-account>@<floating ip address>
```
{: pre}

If your server is running a Windows OS, use an RDP client.

For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows).

### Collecting information from the metadata service
{: #imd-access-md-use}

1. From the virtual server instance, make a request to the metadata token service to retrieve an instance identity access token. Specify how long you want the token to remain valid. For example, you can specify 3600 seconds (1 hour).

   ```json
   export instance_identity_token=`curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2024-11-12"\
     -H "Metadata-Flavor: ibm"\
     -H "Accept: application/json"\
     -d '{
           "expires_in": 3600
         }' | jq -r '(.access_token)'`
   ```
   {: codeblock}

   The response provides the instance identity access token. The parser saves it into the `.access_token` file.

   The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use another parser of your choice.
   {: note}

1. You can now make an API request to the metadata service to gather the initialization information:

   ```sh
   curl -X GET "http://api.metadata.cloud.ibm.com/metadata/v1/instance/initialization?version=2024-11-12"\
      -H "Accept: application/json"\
      -H "Authorization: Bearer $instance_identity_token"\
      | jq -r
   ```
   {: codeblock}

   The API response contains information such as the SSH key and user data that was specified when the virtual server was provisioned. If you configured passwords, that information is also returned. 

1. Use other API methods to retrieve more information about the instance, such as volume and network attachments, or gather information about SSH keys, placement groups, or virtual network interfaces. For more information, see the [Metadata service API](/apidocs/vpc-metadata) reference and the [Summary of instance metadata service information](/docs/vpc?topic=vpc-imd-metadata-summary).

## Next steps
{: #imd-access-md-next-steps}

Use the trusted profile for the instance and generate an IAM token from the instance identity access token to other IAM-enabled services. See [Using a trusted profile to call IAM-enabled services](/docs/vpc?topic=vpc-imd-trusted-profile-metadata).
