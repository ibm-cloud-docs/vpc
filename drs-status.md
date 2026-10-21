---

copyright:
  years: 2026
lastupdated: "2026-10-21"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Dynamic route server status and descriptions
{: #drs-status}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

The list of dynamic route servers in {{site.data.keyword.cloud_notm}} VPC includes columns for lifecycle status and health status.
{: shortdesc}

## Lifecycle status
{: #lifecycle-status}

The lifecycle status indicates the administrative and operational state of the dynamic route server instance. The following table shows the possible dynamic route server lifecycle statuses and a description of what each status means.

| Lifecycle status | Description |
| -------------- | --------- |
| Pending | The dynamic route server instance is created, but provisioning is still in progress. Appliances are being created and initialized. |
| Stable | The dynamic route server instance is fully provisioned and operating as expected. You can configure peers, peer groups, and routing policies. |
| Updating | The dynamic route server instance is being updated. |
| Suspended | The dynamic route server instance is suspended. All service connections and appliances are stopped. Contact IBM Support for more information. |
| Deleting | The dynamic route server instance is being deleted. Associated appliances, service connections, and managed routes are being removed. |
| Failed | The dynamic route server instance is unreachable or inoperative. |
{: caption="Lifecycle status descriptions" caption-side="bottom"}

## Health status
{: #health-status}

The health status reflects the runtime health of the dynamic route server instance, based on appliance availability and peer connectivity. The following table shows the possible dynamic route server health statuses and a description of what each status means.

| Health status | Description |
| -------------- | --------- |
| Healthy | No abnormal behavior is detected. |
| Degraded | One or more appliances or peer connections are unhealthy, but the service is still partially operational. Route management might be impacted. |
| Faulted | The dynamic route server instance is unreachable or inoperative. Appliance health checks or internal dependencies have failed. |
| Inapplicable | The health status doesn't apply because of the current lifecycle status of the dynamic route server. For example, a lifecycle status of `deleting` has a health status of `inapplicable`. |
{: caption="Health status descriptions" caption-side="bottom"}

## Peer group status
{: #peer-group-status}

The following table shows the aggregate session status of the peers in the dynamic route server peer group and a description of what each status means.

| Status | Description |
| -------------- | --------- |
| Degraded | One or more peers in the dynamic route server peer group are unable to establish a session. |
| Down | All peers in the dynamic route server peer group are unable to establish a session. |
| Initializing | The dynamic route server peer group is establishing sessions with its peers. |
| Unknown | The session status of the peers in the dynamic route server peer group can't be determined. |
| Up | All peers in the dynamic route server peer group have established sessions and are operating normally. |
{: caption="Peer group status descriptions" caption-side="bottom"}

## Peer connection status
{: #peer-connection-status}

The following table shows the possible statuses of a peer connection in a dynamic route server peer group and a description of what each status means.

| Status | Description |
| -------------- | --------- |
| Up | The peer is operating normally. |
| Down | The peer is not operational. |
{: caption="Peer connection status descriptions" caption-side="bottom"}

For error codes, reasons for failure, and suggested solutions for recovery, see [Diagnosing dynamic route server health](/docs/vpc?topic=vpc-drs-health).
{: note}
