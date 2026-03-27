---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-27"

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

### TDX virtual servers are supported in Dallas (us-south), Washington DC (us-east), Frankfurt (eu-de) regions only
{: #tdx-wdc-confidential-computing-vpc-known-issues}

**Issue:** All confidential computing profiles support both Intel&reg; Software Guard Extensions (SGX) and Intel&reg; Trusted Domain Extensions (TDX). When you use the API to list instance profiles, such as with `GET /instance/profiles` or `GET /instance/profiles/{name}`, the response indicates that the confidential computing profiles are supported in the Dallas (us-south), Washington DC (us-east), Frankfurt (eu-de), Madrid (eu-es) and London (eu-gb) regions only. If you want to create a virtual server instance with a confidential computing profile, you can create that virtual server instance in only the Dallas (us-south), Washington DC (us-east) and Frankfurt (eu-de) regions. You can’t create a confidential computing virtual server instance in any other region, including Madrid (eu-es), London (eu-gb).

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
