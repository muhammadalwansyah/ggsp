# 🚀 GGSP — Turn Your Google AI Pro Subscription Into a Local OpenAI Agentic API (Zero Extra API Bills!)

[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-0078D6?style=for-the-badge&logo=windows)](releases/windows/gemini-web-proxy.exe)
[![Built With](https://img.shields.io/badge/Built%20With-Rust%20%2B%20Tauri%20v2-DEA584?style=for-the-badge&logo=rust)](#why-ggsp)
[![API](https://img.shields.io/badge/API-OpenAI%20Compatible-10A37F?style=for-the-badge)](#quick-start-60-seconds)
[![Security](https://img.shields.io/badge/Bind-127.0.0.1%20Loopback%20Only-16A34A?style=for-the-badge)](#security--opsec-guarantees)
[![Distribution](https://img.shields.io/badge/Release-Single%20Portable%20.EXE-6366F1?style=for-the-badge)](#download)

> **Stop burning $100+/month on per-token LLM API bills when you already pay for Google AI Pro.**
>
> **GGSP (Gemini Goose Studio Proxy)** is a blazing-fast, single-executable **Rust + Tauri v2** desktop studio that turns your existing Gemini Web session into a rock-solid, local **OpenAI-compatible Agentic API (`http://127.0.0.1:8000/v1`)** — with **full tool-calling, native 4K Vision, live quota tracking, and 1-click Goose Desktop + Mobile Web/Tailscale orchestration.**

---

## ⚡ Why GGSP?

Most web-to-API proxies break mid-stream, choke on tool calls, leak cookies, or require messy Python environments. **GGSP is compiled to a single 22 MB native Rust binary** built specifically for heavy autonomous agent workflows:

- 🧠 **Dual-Tier Stealth Transport (`auto` / `cdp` / `http`)**
  Uses a persistent headless Chrome **CDP WebChannel (`processSession`)** bridge leveraging your machine's authentic browser TLS/HTTP2 fingerprint, paired with a pure-Rust `rustls` HTTP/2 control plane and automatic fallback.
- 🛠️ **Battle-Tested Agentic Tool-Calling**
  Built-in SSE `<tool_call>` holdback, hallucinated post-tool turn cutoff, automatic MCP tool-name normalization, malformed JSON repair, 3-tier 1M-context compaction, and `<5 ms` local title generation.
- 👁️ **Native Multimodal Vision (Up to 64 MB Payloads)**
  Feed 1080p/4K screenshots directly from **Goose** (`read_image`) or OpenAI vision clients with pure-Rust SHA-256 upload deduplication and filename citation mapping.
- 🦢 **1-Click Goose Studio + Control Your PC Agent From Your Phone**
  Auto-configures **Goose Desktop** and **Goose Web UI** across **6 launch modes** (`desktop`, `web-local`, `web-tailscale`, `desktop+web-local`, `desktop+web-tailscale`, `none`) with instant **Tailscale private VPN QR code pairing** — zero public port forwarding, zero `tailscale funnel`.
- 📊 **Live Quota, Multi-Account & Reasoning Effort Control**
  Monitor 5-hour & weekly usage windows in real time, switch between multiple isolated Google AI Pro profiles, and tune **3-level reasoning effort** (`low` / `medium` / `high`) on supported models.
- 🪶 **Zero Dependencies, Truly Portable**
  One `.exe`. No Python, no Node.js, no Docker, no system-wide clutter. Runs straight out of the box on Windows 10/11.

---

## 📦 Download

> **Note:** This repository distributes **pre-compiled release binaries only** (source code is kept private).

| Platform | Binary | Format | Status |
| :--- | :--- | :--- | :--- |
| **Windows 10/11 (x64)** | [`releases/windows/gemini-web-proxy.exe`](releases/windows/gemini-web-proxy.exe) | Single Portable `.exe` (~22.5 MB) | ✅ Ready (`v1.0.0`) |
| **macOS (Apple Silicon / Intel)** | — | `.dmg` / `.app` | 🚧 Planned |

You can also grab the binary directly from the **[GitHub Releases](https://github.com/muhammadalwansyah/ggsp/releases)** tab.

### Verify Integrity (SHA-256)
```powershell
Get-FileHash .\gemini-web-proxy.exe -Algorithm SHA256
```
Expected hash (`v1.0.0` Windows x64 — also listed in [`SHA256SUMS.txt`](SHA256SUMS.txt)):
```text
B8D0B787DAED0A8FA8185F085EFFB81D9BC580AED9BE06C34B32EC1734ACEC17
```

---

## 🚀 Quick Start (60 Seconds)

1. **Download** `gemini-web-proxy.exe` from [`releases/windows/`](releases/windows/gemini-web-proxy.exe) (or the Releases page) and place it anywhere on your Windows 10/11 PC.
   *(WebView2 Runtime is already built into Windows 11 and modern Windows 10.)*
2. **Double-click `gemini-web-proxy.exe`** and choose your storage mode on first launch:
   - **OS AppData (Recommended):** `%APPDATA%\GeminiWebProxy\`
   - **Portable:** `./data/` right next to the `.exe`
   - **Custom Folder:** Any directory you prefer
3. **Sign In Safely (No Cookie Scraping):**
   Go to **Accounts & Profiles**, create a profile, and sign in to [gemini.google.com/app](https://gemini.google.com/app) inside the isolated browser window. GGSP captures the session locally over loopback CDP — it never touches your personal browser's SQLite cookie database.
4. **Hit `Full Auto Launch`:**
   - Starts the local OpenAI-compatible proxy on `http://127.0.0.1:8000/v1`
   - Auto-configures and launches **Goose Desktop** and/or **Goose Web UI** (plus an instant QR code if using **Tailscale**)

### Connect Any OpenAI-Compatible Client

Point your favorite IDE, CLI, or agent client (Goose, Cline, Open WebUI, Continue, custom scripts, etc.) to:

```text
Base URL : http://127.0.0.1:8000/v1
API Key  : <Your Local Proxy API Key shown in GGSP Dashboard>
Endpoints: GET /v1/models | POST /v1/chat/completions | GET /health
```

### Optional: Headless CLI Smoke Test
Verify the binary boots cleanly, enforces auth (`401`), passes `/health`, and shuts down gracefully without opening a GUI:
```cmd
gemini-web-proxy.exe --smoke-test
```

---

## 🛡️ Security & OPSEC Guarantees

GGSP was engineered with strict local-first security invariants:

- **Strict Loopback Binding (`127.0.0.1` Only):** The OpenAI proxy never binds to `0.0.0.0`.
- **Outbound Host Allowlist Firewall:** Every outbound request and HTTP redirect hop is validated against an official Google + loopback allowlist before leaving the process.
- **Zero Browser SQLite Scraping:** Uses isolated browser profiles (`--user-data-dir`) and loopback-only CDP.
- **User-Only File ACLs:** Local `.env` session snapshots and Goose `secrets.yaml` are locked down with user-only NTFS permissions (`icacls`).
- **Automatic Secret Redaction:** Cookies, bearer tokens, `SNlM0e`, and API keys are masked (`<REDACTED:len=N>`) across UI logs, exported `.txt` logs, and rotated `proxy_lastrun.log` files.
- **Private Tailscale VPN by Default:** Remote Goose Web mode binds strictly to your private Tailnet IPv4 (`100.x.x.x`) and never invokes `tailscale funnel`.
- **Clean Binary Distribution:** Zero hardcoded personal paths, cookies, or keys bundled inside the executable.

---

## 📋 Requirements

- **OS:** Windows 10 (updated) or Windows 11 (64-bit)
- **Browser:** Google Chrome or Microsoft Edge installed (for isolated profile sign-in & Level 3 CDP transport)
- **Account:** Active Google account with Gemini Web access (Google AI Pro recommended for higher limits & Pro/effort models)
- **Optional:** [Goose](https://block.github.io/goose/) (Desktop/CLI) & [Tailscale](https://tailscale.com/) for remote mobile/laptop access

---

## ⚠️ Disclaimer

GGSP is an independent personal productivity tool designed to interface with your own authenticated Gemini Web session locally. It is **not affiliated with, endorsed by, or sponsored by Google LLC or Block, Inc. (Goose)**. Use responsibly and in accordance with your account's terms of service.
