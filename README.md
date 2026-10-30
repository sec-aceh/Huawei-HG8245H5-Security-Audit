# Huawei HG8245H5 — Preliminary Security Assessment

A non-destructive, local-network security review of a Huawei HG8245H5 home gateway, performed from Kali Linux.

> **Status:** Preliminary assessment / portfolio documentation  
> **Target ownership:** The device was assessed by its owner on a private home network.  
> **Important:** This repository documents observations, not a confirmed exploit or a vendor-validated vulnerability.

## Overview

This project records a first-pass security assessment of the Huawei HG8245H5. The work focused on service discovery, the local management interface, TLS configuration, HTTP response headers, and a static review of selected JavaScript used by the web interface.

The assessment was intentionally limited to non-destructive checks. It did not attempt password guessing, authentication bypass, denial of service, configuration changes, or exploitation.

## Scope

- Device: Huawei HG8245H5
- Network: Owner-controlled private LAN
- Interface: Local web management interface
- Tools: Kali Linux, Nmap, curl, OpenSSL, grep, Python
- Target address: Redacted in this public repository (`ROUTER_IP`)

Do not assume that the observations apply to every HG8245H5 hardware revision, ISP-customized firmware, or firmware release.

## Key observations

| Area | Observation | Interpretation |
|---|---|---|
| TCP services | Ports 23, 53, and 80 were reported open in the recorded scan | Exposed services increase the local attack surface; this alone does not prove a vulnerability |
| Telnet | TCP/23 identified as a Huawei gateway Telnet service | Review whether Telnet can be disabled or restricted; no login attempt or exploitation is documented |
| DNS | TCP/53 reported open | The scan did not establish whether the service is an open resolver or reachable from the internet |
| Management UI | TLS/HTTPS was served on TCP/80 | HTTPS on a non-standard port is not itself a vulnerability |
| TLS | TLS 1.1 and TLS 1.2 were observed; RSA 2048-bit certificate key | TLS 1.1 is a hardening concern; certificate name/trust validation also needs attention |
| HTTP headers | `X-Frame-Options: SAMEORIGIN`, `Cache-Control: no-cache, no-store, max-age=0`, and `Pragma: no-cache` were observed on the main page | These are positive protections on the response inspected; other routes were not comprehensively tested |
| Login flow | Client-side code posts to `/asp/GetRandCount.asp`, then submits a form to `/login.cgi`; the password field is Base64-encoded in the inspected JavaScript | Base64 is encoding, not encryption. Server-side handling and transport protections were not fully assessed |
| Token endpoint | A GET request returned a 32-character hexadecimal-looking value, with a UTF-8 BOM in the response file | This does not establish token entropy, predictability, replay resistance, or server-side validation |

## Findings and severity

No exploitable vulnerability was confirmed during this preliminary review.

The following items are best recorded as **security observations / hardening opportunities**, not as confirmed vulnerabilities:

1. **Telnet service exposed on the local interface (TCP/23).** Confirm whether it is required and restrict or disable it where supported.
2. **Legacy TLS version support.** TLS 1.1 was observed. Prefer TLS 1.2 or later if the device firmware supports it.
3. **Management certificate validation.** The certificate subject did not match the IP address used for the connection, and the local client did not trust the certificate chain. Use a correctly provisioned certificate if the firmware supports it.
4. **Client-side password encoding.** The inspected login JavaScript Base64-encodes the password field. Base64 is not encryption. This observation alone does not prove credentials are exposed, because the connection used TLS; server-side behavior was not independently verified.
5. **Authentication token behavior is undetermined.** A token-like response was observed, but its security properties were not tested.

## Reproduction notes

Commands in `commands/` use the placeholder `ROUTER_IP`. Replace it only with the address of a device you own or are explicitly authorized to assess.

Evidence in `evidence/` is sanitized. Real tokens, cookies, credentials, and private configuration must never be committed.

## Responsible-use statement

This repository is for educational and defensive security documentation. Only assess devices you own or have explicit permission to test. Do not use these commands against third-party devices or networks.

## Limitations

- The assessment was limited to the local network.
- No firmware extraction or source-code audit was performed.
- No authentication bypass, password guessing, fuzzing, exploit validation, or denial-of-service testing was performed.
- A port being open does not prove that it is exploitable or exposed to the public internet.
- The observed firmware/UI behavior may vary by ISP customization and firmware version.
- No claim is made that the device is fully secure or insecure.

## Repository layout

```text
.
├── README.md
├── LICENSE
├── reports/
│   ├── assessment-summary.md
│   ├── port-scanning.md
│   ├── tls-analysis.md
│   └── authentication-review.md
├── commands/
│   ├── nmap.md
│   ├── curl.md
│   ├── openssl.md
│   └── local-analysis.md
└── evidence/
    ├── nmap-observations-redacted.md
    ├── http-headers-redacted.txt
    └── token-response-redacted.txt
```
