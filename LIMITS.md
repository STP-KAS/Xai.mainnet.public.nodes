# Limits — one Grok Bot sandbox

> **Historical, retired 25 Sep 2026.** This test is finished. The mainnet node on the box was stopped and wiped on 25 Sep 2026 at 16:46 CEST, and no node, fleet or tunnel from it is running. The limits below describe the sandbox as measured on 14 Sep 2026. See [README.md](README.md).

## Hard facts

- **RAM:** ~15 GiB shared; **swap not available** on this sandbox.
- **Compute share:** kaspad fleet + archival primary + TN10 + many agent Node processes.
- **kaspad:** `--ram-scale` cannot go below **0.1**; IBD still balloons RSS.
- **Agents ≠ machines:** creating more Grok bots does not add RAM or CPUs.
- **Disk:** `/tmp` is ephemeral; wipes force fresh IBD.

## Honest density (measured)

| Workload | Practical ceiling on this box |
|----------|-------------------------------|
| Archival primary (public via bore) | **1** (keep this) |
| Non-archival public fleet | **~8–10** before MemAvailable collapses |
| TN10 | Keep; costs multi‑GB if not soft-dialed |

**Retired:** target **200** on one box.

## What “success” means here

- Real `kaspad` processes with real bore advertise addresses.
- `status.json` alive/public_ok counts that match `ps`.
- arewepublicyet checks — not fake `kaspa.stream` claims.

## What will not work on one box

- 200 independent public mainnet nodes.
- Minting “new machines” by creating new agents.
- Swap-backed overcommit (blocked).
