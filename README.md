# Xai.mainnet.public.nodes

**Stress / best-practice report** — how many real Kaspa **mainnet** public nodes one Grok Bot Linux sandbox can run beside existing workloads.

> Honest numbers only. Target was **200**. This box did **not** invent peers or fake `kaspa.stream` listings.

## Amount (snapshot `2026-09-14T20:20:55Z`)

| Class | Count | Notes |
|-------|------:|-------|
| Primary archival mainnet | **1** | Long-lived operator node; public via bore |
| Fleet mainnet (non-archival) | **9** | New slots launched this test; each has own bore |
| **Total mainnet public processes** | **10** | Alive on this box at snapshot |
| Target | 200 | Aspirational ceiling for the test |
| TN10 | kept running | Not stopped; not counted as mainnet fleet |

### Primary
- Advertise: `159.223.110.159:41363`
- Mode: `--archival`, tip-following, RPC localhost only
- Verify: https://arewepublicyet.com (Address + Port from above)

### Fleet endpoints (alive at snapshot)
- slot-001: `159.223.110.159:36216`
- slot-002: `159.223.110.159:15364`
- slot-003: `159.223.110.159:5193`
- slot-004: `159.223.110.159:2749`
- slot-005: `159.223.110.159:52798`
- slot-006: `159.223.110.159:38418`
- slot-007: `159.223.110.159:10134`
- slot-008: `159.223.110.159:1220`
- slot-009: `159.223.110.159:26502`


MemAvailable at snapshot: **415 MB**. Launch stops near ~400 MB free so the box and TN10 survive.

## How

1. **One shared Grok Bot box** — new agents do **not** create new computers; all agents share one Linux sandbox.
2. **Keep existing workloads** — primary archival mainnet + TN10 stayed up (TN10 explicitly protected).
3. **Fleet slots** under `/tmp/kaspa-fleet/slot-NNN/`:
   - Official `kaspad` binary (rusty-kaspa Linux amd64)
   - Non-archival, `--ram-scale=0.15`, low peer caps
   - Unique localhost P2P + RPC ports
   - **bore** `local <p2p> --to bore.pub` → `--externalip=<bore-ipv4>:<port>`
4. **Launch until RAM floor** — add slots one-by-one; stop when memory is too tight or starts fail (honest N ≪ 200).
5. **Counter bot** `kaspa fleet counter` reads `/tmp/kaspa-fleet/status.json` + primary tunnel; never invents counts.
6. **Public check** — https://arewepublicyet.com first; https://kaspa.stream/nodes can lag.

Scripts live on the box: `/workspace/artifacts/kaspa/fleet-launch.sh`, `fleet-status.sh` (when present).

## Why

- Prove what **Grok Bot + Kaspa** can actually host on one sandbox (best practice = measure, don’t claim).
- Learn bore/WARP public-path limits when many tunnels share one egress.
- Keep a reusable pattern: **one operator bot + companions + optional fleet + counter**, without poisoning docs with one user’s ephemeral bore ports.

## Best practice (Kaspa × Grok LLM)

See [BEST_PRACTICE.md](./BEST_PRACTICE.md).

Short version:

1. **GitHub-first START** for stack setup ([Xai.Kaspa.node](https://github.com/STP-KAS/Xai.Kaspa.node)).
2. **One input agent** (`kaspa bot`); companions read-only day-to-day.
3. **Never bake ephemeral bore IP:ports into the shared prompt** — each user/sandbox reads `/tmp/kaspa-tunnel.addr`.
4. **arewepublicyet** = live reachability; **kaspa.stream** = map (can lag).
5. **Price talk forbidden** on explainer bots.
6. **Fleet tests report real N**, not the target.
7. **Don’t kill sibling workloads** (TN10) unless the owner says so.
8. **Refresh bore** when TCP accepts but relay is dead.

## Related

- Stack prompt: https://github.com/STP-KAS/Xai.Kaspa.node  
- Upstream: https://github.com/kaspanet/rusty-kaspa  

