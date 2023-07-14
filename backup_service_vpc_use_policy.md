---

copyright:
  years: 2022, 2023
lastupdated: "2023-06-27"

keywords: backup, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Applying backup policies to resources with tags
{: #backup-use-policies}

Apply backup policies by adding tags to new or existing {{site.data.keyword.block_storage_is_short}} volumes. When these tags match a backup policy tag, a backup is created.
{: shortdesc}

## General procedure
{: #backup-gen-proc-tags}

1. [Create a backup policy and plan](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
2. [Apply backup policy tags](#backup-apply-tags-ui) to your target {{site.data.keyword.block_storage_is_short}} volumes by using the [UI](/docs/vpc?topic=vpc-backup-use-policies&interface=ui#backup-apply-tags-ui), [CLI](/docs/vpc?topic=vpc-backup-use-policies&interface=cli#backup-apply-tags-volumes-cli), or [API](/docs/vpc?topic=vpc-backup-use-policies&interface=api#backup-apply-tags-volumes-api). Go to the {{site.data.keyword.block_storage_is_short}} volume that you want to back up and add at least one tag to it.
3. Verify that your {{site.data.keyword.block_storage_is_short}} volume is associated with a backup policy. For more information, see [View a list of volumes that have a backup policy](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-vol-backup-policies).

## Applying tags to {{site.data.keyword.block_storage_is_short}} volumes in the UI
{: #backup-apply-tags-ui}
{: ui}

Apply tags to new or existing {{site.data.keyword.block_storage_is_short}} volumes in any of these ways:

* Add tags to volumes directly from the block volumes list view by using the tags column.
* Add tags from the volume details page.
* Click **Apply** on the volume details page to associate backup policies with tags.
* Add user tags to volumes during [instance provisioning](/docs/vpc?topic=vpc-backup-use-policies&interface=api#backup-apply-tags-volumes-api).

### Adding tags from the {{site.data.keyword.block_storage_is_short}} volumes list view
{: #backup-tags-volumes-list-ui}

1. Go to the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).
2. Locate an _available_ volume. In the **Tags** column, volumes with tags show a number that indicates the tags that are already applied. You can view the tags by clicking the number link. Volumes without tags have an **Add tags** link.
3. Click **Add tags**.
4. In the new window, type a tag in the User tags text box.
5. Click **Save**.

### Adding tags from the volume details page
{: #backup-tags-vol-details}

1. Go to the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).
2. Click the name of a volume in the list.
3. On the volume details page, click the pencil icon next to the {{site.data.keyword.block_storage_is_short}} volume name.
4. In the new window, type a tag in the User tags text box.
5. Click **Save**.

## Applying tags to {{site.data.keyword.block_storage_is_short}} volumes from the CLI
{: #backup-apply-tags-volumes-cli}
{: cli}

### Before you begin
{: ##backup-apply-tags-volumes-cli-prereq}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

2. Select the current generation of VPC. 
   ```sh
   ibmcloud is target --gen 2
   ```
   {: pre}

### Applying tags to volumes from the CLI
{: #backup-apply-tags-cli}

Issue the `ibmcloud is volume-update VOLUME` command with the `--tags` option to update the user tags of a volume. The volume argument can be defined by either the volume ID or the volume name.

Use the same option to add tags to a volume when you create a volume by using `ibmcloud is volume-create`.
{: tip}

The following example adds user tags `env:test` and `bkp:test` to a volume identified by ID. The output shows information such as name, status, capacity, performance profile, and location. The updated tags appear at the end of the response.

```sh
cloudshell:~$ ibmcloud is volume-update r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4 --tags env:test,bkp:test,bcp:test
Updating volume r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4 under account Test Account as user test.user@ibm.com...
                                          
ID                                     r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4   
Name                                   my-bootable-snapshot-restore-21   
CRN                                    crn:v1:bluemix:public:is:eu-de-2:a/a10d63fa66daffc9b9b5286ce1533080::volume:r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4   
Status                                 available   
Capacity                               100   
IOPS                                   3000   
Bandwidth(Mbps)                        393   
Profile                                general-purpose   
Encryption key                         -   
Encryption                             provider_managed   
Resource group                         defaults   
Created                                2023-02-23T18:52:00+00:00   
Zone                                   eu-de-2   
Health State                           ok   
Volume Attachment Instance Reference   -   
Source snapshot                        ID                                          Name      
                                       r138-92c3efcb-4588-4c9c-828b-c52836629954   my-bootable-snapshot      
                                          
Operating system                       CentOS 7.x - Minimal Install (amd64)   
Source image                           ID                                          Name      
                                       r010-067bd38b-7ddd-49d9-a7f3-6e0a798e0554   ibm-centos-7-9-minimal-amd64-5      
                                          
Active                                 false   
Busy                                   false   
Tags                                   env:test,bkp:test,bcp:test
```
{: screen}

## Applying tags to {{site.data.keyword.block_storage_is_short}} volumes with the API
{: #backup-apply-tags-volumes-api}
{: api}

To apply tags to a {{site.data.keyword.block_storage_is_short}} volume, follow these steps:

1. Make a `GET /volumes/{volume_id}` call and copy the hash string from the `Etag` property in the response header. You need to use the hash string when you specify `If-Match` in the `PATCH /volumes/{volume_id}` request to create user tags for the volume in step 2. To generate the response header information, make an API call similar to the following example:

   ```sh
   curl -sSL -D GET\
   "https://us-south.iaas.cloud.ibm.com/v1/volumes/{volume_id}?version=2022-04-25&generation=2"\
   -H "Authorization: Bearer $TOKEN" -o /dev/null
   ```
   {: pre}

   In the response header, you see something like this:

   ```json
   HTTP/2 200
   date: Tue, 28 Apr 2022 17:48:03 GMT
   content-type: application/json; charset=utf-8
   content-length: 1049
   cf-ray: 69903d250c4966ef-DFW
   cache-control: max-age=0, no-cache, no-store, must-revalidate
   expires: -1
   strict-transport-security: max-age=31536000; includeSubDomains
   cf-cache-status: DYNAMIC
   expect-ct: max-age=604800, report-uri="[uri...]"
   pragma: no-cache
   x-content-type-options: nosniff
   x-request-id: 1fbe2384-6828-4503-ae7d-050426d1b11b
   x-xss-protection: 1; mode=block
   server: cloudflare
   etag: W/xxxyyyzzz123
   ```
   {: codeblock}

2. Make a `PATCH /volumes/{volume_id}` request. Specify the _Etag-hash-string_ for the `If-Match` property in the header. Specify the user tags in the `user_tags` property.

   You can also add tags when you make a `POST /volumes` call to create a volume and specify the `user_tags` property.
   {: tip}

   This example updates the volume by specifying user tags `env:test` and `env:prod`. The value that you obtained from the `Etag` parameter is specified in the `If-Match` header in the call.

   ```sh
   curl -X PATCH\
   "$vpc_api_endpoint/v1/volumes/50fda9c3-eecd-4152-b473-a98018ccfb10?version=2022-04-25&generation=2"\
      -H "Authorization: Bearer"\
      -H "If-Match: <_Etag-hash-string_>"\
      -d `{
         "user_tags": [
            "env:test",
            "env:prod"
         ]
      }'
   ```
   {: codeblock}

   The response shows the tags that were added to the volume:

   ```json
   {
      "id":"50fda9c3-eecd-4152-b473-a98018ccfb10",
        "crn": "crn:[...]",
      "name":"my-volume-update1",
      "href":"https://us-south.iaas.cloud.ibm/v1/volumes/50fda9c3-eecd-4152-b473-a98018ccfb10",
      "capacity":50,
      "iops":100,
      "encryption":"provider_managed",
      "status":"pending",
      "zone":{
         "name":"us-south-1",
         "href":"https://us-south.iaas.cloud.ibm/v1/regions/us-south/zones/us-south-1"
      },
         "profile":{
         "name":"custom",
         "href":"https://us-south.iaas.cloud.ibm/v1/volume/profiles/custom"
      },
      "resource_group":{
         "id":"4bbce614c13444cd8fc5e7e878ef8e21",
         "href":"https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
         "name":"Default"
      },
      "volume_attachments":[],
      "created_at":"2022-04-28T17:46:17.000Z",
      "status_reasons":[],
      "active":false,
      "busy":false,
      "bandwidth":128,
      "user_tags":[
        "env:test",
         "env:prod"
      ]
   }
   ```
   {: codeblock}


## Applying tags to {{site.data.keyword.block_storage_is_short}} volumes with Terraform
{: #backup-apply-tags-volumes-terraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a regional specific based endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the right region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: screen}

To apply user tags to a volume, use the `ibm_is_volume` resource. The following example specifies the volume `r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4` and the tags `env:test`, `bkp:test`, and `bcp:test` to be attached to the volume.

```terraform
resource "ibm_is_volume" "example" {
  name    = "example-volume"
  profile = "10iops-tier"
  zone    = "us-south-1"
  tags = ["dev:test"]
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/ibm_is_volume){: external}.

## Next steps
{: #backup-next-steps-use}

* [Manage your backup policies and plans](/docs/vpc?topic=vpc-backup-service-manage).
* [Create extra backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
