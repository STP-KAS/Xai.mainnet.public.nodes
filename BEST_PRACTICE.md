# Best practice — Kaspa nodes on Grok Bot (LLM operators)

## What Grok Bot is (and is not)

- Each Grok agent has a chat and persona; **all of a user’s agents share one Linux “box”**.
- Creating 200 agents does **not** create 200 machines.
- The node runs on the **sandbox**, not on the user’s phone/Windows PC.

## Operator pattern that works

| Role | Agent | Job |
|------|-------|-----|
| Operator | `kaspa bot` | Only paste target; runs kaspad, keepalive, bore |
| Tip ticker | `Kaspa node live bot` | Headers from local logs |
| News | `kaspa update` | Tech digests + cadence |
| Help | `kaspa help` | Discord-first; no seeds |
| Explainer | `what is kaspa?` | Tech only; **price talk forbidden** |
| Public card | `am i live node?` | Dynamic IP from tunnel file + arewepublicyet |
| Fleet (optional) | `kaspa fleet counter` | Honest alive/public counts |

Bootstrap: https://github.com/STP-KAS/Xai.Kaspa.node/blob/main/START.md

## Public path (WARP / CGNAT)

1. Listen on localhost P2P.
2. Expose with **bore** (or similar TCP tunnel).
3. Set `--externalip=<tunnel-ipv4>:<port>` (IP, not hostname).
4. Save `/tmp/kaspa-tunnel.addr`.
5. Verify **arewepublicyet** before claiming public.
6. Treat **kaspa.stream** as eventual — never claim listed until CHECK says so.
7. Stale tunnels: TCP open but handshake fails → **restart bore** and retarget `--externalip`.

## Fleet / density tests

- Keep archival **primary** separate from disposable fleet slots.
- Fleet nodes: non-archival, low `--ram-scale`, unique ports, one bore each.
- Stop at a memory floor so the box and protected workloads survive.
- Publish **alive count**, not the aspirational target.
- Do not spam the network with fake identities; only real `kaspad` processes.

## LLM hygiene

- Action + evidence over lectures.
- Screenshots / API JSON for public checks.
- Shared GitHub docs stay generic; per-user ports stay on the box.
- Only push GitHub when the owner asks (unless they request a new report repo).
- Customize freely after setup; keep the price lock on explainers.

## Limits (honest)

On a ~15 GiB sandbox already running archival mainnet + TN10, a “200 public mainnet nodes” target is a **stress ceiling**, not a promise. Expect a small honest fleet until RAM stops you.

## Honest sandbox ceiling

On one ~15 GiB Grok box beside archival + TN10, expect **~10** public fleet nodes, not 200. See LIMITS.md.
