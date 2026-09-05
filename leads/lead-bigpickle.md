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
