---

copyright:
  years: 2023, 2026
lastupdated: "2026-01-26"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for security groups and network ACLs
{: #sg-acl-known-issues}

Known issues are identified bugs or unexpected behaviors that were not fixed before release, but weren’t critical enough to delay it. These issues are communicated to you, often with workarounds, and are prioritized for resolution in the near term by the development team.
{: shortdesc}

Known issues for security groups and network ACLs are as follows:

* Security Group and Network ACL rules with ESP protocol issue:
    * Network traffic with the ESP protocol is currently supported by instances with [generation 2 profiles](/docs/vpc?topic=vpc-profiles#x86-64-instance-profile-families). Instances with newer generation profiles, and all bare metal servers, do not currently support ESP traffic.
    * Configuring a security group rule with a `protocol` value of `esp` or `any` will not allow ESP traffic when the security group targets a network interface for an instance with a newer generation profile or a bare metal server.
    * To avoid confusion about where ESP traffic is supported, the ESP protocol is not shown in the IBM Cloud console options for security group and network ACL rules. Support for ESP traffic on newer generation instance profiles and on bare metal servers may be available in a future release.
* Known issues for vpc-go-sdk: 
    * Security Group rules and Network ACL rules backward compatibility issue
        * **Publication date:** 2025-12-18
        * **Affected component:** vpc-go-sdk
        * **Affected operations:** Security group rules and Network ACL rules 
        * **Issue Summary:** Following the new support for [all IPv4 protocols for ACL and Security Group rules](https://cloud.ibm.com/docs/vpc?topic=vpc-release-notes#vpc-dec1225), earlier versions of the Golang SDK must be updated to avoid the following parsing error when handling rules with the new protocols:
            ```text
            error unmarshalling vpcv1.SecurityGroupCollection: error unmarshalling property 'security_groups' as []vpcv1.SecurityGroup: error unmarshalling property 'rules' as []vpcv1.SecurityGroupRuleIntf: unrecognized value for discriminator property 'protocol': any
            ```
        * The patched SDKs implement the correct fallback behavior and error identifiers with the correct model name (for example, NetworkACLRule instead of NetworkACLRuleItem).
        * **Migration and mitigation:** To mitigate this issue, migrate the `vpc-go-sdk` to the latest version ([v0.78.0](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.78.0)) or any of the following patched versions: [v0.77](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.77.0), [v0.76](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.76.5), [v0.75](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.75.1), [v0.74](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.74.2), [v0.73](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.73.1), [v0.72](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.72.1), [v0.71](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.71.2), [v0.70](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.70.2), [v0.69](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.69.2), or [v0.68](https://github.com/IBM/vpc-go-sdk/releases/tag/v0.68.1).
