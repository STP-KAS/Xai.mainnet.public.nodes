> **Experimental only. Not a product.**
>
> Do not use wallet integrations on this GitHub. STP remains a clown. [DISCLAIMER.md](DISCLAIMER.md)

# Xai.mainnet.public.nodes (retired)

> **Retired 25 Sep 2026. This experiment is finished.** The mainnet node on the box was stopped and wiped on **25 Sep 2026 at 16:46 CEST**. The archival node and all fleet nodes from this test are gone, and their endpoints no longer exist. **Do not connect to any address or port in this repo's history.** What is left is the historical result below.

## What was tested, and why

The question was simple: how many real, publicly reachable Kaspa **mainnet** nodes can one Grok Bot Linux sandbox run?

- Official rusty-kaspa `kaspad` ran on the Grok Bot box. There was one archival primary and a fleet of non-archival slots.
- Each fleet slot used low peer caps, its own ports, and its own bore tunnel so it was reachable from outside.
- Slots were launched until free memory reached a floor. Then the test stopped, and only nodes that were really alive and reachable were counted. No fake peers or map listings.
- The Testnet-10 node on the same box was protected and not counted.

The original target was 200 nodes. The point was to measure what one sandbox really holds instead of claiming 200.

## Result (snapshot 14 Sep 2026, 21:20:03 UTC)

| | Count |
|---|---:|
| Archival primary, public | 1 |
| Fleet slots launched | 22 |
| Fleet slots alive and public at snapshot | 9 |
| **Mainnet public processes at snapshot** | **10** |
| Honest ceiling on this box | ~10 fleet + 1 archival (~11) |
| Old target | 200 (dropped) |

The launcher stopped when free memory fell to 153 MB, below its 250 MB floor. At the snapshot, 3,109 MB was available. Raw data: [RESULTS.json](RESULTS.json) and [fleet-status.json](fleet-status.json).

**Why 200 was dropped:** one sandbox cannot hold it. Every Grok agent shares the same box, so more agents do not mean more machines or more RAM. And RAM ran out long before 200.

## Main limits found

Details: [LIMITS.md](LIMITS.md).

- **RAM:** ~15 GiB shared, and swap is not available on the sandbox.
- **Shared compute:** the archival primary, TN10 and the agent runtimes already use most of the machine.
- **IBD balloon:** a syncing `kaspad` grows from tens of MB to 1–2 GiB, even at `--ram-scale=0.1`, which is the lowest value kaspad accepts.
- **Ephemeral `/tmp`:** a box wipe loses the datadirs, and the nodes have to sync from scratch again.
- **Tunnels are cheap; kaspad memory is not.** Each public node needs its own tunnel and `--externalip`.

Going past about 10 nodes needs more machines, not more agents. This test never used extra machines (see [EXPAND.md](EXPAND.md)).

## What it turned into

The mainnet test was not continued. The box's node work moved to **Kaspa Testnet-10 only**: my own TN10 node and miners, transaction stress tests, and vprogs / covenant tests on 25–26 Sep 2026.

- Findings: [STP-KAS/tn10-vprogs-stress-findings](https://github.com/STP-KAS/tn10-vprogs-stress-findings)
- Forum thread for this test: https://kas-smiths.org/t/146

## Files

| File | What |
|---|---|
| [RESULTS.json](RESULTS.json) | Summary numbers of the 14 Sep snapshot (historical, marked retired) |
| [fleet-status.json](fleet-status.json) | Per-slot state at the snapshot (historical, marked retired; its endpoints are gone) |
| [LIMITS.md](LIMITS.md) | The sandbox limits behind the ceiling |
| [EXPAND.md](EXPAND.md) | Ideas for going past one box (historical; extra machines were never used) |
| [BEST_PRACTICE.md](BEST_PRACTICE.md) | Operator notes from the test (historical) |
| [POC-REVISITED.md](POC-REVISITED.md) | Unrelated desk note (kUSD PoC links) |
| [DISCLAIMER.md](DISCLAIMER.md) | Disclaimer |

Related: [STP-KAS/Xai.Kaspa.node](https://github.com/STP-KAS/Xai.Kaspa.node) (the node setup used) · upstream [kaspanet/rusty-kaspa](https://github.com/kaspanet/rusty-kaspa)

---

> **Standard disclaimer.** This GitHub, not the topic above.
>
> Intentions are good; thought process is questionable. STP remains delusional. Si vis pacem, para bellum.
>
> Intern at https://sixpack.wtf/  
> X: https://x.com/StppStp · GitHub: https://github.com/STP-KAS
