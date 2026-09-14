# Xai.mainnet.public.nodes

Honest **one-sandbox density test** for Kaspa mainnet public nodes on a **Grok Bot Linux box**.

> **Target is no longer 200.** That number was a stress fantasy. This repo reports the **measured ceiling** on one shared ~15 GiB sandbox.

## Honest limit (sandbox)

| Class | Honest ceiling | Measured (snapshot `2026-09-14T21:20:03Z`) |
|-------|---------------:|-----------------------------:|
| Primary archival mainnet | **1** | 1 |
| Fleet (non-archival, public via bore) | **~10** | **9** |
| **Total mainnet public processes** | **~11** | **10** |
| Old aspirational target | ~~200~~ | retired |
| TN10 | keep running | not counted as mainnet fleet |

### Why ~10, not 200

1. **One shared box** — every Grok agent shares the same Linux sandbox. New agents do **not** create new machines or RAM.
2. **~15 GiB RAM, no swap** — `swapon` is not permitted here. MemAvailable collapses once IBD spikes.
3. **Sibling workloads** — archival primary + TN10 (often multi‑GB RSS) + agent runtimes already eat most of the machine.
4. **IBD balloon** — each new `kaspad` can jump from tens of MB to **1–2 GiB** while syncing, even with `--ram-scale=0.1` (binary minimum).
5. **Public path cost** — each public node needs its own **bore** tunnel + `--externalip`; tunnels are cheap, **kaspad RSS** is not.
6. **Ephemeral `/tmp`** — box wipes lose datadirs; nodes re-IBD and spike RAM again.

So the honest test for **this** environment is: **pack ~10 public fleet slots beside 1 archival primary**, stop at a memory floor, and publish **alive counts** — never invent peers or map listings.

### Primary (now)
- Advertise: `159.223.110.159:28492`
- Mode: `--archival`, RPC localhost only
- Verify: https://arewepublicyet.com (Address + Port from above)

### Fleet endpoints alive at snapshot
- `slot-005`: `159.223.110.159:38457`
- `slot-007`: `159.223.110.159:8968`
- `slot-013`: `159.223.110.159:9274`
- `slot-016`: `159.223.110.159:44463`
- `slot-018`: `159.223.110.159:65155`
- `slot-019`: `159.223.110.159:12212`
- `slot-020`: `159.223.110.159:51107`
- `slot-021`: `159.223.110.159:7228`
- `slot-022`: `159.223.110.159:40589`


MemAvailable at snapshot: **3109 MB**.

## How we tested

1. Official rusty-kaspa `kaspad` on the Grok Bot box.
2. Keep primary archival + TN10 (TN10 protected).
3. Fleet slots under `/tmp/kaspa-fleet/slot-NNN/` with low peer caps, unique ports, one bore each.
4. Launch until MemAvailable floor (~120–400 MB depending on run).
5. Counter bot reads `/tmp/kaspa-fleet/status.json` — honest alive/public only.

Launcher scripts now use **`TARGET=10`** (honest ceiling), not 200.

## Why this report exists

Prove what **Grok Bot + Kaspa** can actually host on **one** sandbox. Best practice = measure and document limits; do not claim 200 on a single box.

## Limits (plain language)

| Limit | Meaning |
|-------|---------|
| Shared sandbox | All bots → one computer |
| No cloud in this test | Owner chose box-only densify (no Hetzner VMs) |
| No swap | Cannot page past physical RAM |
| `ram-scale` floor **0.1** | Kaspad rejects lower |
| `/tmp` wipe | Fleet + datadir can vanish; IBD restarts |
| kaspa.stream lag | arewepublicyet = live reachability; map listing is eventual |
| Price talk | Unrelated explainer bots still forbid price talk |

True scale beyond ~10 requires **extra machines** (cloud/SSH hosts) — see [EXPAND.md](./EXPAND.md). This repo’s **sandbox test target stays ~10**.

## Best practice

See [BEST_PRACTICE.md](./BEST_PRACTICE.md).

## Related

- Stack: https://github.com/STP-KAS/Xai.Kaspa.node  
- Upstream: https://github.com/kaspanet/rusty-kaspa  

