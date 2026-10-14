# Build and Test

**Project:** `ONVIF`
**Upstream:** https://github.com/nicedoc/onvif
**License:** MIT

## Quick Start

```bash
git clone https://github.com/nicedoc/onvif
cd onvif
pip install -r requirements-anticloud.txt
python anticloud_main.py --offline --pax-local
```

## Anticloud Improvements Applied

1. PAX L5 Narrow L2 General 27B on-device object detection — edge inference, no cloud upload
2. AIOSS tamper-evident video segment log with SHA3-256 frame hashing
3. AES-256 encryption for all stored footage and metadata
4. Single-binary edge deployment on NVIDIA Jetson, Raspberry Pi, x86
5. Zero-cloud: all inference, storage, and alerting runs locally
6. GPU/CPU equalizer: falls back gracefully from GPU to CPU inference
7. Privacy-preserving: face blurring applied before any off-device transmission
8. Offline alert routing to local SMTP/webhook, no SaaS dependency

## Benchmark Targets

| Metric | Target |
| --- | --- |
| Latency | Primary inference task: <5s on CPU, <1s on GPU |
| Throughput | Batch processing: >100 items/hour on single CPU server |
| Memory | <8GB RAM for standard deployment |
| Accuracy | Task-specific accuracy within 5% of cloud-API baseline |

## Build Status

Not yet measured. Run verified build and record actual figures above.
