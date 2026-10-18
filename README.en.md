# hgrac-route-checker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain Profile](https://img.shields.io/badge/domain-bio%20%7C%20domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED%20%7C%20open%20claim-orange.svg)](domain.manifest.json)
[![Domain: HGRAC](https://img.shields.io/badge/domain-human%20genetic%20resource-8a2be2.svg)](domain.manifest.json)
[![Privacy: default-off](https://img.shields.io/badge/privacy-default--off-brightgreen.svg)](privacy.json)

> **Status: RESERVED (placeholder · open for claim)** — this repository ships the four-piece industry-access skeleton already built to the platform protocol, and waits for an operator with the proper domain qualification to claim it and fill in the real rules.

It turns "does this human-genetic-resource activity need filing, and which route" into verifiable attributes: the AI only surfaces the route and requirements — it does **not** issue an approval.

## What this domain covers

Determination of the approval route for human genetic resource (HGRAC) activities. Collection, preservation, utilization and provision-abroad fall under different routes and touch national and biosafety. The AI lays out the route and requirements; approval stays with the administrative authority.

## Quick start

This repo is a **domain-profile skeleton** (four JSON files), not a runnable application. Validate with standard JSON tooling:

```bash
git clone https://github.com/SynomosAI/hgrac-route-checker.git
cd hgrac-route-checker
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json        > /dev/null
python -m json.tool privacy.json         > /dev/null
python -m json.tool checker.io.json      > /dev/null
```

The checker (MCP tool) IO contract is in `checker.io.json`. A minimal call:

```json
{
  "subject_ref": "mpp:cn:5e2a9b7c",
  "attribute_keys": ["bio.hgrac_activity", "bio.hgrac_approval_status"],
  "evidence": ["tsa:2026-09-12#sha256:c3d4..."],
  "request_id": "req-20260912-0007"
}
```

> The checker has exactly one action — *show and verify*. It has **no "judge" option**: "approved and may be provided abroad", "may be open-access", "approval passed" are all on the `_forbidden_outputs` blacklist.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `bio` (globally unique; never reused) |
| Domain name | Bio · Human genetic resources |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Placeholder since | 2026-09-12 |

## Attributes

| Key | Type | Meaning |
|---|---|---|
| `bio.hgrac_activity` | enum | Activity type setting the applicable route · collection/preservation/utilization/provision_abroad/open_access |
| `bio.hgrac_approval_status` | enum | Current administrative-license status · not_required/pending/approved/rejected/expired/unknown |
| `bio.entity_qualification` | enum | Chinese-entity qualification and foreign-entity nature · qualified/unqualified/foreign_entity/under_review |
| `bio.resource_type` | enum | Resource form setting compliance intensity · organ/tissue/cell/blood/genetic_data/other |
| `bio.biosafety_level` | enum | Lab biosafety level · bsl1/bsl2/bsl3/bsl4/not_applicable |

## Red lines (machine-readable)

1. Without an administrative license, no "may be provided abroad" / "may be open-access" wording.
2. Must not advise splitting filings or renaming activities to evade approval.
3. No "approval passed" conclusion; the authority's written decision is the sole basis.

Each red line has a corresponding blocking rule in `gate-map.json`.

## The four-piece access set

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attribute list, issuer requirements, validity, red lines |
| `gate-map.json` | Friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy declaration: default-off, minimal, withdrawable, deletable |
| `checker.io.json` | Checker (MCP tool) IO contract — show & verify, never judge |

## References

Regulations on the Administration of Human Genetic Resources; its implementation rules; Biosafety Law of the PRC. References only explain the source of attributes; they do not constitute legal advice.

## How to claim

1. Fork this repo and fill in `operator` and `checker_endpoint`.
2. Calibrate attribute values and red-line wording per currently effective rules.
3. Run the platform-side validator self-check (all five criteria must pass).
4. Open a PR with qualification evidence and the rule basis.

## License / 署名

Code and configurations are used under the MIT licence. Document copyright belongs to SynomosAI.

© 2026 SynomosAI. All rights reserved.
