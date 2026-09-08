# Slice 0 — pin + inventory freeze

**Status:** draft checklist (repo home = this repo, 2026-09-08)  
**Goal:** freeze pins and plane URLs before coding slices 1+.

## Pins

| Dependency | How | Notes |
|------------|-----|-------|
| [`Brewnix/inference-iface`](https://github.com/Brewnix/inference-iface) | git submodule or release SHA | Schemas under `schemas/`; docs under `docs/`. No fork. |
| Panopticon plane | base URL env (e.g. `PANOPTICON_BASE_URL`) | Client only. |
| Host (optional plane-up) | via Panopticon site jobs | Offline: `$STATE_DIR/owner.sock` (H4). |

## Device bind checklist (plane)

Before site jobs / sell_state reads work:

1. Mint site machine token (`hm_site_…`) for this `site_id`.  
2. Operator `POST`/`PATCH` device with matching `Device.site_id`.  
3. Unbound / wrong site → **404** (by design).

## Plane surfaces this site will call

| Method | Path | Auth |
|--------|------|------|
| `POST`/`GET` | `/api/v1/auditor/v0/tickets…` | `hm_site_…` |
| `POST`/`GET` | `/api/v1/hypermesh/site/jobs` | `hm_site_…` |
| `GET` | `/api/v1/hypermesh/site/devices/{device_id}` | `hm_site_…` |

`lease_stop` on site jobs **requires** `lease_id` (no omit-for-active).

## Out of scope for this freeze

Site mTLS, public-internet design, GHCR/test-plane, privilege_grant plane store, SociACL hosting, forking Panopticon gateway.

## Open (Chris)

Remaining pressure-test items after repo home: Tailscale-only reachability, who binds `Device.site_id` in prod, UI SoT, grant timing, local-only model, dual auth, product naming, IR chat consume?, CI pin location.
