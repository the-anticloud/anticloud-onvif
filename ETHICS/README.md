# Ethics — ONVIF

**Project:** ONVIF  
**Category:** SURVEILLANCE_CAMERA  
**Upstream:** see BENCH.json  
**Pinned commit:** `3fb640b6aabe7c4c7a66eceb8cd05165c8217b8f`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `d38da1049594075f4375a70427d1c84180b9bc256a737a8aa98f42bd39c8ee5d`  
**Date:** October 2026

## Position

ONVIF is packaged for offline deployment with a verifiable audit trail. The
ethical questions this raises are answered by making the system's behaviour
checkable rather than by policy statements.

## The four commitments

1. **No hidden egress.** The deployment has no external API dependency; this is
   testable by running it with the network disconnected.
2. **Attributable output.** Every artifact is recorded in a hash chain, so what
   the system produced can be reconstructed.
3. **Operator control.** The institution owns the hardware and the keys.
4. **Refusal to overclaim.** Where a certification is not held, the project says
   so rather than implying it.

## Dual use

This project is packaged for civilian and public-sector deployment. Where an
upstream has dual-use characteristics, the licence gate and the reference-only
marking in `BENCH.json` record that.
