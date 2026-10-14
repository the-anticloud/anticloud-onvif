# ONVIF

![license](https://img.shields.io/badge/license-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-surveillance_camera-lightgrey)

> Anticloud-hardened packaging of the upstream project `ONVIF` in category **SURVEILLANCE CAMERA**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** SURVEILLANCE CAMERA · **Upstream:** https://github.com/agsh/onvif · **Upstream pin:** `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

# ONVIF

[![Coverage Status](https://raw.githubusercontent.com/agsh/onvif/refs/heads/gh-pages-debug/badges/coverage.svg)](https://github.com/agsh/onvif/tree/v1)

TypeScript-first ONVIF client for Node.js.

- TypeScript + Promise API
- Typed ONVIF/WSDL interfaces
- Profiles [S](https://www.onvif.org/profiles/profile-s/), [T](https://www.onvif.org/profiles/profile-t/),
  [G](https://www.onvif.org/profiles/profile-g/), [M](https://www.onvif.org/profiles/profile-m/),
  [C](https://www.onvif.org/profiles/profile-c/), [A](https://www.onvif.org/profiles/profile-a/)
- WS-Discovery, WS-Security, Digest (MD5 / SHA-1 / SHA-256)
- Lazy-loaded service modules
- [v0.x compatibility layer](https://github.com/agsh/onvif/blob/master/src/compatibility/cam.ts)

> [!TIP]
> **Looking for stable 0.x / 0.8?** This page is for **1.x** (release candidate).  
> README and docs for the default npm install: [branch v0.x](https://github.com/agsh/onvif/tree/v0.x).  
> Staying on 0.x while trying 1.x? Use the [0.x compatibility API](#two-apis).

[![ONVIF](https://github.com/user-attachments/assets/f58fb3c8-6bf6-406c-bcc7-883c1da33c5d)](http://onvif.org)

## Installation

Requires Node.js 18+.

**For new projects, use 1.x.**  
**For existing 0.x projects, keep using the [0.x compatibility API](#two-apis) or migrate to the 1.x `Onvif` API** —
see [innerDocs/migration.md](innerDocs/migration.md).

### 1.x release candidate

```shell
npm install onvif@rc
```

This README describes 1.x. The package is still on the release-candidate channel until 1.0 is published.

### Stable 0.x

```shell
npm install onvif
```

Default `npm install onvif` still resolves to stable **0.x**. README for that line:
[branch v0.x](https://github.com/agsh/onvif/tree/v0.x).

### Development version

If you want to try the latest development version directly from GitHub:

```shell
git source https://github.com/agsh/onvif.git
cd onvif

npm install
npm run build
```

The compiled package is available in `build/`. You can use the locally built package from another project with:
```shell
npm install /path/to/onvif
```

## Documentation

API reference (TypeDoc): **[https://agsh.github.io/onvif/](https://agsh.github.io/onvif/)** —
start from the [`Onvif`](https://agsh.github.io/onvif/classes/Onvif.html) class.

The main entry is `Onvif`. After `connect()`, call methods on service namespaces. Most namespaces are
**lazy-loaded** on first use; `events` is constructed eagerly.

```text
Onvif
├── connect() / request()             # handshake + raw SOAP 
│                                     # + some actions from device/media/media2 (these modules are not loaded)
├── device                            # Device management (lazy)
├── media / media2                    # Profiles S / T media (lazy)
├── ptz                               # Pan-tilt-zoom (lazy)
├── events                            # Pull-point / WS-BaseNotification (eager)
├── imaging                           # Imaging settings (lazy)
├── recording / replay / search       # Profile G NVR (lazy)
├── receiver                          # Stream receivers (lazy)
├── analytics / analyticsDevice       # Analytics (lazy)
├── deviceIO / display / actionEngine # (lazy)
├── thermal / provisioning            # (lazy)
├── doorControl / accessControl / credential / accessRules / schedule / uplink
│                                     # (lazy)
└── advancedSecurity                  # TLS / keystore (experimental, lazy)

Discovery                              # separate export (WS-Discovery on the LAN)
├── probe()                            # find NVT devices; returns Promise of Onvif / info objects
└── on('device' | 'error', …)          # EventEmitter — device found / errors
```

Also exported: [`Discovery`](https://agsh.github.io/onvif/variables/Discovery.html) (WS-Discovery on the LAN),
and the separate [0.x compatibility](#two-apis) entry points.

## Why 1.x?

Version 1.x is a redesign of the original JavaScript API:

- TypeScript-first API with generated ONVIF interfaces
- Native Promise-based methods
- Lazy-loaded services
- More ONVIF services than 0.x (access control, thermal, door control, etc.)
- Improved error handling
- Explicit support for vendor-specific XML extensions (`xs:any` / `xs:anyAttribute`) — [innerDocs/vendor-extensions.md](innerDocs/vendor-extensions.md)
- Optional compatibility layer for existing 0.x applications

## Two APIs

**New API ≠ compatibility API.** Pick one surface and stick to it.

| | 1.x API | 0.x compatibility API |
| --- | --- | --- |
| Import | `import { Onvif } from 'onvif'` | `require('onvif/compatibility')` or `…/promises` |
| Type | `Onvif` + service namespaces | `Cam` / `Discovery` (v0.x shape) |
| For | new projects | existing `Cam`-based apps |

```ts
import { Onvif } from 'onvif';

const onvif = new Onvif({ hostname: '192.168.1.13', port: 8000, username: 'admin', password: 'admin' });
await onvif.connect();
await onvif.media.getProfiles();
```

```js
// Callbacks — onvif/compatibility
const { Cam } = require('onvif/compatibility');

const cam = new Cam(
  { hostname: '192.168.1.13', port: 8000, username: 'admin', password: 'admin' },
  (error) => {
    if (error) throw error;
    cam.getProfiles((err, profiles) => {
      if (err) throw err;
      console.log(profiles);
    });
  },
);
```

```js
// Promises — onvif/compatibility/promises
const { Cam } = require('onvif/compatibility/promises');

const cam = new Cam({ hostname: '192.168.1.13', port: 8000, username: 'admin', password: 'admin' });
await cam.connect();
console.log(await cam.getProfiles());
```

Compatibility import paths, full examples, and known behavioral differences:
[innerDocs/migration.md](innerDocs/migration.md).

## Quick start

```ts
import { Onvif } from 'onvif';

const onvif = new Onvif({ hostname: '192.168.1.13', port: 8000, username: 'admin', password: 'admin' });
await onvif.connect();
const info = await onvif.device.getDeviceInformation();
console.log(info);
```

Same import works from Commo

*(excerpt; full text in `UPSTREAM_CLONE/`)*

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **Node.js / npm** (manifests: package.json, package-lock.json; scanned in UPSTREAM_CLONE)
- Top-level source layout: `__tests__/`, `examples/`, `innerDocs/`, `src/`
- Snapshot size: **1532 files**, **48967 lines of code** (measured; see Benchmarks)
- Primary languages: `.html` (1319), `.ts` (130), `.md` (24), `.js` (22), `(none)` (7), `.sh` (6)
- Upstream commit pinned for this packaging: `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f`

---

## Installation

> Staying on 0.x while trying 1.x? Use the [0.x compatibility API](#two-apis).

[![ONVIF](https://github.com/user-attachments/assets/f58fb3c8-6bf6-406c-bcc7-883c1da33c5d)](http://onvif.org)

*Section quoted from the upstream readme.*
Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

[![ONVIF](https://github.com/user-attachments/assets/f58fb3c8-6bf6-406c-bcc7-883c1da33c5d)](http://onvif.org)

*Section quoted from the upstream readme.*
Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

- TypeScript + Promise API
- Typed ONVIF/WSDL interfaces
- Profiles [S](https://www.onvif.org/profiles/profile-s/), [T](https://www.onvif.org/profiles/profile-t/),
  [G](https://www.onvif.org/profiles/profile-g/), [M](https://www.onvif.org/profiles/profile-m/),
  [C](https://www.onvif.org/profiles/profile-c/), [A](https://www.onvif.org/profiles/profile-a/)
- WS-Discovery, WS-Security, Digest (MD5 / SHA-1 / SHA-256)
- Lazy-loaded service modules
- [v0.x compatibility layer](https://github.com/agsh/onvif/blob/master/src/compatibility/cam.ts)

> [!TIP]
> **Looking for stable 0.x / 0.8?** This page is for **1.x** (release candidate).  
> README and docs for the default npm install: [branch v0.x](https://github.com/agsh/onvif/tree/v0.x).  
> Staying on 0.x while trying 1.x? Use the [0.x compatibility API](#two-apis).

[![ONVIF](https://github.com/user-attachments/assets/f58fb3c8-6bf6-406c-bcc7-883c1da33c5d)](http://onvif.org)

*Section quoted from the upstream readme.*
---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | Node.js / npm |
| Manifests detected | package.json, package-lock.json |
| Files in snapshot | 1532 |
| Lines of code | 48967 |
| Dependency references | 29 |
| Dependencies by ecosystem | npm: 29 |
| Upstream license | MIT |
| Overlay license | Anticommons 0.1.0 |

Top dependency references recorded in the benchmark snapshot:

| Ecosystem | Name | Version | Source file |
|-----------|------|---------|-------------|
| npm | fast-xml-parser | ^5.11.0 | package.json |
| npm | @eslint/js | ^9.39.0 | package.json |
| npm | @types/jest | ^29.5.14 | package.json |
| npm | @types/keypress | ^2.0.30 | package.json |
| npm | @types/node | ^20.11.24 | package.json |
| npm | @types/xml2js | ^0.4.14 | package.json |
| npm | @typescript-eslint/parser | ^8.65.0 | package.json |
| npm | chalk | ^4.1.2 | package.json |
| npm | dot | ^1.1.3 | package.json |
| npm | eslint | ^9.39.0 | package.json |
| npm | eslint-config-prettier | ^10.1.1 | package.json |
| npm | eslint-plugin-import | ^2.31.0 | package.json |
| npm | eslint-plugin-jest | ^29.15.2 | package.json |
| npm | eslint-plugin-n | ^17.23.1 | package.json |
| npm | glob | ^13.0.6 | package.json |
| ... | (14 more) | | |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

├── recording / replay / search       # Profile G NVR (lazy)
├── receiver                          # Stream receivers (lazy)
├── analytics / analyticsDevice       # Analytics (lazy)
├── deviceIO / display / actionEngine # (lazy)
├── thermal / provisioning            # (lazy)
├── doorControl / accessControl / credential / accessRules / schedule / uplink
│                                     # (lazy)
└── advancedSecurity                  # TLS / keystore (experimental, lazy)

Discovery                              # separate export (WS-Discovery on the LAN)
├── probe()                            # find NVT devices; returns Promise of Onvif / info objects
└── on('device' | 'error', …)          # EventEmitter — device found / errors
```

Also exported: [`Discovery`](https://agsh.github.io/onvif/variables/Discovery.html) (WS-Discovery on the LAN),
and the separate [0.x compatibility](#two-apis) entry points.

*Section quoted from the upstream readme.*
Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

*Excerpt from upstream `CONTRIBUTING.md`:*

# Contribution Guidelines

## Add new method
I've tried to explain the easiest the best way to do it in this comment for the issue: https://github.com/agsh/onvif/issues/250#issuecomment-1273208097

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: MIT** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
MIT License

Copyright (c) 2018 Andrew Laptev

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original MIT terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `MIT` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `ONVIF` (category: SURVEILLANCE CAMERA)
- **Upstream URL:** https://github.com/agsh/onvif
- **Pinned commit (SHA):** `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f`
- **Branch:** master
- **Pin provenance:** GitHub API commits/master. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`0790d3720d5d23f283b4e6b87016aad15286258030b7a1d5f769c4a7dfffceb4`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

