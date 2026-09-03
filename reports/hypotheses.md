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
