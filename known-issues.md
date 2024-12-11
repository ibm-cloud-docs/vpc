---

copyright:
  years: 2018, 2024
lastupdated: "2024-12-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

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

**Issue:** If you imported one or more images into a virtual server image for VPC catalog product offering version and you edit that version, an additional version ending in "draft" is created. You can't provision an instance from this draft version. Draft versions might appear on the Virtual server instance creation page in the UI or in the output of the CLI command `ibmcloud is catalog-image-offering`.

## Bare metal servers known issues and limitations
{: #bare-metal-servers-known-issues-limitations}

### iPXE network boot known timing issue
{: #ipxe-network-boot-known-issue}

**Issue:** When you use the iPXE network boot on a Bare Metal Server on VPC, the network configuration might still be processing when the iPXE script starts running. When this issue occurs, the DHCP command might fail or you might seem a timeout error. A fix for this issue is planned.

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

## Additional authorizations beyond those defined in the API specification
{: #api-spec-auth-known-issue}

**Issue:** Some API implementations required authorizations that are different from the authorizations requirements that are defined in the [API specification](/apidocs/vpc/latest). The following table lists such APIs and the extra permissions that are required in addition to what is already defined in the specification. This table is updated as these issues are resolved.

| API | Additional access requirements | Action name|
| ----------- | ---------------------------------- | -----------------------------------------|
| `PATCH /instances/{instance-id}`  | Dedicated Host Operator, Dedicated Host Group Operator | is.dedicated-host.dedicated-host-group.operate (conditional) \n is.dedicated-host.dedicated-host.operate (conditional) |
| `POST /instances` | Subnet Editor| is.subnet.subnet.update (conditional) |
| `POST /instances/{instance-id}/actions` | Instance Editor | is.instance.instance.update |
| `POST /instances/{instance-id}/volume/_attachments` | Instance Editor | is.instance.instance.update |
| `DELETE /instances/{instance-id}/volume_attachments/{vol-attach-id}` | Instance Editor | is.instance.instance.update |
| `GET /network_acls/{nacl-id}` | VPC Viewer | is.vpc.vpc.read |
| `POST /network_acls/{nacl-id}/rules` | VPC Viewer | is.vpc.vpc.read |
| `GET /subnets/{subnet-id}/network_acl` | VPC Viewer | is.vpc.vpc.read |
| `PUT /subnets/{subnet-id}/network_acl` | VPC Viewer | is.vpc.vpc.read |
| `PATCH /floating_ips/{id}` | Subnet Operator | is.subnet.subnet.operate |
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

### Accessor share binding href returned in incorrect format
{: #fileshareaccessorhref}

When a client is retrieving an origin share, the value of the `href` subproperty of the `accessor_bindings` property might be incorrect. A fix for this issue is planned.

### Missing `accessor_binding` and `lifecycle_reasons` in API response
{: #filesharemissingbindinginfo}

Currently, when a client is creating, updating, or retrieving a share, the `accessor_bindings` and `lifecycle_reasons` properties are missing from the response in the `eu-es`, `eu-fr2`, `eu-de`, and `us-south` regions. You can observe this behavior when a file share is created without an accessor share. A fix for this issue is planned.
