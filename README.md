# Brewnix site-defense

**Status:** repo home locked 2026-09-08 (Chris) — implementation vertical not started  
**License:** [MIT](LICENSE)  
**Contracts:** pin [`Brewnix/inference-iface`](https://github.com/Brewnix/inference-iface) (schemas + locked docs). Do **not** fork or weaken those locks here.

Site-local sense → decide → act → receipt for cottage / Brewnix boxes. Policy + typed actuators execute. The LLM judges only. Offline-first: same JSON when Panopticon is unreachable.

## What lives here

| Area | This repo | Elsewhere |
|------|-----------|-----------|
| Locked schemas / policy docs | Pin / submodule `inference-iface` | [`Brewnix/inference-iface`](https://github.com/Brewnix/inference-iface) |
| Site policy executor + receipt chain | **Here** | — |
| OPNsense / Suricata actuators | **Here** (calls into site firewall) | Host image: [`Brewnix/proxmox-firewall`](https://github.com/Brewnix/proxmox-firewall) |
| `notify.operator` → fyber.auditor client | **Here** | Plane: Panopticon `#39` |
| Hypermesh preempt client + H3 drain | **Here** | Host H1–H4; plane `#40` / `#41` |
| Defense UI (digests / tickets / posture) | **Here** | — |
| Plane doors (`hm_site_`, auditor, site jobs, sell_state) | Client only | [`FyberLabs/panopticon`](https://github.com/FyberLabs/panopticon) |
| Host actuators | Client / H4 owner path | [`FyberLabs/hypermesh-host`](https://github.com/FyberLabs/hypermesh-host) |

**Not this repo:** Panopticon SaaS gateway fork, Hypermesh-router IR chat UI, `schemas/` amends, market/renter surfaces.

## Recommended vertical (pending greenlight)

`0 → 1 → 2 → 4 → 5` then package.

0. Pin iface + plane inventory freeze (docs)  
1. Zero-LLM OPNsense loop + `fyber.receipt/v0`  
2. `notify.operator` auditor client  
3. Incident side index (parallel after 1)  
4. Preempt client + H3 (`/site/jobs`, `/site/devices`, H4 offline)  
5. Defense UI v0  
6–8. Model triage, privilege grant, SociACL IR UX  
9. Package as one site daemon

## Plane doors (cite, don’t reimplement)

| Door | Panopticon |
|------|------------|
| Site token | `#38` — `hm_site_…` |
| fyber.auditor | `#39` — `/api/v1/auditor/v0/tickets` |
| Site jobs | `#40` — [`brewnix-executor-bridge-v0`](https://github.com/FyberLabs/panopticon/blob/main/products/hypermesh/docs/brewnix-executor-bridge-v0.md) |
| Live `sell_state` | `#41` — [`heartbeat-sell-state-v0`](https://github.com/FyberLabs/panopticon/blob/main/products/hypermesh/docs/heartbeat-sell-state-v0.md) |

## Axioms (from iface)

1. LLM never executes.  
2. Receipts = digests / policy / effects / hash chain — no prompts or payloads.  
3. Offline-first.  
4. `hypermesh.*` tools never implied by `rails_profile` alone.  
5. Enqueue ≠ apply; Host / OPNsense apply.

See [`docs/slice-0-inventory.md`](docs/slice-0-inventory.md) for the freeze checklist.
