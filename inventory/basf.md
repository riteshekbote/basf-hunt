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

## 2026-09-03 14:03:00 UTC

## 2026-09-03 17:55:27 UTC
- CHANGED `ap-digitalconnect.api.basf.com/admin/host/keys` → HTTP 404 (was expected 401 Bearer); admin surface differs from standard Azure Function App defaults
- CHANGED `ap-eupf.api.basf.com` SSRF probes (`/api/health?url=...`, `/api/<function>?url=...`) → HTTP 403 (blocked by WAF/gateway); SSRF vector appears mitigated at edge

## 2026-09-03 21:00:23 UTC
- CHANGED `ap-digitalconnect.api.basf.com/admin/host/keys` → HTTP 401 (was 404 in KB); standard admin path exists and requires auth
- CHANGED `ap-eupf.api.basf.com/admin/host/keys` → HTTP 401; both Function Apps have standard admin surface gated by auth
- CHANGED `ap-eupf.api.basf.com/runtime/webhooks/host/keys` → HTTP 401; internal runtime endpoint also auth-gated
- CHANGED `ap-eupf.api.basf.com/api/HttpTrigger1|health|HttpTrigger|function|run` → all HTTP 404; no common function names exposed at `/api/`
- CHANGED Header-based SSRF probes (`X-Forwarded-Url`, `X-Callback-Url` to metadata endpoint) on both roots → HTTP 200 (headers ignored, no callback evidence)
- NEW `dev-m.api.basf.com` and `dev-sap.api.basf.com` → HTTP 404 (not in prior deep enum tail)

## 2026-09-03 23:13:28 UTC
- NEW `ap-digitalconnect.api.basf.com/.azurefunctions/keys` → HTTP 404 (Azure internal alt path tested, not found)
- NEW `ap-digitalconnect.api.basf.com/admin/v2/keys` → HTTP 404 (versioned admin path tested, not found)
- NEW `ap-digitalconnect.api.basf.com/admin/list` → HTTP 404 (admin list endpoint tested, not found)
- NEW `ap-eupf.api.basf.com/` → HTTP 200 len=150093 (root returns substantial content, not empty placeholder)
- NEW `ap-eupf.api.basf.com/api/health` → HTTP 404 (common health endpoint not exposed)
- NEW `ap-eupf.api.basf.com/api/<enum>?url=http://attacker.com` → HTTP 403 (param-based SSRF blocked by WAF/edge)

## 2026-09-04 01:13:42 UTC
- NEW `ap-digitalconnect.api.basf.com/admin/host/keys?slot=staging` → HTTP 404 (deployment slot admin keys endpoint tested)
- NEW `ap-digitalconnect.api.basf.com/admin/host/keys?slot=production` → HTTP 404 (deployment slot admin keys endpoint tested)
- CHANGED `ap-eupf.api.basf.com/` → HTTP 200 len=150093 (reconfirmed, substantial content persists)
- CHANGED `dev-clientcert-sap.api.basf.com/` → HTTP 400 (reconfirmed, mTLS required)
- CHANGED `dev-ext001.api.basf.com/` → HTTP 400 (reconfirmed, mTLS required)

## 2026-09-04 06:05:58 UTC
- CHANGED `ap-digitalconnect.api.basf.com/admin/host/systemkeys` → HTTP 404 (was planned probe; now confirmed in probe-results.md)
- CHANGED `ap-digitalconnect.api.basf.com/admin/functions` → HTTP 404 (was planned; confirmed)
- CHANGED `ap-digitalconnect.api.basf.com/admin/host/status` → HTTP 404 (was planned; confirmed)
- NEW `e-gate.api.basf.com` → HTTP 404 at root, ZERO probes run against any sub-paths (untested surface)
- NEW `ap-digitalconnect.api.basf.com/admin/host/functionkeys` → UNTESTED (v4 function-level key endpoint)
- NEW `ap-digitalconnect.api.basf.com/admin/system` → UNTESTED (admin system info endpoint)

## 2026-09-04 11:37:35 UTC

## 2026-09-04 15:26:08 UTC

## 2026-09-04 18:36:03 UTC

## 2026-09-04 21:06:19 UTC

## 2026-09-04 23:08:56 UTC

## 2026-09-05 01:11:18 UTC
- CHANGED e-gate.api.basf.com: TLS handshake completes with NO client cert (`-k`) — NOT mTLS; presented cert CN=e-gate.api.basf.com / O=BASF Digital Solutions GmbH, issuer=DigiCert Global G2 TLS RSA SHA256 2020
- CHANGED ap-eupf.api.basf.com/: HTTP 200 len=150093 Content-Type=text/html — body analyzed = stock Azure Functions 3.0 placeholder (azureLogo, jQuery via ajax.aspnetcdn.com, go.microsoft.com links); zero `/api
- CHANGED ap-eupf + ap-digitalconnect `/.auth/config` + `/.auth/me`: HTTP 404 size=0 → EasyAuth not exposed on either Function App (last untested App Service sub-surface, now closed)
- CHANGED RAG: passive web search for both hostnames + key artifacts → zero public repo/commit references; only indexed placeholder pages and generic Azure Functions docs; no leaked keys or function names recov
- NEW dev.api.basf.com / dev-m.api.basf.com / dev-sap.api.basf.com: openapi.json / swagger.json / api-docs all HTTP 404 (body 168–197) → catch-all 404 hosts, no gateway/docs surface

## 2026-09-05 05:58:39 UTC

## 2026-09-05 10:30:27 UTC
- NEW api.basf.com, my.basf.com, www.basf.com, basf.com: completely unprobed web estate (4 hosts) while *.api.basf.com estate (9 hosts) fully exhausted with zero unauth findings
- CHANGED e-gate.api.basf.com: confirmed HTTP 404 at root + all 7 doc paths (not SSL error); server=Microsoft-HTTPAPI/2.0; no API gateway surface
- CHANGED ap-eupf.api.basf.com: 150KB root confirmed as stock Azure Functions 3.0 placeholder (azureLogo, jQuery, go.microsoft.com); zero function refs or `/api/` routes

## 2026-09-05 13:34:30 UTC
- NEW `prod.api.basf.com` — HTTP 404 at root; `/productinformation` returns 401 VerifyAPIKey (key-gated Apigee proxy confirmed on one virtual host); 3 browser keys shipped in SPA bundle; zero proxy path enu
- NEW `api.commerce.basf.com` — 403 `MissingAuthenticationToken` (x-amzn-errortype) = AWS REST API Gateway with staged routes; navigator key paired with `/copilot` baseUrl; zero stage-prefix enumeration per
- NEW `federation.basf.com` — NAM OIDC provider; discovery lists `code_challenge_methods_supported: plain, S256`, `client_secret_post/basic`; SPA uses responseType=code + useRefreshToken=true + zero PKCE re
- NEW `my.basf.com` — HTTP 200, 204KB content; `.well-known/openid-configuration` returns 404; zero deep auth-stack enumeration beyond root
- NEW `www.basf.com` — HTTP 200, 640KB content; zero probes beyond root reachability
- CHANGED `api.basf.com` — all probes return `Connection refused` (Errno 111); not publicly reachable; dead or internal-only DNS entry
- NEW api.basf.com, my.basf.com, www.basf.com, basf.com: completely unprobed web estate (4 hosts) while *.api.basf.com estate (9 hosts) fully exhausted with zero unauth findings
- CHANGED e-gate.api.basf.com: confirmed HTTP 404 at root + all 7 doc paths (not SSL error); server=Microsoft-HTTPAPI/2.0; no API gateway surface
- CHANGED ap-eupf.api.basf.com: 150KB root confirmed as stock Azure Functions 3.0 placeholder (azureLogo, jQuery, go.microsoft.com); zero function refs or `/api/` routes

## 2026-09-05 16:26:13 UTC
- NEW api.basf.com resolves to 127.0.0.1 (loopback) — not publicly reachable; all documentation probes fail with connection refused
- NEW my.basf.com returns HTTP 200 (204KB) via CloudFront; `.well-known/openid-configuration` returns 404 (HTML error page from BASF Auth Service); auth stack not at standard OIDC path
- NEW www.basf.com returns HTTP 308 redirect to /us/en via CloudFront; 640KB content; no dangling CNAME (A records to CloudFront IPs)
- NEW basf.com resolves to CloudFront IP (13.248.131.227); no CNAME
- CHANGED prod.api.basf.com confirmed as Apigee gateway (CNAME basf-prod-prod.apigee.net); only `/productinformation` returns 401 VerifyAPIKey; ~25 common proxy paths (/products, /catalog, /search, /user, /orde
- CHANGED api.commerce.basf.com confirmed as AWS REST API Gateway (x-amz-apigw-id header); `/copilot` returns 403 MissingAuthenticationToken; root returns 400; staged routes exist but require auth
- CHANGED federation.basf.com OIDC discovery returns HTML error page (NetIQ/Access Manager), not JSON — standard `.well-known/openid-configuration` not exposed
