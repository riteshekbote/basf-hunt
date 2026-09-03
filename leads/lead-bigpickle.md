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
