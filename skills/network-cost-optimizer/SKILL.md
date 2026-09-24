---
name: network-cost-optimizer
description: Analyzes a customer's existing network circuit inventory (spreadsheet of leased lines, cross-connects, WAN links, exchange drops) and produces a Google Cloud networking modernization and cost-savings case. Use when a user uploads or references a network connectivity inventory and asks how Google Cloud could replace, enhance, or reduce the cost of that setup. Also use for follow-on artefacts: savings models, competitive comparison vs AWS/Azure, phased migration plans, and executive proposal documents.
---

# Network Cost Optimizer

Turns a raw circuit inventory into a defensible Google Cloud networking value case: where the money is going, what replaces it, what it saves, and how to migrate.

## When to use

- A customer network/circuit inventory is supplied (xlsx/csv) with endpoint, vendor, bandwidth, and service-type columns.
- The ask is cost reduction, consolidation, modernization, or "how can Google Cloud help here" against existing connectivity spend.
- Follow-ups: per-entity or per-region migration plans, competitive positioning, exec briefing docs, slide decks.

## Step 1 — Parse and classify the inventory

Delegate the file to the code/file agent. Required outputs:

1. **Total circuit count** — state it explicitly and confirm no rows excluded.
2. **Classify every circuit by A-side / Z-side endpoints:**
   - `A-side == Z-side` → **intra-facility cross-connect (XC)**. Pure colocation fee, highest-confidence savings target.
   - Different sites, same metro → **metro/local loop**.
   - Different regions/continents → **long-haul backbone (GWAN)**.
3. **Group by service type** — typically Customer, Exchange, GWAN, Extranet, Internet, Office. Report count + aggregate bandwidth for each.
4. **Group by entity/business unit** — surfaces M&A duplication where acquired brands run parallel infrastructure in the *same* facility.
5. **Group by vendor** — count distinct carriers/colo providers to quantify vendor sprawl.
6. **Identify parallel duplicates** — circuits sharing identical A-side, Z-side, service, vendor, and bandwidth.

## Step 2 — Build the savings model

Apply per-category monthly recurring cost (MRC) assumptions. Defaults when actual billing data is unavailable — **always label these as assumptions**:

| Category | Default MRC | Typical reduction target |
|---|---|---|
| Physical cross-connect | $350/mo | 80% eliminated |
| Customer leased line / local loop | $1,000/mo | 50%+ virtualized |
| Long-haul GWAN circuit | $4,000/mo | 70% decommissioned |

Present as: Baseline Annual → Target Annual → Net Savings, with a total row and a % reduction. Flag that the model excludes categories not yet costed.

## Step 3 — Map circuits to Google Cloud technology

| Legacy pattern | Google Cloud replacement | Why it works |
|---|---|---|
| Intra-facility cross-connects | Dedicated Interconnect (100G pairs) + VLAN segmentation | Tenant isolation moves from physical cabling to software; collapses many ports into few |
| Client leased lines | **Private Service Connect (PSC)** | Publish service once as a Service Attachment; clients self-provision endpoints. Native NAT removes IP overlap. Minutes vs. months |
| Long-haul WAN | **Network Connectivity Center (NCC)** | Global transit over Google-owned fiber; one hub construct, not region-anchored |
| Clients in AWS/Azure | **Cross-Cloud Interconnect** | Direct managed link into rival clouds — no telco middleman |
| On-prem-only clients | Partner Interconnect / Cloud VPN | Bridge for clients with no cloud presence |
| M&A network silos | Shared VPC + VPC Peering | Google VPCs are global by default; unifies entities without per-region meshing |

## Step 4 — Cover all three client postures

Any client-connectivity recommendation MUST address all three:

- **Already on Google Cloud** → PSC endpoint, self-service, <15 min.
- **On AWS/Azure** → PSC + Cross-Cloud Interconnect.
- **On-prem only** → Partner Interconnect or Cloud VPN bridge; phase later.

Never assume 100% client cloud readiness — this is the most common flaw in the savings model.

## Step 5 — Competitive positioning (when asked)

Be honest about parity, sharp about difference:

- **Parity:** Dedicated Interconnect ≈ AWS Direct Connect ≈ Azure ExpressRoute on raw port speed. Don't overclaim.
- **Genuine Google differentiators:**
  1. Sole ownership of 17 of 33 subsea cables (AWS 4, Azure 6, both partial/consortium) → structural transit cost advantage.
  2. **Cross-Cloud Interconnect** — no first-party AWS/Azure equivalent.
  3. **Global VPC by default** vs. regional VPCs/VNets needing TGW or Virtual WAN meshing.
  4. NCC's globally-scoped hub vs. Azure's regional hubs.
- **Counter-objection:** if pressed on routing flexibility vs. AWS Cloud WAN, point to NCC Router Appliance spokes and NCC Gateway/SSE integration.

## Step 6 — Phased migration template

Reusable per entity, region, or circuit cohort:

1. **Discovery & Dependency Mapping** (wks 1–3) — circuit-to-application map; split client-facing (→PSC) from internal (→Shared VPC).
2. **Cloud Landing Zone** (wks 2–5, parallel) — projects, Interconnect pairs, Shared VPC, PSC Service Attachment.
3. **Pilot** (wks 6–9) — 10–15 lowest-risk circuits; validate latency, NAT isolation, failover.
4. **Client Readiness Waves** (wks 10–20) — Wave A GCP-native, Wave B multi-cloud, Wave C on-prem bridge.
5. **Decommissioning** (wks 18–22) — formal disconnect notices; confirm billing drops to $0.
6. **Reconciliation** (wks 23–24) — realized vs. modelled savings; greenlight next cohort.

## Step 7 — Always flag these pitfalls

- **100% Cloud-Readiness Fallacy**: Assuming all external clients or B2B partners are ready for cloud-native endpoints on Day 1. Always include a legacy hybrid bridge (Partner Interconnect/VPN) option.
- **Double-Run Telecom Costs**: Failing to model overlapping billing periods when parallel legacy circuits remain active during migration waves.
- **Ignoring Egress & NAT Costs**: Omitting cloud egress tariffs and Private Service Connect NAT gateway throughput fees when comparing against flat leased line rates.
- **Neglecting Subsea/Cross-Region Transit Routing**: Assuming hairpinned regional connections will match latency of direct subsea or long-haul leased lines without Network Connectivity Center global routing.
