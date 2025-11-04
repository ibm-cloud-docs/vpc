---

copyright:
  years: 2022, 2025
lastupdated: "2025-11-04"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configure the metadata service
{: #imd-configure-service}

Configure the metadata service by obtaining an identity access token from the metadata service. Optionally, generate an IAM access token from this token to access IAM-enabled services in the account.
{: shortdesc}

## Enabling or disabling access to the metadata service
{: #imd-metadata-service-enable}

Access to the metadata service is disabled by default. To retrieve metadata from an instance, enable access to the service on new instances or existing instances by using the VPC UI, CLI, or API.

### Enabling access to metadata service in the console
{: #imd-enable-service-ui}
{: ui}

From the {{site.data.keyword.cloud}} console, you can enable or disable access to the metadata service.

#### Enabling access to the metadata service for an existing instance in the console
{: #imd-enable-on-instance-ui}

Use the UI to enable access to the metadata service on an existing instance.

1. Go to the list of instances. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

2. Click the name of an instance to go to the details page.

3. On the details page, scroll to **Metadata**.

4. Click the toggle (appears green).

#### Enabling access to the metadata service when you create an instance in the console
{: #imd-enable-new-instance-ui}

The following procedure shows how to enable access to the metadata service when you create a virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

2. Click **Create**.

3. [Provision the instance](/docs/vpc?topic=vpc-creating-virtual-servers).

4. Navigate to **Advanced options** and enable **Metadata**.

 For more information about creating virtual server instances, see [Creating virtual server instances in the console](/docs/vpc?topic=vpc-creating-virtual-servers).

### Disabling access to the metadata service in the console
{: #imd-disable-new-instance}
{: ui}

This procedure shows how to disable access to the metadata service for an instance on which it is enabled. By default, access to the metadata service is disabled when you create a new instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

2. Click an instance from the list to go to its details page.

3. Under **Metadata**, the click the toggle button off (appears gray).

#### Enabling or disabling access to the metadata service on instance templates in the console
{: #imd-enable-instance-template-ui}

When you create an [instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui#creating-instance-template), the metadata service toggle is disabled by default. Click the toggle to enable access to the service.

When you view the details of an existing instance template, the page indicates whether the metadata service was enabled or not for the template. You can't change the instance template metadata setting after you create the template.

### Enabling or disabling access to the metadata service from the CLI
{: #imd-metadata-service-enable-cli}
{: cli}

Use the CLI to enable access to the metadata service when you create a new instance or on an existing instance.

Before you begin:

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

1. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli#create-a-vpc-cli).

#### Enabling or disabling access to the metadata service when you create an instance from the CLI
{: #imd-enable-on-instance-cli}

Run the `ibmcloud is instance-create` command and set the `metadata-service` property to `true`. Access to the metadata service is disabled by default. In the response, you see `Metadata service enabled` set to `true`.

```json
ibmcloud is instance-create test-instance-1 7002c1cd-9b0b-43ee-8112-5124dedbe84b us-south-1  bx2-2x8  0711-08206578-d749-49ea-86c9-1014622d1c6f --image-id 9f0050d0-636b-4fe6-82ea-931664fd9d91 --metadata-service true

Creating instance test under account VPC1 as user myuser@mycompany.com...

ID                         264060c2-e5e9-44d4-994f-eea4a6688172
Name                       test-instance-1
CRN                        crn:v1:public:is:us-south-1:a/a1234567::instance:264060c2-e5e9-44d4-994f-eea4a6688172
Status                     pending
Startable                  true
Profile                    bx2-2x8
Architecture               amd64
vCPUs                      2
Memory(GiB)                8
Bandwidth(Mbps)            4000
Metadata service enabled   true
Image                      ID                                          Name
                           9f0050d0-636b-4fe6-82ea-931664fd9d91        ibm-ubuntu-20-04-minimal-amd64-1

VPC                        ID                                          Name
                           7002c1cd-9b0b-43ee-8112-5124dedbe84b        test-vpc1

Zone                       us-south-1
Resource group             ID                                  Name
                           21cabbd983d9c4beb82690daab08717e8   Default

Created                    2022-08-08T22:12:11+05:30
Boot volume                ID   Name           Attachment ID                               Attachment name
                           -    PROVISIONING   954c1c47-906d-423f-a8c8-dd3adfafd278        my-vol-attachment1
```
{: codeblock}

#### Enabling or disabling access to the metadata service for an existing instance from the CLI
{: #imd-enable-on-existing-instance-cli}

Run the `ibmcloud is instance-update` command and specify the instance ID. To enable access to the metadata service, set the `metadata-service` parameter to `true`; to disable, set it to `false`. An example command for enabling access to the service looks like this:

```sh
ibmcloud is instance-update e219a883-41f2-4680-810e-ee63ade35f98 --metadata-service true
```
{: codeblock}

#### Enabling or disabling access to the metadata service when you create new instance templates from the CLI
{: #imd-enable-instance-template-cli}

When you create an [instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=cli#creating-instance-template-cli) from the CLI, you can indicate whether metadata is collected for instances created based on this template.

Use the `ibmcloud is instance-create-from-template` command and specify `--metadata-service true` option to enable or `--metadata-service false` option to disable. After you set the template value, you can't change it.

For example, to create an instance template with access to the metadata service enabled, run this command:

```sh
ibmcloud is instance-template-create my-template-name {template_id} us-south-1 mx2-2x16 {subnet_id} --image-id {image_id} --metadata-service true
```
{: pre}

When you create an instance from this template, specify the `--metadata-service true` option again to enable access to the service on the new instance:

```sh
ibmcloud is instance-create-from-template --template-id {template_id} --name my-instance --metadata-service true
```
{: pre}

If you override the instance template by running the `ibmcloud is instance-template-create-override-source-template` command, you can enable or disable access to the metadata service by specifying the `--metadata-service` option with `true` or `false`.

For more information about these commands, see the [VPC CLI Reference](/docs/vpc?topic=vpc-set-up-environment&interface=cli). For more information about creating an instance template from the CLI, see [Creating an instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=cli#creating-instance-template-cli).

### Enabling or disabling access to the metadata service with the API
{: #imd-metadata-service-enable-api}
{: api}

#### Enabling or disabling access to the metadata service when you create a new instance with the API
{: #imd-disable-on-instance-api}

Access to the metadata service is disabled by default when you create an instance by making a `POST /instances` call.

You can enable access to the metadata service by specifying the `metadata_service` parameter and setting `enabled` to `true`.

This example shows enabling access to the metadata service at instance creation:

```sh
curl -X POST "$vpc_api_endpoint/v1/instances?version=2024-11-12&generation=2"\
-H "Authorization: Bearer $iam_token"\
-d '{
      "image": {
         "id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"
      },
      "keys": [
         {
           "id": "363f6d70-0000-0001-0000-00000013b96c"
         }
      ],
      "name": "my-instance",
      "metadata_service": {
         "enabled": true
      },
      .
      .
      .
   }'
```
{: codeblock}

The response shows that the metadata parameter is set to `true` when you enable access to the service. You can also verify the metadata service setting by making a `GET /instance/{id}` call.

#### Enabling or disabling access to the metadata service for an existing instance with the API
{: #imd-enable-on-instance-api}

To enable or disable access to the service from an existing instance, make a `PATCH /instance/{instance_id}` request and specify the `metadata_service` parameter. By default, the `enabled` property is set to `false`. To enable access to the service, set it to `true`.

This example call shows enabling access to the metadata service for an instance:

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-11-12&generation=2"\
    -H "Authorization: Bearer $iam_token"\
    -d '{
          "metadata_service": {
            "enabled": true
          }
      }'
```
{: codeblock}

The response shows that the metadata parameter is set to `true` when you enable access to the service. You can also verify the metadata service setting by making a `GET /instance/{id}` call.

#### Enabling or disabling access to the metadata service when you create new instance templates by using the API
{: #imd-enable-instance-template-api}

When you create an instance template, you can set this value by making a `POST /instance/templates` request. By default, the `enabled` property is set to `false`. To enable it, set it to `true`.

For example,

```sh
curl -X POST "$vpc_api_endpoint/v1/instance/templates?version=2024-11-12&generation=2"\
    -H "Authorization: Bearer $iam_token"\
    -d '{
         "image": {
           "id": "3f9a2d96-830e-4100-9b4c-663225a3f872"
         },
         "keys": [
           {
             "id": "363f6d70-0000-0001-0000-00000013b96c"
           }
         ],
         "name": "my-instance-template",
         "metadata_service": {
             "enabled": true
         },
         "primary_network_interface": {
           "subnet": {
             "id": "0d933c75-492a-4756-9832-1200585dfa79"
           }
         },
         "profile": {
           "name": "bx2-2x8"
         },
         "vpc": {
           "id": "dc201ab2-8536-4904-86a8-084d84582133"
         },
         "zone": {
           "name": "us-south-1"
         }
       }'
```
{: pre}

You can't use the API to change the `metadata-service` setting after the instance template is created. If you disabled access to the service for a template, create a new instance template with the `metadata-service` enabled set to `true`.

## Configure metadata settings in the console
{: #metadata-config-ui}
{: ui}

You can configure features of the metadata service in the console. When access to the metadata service is enabled, expand the metadata window to access the metadata service settings.

### Select a trusted profile in the console
{: #select-trusted-profile-ui}
{: ui}

You can select an existing trusted profile for the metadata service.

**Select a trusted profile when you provision an instance** To select a trusted profile, navigate to the Default trusted profile option and select a trusted profile from a list of preexisting trusted profiles.

For more information, see [Create a trusted profile for the instance](/docs/vpc?topic=vpc-imd-identity-operations&interface=ui#imd-trusted-profile-config).

### Toggle auto link in the console
{: #auto-link-ui}
{: ui}

You can toggle auto link for the metadata service. When autolink is enabled, the specified trusted profile is automatically linked to the virtual server instance when the instance is provisioned. On instances provisioned with auto link, the trusted profile is available to the instance immediately at startup. When auto link is disabled, the specified trusted profile must be linked to the instance for it to be used by the instance.

**Toggle auto link when you provision an instance** To toggle auto link when you provision an instance, navigate to the Secure access setting in the Metadata window on the instance provision page. Toggle the secure access switch so that it displays `Enabled`.

### Enable secure access in the console
{: #secure-access-ui}
{: ui}

You can enable secure access to the metadata service. When secure access is enabled, the metadata service is accessible only to the virtual server instance by encrypted HTTP secure protocol (HTTPS). When secure access is disabled, the metadata service is accessible only to the virtual server instance by unencrypted HTTP protocol. Secure access is disabled by default.

Additional properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside an instance with secure access to the metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}

#### Enable secure access when you provision an instance
{: #secure-access-ui-provisioning}
{: ui}

To enable secure access when provisioning an instance, navigate to the Secure access setting in the Metadata window when you **Create** an instance from the **Virtual server instances for VPC** page. Toggle the secure access switch so that it displays `Enabled`.

#### Enable secure access on an existing instance
{: #secure-access-ui-existing}
{: ui}

To enable secure access on an existing instance, navigate to the Secure access setting on the **Instance details** page of your instance.

### Set the metadata hop limit in the console
{: #set-hop-limit-ui}
{: ui}

You can set the hop limit for IP response packets from the metadata service. The hop limit can be any value from 1 (default) to 64. Access to the metadata service must be enabled.

#### Set the metadata hop limit when you provision an instance
{: #set-hop-limit-ui-provisioning}
{: ui}

To set the hop limit when you provision an instance, go to the Hop limit setting in the Metadata window when you **Create** an instance from the **Virtual server instances for VPC** page. Specify a hop limit value between 1 and 64.

#### Set the metadata hop limit on an existing instance
{: #set-hop-limit-ui-existing}
{: ui}

To set the hop limit on an existing instance, go to the Hop limit setting on the **Instance details** page of your instance. Specify a hop limit value between 1 and 64.

## Configure metadata settings by using the CLI
{: #metadata-config-cli}
{: cli}

You can enable and disable features of the metadata service using the CLI.

The following example shows an instance with access to the metadata service enabled.

```sh
ibmcloud is instance instance-name
```
{: pre}

```sh
ID                                    0716_9cc6d74d-4b77-4cca-b1f4-31cc6edefe01
Name                                  instance-name
CRN                                   crn:v1:bluemix:public:is:us-south-1:a/a1234567::instance:0716_9cc6d74d-4b77-4cca-b1f4-31cc6edefe01
Status                                running
Availability policy on host failure   restart
Startable                             true
Profile                               bx2-2x8
Architecture                          amd64
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
                                      r006-1025e040-7d6f-408c-b4db-6156dc986fc7   ibm-ubuntu-22-04-1-minimal-amd64-2

Numa Count                            1
VPC                                   ID                                          Name
                                      r006-ac1c1ae4-5573-42eb-9194-854c9a3d5555   fode

.
.
.
```
{: screen}

### Disable auto link
{: #auto-link-cli}
{: cli}

You can disable auto link for the metadata service when provisioning an instance. Auto link is enabled by default when a trusted profile is selected. When autolink is enabled, the specified trusted profile is automatically linked to the virtual server instance when the instance is provisioned. An instance provisioned with auto link the trusted profile is available to the instance immediately at startup. When auto link is disabled, the specified trusted profile must be linked to the instance for it to be used by the instance.

To disable auto link, set the `--default-trusted-profile-auto-link` option to `true` when provisioning an instance. The following example shows instance the provision command with auto link set to `false`.

```sh
ibmcloud is instance-create .... --default-trusted-profile "Profile-9fd84246-7df4-4667-94e4-8ecde51d5ac5" --default-trusted-profile-auto-link false
```

### Enable secure access by using the CLI
{: #secure-access-cli}
{: cli}

You can enable secure access to the metadata service. When secure access is enabled, the metadata service is accessible only to the virtual server instance by encrypted HTTP Secure protocol (HTTPS). When secure access is disabled the metadata service is accessible only to the virtual server instance by unencrypted HTTP protocol. Secure access is disabled by default.

Certain properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside an instance with secure access to the metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}.

#### Enable secure access when you provision an instance
{: #secure-access-cli-provision}
{: cli}

To enable secure access when you provision an instance, specify a value for the `--metadata-service-protocol` option when you use the `instance-create` command. For secure access specify `https`. The default setting is unencrypted `http`.

```sh
ibmcloud is instance-create INSTANCE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET ... [--metadata-service-protocol http | https] ...

--metadata-service-protocol value : The communication protocol to use for the metadata service
  endpoint. Applies only when the metadata service is enabled. One of: http, https. (default: "http")
```

To enable secure access on an existing instance, specify a value for the `protocol` sub-property of the `metadata service` when you use the `instance-update` command.

### Set the metadata hop limit by using the CLI
{: #set-hop-limit-cli}
{: cli}

You can set a hop limit for IP response packets from the metadata service by specifying a hop limit value between `1` (default) and `64` for the `Response hop limit` sub-property of the `Metadata service` property for your instance.

#### Set the metadata hop limit when you provision an instance
{: #set-hop-limit-cli-when-provisioning}
{: cli}

To set the metadata hop limit when you provision an instance, specify a hop limit value between `1` (default) and `64` for the `--metadata-service-response-hop-limit` option when you use the `instance-create` command.

```sh
ibmcloud is instance-create INSTANCE_NAME VPC ZONE_NAME PROFILE_NAME SUBNET ... [--metadata-service-response-hop-limit METADATA-SERVICE-RESPONSE-HOP-LIMIT] ...

--metadata-service-response-hop-limit value : The hop limit (IP time to live) for IP response packets
  from the metadata service. (default: 1)
```

## Configure metadata settings by using the API
{: #metadata-config-api}
{: api}

You can enable and disable features of the metadata service by using the API.

### Disable auto link by using the API
{: #auto-link-api}
{: api}

You can disable auto link for the metadata service when provisioning an instance. Auto link is enabled automatically when a trusted profile is selected. When autolink is enabled, the specified trusted profile is automatically linked to the virtual server instance when the instance is provisioned. The trusted profile that is associated with an instance that is provisioned with auto link is available to the instance immediately at startup. When auto link is disabled, the specified trusted profile must be linked to the instance for it to be used by the instance.

To disable auto link by using the API, the `auto_link` value for the `default_trusted_profile` property must be set to `false`. The following example shows the `default_trusted_profile` property with the `auto_link` option disabled.

```json
"default_trusted_profile": {
      "auto_link": false,
   "target": {
       "id": "Profile-9fd84246-7df4-4667-94e4-8ecde51d5ac5"
   }
  },
```

### Enable secure access by using the API
{: #secure-access-api}
{: api}

You can enable secure access to the metadata service. When secure access is enabled, the metadata service is accessible only to the virtual server instance by encrypted HTTP Secure protocol (HTTPS). When secure access is disabled the metadata service is accessible only to the virtual server instance by unencrypted HTTP protocol. Secure access is disabled by default.
Certain properties might be required in the following scenarios:

- You are using the IBM Cloud CLI Virtual Server Instance for VPC compute resource identity login method. For more information, see [Logging in as a Virtual Server Instance Compute Resource Identity](/docs/cli?topic=cli-vsi-cri-login).
- You are using the IBM Cloud SDK with [VPC Instance Authentication](https://github.com/IBM/ibm-cloud-sdk-common#authentication){: external} inside an instance with secure access to the metadata service enabled. For more information, see [IBM Cloud Go SDK](https://github.com/IBM/go-sdk-core/blob/main/Authentication.md#vpc-instance-authentication){: external}.

#### Enable secure access when you provision an instance
{: #secure-access-api-provision}
{: api}

To enable secure access, when you provision an instance with the [POST /instances](/apidocs/vpc/latest#create-instance) method, specify a value for the `metadata_service.protocol` property for your instance. For secure access specify `https`. The default setting is unencrypted `http`.

#### Enable secure access on an existing instance
{: #secure-access-api-existing}
{: api}

To enable secure access on an existing instance, use the [PATCH /instances/{id}](/apidocs/vpc/latest#update-instance) method to update the instance. Specify a value for the `metadata_service.protocol` property for your instance. For secure access specify `https`. The default setting is unencrypted `http`.

### Set the metadata hop limit by using the API
{: #set-hop-limit-api}
{: api}

You can set the hop limit for IP response packets from the metadata service using the `metadata_service.response_hop_limit` property

This property applies only when access to the metadata service is enabled by setting `metadata_service.enabled` to `true`. The default is `false`.

#### Set the metadata hop limit when you provision an instance
{: #set-hop-limit-api-when-provisioning}
{: api}

To set the response when you provision an instance, call the [POST /instances method](/apidocs/vpc/latest#create-instance) method, and specify the `metadata_service.response_hop_limit` property value between `1` (default) and `64`.

This property applies only when access to the metadata service is enabled by setting `metadata_service.enabled` to `true`. The default is `false`.

#### Set the metadata hop limit on an existing instance
{: #set-hop-limit-api-on-existing-instance}
{: api}

To set the response hop limit on an existing instance, call the [PATCH /instances/{id}](/apidocs/vpc/latest#update-instance) method, and specify the `metadata_service.response_hop_limit` property value between `1` (default) and `64`.

## Next steps
{: #imd-token-next}

After you create an identity access token and enable access to the metadata service, you can retrieve metadata for the instance, SSH keys, and placement groups. For more information, see [Use the metadata service](/docs/vpc?topic=vpc-imd-access-instance-metadata).
