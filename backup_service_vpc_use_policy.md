---

copyright:
  years: 2022
lastupdated: "2022-05-20"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Applying backup policies to resources using tags
{: #backup-use-policies}

Apply backup policies by adding tags to new or existing block storage volumes. When these tags match a backup policy tag, a backup is created.
{: shortdesc}

## General procedure
{: #backup-gen-proc-tags}

1. [Create a backup policy and plan](/docs/vpc?topic=vpc-backup-policy-create).

2. [Apply backup policy tags](#backup-apply-tags-ui) to your target block storage volumes by using the [UI](/docs/vpc?topic=vpc-backup-use-policies&interface=ui#backup-apply-tags-ui), [CLI](/docs/vpc?topic=vpc-backup-use-policies&interface=cli#backup-apply-tags-volumes-cli), or [API](/docs/vpc?topic=vpc-backup-use-policies&interface=api#backup-apply-tags-volumes-api). Go to your block storage volume that you want to backup and add at least one tag to it.

3. Verify that your block storage volume is associated with a backup policy. For more information, see [View a list of volumes that have a backup policy](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-vol-backup-policies).

## Applying tags to block storage volumes with the UI
{: #backup-apply-tags-ui}
{: ui}

Apply tags to new or existing block storage volumes in any of these ways:

* Add tags to volumes directly from the block volumes list view using the tags column.
* Add tags from the volume details page.
* Click the Apply button for backup policies on the volume details page to associate backup policies by using tags

### Add tags from the block storage volumes list view
{: #backup-tags-volumes-list-ui}

1. Navigate to the [list of block storage volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

2. Locate an _available_ volume. In the **Tags** column, volumes with tags show a number indicating tags already applied. You can view the tags by clicking on the number link. Volumes without tags have an **Add tags** link.

3. Click **Add tags**.

4. In the popup, type a tag in the User tags text box.

5. Click **Save**.

### Add tags from the volume details page
{: #backup-tags-vol-details}

1. Navigate to the [list of block storage volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

2. Click on the name of a volume in the list.

3. On the volume details page, click the pencil icon next to the block storage volume name.

4. In the popup, type a tag in the User tags text box.

5. Click **Save**.

## Applying tags to block storage volumes from the CLI
{: #backup-apply-tags-volumes-cli}
{: cli}

Run the `ibmcloud is volume-update` command with the ``--user-tags` parameter to add user tags to a volume.

Use the same parameter to add tags to a volume when creating a new volume using `ibmcloud is volume-create`.
{: tip}

The following example adds user tags `env:test` and `env:prod` to a volume identified by ID.

```text
$ ibmcloud is volume-update 50fda9c3-eecd-4152-b473-a98018ccfb10 --user-tags env:test,env:prod
Updating volume 50fda9c3-eecd-4152-b473-a98018ccfb10 under account VPC1 as user user.mycompany.com...

ID                                     50fda9c3-eecd-4152-b473-a98018ccfb10   
Name                                   my-volume   
CRN                                    crn:bluemix:public:is:us-south:a/823bd195e9fd4f0db40ac2e1bffef3e0:369e7ee7-2c0d-4723-941a-601c1a273727::   
Status                                 available   
Capacity                               500   
IOPS                                   3000   
Bandwidth(Mbps)                        393   
Profile                                general-purpose   
Encryption key                         -   
Encryption                             provider_managed   
Resource group                         Default   
Created                                2022-04-28T11:42:22.287+05:30   
Zone                                   us-south-1   
Volume Attachment Instance Reference   -   
Active                                 false   
Busy                                   false   
User Tags                              env:test,env:prod  
```
{: screen}

## Apply tags to block storage volumes with the API
{: #backup-apply-tags-volumes-api}
{: api}

To apply tags to a block storage volume, follow these steps:

1. Make a `GET /volumes/{volume_id}` call and copy the hash string from `Etag` property in the response header. You will use the hash string when you specify `If-Match` in the `PATCH /volumes/{volume_id}` request to create new user tags for the volume in step 2. For example, to generate the response header information, make a call similar to this:

   ```curl
   curl -sSL -D GET\
   "https://us-south.iaas.cloud.ibm.com/v1/volumes/{volume_id}?version=2022-04-25&generation=2"\
   -H "Authorization: Bearer $TOKEN" -o /dev/null
   ```
   {: pre}

   In the response header, you'll see something like this:

   ```text
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

   You can also add tags when making a `POST /volumes` call to create a new volume and specifying the `user_tags` property.
   {: tip}

   This example updates the volume by specifying user tags `env:test` and `env:prod`. Note that the value you obtained from the `Etag` parameter is specified in the `If-Match` header in the call.

   ```curl
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

   The response shows the tags added to the volume:

   ```text
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

## Next steps
{: #backup-next-steps-use}

* [Manage your backup policies and plans](/docs/vpc?topic=vpc-backup-service-manage).
* [Create additional backup policies](/docs/vpc?topic=vpc-backup-policy-create&interface=ui).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
