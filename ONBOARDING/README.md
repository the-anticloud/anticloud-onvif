# Onboarding — ONVIF

**Project:** ONVIF  
**Category:** SURVEILLANCE_CAMERA  
**Upstream:** see BENCH.json  
**Pinned commit:** `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `d38da1049594075f4375a70427d1c84180b9bc256a737a8aa98f42bd39c8ee5d`  
**Date:** October 2026

## First hour

1. Read `README.md` — what the project is and what it measures.
2. Read `ISOLATED_LAB_RESULTS/03_Result_Register.md` — the 16 checks and their
   evidence hashes.
3. Run the suite: `python tools/run_bench.py --out BENCH.json`.
4. Verify a hash: recompute SHA3-256 of a file in `04_Evidence/` and compare.

## First day

- `10_TECHNICAL_HANDOFF` — architecture and interfaces
- `11_TUTORIAL_DEVELOPERS` — build and test
- `18_COMMAND_LINE_INTERFACE` — CLI reference
- `27_DEPENDENCIES` — the offline dependency mirror

## Getting help

lois@0-1.gg · 0-1.gg
