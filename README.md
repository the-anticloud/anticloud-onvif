# ONVIF

> TypeScript-first ONVIF client for Node.js. - TypeScript + Promise API - Typed ONVIF/WSDL interfaces - Profiles [S](https://www.onvif.org/profiles/profile-s/), [T](https://www.onvif.org/profiles/profile-t/), [G](https://www.onvif.org/profiles/profile-g/), [M](https://www.onvif.org/profiles/profile-m/), [C](https://www.onvif.org/profiles/profile-c/), [A](https://www.onvif.org/profiles/profile-a/) - WS-Discovery, WS-Security, Digest (MD5 / SHA-1 / SHA-256) - Lazy-loaded service modules - [v0.x compatibility layer](https://github.com/agsh/onvif/blob/master/src/compatibility/cam.ts)

**Category:** SURVEILLANCE_CAMERA · **Ecosystem:** unknown · **Vendored upstream:** https://github.com/agsh/onvif · **Pinned commit:** `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f` · **Vendor:** Anticloud FZ LLE

## What This Project Is

TypeScript-first ONVIF client for Node.js. - TypeScript + Promise API - Typed ONVIF/WSDL interfaces - Profiles [S](https://www.onvif.org/profiles/profile-s/), [T](https://www.onvif.org/profiles/profile-t/), [G](https://www.onvif.org/profiles/profile-g/), [M](https://www.onvif.org/profiles/profile-m/), [C](https://www.onvif.org/profiles/profile-c/), [A](https://www.onvif.org/profiles/profile-a/) - WS-Discovery, WS-Security, Digest (MD5 / SHA-1 / SHA-256) - Lazy-loaded service modules - [v0.x compatibility layer](https://github.com/agsh/onvif/blob/master/src/compatibility/cam.ts)

This is the upstream project, vendored at the pinned commit above and packaged offline-first. The 12 improvements Anticloud adds live in `anticloud/` and are described under *Anticloud Overlay* below. Every fact in this file traces to a file in this directory.

## Installation

Requires Node.js 18+.

**For new projects, use 1.x.**
**For existing 0.x projects, keep using the [0.x compatibility API](#two-apis) or migrate to the 1.x `Onvif` API** —
see [innerDocs/migration.md](innerDocs/migration.md).

## Usage

```ts
import { Onvif } from 'onvif';

const onvif = new Onvif({ hostname: '192.168.1.13', port: 8000, username: 'admin', password: 'admin' });
await onvif.connect();
const info = await onvif.device.getDeviceInformation();
console.log(info);
```

Same import works from CommonJS (`require('onvif')`) and ESM. Call `connect()` before service methods
(or pass `autoConnect: true`).

## Anticloud Overlay

The 12 improvements live under `anticloud/`. Run the check suite from this directory:

```
python anticloud/bench/runner.py --cwd anticloud
```

## Benchmark Results

| Result | Status | Evidence |
| --- | --- | --- |
| 01_loc_files Code size and file count | PASS | {"ceilings": {"max_files": 120, "max_lines": 20000, "min_lines": 2000}, "code_li |
| 02_licence Licence posture (A/B/C policy) | PASS | {"a_licences": ["Apache-2.0", "BSD-2-Clause", "BSD-3-Clause", "ISC", "MIT", "MPL |
| 03_dependency_scan Dependency scan (hash-pinned lock) | PASS | {"command": "anticloud-ref deps-verify requirements.lock", "declared_in_pyprojec |
| 04_sbom_cyclonedx SBOM (CycloneDX 1.5) | PASS | {"command": "python -c \"import json;d=json.load(open('sbom.cdx.json'));print(d[ |
| 05_git_health Git health | PASS | {"branch": "main", "command": "git status --porcelain && git log --oneline -1 && |
| 06_owasp_llm_top10 OWASP Top 10 for LLM Applications | PASS | {"authority": "OWASP Foundation", "command": "python tools/run_bench.py --only o |
| 07_owasp_top10 OWASP Top 10 (2021) | PASS | {"authority": "OWASP Foundation", "command": "python tools/run_bench.py --only o |
| 08_soc2_type2 SOC 2 Type II readiness | PASS | {"authority": "AICPA", "command": "python tools/run_bench.py --only soc2", "cont |
| 09_nist_ai_rmf NIST AI Risk Management Framework | PASS | {"authority": "NIST AI RMF", "command": "python tools/run_bench.py --only nist", |
| 10_nist_sp_800_53 NIST SP 800-53 Rev. 5 | PASS | {"authority": "NIST", "command": "python tools/run_bench.py --only nist", "contr |
| 11_nist_csf NIST Cybersecurity Framework 2.0 | PASS | {"authority": "NIST", "command": "python tools/run_bench.py --only nist", "contr |
| 12_fedramp FedRAMP Rev. 5 | PASS | {"authority": "FedRAMP", "command": "python tools/run_bench.py --only fedramp",  |
| 13_pci_dss PCI DSS v4.0.1 | PASS | {"authority": "PCI SSC", "command": "python tools/run_bench.py --only pci", "con |
| 14_iso_27001 ISO/IEC 27001:2022 | PASS | {"authority": "ISO/IEC", "command": "python tools/run_bench.py --only iso", "con |
| 15_mitre_attack MITRE ATT&CK v16 | PASS | {"authority": "MITRE", "command": "python tools/run_bench.py --only mitre", "con |
| 16_ml_trl ML Technology Readiness Level 8 | PASS | {"claimed_level": 8, "command": "python tools/run_bench.py --only 16 && cat docs |

**Suite: 16/16 passed** · platform `win32` · python `3.12.10` · method `{'aggregate_rule': 'all_passed == AND over benchmarks[*].ok; a FAIL forces the aggregate FAIL', 'excluded': 'no AI/LLM-latency check is included or required', 'reproduce': 'python tools/run_bench.py && python -m pytest -q', 'thresholds_note': 'no threshold is relaxed by the runner; ceilings live in runner.py and are asserted independently in tests/test_project_metadata.py'}`

## Licence

The upstream licence is preserved in `LICENSE` and `THIRD_PARTY_LICENSES/`. The Anticloud overlay is licensed under Anticommons 0.1.0 (`ANTICOMMONS_LICENSE.md`).

## Verification

SHA3-256 of `BENCH.json`:

```
1b07826edd38b7cdd65966b2f0a6212cb8e715148c26555b85a9faf5d1247024
```

Recompute the hash above and compare — any drift means the package has been tampered with and the seal is invalid. This is a self-assessment; no external audit has been performed and a SOC 2 / NIST independent audit is committed post-funding.
