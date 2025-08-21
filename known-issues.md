---

copyright:
  years: 2018, 2025
lastupdated: "2025-08-19"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

## Metadata API known issues
{: #metadata-api-known-issues}


### `vcpu.manufacturer` property returns an empty string value
{: #vcpu-manufacturer-instance-metadata-api-known-issues}

**Issue:** When [retrieving an instance](/apidocs/vpc-metadata#get-instance) the value of the `vcpu.manufacturer` property is an empty string `""`.

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

### Creating accessor share with specific resource group fails
{: #accessor-shares-need-default-resource-group}

When you specify a resource group explicitly in your API request to create an accessor share for an origin share, the request fails with 400 Bad Request error. As a workaround you can create the accessor share without specifying a resource group, the system automatically picks the default resource group to create the share.

### When a cross-regional replica is created, the displayed href value of the parent snapshot is incorrect
{: #cross-regional-replica-incorrect-href-source-snapshot}

When you retrieve information about your cross-regional replica share, the source snapshot's href value is incorrect in the API response. Refer to the source snapshot ID or source snapshot CRN instead.

### Regional file share mount targets and `us-south-dal14-a` availability zone
{: #regional-file-share-mount-us-south-dal14-a}

[Beta]{: tag-cyan}

Creating a mount target for a regional share in any of the subnets in `us-south-dal14-a` is not supported in the beta release. Provisioning attempts in that zone do not complete successfully. Instead, the mount target stays in the `lifecycle_state` of `pending`. Regional file shares in the `us-south` region cannot be mounted on virtual server instances that are provisioned in the `us-south-dal14-a` zone by using a mount target in a different zone.

You can determine whether one of your VPC zones is mapped to `us-south-dal14-a` by using the [IBM Cloud console](/infrastructure), [CLI](/docs/vpc?topic=vpc-vpc-reference#zones), or the [VPC API](/apidocs/vpc#list-region-zones). For more information, see [Zone mapping per account](/docs/overview?topic=overview-locations#zone-mapping).

### File share properties missing in API response
{: #file-share-properties-missing-in-api-response}

[Beta]{: tag-cyan}

In an API response, the following properties might be missing or incorrect:

- `allowed_access_protocols` might not be included in the share API response when files shares are retrieved, listed, or updated.
- `storage_generation` might not be included in the share profile API response when file share profiles are listed or retrieved. The `storage_generation` for file share profiles is `2` for the `rfs` profile and `1` for all other profiles.
- `zone` might be absent in the share API response when file shares with `rfs` profile are created, retrieved, listed, updated, or deleted by using an API version of `2025-07-21` or earlier. Also, when a `version` query parameter of `2025-07-21` or earlier is used, the `zone` of an `rfs` share returns the first zone from the region and is informational only. `zone` is not affected for `dp2` shares and is represented correctly for `rfs` shares when an API version of `2025-07-22` or later is used.

### Updating a regional file share to `vpc` access control mode succeeds
{: #regional-file-share-vpc-access-control-mode}

[Beta]{: tag-cyan}

Although regional file shares do not support the `vpc` access control mode, requests to update a regional file share to use an `access_control_mode` of `vpc` can erroneously appear to succeed. However, the resulting file share remains in `pending` state.

### Creating replica file shares with `allowed_transit_encryption_modes` or mount targets with `transit_encryption`
{: #replica-file-share-transit-encryption} 

[Beta]{: tag-cyan}

When you try to create a replica file share with the `allowed_transit_encryption_modes` option specified, the request fails. Additionally, creating a replica file share with a mount target without a `transit_encryption` value in the source share's `allowed_transit_encryption_modes` property fails. This behavior is incorrect. To work around this issue, do not specify `allowed_transit_encryption_modes` in requests to create a replica file share. The replica's `allowed_transit_encryption_modes` are inherited from the source share. When you want to create a mount target for a replica file share, use only the `transit_encryption` values that are specified in the source share's `allowed_transit_encryption_modes` property.

### File share bandwidth for `dp2` file shares is incorrectly reported
{: #dp2-file-share-bandwidth-incorrectly-reported}

[Beta]{: tag-cyan}

For `dp2` profile shares, the `bandwidth` returned in the API response is incorrectly reported as `1` Mbps in the file share. This value is calculated based on the share's selected `iops` value and does not affect `dp2` file share's QoS.

### Regional file share mount target provisioning delays
{: #regional-file-share-mount-target-provisioning}

[Beta]{: tag-cyan}

Creating a share mount target for a regional file share can take more than 10 minutes, during which time its `lifecycle_state` shows as `pending`. Mount targets for shares that use the `dp2` profile are not affected.

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

Due to the unavailability of a local key management service ({{site.data.keyword.keymanagementserviceshort}}) instance in Montreal, the block volume snapshots that are taken in Montreal are routed to and stored in an encrypted {{site.data.keyword.cos_short}} bucket with local KMS keys in the WDC MZR. When the KMS service becomes available in Montreal, all the snapshots will be moved back to Montreal from Washington DC.

### Block volume snapshot is greater in the remote region than the original snapshot
{: #snapshot-CRC-billing}

The first time that you create a cross-regional copy, that snapshot is a full copy of the parent volume's data. Subsequent copies can be incremental or full copies. Whether the remote copy is incremental depends on the immediately preceding snapshot in the chain. If the immediately preceding snapshot exists in the destination region, the copy can be incremental. If the immediately preceding snapshot is not found or it's not stable in the remote region, a new full-copy is created. When a full remote copy is generated from an incremental snapshot, it creates a discrepancy in the billing.
