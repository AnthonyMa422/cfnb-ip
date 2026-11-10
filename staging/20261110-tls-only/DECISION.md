# CFNB non-production candidate assessment — 2026-11-10

Production release status: **NO-GO**. These branch files are evidence-labeled intermediate lists, NOT active subscriptions. Do not merge into main automatically.

## Current evidence
- 16 uniquely selected previously unattempted TCP-screened candidate fronts; one direct TLS connection each via the physical WLAN source IP.
- Direct front TLS with valid certificate: 14/16 (US 6, SG 8). U6 203.88.127.231:443 and U7 107.172.233.219:8443 timed out in TLS after TCP connection.
- Real independent local Xray VLESS → IPIFY HTTPS 200, with TLS and TCP owner-PID witness: 11/14 (US 5, SG 6). US U4 timed out, SG S8 timed out, SG S1 received HTTP 403. A 403 from one service is not global node failure.
- Exact E1 1MiB HTTP 206 Range transfer, with TLS and owner-PID witness and byte-complete: 7/11 (US 2, SG 5). US U1/U8 time-censored, U5 TLS/read error, SG S2 time-censored.
- E1 16MiB escalation: **0/2 completed**. US U3 received 2,326,244 bytes but lost sampler freshness (`WITNESS_STALE`) and has no valid complete trace; SG S3 received 1,638,116 bytes before the 12s test deadline. Do not promote either.
- This set of non-production scripts attempted 41 small identity / range HTTP requests, consuming 11,910,588 application response bytes. It did not fetch an authenticated subscription or perform any full-file download.
- Reports from five local sessions recorded identical v2rayN DB/config hashes before/after. The scripts launch and terminate only their own temporary loopback Xray process.
- Single 1MiB transfer goodput calculated over the WHOLE request (CONNECT + TLS + TTFB + body) ranged ~0.962–1.779 Mbps for the seven complete ranges. It is not a steady-state wire bandwidth measurement.
- Historical SG backend membership hash list is incomplete; the current SG front=backend policy and UUID/path-template hashes were checked, but that does not restore the missing provenance.
- Missing: actual EdgeTunnel backend dial-winner telemetry, first-party Google/Gemini request-region proof, service egress localization and IPv4/IPv6/UDP coverage, robust multi-source long-run throughput, credible Worker deployment digest, production release authorization, demonstrated rollback.
- **The sampler fault is a global test-harness stop condition.** The exploratory script continued to the next independent candidate; that result remains recorded only as a diagnostic, and the entire 16MiB stage has no release authority.

## Files
- `us-front-tls-only.txt`, `sg-front-tls-only.txt`: direct TLS-only 14 candidates
- `ipify-us-http200-only.txt`, `ipify-sg-http200-only.txt`: 11 target-specific VLESS data-plane successes
- `e1-us-1mib-pass-only.txt`, `e1-sg-1mib-pass-only.txt`: 7 small range-complete candidates, only for future comparison

The Cloudflare KV ADD.txt configuration continues to point at `main`, not this branch. The production US/SG node lists and client selection are not changed.

On-device source evidence:
`C:\Tools\cfnb-quality\node-pool-rebuild-20261110-refresh\exploratory-vless-20261110\CANDIDATE_RECONCILIATION.json`
and `SHA256SUMS.txt`.

Do not label these as verified US/SG exits or automatically feed them into an active proxy subscription.
