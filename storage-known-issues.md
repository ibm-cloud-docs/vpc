---

copyright:
  years: 2018, 2026
lastupdated: "2026-02-24"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for storage services
{: #storage-known-issues}

These issues are temporary and are scheduled to be addressed in upcoming releases.
{: shortdesc}

## Block Storage volumes and snapshots
{: #block-storage-known-issue}

### Volumes and snapshots omit the catalog offering information for unbilled catalog offering versions
{: #volumes-catalog-managed-known-issue}

**Issue:** When you retrieve a volume or snapshot that was originally provisioned as a boot volume in an instance with a [billed catalog offering](/docs/vpc?topic=vpc-getting-started-images-on-vpc-catalog&interface=ui#images-on-vpc-catalog-images) and without a billing plan, the response does not include the `catalog_offering` property.

### The Bandwidth property of first-generation volumes profiles incorrectly displays `dependent_range`
{: #gen1-bandwidth-property-dependent}

When details of first-generation volume profiles are retrieved, the responses show the bandwidth type incorrectly as `dependent_range`. The correct value is `dependent` because the bandwidth value is automatically assigned by the system, and that value can't be changed manually or programmatically.

### Block volume snapshot is greater in the remote region than the original snapshot
{: #snapshot-CRC-billing}

The first time that you create a cross-regional copy, that snapshot is a full copy of the parent volume's data. Subsequent copies can be incremental or full copies. Whether the remote copy is incremental depends on the immediately preceding snapshot in the chain. If the immediately preceding snapshot exists in the destination region, the copy can be incremental. If the immediately preceding snapshot is not found or it's not stable in the remote region, a new full-copy is created. When a full remote copy is generated from an incremental snapshot, it creates a discrepancy in the billing.

### Snapshot encryption in regional {{site.data.keyword.cos_short}} in Indian MZRs
{: #snapshot-COS-upload-IN-CHE-EU-GB}

A local {{site.data.keyword.keymanagementserviceshort}} instance is not available in Chennai - Airtel. First-generation block volume snapshots that are taken in Chennai - Airtel are routed to a regional {{site.data.keyword.cos_short}} bucket that is encrypted by using a {{site.data.keyword.keymanagementserviceshort}} instance from the London (`eu-gb`) region temporarily. When the KMS service becomes available in Chennai - Airtel, the snapshots service will switch to use the local {{site.data.keyword.keymanagementserviceshort}} instance for encryption, so both storage and key management are handled within the same region.

### Cross-regional copy of block storage snapshots in Chennai - Airtel
{: #snapshot-CRC-IN-CHE}

A cross-regional copy of block storage volume snapshots is not supported in the Chennai - Airtel region. It can't be selected as a source or target region.

### Creating a volume from a second-generation snapshot that has provider-managed encryption fails when customer-managed encryption is specified for the volume
{: #gen2-volume-from-snapshot-fail}

When a second-generation snapshot with provider-managed encryption is selected to create a volume with customer-managed keys, the volume provisioning gets stuck in `pending` status. When you restore a volume from a second-generation snapshot, make sure that the encryption type of the new volume matches the encryption type of the snapshot.

## File Storage shares and snapshots
{: #file-storage-known-issue}

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

### File share `accessor_bindings` missing in share API response
{: #fs-missing-accessor-info-api}

When creating, retrieving, listing, updating, or deleting file shares, `accessor_bindings` may be absent from the share API response.

### File share `more_info` does not return URL of issue
{: #fs-missing-more-info-url}

When an error is reported while making share API requests, the `more_info` property does not return an error topic URL for the issue encountered. The `more_info` property returns information on how to resolve the issue encountered instead.

### File share properties missing in API response
{: #file-share-properties-missing-in-api-response}

[Select availability]{: tag-green}

When using an API `version` query parameter of `2025-09-15` or earlier, the following properties might be missing or incorrect from the response:

- `zone` might be absent in the share API response when file shares and file share snapshots with `rfs` profile from a source share are created, retrieved, listed, updated, or deleted by using an API version of `2025-09-15` or earlier. When a `version` query parameter of `2025-09-15` or earlier is used, the `zone` of an `rfs` share snapshot returns the first zone from the region and is informational only. `zone` is not affected for `dp2` share snapshots and is represented correctly for `rfs` share snapshots when an API version of `2025-09-16` or later is used.

### Creating replica file shares with `allowed_transit_encryption_modes` or mount targets with `transit_encryption`
{: #replica-file-share-transit-encryption}

[Select availability]{: tag-green}

When you try to create a replica file share with the `allowed_transit_encryption_modes` option specified, the request fails. Additionally, creating a replica file share with a mount target without a `transit_encryption` value in the source share's `allowed_transit_encryption_modes` property fails. This behavior is incorrect. To work around this issue, do not specify `allowed_transit_encryption_modes` in requests to create a replica file share. The replica's `allowed_transit_encryption_modes` are inherited from the source share. When you want to create a mount target for a replica file share, use only the `transit_encryption` values that are specified in the source share's `allowed_transit_encryption_modes` property.

### Regional file share mount target provisioning delays
{: #regional-file-share-mount-target-provisioning}

[Select availability]{: tag-green}

Creating a share mount target for a regional file share can take more than 10 minutes, during which time its `lifecycle_state` shows as `pending`. Mount targets for shares that use the `dp2` profile are not affected.

### Regional file share snapshot size not reported accurately in API and CLI responses
{: #regional-file-share-snapshot-size-incorrect}

When performing file share operations with the CLI or API, the snapshot size field defaults to 1 in the response when a snapshot is created and to 0 otherwise. This value does not represent the actual size of the snapshot for regional shares.

### Cross-regional replication for zonal file shares in Chennai - Airtel
{: #zonalfileshare-CRR-IN}

Cross-regional replication for zonal file shares is not supported in the Chennai - Airtel  region.

## Backup for VPC service
{: #backup-service-known-issue}

### Backup plan ID property in the API response
{: #backup-policy-plan-fs}

When details of a snapshot are retrieved, the API response shows the property name `backup_plan_id` instead of `backup_policy_plan`. A fix for this issue is planned.

### Multi-volume backup creation requests create consistency group snapshots without second-generation volumes
{: #consistency-group-with-mixed-volume-generation-fails}

Multi-volume snapshots are not supported for second-generation volumes. When you try to create a consistency group of snapshots of a mix of first and second-generation volumes, the API request appears successful as snapshots of Gen 1 volumes are created. However, the Gen 2, `sdp` volumes are skipped.

### Private context-based restriction rules for Backups are not working in Montreal (`ca-mon`) and Chennai - Airtel (`in-che`) MZRs.
{: #baas-CBR-issue-MON}

Enabling private CBR rules for backup operations that create and manage automated snapshots of block volumes and file shares in Montreal and Chennai - Airtel is not supported.
