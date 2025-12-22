---

copyright:
  years: 2018, 2025
lastupdated: "2025-12-22"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

## VPC Metadata API known issues
{: #metadata-api-known-issues}

### `vcpu.manufacturer` property returns an empty string value
{: #vcpu-manufacturer-instance-metadata-api-known-issues}

**Issue:** When [retrieving an instance](/apidocs/vpc-metadata#get-instance), the value of the `vcpu.manufacturer` property is an empty string `""`.

## VPC Identity API known issues
{: #identity-api-known-issues}

### The `/instance_identity` methods return incorrect HTTP status
{: #identity-api-incorrect-http-status-known-issues}

**Issue:** When a `version` query parameter of `2025-08-25` or earlier is used from bare metal servers, an incorrect HTTP response of `404` is returned for `/instance_identity` methods that are used to [create an identity token](/apidocs/vpc-identity/latest#create-identity-token), [create an identity certificate](/apidocs/vpc-identity/latest#create-identity-certificate), and [create an IAM token](/apidocs/vpc-identity/latest#create-identity-iam-token). The behavior is correct when a `version` query parameter of `2025-08-26` or later is used.

When a beta `version` query parameter of `2025-07-14` or earlier from bare metal servers is used, an incorrect HTTP response of `404` is returned for all `/instance_identity` methods.

## Confidential computing known issues
{: #confidential-computing-vpc-known-issues}

[Select availability]{: tag-green}

### TDX virtual servers are supported in Washington DC (us-east) and Frankfurt (eu-de) regions only
{: #tdx-wdc-confidential-computing-vpc-known-issues}

**Issue:** All confidential computing profiles support both Intel&reg; Software Guard Extensions (SGX) and Intel&reg; Trusted Domain Extensions (TDX). When you use the API to list instance profiles, such as with `GET /instance/profiles` or `GET /instance/profiles/{name}`, the response indicates that all confidential computing profiles support SGX and TDX. However, TDX is available in the Washington DC (us-east) and Frankfurt (eu-de) regions only. If you want to create a virtual server instance with a confidential computing profile and TDX, you can create that virtual server instance in only the Washington DC (us-east) and Frankfurt (eu-de) regions. You can’t create a virtual server instance with TDX in any other region, including Dallas (us-south).

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

**Issue:** If you imported one or more images into a virtual server image for VPC catalog product offering version and you edit that version, an extra version that ends in "draft" is created. You can't provision an instance from this draft version. Draft versions might appear on the virtual server instance creation page in the console or in the output of the CLI command `ibmcloud is catalog-image-offering`.

## Instance known issues and limitations
{: #instance-known-issues-limitations}

### Default profile on `POST /instances`
{: #default-profile-post-instances}

**Issue:** When an instance is created without a `profile` specified by the user, it defaults to the `bx2-2x8` profile instead of `bxf-2x8`.

## Bare metal servers known issues and limitations
{: #bare-metal-servers-known-issues-limitations}

### iPXE network boot known timing issue
{: #ipxe-network-boot-known-issue}

**Issue:** When you use the iPXE network boot on a Bare Metal Server on VPC, the network configuration process might still be incomplete when the iPXE script starts running. When this issue occurs, the DHCP command might fail or you might see a timeout error. A fix for this issue is planned.

**Workaround:** From the VNC console, manually run the iPXE commands. Or, add the following content to your iPXE script instead of the DHCP command.

```sh
  :retry_dhcp
  dhcp || goto retry_dhcp
  sleep 2
```
{: codeblock}

**Issue:** Flow log collectors are not integrated with bare metal servers. As a result, if you create a flow log collector for a VPC, traffic that flows to and from bare metal servers in that VPC aren't logged.

**Issue:** Network load balancers are not integrated with bare metal servers. As a result, if you create a network load balancer, you can't target a bare metal server as a load balancer pool member target.

**Issue:** You can't delete a subnet when you delete a bare metal server. Wait ~2 minutes after bare metal deletion before you delete the subnet.

Because all bare metal profiles are VMware&reg; certified, the `supported_image_flags` image property and `required_image_flags` profile property that expressed this ability during the beta period are discontinued. These properties might still be visible to API and CLI consumers, but they aren't supported and must not be used. These properties are planned to be removed in a future release.
{: note}

## Extra authorizations beyond the authorizations defined in the API specification
{: #api-spec-auth-known-issue}

**Issue:** Some API implementations require authorizations that are different from the authorization requirements that are defined in the [API specification](/apidocs/vpc/latest). The following table lists these APIs and the extra permissions that are required that are in addition to what are defined in the specification. This table is updated as these issues are resolved.

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

### Backup plan ID property in the API response
{: #backup-policy-plan-fs}

When details of a snapshot are retrieved, the API response shows the property name `backup_plan_id` instead of `backup_policy_plan`. A fix for this issue is planned.

### Multi-volume backup creation requests create consistency group snapshots without second-generation volumes
{: #consistency-group-with-mixed-volume-generation-fails}

Multi-volume snapshots are not supported for second-generation volumes. When you try to create a consistency group of snapshots of a mix of first and second-generation volumes, the API request appears successful as snapshots of Gen 1 volumes are created. However, the Gen 2, `sdp` volumes are skipped.

### The Bandwidth property of first-generation volumes profiles incorrectly displays `dependent_range`
{: #gen1-bandwidth-property-dependent}

When details of first-generation volume profiles are retrieved, the responses show the bandwidth type incorrectly as `dependent_range`. The correct value is `dependent` because the bandwidth value is automatically assigned by the system, and that value can't be changed manually or programmatically.

### Private context-based restriction rules for Backups are not working in Montreal (`ca-mon`) and Chennai (`in-che`) MZRs.
{: #baas-CBR-issue-MON}

Enabling private CBR rules for backup operations that create and manage automated snapshots of block volumes and file shares in Montreal and Chennai is not supported.

### Block volume snapshot is greater in the remote region than the original snapshot
{: #snapshot-CRC-billing}

The first time that you create a cross-regional copy, that snapshot is a full copy of the parent volume's data. Subsequent copies can be incremental or full copies. Whether the remote copy is incremental depends on the immediately preceding snapshot in the chain. If the immediately preceding snapshot exists in the destination region, the copy can be incremental. If the immediately preceding snapshot is not found or it's not stable in the remote region, a new full-copy is created. When a full remote copy is generated from an incremental snapshot, it creates a discrepancy in the billing.

### Snapshot encryption in regional {{site.data.keyword.cos_short}} in Chennai region
{: #snapshot-COS-upload-IN-CHE-EU-GB}

A local {{site.data.keyword.keymanagementserviceshort}} instance is not available in Chennai. First-generation block volume snapshots that are taken in Chennai are routed to a regional {{site.data.keyword.cos_short}} bucket that is encrypted by using a {{site.data.keyword.keymanagementserviceshort}} instance from the London (`eu-gb`) region temporarily. When the KMS service becomes available in Chennai, the snapshots service will switch to use the Chennai-based {{site.data.keyword.keymanagementserviceshort}} instance for encryption, so both storage and key management are handled within the same region.

### Cross-regional replication for zonal file shares in Chennai
{: #zonalfileshare-CRR-IN-CHE}

Cross-regional replication for zonal file shares is not supported in the Chennai region.

### Cross-regional copy of block storage snapshots in Chennai
{: #snapshot-CRC-IN-CHE}

A cross-regional copy of block storage volume snapshots is not supported in the Chennai region. It can't be selected as a source or target region.

## Networking known issues
{: #networking-vpc-known-issues}

### Security Group and Network ACL rules support for any protocol including issues with older SDKs, CLIs and Terraform providers
{: #security-groups-network-acls-protocol-old-sdk-cli-tf-issue}

**Warning**: Do NOT create a rule with a previously unsupported `protocol` value in your production environments until further notice. Creating a rule with these new protocol values anywhere in your VPC can break your existing Kubernetes and OpenShift clusters. They can also break any other components that make security group or network ACL API requests via an SDK, or via a custom API client that does not handle these new protocol values properly. Work is ongoing to resolve these issues. Until the known issues have been resolved, the UI and the current latest version of CLI will not allow rules with previously unsupported `protocol` values to be created.
{: important}

When a security group or network ACL rule is created with a `protocol` value that was previously unsupported, there is an issue with old versions of some tools that use the API. The following tools may experience an error (crash) when retrieving or listing rules with a previously unsupported `protocol` value:
- The [IBM Cloud VPC Go SDK](https://github.com/IBM/vpc-go-sdk). For troubleshooting information, see the [known issues](https://github.com/IBM/vpc-go-sdk/blob/master/known-issues.md).
- The `vpc-infrastructure` plugin for the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli). To prevent errors, update to the latest version of the `vpc-infrastructure` plugin.

### Security Group and Network ACL rules with ESP protocol issue
{: #security-groups-network-acls-protocol-esp-known-issue}

Network traffic with the ESP protocol is currently supported by instances with [generation 2 profiles](/docs/vpc?topic=vpc-profiles&interface=api#profiles-generation). Instances with newer generation profiles, and all bare metal servers, do not currently support ESP traffic.

Configuring a security group rule with a `protocol` value of `esp` or `any` will not allow ESP traffic when the security group targets a network interface for an instance with a newer generation profile or a bare metal server.

To avoid confusion about where ESP traffic is supported, the ESP protocol is not shown in the IBM Cloud console options for security group and network ACL rules. Support for ESP traffic on newer generation instance profiles and on bare metal servers may be available in a future release.

## Known Issues for vpc-go-sdk
{: #vpc-go-sdk-known-issues}

### Security Group rules and Network ACL rules backward compatibility issue
{: #security-groups-network-acls-rule-descriminator-issue}

**Publication date:** 2025-12-18

**Affected component:** vpc-go-sdk

**Affected operations:** Security group rules and Network ACL rules 

**Issue Summary:** Following the new support for [all IPv4 protocols for ACL and Security Group rules](https://cloud.ibm.com/docs/vpc?topic=vpc-release-notes#vpc-dec1225), earlier versions of the Golang SDK must be updated to avoid the following parsing error when handling rules with the new protocols:

```
error unmarshalling vpcv1.SecurityGroupCollection: error unmarshalling property 'security_groups' as []vpcv1.SecurityGroup: error unmarshalling property 'rules' as []vpcv1.SecurityGroupRuleIntf: unrecognized value for discriminator property 'protocol': any
```

The patched SDKs implement the correct fallback behavior and error identifiers with the correct model name (for example, NetworkACLRule instead of NetworkACLRuleItem).

**Migration and mitigation:** To mitigate this issue, migrate the `vpc-go-sdk` to the latest version ([v0.78.0](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.78.0)) or any of the following patched versions:

[v0.77](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.77.0),
[v0.76](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.76.5),
[v0.75](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.75.1),
[v0.74](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.74.2),
[v0.73](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.73.1),
[v0.72](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.72.1),
[v0.71](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.71.2),
[v0.70](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.70.2),
[v0.69](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.69.2), or
[v0.68](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.68.1).
