# Students — ONVIF

**Project:** ONVIF  
**Category:** SURVEILLANCE_CAMERA  
**Upstream:** see BENCH.json  
**Pinned commit:** `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `d38da1049594075f4375a70427d1c84180b9bc256a737a8aa98f42bd39c8ee5d`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `d38da1049594075f4375a70427d1c84180b9bc256a737a8aa98f42bd39c8ee5d`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
