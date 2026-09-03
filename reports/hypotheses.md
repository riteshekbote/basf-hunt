# Hypotheses (ranked)

## RANKED HYPOTHESES 2026-09-03 09:40:45 UTC

## RANKED HYPOTHESES 2026-09-03 14:03:00 UTC
- [65] ap-digitalconnect.api.basf.com: Azure Function master key leak via misconfigured CI/CD or repo exposure (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://ap-eupf.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01 (test SSRF via common url param on Azure 
- LEARN: REJECTED AUTH @ ap-digitalconnect.api.basf.com: master key leak hypothesis lacks concrete verify step without auth; passive GitHub search is opportunistic not s
- LEARN: REJECTED AUTH @ dev-clientcert-sap.api.basf.com: client cert bypass speculative; no CT evidence BASF uses public CA for mTLS; dev endpoints may be non-productio
- LEARN: ACCEPTED SSRF @ ap-eupf.api.basf.com: Azure Functions HTTP triggers are known SSRF vectors; cloud metadata endpoint confirmed reachable from App Service; concre
