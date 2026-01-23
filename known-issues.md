---

copyright:
  years: 2018, 2026
lastupdated: "2026-01-23"

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

## Networking known issues
{: #networking-vpc-known-issues}

### Security Group and Network ACL rules support for any protocol including issues with older SDKs, CLIs and Terraform providers
{: #security-groups-network-acls-protocol-old-sdk-cli-tf-issue}

**Warning**: Do NOT create a rule with a previously unsupported `protocol` value in your production environments until further notice. Creating a rule with these new protocol values anywhere in your VPC can break your existing Kubernetes and OpenShift clusters. They can also break any other components that make security group or network ACL API requests via an SDK, or via a custom API client that does not handle these new protocol values properly. Work is ongoing to resolve these issues. Until the known issues have been resolved, the UI and the current latest version of CLI will not allow rules with previously unsupported `protocol` values to be created.
{: important}

When a security group or network ACL rule is created with a `protocol` value that was previously unsupported, there is an issue with old versions of some tools that use the API. The following tools may experience an error (crash) when retrieving or listing rules with a previously unsupported `protocol` value:
- The [IBM Cloud VPC Go SDK](https://github.com/IBM/vpc-go-sdk). For troubleshooting information, see the [known issues](https://github.com/IBM/vpc-go-sdk/blob/master/known-issues.md).
- The `vpc-infrastructure` plugin for the [IBM Cloud CLI](/docs/cli). To prevent errors, update to the latest version of the `vpc-infrastructure` plugin.

### Security Group and Network ACL rules with ESP protocol issue
{: #security-groups-network-acls-protocol-esp-known-issue}

Network traffic with the ESP protocol is currently supported by instances with [generation 2 profiles](/docs/vpc?topic=vpc-profiles#x86-64-instance-profile-families). Instances with newer generation profiles, and all bare metal servers, do not currently support ESP traffic.

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

```text
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
