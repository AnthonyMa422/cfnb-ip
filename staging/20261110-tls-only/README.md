# 2026-11-10 CFNB TLS-only candidate staging

This directory is **NOT a production subscription**. It is on a separate staging branch.
The production Cloudflare KV `ADD.txt` URLs remain pointed at `main`; this branch was not connected to them.

- 16 historical-attempt-excluded, TCP-pre-screened US/SG candidates were tried once each with a physical-WLAN-source-bound IPv4 TCP connection and a certificate-verifying TLS handshake using the respective official Pages custom hostname as SNI.
- 14 completed TLS certificate verification (US 6, SG 8). 2 US candidates established TCP but TLS timed out: 203.88.127.231:443 and 107.172.233.219:8443.
- Zero HTTP requests, zero response-body downloads. No authentication requests, no subscription refresh.
- Lists below are sorted by **one cold TCP+TLS connection elapsed time** only. This is NOT real VLESS throughput, long-run stability, country, actual ProxyIP dial winner, Google/Gemini region or production eligibility.
- US entries use FRONT_ONLY as the intended *future* probe mode. SG entries use COUPLED as the intended *future* probe mode; neither mode was invoked in the TLS test.
- TLS timeouts are one-observation HOLDs, not permanent node failure.
- The test checked client DB and config file hashes before/after and reported unchanged; no Cloudflare, DNS, Worker, KV, v2rayN, TUN or production-main changes.
- The on-device evidence is `C:\Tools\cfnb-quality\node-pool-rebuild-20261110-refresh\tls-front-prefilter-20261110\TLS_FRONT_PREFILTER_RESULT.json` with local SHA256SUMS.

Do not merge into `main` until isolated VLESS transfer, service egress, US/SG region checks, baseline consistency, and release/rollback gates pass.
