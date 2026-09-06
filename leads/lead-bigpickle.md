## 2026-09-03 17:47:35 UTC [target] (model bigpickle)
[HYP] Azure Functions SSRF to cloud metadata via url parameter
class: SSRF
asset: ap-eupf.api.basf.com
confidence: 85
reasoning: Azure Functions HTTP triggers accept arbitrary url params; cloud metadata 169.254.169.254 reachable from App Service; host confirmed HTTP 200
evidence_needed: metadata JSON response body (instance, identity, compute keys)
verify_steps: GET https://ap-eupf.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01
impact: Cloud managed identity token theft, lateral move to linked Azure resources, data exfiltration from storage/keyvault; CRITICAL
testability: PASSIVE
[HYP] Azure Functions SSRF to cloud metadata via url parameter
class: SSRF
asset: ap-digitalconnect.api.basf.com
confidence: 60
reasoning: Same Azure Functions platform as ap-eupf; HTTP 200 suggests active endpoints; Digital Connect likely proxies external data
evidence_needed: metadata JSON response or error distinguishing valid SSRF from generic error
verify_steps: GET https://ap-digitalconnect.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01
impact: Same as above; CRITICAL
testability: PASSIVE
[HYP] Dev endpoint information disclosure via error messages
class: MISCONFIG
asset: dev-ext001.api.basf.com
confidence: 45
reasoning: HTTP 400 without client cert suggests structured error response; dev endpoints may leak stack traces or internal paths
evidence_needed: Full error response body showing internal details
verify_steps: GET https://dev-ext001.api.basf.com/ and inspect response body for stack traces, internal paths, or version info
impact: Internal information disclosure aiding further attacks; LOW-MEDIUM
testability: PASSIVE
[NEXT] PROBE: GET https://ap-eupf.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01
[LEARN] ACCEPTED SSRF @ ap-eupf.api.basf.com: Azure Functions confirmed via KB; metadata endpoint reachable from App Service; concrete url param for testing
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com: master key leak lacks verify without auth; passive GitHub search not systematic
[LEARN] REJECTED AUTH @ dev-clientcert-sap.api.basf.com: mTLS bypass speculative; no CT evidence of public CA use
[RISK] BASF SE program: 70 — Large cloud surface across Azure Functions + SAP dev endpoints; SSRF→metadata chain high impact if confirmed; dev endpoints may have weaker controls but lower business value
## 2026-09-03 20:54:49 UTC [target] (model bigpickle)
[HYP] SSRF via header-injected URL on Azure Functions health endpoint
class: SSRF
asset: ap-eupf.api.basf.com
confidence: 62
reasoning: url param probes return 403 (WAF); Azure Functions HTTP triggers may forward X-Forwarded-For, X-Original-URL, or Referer headers to backend; metadata endpoint confirmed reachable from App Service; common pattern in Azure Functions with reverse proxy
evidence_needed: metadata JSON response or header-specific 403/timeout distinguishing blocked vs allowed header injection
verify_steps: GET https://ap-eupf.api.basf.com/api/health with headers: X-Forwarded-For: 169.254.169.254 / Referer: http://169.254.169.254/metadata/instance?api-version=2021-02-01
impact: Managed identity token theft, lateral move to linked Azure resources, keyvault/storage exfiltration; CRITICAL
testability: PASSIVE
[HYP] Azure Function admin key via non-standard path enumeration
class: AUTH
asset: ap-digitalconnect.api.basf.com
confidence: 50
reasoning: /admin/host/keys returns 404 not 401; admin surface non-standard — suggests custom routing; alternative paths (/admin/list, /admin/keys, /admin/.env, /.azure-functions-host) may expose master key or admin dashboard; Azure Functions v4 default admin path varies by extension version
evidence_needed: HTTP 200 with master key in response body or admin dashboard HTML
verify_steps: GET https://ap-digitalconnect.api.basf.com/admin/list, GET /admin/keys, GET /.azure-functions-host
impact: Full admin control over Azure Function app, code execution, data access; CRITICAL
testability: PASSIVE
[HYP] Dev endpoint stack trace / debug info disclosure
class: MISCONFIG
asset: dev-ext001.api.basf.com
confidence: 48
reasoning: HTTP 400 without client cert; dev endpoints often have verbose error responses; stack traces may leak internal paths, framework versions, or connection strings
evidence_needed: Full error response body with stack trace, internal paths, or version strings
verify_steps: GET https://dev-ext001.api.basf.com/ and inspect for stack traces, internal paths, or version info
impact: Internal information disclosure aiding further attacks; LOW-MEDIUM
testability: PASSIVE
[NEXT] PROBE: GET https://ap-eupf.api.basf.com/api/health with headers X-Forwarded-For: 169.254.169.254 and Referer: http://169.254.169.254/metadata/instance?api-version=2021-02-01 (test header-based SSRF bypass on Azure Functions)
[LEARN] REJECTED MISCONFIG @ dev-ext001.api.basf.com: confidence too low for active probe; dev client cert requirement blocks passive disclosure
[LEARN] ACCEPTED SSRF @ ap-eupf.api.basf.com: Azure Functions confirmed; metadata endpoint reachable from App Service; url param blocked by WAF; header injection remains untested
[RISK] BASF SE program: 68 — Large cloud surface across Azure Functions + SAP dev endpoints; SSRF→metadata chain high impact if confirmed; WAF blocks common url param injection; header-based bypass needs validation; dev endpoints may have weaker controls but lower business value
## 2026-09-03 23:12:07 UTC [target] (model bigpickle)
[HYP] Azure Functions SSRF to cloud metadata via header injection
class: SSRF
asset: ap-eupf.api.basf.com
confidence: 65
reasoning: url param blocked by WAF (403); header-based probes (X-Forwarded-Url, X-Callback-Url) return 200 but headers ignored per KB; Azure Functions HTTP triggers accept arbitrary url params; metadata endpoint 169.254.169.254 reachable from App Service per prior KB acceptance; standard health endpoint exists at /api/health; no common function names resolve; header injection remains untested with content-type or Referer vectors
evidence_needed: metadata JSON response body or timeout/error distinguishing allowed vs blocked SSRF
verify_steps: GET https://ap-eupf.api.basf.com/api/health with headers: Referer: http://169.254.169.254/metadata/instance?api-version=2021-02-01 AND Content-Type: application/json
impact: Managed identity token theft, lateral move to linked Azure resources, keyvault/storage exfiltration; CRITICAL
testability: PASSIVE
[HYP] Azure Function admin key via non-standard path enumeration
class: AUTH
asset: ap-digitalconnect.api.basf.com
confidence: 55
reasoning: /admin/host/keys returns 401 (confirmed standard); standard admin surface is auth-gated; alternative paths (/admin/list, /admin/keys, /.azure-functions-host) remain untested; Azure Functions v4 default admin path varies by extension version; Digital Connect likely proxies external data
evidence_needed: HTTP 200 with master key in response body or admin dashboard HTML
verify_steps: GET https://ap-digitalconnect.api.basf.com/admin/list, GET /admin/keys, GET /.azure-functions-host
impact: Full admin control over Azure Function app, code execution, data access; CRITICAL
testability: PASSIVE
[HYP] Dev endpoint stack trace / debug info disclosure
class: MISCONFIG
asset: dev-ext001.api.basf.com
confidence: 50
reasoning: HTTP 400 without client cert; dev endpoints often have verbose error responses; stack traces may leak internal paths, framework versions, or connection strings; low confidence due to active probe requirement
evidence_needed: Full error response body with stack trace, internal paths, or version strings
verify_steps: GET https://dev-ext001.api.basf.com/ and inspect for stack traces, internal paths, or version info
impact: Internal information disclosure aiding further attacks; LOW-MEDIUM
testability: PASSIVE
[PARKED] Dev endpoint stack trace / debug info disclosure: confidence 50 < threshold; KB says dev client cert requirement blocks passive disclosure; no concrete verify step without auth
[FINAL] Azure Functions SSRF to cloud metadata via header injection — 65, SSRF, ap-eupf.api.basf.com
[FINAL] Azure Function admin key via non-standard path enumeration — 55, AUTH, ap-digitalconnect.api.basf.com
[NEXT] PROBE: GET https://ap-eupf.api.basf.com/api/health with headers Referer: http://169.254.169.254/metadata/instance?api-version=2021-02-01 (test header-based SSRF bypass on Azure Functions)
[LEARN] REJECTED MISCONFIG @ dev-ext001.api.basf.com: confidence too low for active probe; dev client cert requirement blocks passive disclosure
[LEARN] ACCEPTED SSRF @ ap-eupf.api.basf.com: Azure Functions confirmed; metadata endpoint reachable from App Service; url param blocked by WAF; header injection remains untested
[LEARN] ACCEPTED AUTH @ ap-digitalconnect.api.basf.com: standard `/admin/host/keys` and `/runtime/webhooks/host/keys` return 401 (not 404); admin surface is standard Azure Functions, auth-gated — shifts key leak hunt to alt paths (`.azurefunctions/keys`, versioned) + passive repo search
[LEARN] MITIGATED SSRF @ ap-eupf.api.basf.com: common `url` param probes return 403; WAF/edge blocks metadata endpoint; header-based probes (`X-Forwarded-Url`, `X-Callback-Url`) return 200 but headers ignored; no function names resolve at `/api/`; SSRF vector appears mitigated at edge for unauthenticated requests
[LEARN] REJECTED AUTH @ dev-clientcert-sap.api.basf.com: mTLS bypass speculative; no CT evidence of public CA use
[RISK] BASF SE: 68 — Large cloud surface across Azure Functions + SAP dev endpoints; SSRF→metadata chain high impact if confirmed; WAF blocks common url param injection; header-based bypass needs validation; dev endpoints may have weaker controls but lower business value
## 2026-09-04 01:06:30 UTC [target] (model bigpickle)
[HYP] Azure Functions admin/system key via deployment-slot key inheritance on Function App
class: AUTH
asset: ap-digitalconnect.api.basf.com
confidence: 42
reasoning: Standard admin endpoints (/admin/host/status, /admin/host/keys, /admin/functions) return 401 key-gated; all alternate paths tested 404; Azure Function App deployment slots share the host key store unless sync-slot-settings is disabled, and slot keys are retrievable via `/admin/host/systemkeys` when read-access is misconfigured; Front Door (x-azure-ref) fronts both apps
evidence_needed: HTTP 200 exposing host/system key JSON, or 401 confirming continued gating
verify_steps: GET https://ap-digitalconnect.api.basf.com/admin/host/systemkeys (slot system keys), GET https://ap-digitalconnect.api.basf.com/admin/functions (function list), GET https://ap-digitalconnect.api.basf.com/admin/host/status
impact: Full Function App takeover → code execution, env var/conn-string disclosure, Azure lateral movement; CRITICAL
testability: PASSIVE
[HYP] Root Function App landing page discloses deployment/slot tokens in response headers
class: MISCONFIG
asset: ap-eupf.api.basf.com
confidence: 40
reasoning: Root serves default landing page via Front Door (x-azure-ref present); Azure Function App default pages sometimes append staging slot name or token in Location/Set-Cookie on slot traffic; would indicate slot routing misconfig
evidence_needed: Response header revealing slot name, token, or slot-specific origin
verify_steps: GET https://ap-eupf.api.basf.com/ and inspect all response headers (Location, Set-Cookie, x-ms-*); compare to ap-digitalconnect fingerprint
impact: Reveals deployment topology/slots aiding key-theft; LOW
testability: PASSIVE
[NEXT] PROBE: GET https://ap-digitalconnect.api.basf.com/admin/host/systemkeys (test Function App slot/system-keys endpoint — last untested standard admin sub-resource; expect 401, confirm gating, ~1 rps)
[RISK] BASF SE: 45 — Two production Azure Function Apps (Digital Connect + EU PF), both behind Azure Front Door with a fully key-gated admin API (401 on standard paths, 404 on all tested alt paths) and WAF-blocked SSRF params; dev SAP endpoints mTLS-gated; root is a stock default page. No unauthenticated finding survives. Residual risk concentrates in (a) a leaked/weak host key via CI/CD or slot misconfig and (b) any authenticated function trigger that fetches user URLs — both require authenticated or internal access. Low unauth exposure, moderate overall due to business value of SAP/Digital Connect backend if any gate falls.
## 2026-09-04 06:04:36 UTC [target] (model bigpickle)
[CHANGED] `ap-digitalconnect.api.basf.com/admin/host/systemkeys` → HTTP 404 (was planned probe; now confirmed in probe-results.md)
[CHANGED] `ap-digitalconnect.api.basf.com/admin/functions` → HTTP 404 (was planned; confirmed)
[CHANGED] `ap-digitalconnect.api.basf.com/admin/host/status` → HTTP 404 (was planned; confirmed)
[NEW] `e-gate.api.basf.com` → HTTP 404 at root, ZERO probes run against any sub-paths (untested surface)
[NEW] `ap-digitalconnect.api.basf.com/admin/host/functionkeys` → UNTESTED (v4 function-level key endpoint)
[NEW] `ap-digitalconnect.api.basf.com/admin/system` → UNTESTED (admin system info endpoint)
[PRIO] e-gate.api.basf.com,6.8,attack_surface=7,business_value=8,tech_exposure=7,gate_ease=6,cloud_surface=3,freshness=10
[PRIO] ap-eupf.api.basf.com,5.9,attack_surface=5,business_value=8,tech_exposure=5,gate_ease=3,cloud_surface=8,freshness=8
[PRIO] ap-digitalconnect.api.basf.com,5.5,attack_surface=5,business_value=9,tech_exposure=5,gate_ease=2,cloud_surface=8,freshness=8
[PRIO] dev-int001.api.basf.com,3.2,attack_surface=3,business_value=5,tech_exposure=3,gate_ease=1,cloud_surface=4,freshness=8
[HYP] API gateway OpenAPI/Swagger docs or hidden API routes on e-gate
class: MISCONFIG
asset: e-gate.api.basf.com
confidence: 55
reasoning: Host named "e-gate" suggests API gateway for business transactions (procurement, supplier, logistics). Standard root returns 404 — no probes have touched sub-paths. Common API gateway platforms (Azure APIM, Kong, AWS API GW) expose `/swagger`, `/openapi.json`, `/api-docs`, `/_swagger`, `/docs`, `/.well-known/openapi` at root. If exposed, full endpoint map including IDOR/BOLA-prone business-logic routes becomes visible.
evidence_needed: HTTP 200 with JSON/YAML OpenAPI spec, or Swagger UI HTML, or any non-404 response on documented sub-paths
verify_steps: GET https://e-gate.api.basf.com/openapi.json, GET /swagger.json, GET /api-docs, GET /swagger/ui, GET /_swagger, GET /docs, GET /.well-known/openapi-schema
impact: Full API surface disclosure enabling targeted auth/logic/IDOR testing; MEDIUM-HIGH (roadmap to HIGH-VALUE finding)
testability: PASSIVE
[HYP] Root 150KB page on ap-eupf contains API route map or function discovery endpoint
class: MISCONFIG
asset: ap-eupf.api.basf.com
confidence: 42
reasoning: Root returns 200 with 150093 bytes — far too large for default Azure Functions placeholder (typically <5KB). Likely a custom SPA, API documentation page, or function dashboard. Response body may contain JavaScript bundle with route definitions, fetch() calls to `/api/<function>` paths, or Azure Functions runtime config. All `/api/<function>` probes returned 404 — function names may be in the JS bundle rather than discoverable via path brute.
evidence_needed: Response Content-Type header; any `<script src>` tags, fetch() URLs, or `/api/` path references in the 150KB body
verify_steps: GET https://ap-eupf.api.basf.com/ with Accept: text/html — extract Content-Type and inspect response body for route references
impact: Function name disclosure enabling targeted SSRF or logic testing; MEDIUM
testability: PASSIVE
[HYP] Admin function-level keys endpoint on ap-digitalconnect via v4 system key path
class: AUTH
asset: ap-digitalconnect.api.basf.com
confidence: 35
reasoning: 6 standard admin paths tested (401/404), but Azure Functions v4 introduced `/admin/host/functionkeys` and `/admin/system` endpoints that may not be gated by the same middleware as `/admin/host/keys`. Low probability because Front Door + WAF likely apply uniform routing rules, but last-resort untested standard paths.
evidence_needed: HTTP 200 with function key JSON
verify_steps: GET https://ap-digitalconnect.api.basf.com/admin/host/functionkeys, GET https://ap-digitalconnect.api.basf.com/admin/system
impact: Function-level key disclosure; MEDIUM
testability: PASSIVE
[PARKED] Admin function-level keys endpoint on ap-digitalconnect via v4 system key path: confidence 35 — below 40 threshold. 6 admin paths already tested uniformly return 401/404; Azure Front Door + WAF applies consistent routing; functionkeys/system endpoints are unlikely to bypass existing auth gate. Low ROI.
[FINAL] API gateway OpenAPI/Swagger docs or hidden API routes on e-gate — 55, MISCONFIG, e-gate.api.basf.com
[FINAL] Root 150KB page on ap-eupf contains API route map or function discovery endpoint — 42, MISCONFIG, ap-eupf.api.basf.com
[NEXT] PROBE: GET https://e-gate.api.basf.com/openapi.json && GET /swagger.json && GET /api-docs && GET /swagger/ui && GET /docs && GET /.well-known/openapi-schema (test 6 common API gateway documentation paths on untested host — ~1 rps, GET only)
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/systemkeys: returns 404; system keys endpoint not exposed
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/functions: returns 404; admin function list not exposed
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/status: returns 404; admin host status not exposed
[LEARN] ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 6 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface fully mapped — no unauthenticated path exists
[LEARN] ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; zero probes run; API gateway naming convention warrants documentation path enumeration
[RISK] BASF SE: 42 — Two production Azure Function Apps (Digital Connect + EU PF) with fully mapped admin surface (401/404 on all 6 standard paths), WAF-blocked SSRF params, mTLS-gated dev endpoints. e-gate.api.basf.com is the last untested production surface — if API docs are exposed, attack surface jumps significantly. If not, program is near-exhausted for unauthenticated testing. Residual risk requires authenticated access (Function App keys, client certs, or CI/CD artifact leaks via passive repo search) for any finding. Low overall unauth exposure.
## 2026-09-04 11:29:29 UTC [target] (model bigpickle)
[PRIO] e-gate.api.basf.com,6.8,attack_surface=7,business_value=8,tech_exposure=7,gate_ease=6,cloud_surface=3,freshness=10
[PRIO] ap-eupf.api.basf.com,5.9,attack_surface=5,business_value=8,tech_exposure=5,gate_ease=3,cloud_surface=8,freshness=8
[PRIO] ap-digitalconnect.api.basf.com,5.5,attack_surface=5,business_value=9,tech_exposure=5,gate_ease=2,cloud_surface=8,freshness=8
[HYP] API gateway OpenAPI/Swagger docs or hidden API routes on e-gate
class: MISCONFIG
asset: e-gate.api.basf.com
confidence: 55
reasoning: Host named "e-gate" suggests API gateway for business transactions (procurement, supplier, logistics). Standard root returns 404 — no probes have touched sub-paths. Common API gateway platforms (Azure APIM, Kong, AWS API GW) expose `/swagger`, `/openapi.json`, `/api-docs`, `/_swagger`, `/docs`, `/.well-known/openapi` at root. If exposed, full endpoint map including IDOR/BOLA-prone business-logic routes becomes visible.
evidence_needed: HTTP 200 with JSON/YAML OpenAPI spec, or Swagger UI HTML, or any non-404 response on documented sub-paths
verify_steps: GET https://e-gate.api.basf.com/openapi.json, GET /swagger.json, GET /api-docs, GET /swagger/ui, GET /_swagger, GET /docs, GET /.well-known/openapi-schema
impact: Full API surface disclosure enabling targeted auth/logic/IDOR testing; MEDIUM-HIGH (roadmap to HIGH-VALUE finding)
testability: PASSIVE
[HYP] Root 150KB page on ap-eupf contains API route map or function discovery endpoint
class: MISCONFIG
asset: ap-eupf.api.basf.com
confidence: 42
reasoning: Root returns 200 with 150093 bytes — far too large for default Azure Functions placeholder (typically <5KB). Likely a custom SPA, API documentation page, or function dashboard. Response body may contain JavaScript bundle with route definitions, fetch() calls to `/api/<function>` paths, or Azure Functions runtime config. All `/api/<function>` probes returned 404 — function names may be in the JS bundle rather than discoverable via path brute.
evidence_needed: Response Content-Type header; any `<script src>` tags, fetch() URLs, or `/api/` path references in the 150KB body
verify_steps: GET https://ap-eupf.api.basf.com/ with Accept: text/html — extract Content-Type and inspect response body for route references
impact: Function name disclosure enabling targeted SSRF or logic testing; MEDIUM
testability: PASSIVE
[HYP] Admin function-level keys endpoint on ap-digitalconnect via v4 system key path
class: AUTH
asset: ap-digitalconnect.api.basf.com
confidence: 35
reasoning: 6 standard admin paths tested (401/404), but Azure Functions v4 introduced `/admin/host/functionkeys` and `/admin/system` endpoints that may not be gated by the same middleware as `/admin/host/keys`. Low probability because Front Door + WAF likely apply uniform routing rules, but last-resort untested standard paths.
evidence_needed: HTTP 200 with function key JSON
verify_steps: GET https://ap-digitalconnect.api.basf.com/admin/host/functionkeys, GET https://ap-digitalconnect.api.basf.com/admin/system
impact: Function-level key disclosure; MEDIUM
testability: PASSIVE
[NEXT] PROBE: GET https://e-gate.api.basf.com/openapi.json && GET /swagger.json && GET /api-docs && GET /swagger/ui && GET /docs && GET /.well-known/openapi-schema (test 6 common API gateway documentation paths on untested host — ~1 rps, GET only)
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/systemkeys: returns 404; system keys endpoint not exposed
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/functions: returns 404; admin function list not exposed
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/status: returns 404; host status endpoint not exposed
[LEARN] ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 6 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface fully mapped — no unauthenticated path exists
[LEARN] ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; zero probes run; API gateway naming convention warrants documentation path enumeration
[RISK] BASF SE: 42 — Two production Azure Function Apps (Digital Connect + EU PF) with fully mapped admin surface (401/404 on all 6 standard paths), WAF-blocked SSRF params, mTLS-gated dev endpoints. e-gate.api.basf.com is the last untested production surface — if API docs are exposed, attack surface jumps significantly. If not, program is near-exhausted for unauthenticated testing. Residual risk requires authenticated access (Function App keys, client certs, or CI/CD artifact leaks via passive repo search) for any finding. Low overall unauth exposure.
## 2026-09-04 15:07:26 UTC [target] (model bigpickle)
[PRIO] e-gate.api.basf.com,7.4,attack_surface=7,business_value=8,tech_exposure=7,gate_ease=6,cloud_surface=3,freshness=10
[PRIO] ap-eupf.api.basf.com,5.9,attack_surface=5,business_value=8,tech_exposure=5,gate_ease=3,cloud_surface=8,freshness=8
[PRIO] ap-digitalconnect.api.basf.com,5.5,attack_surface=5,business_value=9,tech_exposure=5,gate_ease=2,cloud_surface=8,freshness=8
[HYP] API gateway OpenAPI/Swagger documentation exposure on e-gate
class: MISCONFIG
asset: e-gate.api.basf.com
confidence: 68
reasoning: Host named "e-gate" suggests API gateway for business transactions (procurement, supplier, logistics). Standard root returns 404 — zero probes have touched sub-paths. Common API gateway platforms (Azure APIM, Kong, AWS API GW) expose `/swagger`, `/openapi.json`, `/api-docs`, `/_swagger`, `/docs`, `/.well-known/openapi` at root. If exposed, full endpoint map including IDOR/BOLA-prone business-logic routes becomes visible.
evidence_needed: HTTP 200 with JSON/YAML OpenAPI spec, or Swagger UI HTML, or any non-404 response on documented sub-paths
verify_steps: GET https://e-gate.api.basf.com/openapi.json, GET /swagger.json, GET /api-docs, GET /swagger/ui, GET /_swagger, GET /docs, GET /.well-known/openapi-schema
impact: Full API surface disclosure enabling targeted auth/logic/IDOR testing; MEDIUM-HIGH (roadmap to HIGH-VALUE finding)
testability: PASSIVE
[HYP] Root 150KB page on ap-eupf contains API route map or function discovery endpoint
class: MISCONFIG
asset: ap-eupf.api.basf.com
confidence: 48
reasoning: Root returns 200 with 150093 bytes — far too large for default Azure Functions placeholder (typically <5KB). Likely a custom SPA, API documentation page, or function dashboard. Response body may contain JavaScript bundle with route definitions, fetch() calls to `/api/<function>` paths, or Azure Functions runtime config. All `/api/<function>` probes returned 404 — function names may be in the JS bundle rather than discoverable via path brute.
evidence_needed: Response Content-Type header; any `<script src>` tags, fetch() URLs, or `/api/` path references in the 150KB body
verify_steps: GET https://ap-eupf.api.basf.com/ with Accept: text/html — extract Content-Type and inspect response body for route references
impact: Function name disclosure enabling targeted SSRF or logic testing; MEDIUM
testability: PASSIVE
[HYP] Admin function-level keys endpoint on ap-digitalconnect via v4 system key path
class: AUTH
asset: ap-digitalconnect.api.basf.com
confidence: 32
reasoning: 8 standard admin paths tested (401/404), but Azure Functions v4 introduced `/admin/host/functionkeys` and `/admin/system` endpoints that may not be gated by the same middleware as `/admin/host/keys`. Very low probability because Front Door + WAF likely apply uniform routing rules across all admin paths.
evidence_needed: HTTP 200 with function key JSON
verify_steps: GET https://ap-digitalconnect.api.basf.com/admin/host/functionkeys, GET https://ap-digitalconnect.api.basf.com/admin/system
impact: Function-level key disclosure; MEDIUM
testability: PASSIVE
[PARKED] Admin function-level keys endpoint on ap-digitalconnect via v4 system key path: confidence 32 — below 40 threshold. 8 admin paths already tested uniformly return 401/404; Azure Front Door + WAF applies consistent routing; functionkeys/system endpoints are unlikely to bypass existing auth gate. Low ROI.
[FINAL] API gateway OpenAPI/Swagger docs or hidden API routes on e-gate — 68, MISCONFIG, e-gate.api.basf.com
[FINAL] Root 150KB page on ap-eupf contains API route map or function discovery endpoint — 48, MISCONFIG, ap-eupf.api.basf.com
[NEXT] PROBE: GET https://e-gate.api.basf.com/openapi.json && GET /swagger.json && GET /api-docs && GET /swagger/ui && GET /docs && GET /.well-known/openapi-schema (test 6 common API gateway documentation paths on untested host — ~1 rps, GET only)
[LEARN] ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface fully mapped — no unauthenticated path exists
[LEARN] ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; zero probes run against any sub-paths; API gateway naming convention warrants documentation path enumeration
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/systemkeys: returns 404; system keys endpoint not exposed
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/functions: returns 404; admin function list not exposed
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/status: returns 404; host status endpoint not exposed
[LEARN] ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
[LEARN] REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
[LEARN] MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names
[RISK] BASF SE: 42 — Three production surfaces: ap-digitalconnect admin fully mapped (401/404 on all 8 standard paths), ap-eupf WAF-blocked SSRF + unanalyzed 150KB root, e-gate.api.basf.com completely untested. If API docs are exposed on e-gate, attack surface jumps significantly. If not, program is near-exhausted for unauthenticated testing. Residual risk requires authenticated access (Function App keys, client certs, or CI/CD artifact leaks via passive repo search) for any finding. Low overall unauth exposure.
## 2026-09-04 18:30:36 UTC [target] (model bigpickle)
[PRIO] ap-eupf.api.basf.com,6.9,attack_surface=5,business_value=8,tech_exposure=6,gate_ease=5,cloud_surface=8,freshness=8
[PRIO] e-gate.api.basf.com,4.8,attack_surface=6,business_value=8,tech_exposure=5,gate_ease=1,cloud_surface=3,freshness=10
[PRIO] ap-digitalconnect.api.basf.com,3.5,attack_surface=3,business_value=9,tech_exposure=3,gate_ease=1,cloud_surface=8,freshness=4
[HYP] API route/function discovery in 150KB root response body on ap-eupf
class: MISCONFIG
asset: ap-eupf.api.basf.com
confidence: 62
reasoning: Root returns 200 with 150093 bytes — far too large for default Azure Functions placeholder (<5KB). Likely custom SPA or API dashboard. Response body may contain JavaScript with route definitions, fetch() calls to /api/<function> paths, or runtime config. All /api/<function> probes returned 404 — function names may be discoverable only via JS bundle analysis rather than path brute.
evidence_needed: Response Content-Type header; any <script src> tags, fetch() URLs, route definitions, or /api/ path references in the 150KB body
verify_steps: GET https://ap-eupf.api.basf.com/ — inspect Content-Type, extract script tags and any /api/ or function-name references from response body
impact: Function name disclosure enabling targeted SSRF, IDOR, or logic testing; MEDIUM
testability: PASSIVE
[HYP] e-gate.api.basf.com mTLS-gated host with SSL certificate mismatch
class: MISCONFIG
asset: e-gate.api.basf.com
confidence: 55
reasoning: All 6 API doc probes returned SSL CERTIFICATE_VERIFY_FAILED errors (not HTTP 404/403). Same pattern as dev-clientcert-sap and dev-ext001 which require client certificates. The host may present a self-signed/internal CA certificate or require mutual TLS. If the SSL error is due to self-signed cert rather than mTLS, bypassing certificate validation could reveal the API surface. Business-critical gateway for procurement/supplier/logistics.
evidence_needed: SSL certificate details (issuer, subject, SAN); whether error is self-signed cert vs client cert requirement
verify_steps: curl -k -v https://e-gate.api.basf.com/ 2>&1 | grep -E "subject:|issuer:|SSL error|certificate" (test with -k flag to bypass cert verification)
impact: If self-signed cert (not mTLS), bypassing reveals full API gateway surface; HIGH
testability: PASSIVE
[HYP] ap-digitalconnect admin surface exhaustion — program near-exhausted for unauth testing
class: RECON
asset: ap-digitalconnect.api.basf.com
confidence: 95
reasoning: All 8 standard Azure Functions admin paths tested (401 on auth-gated, 404 on non-existent). functionkeys, system, status, systemkeys, functions all confirmed 404. No unauthenticated path exists. Front Door + WAF applies uniform routing. Only residual risk is leaked host key via CI/CD or passive repo search.
evidence_needed: None — surface fully mapped
verify_steps: None — exhausted
impact: None unauthenticated; LOW
testability: REJECTED
[PARKED] ap-digitalconnect admin surface exhaustion — REJECTED class (RECON only); no actionable probe; surface fully mapped
[FINAL] API route/function discovery in 150KB root response body on ap-eupf — 62, MISCONFIG
[FINAL] e-gate.api.basf.com mTLS-gated host with SSL certificate mismatch — 55, MISCONFIG
[NEXT] PROBE: GET https://ap-eupf.api.basf.com/ — save response body, extract Content-Type, <script> tags, fetch() URLs, and any /api/ or function-name references. THEN: curl -k -v https://e-gate.api.basf.com/ 2>&1 — extract SSL certificate details (issuer, subject, SAN) to determine if mTLS or self-signed cert
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/functionkeys: returns 404; v4 function-level key endpoint not exposed
[LEARN] REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/system: returns 404; admin system info endpoint not exposed
[LEARN] ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface fully mapped — no unauthenticated path exists
[LEARN] MITIGATED SSRF @ e-gate.api.basf.com: all 6 API doc probes return SSL CERTIFICATE_VERIFY_FAILED; host likely mTLS-gated like dev endpoints; SSL error blocks all unauthenticated HTTP probing
[LEARN] ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; SSL errors on all sub-paths; mTLS or self-signed cert required; surface inaccessible without client cert
[RISK] BASF SE: 38 — Program near-exhausted for unauthenticated testing. ap-digitalconnect fully mapped (401/404 on all 8 admin paths). e-gate appears mTLS-gated (SSL errors on all probes). Only actionable lead is ap-eupf 150KB root analysis — if function names are discoverable, attack surface jumps. If not, residual risk requires authenticated access (leaked Function App keys via CI/CD artifacts, client certs, or passive GitHub/secret scanning). Low overall unauth exposure.
## 2026-09-04 21:03:10 UTC [target] (model bigpickle)
## 2026-09-04 23:08:18 UTC [target] (model bigpickle)
[HYP] ap-eupf unlisted function names recoverable via passive artifact/code search
class: OTHER
asset: ap-eupf.api.basf.com
confidence: 45
reasoning: Fact — all /api/<common-name> probes 404, root+placeholder bodies contain zero function names, runtime status 401 (key-gated), SSRF params edge-blocked. The only unknown on this live Function App is the real function-name set; Microsoft-HTTP-triggered write/IDOR surfaces are unreachable without it. Name enumeration is verifiable, unlike secret leak hunting previously rejected as un-verifiable.
evidence_needed: any GitHub code/commit, CT log, or public Azure artifact mentioning ap-eupf function names or Azure resource/consumer-group names
verify_steps: passive — GitHub code search + secret-scanning for "ap-eupf.api.basf.com", "ap-digitalconnect.api.basf.com", correlate resource-name patterns; if a name is recovered, single read-only GET /api/<name> — 403/200 = exists, 404 = dead
impact: MEDIUM only if a function is recovered (targeted SSRF/IDOR/business-logic on the single live unauth-reachable app)
testability: PASSIVE
[NEXT] RAG: passive GitHub-code/commit + secret-scanning + CT-log search for "ap-eupf.api.basf.com", "ap-digitalconnect.api.basf.com", "basf digitalconnect" to recover ap-eupf Azure function names/resource names; on any hit, single read-only GET /api/<name> to confirm
[RISK] BASF SE: 30 — Unauthenticated surface effectively exhausted: both Function Apps are stock-placeholder + fully auth-gated with no reachable functions, e-gate is a Windows HTTPAPI host 404 on everything (plus revoked/expired-trust cert — operational, out-of-scope), dev hosts require client certs. Only residual path = passive artifact search for ap-eupf function names (low probability) or obtaining authenticated access (host/function keys, client cert, CI/CD leak). Overall unauth exposure low.
## 2026-09-05 01:11:06 UTC [target] (model bigpickle)
[CHANGED] e-gate.api.basf.com: TLS handshake completes with NO client cert (`-k`) — NOT mTLS; presented cert CN=e-gate.api.basf.com / O=BASF Digital Solutions GmbH, issuer=DigiCert Global G2 TLS RSA SHA256 2020 CA1; prior CERTIFICATE_VERIFY_FAILED = client-side chain error (intermediate not served, out-of-scope TLS); root + 6/6 doc paths return HTTP 404 body=315 under `-k`
[CHANGED] ap-eupf.api.basf.com/: HTTP 200 len=150093 Content-Type=text/html — body analyzed = stock Azure Functions 3.0 placeholder (azureLogo, jQuery via ajax.aspnetcdn.com, go.microsoft.com links); zero `/api/` refs, zero function names → 150KB route-discovery hypothesis conclusively closed
[CHANGED] ap-eupf + ap-digitalconnect `/.auth/config` + `/.auth/me`: HTTP 404 size=0 → EasyAuth not exposed on either Function App (last untested App Service sub-surface, now closed)
[CHANGED] RAG: passive web search for both hostnames + key artifacts → zero public repo/commit references; only indexed placeholder pages and generic Azure Functions docs; no leaked keys or function names recoverable
[NEW] dev.api.basf.com / dev-m.api.basf.com / dev-sap.api.basf.com: openapi.json / swagger.json / api-docs all HTTP 404 (body 168–197) → catch-all 404 hosts, no gateway/docs surface
[PRIO] my.basf.com,7.0,attack_surface=6,business_value=9,tech_exposure=7,gate_ease=6,cloud_surface=3,freshness=10
[PRIO] www.basf.com,6.9,attack_surface=7,business_value=9,tech_exposure=6,gate_ease=5,cloud_surface=2,freshness=10
[PRIO] ap-eupf.api.basf.com,4.2,attack_surface=2,business_value=8,tech_exposure=2,gate_ease=3,cloud_surface=8,freshness=1
[PRIO] ap-digitalconnect.api.basf.com,4.1,attack_surface=2,business_value=9,tech_exposure=2,gate_ease=1,cloud_surface=8,freshness=1
[PRIO] dev-clientcert-sap.api.basf.com,3.8,attack_surface=3,business_value=7,tech_exposure=3,gate_ease=1,cloud_surface=6,freshness=1
[PRIO] e-gate.api.basf.com,3.5,attack_surface=2,business_value=8,tech_exposure=2,gate_ease=2,cloud_surface=3,freshness=1
[HYP] BASF portal/web estate (my.basf.com, www.basf.com) unprobed while in program scope
class: OTHER
asset: my.basf.com / www.basf.com
confidence: 50
reasoning: Recorded facts — deep enum mapped the full `basf.com` zone to 9 dedicated hosts; this iteration exhausts every one of them unauthenticated (2 Azure Function placeholders, admin 8/8 paths 401/404, `/.auth/*` 404, SSRF edge-blocked 403/404; e-gate 404 on all paths with validly-issued DigiCert cert, not mTLS; 3 mTLS dev hosts; 3 catch-all 404 hosts). Program scope is BASF Group-wide ("all company-owned infrastructure… customer and supplier portals, websites"). Seed inventory lists my.basf.com and www.basf.com as live, dedicated, but ZERO probes have ever run against them. Basin of remaining HIGH-VALUE classes (portal OAuth redirect_uri/state, session/ATO, IDOR/BOLA in supplier flows) sits entirely in that unprobed estate.
evidence_needed: live reachability and auth stack of both hosts (OAuth/OIDC/SAML discovery), presence of registrable supplier/customer portal entry
verify_steps: once scoped — GET https://www.basf.com/ and https://my.basf.com/ (read-only, 1 rps); then passive OIDC/SAML discovery at /.well-known/openid-configuration, /saml/metadata, /oauth/authorize for provider/config disclosure
impact: reopens HIGH-VALUE test surface (auth/business-logic/IDOR chains) currently invisible; MEDIUM-HIGH if either host is a portal
testability: PASSIVE
[PARKED] e-gate mTLS-gated host theory: disproven today — handshake completes without client cert; cert issued by DigiCert for exact hostname; all sub-paths 404; chain issue is out-of-scope TLS
[PARKED] ap-eupf route discovery in 150KB root: closed — body is stock Azure Functions 3.0 placeholder, no routes/script refs
[PARKED] ap-eupf function-name recovery via passive artifact search: performed today, 0 hits — hypothesis unverifiable and exhausted
[PARKED] Function App key leak via private CI/CD: no passive verifiability; consistent with prior REJECTED master-key-hunt
[FINAL] BASF portal/web estate unprobed — 50, OTHER, my.basf.com/www.basf.com (only survivor)
[NEXT] HUMAN: *.api.basf.com unauth surface fully consumed (2 placeholder Function Apps auth-gated + `/.auth/*` closed, 3 mTLS dev hosts, 3 catch-all 404 hosts, e-gate 404 everywhere with non-mTLS DigiCert cert) — request program operator: (a) scope confirmation + read-only green-light for `GET https://my.basf.com/` and `GET https://www.basf.com/` (highest remaining value, currently seed-listed only), and/or (b) an authenticated test vector (client cert for e-gate/dev hosts, Function access key, or supplier/customer portal account). Further unauth probing of *.api.basf.com yields nothing.
[LEARN] REJECTED MISCONFIG @ e-gate.api.basf.com: not mTLS — handshake succeeds without client cert; cert CN=e-gate.api.basf.com issued by DigiCert Global G2 TLS RSA SHA256 2020 CA1 (O=BASF Digital Solutions GmbH); verify failure was client-side missing intermediate = out-of-scope TLS; 6/6 doc paths + root 404 under -k
[LEARN] REJECTED MISCONFIG @ ap-eupf.api.basf.com: 150093-byte root is stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com) — no `/api/`, fetch(), or function-name refs; no route discovery possible from body
[LEARN] REJECTED MISCONFIG @ ap-eupf.api.basf.com + ap-digitalconnect.api.basf.com: /.auth/config and /.auth/me return 404 — App Service EasyAuth not exposed on either Function App
[LEARN] REJECTED OTHER @ ap-eupf.api.basf.com: passive artifact/code search for function names and keys returned zero public hits — unverifiable, exhausted
[LEARN] ACCEPTED RECON @ dev.api.basf.com + dev-m.api.basf.com + dev-sap.api.basf.com: openapi.json/swagger.json/api-docs all 404 — catch-all hosts, no gateway/docs surface
[LEARN] ACCEPTED RECON @ *.api.basf.com estate: full 9-host unauth surface now mapped end-to-end (2 placeholders auth-gated, 3 mTLS 400, 3 catch-all 404, e-gate 404) — no reachable endpoint, function, key, or config beyond auth gates/404
[RISK] BASF SE: 15 — Unauthenticated exposure on the mapped estate is effectively zero: both Function Apps are stock placeholders fully auth-gated (8/8 admin paths 401/404, /.auth/* 404, SSRF blocked at edge, no reachable /api/<func>), dev hosts require client certs, e-gate and dev-* 404 everywhere, and passive search found no leaked keys/resource names. Verified-finding probability under current unauth constraints is nil; residual risk (managed-identity key theft, portal ATO/IDOR, supplier-flow logic) is only reachable via authenticated vectors (client certs, Function access keys, portal accounts) or by extending testing to the unprobed but in-scope my.basf.com/www.basf.com web estate.
## 2026-09-05 05:58:29 UTC [target] (model bigpickle)
[HYP] prod.api.basf.com additional unauthenticated/public-key Apigee proxies beyond /productinformation
class: MISCONFIG
asset: prod.api.basf.com
confidence: 55
reasoning: Recorded — gateway 404s (ApplicationNotFound) on 8/9 probes but `/productinformation` returns 401 VerifyAPIKey, proving path-routed proxy deployment on one virtual host (`BASF_secure`); the SPA ships 3 static browser keys (core/csp/navigator prefixes) implying key-only VerifyAPIKey proxies exist somewhere in estate; 401-vs-404 status split is a reliable live-vs-dead proxy oracle
evidence_needed: any probe path returning unique non-404 (401/200) indicating a deployed proxy; then whether a shipped browser key is accepted and what resource class it gates
verify_steps: passive→read-only GET sweep of ~25 probable proxy base paths (`/products,/catalog,/search,/user,/order,/cart,/price,/availability,/documents,/mss,/dus,/copilot,/v1/productinformation,/v2/*…`) at 1 rps, noting status split; on any 401/200, retry with each of the 3 discovered x-api-keys; any 200 w/o user token = key-only read access finding
impact: if a browser-published key opens a key-only proxy → unauthenticated product/commerce data access, IDOR surface on catalog/order/price data; MEDIUM-HIGH
testability: PASSIVE_GET (read-only, live)
[HYP] api.commerce.basf.com AWS API Gateway stage-prefix switching reaches handler behind MissingAuthenticationToken
class: AUTH
asset: api.commerce.basf.com
confidence: 45
reasoning: Recorded — `/copilot` returns 403 `MissingAuthenticationToken` (x-amzn-errortype) = REST API Gateway with staged routes; absent stage prefix makes every path 403; config pairs navigator key `ahWV…` with baseUrl `…/copilot`, so a matching stage + gateway route is expected to accept it
evidence_needed: a live stage-prefix combination returning non-403/non-400
verify_steps: read-only GET `/v1/copilot`, `/prod/copilot`, `/dev/copilot`, `/staging/copilot`, `/copilot/v1`, `/api/copilot` with navigator key header at 1 rps; mark first non-403 response
impact: reachable commerce copilot/handler via public browser key → LLM/business-data surface, possible prompt/IDOR access; MEDIUM if only metadata, HIGH if data
testability: PASSIVE_GET
[HYP] federation.basf.com NAM OIDC client uses plain-PKCE or non-PKCE code flow making authz-code replay feasible for client `86cc4bf9-…`
class: OATH
asset: federation.basf.com
confidence: 40
reasoning: Recorded — discovery lists `code_challenge_methods_supported: plain, S256` (issuer allows deprecated plain PKCE) and token auth `client_secret_post/basic` (no `none`); SPA config: responseType=code, useRefreshToken=true, refresh_token scope, SPA bundle contains zero PKCE/challenge references; public client with refresh_token = long-lived bearer
evidence_needed: observed code exchange/PKCE params in an authentic login flow; refresh-token lifetime/rotation
verify_steps: AUTH_HELPED — requires interactive login to capture authz request params and token response; passive parts exhausted
impact: code interception/refresh-token replay → full portal account ATO on the highest-value BASF customer/supplier commerce platform; HIGH
testability: AUTH_HELPED
[NEXT] PROBE: read-only GET sweep of ~25 probable proxy base-paths on https://prod.api.basf.com (products, catalog, search, user, order, cart, price, availability, documents, copilot, mss, dus, v2/*) at 1 rps logging only status code; on first non-404 (401/200), apply the 3 discovered browser keys via `x-api-key` and record result — a 200 key-only hit is the finding
[RISK] BASF SE: 45 — The prior `*.api.basf.com` "estate exhausted" conclusion was incomplete: the portal boot config exposed 7 production backend hosts the estate map never covered, including a production Apigee gateway with a live x-api-key proxy and an AWS API Gateway commerce tier, plus the corporate NetIQ identity provider. Defenses observed are solid (strict redirect_uri, key-gated gateway, mTLS-capable backends) and no unauth data access is demonstrated yet, but the proxy/stage enumeration surface is large and key-scoping of browser-shipped API keys is the single most probable real finding; any key-only proxy with a mis-scoped browser key yields HIGH impact data access on BASF's customer/supplier commerce platform.
## 2026-09-05 10:30:19 UTC [target] (model bigpickle)
## 2026-09-05 13:33:24 UTC [target] (model bigpickle)
[NEW] `prod.api.basf.com` — HTTP 404 at root; `/productinformation` returns 401 VerifyAPIKey (key-gated Apigee proxy confirmed on one virtual host); 3 browser keys shipped in SPA bundle; zero proxy path enumeration performed
[NEW] `api.commerce.basf.com` — 403 `MissingAuthenticationToken` (x-amzn-errortype) = AWS REST API Gateway with staged routes; navigator key paired with `/copilot` baseUrl; zero stage-prefix enumeration performed
[NEW] `federation.basf.com` — NAM OIDC provider; discovery lists `code_challenge_methods_supported: plain, S256`, `client_secret_post/basic`; SPA uses responseType=code + useRefreshToken=true + zero PKCE references; zero auth-flow probes
[NEW] `my.basf.com` — HTTP 200, 204KB content; `.well-known/openid-configuration` returns 404; zero deep auth-stack enumeration beyond root
[NEW] `www.basf.com` — HTTP 200, 640KB content; zero probes beyond root reachability
[CHANGED] `api.basf.com` — all probes return `Connection refused` (Errno 111); not publicly reachable; dead or internal-only DNS entry
[PRIO] my.basf.com,7.0,attack_surface=6,business_value=9,tech_exposure=7,gate_ease=6,cloud_surface=3,freshness=10
[PRIO] www.basf.com,6.9,attack_surface=7,business_value=9,tech_exposure=6,gate_ease=5,cloud_surface=2,freshness=10
[PRIO] prod.api.basf.com,6.4,attack_surface=7,business_value=9,tech_exposure=7,gate_ease=5,cloud_surface=4,freshness=10
[PRIO] federation.basf.com,5.8,attack_surface=5,business_value=8,tech_exposure=8,gate_ease=3,cloud_surface=5,freshness=10
[PRIO] api.commerce.basf.com,5.2,attack_surface=5,business_value=8,tech_exposure=6,gate_ease=2,cloud_surface=6,freshness=10
[HYP] prod.api.basf.com additional unauthenticated/public-key Apigee proxies beyond /productinformation
class: MISCONFIG
asset: prod.api.basf.com
confidence: 55
reasoning: Gateway 404s (ApplicationNotFound) on 8/9 probes but `/productinformation` returns 401 VerifyAPIKey, proving path-routed proxy deployment on one virtual host (`BASF_secure`); the SPA ships 3 static browser keys (core/csp/navigator prefixes) implying key-only VerifyAPIKey proxies exist somewhere in estate; 401-vs-404 status split is a reliable live-vs-dead proxy oracle
evidence_needed: any probe path returning unique non-404 (401/200) indicating a deployed proxy; then whether a shipped browser key is accepted and what resource class it gates
verify_steps: passive→read-only GET sweep of ~25 probable proxy base paths (`/products,/catalog,/search,/user,/order,/cart,/price,/availability,/documents,/mss,/dus,/copilot,/v1/productinformation,/v2/*…`) at 1 rps, noting status split; on any 401/200, retry with each of the 3 discovered x-api-keys; any 200 w/o user token = key-only read access finding
impact: if a browser-published key opens a key-only proxy → unauthenticated product/commerce data access, IDOR surface on catalog/order/price data; MEDIUM-HIGH
testability: PASSIVE_GET
[HYP] my.basf.com portal auth-stack discovery revealing OAuth redirect_uri/state flaws or ATO surface
class: OATH
asset: my.basf.com
confidence: 52
reasoning: HTTP 200 with 204KB content confirmed; `.well-known/openid-configuration` returns 404 — auth stack not at standard OIDC discovery path; portal likely uses NetIQ/ADFS/SAML (BASF's NAM OIDC found at federation.basf.com); SPA bundle (204KB) may contain OAuth client_id, redirect_uri, or auth endpoint URLs in JavaScript; customer/supplier portal is highest business value in scope; zero auth-flow probes performed beyond OIDC discovery 404
evidence_needed: auth endpoint URLs (OAuth authorize, SAML SSO, ADFS metadata), client_id/redirect_uri pairs in SPA JS, session cookie names/flags indicating session management quality
verify_steps: GET https://my.basf.com/ -I (inspect all response headers: Location, Set-Cookie, x-ms-* for Azure AD tenant, X-Powered-By); grep response body for `client_id`, `redirect_uri`, `authorize`, `saml`, `oauth`, `adfs`, `login`; GET https://my.basf.com/saml/metadata; GET https://my.basf.com/adfs/.well-known/openid-configuration; GET https://my.basf.com/oauth2/authorize?response_type=code&client_id=test&redirect_uri=https://evil.com
impact: OAuth redirect_uri validation bypass or state parameter flaw → full portal account ATO on highest-value BASF customer/supplier platform; HIGH
testability: PASSIVE_GET
[HYP] api.commerce.basf.com AWS API Gateway stage-prefix switching reaches handler behind MissingAuthenticationToken
class: AUTH
asset: api.commerce.basf.com
confidence: 45
reasoning: `/copilot` returns 403 `MissingAuthenticationToken` (x-amzn-errortype) = REST API Gateway with staged routes; absent stage prefix makes every path 403; SPA config pairs navigator key `ahWV…` with baseUrl `…/copilot`, so a matching stage + gateway route is expected to accept it
evidence_needed: a live stage-prefix combination returning non-403/non-400
verify_steps: read-only GET `/v1/copilot`, `/prod/copilot`, `/dev/copilot`, `/staging/copilot`, `/copilot/v1`, `/api/copilot` with navigator key header at 1 rps; mark first non-403 response
impact: reachable commerce copilot/handler via public browser key → LLM/business-data surface, possible prompt/IDOR access; MEDIUM if only metadata, HIGH if data
testability: PASSIVE_GET
[FINAL] prod.api.basf.com additional unauthenticated/public-key Apigee proxies beyond /productinformation — 55, MISCONFIG, prod.api.basf.com
[FINAL] my.basf.com portal auth-stack discovery revealing OAuth redirect_uri/state flaws or ATO surface — 52, OATH, my.basf.com
[FINAL] api.commerce.basf.com AWS API Gateway stage-prefix switching reaches handler behind MissingAuthenticationToken — 45, AUTH, api.commerce.basf.com
[PARKED] federation.basf.com plain-PKCE OIDC code-flow exploit: confidence 40 — below actionable threshold; requires interactive login to capture authz params; passive discovery already completed (code_challenge_methods_supported: plain, S256); refresh_token scope on public client = high impact if confirmed but no passive verify step exists
[PARKED] www.basf.com subdomain takeover via dangling CNAME: confidence 30 — 640KB live page suggests active hosting; no evidence of dangling records; low probability on primary corporate domain
[NEXT] PROBE: read-only GET sweep of ~25 probable proxy base-paths on https://prod.api.basf.com (products, catalog, search, user, order, cart, price, availability, documents, copilot, mss, dus, v2/*) at 1 rps logging only status code; on first non-404 (401/200), apply the 3 discovered browser keys via `x-api-key` header and record result — a 200 key-only hit is the finding
[LEARN] ACCEPTED RECON @ prod.api.basf.com: `/productinformation` returns 401 VerifyAPIKey; path-routed Apigee proxy confirmed; 401-vs-404 oracle operational
[LEARN] ACCEPTED RECON @ api.commerce.basf.com: `/copilot` returns 403 MissingAuthenticationToken; AWS REST API Gateway confirmed; staged routes exist
[LEARN] ACCEPTED RECON @ federation.basf.com: OIDC discovery at `.well-known/openid-configuration` lists plain+S256 PKCE; SPA uses code+refresh tokens with zero PKCE references; public client with refresh_token = long-lived bearer
[LEARN] ACCEPTED RECON @ my.basf.com: HTTP 200, 204KB; `.well-known/openid-configuration` 404; portal auth stack not at standard OIDC discovery path; deep enum required
[LEARN] ACCEPTED RECON @ api.basf.com: Connection refused on all probes; not publicly reachable; dead or internal-only DNS entry
[LEARN] CHANGED RECON @ my.basf.com: `.well-known/openid-configuration` → HTTP 404 (was previously unprobed); auth stack not at standard OIDC discovery path
[RISK] BASF SE: 45 — The prior `*.api.basf.com` "estate exhausted" conclusion was incomplete: the portal boot config exposed 7 production backend hosts the estate map never covered, including a production Apigee gateway with a live x-api-key proxy and an AWS API Gateway commerce tier, plus the corporate NetIQ identity provider. Defenses observed are solid (strict redirect_uri, key-gated gateway, mTLS-capable backends) and no unauth data access is demonstrated yet, but the proxy/stage enumeration surface is large and key-scoping of browser-shipped API keys is the single most probable real finding; any key-only proxy with a mis-scoped browser key yields HIGH impact data access on BASF's customer/supplier commerce platform.
## 2026-09-05 16:16:28 UTC [target] (model bigpickle)
## 2026-09-05 18:35:20 UTC [target] (model bigpickle)
[PRIO] my.basf.com,70,attack_surface=6,business_value=9,tech_exposure=7,gate_ease=6,cloud_surface=3,freshness=10
[PRIO] prod.api.basf.com,64,attack_surface=7,business_value=9,tech_exposure=7,gate_ease=5,cloud_surface=4,freshness=10
[PRIO] api.commerce.basf.com,52,attack_surface=5,business_value=8,tech_exposure=6,gate_ease=2,cloud_surface=6,freshness=10
[HYP] my.basf.com authentication stack enumeration for OAuth/OIDC misconfiguration
class: OATH
asset: my.basf.com
confidence: 52
reasoning: HTTP 200 with 204KB content confirmed; `.well-known/openid-configuration` returns 404; portal likely uses NetIQ/ADFS/SAML (BASF's NAM OIDC found at federation.basf.com); SPA bundle (204KB) may contain OAuth client_id, redirect_uri, or auth endpoint URLs in JavaScript; customer/supplier portal is highest business value in scope; zero auth-flow probes performed beyond OIDC discovery 404
evidence_needed: auth endpoint URLs (OAuth authorize, SAML SSO, ADFS metadata), client_id/redirect_uri pairs in SPA JS, session cookie names/flags indicating session management quality
verify_steps: GET https://my.basf.com/ -I (inspect all response headers: Location, Set-Cookie, x-ms-* for Azure AD tenant, X-Powered-By); grep response body for `client_id`, `redirect_uri`, `authorize`, `saml`, `oauth`, `adfs`, `login`; GET https://my.basf.com/saml/metadata; GET https://my.basf.com/adfs/.well-known/openid-configuration; GET https://my.basf.com/oauth2/authorize?response_type=code&client_id=test&redirect_uri=https://evil.com
impact: OAuth redirect_uri validation bypass or state parameter flaw → full portal account ATO on highest-value BASF customer/supplier platform; HIGH
testability: PASSIVE_GET
[HYP] prod.api.basf.com additional unauthenticated/public-key Apigee proxies beyond /productinformation
class: MISCONFIG
asset: prod.api.basf.com
confidence: 55
reasoning: Gateway 404s (ApplicationNotFound) on 8/9 probes but `/productinformation` returns 401 VerifyAPIKey, proving path-routed proxy deployment on one virtual host (`BASF_secure`); the SPA ships 3 static browser keys (core/csp/navigator prefixes) implying key-only VerifyAPIKey proxies exist somewhere in estate; 401-vs-404 status split is a reliable live-vs-dead proxy oracle
evidence_needed: any probe path returning unique non-404 (401/200) indicating a deployed proxy; then whether a shipped browser key is accepted and what resource class it gates
verify_steps: passive→read-only GET sweep of ~25 probable proxy base paths (`/products,/catalog,/search,/user,/order,/cart,/price,/availability,/documents,/mss,/dus,/copilot,/v1/productinformation,/v2/*…`) at 1 rps, noting status split; on any 401/200, retry with each of the 3 discovered x-api-keys; any 200 w/o user token = key-only read access finding
impact: if a browser-published key opens a key-only proxy → unauthenticated product/commerce data access, IDOR surface on catalog/order/price data; MEDIUM-HIGH
testability: PASSIVE_GET
[HYP] api.commerce.basf.com AWS API Gateway stage-prefix switching reaches handler behind MissingAuthenticationToken
class: AUTH
asset: api.commerce.basf.com
confidence: 45
reasoning: `/copilot` returns 403 `MissingAuthenticationToken` (x-amzn-errortype) = REST API Gateway with staged routes; absent stage prefix makes every path 403; SPA config pairs navigator key `ahWV…` with baseUrl `…/copilot`, so a matching stage + gateway route is expected to accept it
evidence_needed: a live stage-prefix combination returning non-403/non-400
verify_steps: read-only GET `/v1/copilot`, `/prod/copilot`, `/dev/copilot`, `/staging/copilot`, `/copilot/v1`, `/api/copilot` with navigator key header at 1 rps; mark first non-403 response
impact: reachable commerce copilot/handler via public browser key → LLM/business-data surface, possible prompt/IDOR access; MEDIUM if only metadata, HIGH if data
testability: PASSIVE_GET
[PARKED] federation.basf.com plain-PKCE OIDC code-flow exploit: confidence 40 — below actionable threshold; requires interactive login to capture authz params; passive discovery already completed (code_challenge_methods_supported: plain, S256); refresh_token scope on public client = high impact if confirmed but no passive verify step exists
[PARKED] www.basf.com subdomain takeover via dangling CNAME: confidence 30 — 640KB live page suggests active hosting; no evidence of dangling records; low probability on primary corporate domain
[FINAL] my.basf.com — 52, OATH
[FINAL] prod.api.basf.com — 55, MISCONFIG
[FINAL] api.commerce.basf.com — 45, AUTH
[NEXT] PROBE: read-only GET sweep of ~25 probable proxy base-paths on https://prod.api.basf.com (products, catalog, search, user, order, cart, price, availability, documents, copilot, mss, dus, v2/*) at 1 rps logging only status code; on first non-404 (401/200), apply the 3 discovered browser keys via `x-api-key` header and record result — a 200 key-only hit is the finding
[PRIO] my.basf.com,7.0,attack_surface=6,business_value=9,tech_exposure=7,gate_ease=6,cloud_surface=3,freshness=10
[PRIO] www.basf.com,6.9,attack_surface=7,business_value=9,tech_exposure=4,gate_ease=6,cloud_surface=2,freshness=10
[PRIO] federation.basf.com,5.8,attack_surface=5,business_value=8,tech_exposure=8,gate_ease=3,cloud_surface=5,freshness=10
[HYP] my.basf.com portal auth-stack discovery revealing OAuth client config / redirect_uri or state flaw → ATO
class: OATH
asset: my.basf.com
confidence: 52
reasoning: Live 204KB Magnolia+Angular commerce SPA; `.well-known/openid-configuration` 404 — auth stack off-standard path; SSR boot config discloses OIDC client `86cc4bf9-…`, responseType=code, useRefreshToken=true, zero PKCE refs; the remaining in-scope HIGH-VALUE surface with no probes past root
evidence_needed: client_id/redirect_uri/authorize-endpoint strings in SPA body; alternative OIDC discovery JSON; SAML/ADFS metadata
verify_steps: GET https://my.basf.com/ -I (headers); grep saved body for `client_id`,`redirect_uri`,`authorize`,`oauth`,`openid-configuration`,`saml`,`adfs`,`token`; GET /auth/./well-known/openid-configuration, /basf/./well-known/openid-configuration, /nidp/./well-known/openid-configuration, /saml/metadata at 1 rps
impact: redirected/auth-code or state flaw → ATO on BASF customer/supplier portal; HIGH
testability: PASSIVE_GET
[HYP] www.basf.com hosts login/portal sub-surfaces exposing OAuth redirects or supplier-registration flows
class: OATH
asset: www.basf.com
confidence: 48
reasoning: Live 308→/us/en via CloudFront, 640KB corporate estate, SPA config pointed to my.basf.com portal integration; corporate site commonly embeds partner/supplier login links (BASF WorldAccount, Connect) that route to a central IdP; any embedded authz endpoint URL is a passive discovery win for redirect_uri/orchestration testing
evidence_needed: login/partner-portal URLs, SSO links, embedded OAuth authorize endpoints in the 640KB body
verify_steps: GET https://www.basf.com/us/en each of top-3 country roots, grep for `login`, `my.basf`, `WorldAccount`, `sso`, `oauth`, `signin`, `/auth/`; resolve found auth endpoints read-only
impact: discovers additional IdP-facing OAuth surfaces for redirect_uri/state testing; MEDIUM (roadmap to HIGH)
testability: PASSIVE_GET
[PARKED] prod.api.basf.com additional proxies: dead — 66 paths 404, only `/productinformation` live, all 4 browser keys rejected (Invalid ApiKey); key-scope exhausted
[PARKED] api.commerce.basf.com stage-switch: dead — MissingAuthenticationToken on every prefix; route is IAM-gated, x-api-key is not the credential class, no passive bypass
[PARKED] api.productinformation key-scope: dead — ForbiddenException with pi+core keys; Lambda-authorizer deny, needs IAM/AWS creds
[PARKED] federation.basf.com plain-PKCE: confidence 40 < threshold; requires interactive login (AUTH_HELPED)
[FINAL] my.basf.com — 52, OATH
[FINAL] www.basf.com — 48, OATH
[NEXT] PROBE: GET https://my.basf.com/ -I (capture full headers), then grep saved 204KB SPA body for `client_id`,`redirect_uri`,`authorize`,`oauth`,`openid-configuration`,`saml`,`adfs`,`token`; then GET the 4 alternative discovery paths (`/auth/.well-known/openid-configuration`, `/basf/.well-known/openid-configuration`, `/nidp/.well-known/openid-configuration`, `/saml/metadata`) at 1 rps, logging status + body-type
[LEARN] REJECTED MISCONFIG @ prod.api.basf.com: 66 proxy-path probes all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected Invalid ApiKey — no additional proxy, key-scope exhausted
[LEARN] REJECTED AUTH @ api.commerce.basf.com: `/copilot` + 8 stage prefixes all 403 MissingAuthenticationTokenException with navigator key — route IAM/SigV4-gated, x-api-key not credential class, stage-switching irrelevant
[LEARN] REJECTED MISCONFIG @ api.productinformation.basf.com + api-imp.productinformation.basf.com: 403 ForbiddenException with pi+core keys — Lambda-authorizer/API-GW-policy deny regardless of key
[LEARN] ACCEPTED RECON @ api-imp.commerce.basf.com: HTTP 400 empty root gate (CloudFront→API GW ingest); no auth oracle; imp-* = import/processing tier
[RISK] BASF SE: 35 — AWS commerce/productinformation tier uniformly auth-gated (IAM MissingAuthenticationToken / Lambda-authorizer Forbidden), Apigee key-gated with all browser keys rejected; proxy/stage/key enumeration exhausted across all 6 SPA-disclosed backend hosts. Residual risk concentrates on my.basf.com portal auth-stack (off-standard discovery, NetIQ NAM OIDC allowing plain PKCE) and the federation.basf.com OIDC public client with refresh_token (interactive-only). Unauth surface thin; exploit value concentrates in portal OAuth/ATO chain.
[PRIO] my.basf.com,6.6,attack_surface=6,business_value=9,tech_exposure=7,gate_ease=6,cloud_surface=3,freshness=10
[PRIO] federation.basf.com,5.9,attack_surface=5,business_value=8,tech_exposure=8,gate_ease=3,cloud_surface=5,freshness=10
[PRIO] prod.api.basf.com,3.4,attack_surface=3,business_value=9,tech_exposure=5,gate_ease=2,cloud_surface=4,freshness=10
[HYP] DigitalCommercePlatform public OAuth client uses code+refresh_token flow with NO PKCE → authz-code interception / refresh-token replay
class: OATH
asset: federation.basf.com (client 86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4)
confidence: 42
reasoning: Recovered SSR config: responseType=code, scope includes refresh_token, redirectUrl my.basf.com/.auth, customParams only resourceServer+scope — no code_challenge machinery; IdP discovery allows S256 plugin; public client (no secret in bundle); redirect_uri exact-match mitigation verified strong, so exploitation needs code interception at another layer (malware/browser addon/compromised sub-origin), not redirect manipulation
evidence_needed: capture of an authentic authorize request proving absence of code_challenge + a token-response refresh_token with no rotation
verify_steps: AUTH_HELPED — with a test account, log in via https://federation.basf.com/nidp/oauth/nam/authz (client_id=86cc4bf9-…, redirect_uri=https://my.basf.com/.auth), record uttered params + refresh_token lifetime/rotation on reuse
impact: intercepted authz code or stolen refresh_token = full ATO of BASF customer/supplier portal account, order/price/PII access; HIGH
testability: AUTH_HELPED
[PARKED] prod.api.basf.com extra proxies / browser-key scope: dead — 66/66 paths 404 except productinformation; 4/4 keys Invalid ApiKey
[PARKED] api.commerce.basf.com stage-switch: dead — every prefix MissingAuthenticationToken; IAM-gated; x-api-key not credential class
[PARKED] api.productinformation.basf.com key-scope: dead — ForbiddenException; Lambda-authorizer deny
[PARKED] federation.basf.com redirect_uri/open-redirect chain: dead — exact-match oracle across 10 variants, no bypass
[PARKED] www/metrics products deep-hunt: dead — static marketing facade, no IdP entry, login client-side only
[FINAL] DigitalCommercePlatform OAuth public-client no-PKCE refresh_token — 42, OATH (only survivor; interactive proof required)
[NEXT] HUMAN: passive estate fully exercised — every backend key/IAM/authorizer-gated, redirect_uri oracle clean, proxy+stage enumeration exhausted across 11 hosts. Request from program operator: (a) a sandbox/supplier test account to run the interactive OIDC code-exchange for client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` and check whether the authorize request carries `code_challenge` and whether the returned `refresh_token` rotates on reuse (the one real high-severity candidate), and/or (b) confirmation to report. Report channel bugs.olivermaicher.eu.
[LEARN] REJECTED MISCONFIG @ prod.api.basf.com: 66 proxy paths all 404 except /productinformation (401); 4 browser keys Invalid ApiKey — no extra proxy, key scope exhausted
[LEARN] REJECTED AUTH @ api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — IAM-gated, x-api-key not credential class
[LEARN] REJECTED MISCONFIG @ api.productinformation.basf.com + api-imp.productinformation.basf.com: 403 ForbiddenException / 400 empty with pi+core keys — authorizer denies
[LEARN] REJECTED OATH @ federation.basf.com: redirect_uri oracle exact-match — @ userinfo, %2f%2f, CRLF, port, suffix, dot-segment all rejected; no bypass
[LEARN] ACCEPTED RECON @ my.basf.com: SSR boot config discloses full OAuth client (86cc4bf9-…, redirect /.auth, refresh_token scope, acr 3IAM/Login/External, code flow, no PKCE)
[LEARN] ACCEPTED RECON @ federation.basf.com: authorize endpoint live; legit full flow → 302 /nidp/app/login; error pages styled as BASF Authentication Service
[LEARN] ACCEPTED RECON @ products.basf.com: CloudFront Magnolia SPA (252KB), same WCMS stack, zero auth entry in HTML
[RISK] BASF SE: 30 — Unauthenticated backend exposure across the full 11-host estate now proven gated (Apigee VerifyAPIKey, AWS IAM/authorizer, NAM OIDC exact-match redirect). The only genuine high-severity weakness is the digital-commerce public OAuth client emitting refresh_token without PKCE — a design-level flaw whose exploitability requires interactive code capture, not currently demonstrable passively. Without an authenticated test vector, no further unauth progress is possible.
## 2026-09-05 20:48:44 UTC [target] (model bigpickle)
[HYP] DigitalCommercePlatform public OAuth client code+refresh_token with no PKCE → authz-code interception / refresh-token replay ATO
class: OATH
asset: federation.basf.com (client 86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4)
confidence: 45
reasoning: my.basf.com now proven server-side blind (4/4 catch-all SPA → all auth funnels through federation.basf.com); SSR config responseType=code, useRefreshToken=true, zero code_challenge refs; registered allowlist oracle re-confirms exact-match ({my.basf.com, my.basf.com/.auth, my-basf-world.basf.com root}); implicit disabled (response_type=token → unauthorized_client) so fragment-token width is closed; residual vector = code/refresh theft requiring interception layer, not redirect manipulation
evidence_needed: capture of a genuine authorize request proving absence of code_challenge + a refresh_token that does not rotate on reuse
verify_steps: AUTH_HELPED — with sandbox account: GET https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-…&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth, log uttered params (code_challenge/code_challenge_method), exchange at /nidp/oauth/nam/token, reuse refresh_token twice and compare JTI/lifetime
impact: intercepted authz code or stolen non-rotating refresh_token = full ATO of BASF customer/supplier portal account, order/price/PII access; HIGH
testability: AUTH_HELPED
[NEXT] HUMAN: all passive threads on the OAuth chain now exhausted (portal server-surface blind, redirect oracle exact-match, implicit disabled, allowlist enumerated, dead-host default identified). Request from program operator: (a) a sandbox/supplier test account to run the interactive code-exchange for client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` and record whether authorize carries `code_challenge` and whether `refresh_token` rotates on reuse — the single remaining high-severity candidate; and/or (b) scoped confirmation for the adult-resolving `my-basf-world.basf.com` entry (legacy WorldAccount OAuth redirect host, externally dead). Report channel bugs.olivermaicher.eu.
## 2026-09-05 22:47:01 UTC [target] (model bigpickle)
[HYP] NAM IdP exposes unprotected OIDC/SAML admin discovery or registration endpoints leaking client config / JWKS / supported flows
class: OATH
asset: federation.basf.com (NetIQ Access Manager provider)
confidence: 42
reasoning: provider is NAM (error page styling + `/nidp/app/login` reached); NAM exposes OAuth metadata under `/nidp/oauth/nam/discovery*` and SAML metadata under `/nidp/idp/...` in addition to browser-config `.well-known`; root `.well-known/openid-configuration` returns provider HTML 404 (off-standard path config) so the real discovery surface was never enumerated; only `authz` confirmed live
evidence_needed: any 200 JSON at discovery/registration/metadata paths, or a body embedding issuer/client-id/jwks/key-id
verify_steps: at 1 rps: GET /nidp/oauth/nam/discovery/token and /nidp/oauth/nam/discovery/authorize and /nidp/oauth/nam/discovery and /nidp/oauth/nam/clientRegistration and /nidp/idp/saml2/metadata and /nidp/saml2/metadata — log status, Content-Type, first 2KB; then GET /nidp/app/login (200 known) and grep body for `client_id`,`entityID`,`metadata`,`oauth`,`issuer`,`jwks`
impact: exposed metadata discloses issuer, PKCE mode policy, token/introspection/userinfo endpoints, supported signing/server-assertion — feeds alg-confusion/JWK-confusion and unregistered OAuth client testing; MEDIUM (→ HIGH if registration/keys readable); exploit-only-read, no customer data touched
testability: PASSIVE
[NEXT] PROBE: GET https://federation.basf.com/nidp/oauth/nam/discovery/token -k and /nidp/oauth/nam/discovery/authorize and /nidp/oauth/nam/discovery and /nidp/oauth/nam/clientRegistration and /nidp/idp/saml2/metadata and /nidp/saml2/metadata at 1 rps (log status + Content-Type + first 2KB), then GET /nidp/app/login and grep the saved body for `client_id`,`clientId`,`oauth`,`entityID`,`jwks`,`issuer`,`registration`
## 2026-09-06 00:50:51 UTC [target] (model bigpickle)
## 2026-09-06 05:50:39 UTC [target] (model bigpickle)
## 2026-09-06 10:50:38 UTC [target] (model bigpickle)
[CHANGED] federation.basf.com: NAM OIDC discovery at /nidp/oauth/nam/.well-known/openid-configuration discloses real registration_endpoint /nidp/oauth/nam/clients (401 auth-required — earlier 404 test hit wrong path), jwks_uri /nidp/oauth/nam/keys (200, 1x RS256 kid=1111010011111111, leaf openidsigning.federation.basf.com O=BASF Digital Solutions GmbH, issuer BASF-Federation-RootCA), introspect /nidp/oauth/v1/nam/introspect (405), revoke (405), userinfo (401 invalid_token), end_session (302 /nidp/app/logout)
[NEW] federation.basf.com/nidp/saml2/metadata -> 200 signed text/xml IdP+SP SAML2 descriptor (21434B): entityID, SSO POST/Redirect /nidp/saml2/sso, SLO /nidp/saml2/slo(+_return), SOAP /nidp/saml2/soap + spsoap, ACS /nidp/saml2/spassertion_consumer; signing cert BASF-Fed_Sign-Enc_SHA256_Default (root BASF-Fed_Sign-Enc_Root) + encryption cert BASF-Fed_Encryption (OU=G-FDI); all SAML endpoints benign on GET (error page / 302 /nidp / SOAP fault)
[NEW] federation.basf.com OIDC discovery content: grant_types incl password(ROPC)+hybrid, code_challenge_methods plain+S256, scopes urn:netiq.com:nam:scope:oauth:registration:full|read, claims incl '/UserAttribute[@ldap:targetAttribute="groupMembership"]' + basfOTPUsed, id_token alg RS256 only
[PARKED] federation.basf.com NAM ROPC/plain-PKCE/registration exploitation: surface fully gated (401/405/302); ROPC+plain-PKCE require a misconfigured REGISTERED client + creds for Password-grant (credential/brute-force = out-of-scope class); no passive verify step
[FINAL] DigitalCommercePlatform public OAuth client no-PKCE refresh_token — 75 (ranked), AUTH_HELPED, OATH: single surviving high-severity candidate; NAM IAM-surface mapping this session adds no unauth vector
[NEXT] HUMAN: NAM discovery-exposed surface now fully mapped & gated (registration 401, introspect/revoke 405, userinfo 401, keys public, SAML services benign). No unauth vector opened. Still request from program operator: sandbox/supplier account for interactive OIDC code-exchange on client 86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4 — record whether authorize carries code_challenge and whether refresh_token rotates on reuse (the [75] no-PKCE refresh_token ATO proof), or confirmation to report. Report channel bugs.olivermaicher.eu.
[LEARN] ACCEPTED RECON @ federation.basf.com: NAM OIDC/SAML/JWKS surface mapped end-to-end via discovery-advertised paths; all administrative endpoints auth-gated (clients 401, introspect/revoke 405, userinfo 401, end_session 302 logout) — no unauth config/registration/key hole
[LEARN] REJECTED OATH @ federation.basf.com: /nidp/oauth/nam/discovery/* + clientRegistration all 404 — wrong path guess; the NAM registration endpoint is /nidp/oauth/nam/clients (401)
[LEARN] ACCEPTED RECON @ federation.basf.com: discovery exposes provider config — password(ROPC)+hybrid grants, plain+S256 PKCE, registration:full/read scopes, LDAP groupMembership + basfOTPUsed claims; informational config/attr-name leak only, exploitation needs a misconfigured registered client (out-of-scope class)
[RISK] BASF SE: 30 — NAM identity plane re-verified fully gated after complete discovery+JWKS+SAML enumeration; deepest remaining exposure unchanged: my.basf.com/federation public OAuth client 86cc4bf9 emits refresh_token without PKCE (provider allows plain PKCE + ROPC provider-wide) — interactive proof still required; no new unauth surface opened anywhere in the 11-host estate
[HYP] DigitalCommercePlatform public OAuth client `86cc4bf9-…` emits refresh_token with no PKCE → authz-code/refresh-token replay ATO
class: OATH
asset: federation.basf.com / my.basf.com (client 86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4)
confidence: 75
reasoning: SSR boot config — response_type=code, useRefreshToken=true, refresh_token in scope, zero code_challenge refs; NAM discovery confirms provider allows code_challenge S256 AND plain, and newly confirms ROPC `password`/`hybrid` grants provider-wide; public client (no secret in SPA); redirect_uri exact-match oracle verified clean (10 variants, no bypass), implicit closed (token→unauthorized_client); residual theft plane is code/refresh interception, not redirect manipulation
evidence_needed: authentic authorize request showing absence of code_challenge + token response refresh_token that does not rotate on reuse (compare JTI/lifetime on repeat use)
verify_steps: AUTH_HELPED — with sandbox account: GET https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth, log uttered params; exchange at /nidp/oauth/nam/token (client_secret_post), reuse refresh_token twice, diff JTI/lifetime
impact: intercepted code or stolen non-rotating refresh_token = full ATO of BASF customer/supplier portal account (orders, pricing, PII); HIGH
testability: AUTH_HELPED
[NEXT] HUMAN: NAM discovery-exposed OIDC/SAML/JWKS surface fully mapped and gated — no unauth vector opened anywhere in the 11-host estate. Request from program operator: sandbox/supplier test account for the interactive OIDC code-exchange on client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` (record whether authorize carries `code_challenge`, and whether the returned `refresh_token` rotates on reuse) — the single requiring-evidence candidate for the [75] no-PKCE refresh_token ATO — or confirmation to report. Report channel bugs.olivermaicher.eu.
[LEARN] ACCEPTED RECON @ federation.basf.com: NAM OIDC discovery + JWKS + SAML metadata + admin/registration/introspection endpoints mapped end-to-end — clients 401, introspect/revoke 405, userinfo 401, keys 200 public, SAML services benign on GET; no unauth config/registration/key hole
[LEARN] REJECTED OATH @ federation.basf.com: /nidp/oauth/nam/discovery/* + clientRegistration 404 — wrong-path guesses; real NAM registration endpoint is /nidp/oauth/nam/clients (401)
[LEARN] ACCEPTED RECON @ federation.basf.com: discovery exposes provider-level config — password(ROPC)+hybrid grants, plain+S256 PKCE, registration:full/read scopes, LDAP groupMembership + basfOTPUsed claims; informational, exploitation needs a misconfigured registered client (out-of-scope class)
[RISK] BASF SE: 30 — NAM identity plane re-verified fully gated after complete discovery+JWKS+SAML enumeration (registration 401, introspect/revoke 405, userinfo 401, JWKS single RS256). Deepest real exposure unchanged: public OAuth client `86cc4bf9` emitting refresh_token without PKCE, on an IdP that allows plain PKCE+ROPC provider-wide — exploitability still requires interactive proof; no new unauth surface across the 11-host estate.
## 2026-09-06 14:06:46 UTC [target] (model bigpickle)
[HYP] DigitalCommercePlatform public OAuth client code+refresh_token with no PKCE → authz-code interception / refresh-token replay ATO
class: OATH
asset: federation.basf.com / my.basf.com (client 86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4)
confidence: 75
reasoning: SSR boot config — response_type=code, useRefreshToken=true, refresh_token in scope, zero code_challenge refs; NAM discovery confirms provider allows code_challenge S256 AND plain, and newly confirms ROPC password/hybrid grants provider-wide; public client (no secret in SPA); redirect_uri exact-match oracle verified clean (10 variants, no bypass), implicit closed (token→unauthorized_client); residual theft plane is code/refresh interception, not redirect manipulation
evidence_needed: authentic authorize request showing absence of code_challenge + token response refresh_token that does not rotate on reuse (compare JTI/lifetime on repeat use)
verify_steps: AUTH_HELPED — with sandbox account: GET https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth, log uttered params (code_challenge/code_challenge_method); exchange at /nidp/oauth/nam/token (client_secret_post); reuse refresh_token twice and diff JTI/lifetime
impact: intercepted code or stolen non-rotating refresh_token = full ATO of BASF customer/supplier portal account (orders, pricing, PII); HIGH
testability: AUTH_HELPED
[PARKED] NAM IdP ROPC/plain-PKCE exploitation: surface fully gated (401/405/302); ROPC+plain-PKCE require misconfigured REGISTERED client + creds for Password-grant (credential/brute-force = out-of-scope class); no passive verify step
[PARKED] federation.basf.com redirect_uri bypass: exact-match verified across 10 variants (userinfo, %2f%2f, CRLF, port, suffix, dot-segment) — all rejected; no bypass possible
[PARKED] *.api.basf.com (9 hosts): full unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
[FINAL] DigitalCommercePlatform public OAuth client no-PKCE refresh_token — 75 (ranked), AUTH_HELPED, OATH: single surviving high-severity candidate; all other hypotheses passively exhausted
[LEARN] ACCEPTED RECON @ federation.basf.com: NAM OIDC/SAML/JWKS surface mapped end-to-end via discovery-advertised paths; all administrative endpoints auth-gated (clients 401, introspect/revoke 405, userinfo 401, end_session 302 logout) — no unauth config/registration/key hole
[LEARN] ACCEPTED RECON @ federation.basf.com: discovery exposes provider config — password(ROPC)+hybrid grants, plain+S256 PKCE, registration:full/read scopes, LDAP groupMembership + basfOTPUsed claims; informational config/attr-name leak only, exploitation needs a misconfigured registered client (out-of-scope class)
[LEARN] ACCEPTED RECON @ federation.basf.com: NAM OIDC discovery + JWKS + SAML metadata + admin/registration/introspection endpoints mapped end-to-end — clients 401, introspect/revoke 405, userinfo 401, keys 200 public, SAML services benign on GET; no unauth config/registration/key hole
[LEARN] ACCEPTED RECON @ *.api.basf.com estate: full 9-host unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
[LEARN] REJECTED OATH @ federation.basf.com: redirect_uri oracle exact-match — all 10 bypass variants rejected; no open redirect or path traversal possible
[LEARN] REJECTED MISCONFIG @ federation.basf.com: dynamic client registration endpoint `/nidp/oauth/nam/discovery/registration` returns 404 — not exposed
[LEARN] ACCEPTED RECON @ my.basf.com: SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with redirect_uri, scope, refresh_token, zero PKCE refs
[LEARN] ACCEPTED RECON @ federation.basf.com: authorize endpoint live; legit full flow → 302 /nidp/app/login; error pages styled as BASF Authentication Service; OIDC discovery JSON at two NetIQ paths confirms plain+S256 PKCE
[LEARN] ACCEPTED RECON @ my.basf.com + products.basf.com: CloudFront Magnolia SPA (252KB/640KB), same WCMS stack, zero auth entry in HTML — no login links or partner portals discoverable
[LEARN] REJECTED MISCONFIG @ api.basf.com: resolves to 127.0.0.1 (loopback); connection refused on all probes — dead/internal-only DNS entry, zero external attack surface
[LEARN] REJECTED AUTH @ api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — IAM-gated, x-api-key not credential class
[LEARN] REJECTED MISCONFIG @ api.productinformation.basf.com + api-imp.productinformation.basf.com: 403 ForbiddenException / 400 empty with pi+core keys — Lambda authorizer denies
[LEARN] REJECTED MISCONFIG @ prod.api.basf.com: 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected Invalid ApiKey — no additional proxy, key scope exhausted
[RISK] BASF SE: 30
## 2026-09-06 17:13:02 UTC [target] (model bigpickle)
[HYP] DigitalCommercePlatform public OAuth client code+refresh_token with no PKCE → authz-code interception / refresh-token replay ATO
class: OATH
asset: federation.basf.com / my.basf.com (client 86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4)
confidence: 75
reasoning: SSR boot config — response_type=code, useRefreshToken=true, refresh_token in scope, zero code_challenge refs; NAM discovery confirms provider allows code_challenge S256 AND plain, and confirms ROPC password/hybrid grants provider-wide; public client (no secret in SPA); redirect_uri exact-match oracle verified clean (10 variants, no bypass), implicit closed (token→unauthorized_client); residual theft plane is code/refresh interception, not redirect manipulation
evidence_needed: authentic authorize request showing absence of code_challenge + token response refresh_token that does not rotate on reuse (compare JTI/lifetime on repeat use)
verify_steps: AUTH_HELPED — with sandbox account: GET https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth, log uttered params (code_challenge/code_challenge_method); exchange at /nidp/oauth/nam/token (client_secret_post); reuse refresh_token twice and diff JTI/lifetime
impact: intercepted code or stolen non-rotating refresh_token = full ATO of BASF customer/supplier portal account (orders, pricing, PII); HIGH
testability: AUTH_HELPED
[PARKED] NAM IdP ROPC/plain-PKCE exploitation: surface fully gated (401/405/302); ROPC+plain-PKCE require misconfigured REGISTERED client + creds for Password-grant (credential/brute-force = out-of-scope class); no passive verify step
[PARKED] federation.basf.com redirect_uri bypass: exact-match verified across 10 variants — all rejected; no bypass possible
[PARKED] *.api.basf.com (9 hosts): full unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
[FINAL] DigitalCommercePlatform public OAuth client no-PKCE refresh_token — 75 (ranked), AUTH_HELPED, OATH: single surviving high-severity candidate; all other hypotheses passively exhausted
[NEXT] HUMAN: NAM discovery-exposed OIDC/SAML/JWKS surface fully mapped & gated — no unauth vector opened anywhere in the 11-host estate. All passive threads consumed. Request from program operator: sandbox/supplier test account for the interactive OIDC code-exchange on client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` (record whether authorize carries `code_challenge`, and whether the returned `refresh_token` rotates on reuse) — the single requiring-evidence candidate for the [75] no-PKCE refresh_token ATO — or confirmation to report. Report channel bugs.olivermaicher.eu.
[LEARN] ACCEPTED RECON @ federation.basf.com: NAM OIDC/SAML/JWKS surface mapped end-to-end via discovery-advertised paths; all administrative endpoints auth-gated (clients 401, introspect/revoke 405, userinfo 401, end_session 302 logout) — no unauth config/registration/key hole
[LEARN] ACCEPTED RECON @ federation.basf.com: discovery exposes provider config — password(ROPC)+hybrid grants, plain+S256 PKCE, registration:full/read scopes, LDAP groupMembership + basfOTPUsed claims; informational config/attr-name leak only, exploitation needs a misconfigured registered client (out-of-scope class)
[LEARN] ACCEPTED RECON @ federation.basf.com: NAM OIDC discovery + JWKS + SAML metadata + admin/registration/introspection endpoints mapped end-to-end — clients 401, introspect/revoke 405, userinfo 401, keys 200 public, SAML services benign on GET; no unauth config/registration/key hole
[LEARN] REJECTED OATH @ federation.basf.com: redirect_uri oracle exact-match — all 10 bypass variants rejected; no open redirect or path traversal possible
[LEARN] ACCEPTED RECON @ *.api.basf.com estate: full 9-host unauth surface mapped end-to-end — zero reachable endpoints beyond auth gates/404
[LEARN] REJECTED MISCONFIG @ federation.basf.com: dynamic client registration endpoint `/nidp/oauth/nam/discovery/registration` returns 404 — not exposed
[LEARN] ACCEPTED RECON @ my.basf.com: SSR boot config fully discloses public OAuth client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` with redirect_uri, scope, refresh_token, zero PKCE refs
[LEARN] REJECTED MISCONFIG @ api.basf.com: resolves to 127.0.0.1 (loopback); connection refused — dead/internal-only DNS entry
[LEARN] REJECTED AUTH @ api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — IAM-gated, x-api-key not credential class
[LEARN] REJECTED MISCONFIG @ api.productinformation.basf.com + api-imp.productinformation.basf.com: 403 ForbiddenException / 400 with pi+core keys — Lambda authorizer denies
[LEARN] REJECTED MISCONFIG @ prod.api.basf.com: 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys rejected Invalid ApiKey — no additional proxy, key scope exhausted
[RISK] BASF SE: 30 — NAM identity plane re-verified fully gated after complete discovery+JWKS+SAML enumeration (registration 401, introspect/revoke 405, userinfo 401, JWKS single RS256). Deepest real exposure unchanged: public OAuth client `86cc4bf9` emitting refresh_token without PKCE, on an IdP that allows plain PKCE+ROPC provider-wide — exploitability still requires interactive proof; no new unauth surface across the 11-host estate.
## 2026-09-06 19:18:59 UTC [target] (model bigpickle)
[PRIO] my.basf.com / federation.basf.com (OAuth no-PKCE),42,9/3/8/0/6/4 — ATO via refresh_token theft; highest value class; but gate_ease=0 (AUTH_HELPED required, no test creds available)
[HYP] DigitalCommercePlatform public OAuth client code+refresh_token with no PKCE → authz-code interception / refresh-token replay ATO
class: OATH
asset: federation.basf.com / my.basf.com (client 86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4)
confidence: 75
reasoning: SSR boot config — response_type=code, useRefreshToken=true, refresh_token in scope, zero code_challenge refs; NAM discovery confirms provider allows code_challenge S256 AND plain, and confirms ROPC password/hybrid grants provider-wide; public client (no secret in SPA); redirect_uri exact-match oracle verified clean (10 variants, no bypass), implicit closed (token→unauthorized_client); residual theft plane is code/refresh interception, not redirect manipulation
evidence_needed: authentic authorize request showing absence of code_challenge + token response refresh_token that does not rotate on reuse (compare JTI/lifetime on repeat use)
verify_steps: AUTH_HELPED — with sandbox account: GET https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth, log uttered params (code_challenge/code_challenge_method); exchange at /nidp/oauth/nam/token (client_secret_post); reuse refresh_token twice and diff JTI/lifetime
impact: intercepted code or stolen non-rotating refresh_token = full ATO of BASF customer/supplier portal account (orders, pricing, PII); HIGH
testability: AUTH_HELPED
[PARKED] NAM IdP ROPC/plain-PKCE exploitation: surface fully gated (401/405/302); ROPC+plain-PKCE require misconfigured REGISTERED client + creds for Password-grant (credential/brute-force = out-of-scope class); no passive verify step
[PARKED] federation.basf.com redirect_uri bypass: exact-match verified across 10 variants — all rejected; no bypass possible
[PARKED] *.api.basf.com (9 hosts): full unauth surface mapped end-to-end — zero reachable endpoints beyond auth gates/404
[FINAL] DigitalCommercePlatform public OAuth client no-PKCE refresh_token — 75 (ranked), AUTH_HELPED, OATH: single surviving high-severity candidate; all other hypotheses passively exhausted
[NEXT] HUMAN: All passive threads consumed across 11-host estate. Provide sandbox/supplier test account for interactive OIDC code-exchange on client `86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4` (record whether authorize carries `code_challenge`, and whether the returned `refresh_token` rotates on reuse) — the single requiring-evidence candidate for the [75] no-PKCE refresh_token ATO — or confirmation to report. Report channel bugs.olivermaicher.eu.
[LEARN] ACCEPTED RECON @ *.api.basf.com estate: full 9-host unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
[LEARN] REJECTED MISCONFIG @ api.basf.com: resolves to 127.0.0.1 (loopback); connection refused — dead/internal-only DNS entry
[LEARN] ACCEPTED RECON @ federation.basf.com: NAM OIDC/SAML/JWKS surface mapped end-to-end via discovery-advertised paths; all administrative endpoints auth-gated — no unauth config/registration/key hole
[LEARN] REJECTED OATH @ federation.basf.com: redirect_uri oracle exact-match — all 10 bypass variants rejected; no open redirect or path traversal possible
[RISK] BASF SE: 30 — 11-host estate fully mapped end-to-end; zero unauth vectors found; sole surviving candidate (no-PKCE refresh_token ATO, confidence 75) is AUTH_HELPED — requires sandbox/supplier test account to demonstrate. No new attack surface discoverable without interactive access.
