# Experimental protocol

This guide describes the experimental design.

## Research question

When does specialization in information discovery compensate for lost transport capacity in decentralized, energy-constrained search, pickup and delivery?

The matched HSR-1/Soft-HSR-1 comparison holds the scout's exploration profile and common controller mechanisms fixed and changes transport permission. HSR-0 versus TGR/VGE/AIR keeps all agents transport-capable and changes exploration-goal selection.

## Shared mission rules

- Static warehouse topology is known; object positions and object count are hidden from the policies. The evaluator knows the object count for metric normalization.
- Four directed warehouse entrances/exits are retained on each grid. All agents start at the outside cell `[0,0]`.
- One grid move consumes one energy unit.
- Local vision has graph-distance radius 3; walls block visibility. Radio uses Manhattan distance and synchronous, one-hop exchange.
- Radio −1 disables communication; 0 permits co-location only; 200 is effectively global for the tested grids.
- All policies share shortest-path routing, sensing, object allocation, local claims, pickup/delivery, energy accounting and safe return. Agents may overlap.
- Return safety uses exact directed distance to a depot plus a two-move margin. Parked agents do not move or sense but can remain radio relays while they have energy.
- Mission horizons are 376, 715 and 1,251 ticks for grid sizes 50, 75 and 100. They do not depend on team size or tested energy multiplier.
- Completion or horizon triggers mandatory return. A run ends when every agent is parked or exhausted; `active_outside` must be zero.

Mission outcomes at the horizon and end-to-end diagnostics are distinct. Mandatory post-horizon return can deliver carried objects without changing deadline metrics.

### Energy allocation

Let $T_{\mathrm{med}}(L)$ denote the number of traversable cells (i.e., empty, depot, entrance, or exit) in the medium-density reference grid of side length $L$; it follows, then, that the energy assigned to each agent is given by $e_i = \lfloor (k \times T_{\mathrm{med}}(L))/N \rfloor$ (for $i=\{1, \ldots, N\}$), with $k$ a constant set to $0.8$, and $N$ the agent number; while the team-size independent mission horizon (i.e., ticks) is given by

$$
H(L)=\left\lceil (k \times T_{\mathrm{med}}(L))/N_{ref}\right\rceil+\gamma,
$$

where $N_{ref}$ and $\gamma$ are user-defined parameters, set to $5$ and $100$ in our investigation, respectively. All agents are identical in sensing radius, radio range, energy budget, and movement capability; policies differ only in the assigned spatial profile and in whether an agent is permitted to transport objects.

Team energy is fixed within map size, split evenly with a seed-permuted remainder.

## Policies

| Policy | Roles and exploration |
|---|---|
| HSR-0 | Worker profiles; every agent can transport |
| HSR-1 | One scout profile barred from transport; remaining agents are workers |
| Soft-HSR-0 | Exact HSR-0 equivalence control |
| Soft-HSR-1 | Same scout profile, with transport permission restored |
| TGR | Transport-capable generalists with randomized unseen goals |
| VGE | Generalists using visibility gain relative to travel and return cost |
| AIR | VGE criterion reweighted by recent teammate waypoint intentions |
| Oracle | Information diagnostic: object positions known at tick zero |

HSR uses radial profiles and repulsive pheromone. Soft scouts may enter retrieval and carrying modes, so their realized trajectories can differ from hard scouts even though their exploration rule is unchanged.

## Maps and frozen splits

All instances contain 25 objects and four depots, with sizes 50/75/100, densities few/medium/full, and random/border placement. Topology and placement seeds are separate.

| Split | Topology seeds per size/density stratum | Object seeds | Topologies | Instances |
|---|---|---|---:|---:|
| Development | 1001–1005 | 11001–11002 | 45 | 180 |
| Test | 3001–3030 | 13001–13003 | 270 | 1,620 |
| Calibration reference | Supplied seed-42 instances | Supplied placements | 9 | 18 |

Generated maps belong to a family calibrated to the supplied reference maps, rather than i.i.d. replicas of them. The reference maps remain a separate descriptive dataset. The independent sampling unit is topology, not placement or policy seed.

## Development-only AIR tuning

Eight fixed candidate parameterizations were evaluated on development maps with teams 5/10, radio −1/0/4/8/200 and policy seeds 21001–21005: 72,000 runs.

The selection rule chose the least complex configuration within 0.25 percentage points of the highest development delivery AUC. The selected intent-only setting has `contact_stale=8`, `gossip_wait=0`, `gossip_threshold=0`, `intent_ttl=12`, `claim_ttl=12`.

## Main test and paired analysis

Eight configurations × 1,620 instances × two team sizes × five radio settings × five policy seeds (23001–23005) yield **648,000** main-study runs. Common settings include 64 exploration probes, vision 3, return margin 2 and claim TTL 12.

Primary endpoint: delivery AUC. Secondary comparisons include delivery ratio, secured objects, discovered objects, coverage and duplicate-target agent-ticks.

Policy seeds are averaged within placement; placements within topology. Balanced descriptive summaries weight environments equally. Effects are paired within topology, placement, seed, team and radio. Prespecified contrasts are AIR–VGE, AIR–HSR-0, Soft-HSR-1–HSR-1, HSR-0–HSR-1 and HSR-0–TGR. Reported 95% percentile intervals use 10,000 bootstrap replicates that resample complete topologies within the nine size/density strata.

## Additional campaigns

| Campaign | Recorded runs |
|---|---:|
| Main test | 648,000 |
| Claim TTL 0 versus 12 | 648,000 |
| HSR pheromone × radial-bias factorial | 648,000 |
| Hard-scout sweep, N=5 | 121,500 |
| Hard-scout sweep, N=10 | 162,000 |
| Calibration-reference replication | 7,200 |
| Development-only AIR tuning | 72,000 |
| **Total full study** | **2,306,700** |
