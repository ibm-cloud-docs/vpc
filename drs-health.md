---

copyright:
  years: 2026
lastupdated: "2026-10-21"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Diagnosing dynamic route server health
{: #drs-health}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

Use this table to identify the health of dynamic route servers, find the reasons for failure, and find solutions for recovery.

| Code | Message | Information |
| ------------- | -------- | ---------------- |
| `cannot_start_capacity` | `Cannot start one or more members because resource capacity is unavailable.` | This issue occurs when sufficient infrastructure capacity isn't available to start one or more dynamic route server members. Retry the operation after some time or reduce resource pressure. If the issue persists, contact IBM Support. |
{: caption="Dynamic route server health reasons" caption-side="bottom"}

## Diagnosing dynamic route server member health
{: #drs-member-health}

Use this table to identify the health of dynamic route server members, find the reasons for failure, and find solutions for recovery.

| Code | Message | Information |
| ------------- | -------- | ---------------- |
| `cannot_start_free_range_routing_daemon` | `The dynamic route server cannot start due to an error while starting the free_range_routing_daemon.` | The routing daemon failed to start during initialization. This error can prevent BGP operations. Retry the operation or re-create the resource. If the issue persists, contact IBM Support. |
| `cannot_start_log_agent` | `An error occurred while starting the log agent.` | The logging component failed to initialize. While this error might not immediately stop routing, it can affect observability. Retry the operation and make sure that sufficient system resources are available. |
| `cannot_start_database_client` | `An error occurred while starting the database client.` | The internal database client failed to start, which can affect configuration and state management. Retry the operation or re-create the member. |
| `read_only_filesystem_failure` | `An error occurred while creating new files.` | The file system became read-only, preventing required operations. This error typically occurs at the infrastructure level. Contact IBM Support. |
{: caption="Dynamic route server member status reasons" caption-side="bottom"}

## Diagnosing dynamic route server member operation health
{: #drs-member-operation-health}

Use this table to identify failures during dynamic route server member operations.

| Code | Message | Information |
| ------------- | -------- | ---------------- |
| `member_configuration_failed` | `The dynamic route server member operation failed due to invalid BGP configuration.` | The member configuration contains invalid or unsupported BGP settings (for example, ASN conflicts or incorrect peer configuration). Verify BGP parameters and retry. |
| `member_action_failed` | `An error occurred while performing an action on the member.` | The requested operation (such as update, restart, or delete) failed. Retry the operation and verify the configuration. If the issue persists, contact IBM Support. |
{: caption="Dynamic route server member operation status reasons" caption-side="bottom"}

## Diagnosing dynamic route server peer group connection health
{: #drs-peer-group-health}

Use this table to identify the health of peer group connections, including transit gateway connections.

| Code | Message | Information |
| ------------- | -------- | ---------------- |
| `peer_not_responding` | `The connection is down because the peer is not responding.` | The peer is unreachable or not responding to connection attempts. Verify peer configuration, connectivity, and routing. |
| `internal_error` | `Internal error.` | An internal error occurred within the service. Retry the operation or contact IBM Support if the issue persists. |
{: caption="Dynamic route server peer group connection status reasons" caption-side="bottom"}


## Diagnosing dynamic route server static route failures
{: #drs-static-route-failure}

Use this table to identify issues when you add static routes to VPC routing tables.

| Code | Message | Information |
| ------------- | -------- | ---------------- |
| `route_not_added` | `The route could not be added to the routing table.` | This issue occurs when the static route cannot be applied to the VPC routing table. Possible causes include invalid destination CIDR, conflicts with existing routes, or routing table limits. Verify the route configuration and ensure that no conflicts or quota limitations exist. |
{: caption="Dynamic route server static route status reasons" caption-side="bottom"}
