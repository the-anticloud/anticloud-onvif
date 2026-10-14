# Educators — ONVIF

**Project:** ONVIF  
**Category:** SURVEILLANCE_CAMERA  
**Upstream:** see BENCH.json  
**Pinned commit:** `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `d38da1049594075f4375a70427d1c84180b9bc256a737a8aa98f42bd39c8ee5d`  
**Date:** October 2026

## Teaching with ONVIF

The project is usable as a worked example of offline-first packaging with a
cryptographic audit chain. It ships with the assurance suite, the evidence
register and the ledger, so students can verify claims rather than take them on
faith.

## Suggested exercises

1. Run `python tools/run_bench.py --out BENCH.json` and read the 16 results.
2. Recompute the SHA3-256 of a row's evidence file and compare to the register.
3. Walk the AIOSS chain from genesis to head `d38da1049594075f4375a70427d1c84180b9bc256a737a8aa98f42bd39c8ee5d` and confirm every link.
4. Break one artifact and observe the chain fail to verify.

## Licence for teaching

Apache 2.0 terms apply to academic and teaching use. See `24_ANTICOMMONS_LICENSE`.
