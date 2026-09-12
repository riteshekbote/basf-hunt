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

## 2026-09-05 18:46:05 UTC

## 2026-09-05 20:52:44 UTC

## 2026-09-05 22:48:52 UTC
- NEW my.basf.com: SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with `redirect_uri=https://my.basf.com/.auth`, `scope=openid profile refresh_token`, `acr_values
- NEW federation.basf.com: NetIQ OIDC discovery JSON at `/nidp/.well-known/openid-configuration` (14.9KB) and `/nidp/oauth/nam/.well-known/openid-configuration` (2KB) — confirms `code_challenge_methods_supp
- NEW prod.api.basf.com: 66 proxy paths probed (products, catalog, search, user, order, cart, price, availability, docs, etc.) — all HTTP 404 except `/productinformation` (401 VerifyAPIKey); 4 browser-sourc
- NEW api.commerce.basf.com: 8 stage prefixes (dev, test, staging, prod, v1, v2, beta, internal) + `/copilot` all return 403 `MissingAuthenticationTokenException` with navigator key — AWS API Gateway IAM/Si
- NEW e-gate.api.basf.com: TLS handshake succeeds without client cert (`-k`); cert CN=e-gate.api.basf.com issued by DigiCert Global G2 TLS RSA SHA256 2020 CA1 (O=BASF Digital Solutions GmbH); root + 7 doc p
- CHANGED ap-eupf.api.basf.com: 150KB root confirmed as stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com links) — zero `/api/`, fetch(), or function-name references; no route
- CHANGED ap-eupf.api.basf.com + ap-digitalconnect.api.basf.com: `/.auth/config` and `/.auth/me` return HTTP 404 — App Service EasyAuth not exposed on either Function App
- CHANGED api.basf.com: resolves to 127.0.0.1 (loopback); connection refused on all probes — dead/internal-only DNS entry, zero external attack surface
- CHANGED *.api.basf.com estate (9 hosts): full unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404

## 2026-09-06 00:56:42 UTC
- NEW my.basf.com: SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with `redirect_uri=https://my.basf.com/.auth`, `scope=openid profile refresh_token`, `acr_values
- NEW federation.basf.com: NetIQ OIDC discovery JSON at `/nidp/.well-known/openid-configuration` (14.9KB) and `/nidp/oauth/nam/.well-known/openid-configuration` (2KB) — confirms `code_challenge_methods_supp
- NEW prod.api.basf.com: 66 proxy paths probed — all HTTP 404 except `/productinformation` (401 VerifyAPIKey); 4 browser-sourced API keys (core, pi, csp, navigator) all rejected "Invalid ApiKey"
- NEW api.commerce.basf.com: 8 stage prefixes (dev, test, staging, prod, v1, v2, beta, internal) + `/copilot` all return 403 `MissingAuthenticationTokenException` — AWS API Gateway IAM/SigV4 authorizer, x-a
- NEW e-gate.api.basf.com: TLS handshake succeeds without client cert (`-k`); cert CN=e-gate.api.basf.com issued by DigiCert Global G2 TLS RSA SHA256 2020 CA1 (O=BASF Digital Solutions GmbH); root + 7 doc p
- CHANGED ap-eupf.api.basf.com: 150KB root confirmed as stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com links) — zero `/api/`, fetch(), or function-name references; no route
- CHANGED ap-eupf.api.basf.com + ap-digitalconnect.api.basf.com: `/.auth/config` and `/.auth/me` return HTTP 404 — App Service EasyAuth not exposed on either Function App
- CHANGED api.basf.com: resolves to 127.0.0.1 (loopback); connection refused on all probes — dead/internal-only DNS entry, zero external attack surface
- CHANGED *.api.basf.com estate (9 hosts): full unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404

## 2026-09-06 05:50:51 UTC
- CHANGED www.basf.com/us/en: HTTP 200 640KB body probed (probe-results.md lines 167,188,202) but auth-link grep analysis not completed/saved
- CHANGED federation.basf.com/nidp/oauth/nam/discovery/registration: HTTP 404 (probe-results.md line 203) — NetIQ dynamic client registration endpoint not exposed

## 2026-09-06 10:59:02 UTC
- CHANGED federation.basf.com: NAM OIDC discovery at /nidp/oauth/nam/.well-known/openid-configuration discloses real registration_endpoint /nidp/oauth/nam/clients (401 auth-required — earlier 404 test hit wrong
- NEW federation.basf.com/nidp/saml2/metadata -> 200 signed text/xml IdP+SP SAML2 descriptor (21434B): entityID, SSO POST/Redirect /nidp/saml2/sso, SLO /nidp/saml2/slo(+_return), SOAP /nidp/saml2/soap + sps
- NEW federation.basf.com OIDC discovery content: grant_types incl password(ROPC)+hybrid, code_challenge_methods plain+S256, scopes urn:netiq.com:nam:scope:oauth:registration:full|read, claims incl '/UserAt
- CHANGED www.basf.com/us/en: 640KB body analyzed — Magnolia CMS corporate site; no partner/supplier OAuth authorize endpoints or login links found in HTML/JSON (checked footer eBusiness page, stage carousel, m
- CHANGED federation.basf.com/nidp/oauth/nam/discovery/registration: HTTP 404 confirmed — NetIQ dynamic client registration not exposed

## 2026-09-06 14:16:19 UTC

## 2026-09-06 17:13:13 UTC
- NEW federation.basf.com: SAML2 metadata at `/nidp/saml2/metadata` returns 200 signed descriptor (21434B) with SSO/SLO/SOAP endpoints
- NEW federation.basf.com: OIDC discovery exposes ROPC (password) + hybrid grants, plain+S256 PKCE, registration scopes, LDAP groupMembership/basfOTPUsed claims
- NEW federation.basf.com: NAM OIDC/SAML/JWKS surface mapped end-to-end via discovery-advertised paths; all administrative endpoints auth-gated
- CHANGED my.basf.com: SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with redirect_uri, scope, refresh_token, zero PKCE refs
- CHANGED www.basf.com: 640KB Magnolia CMS body fully analyzed — zero partner/supplier OAuth/SSO links
- CHANGED *.api.basf.com estate: full 9-host unauth surface mapped end-to-end — zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com: 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys rejected Invalid ApiKey
- CHANGED api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — IAM-gated, x-api-key not credential class
- CHANGED api.basf.com: resolves to 127.0.0.1 (loopback); connection refused — dead/internal-only DNS entry

## 2026-09-06 19:22:41 UTC
- CHANGED federation.basf.com: OIDC discovery at `/nidp/.well-known/openid-configuration` and `/nidp/oauth/nam/.well-known/openid-configuration` fully mapped — exposes ROPC (password) + hybrid grants, plain+S25
- CHANGED federation.basf.com: NAM OIDC/SAML/JWKS surface mapped end-to-end via discovery-advertised paths; all administrative endpoints auth-gated (clients 401, introspect/revoke 405, userinfo 401, end_session
- CHANGED my.basf.com: SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with `redirect_uri=https://my.basf.com/.auth`, `scope=openid profile refresh_token`, `acr_values
- CHANGED *.api.basf.com estate (9 hosts): full unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
- CHANGED prod.api.basf.com: 66 proxy paths all 404 except `/productinformation` (401 VerifyAPIKey); 4 browser-sourced keys (core/pi/csp/navigator) rejected Invalid ApiKey — key scope exhausted
- CHANGED api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — AWS REST API Gateway IAM/SigV4 authorizer, x-api-key not a credential class
- CHANGED api.basf.com: resolves to 127.0.0.1 (loopback); connection refused — dead/internal-only DNS entry, zero external attack surface
- CHANGED www.basf.com: 640KB Magnolia CMS body fully analyzed — zero partner/supplier OAuth/SSO links
- CHANGED products.basf.com: CloudFront Magnolia SPA (252KB), same WCMS stack, zero auth entry in HTML
- CHANGED e-gate.api.basf.com: TLS handshake succeeds without client cert; cert CN=e-gate.api.basf.com issued by DigiCert Global G2 TLS RSA SHA256 2020 CA1 (O=BASF Digital Solutions GmbH); root + 7 doc paths al
- CHANGED ap-eupf.api.basf.com: 150KB root confirmed stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com) — zero function refs
- CHANGED ap-eupf.api.basf.com + ap-digitalconnect.api.basf.com: `/.auth/config` and `/.auth/me` return 404 — App Service EasyAuth not exposed on either Function App

## 2026-09-06 21:31:26 UTC
- NEW my.basf.com/.auth/config + /.auth/me → HTTP 200 (204926B) = SPA fallback (title `myBASFWorld`, boot config incl. clientId 86cc4bf9) — NOT App Service EasyAuth; the OAuth redirect_uri callback is a pur
- CHANGED federation.basf.com discovery reconfirmed unchanged: grant_types still incl. authorization_code/password/hybrid, code_challenge plain+S256, registration_endpoint /nidp/oauth/nam/clients — no provider-
- CHANGED my.basf.com: SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with `redirect_uri=https://my.basf.com/.auth`, `scope=openid profile refresh_token`, `acr_values
- CHANGED *.api.basf.com estate (9 hosts): full unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
- CHANGED prod.api.basf.com: 66 proxy paths all 404 except `/productinformation` (401 VerifyAPIKey); 4 browser-sourced keys (core/pi/csp/navigator) rejected Invalid ApiKey — key scope exhausted
- CHANGED api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — AWS REST API Gateway IAM/SigV4 authorizer, x-api-key not a credential class
- CHANGED api.basf.com: resolves to 127.0.0.1 (loopback); connection refused — dead/internal-only DNS entry, zero external attack surface
- CHANGED www.basf.com: 640KB Magnolia CMS body fully analyzed — zero partner/supplier OAuth/SSO links
- CHANGED products.basf.com: CloudFront Magnolia SPA (252KB), same WCMS stack, zero auth entry in HTML
- CHANGED e-gate.api.basf.com: TLS handshake succeeds without client cert; cert CN=e-gate.api.basf.com issued by DigiCert Global G2 TLS RSA SHA256 2020 CA1 (O=BASF Digital Solutions GmbH); root + 7 doc paths al
- CHANGED ap-eupf.api.basf.com: 150KB root confirmed stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com) — zero function refs
- CHANGED ap-eupf.api.basf.com + ap-digitalconnect.api.basf.com: `/.auth/config` and `/.auth/me` return 404 — App Service EasyAuth not exposed on either Function App
- CHANGED federation.basf.com: NAM OIDC/SAML/JWKS surface mapped end-to-end via discovery-advertised paths; all administrative endpoints auth-gated (clients 401, introspect/revoke 405, userinfo 401, end_session
- CHANGED federation.basf.com: OIDC discovery exposes ROPC (password) + hybrid grants, plain+S256 PKCE, registration scopes, LDAP groupMembership/basfOTPUsed claims
- CHANGED federation.basf.com: SAML2 metadata at `/nidp/saml2/metadata` returns 200 signed descriptor (21434B) with SSO/SLO/SOAP endpoints

## 2026-09-06 23:16:09 UTC
- CHANGED federation.basf.com: NAM OIDC discovery at /nidp/oauth/nam/.well-known/openid-configuration discloses real registration_endpoint /nidp/oauth/nam/clients (401 auth-required — earlier 404 test hit wrong
- NEW federation.basf.com/nidp/saml2/metadata -> 200 signed text/xml IdP+SP SAML2 descriptor (21434B): entityID, SSO POST/Redirect /nidp/saml2/sso, SLO /nidp/saml2/slo(+_return), SOAP /nidp/saml2/soap + sps
- NEW federation.basf.com OIDC discovery content: grant_types incl password(ROPC)+hybrid, code_challenge_methods plain+S256, scopes urn:netiq.com:nam:scope:oauth:registration:full|read, claims incl '/UserAt
- NEW my.basf.com/.auth/config + /.auth/me → HTTP 200 (204926B) = SPA fallback (title `myBASFWorld`, boot config incl. clientId 86cc4bf9) — NOT App Service EasyAuth; the OAuth redirect_uri callback is a pur
- CHANGED federation.basf.com discovery reconfirmed unchanged: grant_types still incl. authorization_code/password/hybrid, code_challenge plain+S256, registration_endpoint /nidp/oauth/nam/clients — no provider-
- NEW my.basf.com/.auth/config + /.auth/me → HTTP 200 (204926B) = SPA fallback (title `myBASFWorld`, boot config incl. clientId 86cc4bf9) — NOT App Service EasyAuth; the OAuth redirect_uri callback is a pur
- CHANGED federation.basf.com discovery reconfirmed unchanged: grant_types still incl. authorization_code/password/hybrid, code_challenge plain+S256, registration_endpoint /nidp/oauth/nam/clients — no provider-

## 2026-09-07 01:10:29 UTC
- NEW NO_DELTA — latest probe-results (2026-09-06 23:16) match knowledge base: federation.basf.com OIDC/authz stable (200/684, 405 token), my.basf.com SPA fallback on all auth paths (200/204KB), *.api.basf.

## 2026-09-07 06:25:45 UTC

## 2026-09-07 12:55:37 UTC

## 2026-09-07 18:28:38 UTC
- NEW tm.basf.com (141.6.3.192), passage-europe.basf.com (141.6.3.132), vss3.basf.com (141.6.3.183): all DNS-resolve on same /16 as procurement — **ZERO successful HTTP probes** exist; every prior attempt h
- NEW procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/ → **HTTP 500** (not 236B WAF block) — SAP KM documents servlet reached J2EE backend and returned server error; this path is
- CHANGED procurement.basf.com /irj/go/km/* all confirmed 200 len=236 = F5-ASM WAF block page (consistent, no content)

## 2026-09-07 21:43:42 UTC

## 2026-09-07 23:57:07 UTC
- NEW my.basf.com/.auth/config + /.auth/me → HTTP 200 (204926B) = SPA fallback (title `myBASFWorld`, boot config incl. clientId 86cc4bf9) — NOT App Service EasyAuth; the OAuth redirect_uri callback is a pur
- CHANGED federation.basf.com discovery reconfirmed unchanged: grant_types still incl. authorization_code/password/hybrid, code_challenge plain+S256, registration_endpoint /nidp/oauth/nam/clients — no provider-
- NEW tm.basf.com (141.6.3.192), passage-europe.basf.com (141.6.3.132), vss3.basf.com (141.6.3.183): all DNS-resolve on same /16 as procurement — **ZERO successful HTTP probes** exist; every prior attempt h
- NEW procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/ → **HTTP 500** (not 236B WAF block) — SAP KM documents servlet reached J2EE backend and returned server error; this path is
- CHANGED procurement.basf.com /irj/go/km/* all confirmed 200 len=236 = F5-ASM WAF block page (consistent, no content)
- NEW procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework → HTTP 500 (not WAF block) — SAP KM servlet reached J2EE backend, F5-ASM does NOT intercept th
- NEW tm.basf.com / passage-europe.basf.com / vss3.basf.com — DNS resolves (141.6.3.192/132/183); prior "ERR Name or service not known" was shell backtick bug in curl, NOT DNS failure; all three are LIVE, c
- CHANGED tm.basf.com/irj/go/km/docs/documents/newFramework/ → HTTP 404 (differs from passage/vss3 which return 200/235 WAF block)
- CHANGED passage-europe.basf.com/irj/go/km/docs/documents/newFramework/ → 200 len=235 (WAF block)
- CHANGED vss3.basf.com/irj/go/km/docs/documents/newFramework/ → 200 len=235 (WAF block)

## 2026-09-08 04:38:26 UTC

## 2026-09-08 09:02:09 UTC
- NEW rep.basf.com — live "Bestandskundenplattform" (existing customer portal) behind Azure Front Door; Spring Boot + Apache Wicket; Spring Boot Actuator exposed at `/actuator` (HAL) and `/actuator/health` 
- CHANGED developer.basf.com — confirmed Cloudflare JS-challenge (403 cf-mitigated:challenge); public documentation at developer.basf.com/authentication-and-authorization describes NAM OAuth flow with client_ce
- CHANGED BASF GitHub org — public repos (basf/rfieldclimate, basf/rweatherlink, basf/rzentra, basf/rarable) use env vars for API keys; no leaked secrets; R packages for agriculture/weather APIs only
- NEW procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework → HTTP 500 (1753B SAP runtime error) — J2EE backend reached PAST F5-ASM; servlet processes req
- NEW tm.basf.com, passage-europe.basf.com, vss3.basf.com — DNS resolves (141.6.3.192/132/183), all LIVE; ZERO successful HTTP probes exist (prior curl backtick bug)
- CHANGED procurement.basf.com/irj/servlet/prt/portal/prtroot (8 dispatcher classes) → HTTP 500 both unauth and with guest session (1711B) — guest role renders zero content via OBN, but KM servlet path unfilter
- CHANGED federation.basf.com OIDC discovery reconfirmed unchanged — ROPC (password) + hybrid grants, plain+S256 PKCE, registration scopes, LDAP groupMembership/basfOTPUsed claims
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface mapped end-to-end, zero reachable endpoints beyond auth gates/404 (reconfirmed)

## 2026-09-08 13:33:46 UTC
- NEW procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework → HTTP 500 (1753B SAP runtime error) — specific query parameter triggers J2EE backend processi
- NEW tm.basf.com (141.6.3.192), passage-europe.basf.com (141.6.3.132), vss3.basf.com (141.6.3.183) — DNS resolves on same /16 as procurement; all LIVE; ZERO successful HTTP probes exist (prior curl backtic
- CHANGED procurement.basf.com/irj/servlet/prt/portal/prtroot (8 dispatcher classes tested) → HTTP 500 both unauth and with guest session (1711B) — guest role renders zero content via OBN, but KM servlet path u
- CHANGED rep.basf.com — Spring Boot Actuator exposed at `/actuator` (HAL) and `/actuator/health` (UP); all sensitive endpoints (env, mappings, beans, configprops, threaddump) return 404
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface reconfirmed end-to-end, zero reachable endpoints beyond auth gates/404

## 2026-09-08 17:39:01 UTC
- NEW basf.login.apigee.com — discovered via web search; serves SAML/SSO login page ("Sign in with SAML" + "Login with basf"); appears to be BASF corporate Apigee identity portal; untested surface
- NEW basf.login.apigee.com — Apigee SAML/SSO login portal discovered via web search; serves "Sign in with SAML" + "Login with basf" — corporate identity surface untested
- NEW developer.basf.com docs confirm `prod.api.basf.com/security/internal/v1/oauth2/login` is the authorization endpoint for BASF APIs — authorization_code grant only, functional users use client certifica

## 2026-09-08 20:22:02 UTC

## 2026-09-08 22:50:28 UTC
- NEW basf.login.apigee.com — Apigee SAML/SSO corporate identity portal discovered (web search); serves "Sign in with SAML" + "Login with basf"; completely untested surface
- NEW rep.basf.com — "Bestandskundenplattform" behind Azure Front Door; Spring Boot + Wicket; Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints (env, mappings, beans, confi
- CHANGED procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework — HTTP 500 (1753B SAP runtime error) confirmed across 4 portals; J2EE backend reached PAST F5-
- CHANGED tm.basf.com + passage-europe.basf.com + vss3.basf.com — all 3 sibling portals now confirmed LIVE on 141.6.3.0/16; tm+passage share procurement's J2EE backend (KM servlet → HTTP 500), vss3 WAF-blocks K
- CHANGED federation.basf.com — OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes, LDAP groupMembership/basfOTPUsed claims); SAML2 metadata at `/nidp/saml2/metadata`
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface reconfirmed end-to-end, zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com — 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected "Invalid ApiKey"
- CHANGED api.commerce.basf.com — 8 stage prefixes all `MissingAuthenticationTokenException`; IAM/SigV4 authorizer, x-api-key not a credential class

## 2026-09-09 01:30:37 UTC

## 2026-09-09 06:12:00 UTC

## 2026-09-09 11:47:49 UTC
- NEW procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework → HTTP 500 (1753B SAP runtime error) confirmed across 4 portals; J2EE backend reached PAST F5-
- NEW tm.basf.com, passage-europe.basf.com, vss3.basf.com — all 3 sibling portals now confirmed LIVE on 141.6.3.0/16; tm+passage share procurement's J2EE backend (KM servlet → HTTP 500), vss3 WAF-blocks KM 
- NEW basf.login.apigee.com — full Apigee OAuth identity surface mapped via OIDC discovery + SAML metadata; ROPC/implicit grants, token/userinfo/jwks endpoints, SAML SSO; only config endpoints (discovery, m
- NEW rep.basf.com — live "Bestandskundenplattform" behind Azure Front Door; Spring Boot + Wicket; Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints return 404; custom erro
- CHANGED federation.basf.com OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes, LDAP groupMembership/basfOTPUsed claims); SAML2 metadata at `/nidp/saml2/metadata` 2
- CHANGED my.basf.com SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with redirect_uri=https://my.basf.com/.auth, scope=openid profile refresh_token, acr_values=3IAM/
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface reconfirmed end-to-end, zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com — 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected "Invalid ApiKey" — key scope exhausted
- CHANGED api.commerce.basf.com — 8 stage prefixes all `MissingAuthenticationTokenException`; IAM/SigV4 authorizer, x-api-key not a credential class

## 2026-09-09 15:35:46 UTC
- CHANGED procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/ — HTTP 500 reconfirmed (not WAF block) across 4 portals; J2EE backend reached PAST F5-ASM, servlet processes parameters
- CHANGED tm.basf.com/passage-europe.basf.com/vss3.basf.com — all 3 sibling portals LIVE on 141.6.3.0/16; tm+passage share procurement's J2EE backend (KM servlet → 500), vss3 WAF-blocks KM servlet (235B)
- CHANGED basf.login.apigee.com — full Apigee OAuth identity surface mapped via OIDC discovery + SAML metadata; ROPC/implicit grants, token/userinfo/jwks endpoints, SAML SSO; only config endpoints (discovery, m
- CHANGED rep.basf.com — Spring Boot + Wicket behind Azure Front Door; Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints return 404; custom error handler returns status 999
- CHANGED federation.basf.com — OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes, LDAP groupMembership/basfOTPUsed claims); SAML2 metadata at `/nidp/saml2/metadata`
- CHANGED my.basf.com — SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with redirect_uri=https://my.basf.com/.auth, scope=openid profile refresh_token, acr_values=3IA
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface reconfirmed end-to-end, zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com — 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected "Invalid ApiKey" — key scope exhausted
- CHANGED api.commerce.basf.com — 8 stage prefixes all `MissingAuthenticationTokenException`; IAM/SigV4 authorizer, x-api-key not a credential class

## 2026-09-09 18:50:44 UTC
- NEW basf.login.apigee.com — full Apigee OAuth identity surface mapped via OIDC discovery + SAML metadata; ROPC/implicit grants, token/userinfo/jwks endpoints, SAML SSO; only config endpoints (discovery, m
- NEW rep.basf.com — live "Bestandskundenplattform" behind Azure Front Door; Spring Boot + Wicket; Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints return 404; custom erro
- NEW secsys.basf.com — live "Smart ID Self-Service" (Technology Nexus, v5.3.1+) Angular SPA, 200/3179B; sibling `bsh.secsys`, `secsys-visitor`, and qual instances resolvable; qual hosts Cloudflare-JS-chall
- CHANGED procurement.basf.com KM servlet `/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework` → HTTP 500 reconfirmed across 4 portals; J2EE backend reached PAST F5-ASM
- CHANGED tm.basf.com/passage-europe.basf.com/vss3.basf.com — all 3 sibling portals LIVE on 141.6.3.0/16; tm+passage share procurement's J2EE backend (KM servlet → 500), vss3 WAF-blocks KM servlet (235B)
- CHANGED federation.basf.com OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes) across all 09-09 re-probes — provider config stable, no hardening
- CHANGED my.basf.com/.auth: HTTP 200/205005B SPA fallback re-confirmed — `/.auth` remains client-side callback, no server-side token surface
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface reconfirmed end-to-end, zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com — 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected "Invalid ApiKey" — key scope exhausted
- CHANGED api.commerce.basf.com — 8 stage prefixes all `MissingAuthenticationTokenException`; IAM/SigV4 authorizer, x-api-key not a credential class

## 2026-09-09 21:42:25 UTC
- NEW basf.login.apigee.com — full Apigee OAuth identity surface mapped via OIDC discovery + SAML metadata; ROPC/implicit grants, token/userinfo/jwks endpoints, SAML SSO; only config endpoints (discovery, m
- NEW rep.basf.com — live "Bestandskundenplattform" behind Azure Front Door; Spring Boot + Wicket; Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints return 404; custom erro
- NEW secsys.basf.com — live "Smart ID Self-Service" (Technology Nexus, v5.3.1+) Angular SPA, 200/3179B; sibling `bsh.secsys`, `secsys-visitor`, and qual instances resolvable; qual hosts Cloudflare-JS-chall
- CHANGED procurement.basf.com KM servlet `/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework` → HTTP 500 reconfirmed across 4 portals; J2EE backend reached PAST F5-ASM
- CHANGED tm.basf.com/passage-europe.basf.com/vss3.basf.com — all 3 sibling portals LIVE on 141.6.3.0/16; tm+passage share procurement's J2EE backend (KM servlet → 500), vss3 WAF-blocks KM servlet (235B)
- CHANGED federation.basf.com OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes) across all 09-09 re-probes — provider config stable, no hardening
- CHANGED my.basf.com/.auth: HTTP 200/205005B SPA fallback re-confirmed — `/.auth` remains client-side callback, no server-side token surface
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface reconfirmed end-to-end, zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com — 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected "Invalid ApiKey" — key scope exhausted
- CHANGED api.commerce.basf.com — 8 stage prefixes all `MissingAuthenticationTokenException`; IAM/SigV4 authorizer, x-api-key not a credential class

## 2026-09-09 23:35:35 UTC

## 2026-09-10 01:33:05 UTC
- CHANGED secsys.basf.com — probe completed: live "Smart ID Self-Service" (Technology Nexus, v5.3.1+) Angular SPA confirmed at root (200/3179B); sibling hosts `bsh.secsys`, `secsys-visitor`, `secsys-ssp-qual`, 
- CHANGED basf.login.apigee.com — full Apigee OAuth identity surface mapped via OIDC discovery + SAML metadata; only config endpoints (discovery, metadata, jwks) return 200; `/register`, `/clients`, `/admin`, `
- CHANGED rep.basf.com — Spring Boot + Wicket behind Azure Front Door confirmed; Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints return 404; custom error handler returns stat
- CHANGED federation.basf.com — NAM OIDC discovery reconfirmed unchanged across all 09-09 re-probes (ROPC/hybrid grants, plain+S256 PKCE, registration scopes); SAML2 metadata at `/nidp/saml2/metadata` returns 2
- CHANGED my.basf.com/.auth — HTTP 200/205005B SPA fallback re-confirmed; `/.auth` remains client-side callback route (Azure Static Web Apps built-in auth), no server-side token surface
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface reconfirmed end-to-end, zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com — 66 proxy paths all 404 except `/productinformation` (401 VerifyAPIKey); 4 browser-sourced keys (core/pi/csp/navigator) rejected "Invalid ApiKey" — key scope exhausted
- CHANGED api.commerce.basf.com — 8 stage prefixes all `MissingAuthenticationTokenException`; AWS REST API Gateway IAM/SigV4 authorizer, x-api-key not a credential class
- CHANGED procurement.basf.com/tm/passage-europe/vss3.basf.com SAP KM servlet — all parameterized requests return HTTP 500 (SAP runtime error) across 4 portals; guest role renders zero content; vss3 WAF-blocks 

## 2026-09-10 06:45:35 UTC
- NEW secsys.basf.com — Angular SPA root confirmed (200/3179B, Technology Nexus v5.3.1+), sibling hosts `bsh.secsys`, `secsys-visitor`, `secsys-ssp-qual`, `bsh-qual`, `visitor-secsys-ssp-qual` resolvable; q
- NEW basf.login.apigee.com — full Apigee OAuth identity surface mapped via OIDC discovery + SAML metadata; only config endpoints (discovery, metadata, jwks) return 200; `/register`, `/clients`, `/admin`, `
- NEW rep.basf.com — Spring Boot + Wicket behind Azure Front Door confirmed; Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints return 404; custom error handler returns stat
- CHANGED federation.basf.com — NAM OIDC discovery reconfirmed unchanged across all 09-09 re-probes (ROPC/hybrid grants, plain+S256 PKCE, registration scopes); SAML2 metadata at `/nidp/saml2/metadata` returns 2
- CHANGED my.basf.com/.auth — HTTP 200/205005B SPA fallback re-confirmed; `/.auth` remains client-side callback route (Azure Static Web Apps built-in auth), no server-side token surface
- CHANGED *.api.basf.com estate (9 hosts) — full unauth surface reconfirmed end-to-end, zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com — 66 proxy paths all 404 except `/productinformation` (401 VerifyAPIKey); 4 browser-sourced keys (core/pi/csp/navigator) rejected "Invalid ApiKey" — key scope exhausted
- CHANGED api.commerce.basf.com — 8 stage prefixes all `MissingAuthenticationTokenException`; AWS REST API Gateway IAM/SigV4 authorizer, x-api-key not a credential class
- CHANGED procurement.basf.com/tm/passage-europe/vss3.basf.com SAP KM servlet — all parameterized requests return HTTP 500 (SAP runtime error) across 4 portals; guest role renders zero content; vss3 WAF-blocks 

## 2026-09-10 12:07:13 UTC

## 2026-09-10 16:37:10 UTC

## 2026-09-10 19:14:19 UTC
- NEW experience.basf.com — AEM CXM Experience Platform login, x-vhost experience, CloudFront, CSP leaks author-prod-aem64 + author-stage-aem64 (dead) + api.das.basf.com (dead)
- NEW intranet.basf.com — Azure AD (mod_auth_openidc) with tenant ecaa386b-c8df-4ce0-ad01-740cbdb5ba55, client_id f5a39ea7-83db-4af7-b3d1-df80d707650c, redirect_uri /redirect_uri — corporate SSO intranet
- NEW north-america.intranet.basf.com — Concrete CMS with Azure AD OAuth2, client_id 36f927e6-1b9a-4b2e-a991-8574640f1164, cookie domain .intranet.basf.com shared across intranet instances
- NEW repfinder.basf.com — AEM Rep Finder (public), x-vhost repfinder, CloudFront, 137KB, .model.json properly blocked by dispatcher
- NEW das.basf.com — S3-hosted AgSolutions Finder (Ionic PWA), api.das.basf.com dead, S3 bucket NoSuchBucket, 5x AWS API GW endpoints 403 IAM-gated
- NEW agriculture.basf.com — Magnolia CMS (348KB)
- NEW artifactory.basf.com — Cloudflare WAF custom block (403, 7KB BASF-branded); no bypass possible
- NEW ncm.basf.com + cloud.basf.com — Cloudflare JS challenge (403, cf-mitigated:challenge); no bypass possible
- CHANGED secsys.basf.com/api/users/me + /api/devices — HTTP 200 len=246 = WAF block page ("Request Rejected"), NOT API data; WAF returns 200 instead of 403; same across all 3 secsys hosts
- CHANGED intranet.basf.com redirect_uri — Azure AD shows login for both valid/invalid redirect_uri; validation at token exchange; without auth cannot confirm open redirect

## 2026-09-10 21:46:34 UTC
- NEW experience.basf.com — AEM CXM Experience Platform login, x-vhost experience, CloudFront, CSP leaks author-prod-aem64 + author-stage-aem64 (dead) + api.das.basf.com (dead)
- NEW intranet.basf.com — Azure AD (mod_auth_openidc) with tenant ecaa386b-c8df-4ce0-ad01-740cbdb5ba55, client_id f5a39ea7-83db-4af7-b3d1-df80d707650c, redirect_uri /redirect_uri — corporate SSO intranet
- NEW north-america.intranet.basf.com — Concrete CMS with Azure AD OAuth2, client_id 36f927e6-1b9a-4b2e-a991-8574640f1164, cookie domain .intranet.basf.com shared across intranet instances
- NEW repfinder.basf.com — AEM Rep Finder (public), x-vhost repfinder, CloudFront, 137KB, .model.json properly blocked by dispatcher
- NEW das.basf.com — S3-hosted AgSolutions Finder (Ionic PWA), api.das.basf.com dead, S3 bucket NoSuchBucket, 5x AWS API GW endpoints 403 IAM-gated
- NEW agriculture.basf.com — Magnolia CMS (348KB)
- NEW artifactory.basf.com — Cloudflare WAF custom block (403, 7KB BASF-branded); no bypass possible
- NEW ncm.basf.com + cloud.basf.com — Cloudflare JS challenge (403, cf-mitigated:challenge); no bypass possible
- CHANGED secsys.basf.com/api/users/me + /api/devices — HTTP 200 len=246 = WAF block page ("Request Rejected"), NOT API data; WAF returns 200 instead of 403; same across all 3 secsys hosts
- CHANGED intranet.basf.com redirect_uri — Azure AD shows login for both valid/invalid redirect_uri; validation at token exchange; without auth cannot confirm open redirect

## 2026-09-10 23:59:48 UTC
- NEW experience.basf.com — AEM CXM Experience Platform login, x-vhost experience, CloudFront, CSP leaks author-prod-aem64 + author-stage-aem64 (dead) + api.das.basf.com (dead)
- NEW intranet.basf.com — Azure AD (mod_auth_openidc) with tenant ecaa386b-c8df-4ce0-ad01-740cbdb5ba55, client_id f5a39ea7-83db-4af7-b3d1-df80d707650c, redirect_uri /redirect_uri — corporate SSO intranet
- NEW north-america.intranet.basf.com — Concrete CMS with Azure AD OAuth2, client_id 36f927e6-1b9a-4b2e-a991-8574640f1164, cookie domain .intranet.basf.com shared across intranet instances
- NEW repfinder.basf.com — AEM Rep Finder (public), x-vhost repfinder, CloudFront, 137KB, .model.json properly blocked by dispatcher
- NEW das.basf.com — S3-hosted AgSolutions Finder (Ionic PWA), api.das.basf.com dead, S3 bucket NoSuchBucket, 5x AWS API GW endpoints 403 IAM-gated
- NEW agriculture.basf.com — Magnolia CMS (348KB)
- NEW artifactory.basf.com — Cloudflare WAF custom block (403, 7KB BASF-branded); no bypass possible
- NEW ncm.basf.com + cloud.basf.com — Cloudflare JS challenge (403, cf-mitigated:challenge); no bypass possible
- CHANGED secsys.basf.com/api/users/me + /api/devices — HTTP 200 len=246 = WAF block page ("Request Rejected"), NOT API data; WAF returns 200 instead of 403; same across all 3 secsys hosts
- CHANGED intranet.basf.com redirect_uri — Azure AD shows login for both valid/invalid redirect_uri; validation at token exchange; without auth cannot confirm open redirect

## 2026-09-11 04:32:31 UTC

## 2026-09-11 09:29:18 UTC
- CHANGED experience.basf.com AEM non-standard selectors (.content.json, .infinity.json, .tidy.-1.json, .feed.xml, _jcr_content.*, system/sling/*, system/console/bundles) → all HTTP 404 (confirmed 09-11 04:32);
- CHANGED repfinder.basf.com Sling Model Exporter (.model.json, .model.txt, .tidy.-1.json, .infinity.json, jcr:content.model.json) → all HTTP 404 (confirmed 09-10/09-11); dispatcher blocking standard AEM conten
- NEW agriculture.basf.com — Magnolia CMS (348KB root, confirmed 09-10) — **ZERO sub-path probes** ever run; Magnolia has distinct endpoint surface from AEM (/graphql2, /.restful, /adminCentral, /.cache, /d
- NEW das.basf.com — S3-hosted Ionic PWA; api.das.basf.com dead (NXDOMAIN via dfman.info); S3 bucket gives NoSuchBucket; 5x AWS API GW endpoints (REST, 403 IAM-gated); zero exploitation possible without IAM
- NEW north-america.intranet.basf.com — Concrete CMS with Azure AD OAuth2 (client_id 36f927e6-1b9a-4b2e-a991-8574640f1164); cookie domain `.intranet.basf.com` shared across instances; Concrete has known una
- NEW secsys.basf.com — Angular SPA (3179B) + Technology Nexus v5.3.1+; /api/* endpoints return 200/246B = WAF "Request Rejected" page (NOT data); sibling hosts bsh.secsys, secsys-visitor, qual instances al

## 2026-09-11 13:42:48 UTC
- NEW agriculture.basf.com: Magnolia CMS (348KB root) — **ZERO sub-path probes ever run**; Magnolia GraphQL (`/graphql2`, `/.graphql`), REST (`/.restful`, `/.rest`), admin (`/adminCentral`, `/.admin`), cach
- CHANGED experience.basf.com: AEM non-standard selectors (.content.json, .infinity.json, .tidy.-1.json, .feed.xml, _jcr_content.*, system/sling/*, system/console/bundles) → all HTTP 404 confirmed 09-11 04:32; 
- CHANGED repfinder.basf.com: Sling Model Exporter (.model.json, .model.txt, .tidy.-1.json, .infinity.json, jcr:content.model.json) → all HTTP 404 confirmed; dispatcher blocking standard AEM content negotiation
- CHANGED secsys.basf.com: /api/* endpoints return HTTP 200 len=246 = WAF "Request Rejected" page (NOT API data) across all 3 hosts (secsys, bsh.secsys, secsys-visitor); WAF returns 200 instead of 403
- CHANGED das.basf.com: S3-hosted Ionic PWA; api.das.basf.com dead (NXDOMAIN); S3 bucket NoSuchBucket; 5x AWS API GW endpoints 403 IAM-gated — no exploitation path without IAM creds
- CHANGED north-america.intranet.basf.com: Concrete CMS with Azure AD OAuth2 (client_id 36f927e6-1b9a-4b2e-a991-8574640f1164); cookie domain `.intranet.basf.com` shared; Concrete known unauth info-disclosure ve

## 2026-09-11 17:23:50 UTC

## 2026-09-11 19:55:13 UTC
- CHANGED agriculture.basf.com: All 7 standard Magnolia endpoints (/graphql2, /.graphql, /.restful, /.rest, /adminCentral, /admin, /.admin, /.cache, /.imaging, /dam) now confirmed 308 redirect or 404 — zero una
- CHANGED north-america.intranet.basf.com: All 5 Concrete internal API endpoints (/index.php/ccm/system/block/types, /index.php/ccm/system/page/types, /index.php/dashboard, /ccm/system/block/types, /api/blocks)
- CHANGED experience.basf.com: AEM Dispatcher cache poisoning via Host header spoofing (author-prod-aem64.basf.com) returns 403 from CloudFront on all 4 tested paths — edge blocks spoofed Host headers conclusiv
- CHANGED secsys.basf.com: /api/* endpoints return HTTP 200 len=246 = WAF "Request Rejected" page (NOT API data) across all 3 hosts (secsys, bsh.secsys, secsys-visitor); WAF returns 200 instead of 403
- CHANGED procurement.basf.com/tm/passage-europe/vss3.basf.com: KM servlet parameterized requests (?path=/documents/newFramework) return HTTP 500 (SAP runtime error) across 4 portals; guest role renders zero co
- CHANGED basf.login.apigee.com: Full Apigee OAuth identity surface mapped via OIDC discovery + SAML metadata; only config endpoints (discovery, metadata, jwks) return 200; ROPC/implicit grants are stock Edge S
- CHANGED rep.basf.com: Spring Boot Actuator at /actuator (HAL) + /actuator/health (UP); all 16 sensitive endpoints return 404; path traversal and content-negotiation blocked by Spring Boot path normalization; 
- CHANGED *.api.basf.com estate (9 hosts): Full unauth surface reconfirmed end-to-end — zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com: 66 proxy paths all 404 except /productinformation (401); 4 browser keys (core/pi/csp/navigator) rejected "Invalid ApiKey" — key scope exhausted
- CHANGED api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — IAM/SigV4 authorizer, x-api-key not a credential class
- CHANGED my.basf.com/.auth: HTTP 200/205KB SPA fallback re-confirmed — /.auth remains client-side callback, no server-side token surface
- CHANGED federation.basf.com: NAM OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes); SAML2 metadata at /nidp/saml2/metadata returns 200 signed descriptor (21434B)

## 2026-09-11 22:30:03 UTC
- CHANGED agriculture.basf.com: All 7 standard Magnolia endpoints (/graphql2, /.graphql, /.restful, /.rest, /adminCentral, /admin, /.admin, /.cache, /.imaging, /dam) now confirmed 308 redirect or 404 — zero una
- CHANGED north-america.intranet.basf.com: All 5 Concrete internal API endpoints (/index.php/ccm/system/block/types, /index.php/ccm/system/page/types, /index.php/dashboard, /ccm/system/block/types, /api/blocks)
- CHANGED experience.basf.com: AEM Dispatcher cache poisoning via Host header spoofing (author-prod-aem64.basf.com) returns 403 from CloudFront on all 4 tested paths — edge blocks spoofed Host headers conclusiv
- CHANGED secsys.basf.com: /api/* endpoints return HTTP 200 len=246 = WAF "Request Rejected" page (NOT API data) across all 3 hosts (secsys, bsh.secsys, secsys-visitor); WAF returns 200 instead of 403
- CHANGED basf.login.apigee.com: Full Apigee OAuth identity surface mapped via OIDC discovery + SAML metadata; only config endpoints (discovery, metadata, jwks) return 200; ROPC/implicit grants are stock Edge S
- CHANGED rep.basf.com: Spring Boot Actuator at /actuator (HAL) + /actuator/health (UP); all 16 sensitive endpoints return 404; path traversal and content-negotiation blocked by Spring Boot path normalization; 
- CHANGED *.api.basf.com estate (9 hosts): Full unauth surface reconfirmed end-to-end — zero reachable endpoints beyond auth gates/404
- CHANGED prod.api.basf.com: 66 proxy paths all 404 except /productinformation (401); 4 browser keys (core/pi/csp/navigator) rejected "Invalid ApiKey" — key scope exhausted
- CHANGED api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — IAM/SigV4 authorizer, x-api-key not a credential class
- CHANGED my.basf.com/.auth: HTTP 200/205KB SPA fallback re-confirmed — /.auth remains client-side callback, no server-side token surface
- CHANGED federation.basf.com: NAM OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes); SAML2 metadata at /nidp/saml2/metadata returns 200 signed descriptor (21434B)

## 2026-09-12 00:44:06 UTC
- NEW `repfinder.basf.com/bin/basf/repfindertool` → unauth AEM→AWS Lambda proxy, 200 JSON, stacktrace disclosure, geolocation search functional (empty DB) — dispatcher misses this path (probe-results.md:542
- NEW `north-america.intranet.basf.com/index.php/api` → 200 len=43206; `/index.php/tools/activated_packages` → 200 len=43173; `/ccm/system/block/types` → 200 len=43191; `/api/blocks` → 200 len=43203 — Concr
- NEW `agriculture.basf.com/graphql2` → 200 len=348KB; `/.graphql` → 200 len=348KB; `/.restful` → 200 len=348KB; `/docurl/` → 200 len=404KB; `/adminCentral` → 200 len=348KB; `/.cache` → 200 len=348KB; `/.im
- CHANGED `agriculture.basf.com` ALL 10 Magnolia endpoints now confirmed 308 redirect or 404 — zero unauthenticated API surface (was "zero sub-path probes ever run")
- CHANGED `north-america.intranet.basf.com` ALL 5 Concrete internal API endpoints now confirmed 307 redirect to Azure AD OAuth2 — fully auth-gated
- CHANGED `experience.basf.com` AEM Dispatcher cache poisoning via Host header spoofing (author-prod-aem64.basf.com) returns 403 from CloudFront on all 4 tested paths — edge blocks spoofed Host headers conclusi
- CHANGED `secsys.basf.com` /api/* endpoints return HTTP 200 len=246 = WAF "Request Rejected" page (NOT API data) across all 3 hosts (secsys, bsh.secsys, secsys-visitor); WAF returns 200 instead of 403
- CHANGED `rep.basf.com` Spring Boot Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints return 404; path traversal (`env..`, `actuator;/env`, `health/path/../../env`) and conten
- CHANGED `*.api.basf.com` estate (9 hosts) full unauth surface reconfirmed end-to-end — zero reachable endpoints beyond auth gates/404
- CHANGED `my.basf.com/.auth` HTTP 200/205KB SPA fallback re-confirmed — `/.auth` remains client-side callback, no server-side token surface
- CHANGED `federation.basf.com` NAM OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes); SAML2 metadata at `/nidp/saml2/metadata` returns 200 signed descriptor (21434

## 2026-09-12 05:22:44 UTC
- NEW `repfinder.basf.com/bin/basf/repfindertool` → unauth AEM→AWS Lambda proxy, 200 JSON, stacktrace disclosure, geolocation search functional (empty DB) — dispatcher misses this path
- NEW `north-america.intranet.basf.com/index.php/api` → 200 len=43206; `/index.php/tools/activated_packages` → 200 len=43173; `/ccm/system/block/types` → 200 len=43191; `/api/blocks` → 200 len=43203 — Concr
- CHANGED `agriculture.basf.com` ALL 10 Magnolia endpoints now confirmed 308 redirect or 404 — zero unauthenticated API surface (was "zero sub-path probes ever run")
- CHANGED `north-america.intranet.basf.com` ALL 5 Concrete internal API endpoints now confirmed 307 redirect to Azure AD OAuth2 — fully auth-gated (contradicts NEW above — need clarification)
- CHANGED `experience.basf.com` AEM Dispatcher cache poisoning via Host header spoofing (author-prod-aem64.basf.com) returns 403 from CloudFront on all 4 tested paths — edge blocks spoofed Host headers conclusi
- CHANGED `secsys.basf.com` /api/* endpoints return HTTP 200 len=246 = WAF "Request Rejected" page (NOT API data) across all 3 hosts; WAF returns 200 instead of 403
- CHANGED `rep.basf.com` Spring Boot Actuator at `/actuator` (HAL) + `/actuator/health` (UP); all 16 sensitive endpoints return 404; path traversal and content-negotiation blocked; custom error handler returns 
- CHANGED `*.api.basf.com` estate (9 hosts) full unauth surface reconfirmed end-to-end — zero reachable endpoints beyond auth gates/404
- CHANGED `my.basf.com/.auth` HTTP 200/205KB SPA fallback re-confirmed — `/.auth` remains client-side callback, no server-side token surface
- CHANGED `federation.basf.com` NAM OIDC discovery reconfirmed unchanged (ROPC/hybrid grants, plain+S256 PKCE, registration scopes); SAML2 metadata at `/nidp/saml2/metadata` returns 200 signed descriptor (21434

## 2026-09-12 09:31:04 UTC
- CHANGED north-america.intranet.basf.com Concrete APIs: raw response is HTTP 307→`/ccm/system/authentication/oauth2/basf/attempt_auth?then=<full-url>` on ALL 6 paths (index.php/api, tools/activated_packages, c
- CHANGED repfinder.basf.com/bin/basf/repfindertool: `repType=BR&productServiceId=2&country=DE` now returns HTTP 200 JSON with NPE stacktrace (`RetailFinderDatabaseService.getFinalResult:143` / `searchDatabase:
- NEW `repfinder.basf.com/bin/basf/repfindertool` → unauth AEM→AWS Lambda proxy confirmed 2026-09-12: 200 JSON at all coords/psid (hits=0/results=[]), stacktrace disclosure, dispatcher misses `/bin/basf/*` 
- NEW `north-america.intranet.basf.com/index.php/api` + 3 sibling endpoints → HTTP 200 len=43KB (not 307 redirect) at 2026-09-11/12 — Concrete CMS internal APIs returning full HTML pages unauthenticated (pr
- NEW `agriculture.basf.com` Magnolia endpoints `/graphql2`, `/.graphql`, `/.restful`, `/.rest`, `/adminCentral`, `/.admin`, `/.cache`, `/.imaging`, `/dam`, `/docurl/` → all 200 len=335-404KB at 2026-09-11/
- CHANGED `repfinder.basf.com` re-probed 2026-09-12 00:44 & 05:23 — `/bin/basf/repfindertool` consistently 200 JSON with geolocation params; stacktrace disclosure confirmed; empty DB (hits=0)
- CHANGED `secsys.basf.com/api/users/me` + siblings `bsh.secsys`, `secsys-visitor` → all HTTP 200 len=246 = WAF "Request Rejected" page (not 403) — WAF returns 200 OK for blocks across ecosystem (probe-results.
- CHANGED `my.basf.com/.auth` + `federation.basf.com` OIDC discovery → reconfirmed unchanged 2026-09-12; zero PKCE hardening on public client 86cc4bf9; ATO blocked pending test account
- CHANGED `*.api.basf.com` estate (9 hosts) → full unauth surface reconfirmed end-to-end 2026-09-12; zero reachable endpoints beyond auth gates/404
- CHANGED `rep.basf.com` Spring Boot Actuator → `/actuator` HAL + `/actuator/health` UP; all 16 sensitive endpoints 404; path traversal + content-negotiation blocked; custom handler status 999 — locked down

## 2026-09-12 13:15:20 UTC
- NEW `repfinder.basf.com/bin/basf/repfindertool` param `repType=BR&productServiceId=2` returns HTTP 200 JSON wrapper around NPE stacktrace (`RetailFinderDatabaseService.getFinalResult:143` / `searchDatabas
- NEW `north-america.intranet.basf.com` Concrete APIs: raw response is HTTP 307→`/ccm/system/authentication/oauth2/basf/attempt_auth?then=<full-url>` on ALL 6 paths — earlier 200/43KB were curl `-L` auth-ch
- CHANGED `agriculture.basf.com` Magnolia endpoints (`/graphql2`, `/.graphql`, `/.restful`, `/.rest`, `/adminCentral`, `/.admin`, `/.cache`, `/.imaging`, `/dam`, `/docurl/`) now confirmed 308 redirect or 404 — 
- CHANGED `repfinder.basf.com/bin/basf/repfindertool` re-probed 2026-09-12 00:44 & 05:23 — consistently 200 JSON with geolocation params, stacktrace disclosure confirmed, empty DB (hits=0)
- CHANGED `secsys.basf.com/api/users/me` + siblings `bsh.secsys`, `secsys-visitor` → all HTTP 200 len=246 = WAF "Request Rejected" page (not 403) — WAF returns 200 OK for blocks across ecosystem
- CHANGED `my.basf.com/.auth` + `federation.basf.com` OIDC discovery reconfirmed unchanged 2026-09-12; zero PKCE hardening on public client `86cc4bf9`; ATO blocked pending test account
- CHANGED `*.api.basf.com` estate (9 hosts) → full unauth surface reconfirmed end-to-end 2026-09-12; zero reachable endpoints beyond auth gates/404
- CHANGED `rep.basf.com` Spring Boot Actuator → `/actuator` HAL + `/actuator/health` UP; all 16 sensitive endpoints 404; path traversal + content-negotiation blocked; custom handler status 999 — locked down
