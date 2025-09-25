---

copyright:
  years: 2025
lastupdated: "2025-09-25"

keywords: public address ranges, limitations

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for public address ranges
{: #par-known-issues}  

As the rollout of Public Address Ranges for VPC continues, a few known issues and limitations have been identified. These are temporary and are scheduled to be addressed in upcoming updates.

* Intermittent 500 error when listing or deleting public address ranges

   Description: You might occasionally encounter a `500 Internal Server Error` when listing or deleting public address ranges. This is caused by back-end delays while confirming the deletion across all zonal nodes. As a result, the operation might exceed the 30-second timeout.

   Workaround: Wait a few minutes and retry the operation. In most cases, the operation completes successfully on subsequent attempts.

* CIDR allocation conflict error

   Description: This issue can occur when multiple requests attempt to allocate the same CIDR block at the same time, resulting in a failure.

   Workaround: Retry the operation after a short delay. The request typically completes successfully on retry.

* 500 Error from Cloudflare when creating public address ranges

   Description: You might see a `500 Worker Threw Exception` error during the creation of a public address range. This error originates from Cloudflare’s edge layer before the request reaches IBM  servers. 

   Workaround: Retry the request. This issue is transient, and most retries are successful.

* 502 Bad Gateway errors when creating or updating public address ranges

   Description: A `502 Bad Gateway` error might occur when creating or updating a public address range. This happens when a connection to the backend is closed before the server finishes process the request, such as in scenarios involving client-side timeouts, transient network issues, or high system load.

   Workaround: Retry the operation. The backend is healthy, and most retries complete successfully.

## Related links
{: #known-issues-par-related-link} 

[About public address ranges](/docs/vpc?topic=vpc-about-par)
