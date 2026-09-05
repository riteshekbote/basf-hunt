# Hypotheses (ranked)

## RANKED HYPOTHESES 2026-09-03 09:40:45 UTC

## RANKED HYPOTHESES 2026-09-03 14:03:00 UTC
- [65] ap-digitalconnect.api.basf.com: Azure Function master key leak via misconfigured CI/CD or repo exposure (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://ap-eupf.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01 (test SSRF via common url param on Azure 
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com: master key leak hypothesis lacks concrete verify step without auth; passive GitHub search is opportunistic not s
- LEARN: REJECTED AUTH @ dev-clientcert-sap.api.basf.com: client cert bypass speculative; no CT evidence BASF uses public CA for mTLS; dev endpoints may be non-productio
- LEARN: ACCEPTED SSRF @ ap-eupf.api.basf.com: Azure Functions HTTP triggers are known SSRF vectors; cloud metadata endpoint confirmed reachable from App Service; concre

## RANKED HYPOTHESES 2026-09-03 17:55:27 UTC
- [85] ap-eupf.api.basf.com: Azure Functions SSRF to cloud metadata via url parameter (from art/lead_bigpickle.txt)
- [55] ap-digitalconnect.api.basf.com: Azure Function master key leak via non-standard admin path exposure (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://ap-digitalconnect.api.basf.com/admin/keys (test alternate admin keys path)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://ap-eupf.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01
- LEARN: REJECTED AUTH @ dev-clientcert-sap.api.basf.com: client cert bypass speculative; no CT evidence BASF uses public CA for mTLS; dev endpoints may be non-productio
- LEARN: CHANGED AUTH @ ap-digitalconnect.api.basf.com: `/admin/host/keys` returns 404 not 401; admin surface non-standard — shifts key leak hunt to alt paths + passive 
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com: common `url` param probes return 403; WAF/edge blocks metadata endpoint; header-based or alternate function triggers rema
- LEARN: ACCEPTED SSRF @ ap-eupf.api.basf.com: Azure Functions confirmed via KB; metadata endpoint reachable from App Service; concrete url param for testing
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com: master key leak lacks verify without auth; passive GitHub search not systematic
- LEARN: REJECTED AUTH @ dev-clientcert-sap.api.basf.com: mTLS bypass speculative; no CT evidence of public CA use

## RANKED HYPOTHESES 2026-09-03 21:00:23 UTC
- [62] ap-eupf.api.basf.com: SSRF via header-injected URL on Azure Functions health endpoint (from art/lead_bigpickle.txt)
- [55] ap-digitalconnect.api.basf.com: Azure Function master key leak via non-standard admin path or CI/CD exposure (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://ap-digitalconnect.api.basf.com/.azurefunctions/keys (test Azure internal alternate keys path)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://ap-eupf.api.basf.com/api/health with headers X-Forwarded-For: 169.254.169.254 and Referer: http://169.254.169.254/metadata/instance?api-versi
- LEARN: ACCEPTED AUTH @ ap-digitalconnect.api.basf.com: standard `/admin/host/keys` and `/runtime/webhooks/host/keys` return 401 (not 404); admin surface is standard Az
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com: common `url` param probes return 403; WAF/edge blocks metadata endpoint; header-based probes (`X-Forwarded-Url`, `X-Callb
- LEARN: REJECTED AUTH @ dev-clientcert-sap.api.basf.com: client cert bypass speculative; no CT evidence BASF uses public CA for mTLS; dev endpoints may be non-productio
- LEARN: REJECTED MISCONFIG @ dev-ext001.api.basf.com: confidence too low for active probe; dev client cert requirement blocks passive disclosure
- LEARN: ACCEPTED SSRF @ ap-eupf.api.basf.com: Azure Functions confirmed; metadata endpoint reachable from App Service; url param blocked by WAF; header injection remain

## RANKED HYPOTHESES 2026-09-03 23:13:28 UTC
- [65] ap-eupf.api.basf.com: Azure Functions SSRF to cloud metadata via header injection (from art/lead_bigpickle.txt)
- [50] ap-digitalconnect.api.basf.com: Azure Function master key leak via non-standard admin path or CI/CD artifact exposure (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://ap-eupf.api.basf.com/api/health with headers Referer: http://169.254.169.254/metadata/instance?api-version=2021-02-01 (test header-based SSRF
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://ap-digitalconnect.api.basf.com/admin/host/keys?slot=staging (test deployment slot admin keys endpoint)
- LEARN: REJECTED MISCONFIG @ dev-ext001.api.basf.com: confidence too low for active probe; dev client cert requirement blocks passive disclosure
- LEARN: ACCEPTED SSRF @ ap-eupf.api.basf.com: Azure Functions confirmed; metadata endpoint reachable from App Service; url param blocked by WAF; header injection remain
- LEARN: ACCEPTED AUTH @ ap-digitalconnect.api.basf.com: standard `/admin/host/keys` and `/runtime/webhooks/host/keys` return 401 (not 404); admin surface is standard Az
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com: common `url` param probes return 403; WAF/edge blocks metadata endpoint; header-based probes (`X-Forwarded-Url`, `X-Callb
- LEARN: REJECTED AUTH @ dev-clientcert-sap.api.basf.com: mTLS bypass speculative; no CT evidence of public CA use
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/.azurefunctions/keys: returns 404; Azure internal alternate keys path not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/v2/keys: returns 404; versioned admin keys path not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/list: returns 404; admin list endpoint not exposed
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content; indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404; common health endpoint not exposed
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403; param-based SSRF blocked at edge/WAF for enumerated function names

## RANKED HYPOTHESES 2026-09-04 01:13:42 UTC
- [50] ap-digitalconnect.api.basf.com: Azure Function master key leak via deployment slot misconfiguration or CI/CD artifact exposure (from art/lead_nemotron3.txt)
- [42] ap-digitalconnect.api.basf.com: Azure Functions admin/system key via deployment-slot key inheritance on Function App (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://ap-digitalconnect.api.basf.com/admin/host/systemkeys (test Function App slot/system-keys endpoint — last untested standard admin sub-resource
- NEXT(hypotheses-nemotron3.txt): RAG: Passive GitHub/GitLab dork search for `basf.com` + `function` + `master` + `key` + `WEBSITE_AUTH_` + `ap-digitalconnect` + `ap-eupf` to find leaked Functio
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/keys?slot=staging: returns 404; deployment slot staging admin keys path not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/keys?slot=production: returns 404; deployment slot production admin keys path not exposed
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
- LEARN: ACCEPTED AUTH @ ap-digitalconnect.api.basf.com: standard `/admin/host/keys` and `/runtime/webhooks/host/keys` return 401 (not 404); admin surface is standard Az
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names

## RANKED HYPOTHESES 2026-09-04 06:05:58 UTC
- [55] e-gate.api.basf.com: API gateway OpenAPI/Swagger docs or hidden API routes on e-gate (from art/lead_bigpickle.txt)
- [50] ap-digitalconnect.api.basf.com: Azure Function master key leak via CI/CD artifact or storage misconfiguration exposure (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): RAG: Passive GitHub/GitLab dork search for `basf.com` + `function` + `master` + `key` + `WEBSITE_AUTH_` + `ap-digitalconnect` + `ap-eupf` to find leaked Functio
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://e-gate.api.basf.com/openapi.json && GET /swagger.json && GET /api-docs && GET /swagger/ui && GET /docs && GET /.well-known/openapi-schema (te
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/keys?slot=staging: returns 404; deployment slot staging admin keys path not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/keys?slot=production: returns 404; deployment slot production admin keys path not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/systemkeys: returns 404; slot/system keys endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/functions: returns 404; function list endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/status: returns 404; host status endpoint not exposed
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
- LEARN: ACCEPTED AUTH @ ap-digitalconnect.api.basf.com: standard `/admin/host/keys` and `/runtime/webhooks/host/keys` return 401 (not 404); admin surface is standard Az
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/systemkeys: returns 404; system keys endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/functions: returns 404; admin function list not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/status: returns 404; admin host status not exposed
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 6 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; zero probes run; API gateway naming convention warrants documentation path enumeration

## RANKED HYPOTHESES 2026-09-04 11:37:35 UTC
- [55] e-gate.api.basf.com: API gateway OpenAPI/Swagger documentation exposure on e-gate (from art/lead_nemotron3.txt)
- [55] e-gate.api.basf.com: API gateway OpenAPI/Swagger docs or hidden API routes on e-gate (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://e-gate.api.basf.com/openapi.json && GET /swagger.json && GET /api-docs && GET /swagger/ui && GET /docs && GET /.well-known/openapi-schema (te
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://e-gate.api.basf.com/openapi.json && GET https://e-gate.api.basf.com/swagger.json && GET https://e-gate.api.basf.com/api-docs && GET https://e
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/systemkeys: returns 404; system keys endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/functions: returns 404; admin function list not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/status: returns 404; host status endpoint not exposed
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 6 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; zero probes run; API gateway naming convention warrants documentation path enumeration
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/keys?slot=staging: returns 404; deployment slot staging admin keys path not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/keys?slot=production: returns 404; deployment slot production admin keys path not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/systemkeys: returns 404; system keys endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/functions: returns 404; function list endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/status: returns 404; host status endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/functionkeys: returns 404; v4 function-level key endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/system: returns 404; admin system info endpoint not exposed
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; zero probes run against any sub-paths; API gateway naming convention warrants documentation path

## RANKED HYPOTHESES 2026-09-04 15:26:08 UTC
- [68] e-gate.api.basf.com: API gateway OpenAPI/Swagger documentation exposure on e-gate (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://e-gate.api.basf.com/openapi.json && GET /swagger.json && GET /api-docs && GET /swagger/ui && GET /docs && GET /.well-known/openapi-schema (te
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; zero probes run against any sub-paths; API gateway naming convention warrants documentation path
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/systemkeys: returns 404; system keys endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/functions: returns 404; admin function list not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/status: returns 404; host status endpoint not exposed
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names

## RANKED HYPOTHESES 2026-09-04 18:36:03 UTC
- [75] e-gate.api.basf.com: API gateway OpenAPI/Swagger documentation exposure on e-gate (from art/lead_nemotron3.txt)
- [62] ap-eupf.api.basf.com: API route/function discovery in 150KB root response body on ap-eupf (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://ap-eupf.api.basf.com/ — save response body, extract Content-Type, <script> tags, fetch() URLs, and any /api/ or function-name references. THE
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://e-gate.api.basf.com/openapi.json -k && GET https://e-gate.api.basf.com/swagger.json -k && GET https://e-gate.api.basf.com/api-docs -k && GET 
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/functionkeys: returns 404; v4 function-level key endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/system: returns 404; admin system info endpoint not exposed
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: MITIGATED SSRF @ e-gate.api.basf.com: all 6 API doc probes return SSL CERTIFICATE_VERIFY_FAILED; host likely mTLS-gated like dev endpoints; SSL error blocks all
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; SSL errors on all sub-paths; mTLS or self-signed cert required; surface inaccessible without cli
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/functionkeys: returns 404; v4 function-level key endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/system: returns 404; admin system info endpoint not exposed
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns 404 at root; 6/6 documentation paths return SSL cert verify failure (not 404) — endpoints exist but TLS misco
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names

## RANKED HYPOTHESES 2026-09-04 21:06:19 UTC
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/functionkeys: returns 404; v4 function-level key endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/system: returns 404; admin system info endpoint not exposed
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns SSL CERTIFICATE_VERIFY_FAILED at root; 6/6 documentation paths return SSL cert verify failure (not 404) — end
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names
- LEARN: CHANGED RECON @ e-gate.api.basf.com/: root changed from HTTP 404 to SSL CERTIFICATE_VERIFY_FAILED — host is live with TLS misconfiguration (mTLS/self-signed)

## RANKED HYPOTHESES 2026-09-04 23:08:56 UTC
- [75] e-gate.api.basf.com: API gateway OpenAPI/Swagger documentation exposure on e-gate (from art/lead_nemotron3.txt)
- [45] ap-eupf.api.basf.com: ap-eupf unlisted function names recoverable via passive artifact/code search (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): RAG: passive GitHub-code/commit + secret-scanning + CT-log search for "ap-eupf.api.basf.com", "ap-digitalconnect.api.basf.com", "basf digitalconnect" to recover
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://e-gate.api.basf.com/openapi.json -k && GET https://e-gate.api.basf.com/swagger.json -k && GET https://e-gate.api.basf.com/api-docs -k && GET 
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/functionkeys: returns 404; v4 function-level key endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/system: returns 404; admin system info endpoint not exposed
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns SSL CERTIFICATE_VERIFY_FAILED at root; 6/6 documentation paths return SSL cert verify failure (not 404) — end
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names
- LEARN: CHANGED RECON @ e-gate.api.basf.com/: root changed from HTTP 404 to SSL CERTIFICATE_VERIFY_FAILED — host is live with TLS misconfiguration (mTLS/self-signed)

## RANKED HYPOTHESES 2026-09-05 01:11:18 UTC
- [75] e-gate.api.basf.com: API gateway OpenAPI/Swagger documentation exposure on e-gate (from art/lead_nemotron3.txt)
- [50] my.basf.com: BASF portal/web estate (my.basf.com, www.basf.com) unprobed while in program scope (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: *.api.basf.com unauth surface fully consumed (2 placeholder Function Apps auth-gated + `/.auth/*` closed, 3 mTLS dev hosts, 3 catch-all 404 hosts, e-gate
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://e-gate.api.basf.com/openapi.json -k && GET https://e-gate.api.basf.com/swagger.json -k && GET https://e-gate.api.basf.com/api-docs -k && GET 
- LEARN: REJECTED MISCONFIG @ e-gate.api.basf.com: not mTLS — handshake succeeds without client cert; cert CN=e-gate.api.basf.com issued by DigiCert Global G2 TLS RSA SH
- LEARN: REJECTED MISCONFIG @ ap-eupf.api.basf.com: 150093-byte root is stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com) — no `/api/
- LEARN: REJECTED MISCONFIG @ ap-eupf.api.basf.com + ap-digitalconnect.api.basf.com: /.auth/config and /.auth/me return 404 — App Service EasyAuth not exposed on either 
- LEARN: REJECTED OTHER @ ap-eupf.api.basf.com: passive artifact/code search for function names and keys returned zero public hits — unverifiable, exhausted
- LEARN: ACCEPTED RECON @ dev.api.basf.com + dev-m.api.basf.com + dev-sap.api.basf.com: openapi.json/swagger.json/api-docs all 404 — catch-all hosts, no gateway/docs sur
- LEARN: ACCEPTED RECON @ *.api.basf.com estate: full 9-host unauth surface now mapped end-to-end (2 placeholders auth-gated, 3 mTLS 400, 3 catch-all 404, e-gate 404) — 
- LEARN: ACCEPTED RECON @ ap-digitalconnect.api.basf.com: all 8 standard Azure Functions admin endpoints tested (401 on auth-gated, 404 on non-existent); admin surface f
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/host/functionkeys: returns 404; v4 function-level key endpoint not exposed
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com/admin/system: returns 404; admin system info endpoint not exposed
- LEARN: ACCEPTED RECON @ e-gate.api.basf.com: host returns SSL CERTIFICATE_VERIFY_FAILED at root; 6/6 documentation paths return SSL cert verify failure (not 404) — end
- LEARN: ACCEPTED RECON @ ap-eupf.api.basf.com/: root returns 200 with 150KB content (reconfirmed); indicates active function handler or custom landing page at root
- LEARN: REJECTED SSRF @ ap-eupf.api.basf.com/api/health: returns 404 (reconfirmed); common health endpoint not exposed
- LEARN: MITIGATED SSRF @ ap-eupf.api.basf.com/api/<enum>?url=: returns 403 (reconfirmed); param-based SSRF blocked at edge/WAF for enumerated function names
- LEARN: CHANGED RECON @ e-gate.api.basf.com/: root changed from HTTP 404 to SSL CERTIFICATE_VERIFY_FAILED — host is live with TLS misconfiguration (mTLS/self-signed)

## RANKED HYPOTHESES 2026-09-05 05:58:39 UTC
- [65] my.basf.com: Corporate portal authentication bypass via subdomain takeover or misconfigured Azure AD B2C (from art/lead_nemotron3.txt)
- [55] prod.api.basf.com: prod.api.basf.com additional unauthenticated/public-key Apigee proxies beyond /productinformation (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: read-only GET sweep of ~25 probable proxy base-paths on https://prod.api.basf.com (products, catalog, search, user, order, cart, price, availability, doc
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.basf.com/ -I && GET https://api.basf.com/openapi.json -k && GET https://api.basf.com/swagger.json -k && GET https://api.basf.com/docs -k 
- LEARN: REJECTED MISCONFIG @ e-gate.api.basf.com: all 7 documentation paths + root return HTTP 404 (not SSL error) — server=Microsoft-HTTPAPI/2.0, no API gateway docs e
- LEARN: ACCEPTED RECON @ *.api.basf.com: full 9-host unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
- LEARN: REJECTED MISCONFIG @ ap-eupf.api.basf.com: 150KB root is stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com) — zero function r

## RANKED HYPOTHESES 2026-09-05 10:30:27 UTC
- [70] api.basf.com: API gateway OpenAPI/Swagger documentation exposure on api.basf.com (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.basf.com/ -I && GET https://api.basf.com/openapi.json -k && GET https://api.basf.com/swagger.json -k && GET https://api.basf.com/docs -k 
- LEARN: REJECTED MISCONFIG @ e-gate.api.basf.com: all 7 documentation paths + root return HTTP 404 (not SSL error) — server=Microsoft-HTTPAPI/2.0, no API gateway docs e
- LEARN: ACCEPTED RECON @ *.api.basf.com: full 9-host unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
- LEARN: REJECTED MISCONFIG @ ap-eupf.api.basf.com: 150KB root is stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com) — zero function r
- LEARN: REJECTED MISCONFIG @ ap-eupf.api.basf.com + ap-digitalconnect.api.basf.com: /.auth/config and /.auth/me return 404 — App Service EasyAuth not exposed on either 
- LEARN: REJECTED OTHER @ ap-eupf.api.basf.com: passive artifact/code search for function names and keys returned zero public hits — unverifiable, exhausted
- LEARN: ACCEPTED RECON @ dev.api.basf.com + dev-m.api.basf.com + dev-sap.api.basf.com: openapi.json/swagger.json/api-docs all 404 — catch-all hosts, no gateway/docs sur

## RANKED HYPOTHESES 2026-09-05 13:34:30 UTC
- [70] api.basf.com: API gateway OpenAPI/Swagger documentation exposure on api.basf.com (from art/lead_nemotron3.txt)
- [55] prod.api.basf.com: prod.api.basf.com additional unauthenticated/public-key Apigee proxies beyond /productinformation (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: read-only GET sweep of ~25 probable proxy base-paths on https://prod.api.basf.com (products, catalog, search, user, order, cart, price, availability, doc
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.basf.com/ -I && GET https://api.basf.com/openapi.json -k && GET https://api.basf.com/swagger.json -k && GET https://api.basf.com/docs -k 
- LEARN: ACCEPTED RECON @ prod.api.basf.com: `/productinformation` returns 401 VerifyAPIKey; path-routed Apigee proxy confirmed; 401-vs-404 oracle operational
- LEARN: ACCEPTED RECON @ api.commerce.basf.com: `/copilot` returns 403 MissingAuthenticationToken; AWS REST API Gateway confirmed; staged routes exist
- LEARN: ACCEPTED RECON @ federation.basf.com: OIDC discovery at `.well-known/openid-configuration` lists plain+S256 PKCE; SPA uses code+refresh tokens with zero PKCE re
- LEARN: ACCEPTED RECON @ my.basf.com: HTTP 200, 204KB; `.well-known/openid-configuration` 404; portal auth stack not at standard OIDC discovery path; deep enum required
- LEARN: ACCEPTED RECON @ api.basf.com: Connection refused on all probes; not publicly reachable; dead or internal-only DNS entry
- LEARN: CHANGED RECON @ my.basf.com: `.well-known/openid-configuration` → HTTP 404 (was previously unprobed); auth stack not at standard OIDC discovery path
- LEARN: REJECTED MISCONFIG @ e-gate.api.basf.com: all 7 documentation paths + root return HTTP 404 (not SSL error) — server=Microsoft-HTTPAPI/2.0, no API gateway docs e
- LEARN: ACCEPTED RECON @ *.api.basf.com: full 9-host unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/404
- LEARN: REJECTED MISCONFIG @ ap-eupf.api.basf.com: 150KB root is stock Azure Functions 3.0 placeholder (azureLogo, aspnetcdn jQuery, go.microsoft.com) — zero function r
- LEARN: REJECTED MISCONFIG @ ap-eupf.api.basf.com + ap-digitalconnect.api.basf.com: /.auth/config and /.auth/me return 404 — App Service EasyAuth not exposed on either 
- LEARN: REJECTED OTHER @ ap-eupf.api.basf.com: passive artifact/code search for function names and keys returned zero public hits — unverifiable, exhausted
- LEARN: ACCEPTED RECON @ dev.api.basf.com + dev-m.api.basf.com + dev-sap.api.basf.com: openapi.json/swagger.json/api-docs all 404 — catch-all hosts, no gateway/docs sur

## RANKED HYPOTHESES 2026-09-05 16:26:13 UTC
- [70] my.basf.com: my.basf.com authentication stack enumeration for OAuth/OIDC misconfiguration (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://my.basf.com/ -k && GET https://my.basf.com/auth/.well-known/openid-configuration -k && GET https://my.basf.com/basf/.well-known/openid-config
- LEARN: REJECTED MISCONFIG @ api.basf.com: resolves to 127.0.0.1 (loopback); connection refused on all probes; dead/internal-only DNS entry — zero external attack surfa
- LEARN: ACCEPTED RECON @ my.basf.com: HTTP 200, 204KB SPA via CloudFront; `.well-known/openid-configuration` returns 404 (HTML error from BASF Auth Service/NetIQ); auth
- LEARN: ACCEPTED RECON @ www.basf.com: HTTP 308 to /us/en via CloudFront; 640KB; A records to CloudFront IPs (18.172.x.x); no CNAME, no dangling resource
- LEARN: ACCEPTED RECON @ basf.com: A record to CloudFront IP (13.248.131.227); no CNAME
- LEARN: ACCEPTED RECON @ prod.api.basf.com: CNAME basf-prod-prod.apigee.net (Apigee); `/productinformation` returns 401 VerifyAPIKey; 25 common proxy paths return 404; 
- LEARN: ACCEPTED RECON @ api.commerce.basf.com: AWS REST API Gateway (x-amz-apigw-id); `/copilot` returns 403 MissingAuthenticationToken; root returns 400; staged route
- LEARN: ACCEPTED RECON @ federation.basf.com: NetIQ/Access Manager login page at `.well-known/openid-configuration`; standard OIDC discovery not exposed; HTML error pag

## RANKED HYPOTHESES 2026-09-05 18:46:05 UTC
- [52] my.basf.com: my.basf.com authentication stack enumeration for OAuth/OIDC misconfiguration (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: read-only GET sweep of ~25 probable proxy base-paths on https://prod.api.basf.com (products, catalog, search, user, order, cart, price, availability, doc
- LEARN: REJECTED MISCONFIG @ prod.api.basf.com: 66 proxy-path probes all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected Invalid
- LEARN: REJECTED AUTH @ api.commerce.basf.com: `/copilot` + 8 stage prefixes all 403 MissingAuthenticationTokenException with navigator key — route IAM/SigV4-gated, x-a
- LEARN: REJECTED MISCONFIG @ api.productinformation.basf.com + api-imp.productinformation.basf.com: 403 ForbiddenException with pi+core keys — Lambda-authorizer/API-GW-
- LEARN: ACCEPTED RECON @ api-imp.commerce.basf.com: HTTP 400 empty root gate (CloudFront→API GW ingest); no auth oracle; imp-* = import/processing tier
- LEARN: REJECTED MISCONFIG @ prod.api.basf.com: 66 proxy paths all 404 except /productinformation (401); 4 browser keys Invalid ApiKey — no extra proxy, key scope exhau
- LEARN: REJECTED AUTH @ api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — IAM-gated, x-api-key not credential class
- LEARN: REJECTED MISCONFIG @ api.productinformation.basf.com + api-imp.productinformation.basf.com: 403 ForbiddenException / 400 empty with pi+core keys — authorizer de
- LEARN: REJECTED OATH @ federation.basf.com: redirect_uri oracle exact-match — @ userinfo, %2f%2f, CRLF, port, suffix, dot-segment all rejected; no bypass
- LEARN: ACCEPTED RECON @ my.basf.com: SSR boot config discloses full OAuth client (86cc4bf9-…, redirect /.auth, refresh_token scope, acr 3IAM/Login/External, code flow,
- LEARN: ACCEPTED RECON @ federation.basf.com: authorize endpoint live; legit full flow → 302 /nidp/app/login; error pages styled as BASF Authentication Service
- LEARN: ACCEPTED RECON @ products.basf.com: CloudFront Magnolia SPA (252KB), same WCMS stack, zero auth entry in HTML

## RANKED HYPOTHESES 2026-09-05 20:52:44 UTC
- [45] federation.basf.com: DigitalCommercePlatform public OAuth client code+refresh_token with no PKCE → authz-code interception / refresh-token replay ATO (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: all passive threads on the OAuth chain now exhausted (portal server-surface blind, redirect oracle exact-match, implicit disabled, allowlist enumerated, 
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://my.basf.com/ -I (capture full headers: Set-Cookie, Location, x-ms-*, Server); save body; grep body for `client_id`,`redirect_uri`,`authorize`
- LEARN: REJECTED MISCONFIG @ prod.api.basf.com: 66 proxy paths all 404 except `/productinformation` (401); 4 browser keys (core/pi/csp/navigator) rejected Invalid ApiKe
- LEARN: REJECTED AUTH @ api.commerce.basf.com: 8 stage prefixes all MissingAuthenticationTokenException — IAM-gated, x-api-key not credential class
- LEARN: REJECTED MISCONFIG @ api.productinformation.basf.com + api-imp.productinformation.basf.com: 403 ForbiddenException / 400 with pi+core keys — Lambda authorizer d
- LEARN: REJECTED OATH @ federation.basf.com: redirect_uri oracle exact-match — @ userinfo, %2f%2f, CRLF, port, suffix, dot-segment all rejected; no bypass
- LEARN: ACCEPTED RECON @ my.basf.com: SSR boot config discloses full OAuth client (86cc4bf9-…, redirect /.auth, refresh_token scope, acr 3IAM/Login/External, code flow,
- LEARN: ACCEPTED RECON @ federation.basf.com: authorize endpoint live; legit full flow → 302 /nidp/app/login; error pages styled as BASF Authentication Service
- LEARN: ACCEPTED RECON @ products.basf.com: CloudFront Magnolia SPA (252KB), same WCMS stack, zero auth entry in HTML
- LEARN: ACCEPTED RECON @ *.api.basf.com estate: full 9-host unauth surface mapped end-to-end — zero reachable endpoints, functions, keys, or configs beyond auth gates/4
