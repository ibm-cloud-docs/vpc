---

copyright:
  years: 2021, [{CURRENT_YEAR}]
lastupdated: "[{LAST_UPDATED_DATE}]"

keywords: api, errors, error codes, status codes, metadata, http status codes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Instance metadata API error codes
{: #instance-metadata-error-codes}

As covered in [Error handling](/apidocs/vpc-metadata#error-handling-metadata), the VPC Instance Metadata API uses standard HTTP response codes to indicate the outcome of a request. For example, a `4xx`-series response indicates a failure that the client must resolve. A `5xx`-series response indicates a service failure.
{: shortdesc}

Additionally, all `4xx` and `5xx` responses include a JSON error response object that provides additional information about the problem. This information includes a `trace` property whose value might be requested by IBM support when they troubleshoot the failure, and an `errors` array property that contains one or more specific errors that are related to the problem. Each item in the `errors` array uses the following JSON schema:

- `code` - Error code string, such as `invalid_value`
- `message` - Text string that describes the error message, for example, "The value provided for the `expires_in` field must be between `5` and `3600`."
- `more_info` - If present, a link to the documentation about this error
- `target` - For errors that return a `target` property in the response, review the subproperties for clues:
    - `name` of the problematic field, query parameter, or header
    - `type` of input where the problem was found, such as a field
    - `value` (if present) the problematic value within the field, query parameter, or header

Example `400` JSON error response object:

```json
{
  "errors": [
    {
      "code": "invalid_value",
      "message": "The `expires_in` field must not exceed `3600`.",
      "more_info": "https://cloud.ibm.com/docs/vpc?topic=vpc-imd-configure-service",
      "target": {
        "name": "expires_in",
        "type": "field",
        "value": "7200"
      }
    }
  ],
  "status_code": 400,
  "trace": "e37872f6-f9a4-4084-a1a8-e56a1c8c8d3d"
}
```
{: codeblock}

Error codes can be added, removed, or modified in subsequent releases, with updates announced in the VPC Instance Metadata API change log. If you use error codes programmatically, we recommend that you code defensively. Any code that checks for specific error codes must always have a "default" or "catch-all" clause. So it can handle the case where the returned error code does not match any of the ones the code expected.
{: important}

## `invalid_request`
{: #invalid-request-error-code}

Used when the request cannot be parsed, such as when the JSON request is malformed or the request body is too large.

`invalid_request` error code can accompany a `400` HTTP status code.

Example message: The request body was malformed.

## `invalid_value`
{: #invalid-value-error-code}

Used for invalid values for headers, query parameters, path parameters, or properties (identified by the `target`). Includes integer values that are out of range, string values that have invalid characters, enumerations values out of the listed set, and so on.

`invalid_value` error code can accompany the following HTTP status codes:

- `404` for path parameters
- `400` for all other cases

Example message: The value provided for the `expires_in` field must be between `5` and `3600`.

## `missing_field`
{: #missing-field-error-code}

Used in any situation where a required header, query parameter, or property is not provided.

`missing_field` error code can accompany a `400` HTTP status code.

Example message: A trusted profile ID was not passed in the request body.

## `missing_value`
{: #missing-value-error-code}

Used for missing required headers, query parameters, or body properties (identified by the `target`).

`missing_value` error code can accompany a `400` HTTP status code.

Example message: A value such as `ibm` must be provided in the `Metadata-Flavor` header.

## `not_found`
{: #not-found-error-code}

Used for headers, query parameters, path parameters, or body properties (identified by the `target`) that are syntactically valid but refer to a resource that does not exist.

`not_found` error code can accompany the following HTTP status codes:

- `404` for path parameters
- `400` for all other cases

Example message: Placement group not found.

## `profile_not_linked`
{: #profile-not-linked-error-code}

Used when a trusted profile is not linked to a virtual server instance. This error code is returned only for the `POST /instance_identity/v1/iam_token` method.

`profile_not_linked` error code can accompany a `400` HTTP status code.

Example message: The virtual server instance is not linked to the specified trusted profile.

## `service_error`
{: #service-error-error-code}

Used when the client encounters a service-side issue.

`service_error` error code can accompany a `500` HTTP status code.

Example message: An internal error occurred.

## `unauthenticated`
{: #unauthenticated-error-code}

Used when a Bearer token is provided in the `Authorization` header, but the token is expired, malformed, or otherwise syntactically correct but not valid.

`unauthenticated` error code can accompany a `401` HTTP status code.

Example message: The provided token is invalid or expired.

## `unauthorized`
{: #unauthorized-error-code}

Used for headers, parameters, paths, or properties (identified by the `target`) that are syntactically valid but refer to a resource that you are not authorized to operate on in the requested manner.

`unauthorized` error code can accompany a `403` HTTP status code.

Example message: The metadata service is not enabled on the provided instance.

## `unknown_field`
{: #unknown_field-error-code}

Used when an unknown query parameter or property is provided.

`unknown_field` error code can accompany a `400` HTTP status code.

Example message: Unknown property `xyzzy` was specified in the request body.
