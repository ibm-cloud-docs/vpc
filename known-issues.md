---

copyright:
  years: 2018, 2025
lastupdated: "2025-07-16"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

## Confidential computing known issues
{: #confidential-computing-vpc-known-issues}

[Select availability]{: tag-green}

### TDX virtual servers are supported in Washington DC (us-east) region only
{: #tdx-wdc-confidential-computing-vpc-known-issues}

**Issue:** All confidential computing profiles support both Intel&reg; Software Guard Extensions (SGX) and Intel&reg; Trusted Domain Extensions (TDX). When you use the API to list instance profiles, such as with `GET /instance/profiles` or `GET /instance/profiles/{name}`, the response indicates that all confidential computing profiles support SGX and TDX. However, TDX is currently available in the Washington DC (us-east) region only. If you want to create a virtual server instance with a confidential computing profile and TDX, you can create that virtual server instance only in the Washington DC (us-east) region. You can’t create a virtual server instance with TDX in any other region, including Dallas (us-south) and Frankfurt (eu-de).

### s390x profiles don't include 'values' property
{: #s390x-confidential-computing-vpc-known-issues}

**Issue:** When [listing instance profiles](/apidocs/vpc#list-instance-profiles) or [retrieving an instance profile](/apidocs/vpc#get-instance-profile), s390x instance profiles don't include the required `values` property in the `confidential_compute_modes` object. See [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles&interface=ui) for a complete list of profiles.

## Image known issues
{: #image-vpc-known-issues}

### Checksum not available for some public images
{: #RIOS-1410}

**Issue:** When you use the API or CLI to list images, some public stock images might not include a checksum. The checksum is for informational purposes only for stock images. No fix is available.

### A custom image that is created from a boot volume that was provisioned from an unencrypted image is bigger than the original image
{: #boot-volume-larger-minimum-provisioned-size}

**Issue**: If your custom image is not encrypted and the image is under 100 GB virtual disk size, deploying that image to an instance and creating a custom image from that instance's boot volume results in a `minimum_provisioned_size` of 100 GB. No fix is available.

### Custom images in a private catalog known issue
{: #custom-images-private-catalog-known-issues}

**Issue:** If you imported one or more images into a virtual server image for VPC catalog product offering version and you edit that version, an extra version ending in "draft" is created. You can't provision an instance from this draft version. Draft versions might appear on the Virtual server instance creation page in the console or in the output of the CLI command `ibmcloud is catalog-image-offering`.

## Bare metal servers known issues and limitations
{: #bare-metal-servers-known-issues-limitations}

### iPXE network boot known timing issue
{: #ipxe-network-boot-known-issue}

**Issue:** When you use the iPXE network boot on a Bare Metal Server on VPC, the network configuration process might still be incomplete when the iPXE script starts running. When this issue occurs, the DHCP command might fail or you might seem a timeout error. A fix for this issue is planned.

**Workaround:** From the VNC console, manually run the iPXE commands. Or, add the following to your iPXE script instead of the DHCP command.

```sh
  :retry_dhcp
  dhcp || goto retry_dhcp
  sleep 2
```
{: codeblock}

**Issue:** Flow log collectors are not integrated with bare metal servers. As a result, if you create a flow log collector for a VPC, traffic that flows to and from bare metal servers in that VPC aren't logged.

**Issue:** Network load balancers are not integrated with bare metal servers. As a result, if you create a network load balancer, you can't target a bare metal server as a load balancer pool member target.

**Issue:** You can't delete a subnet when you delete a bare metal server. Wait ~2 minutes after bare metal deletion before you delete the subnet.

Because all bare metal profiles are VMware&reg; certified, the `supported_image_flags` image property and `required_image_flags` profile property that expressed this ability during the beta period are discontinued. These properties might still be visible to API and CLI consumers, but they aren't supported and must not be used. These properties will be removed entirely in a future release.
{: note}

## Extra authorizations beyond the authorizations defined in the API specification
{: #api-spec-auth-known-issue}

**Issue:** Some API implementations required authorizations that are different from the authorizations requirements that are defined in the [API specification](/apidocs/vpc/latest). The following table lists such APIs and the extra permissions that are required in addition to what is already defined in the specification. This table is updated as these issues are resolved.

| API | Additional access requirements | Action name|
| ----------- | ---------------------------------- | -----------------------------------------|
| `PATCH /instances/{instance-id}`  | Dedicated Host Operator, Dedicated Host Group Operator | `is.dedicated-host.dedicated-host-group.operate` (conditional) \n `is.dedicated-host.dedicated-host.operate` (conditional) |
| `POST /instances` | Subnet Editor| `is.subnet.subnet.update` (conditional) |
| `POST /instances/{instance-id}/actions` | Instance Editor | `is.instance.instance.update` |
| `POST /instances/{instance-id}/volume/_attachments` | Instance Editor | `is.instance.instance.update` |
| `DELETE /instances/{instance-id}/volume_attachments/{vol-attach-id}` | Instance Editor | `is.instance.instance.update` |
| `GET /network_acls/{nacl-id}` | VPC Viewer | `is.vpc.vpc.read` |
| `POST /network_acls/{nacl-id}/rules` | VPC Viewer | `is.vpc.vpc.read` |
| `GET /subnets/{subnet-id}/network_acl` | VPC Viewer | `is.vpc.vpc.read` |
| `PUT /subnets/{subnet-id}/network_acl` | VPC Viewer | `is.vpc.vpc.read` |
| `PATCH /floating_ips/{id}` | Subnet Operator | `is.subnet.subnet.operate` |
{: caption="API additional authorization requirements" caption-side="bottom"}

## Storage known issues
{: #storage-vpc-known-issues}

### Fast restore snapshots with customer-managed encryption issue
{: #snapshots-fast-restore-known-issue}

**Issue:** When you restore a volume from a snapshot by using the fast restore feature, you can use a different encryption key for the snapshot and for the new volume. If you delete the snapshot encryption key from the key management service, the volume might still become inaccessible when it is attached or reattached to a virtual server instance.

**Workaround:** To recover the snapshot encryption key, use [the key recovery procedure](/docs/key-protect?topic=key-protect-restore-keys). When the key is recovered, the volume becomes accessible.

### Volumes and snapshots omit the catalog offering information for unbilled catalog offering versions
{: #volumes-catalog-managed-known-issue}

**Issue:** When you retrieve a volume or snapshot that was originally provisioned as a boot volume in an instance with a [billed catalog offering](/docs/vpc?topic=vpc-getting-started-images-on-vpc-catalog&interface=ui#images-on-vpc-catalog-images) and without a billing plan, the response does not include the `catalog_offering` property.

### File share replication snapshots
{: #replicationsnapshots}

When replication occurs between the source share and its replica, the system creates temporary snapshots in the `.snapshot` directory to support the data synchronization. These system-managed snapshots are named by using the word "replication" and the associated creation timestamp rather than a fingerprint. These snapshots are automatically released and deleted when they are no longer needed. These snapshots are not visible in the console, in the CLI or API responses.

### File share snapshots are not visible in UI, CLI, or API responses for Accessor shares
{: #filesnapshotsonaccessorshare}

Users of Accessor shares have access to all the data of the source share, which includes the snapshots of the file share. Although users of an Accessor share can't see the file share snapshots of the source share in the console, in the CLI or API responses, they can access the snapshots in the `.snapshot` directory of the Accessor share.

### File share snapshot directory visible property in API response
{: #snapshotdirectoryvisibleAPI}

The property `snapshot_directory_visible` is included in the API response for the methods that are listing, creating, deleting, retrieving, or updating a file share. This field is not recommended for use, and it is planned to be removed.

## Creating accessor share with specific resource group fails
{: #accessor-shares-need-default-resource-group}

When you specify a resource group explicitly in your API request to create an accessor share for an origin share, the request fails with 400 Bad Request error. As a workaround you can create the accessor share without specifying a resource group, the system automatically picks the default resource group to create the share.

## When a cross-regional replica is created, the displayed href value of the parent snapshot is incorrect
{: #crr-incorrect-href-source-snapshot}

When you retrieve information about your cross-regional replica share, the source snapshot's href value is incorrect in the API response. Refer to the source snapshot ID or source snapshot CRN instead.

### Backup plan ID property in the API response
{: #backup-policy-plan-fs}

When details of a snapshot are retrieved, the API response shows the property name `backup_plan_id` instead of `backup_policy_plan`. A fix for this issue is planned.

### Multi-volume snapshot creation requests fail to complete if the consistency group contains second-generation volumes
{: #consistency-group-with-mixed-volume-generation-fails}

Multi-volume snapshots are not supported for second-generation volumes. When you try to create a consistency group of snapshots of a mix of first and second-generation volumes, the API request appears successful. However, the new snapshot consistency group ends up in a failed state or gets stuck in the pending state without an error message.

### The Bandwidth property of first-generation volumes profiles incorrectly displays `dependent_range`
{: #gen1-bandwidth-property-dependent}

When details of first-generation volume profiles are retrieved, the responses show the bandwidth type incorrectly as `dependent_range`. The correct value is `dependent` because the bandwidth value is automatically assigned by the system, and that value can't be changed manually or programmatically.

### Block volume snapshots that are taken in Montreal are stored in {{site.data.keyword.cos_short}} in Washington, DC
{: #snapshot-COS-upload-CA-MON-US-EAST}

The Montreal MZR currently does not have a local key management service ({{site.data.keyword.keymanagementserviceshort}}) instance available. Hence, the block volume snapshots that are taken in Montreal are routed to and stored in an encrypted {{site.data.keyword.cos_short}} bucket with local KMS keys in the WDC MZR. When the KMS service becomes available in Montreal, all the snapshots will be moved back to Montreal from Washington DC.
