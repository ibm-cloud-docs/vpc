---

copyright:
  years: 2026
lastupdated: "2026-10-21"

keywords: dynamic route server, routing table, zone priority, route insertion, ingress, egress

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Route insertion behavior for dynamic route server
{: #drs-route-insertion}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

When a dynamic route server policy is configured to update a VPC routing table, the dynamic route server adds routes on behalf of healthy next-hop peers. The rules for which routes are added, and how many, depend on whether the target routing table is an ingress table or an egress table.
{: shortdesc}

## Which peers contribute routes
{: #drs-route-insertion-eligible-peers}

A peer contributes routes to a routing table only when all of the following conditions are met:

- The peer group has a `Stable` lifecycle status and its peer group status is not `Down`.
- The peer itself is not being deleted and its status is not `Down`.

Peers that do not meet these conditions are excluded from route insertion until they recover.

## Routes in ingress routing tables
{: #drs-route-insertion-ingress}

For ingress routing tables, the dynamic route server adds one route entry per healthy peer. Each route entry is placed in the zone where the peer resides and uses that peer's IP address as the next hop.

| Property | Value |
| --- | --- |
| Zone of route | The peer's availability zone |
| Next hop | The peer's IP address |
| Priority | The priority configured on the peer |
{: caption="Ingress route properties" caption-side="bottom"}

For example, if two healthy peers exist, one in `us-south-1` and one in `us-south-2`, and both are associated with ingress routing table `rt-aaa`, the following routes are added:

| Route zone | Routing table | Next hop |
| --- | --- | --- |
| `us-south-1` | `rt-aaa` | Peer IP in `us-south-1` |
| `us-south-2` | `rt-aaa` | Peer IP in `us-south-2` |
{: caption="Example ingress route entries" caption-side="bottom"}

## Routes in egress routing tables
{: #drs-route-insertion-egress}

For egress routing tables, traffic that originates in any zone might need to reach a peer in a different zone. Therefore, the dynamic route server evaluates every healthy peer as a candidate for every zone in the region, not just the peer's own zone.

### Candidate selection and ranking
{: #drs-route-insertion-egress-ranking}

For each zone, the dynamic route server generates route candidates and selects the best ones by using the following process:

#### Step 1: Generate candidates
{: #drs-ranking-step-1}

Each healthy peer produces one route candidate for each zone in the region:

| Candidate | Route zone | Zone relationship to peer |
| --- | --- | --- |
| Local candidate | The peer's own zone | Same zone |
| Cross-zone candidates | All other zones in the region | Different zone |
{: caption="Route candidates generated per peer" caption-side="bottom"}

All candidates for the same peer carry the same priority value.

#### Step 2: Rank candidates
{: #drs-ranking-step-2}

For each combination of route zone and destination, candidates are ranked in the following order:

| Rank | Criterion | Details |
| --- | --- | --- |
| 1 | Priority (ascending) | A lower priority number is preferred. |
| 2 | Zone locality | At equal priority, a peer in the route's own zone is preferred over a cross-zone peer. |
| 3 | Peer IP address (ascending) | Used as a deterministic tie-breaker when priority and locality are equal. The one with the lower IP address is preferred. |
{: caption="Candidate ranking criteria" caption-side="bottom"}

#### Step 3: Select routes
{: #drs-ranking-step-3}

- The highest-ranked candidate always receives a route entry.
- The second-ranked candidate also receives a route entry only if its priority equals the first candidate's priority.
- Route entries for the same destination in the same zone must all have the same priority. A maximum of two route entries are added per zone.

### Example
{: #drs-route-insertion-egress-example}

Consider three peers, one per zone, with the following configuration:

| Peer | Zone | Priority | IP address |
| --- | --- | --- | --- |
| Peer A | `us-south-1` | 1 | `10.0.1.5` |
| Peer B | `us-south-2` | 1 | `10.0.2.5` |
| Peer C | `us-south-3` | 2 | `10.0.3.5` |
{: caption="Example peer configuration" caption-side="bottom"}

The dynamic route server adds the following routes for each zone:

| Route zone | Routes added | Reason |
| --- | --- | --- |
| `us-south-1` | Peer A (`10.0.1.5`), Peer B (`10.0.2.5`) | Peers A and B share priority 1. Peer C has a higher priority number and is excluded. |
| `us-south-2` | Peer B (`10.0.2.5`), Peer A (`10.0.1.5`) | Same reasoning. Peer B is local to this zone and ranks first whereas Peer A ranks second at equal priority. |
| `us-south-3` | Peer A (`10.0.1.5`), Peer B (`10.0.2.5`) | Peers A and B have priority 1. Peer C has priority 2 and is excluded even though it is local to this zone. Between A and B, Peer A ranks first on IP address. |
{: caption="Example egress route entries by zone" caption-side="bottom"}

## Active-active and active-passive configurations
{: #drs-route-insertion-active-active-passive}

The ranking rules in the previous section always run the same way. The routes that result depend on how you set peer priorities.

An active-active configuration results when you assign the same priority to multiple peers. Step 3 allows both the first-ranked and second-ranked candidates to be inserted when they have the same priority value, so all peers at that level receive route entries simultaneously. Traffic is distributed across them at the same time by using equal-cost multipath (ECMP).

An active-passive configuration results when you assign different priorities to the peers. The peer with the lowest priority number always wins the ranking, and Step 3 inserts only that peer's route. Every other peer is excluded from the routing table entirely; it is not inserted at a lower weight. These peers remain in the ranking logic's consideration set, so they are reevaluated whenever the set of healthy peers changes. When the active peer goes down and is removed from the candidate set, the next-best peer becomes the top-ranked candidate and is inserted.

You can also mix both patterns in the same policy: two peers at priority 1 give you active-active between the two peers, while a third peer at priority 2 stays passive until both priority-1 peers fail.

## Ingress and egress comparison
{: #drs-route-insertion-comparison}

The following table summarizes the key differences in how route entries are added to each routing table type.

| Property | Ingress routing table | Egress routing table |
| --- | --- | --- |
| Route entries per table | One per healthy peer | Up to two per zone |
| Zone of each route entry | The peer's own zone only | Any zone in the region |
| Selection logic | All healthy peers are used | Ranked by priority, then zone locality, then IP address |
{: caption="Ingress and egress route insertion comparison" caption-side="bottom"}

## Route insertion for static routes
{: #drs-route-insertion-static-routes}

Static routes follow the same ingress and egress insertion rules described previously, but the inputs are different. For learned routes, the zone, priority, and IP address used in the ranking logic come from the peer configuration, and the dynamic route server derives those values automatically as peers establish BGP sessions. For static routes, those values are derived from the static route definition that you provide.

When you create a static route, you specify:

- The destination IP address
- The next-hop peer group
- The routing tables to update
- Whether to advertise the route

The dynamic route server uses the zone and priority of each healthy peer in the specified next-hop peer group to determine which route entries to add, applying the same ranking rules. The difference is that no BGP session is required to supply those values; they are read directly from your static route configuration.

This configuration means that static routes are also subject to active-active and active-passive behavior. If the peers in the next-hop peer group have the same priority, multiple route entries are inserted simultaneously. If the peers have different priorities, only the best-priority peer's route is inserted, and the others are held as passive fallback.

## Related links
{: #drs-route-insertion-related}

- See [Creating a peer group policy](/docs/vpc?topic=vpc-drs-create-peer-group-policy) to configure the VPC routing table policy type that drives route insertion.
- See [Diagnosing dynamic route server health](/docs/vpc?topic=vpc-drs-health) for error codes and recovery guidance when routes are not added as expected.
