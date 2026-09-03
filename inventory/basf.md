# BASF SE / BASF Group inventory (discovery seed 2026-09-02)
# NOTE: hosts below are discovery candidates from passive DNS/CT; confirm in-scope vs program scope before active testing.
api.basf.com
basf.com
my.basf.com
www.basf.com

## PASSIVE RECON 2026-09-02 (read-only, non-intrusive)

> Recon observations only. These are NOT confirmed vulnerabilities; ownership/in-scope of each host must be confirmed against the program scope before any active testing. Hosts resolve + serve HTTP — investigation requires scoped authorization.

**Probed:** 4 hosts | **Live HTTP:** 0

| Host | Status | Server/Tech |
|---|---|---|

## DEEP ENUM (wildcard-cleaned) 2026-09-03
**Root zone:** `basf.com` | **dedicated hosts after wildcard-filter: 9**
> Audit: brute+passive subfinder produced 10,083 resolving hostnames; zone-wildcard + IP-fingerprint filtering dropped 9,973 (98.9%) DNS-wildcard noise (random labels resolving to shared wildcard IPs e.g. account.cineplex.de, a.hypofriend.de, account.live-manager.de, docker.jtl-software.de, *.ggamdom.com, *.dev.alfaview.com). Only genuine dedicated hosts listed below. These are surface-map observations; live HTTP status captured read-only (GET / via curl). No findings claimed; scope must be confirmed with the program.
- `ap-digitalconnect.api.basf.com`  [HTTP 200]
- `ap-eupf.api.basf.com`  [HTTP 200]
- `dev-clientcert-sap.api.basf.com`  [HTTP 400]
- `dev-ext001.api.basf.com`  [HTTP 400]
- `dev-int001.api.basf.com`  [HTTP 400]
- `dev-m.api.basf.com`  [HTTP 404]
- `dev-sap.api.basf.com`  [HTTP 404]
- `dev.api.basf.com`  [HTTP 404]
- `e-gate.api.basf.com`  [HTTP 404]

## 2026-09-03 09:40:45 UTC
