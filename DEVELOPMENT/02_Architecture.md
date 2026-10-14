# Technical Architecture — ONVIF

**Upstream:** [https://github.com/nicedoc/onvif](https://github.com/nicedoc/onvif)
**License:** MIT
**Category:** SURVEILLANCE_CAMERA
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

ONVIF-compatible camera control library

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B on-device object detection — edge inference, no cloud upload
2. AIOSS tamper-evident video segment log with SHA3-256 frame hashing
3. AES-256 encryption for all stored footage and metadata
4. Single-binary edge deployment on NVIDIA Jetson, Raspberry Pi, x86
5. Zero-cloud: all inference, storage, and alerting runs locally
6. GPU/CPU equalizer: falls back gracefully from GPU to CPU inference
7. Privacy-preserving: face blurring applied before any off-device transmission
8. Offline alert routing to local SMTP/webhook, no SaaS dependency

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_onvif.spec` or `go build -o onvif`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |