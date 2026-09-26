# Expanding past one-box RAM (full gusto)

> **Historical, retired 25 Sep 2026.** This test is finished. The mainnet node on the box was stopped and wiped on 25 Sep 2026 at 16:46 CEST, and no node, fleet or tunnel from it is running. The extra-machine path in section 5 was never used (see `extra_machines: false` in RESULTS.json). See [README.md](README.md).

First density run on this Grok Bot sandbox stopped at **9 fleet + 1 archival** when MemAvailable hit ~400 MB. Target **200** is a stress ceiling, not a promise on one ~15 GiB box.

## What blocked us

| Lever | Result on this sandbox |
|-------|------------------------|
| More agents | **Does not add machines** — all agents share one Linux box |
| Swap file | `swapon` → **Operation not permitted** (no root / no swap) |
| Lower `--ram-scale` | Helps; IBD still balloons RSS (hundreds of MB → multi‑GB) |
| More bore tunnels | Cheap; **kaspad RSS** is the wall |
| Fake peers / fake map listings | **Forbidden** — only real `kaspad` |

## Solutions (ordered)

### 1. Soft-dial sibling workloads (same box)
- TN10 was ~**3.5 GiB** RSS — largest single consumer.
- Keep TN10 **alive**, but restart with lower `--ram-scale`, fewer `--outpeers` / `--maxinpeers`, miner `-t 1`.
- Do **not** wipe TN10 or primary archival datadirs.

### 2. Ultra-dense fleet flags
- Non-archival, `--ram-scale=0.05`, `--async-threads=1`, `--outpeers=1`, `--maxinpeers=2`.
- Unique localhost ports + one **bore** each → `--externalip=<bore-ipv4>:<port>`.
- Recycle any slot whose RSS explodes during IBD (>~900 MB) so count stays high.

### 3. Stagger IBD
- Launch in small batches; avoid N nodes all IBD-spiking at once.
- Prefer more thin public listeners over few fat syncing monsters.

### 4. Lower the memory floor (honest risk)
- Prior floor ~400 MB; expansion uses ~100 MB until starts fail.
- Stop on real OOM / failed starts — never invent alive counts.

### 5. True “no limit”: more machines
One Grok box cannot mint more RAM. To go beyond physical RAM:

| Path | How |
|------|-----|
| Extra cloud VMs | Hetzner / DO / AWS — one `kaspad` (or small fleet) per VM, same bore-or-public-IP pattern |
| Bare metal / home | Real inbound or tunnel; archival only where disk allows |
| Multiple Grok users/sandboxes | Each sandbox is another independent box (not creatable from inside one agent) |
| Dedicated node host | Best for long-lived archival + public map presence |

Wire each host’s advertise address into that user’s `am i live node?` / fleet counter — **never** bake ephemeral ports into shared GitHub prompts.

### 6. Network / public path
- WARP/CGNAT → **bore** (or similar) required.
- Verify **arewepublicyet**; treat **kaspa.stream** as eventual.
- Refresh stale tunnels (TCP accept, dead relay).

## What we will not do

- Claim 200 without 200 live processes.
- Spam the network with fake identities.
- Kill primary archival data for density vanity.
- Stop TN10 permanently unless the owner says so (soft-dial only).

## Measure

During the test: `/tmp/kaspa-fleet/status.json` + `kaspa fleet counter` bot — alive / public_ok only. Both are gone; the final numbers are in [fleet-status.json](fleet-status.json).
