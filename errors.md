---

copyright:
  years: 2018, 2021
lastupdated: "2021-11-20"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}

# API error messages
{: #rias-error-messages}

When you receive an error message from the {{site.data.keyword.cloud}} Virtual Private Cloud APIs, you can use the message ID to find more information about how to resolve the problem.
{: shortdesc}

This API error messages topic is deprecated. Use the information in this topic only if you are linked to a specific message ID from an API error message. 
{: deprecated}

## account_disabled
{: #account-disabled}

**Message**: The account requesting this resource is disabled.

Create a new account.

## account_missing
{: #account-missing}

**Message**: The account in the token was empty or did not exist in the request.

Recreate a token with a valid account and try again.

## account_setup_incomplete
{: #account-setup-incomplete}

**Message**: The account setup is not complete.
The account is on a Pay-As-You-Go plan but setup is not complete.  Please try again later.

## account_type_invalid
{: #account-type-invalid}

**Message**: You must have a Pay-As-You-Go account to provision a Virtual Private Cloud.

Your account must be on a Pay-As-You-Go plan to provision VPCs. Refer to [Account types](/docs/account?topic=account-accounts) for instructions on how to upgrade your account.

## acl_in_use
{: #acl-in-use}

**Message**: The network ACL cannot be deleted because it is attached to resources.

The network ACL cannot be deleted because it is attached to a subnet or VPC.

To see if a subnet is using the network ACL, use the `GET /v1/subnets?version=$version&generation=1` API. Equivalent CLI command: `ibmcloud is subnets`.  To update the network ACL used by a subnet use the `PATCH /v1/subnets/{subnet_id}?version=$version&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API or `ibmcloud is subnet-update` CLI.

To see if a VPC is using the network ACL, use the `GET /v1/vpcs?version=$version&generation=1` API. Equivalent CLI command: `ibmcloud is vpcs`. It is not possible to update or delete the default network ACL used by a VPC. The default network ACL is deleted automatically when the VPC gets deleted.

## acl_rule_does_not_allow
{: #acl-rule-does-not-allow}

**Message**: An ACL rule on the subnet does not allow this action.

A network ACL rule would block the necessary network connections that are needed to perform the desired action.

If you are trying to create an instance, ensure that your subnet's ACL rules allow connectivity to all addresses in `161.26.0.0/16` for tcp and udp on source/destination ports `53`, `1688`, `80`, `443`, and `8443`.

## address_prefix_conflict
{: #address-prefix-conflict}

**Message**: The address prefix with this CIDR is in use.

The CIDR for the requested address prefix conflicts with an existing address prefix.

To view the list of address prefixes for a VPC, use the `GET /vpcs/{vpc-id}/address_prefixes` API and check that the requested CIDR address is not in use by the `cidr` field of another address prefix.
Equivalent CLI command: `ibmcloud is vpc-address-prefix`

## address_prefix_in_use
{: #address-prefix-in-use}

**Message**: Cannot delete an address prefix in use.

One or more subnets are using the address prefix.  To determine which subnets are using the address prefix, use the `GET /v1/subnets?version=$version&generation=1` API command and  check the `ipv4_cidr_block` field.  Equivalent CLI command:  `ibmcloud is subnets` and check the `Subnet CIDR` column. You must delete all the subnets using the address prefix before the address prefix can be deleted.

## backend_service_unavailable
{: #backend-service-unavailable}

**Message**: The back-end service is unavailable.

A back-end cloud service that is used by VPC failed to respond. VPC uses multiple IBM Cloud services, such as:

- [Identity and Asset Management](/docs/account?topic=account-iamoverview) (IAM)
- [Global Catalog](https://{DomainName}/catalog){: external}
- Resource Controller
- Resource Manager

You can check [status](https://{DomainName}/status){: external} of IBM Cloud services and try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_field
{: #bad-field}

**Message**: Correct instance UUID should be provided

One of the values provided in the request is incorrect. Check the `target` value in the error returned for clues as to which parameter was incorrect. In some cases, the UUID or volume in the request cannot be found. Provide a valid value and try again.

Refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for additional help. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_request
{: #bad-request}

**Message**: The information given was invalid, malformed, or missing a required field.

The request was not what we expected. Use the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} to help you format the request.

If you are following the specification but still get the error, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## classic_access_vpc_conflict_duplicate_res
{: #classic-access-vpc-conflict-duplicate-res}

**Message**: Only one VPC with Classic Access can be created per region.

Only one VPC with Classic Access can be created per region. To list the VPC with Classic Access, run `ibmcloud is vpcs --classic-access`. The existing VPC with Classic Access must be deleted before another one with Classic Access can be created.

## classic_access_vpc_account_not_VRF_enabled
{: #classic-access-vpc-account-not-VRF-enabled}

**Message**: Account must be VRF-enabled to create a classic access VPC.

The linked classic account is not VRF-enabled. A classic access VPC requires the linked classic account to be VRF-enabled. To enable your account for VRF, refer to [VRF on IBM Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc#vrf-conversion).

## cos_file_too_large
{: #cos-file-too-large}

**Message**: The file from Cloud Object Storage is greater than 100 GB.

The file specified is too large. Only files less than 100 GB are allowed.

## cos_invalid_file_extension
{: #cos-invalid-file-extension}

**Message** The URI provided contains an unsupported file extension.

The file extension in the URI must be **.vhd**.

## cos_invalid_qcow2_format
{: #cos-invalid-qcow2-format}

**Message** The image provided contains an invalid qcow2 formatted image.

Even though the import image file name has a **.qcow2** extension, the contents is not valid.

## cos_not_authorized
{: #cos-not-authorized}

**Message**: You are not authorized to access the Cloud Object Storage resource.

This error occurs if a user tries to import an image from a Cloud Object Storage to which they do not have access. Use the [Granting access between services](/docs/account?topic=account-serviceauth){: external} to help you grant authorization from the image service to the Cloud Object Storage bucket.

## cos_not_found
{: #cos-not-found}

**Message**: Cloud Object Storage resource not found.

The Cloud Object Storage resource specified could not be found. Verify that the resource identification is correct.

## cos_region_mismatch
{: #cos-region-mismatch}

**Message**: Cloud Object Storage resource not found in the target region.

The Cloud Object Storage bucket specified could not be found in the target region. Make sure the bucket exists in the target region.

## cos_scheme_uri_invalid
{: #cos-scheme-uri-invalid}

**Message**: The Cloud Object Storage URI provided is not in the correct format.

This error occurs if the URI has an incorrect scheme. The URI should follow this format: `cos://<region>/<bucket>/<sourceFile>`

## cos_uri_not_supported
{: #cos-uri-not-supported}

**Message**: The Cloud Object Storage URI that was specified is not supported.

A Cloud Object Storage URI cannot specify a single data center, such as ams03. A Cloud Object Storage URI cannot have an image nested within a folder in the file path, for example: cos://us-south/myBucket/MyFolder/myImage.vhd.

The following example shows a Cloud Object Storage URI with a supported format: cos://us-south/myBucket/myImage.vhd.

## cos_image_not_encrypted
{: #cos-image-not-encrypted}

**Message**: Encryption keys are specified in the image template, but the provided image is not encrypted.

Either remove the encryption keys, or specify an encrypted image in the image template and try again.

## cos_image_encrypted
{: #cos-image-encrypted}

**Message**: No encryption key is specified in the image template, but the provided image is encrypted.

Either specify the encryption keys, or specify an unencrypted image in the image template and try again.

## cos_encrypted_image_not_supported
{: #cos-encrypted-image-not-supported}

**Message**: The encrypted image feature is not supported in the current configuration.

Verify that the image is not encrypted, or remove the encryption keys in the image template and try again.

## default_address_prefix_not_found
{: #default-address-prefix-not-found}

**Message**: Default address prefix not found.

You might see this error message if the default address prefix is not found.

## duplicate_error
{: #duplicate-error}

**Message**: The input provided already exists.

The resource specified already exists. Try using a different name for the resource you want to create. For example, when creating a new VPC, you can list existing VPCs by running `ibmcloud is vpcs`, and choose a name that is not shown.

## field_no_longer_supported
{: #field-no-longer-supported}

**Message**: The field is no longer supported and should not be supplied.

The field provided was once supported but is no longer supported in the version specified. Look at the `target` values to determine which field is no longer supported.

For details, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## floating_ip_in_use
{: #floating-ip-in-use}

**Message**: The floating IP is in use.

This floating IP is associated with a virtual server instance or public gateway.

To see where the floating IP is used, use the `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=$version` API and look at the "target" values. Equivalent CLI command: `ibmcloud is floating-ip FLOATINGIP_ID`.

## gateway_too_many
{: #gateway-too-many}

**Message**: Can only have one public gateway per zone in a VPC.

Only one public gateway per zone is allowed in a VPC but the one public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the GET public_gateways API and look at the "vpc" and "zone" values. If using the CLI, you can run the `ibmcloud is public-gateways` command and see the "VPC" and "Zone" value.

Use the public gateway's ID to attach it to a subnet, see an example in our [API examples](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#attach-public-gateway-to-the-subnet). If using the CLI, you can use the subnet update command to attach it to a public gateway, for example, `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`.

## health_monitor_invalid_delay
{: #health-monitor-invalid-delay}

**Message**: The specified health monitor delay <health_monitor_delay> is invalid

Provide a value between 2 and 60 for the parameter `health monitor delay`.

## health_monitor_invalid_max_retries
{: #health-monitor-invalid-max-retries}

**Message**:  The specified health monitor max retries <health_monitor_max_retries> is invalid

Provide a value between 1 and 10 for `health max retries`.

## health_monitor_invalid_timeout
{: #health-monitor-invalid-timeout}

**Message**:  The specified health monitor timeout <health_monitor_timeout> is invalid.

Provide a value between 1 and 59 for `health timeout`. The selected value must be less than the value of `health monitor delay`.

## health_monitor_invalid_url_path
{: #health-monitor-invalid-url-path}

**Message**: The specified health URL path <health_monitor_url> is invalid.

Provide a valid URL for the health monitor. The URL may contain alphanumeric characters, hyphens, and periods. No spaces or backslashes are allowed. The maximum length of the URL path is 255 characters.

## health_monitor_invalid_protocol
{: #health-monitor-invalid-protocol}

**Message**: The specified health monitor protocol <health_monitor_protocol> is invalid.

Provide a valid value for the health monitor protocol. Acceptable values are `http` and `tcp`.

## health_monitor_missing_protocol
{: #health-monitor-missing-protocol}

**Message**:  Health monitor protocol is missing

Health monitor protocol is a required field. In your request, specify the health monitor protocol. The acceptable values are `http` and `tcp`.

## health_monitor_missing_url_path
{: #health-monitor-missing-url-path}

**Message**:  Health monitor URL path is missing. It is required for a HTTP type health monitor.

For a health monitor using HTTP protocol, health monitor URL path is required field. Provide a valid URL path.

## health_monitor_timeout_delay_conflict
{: #health-monitor-timeout-delay-conflict}

**Message**:  Health Monitor timeout <health_monitor_timeout> should not be greater than or equal to delay <health_monitor_delay>.

The value of the parameter `health monitor delay` must be greater than the value of `health monitor timeout`.

## http_request_size_exceeded
{: #http-request-size-exceeded}

**Message**: The HTTP request is too large.

This problem occurs when the payload you have sent in your request has too many characters. Try again with a smaller payload. For example, instead of trying to do everything in a single request, try creating a minimal resource in one request, and then appending state to it incrementally in several subsequent requests.

Refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for additional help. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## iam_failure
{: #iam-failure}

**Message**: None

A failure has occurred in the IAM service, verifying authentication or authorization. The outage of the IAM service may be temporary. Retry the request after few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policies_quota_exceeded
{: #ike-policies-quota-exceeded}

**Message**: The IKE policy cannot be created because your account has reached the quota.

The quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas) page.

To view current IKE policies, use the `GET /ike_policies` API.
Equivalent CLI command: `ibmcloud is ike-policies`

## ike_policy_duplicate_name
{: #ike-policy-duplicate-name}

**Message**: The name `<ike_policy_name>` is in use already by IKE policy `<ike_policy_id>`.

Provide a different IKE policy name. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_in_use
{: #ike-policy-in-use}

**Message**: The IKE policy `<ike_policy_id>` is in use by one or more connections.

An IKE policy cannot be deleted if it is in use by one or more connections.

To see which connections are using the IKE policy, use the `GET /ike_policies/<ike_policy_id>/connections` API.
Equivalent CLI command: `ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
{: #ike-policy-invalid-name}

**Message**: The name `<ike_policy_name>` is not a valid IKE policy name.

A valid IKE policy name starts with a letter, followed by letters, digits, underscores or hyphens.

## ike_policy_not_created
{: #ike-policy-not-created}

**Message**: The IKE policy `<ike_policy_id>` could not be created.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_deleted
{: #ike-policy-not-deleted}

**Message**: The IKE policy `<ike_policy_id>` could not be deleted.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_found
{: #ike-policy-not-found}

**Message**: The IKE policy `<ike_policy_id>` could not be found.

You referenced an IKE policy that does not exist. Review your request to ensure that you specified the proper IKE policy ID. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_updated
{: #ike-policy-not-updated}

**Message**: The IKE policy `<ike_policy_id>` could not be updated.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_delete_conflict
{: #instance-delete-conflict}

**Message**: The instance cannot be deleted in the current status.

You cannot delete an instance when it is in `deleting`, `pending`, `starting`, `stopping`, or `restarting` status. The instance must be in `running` or `stopped` status. If the status does not change, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_invalid_hostname
{: #instance-invalid-hostname}

**Message**: The instance cannot be created because the hostname is not valid.

The instance hostname must meet the following requirements:
* The hostname must contain only alphanumeric characters and dashes
* Uppercase characters are not allowed
* Leading and trailing dashes are not allowed
* Leading numbers are not allowed
* Consecutive dashes are not allowed
* Maximum length is 15 characters for Windows
* Maximum length is 40 characters for other operating systems

## instance_invalid_port_speed
{: #instance-invalid-port-speed}

**Message**: The instance cannot be created because the specified network interface port speed is not valid.

The network interface port speed must be `100` or `1000` Mbps.

## instance_name_exists
{: #instance-name-exists}

**Message**: The same instance name already exists.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_sec_volume_over_quota
{: #instance-sec-volume-over-quota}

**Message**: The instance cannot be created because the number of volume attachments would exceed the quota.

The VPC quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas) page.

## instance_too_many_keys
{: #instance-too-many-keys}

**Message**: The Windows instance cannot be created because the request contains multiple keys.

Windows instances only support one key.

## insufficient_space_for_subnet
{: #insufficient-space-for-subnet}

**Message**: Insufficient space for subnet in address prefix.

The subnet cannot be created because the number of addresses requested cannot be allocated. Try again using a smaller address count or a larger CIDR.

## internal_error
{: #internal-error}

**Message**: An internal error occurred.

An unexpected error occurred. This problem may be temporary. Try the request again in a few minutes. If this error persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_server_error
{: #internal-server-error}

**Message**: None

An unexpected error occurred. This problem may be temporary. Try the request again in a few minutes. If this error persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_solution
{: #internal solution}

**Message**: Please contact your administrator.

An unexpected error occurred. This problem may be temporary. Try the request again in a few minutes. If this error persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## invalid_action_for_public_image
{: #invalid-action-for-public-image}

**Message**: The action cannot be performed on a public image.

Public images cannot be added, modified, or deleted.

## invalid_generation_parameter
{: #invalid-generation-parameter}

**Message**: The generation query parameter is invalid.

For versions on and after 5/31/2019, the 'generation' query parameter must be set to 1 to allow VPC API requests for use with generation 1 compute resources
and set to 2 to allow VPC API requests for use with generation 2 compute resources.

How to set the generation parameter

In the CLI:
`ibmcloud is target --gen 1`

In the API:
```sh
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1"
-H "Authorization: $iam_token"
```
{: pre}

## invalid_id_format
{: #invalid-id-format}

**Message**: Bad ID format. Ensure format is correct.

Make sure that the ID you provided does not contain any malformed data.

You may get this error message if you provide a malformed start query when making a pagination request. For example,
`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=$version&generation=1` contains two `?`s. Fix the query and try again.

## invalid_route
{: #invalid-route}

**Message**: The requested route does not exist.

The requested route on the API URL you provided does not exist. Verify that the URL you specified to call the API endpoint is correct.

## invalid_request_field
{: #invalid-request-field}

**Message**: A field provided in the request is not valid.

For example, to update the network ACL used by a subnet use the `PATCH /v1/subnets/{subnet_id}?version=$version&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API.

The following request would be invalid because “networkacl” is not a valid field, `PATCH /v1/subnets/{subnet_id}?version=$version&generation=1  -d '{ "networkacl":{ "id": “{network_acl_id}” } }’`

Refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for acceptable values.

## invalid_state
{: #invalid-state}

**Message**: An action was requested on a resource which is not supported at the current status of the resource.

If the resource is an instance:

1. A reboot operation may already be in progress for the instance. Refer to [actions allowed](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-invoke-action), depending on the status of the instance.

2. While the status of the instance is `pending` you cannot perform the following actions:
    * delete the instance
    * attach a volume to the instance
    * detach a volume from the instance
    * update the instance

Try your action again later. If the status of the resource does not change, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

If the resource is an image:

While the status of the instance is `deleting` you cannot perform the following actions:
    * patch the image
    * delete the image

Do not make the request again. Deletion of the image will complete in its own time. Once deletion is complate, all requests on that image will fail with the error `not_found` instead.

## invalid_version
{: #invalid-version}

**Message**: The `version` parameter is invalid, it must be of the form `YYYY-MM-DD`.

The version must comply with the format _YYYY-MM-DD_. For single-digit months or dates, such as January 1st, the version should look like `2019-01-01`. The version parameter must be present in the URL. The date given in the version parameter must be later than 2019-01-01 but before the current date.

## invalid_version_range
{: #invalid-version-range}

**Message**: The `version` value cannot be set at a future date nor before `2019-01-01`.

The date given in the version parameter must be later than 2019-01-01 but before the current date.

## invalid_template
{: #invalid-template}

**Message**: The template provided is not valid.

To fix this problem, be sure the template provided is valid and that your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## invalid_zone
{: #invalid-zone}

**Message**: Please check whether the resources you are requesting are in the same zone.

You may see this message attempting to attach a public gateway in zone-1 to a subnet in zone-2.

## ipsec_policies_quota_exceeded
{: #ipsec-policies-quota-exceeded}

**Message**: The IPsec policy cannot be created because your account has reached the quota.

The quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas) page.

To view current IPsec policies, use the `GET /ipsec_policies` API.
Equivalent CLI command: `ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
{: #ipsec-policy-duplicate-name}

**Message**: The name `<ipsec_policy_name>` is in use already by IPsec policy `<ipsec_policy_id>`.

Provide a different IPsec policy name. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_in_use
{: #ipsec-policy-in-use}

**Message**: The IPsec policy `<ipsec_policy_id>` is in use by one or more connections.

An IPsec policy cannot be deleted if it is in use by one or more connections.

To see which connections are using the IPsec policy, use the `GET /ipsec_policies/<ipsec_policy_id>/connections` API.
Equivalent CLI command: `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
{: #ipsec-policy-invalid-name}

**Message**: The name `<ipsec_policy_name>` is not a valid IPsec policy name.

A valid IPsec policy name starts with a letter, followed by letters, digits, underscores or hyphens.

## ipsec_policy_not_created
{: #ipsec-policy-not-created}

**Message**: The IPsec policy `<ipsec_policy_id>` could not be created.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_deleted
{: #ipsec-policy-not-deleted}

**Message**: The IPsec policy `<ipsec_policy_id>` could not be deleted.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_found
{: #ipsec-policy-not-found}

**Message**: The IPsec policy `<ipsec_policy_id>` could not be found.

You referenced an IPsec policy that does not exist. Review your request and specify a valid IPsec policy ID. If you are sure the policy exists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_updated
{: #ipsec-policy-not_updated}

**Message**: The IPsec policy `<ipsec_policy_id>` could not be updated.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_content_exists
{: #key-content-exists}

**Message**: The same key content already exists.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_name_exists
{: #key-name-exists}

**Message**: The same key name already exists.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_invalid_name
{: #key-invalid-name}

**Message**: The key cannot be created because the name is not valid.

The key name must meet the following requirements:
* it must begin with alphabetical character, [A-Za-z]
* it must contain only alphanumeric characters, dashes or underscores, [-A-Za-z0-9_]
* the length must not exceed 65 characters

## key_invalid_type
{: #key-invalid-type}

**Message**: The key cannot be created because the type is not valid.

The only supported key type is `rsa`.

## key_parse_failure
{: #key-parse-failure}

**Message**: The key cannot be created because the public key value is not valid.

The public key provided in the request is not valid. Check the value or regenerate the public key and try again.

In Linux or macOS operating systems, you can run the following command to validate the public key `ssh-keygen -l -v -f <key_file>`.

## listener_certificate_not_found
{: #listener-certificate-not-found}

**Message**: Certificate instance with CRN `<listener_certificate_crn>` cannot be found or no permission to access the certificate instance.

Provide an existing certificate instance CRN or ask your account administrator to grant you access permissions to the provided certificate instance.

## listener_duplicate_port
{: #listener-duplicate-port}

**Message**: Port `<listener_port>` is used by another listener instance. Please choose a different port.

Choose a different port.

## listener_invalid_certificate
{: #listener-invalid-certificate}

**Message**: CRN of certificate instance for the HTTPS listener is invalid.

Provide a valid certificate instance CRN.

## listener_invalid_connection_limit
{: #listener-invalid-connection-limit}

**Message**: Listener connection limit `<listener_connection_limit>` in invalid.

Provide a valid connection limit. THe connection limit must be an integer between 1 and 15000.

## listener_invalid_port
{: #listener-invalid-port}

**Message**: Listener port `<listener_port>` is invalid.

Choose a different port.

## listener_invalid_protocol
{: #listener-invalid-protocol}

**Message**: Listener protocol `<listener_protocol>` is invalid.

Provide a valid listener protocol. Acceptable values for listener protocol are `http`, `https`, and `tcp`.

## listener_missing_certificate_crn
{: #listener-missing-certificate-crn}

**Message**: CRN of the certificate instance for the `https` listener is missing.

The CRN of the certificate instance is a required field.

## listener_missing_port
{: #listener-missing-port}

**Message**: Listener port is missing.

Listener port is a required field.

## listener_missing_protocol
{: #listener-missing-protocol}

**Message**: Listener protocol is missing.

Listener protocol is a required field. The acceptable values are `http`, `https` and `tcp`.

## listener_not_found
{: #listener-not-found}

**Message**: The listener with ID `<listener_id>` cannot be found.

Provide an existing listener ID.

## listener_over_quota
{: #listener-over-quota}

**Message**: Listener cannot be created. Quota of listeners for the load balancer resource has reached the maximum limit.

The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc/?topic=vpc-quotas).

## listener_pool_protocols_conflict
{: #listener-pool-protocols-conflict}

**Message**: Listener protocol(`<listener_protocol>`) and pool protocol(`<pool_protocol>`) are in conflict.

A listener with `https` or `http` protocol can only be associated with a pool with `http` protocol. Similarly, a `tcp` listener can only be associated with a `tcp` pool.

## listener_reserved_port_conflict
{: #listener-reserved-port-conflict}

**Message**: Listener port `<listener_port>` is one of the reserved ports. The port range of 56500 to 56520 is reserved for management purposes. Choose a different port.

Choose a different port.

## load_balancer_delete_conflict
{: #load-balancer-delete-conflict}

**Message**: The load balancer with ID <load_balancer_id> cannot be deleted because its status is one of these:
* `UPDATE_PENDING`
* `CREATE_PENDING`
* `DELETE_PENDING`
* `MAINTENANCE_PENDING`

Try deleting the load balancer when it is in `ACTIVE` status.

## load_balancer_duplicate_name
{: #load-balancer-duplicate-name}

**Message**: Name `<load_balancer_name>` is used by another load balancer instance. Please choose a different name.

Provide a different name for the load balancer instance. The load balancer name should be unique within the VPC and the account.

Your Load Balancer for VPC will not conflict with your Cloud Load Balancer, because DNS domain names are different for these services, and the name of the Load Balancer for VPC is not included in the DNS name.
{: note}

## load_balancer_insufficient_ips
{: #load-balancer-insufficient-ips}

**Message**: The subnet with ID(s) <subnet_id> has insufficient available IPv4 addresses.

In your request, provide a valid subnet with available IPv4 addresses, in which you wish to create the load balancer.

## load_balancer_invalid_description
{: #load-balancer-invalid-description}

**Message**: Load balancer description `<load_balancer_description>` is not valid.

A load balancer description cannot exceed 255 characters.

## load_balancer_invalid_is_public
{: #load-balancer-invalid-is-public}

**Message**: Value of is_public `<load_balancer_is_public>` is not valid.

The value of the load balancer parameter `is_public` must be either _true_ or _false_.

## load_balancer_invalid_name
{: #load-balancer-invalid-name}

**Message**: Name `<load_balancer_name>` is invalid.

Names may not be empty. A valid load balancer name starts with a letter, followed by letters, digits, or underscores. The length of the name may not exceed 40 characters.

## load_balancer_invalid_subnet
{: #load-balancer-invalid-subnet}

**Message**: The subnet with ID <subnet_id>  is not valid. Make sure to use an existing subnet with 'available' status.

Provide valid subnets that are in `available` status when entering your request to create a load balancer.

## load_balancer_missing_is_public
{: #load-balancer-missing-is-public}

**Message**: 'is_public' field is missing.

The field is `is_public` is required. Provide a value for the `is_public` field.

## load_balancer_missing_name
{: #load-balancer-missing-name}

**Message**: Load balancer name is missing.

The load balancer `name` is a required field. Provide a name for the load balancer.

## load_balancer_missing_subnets
{: #load-balancer-missing-subnets}

**Message**: Load balancer subnets is missing.

The field `subnets` is required. In your request to create a load balancer, provide information about the subnets in which you wish to create the load balancer.

## load_balancer_missing_subnet_id
{: #load-balancer-missing-subnet-id}

**Message**: Subnet ID is missing.

The **Subnet ID** is a required field. Provide a subnet ID with your command.

## load_balancer_not_found
{: #load-balancer-not-found}

**Message**: The load balancer with ID `<load_balancer_id>` cannot be found.

Provide the ID of an existing load balancer.

## load_balancer_over_quota
{: #load-balancer-over-quota}

**Message**: Load balancer cannot be created. Quota of load balancer instances under your account has reached maximum limit.

Delete an existing load balancer, or contact support to increase the load balancer quota on your account.

The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## load_balancer_subnet_not_found
{: #load-balancer-subnet-not-found}

**Message**: The subnet with ID <subnet_id>  cannot be found.

In your request, you must provide IDs of existing subnets in which you wish to create the load balancer.

## load_balancer_unchanged_update
{: #load-balancer-unchanged-update}

**Message**: There is nothing to update the load balancer with ID `<load_balancer_id>`.

No information was given for updating a load balancer instance with the ID you specified.

## load_balancer_update_conflict
{: #load-balancer-update-conflict}

**Message**: The load balancer with ID <load_balancer_id> cannot be updated because its status is one of these:
* `UPDATE_PENDING`
* `CREATE_PENDING`
* `DELETE_PENDING`
* `MAINTENANCE_PENDING`

Try updating the load balancer when it is in `ACTIVE` status.

## member_duplicate_address_and_port
{: #member-duplicate-address-and-port}

**Message**:  Member with address <member_address> and port <member_port> already exists in the pool.

Provide a unique member address and port combination in your request.

## member_invalid_address
{: #member-invalid-address}

**Message**: The specified member IP address <member_ip> is invalid.

In your request, provide a valid IPv4 CIDR address for the member.

## member_invalid_port
{: #member-invalid-port}

**Message**: The specified member port `<member_port>` is invalid.

Provide a valid port number. A valid port number is in the range from 1 to 65535.

## member_invalid_weight
{: #member-invalid-weight}

**Message**: The specified member weight `<member_weight>` is invalid.

Provide a valid member weight. The value must be an integer between 0 and 100.

## member_ip_not_found
{: #member-ip-not-found}

**Message**: Member with IP address <member_IP> does not belong to any subnets in the Region and VPC of the load balancer.

In your request, provide the address of a member that belongs to a subnet in same the Region and VPC as the load balancer.

## member_missing_address
{: #member-missing-address}

**Message**: Member address is missing.

Member address is a required field. Provide a value for member address.

## member_missing_protocol_port
{: #member-missing-protocol-port}

**Message**: Member port is missing.

Member port is a required field. Provide a value for member port.

## member_not_found
{: #member-not-found}

**Message**:  Member with ID <member_id> cannot be found.

Provide a member ID where the user has read access and read access to the subnet where the member resides.

## member_over_quota
{: #member-over-quota}

**Message**: Member cannot be created. Quota of member instances under the pool has reached maximum limit.

The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## missing_generation_parameter
{: #missing-generation-parameter}

**Message**: The generation query parameter is required.

For versions on and after 5/31/2019, the `generation` query parameter is required for VPC for generation 1 compute resources API requests.

## missing_ims_account_id
{: #missing-ims-account-id}

**Message**: There is no classic infrastructure (IMS) account linked to your account.

To create a Classic Access VPC, your account must be linked to a classic infrastructure (IMS) account. See [Setting up access to your Classic Infrastructure from VPC](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc) to learn more.

## missing_version
{: #missing-version}

**Message**: The `version` parameter is required, and it must be of the form `YYYY-MM-DD`.

A version parameter is required for all API requests. The version must comply with the format _YYYY-MM-DD_. For single-digit months or dates, such as January 1st, the version should look like `2019-01-01`. The date given in the version parameter must be later than 2019-01-01 but before the current date. Check out these [API examples](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) for how to provide the version parameter.

## network_conflict
{: #network-conflict}

**Message**: CIDR conflicts with existing Address Prefix in VPC.

You might see this message if you provided a network CIDR that conflicts with an existing network CIDR in the same IP space.

To find the existing CIDRs in Address Prefixes for the VPC, run the API command `GET /v1/vpcs/<vpc-id>/address_prefixes` and look at the `cidr` values of the response. Check the value of `cidr` to make sure that the CIDR you've provided is not in use by other Address Prefixes.

If you are using the CLI, run the command `ibmcloud is vpc-addrs <vpc-id>` and check the value of `CIDR Block` for conflicts.

To find the existing CIDRs in subnets, run the API command  `GET /v1/subnets` to list all the subnets for the VPC. Then, run the API command `GET /v1/subnets/<subnet-id>` for each subnet in the VPC and check the value of `ipv4_cidr_block` in the response. Check the value of `ipv4_cidr_block` to make sure that the CIDR you've provided is not in use by other Address Prefixes.

If you are using the CLI, run the command `ibmcloud is subnets` to list all the subnets for the VPC. Then, run the command `ibmcloud is subnet <subnet-id>` for each subnet in the VPC, and check for conflicts with the value of `IPv4 CIDR` in the CLI output.

## new_account_provisioning_disabled
{: #new-account-provisioning-disabled}

**Message**: Provisioning of resources for accounts new to VPC are disabled for VPC Gen1. Please use VPC Gen2.

Due to the end of life of VPC Gen 1, new accounts are not able to provision new resources and will see this error when doing so. Use VPC Gen 2 to provision new resources.

## not_authorized
{: #not-authorized}

**Message**: The request is not authorized.

You may see this error is if your IAM token is missing or expired. For instructions on how to generate a token, refer to [Creating a VPC using the REST APIs](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis). If the token is not expired, make sure the account you are using is authorized to use this function and you have the [correct permissions](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

If you have correct authorization but you are still getting this error, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_found
{: #not-found}

**Message**: Please check whether the resource you are requesting exists.

You referenced a resource that does not exist or one to which you do not have access. Review your request to ensure that you specified the proper IDs and references.

If the resource is an operating system, you can use the request, `GET /v1/operating_systems` to list valid options.  For more information, see [Retrieves all operating systems](/apidocs/vpc/latest#retrieves-all-operating-systems){: external}.

Refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for additional help. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_implemented
{: #not-implemented}

**Message**: None

The method is not implemented. Check your request to make sure you are calling a [valid API](https://{DomainName}/apidocs/vpc-on-classic){: external}. [Contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) if you expect this API to be implemented.

## not_in_address_prefix
{: #not-in-address-prefix}

**Message**: The provided CIDR does not fit in any of the address prefixes in the provided zone.

This error occurs if a user is trying to create a subnet with a CIDR that does not fall into any address prefixes for the given zone.

Run `GET /vpcs/{vpc_id}/address_prefixes` to get the list of address prefixes for the VPC. If using the CLI, you can run `ibmcloud is vpc-address-prefixes` to list all address prefixes for your VPC. Look at the `cidr` and `zone` values of the response and make sure the subnet's `cidr` is a subset of the `cidr` of the address prefix for the zone you are trying to create it.

## no_default_address_prefix
{: #no-default-address-prefix}

**Message**: The zone must have a default address prefix to create a subnet using total_address_count.

This error occurs if a user is trying to create a subnet by address count and no default address prefix is available in the given zone.

## operating_system_restricted
{: #operating-system-restricted}

**Message**: You cannot use this operating system to create a custom image.

The referenced operating system is restricted for use with system images only. Select a different operating system and retry your request.

## over_quota
{: #over-quota}

**Message**: The request would exceed the quota for a resource type.

The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## password_not_ready
{: #password-not-ready}

**Message**: None

Your instance must be running before you can retrieve the password. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## pool_delete_conflict
{: #pool-delete-conflict}

**Message**: Pool cannot be deleted because it is still associated with one or more listeners.

Ensure the pool is disassociated from any listeners and try again.

## pool_duplicate_name
{: #pool-duplicate-name}

**Message**: Pool name `<pool_name>` is already used by another pool. Choose a different name.

Choose a different pool name.

## pool_invalid_name
{: #pool-invalid-name}

**Message**:  Name <pool_name> is invalid.

Provide a valid pool name.  A valid load balancer name starts with a letter followed by letters, digits, hyphens. The length of the name may not exceed 40 characters.

## pool_invalid_session_persistence
{: #pool-invalid-session-persistence}

**Message**: Pool session persistence value is not valid.

Provide a valid session persistence value. Acceptable value is `SOURCE_IP`.

## pool_missing_algorithm
{: #pool-missing-algorithm}

**Message**: Load balancing algorithm is missing.

Load balancing algorithm is a required field. Provide the load balancing algorithm in your request. The acceptable values are `round_robin`, `weighted_round_robin`, and `least_connections`.

## pool_missing_health_monitor
{: #pool-missing-health-monitor}

**Message**: Pool health monitor is missing.

Pool health monitor is a required field.

## pool_missing_members
{: #pool-missing-members}

**Message**: Pool members are missing.

Provide pool members.

## pool_missing_name
{: #pool-missing-name}

**Message**: Pool name is missing.

Pool name is a required field.

## pool_missing_protocol
{: #pool-missing-protocol}

**Message**: Pool protocol is missing.

Pool protocol is a required field. Provide the pool protocol in your request. The acceptable values are `http` and `tcp`.

## pool_not_found
{: #pool-not-found}

**Message**: The pool with ID `<pool_id>` cannot be found.

Provide an existing pool ID.

## pool_over_quota
{: #pool-over-quota}

**Message**: Pool cannot be created. Quota of pools for the load balancer resource has reached maximum limit.

The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## public_gateway_in_use
{: #public-gateway-in-use}

**Message**: Cannot delete a public gateway when it is in use.

The public gateway currently is attached to one or more subnets. You must detach the public gateway from all subnets before you can delete it.

To see which subnet is using the public gateway, use the `GET /v1/subnets?version=$version&generation=1` API. Equivalent CLI command: `ibmcloud is subnets`. To detach the public gateway from the subnet, use the API command `DELETE /v1/subnets/{subnet_id}/public_gateway?version=$version&generation=1` or the CLI command `ibmcloud is subnet-public-gateway-detach`.

## rate_limit_exceeded
{: #rate-limit-exceeded}

**Message**: Too many requests within a short time.

This error message is returned if too many requests are received within a specified time interval. Wait a while and try again.

## reserved_ip_in_use
{: #reserved-ip-in-use}

**Message**: The reserved IP is in use and cannot be deleted.

To see a list of reserved IP addresses, use the `GET /v1/subnets/{subnet_id}/reserved_ips` API, or the CLI command `ibmcloud is subnet-reserved-ip` and make sure that the specified reserved IP address is not in use.

## reserved_ip_owned_by_provider
{: #reserved-ip-owned-by-provider}

**Message**: The specified reserved IP is owned by the provider.

To see a list of reserved IPs, use the `GET /v1/subnets/{subnet_id}/reserved_ips` API, or the CLI command `ibmcloud is subnet-reserved-ip` and make sure that the specified reserved IP is not owned by a provider.

## address_is_already_reserved
{: #address-is-already-reserved}

**Message**: The specified address is already reserved on the subnet.

To see a list of reserved IPs, use the `GET /v1/subnets/{subnet_id}/reserved_ips` API, or the CLI command `ibmcloud is subnet-reserved-ip` and make sure that the specified address is not already reserved.

## security_group_active_transactions
{: #security-group-active-transactions}

**Message**: The interface cannot be attached or detached until the instance appears in Active state.

The instance must be active before its network interfaces can be attached to a security group. Use `GET /v1/instances/{id}?version=$version&generation=1` or `ibmcloud is instance` to check on the status of the instance. Once the status is `running`, try again. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_already_attached
{: #security-group-already-attached}

**Message**: The interface is already attached to the security group.

The interface is already attached to the security group. Use `GET /v1/security_groups/{id}/network_interfaces?version=$version&generation=1` or `ibmcloud is security-group-network-interfaces` to view attached interfaces.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_exists
{: #security-group-exists}

**Message**: The security group already exists.

Security group names must be unique within a VPC. A security group with that name already exists in the targeted VPC. Use `GET /v1/security_groups?version=$version&generation=1` or `ibmcloud is security-groups` to view current security groups.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} or the [Using Security Groups document](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_interfaces_attached
{: #security-group-interfaces-attached}

**Message**: Cannot delete the security group while interfaces are attached.

Detach all interfaces before deleting the security group. Use `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=$version&generation=1` or `ibmcloud is security-group-network-interface-remove` to detach an interface.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} or the
[Using Security Groups document](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_interfaces_per_sg_exceeded
{: #security-group-interfaces-per-sg-exceeded}

**Message**: Exceeded limit of interfaces per security group.

Attaching another interface to the security group would exceed the limit of interfaces per security group. The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas). Evaluate your strategy and consider creating another security group with similar rules.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}
or the [Using Security Groups document](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_last_security_group_is_default
{: #security-group-last-security-group-is-default}

**Message**: The default security group cannot be removed when it is the only security group attached.

A network interface must be attached to at least one security group.
It will be attached to the VPC's default security group if one is not specified.
Attach the interface to a different security group, then try again to detach it from the default security group.
For information on the default security group, refer to the [Using Security Groups document](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_limit_exceeded
{: #security-group-limit-exceeded}

**Message**: Exceeded security group limit.

You have attempted to create a new security group, but you are currently at your account quota. The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas). Evaluate your strategy for assigning instances to security groups. It is often possible to reduce the overall number of security groups by assigning multiple instances to the same security group. This strategy will reduce the number of security groups, and drop you below your account quota. In rare cases, generally for large organizations, there is a need for expanding the quota. In this case, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) to inquire if this is possible.

## security_group_network_interface_not_active
{: #security-group-network-interface-not-active}

**Message**: The interface cannot be attached because it is not active.

Network interfaces must be active before attaching to security groups. Use `GET /v1/instances/{id}/network_interfaces/{id}?version=$version&generation=1` or `ibmcloud is instance-network-interface` to view the status of an interface. Once the status is 'available', try again. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_not_attached
{: #security-group-not-attached}

**Message**: The interface is not attached.

The interface is not attached to the security group. Use `GET /v1/security_groups/{id}/network_interfaces?version=$version&generation=1` or `ibmcloud is security-group-network-interfaces` to view attached interfaces.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} or the [Using Security Groups document](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli).

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_not_in_vpc
{: #security-group-not-in-vpc}

**Message**: The interface and security group are in different VPCs.

To attach a network interface to a security group, the instance must be in the same VPC as the security group. Use `GET /v1/security_groups/{id}?version=$version&generation=1` or `ibmcloud is security-group` to view the security groups details and VPC. Use `GET /v1/instances/{id}?version=$version&generation=1` or `ibmcloud is instance` to view the instance details and VPC.

## security_group_order_bindings
{: #security-group-order-bindings}

**Message**: Cannot delete the security group while it has pending instance orders.

If an instance was created with security group(s) specified on the network interfaces, the instance and network interfaces must be active before the security group can be deleted. Use `GET /v1/security_groups/{id}/network_interfaces?version=$version&generation=1` or `ibmcloud is security-group-network-interfaces` to view attached network interfaces and their status. Once the status of the interfaces is 'available', try again. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_port_max_less_than_port_min
{: #security-group-port-max-less-than-port-min}

**Message**: TCP/UDP max port cannot be less than min port.

The maximum port value cannot be less than the minimum port value. Specify a maximum port value that is larger than the minimum port value.

## security_group_port_range_both_or_neither
{: #security-group-port-range-both-or-neither}

**Message**: Port range must be unset, or both a minimum and maximum port must be set for 'tcp' and 'udp'.

Rules with a 'tcp' or 'udp' protocol may have an unset port range (to apply the rule to all traffic), or they may specify a port range. To restrict traffic to a single port, set both the minimum and maximum port to the port value. For example, the following command would create a rule to allow SSH on port 22: `ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`.

For further information on security group rule configurations, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## security_group_port_range_invalid_protocol
{: #security-group-port-range-invalid-protocol}

**Message**: A port range was specified with a protocol of 'icmp'. A port range is only valid for a protocol of 'tcp' or 'udp'.

Rules with an 'icmp' protocol have a type and code. Rules with 'tcp' or 'udp' protocols have a port range. For further information on security group rule configurations, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## security_group_remote_group_not_in_vpc
{: #security-group-remote-group-not-in-vpc}

**Message**: The remote group is not in the same VPC as this security group.

Security group rules may refer to a remote security group, allowing traffic between attached interfaces of the two groups. Remote security groups must be in the same VPC. Use `GET /v1/security_groups?2019-01-01` or `ibmcloud is security-groups` to view current security groups and their VPCs.

For further instructions to fix this problem, refer to the [Using Security Groups document](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).

## security_group_remoting_rules
{: #security-group-remoting-rules}

**Message**: Cannot delete the security group while remoting rules are attached.

The security group contains one or more rules referencing a remote security group. Use `GET /v1/security_groups/{id}/rules?version=$version&generation=1` or `ibmcloud is security-group-rules` to view the rules. The 'remote' field will specify any remote security groups. Try again after deleting or editing the remoting rule.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} or the [Using Security Groups document](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).

## security_group_remoting_rules_per_sg_exceeded
{: #security-group-remoting-rules-per-sg-exceeded}

**Message**: Exceeded limit of remoting rules per security group.

Adding another rule refering to a remote security group would exceed the limit of remoting rules per security group. The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas).

## security_group_rules_per_sg_exceeded
{: #security-group-rules-per-sg-exceeded}

**Message**: Exceeded limit of rules per security group.

Adding a rule would exceed the limit of rules per security group. The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas). Evaluate your strategy and consider creating another security group.

## security_group_sgs_per_interface_exceeded
{: #security-group-sgs-per-interface-exceeded}

**Message**: Exceeded limit of security groups per interface.

Attaching this interface to another security group would exceed the limit of security groups per interface. The quotas per resource are specified in [Quotas and limits for VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas). Evaluate your strategy and consider adding rules to an existing security group.

## security_group_type_code_invalid_protocol
{: #security-group-type-code-invalid-protocol}

**Message**: An 'icmp' type/code was given, but the requested protocol was not 'icmp'. Set the protocol to 'icmp' or specify a port range.

Only rules with an 'icmp' protocol have a type and code. Rules with 'tcp' or 'udp' protocols have a port range. For further information on security group rule configurations, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## security_group_vpc_default
{: #security-group-vpc-default}

**Message**: Cannot delete the default security group for a VPC.

The default security group cannot be deleted. For information on the default security group, refer to [Updating the default security group](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group).

## service_manager_service_failure
{: #service-manager-service-failure}

**Message**: None

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_conflict
{: #subnet-conflict}

**Message**: CIDR conflicts with existing Subnet in VPC.

Run the `GET /subnets` API to retrieve all subnets in VPC. Check the value of `ipv4_cidr_block` to make sure that the CIDR you have provided is not in use by other subnets.

If using the CLI, you can run `ibmcloud is subnets` and look at "Subnet CIDR" value for conflicts.

Refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for additional help. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty
{: #subnet-not-empty}

**Message**: Cannot delete the subnet until it is empty. Delete any resources in the subnet and retry.

There was a request to delete a subnet, but the subnet still has resources in it. Subnets may have instances, VPNs, load balancers, or public gateways in them. You must delete your resources in the subnet before you can delete the subnet.

In some situations, this error can occur even when the console shows 0 VSIs and 0 load balancers. Deletion is asynchronous, and it may take a few minutes for the internal status to change. Try your subnet deletion again in a few minutes.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_pgway_exists
{: #subnet-not-empty-pgway-exists}

**Message**: Cannot delete the subnet while it is attached to a public gateway. Detach the public gateway and retry.

There was a request to delete a subnet, but the subnet still has a public gateway attached to it. You must delete or detach the public gateway before you can delete the subnet.

If using the CLI, you can run `ibmcloud is public-gateways` to list the public gateways and `ibmcloud is subnet-public-gateway-detach` to detach a public gateway from a subnet.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_iks_worker_node_exists
{: #subnet-not-empty-iks-worker-node-exists}

**Message**: Cannot delete the subnet while it is in use by IBM Cloud Kubernetes Service. Please remove Kubernetes worker nodes from the subnet and retry.

There was a request to delete a subnet, but the subnet still has some IKS worker nodes on it. You must delete the IKS worker nodes before you can delete the subnet.

The `value` field inside the error message should contain the ID list of IKS clusters that are on the subnet.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_ipaddr_exists
{: #subnet-not-empty-ipaddr-exists}

**Message**: Cannot delete the subnet while it contains IP addresses. Please delete any server instance associated with the IP address and retry.

There was a request to delete a subnet, but the subnet still contains IP addresses. You must delete the server instance associated with the IP address before you can delete the subnet.

If using the CLI, you can run `ibmcloud is instances` to list the server instances. Look at the "Address" value to determine its IP Address and "Status" to determine its status. Run `ibmcloud is instance-delete` to delete a server instance that doesn't already have a status of "deleting".

In some situations, this error can occur even when the console shows 0 VSIs. Deletion is asynchronous, and it may take a few minutes for the internal status to change. Try your subnet deletion again in a few minutes.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_loadbalancer_exists
{: #subnet-not-empty-loadbalancer-exists}

**Message**: Cannot delete the subnet while it contains a load balancer. Delete the load balancer and retry.

There was a request to delete a subnet, but the subnet still contains a load balancer. You must delete the load balancer before you can delete the subnet.

If using the CLI, you can run `ibmcloud is load-balancers` to list the load balancers. Look at the "Subnets" value to determine its subnet, and "Status" to determine its status. Run `ibmcloud is load-balancer-delete` to delete a load balancer that doesn't already have a status of "deleting".

In some situations, this error can occur even when the console shows 0 load balancers. Deletion is asynchronous, and it may take a few minutes for the internal status to change. Try your subnet deletion again in a few minutes.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_vpn_gway_exists
{: #subnet-not-empty-vpn-gway-exists}

**Message**: Cannot delete the subnet while it contains a VPN gateway. Delete the VPN gateway and retry.

There was a request to delete a subnet, but the subnet still has a VPN gateway attached to it. You must delete the VPN gateway before you can delete the subnet.

If using the CLI, you can run `ibmcloud is vpn-gateways` to list the VPN gateways. Look at the "Subnets" value to determine its subnet and "Status" to determine its status. Run `ibmcloud is vpn-gateway-delete` to delete a VPN gateway that doesn't already have a status of "deleting".

In some situations, this error can occur even when the console shows 0 VPN gateways. Deletion is asynchronous, and it may take a few minutes for the internal status to change. Try your subnet deletion again in a few minutes.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_out_of_ip_addresses
{: #subnet-out-of-ip-addresses}

**Message**: There are no more IP addresses available in the subnet.

There was a request to allocate an IP address in the subnet, but the subnet is out of IP addresses.  Try using a different subnet, or delete IP addresses in the subnet to free up space.

To find the IP addresses currently allocated in the subnet, run the API command `GET /v1/subnets/{subnet ID}/reserved_ips?version=$version&future_version=true&generation=1` (disclaimer: this is currently a Beta API).

## subnet_unknown_state
{: #subnet-unknown-state}

**Message**: The subnet `<subnet_id>` is in an invalid state for the requested operation.

The subnet must be in `available` status before you can operate it. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## target_in_use
{: #target-in-use}

**Message**: The target already has floating IP attached.

There was a request to attach a floating IP address to a server’s network interface, but the network interface already has a floating IP attached to it.

To find the floating IP address currently attached to a network interface, run the API command `GET /v1/floating_ips?version=$version&generation=1` and look for the network interface ID in the `target.id` field.

If you are using the CLI, run the command `ibmcloud is floating-ips` and look at the `Target` value. Be aware that the CLI truncates the network interface ID at the first dash (“-“) character. For example, a server’s primary network interface with an ID of `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` appears as `primary(abdfcb29-.)` in the CLI output.

## token_invalid
{: #token-invalid}

**Message**: The service token was expired or invalid.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## token_missing
{: #token-missing}

**Message**: The service token was empty or did not exist in the request.

Recreate a token and try again.

## validation_enum
{: #validation-enum}

**Message**: The value supplied was not a valid option.

One of the fields supplied has a value that must come from an enumeration of possible values.

For Network ACL rules, you may specify a direction:

```text
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

For example, the following value would be invalid because `northbound` is not a valid option in the enumeration `[ inbound, outbound ]`.

```json
{
    "direction":"northbound"
}
```

Refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} or the [CLI reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference) for acceptable values.

## validation_failure
{: #validation-failure}

**Message**: The JSON provided did not match the expected structure.

To fix this problem, be sure the content of your request is valid JSON and that your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## validation_invalid_cidr
{: #validation-invalid-cidr}

**Message**: The value is not a valid CIDR.

The value must be a valid internal CIDR block with a 0 host part.

Certain IP address ranges are reserved. More information about reserved IP ranges is available in our overview of [Using your VPC with Regions and Subnets](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets).

## validation_invalid_ipv4_cidr
{: #validation-invalid-ipv4-cidr}

**Message**: The value is not a valid IPv4 CIDR.

Must be a IPv4 internal CIDR block with a 0 host part.

Certain IP address ranges are reserved. More information about reserved IP ranges is available in our overview of [Using your VPC with Regions and Subnets](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets).

## validation_invalid_ipv6_cidr
{: #validation-invalid-ipv6-cidr}

**Message**: The value is not a valid IPv6 CIDR.

Currently, IPv6 is not supported, use an IPv4 address.

## validation_invalid_address
{: #validation-invalid-address}

**Message**: The value is not a valid address.

Must be a valid IP address.

A list of individually reserved IP addresses is given in the [Regions and Subnets](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) document.

## validation_invalid_ipv4_address
{: #validation-invalid-ipv4-address}

**Message**: The value is not a valid IPv4 address.

Provide a valid IPv4 address.

## validation_invalid_ipv6_address
{: #validation-invalid-ipv6-address}

**Message**: The value is not a valid IPv6 address.

Give a valid IPv6 address. Currently, IPv6 is not supported; use an IPv4 address.

## validation_invalid_field_type
{: #validation-invalid-field-type}

**Message**: The value type does not match the field type.

To fix this problem, be sure the content of your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for the endpoint you are calling.

## validation_max_value
{: #validation-max-value}

**Message**: A value provided for a parameter is larger than allowed.

Provide a smaller value that meets the maximum as given by the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## validation_min_value
{: #validation-min-value}

**Message**: A value provided for a parameter is smaller than allowed.

You may get this error if you try to create a subnet with `total_ipv4_address_count` less than 8.

Provide a larger value that meets the minimum as given by the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## validation_not_null
{: #validation-not-null}

**Message**: The field supplied must be null.

Be sure the value is null. Refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

Here is an invalid example:

```json
{
    "name": null
}
```

## validation_only_one
{: #validation-only-one}

**Message**: The value must match one of the subschemas.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## validation_discriminator_forbidden
{: #validation-discriminator-forbidden}

**Message**: The discriminator field forbids this substructure.

For example, if you specify

```json
{
protocol: icmp
port_min: 5
port_max: 100
}
```
{: codeblock}

The protocol is `icmp`, and _port_min_ is a `tcp` field, so you'll get an error.
1. validation_discriminator_required for missing `icmp` rules
2. validation_discriminator_forbidden for `tcp` fields with `icmp` specified

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_internal_error
{: #validation-internal-error}

**Message**: None

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_name
{: #validation-invalid-name}

**Message**: The value is not a valid name.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_ref
{: #validation-invalid-ref}

**Message**: The value is not a valid HREF.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_non_empty_uuid
{: #validation-non-empty-uuid}

**Message**: None

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_required_field
{: #validation-required-field}

**Message**: Missing a required field.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_unique_failed
{: #validation-unique-failed}

**Message**: The field supplied must be unique.

You might have specified a name that is already in use. Try a different value.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## version_not_supported
{: #version-not-supported}

**Message** Version specified is not supported. The version query parameter must be 2019-05-31 or later.

## volume_action_invalid_request
{: #volume-action-invalid-request}

**Message**: The volume cannot be attached or detached from the instance in its current status.

You cannot attach or detach a volume when the target instance is in `deleting`, `pending`, `starting`, `stopping` or `restarting` status, or it has another volume attachment in `attaching` or `detaching` status. If the status of the instance or volume attachment does not change, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_attachment_delete_invalid_request
{: #volume-attachment-delete-invalid-request}

**Message**: The volume attachment cannot be deleted.

This operation is not allowed. The boot volume's volume attachment cannot be deleted because the boot volume is required by the instance.

## volume_attachment_over_quota
{: #volume-attachment-over-quota}

**Message**: The volume cannot be attached because the number of volume attachments would exceed the quota.

The quotas per instance are specified in [Quotas: Block storage volumes](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#block-storage-quotas).

## volume_attachment_update_invalid_request
{: #volume-attachment-update-invalid-request}

**Message**: The volume attachment could not be updated because the request is invalid.

The volume attachment could not be updated for either of these reasons:

* The boot volume's volume attachment cannot be renamed
* The boot volume's volume attachment `delete_volume_on_instance_delete` field cannot be set to `true`

## volume_capacity_max
{: #volume-capacity-max}

**Message**: The maximum volume capacity allowed is 2000 GB.

The volume capacity you specified exceeds the maximum volume capacity. Specify a value between 10 and 2000 GB. See the [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) documentation for accepted capacity ranges for a Custom profile.

## volume_capacity_missing
{: #volume-capacity-missing}

**Message**: The required volume capacity value is missing in the request.

When you create a volume, you must specify a volume capacity based on this profile definition. For more information, see the [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) documentation.

## volume_capacity_zero_or_negative
{: #volume-capacity-zero-or-negative}

**Message**: The volume capacity should be greater than zero.

When creating a volume, the capacity value specified in the request must be a positive number between 10 GB and 2000 GB. See the [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) documentation for supported volume capacity values.

## volume_crn_account_id_mismatch
{: #volume-crn-account-id-mismatch}

**Message**: The CRN specified in the request does not belong to current user's account.

The Key Protect root key CRN does not match your IAM authorization account ID. Specify a different Key Protect root key CRN for your IAM account. See the [Key Protect](/docs/key-protect?topic=key-protect-getting-started-tutorial) documentation for more information.

## volume_crn_cname_mismatch
{: #volume-crn-cname-mismatch}

**Message**: The CRN specified in the request cannot be used for the current environment.

The CRN that you specified in the request cannot be used in the current environment. Provide a CRN applicable to your environment.

## volume_encryption_key_crn_invalid
{: #volume-encryption-key-crn-invalid}

**Message**: The volume encryption key CRN specified in the request is not valid.

The Key Protect CRN you provided is not valid. Obtain the CRN of the root key in the
Cloud Identity and Access Management service. For more information, see the  [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption) documentation.

## volume_encryption_key_not_active
{: #volume-encryption-key-not-active}

**Message**: The volume encryption key specified in the request is not active.

Activate the exising key or provide a new key. If you cannot activate your own key, contact your system administrator or [customer support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_encryption_key_not_authorized
{: #volume-encryption-key-not-authorized}

**Message**: You are not authorized to access the provided volume encryption key.

Provide another encryption key for which you have access, or contact your system administrator to obtain access to the current key.

## volume_encryption_key_not_implemented
{: #volume-encryption-key-not-implemented}

**Message**: The volume encryption key feature is not supported.

A volume could not be created using your encryption key. This version supports volumes with provider-managed encryption only. To use your own encryption key, use a version of Block Storage for VPC that supports customer-managed encryption.
Contact [customer support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) for more information.

## volume_id_invalid
{: #volume-id-invalid}

**Message**: The volume ID specified in the request parameter is not valid.

The volume ID you specified is not in the correct format. Verify that you correctly entered the volume ID and try again. If you're not sure of the volume ID, specify `ibmcloud is volumes` in the CLI or `GET volumes` in the API and search the list of volumes.

## volume_id_missing
{: #volume-id-missing}

**Message**: The volume ID is missing in the request parameter.

You must provide the volume ID in the request parameter when getting a specific volume.

## volume_id_not_found
{: #volume-id-not-found}

**Message**: Volume not found. Volume ID `<volume_id>` does not exist, where `<volume_id>` is the provided volume ID.

The specified volume ID does not exist. Provide a valid volume ID.

## volume_iops_zero_or_negative
{: #volume-iops-zero-or-negative}

**Message**: The volume IOPS should be greater than zero.

When creating a volume, the IOPS value specified in the request must be a positive number. See the [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) documentation for supported IOPS values.

## volume_name_duplicate
{: #volume-name-duplicate}

**Message**: The volume name is a duplicate. Volume name `<volume_name>` provided in the request already exists, where `<volume_name>` is the provided volume name.

The volume name specified in the request already exists in the VPC infrastructure. Provide a different name for the volume. For more information about volume naming, refer to [the volume naming guidelines](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-managing-block-storage-gen1#volume-name-conventions-gen1).

## volume_name_invalid
{: #volume-name-invalid}

**Message**: The volume name specified in the request is not valid.

Volume names can use lowercase alpha-numeric characters (a-z, 0-9), the hyphen (-), and be up to 63 characters long. Volume names must begin with a lowercase letter.

## volume_not_available
{: #volume-not-available}

**Message**: The Volume is not available. Volume can only be modified in available status. Current volume `<volume_id>` status is `<volume_status>`, where `<volume_id>` is the volume ID provided in the request and `<volume_status>` is the current volume status.

A volume can be modified only when it's in an `available` status. Make sure the volume is available before you modify it.

## volume_not_deletable
{: #volume-not-deletable}

**Message**: Delete failed.

The volume can only be deleted if it is in `available` or `failed` status. Make sure the volume is in a valid status to be deleted.

## volume_not_found
{: #volume-not-found}

**Message**: A volume with the specified ID is not found.

Verify that the volume ID you entered is correct and try again. For a list of volumes, specify `ibmcloud is volumes` in the CLI or `GET volumes` in the API.

## volume_not_implemented
{: #volume-not-implemented}

**Message**: The requested operation is not supported in this release.

Contact [customer support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) for more information.

## volume_profile_capacity_iops_invalid
{: #volume-profile-capacity-iops-invalid}

**Message**: The volume profile specified in the request is not valid for the provided capacity and/or IOPS.

The volume capacity and/or IOPS value that you specified in the request is not allowed by the volume profile. See the documentation for valid minimum and maximum capacity and IOPS values for a [Custom profile](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-profiles-gen1#custom-gen1).

## volume_profile_invalid
{: #volume-profile-invalid}

**Message**: The volume profile in the request is not valid.

The volume profile is missing in the request. Provide one of the following volume profiles in the request: `general-purpose`, `5iops-tier`, `10iops-tier`, and `custom`. For more information, see [{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-profiles-gen1).

## volume_profile_iops_invalid
{: #volume-profile-iops-invalid}

**Message**: The volume profile specified in the request cannot accept custom IOPS.

Your volume profile does not accept a Custom IOPS value. You might have specified a Tiered IOPS profile, which does not require that you specify an IOPS value. If you want to provide a specific IOPS value, use the Custom IOPS profile. For more information, see [{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-profiles-gen1).

## volume_profile_name_missing
{: #volume-profile-name-missing}

**Message**: The required volume profile name is missing in the request.

The volume profile name is missing in the request body when creating a volume or in the request parameter when getting a volume. Provide a volume profile name in your request and try again. These are: `general-purpose`, `5iops-tier`, `10iops-tier`, and `custom`. See the documentation for more information about [{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-profiles-gen1).

## volume_profile_not_found
{: #volume-profile-not-found}

**Message**: A volume profile with the specified name is not found.

A volume profile with that name could not be found. Verify that you provided the correct volume profile name. These are: `general-purpose`, `5iops-tier`, `10iops-tier`, and `custom`. See the documentation for more information about [{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-profiles-gen1).

## volume_quota_reached
{: #volume-quota-reached}

**Message**: The volume quota has been reached.

You are out of quota for the current account. Try deleting some volumes or contact [customer support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) to increase your quota. See the documentation for information about [quotas for the Virtual Private Cloud infrastructure](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## volume_resource_group_id_invalid
{: #volume-resource-group-id-invalid}

**Message**: The resource group ID specified in the request is not valid.

The resource group ID that you specified in the request is not valid. Verify the correct resource group ID. From the CLI, use the `ibmcloud is volume VOLUME_ID` command. The resulting information will include the resource group and resource group ID. See the [IBM Cloud CLI for VPC Reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference#storage-cli-section) for more information.

## volume_resource_group_not_authorized
{: #volume-resource-group-not-authorized}

**Message**: The current action is not authorized on the specified resource group.

You are not authorized to list or create volumes in the specified resource group. Use a different resource group for which you have permission or request permission from your administrator for list and create privileges on the resource group.

## volume_resource_group_not_found
{: #volume-resource-group-not-found}

**Message**: A resource group with the specified ID could not be found.

The resource group ID could not be found because you might have entered it incorrectly or it does not exist. Verify the correct resource group ID. From the CLI, use the `ibmcloud is volume VOLUME_ID` command. The resulting information will include the resource group and resource group ID. See the [IBM Cloud CLI for VPC Reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference#storage-cli-section) for more information.

## volume_start_not_found
{: #volume-start-not-found}

**Message**: A volume with the ID specified as the page start parameter is not found.

The volume ID that you specified in the start parameter of the list volume call could not be found. Verify that the volume ID is correct and whether you have access to the volume.

## volume_status_not_available
{: #volume-status-not-available}

**Message**: The volume with the specified ID is not available.

The volume is not in an Available state; it's status might be in a Pending state. Wait for volume to become available and try again. If the volume is being deleted, use a different volume. To check the volume's status, specify `ibmcloud is volume VOLUME_ID` in the CLI or use `GET $rias_endpoint/v1/volumes/$volume_id?version=$version&generation=1` in the API request.

## volume_still_attached
{: #volume-still-attached}

**Message**: The volume is still attached to an instance.

You cannot delete a volume that is still attached to a VSI. If you set automatic deletion for the volume, wait for the VSI to be deleted and volume will be detached and deleted. If you did not set automatic deletion, detach the volume manually.

## volume_template_invalid
{: #volume-template-invalid}

**Message**: Invalid volume template provided.

You provided an invalid volume template for creating a volume. See the [VPC API Reference](https://{DomainName}/apidocs/vpc-on-classic){: external} to verify that you are providing the correct parameters in the request body.

## volume_update_invalid_request
{: #volume-update-invalid-request}

**Message**: The volume update request is not valid.

The update request body property that you specified is not valid. See the [VPC API Reference](https://{DomainName}/apidocs/vpc-on-classic){: external} for information and correct API request syntax.

## vpc_not_empty
{: #vpc-not-empty}

**Message**: The VPC cannot be deleted because it is not empty.

All resources in the VPC must be deleted before the VPC can be deleted. A VPC contains instances, subnets, public gateways, load balancers and VPN gateways, and some of these resources contain resources as well. Refer to [How to Delete a VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) documentation for details.

If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpc_resource_separation
{: #vpc-resource-separation}

**Message**: The resources are in different VPCs.

The resources must be in the same VPC. For example, if you are attempting to attach a public gateway to a subnet, the public gateway must be in the same VPC as the subnet.

To determine which VPC contains the public gateway, run the `GET /public_gateways/{id}` command and look at the "vpc" value. The equivalent CLI command is `ibmcloud is public-gateway <id>`.

## vpn_connection_cidr_duplicated
{: #vpn-connection-cidr-duplicated}

**Message**: The `<cidr_type>` CIDR blocks have duplicated CIDR blocks `<cidr_blocks>`.

Duplicate CIDR blocks have been provided when creating the connection. Ensure that the CIDR blocks are unique.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If the problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_created
{: #vpn-connection-cidr-not-created}

**Message**: A CIDR block could not be added to the VPN connection `<vpn_connection_id>`.

Provide a valid CIDR that meets the requirements given by the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_deleted
{: #vpn-connection-cidr-not-deleted}

**Message**: A CIDR block could not be deleted from the VPN connection `<vpn_connection_id>`.

Provide a valid CIDR that exists on the connection. A connection must have at least one local and peer CIDR.

To view the connection's CIDR blocks, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` and `peer_cidrs` fields.
Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
{: #vpn-connection-cidr-not-found}

**Message**: The CIDR block was not found in the VPN connection `<vpn_connection_id>`.

Provide a valid CIDR that is in the connection.

To view the connection's CIDR blocks, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` and `peer_cidrs` fields.
Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
{: #vpn-connection-cidr-not-updated}

**Message**: A CIDR block could not be updated in the VPN connection `<vpn_connection_id>`.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_valid
{: #vpn-connection-cidr-not-valid}

**Message**: The CIDR block `<cidr_block>` does not represent a valid address.

The value must be a valid internal CIDR with no host bits set.

To view the connection's CIDR blocks, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` and `peer_cidrs` fields.

Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
{: #vpn-connection-cidr-overlap}

**Message**: The CIDR block `<cidr_block_1>` overlaps with `<cidr_block_2>`. Two peer CIDR blocks cannot overlap on the same VPC and two local CIDR blocks cannot overlap on the same connection.

Provide a valid CIDR that meets the requirements given by the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}..

To view the connection's CIDR blocks, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` and `peer_cidrs` fields.

Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
{: #vpn-connection-duplicate-name}

**Message**: The name `<vpn_connection_name>` is already in use by VPN connection `<vpn_connection_id>`.

Provide a different connection name. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_invalid_name
{: #vpn-connection-invalid-name}

**Message**: The name `<vpn_connection_name>` is not a valid VPN connection name.

A valid connection name starts with a letter, followed by letters, digits, underscores, or hyphens.

## vpn_connection_invalid_psk_format
{: #vpn-connection-invalid-psk-format}

**Message**: The pre-shared key (PSK) `<psk>` is not in the valid format.

A valid PSK should only contain 6 to 128 characters which are letters, digits, `-`,`+`,`&`,`!`,`@`,`#`,`$`,`%`,`^`,`*`,`(`,`)`,`,`,`.`,`:`,`_`.

## vpn_connection_local_cidrs_required
{: #vpn-connection-local-cidrs-required}

**Message**: At least one local CIDR block is required when creating a VPN connection.

Provide a valid local CIDR that meets the requirements given by the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## vpn_connection_local_subnets_quota_exceeded
{: #vpn-connection-local-subnets-quota-exceeded}

**Message**: Local subnets across VPN connections for the VPN gateway `<vpn_gateway_id>` has reached the quota.

The quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas) page.

To view the current local subnets across VPN connections, use the `GET /vpn_gateways/<vpn_gateway_id>/connections` API and check the `local_cidrs` field for each connection.

Equivalent CLI command: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
{: #vpn-connection-local-subnets-quota-exceeded-for-connection}

**Message**: Local subnets for the VPN connection has reached the quota.

The quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas) page.

To view the current local subnets for a VPN connection, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` field.

Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
{: #vpn-connection-not-created}

**Message**: The VPN connection `<vpn_connection_id>` could not be created.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_deleted
{: #vpn-connection-not-deleted}

**Message**: The VPN connection `<vpn_connection_id>` could not be deleted.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_found
{: #vpn-connection-not-found}

**Message**: The VPN connection `<vpn_connection_id>` could not be found.

Check whether the connection ID is correct. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_updated
{: #vpn-connection-not-updated}

**Message**: The VPN connection `<vpn_connection_id>` could not be updated.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_pair_cidrs_duplicated
{: #vpn-connection-pair-cidrs-duplicated}

**Message**: At least one pair of local CIDR and peer CIDR with the same peer gateway has been duplicated.

Another connection which has a matching local CIDR and peer CIDR to this one exists on this gateway.  Provide a valid set of local / peer CIDRs.

## vpn_connection_peer_cidrs_required
{: #vpn-connection-peer-cidrs-required}

**Message**: At least one peer CIDR block is required when creating a VPN connection.

Provide a valid peer CIDR that meets the requirements given by the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## vpn_connection_peer_subnets_quota_exceeded
{: #vpn-connection-peer-subnets-quota-exceeded}

**Message**: Peer subnets across VPN connections for the VPN gateway `<vpn_gateway_id>` has reached the quota.

The quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas) page.

To view the current peer subnets across VPN connections, use the `GET /vpn_gateways/<vpn_gateway_id>/connections` API and check the `peer_cidrs` field for each connection.

Equivalent CLI command: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
{: #vpn-connection-peer-subnets-quota-exceeded-for-connection}

**Message**: Peer subnets for the VPN connection has reached the quota.

The quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas) page.

To view the current peer subnets for a VPN connection, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `peer_cidrs` field.

Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
{: #vpn-connection-static-route-not-created}

**Message**: Failed to add a static route for the peer CIDR block `<peer_cidr>`.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_static_route_not_deleted
{: #vpn-connection-static-route-not-deleted}

**Message**: Failed to remove a static route for the peer CIDR block `<peer_cidr>`.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_update_cidrs_not_permitted
{: #vpn-connection-update-cidrs-not-permitted}

**Message**: CIDR blocks cannot be changed when updating a connection. Please use the correct API when creating or deleting CIDR blocks.

Provide a valid request value that meets the requirements given by the [API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external}.

## vpn_connections_quota_exceeded
{: #vpn-connections-quota-exceeded}

**Message**: The VPN connection cannot be created because the VPN gateway `<vpn_gateway_id>` has reached the quota.

The quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas) page.

To view the current VPN connections for a VPN gateway, use the `GET /vpn_gateways/<vpn_gateway_id>/connections` API.
Equivalent CLI command: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
{: #vpn-gateway-duplicate-name}

**Message**: The name `<vpn_gateway_name>` is already in use by VPN gateway `<vpn_gateway_id>`.

Provide a different VPN gateway name. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_initialization_error
{: #vpn-gateway-initialization-error}

**Message**: The VPN gateway could not be initialized.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_invalid_name
{: #vpn-gateway-invalid-name}

**Message**: The name `<vpn_gateway_name>` is not a valid VPN gateway name.

A valid VPN gateway name starts with a letter, followed by letters, digits, underscores, or hyphens.

## vpn_gateway_invalid_state
{: #vpn-gateway-invalid-state}

**Message**: The VPN gateway `<vpn_gateway_id>` is in an invalid state for the requested operation.

VPN Gateway must be in `available` status before you can operate it. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_ip_create_error
{: #vpn-gateway-ip-create-error}

**Message**: Unable to create an IP address for the VPN gateway.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_missing_subnet_id
{: #vpn-gateway-missing-subnet-id}

**Message**: Could not find a subnet identifier for the given VPN gateway template.

The **Subnet ID** is a required field. Provide a subnet ID with your command.

## vpn_gateway_missing_name
{: #vpn-gateway-missing-name}

**Message**: Could not find a name for the given VPN gateway template.

The **name** is a required field. Provide a name with your command.

## vpn_gateway_not_created
{: #vpn-gateway-not-created}

**Message**: The VPN gateway `<vpn_gateway_id>` could not be created.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_deleted
{: #vpn-gateway-not-deleted}

**Message**: The VPN gateway `<vpn_gateway_id>` could not be deleted.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_found
{: #vpn-gateway-not-found}

**Message**: The VPN gateway `<vpn_gateway_id>` could not be found.

Check whether the VPN gateway ID is correct. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_updated
{: #vpn-gateway-not-updated}

**Message**: The VPN gateway `<vpn_gateway_id>` could not be updated.

Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_not_found
{: #vpn-gateway-subnet-not-found}

**Message**: Could not find the subnet `<subnet_id>` for the given VPN gateway.

You've referenced a subnet that does not exist. Review your request to ensure that you specified the proper subnet ID. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_status_error
{: #vpn-gateway-subnet-status-error}

**Message**: The VPN gateway could not be created due to subnet status.

Provide a subnet that is in `available` status. Try again in a few minutes. If this problem persists, [contact support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateways_quota_exceeded
{: #vpn-gateways-quota-exceeded}

**Message**: The VPN Gateway cannot be created because your account and/or the region has reached the quota.

The quotas per resource are specified on the [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas) page.

To view the current VPN gateways, use the `GET /vpn_gateways` API.
Equivalent CLI command: `ibmcloud is vpn-gateways`

## zone_inconsistency
{: #zone-inconsistency}

**Message**: The resources must belong to the same zone.

* When you attach a volume, make sure the volume and the instance are in the same zone.
* When you associate a floating IP, make sure the floating IP and the subnet are in the same zone.
