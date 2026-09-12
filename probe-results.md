
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

## 2026-09-08 13:34:06 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://passage-europe.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://vss3.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> 200 len=235
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://rep.basf.com/actuator/heapdump -> HTTP 404
https://rep.basf.com/actuator/mappings -> HTTP 404
https://rep.basf.com/actuator/configprops -> HTTP 404
https://rep.basf.com/actuator/beans -> HTTP 404
https://rep.basf.com/actuator/threaddump -> HTTP 404
https://rep.basf.com/actuator/env -> HTTP 404

## 2026-09-08 17:39:18 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://passage-europe.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://vss3.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> 200 len=235
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://rep.basf.com/actuator/health -> 200 len=?
https://rep.basf.com/actuator/metrics -> HTTP 404
https://rep.basf.com/actuator/loggers -> HTTP 404
https://rep.basf.com/actuator/scheduledtasks -> HTTP 404
https://rep.basf.com/actuator/httpexchanges -> HTTP 404
https://rep.basf.com/actuator/integrationgraph -> HTTP 404

## 2026-09-08 20:22:14 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://passage-europe.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://vss3.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> 200 len=235
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684

## 2026-09-08 22:50:37 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://passage-europe.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://vss3.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> 200 len=235
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684

## 2026-09-09 01:30:39 UTC
https://my.basf.com/.auth -> 200 len=205005

## 2026-09-09 06:12:11 UTC
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://passage-europe.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://vss3.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> 200 len=235
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684

## 2026-09-09 11:48:04 UTC
https://my.basf.com/.auth -> 200 len=205005
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://passage-europe.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://vss3.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> 200 len=235
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://my.basf.com/.auth` -> 200 len=204926

## 2026-09-09 15:35:57 UTC
https://my.basf.com/.auth -> 200 len=205005
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://passage-europe.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://vss3.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> 200 len=235
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684

## 2026-09-09 18:50:58 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://procurement.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://procurement.basf.com/irj/servlet/webdav/documents/newFramework/ -> 200 len=236
https://tm.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://passage-europe.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> HTTP 500
https://vss3.basf.com/irj/servlet/prt/portal/prtroot/com.sap.km.cm.documents/?path=/documents/newFramework -> 200 len=235
https://my.basf.com/.auth` -> 200 len=204924
https://my.basf.com/.auth -> 200 len=205005

## 2026-09-09 21:42:45 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://rep.basf.com/wicket/bookmarkable/ -> HTTP 404
https://rep.basf.com/app/ -> HTTP 404
https://rep.basf.com/wicket/resource/ -> HTTP 404
https://rep.basf.com/wicket/bookmarkable/com.basf.rep.pages.CustomerDashboard -> HTTP 404
https://rep.basf.com/wicket/bookmarkable/com.basf.rep.pages.OrderHistory -> HTTP 404
https://secsys.basf.com/ -> 200 len=3179
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/devices -> 200 len=246
https://secsys.basf.com/services/ -> HTTP 404
https://secsys.basf.com/rest/ -> HTTP 404
https://bsh.secsys.basf.com/ -> 200 len=3179

## 2026-09-09 23:35:37 UTC
https://my.basf.com/.auth -> 200 len=205005

## 2026-09-10 01:33:25 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://rep.basf.com/wicket/bookmarkable/ -> HTTP 404
https://rep.basf.com/app/ -> HTTP 404
https://rep.basf.com/wicket/resource/ -> HTTP 404
https://rep.basf.com/wicket/bookmarkable/com.basf.rep.pages.CustomerDashboard -> HTTP 404
https://rep.basf.com/wicket/bookmarkable/com.basf.rep.pages.OrderHistory -> HTTP 404
https://secsys.basf.com/ -> 200 len=3179
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/devices -> 200 len=246
https://secsys.basf.com/services/ -> HTTP 404
https://secsys.basf.com/rest/ -> HTTP 404
https://bsh.secsys.basf.com/ -> 200 len=3179

## 2026-09-10 06:45:55 UTC
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://rep.basf.com/wicket/bookmarkable/ -> HTTP 404
https://rep.basf.com/app/ -> HTTP 404
https://rep.basf.com/wicket/resource/ -> HTTP 404
https://rep.basf.com/wicket/bookmarkable/com.basf.rep.pages.CustomerDashboard -> HTTP 404
https://rep.basf.com/wicket/bookmarkable/com.basf.rep.pages.OrderHistory -> HTTP 404
https://secsys.basf.com/ -> 200 len=3179
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/devices -> 200 len=246
https://secsys.basf.com/services/ -> HTTP 404
https://secsys.basf.com/rest/ -> HTTP 404
https://bsh.secsys.basf.com/ -> 200 len=3179

## 2026-09-10 12:07:15 UTC
https://my.basf.com/.auth -> 200 len=204983

## 2026-09-10 16:37:10 UTC


## 2026-09-10 19:14:33 UTC
https://experience.basf.com/crx/de -> HTTP 404
https://experience.basf.com/system/console -> HTTP 404
https://experience.basf.com/libs/granite/ui/content/dumplibs.html -> HTTP 404
https://experience.basf.com/content -> HTTP 404
https://experience.basf.com/apps -> HTTP 404
https://experience.basf.com/etc -> HTTP 404
https://experience.basf.com/bin/receive -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.model.json -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.model.txt -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.tidy.-1.json -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.infinity.json -> HTTP 404
https://repfinder.basf.com/jcr:content.model.json -> HTTP 404

## 2026-09-10 21:46:50 UTC
https://experience.basf.com/crx/de -> HTTP 404
https://experience.basf.com/system/console -> HTTP 404
https://experience.basf.com/libs/granite/ui/content/dumplibs.html -> HTTP 404
https://experience.basf.com/crx/packmgr -> HTTP 404
https://experience.basf.com/content -> HTTP 404
https://experience.basf.com/apps -> HTTP 404
https://experience.basf.com/etc -> HTTP 404
https://experience.basf.com/bin/receive -> HTTP 404
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://repfinder.basf.com/content/repfinder/en.model.json -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.model.txt -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.tidy.-1.json -> HTTP 404

## 2026-09-11 00:00:03 UTC
https://experience.basf.com/crx/de -> HTTP 404
https://experience.basf.com/system/console -> HTTP 404
https://experience.basf.com/libs/granite/ui/content/dumplibs.html -> HTTP 404
https://experience.basf.com/crx/packmgr -> HTTP 404
https://experience.basf.com/content -> HTTP 404
https://experience.basf.com/apps -> HTTP 404
https://experience.basf.com/etc -> HTTP 404
https://experience.basf.com/bin/receive -> HTTP 404
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684
https://repfinder.basf.com/content/repfinder/en.model.json -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.model.txt -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.tidy.-1.json -> HTTP 404

## 2026-09-11 04:32:46 UTC
https://experience.basf.com/content.json -> HTTP 404
https://experience.basf.com/content.infinity.json -> HTTP 404
https://experience.basf.com/content.tidy.-1.json -> HTTP 404
https://experience.basf.com/content.feed.xml -> HTTP 404
https://experience.basf.com/jcr:content.feed.xml -> HTTP 404
https://experience.basf.com/_jcr_content.html -> HTTP 404
https://experience.basf.com/_jcr_content.json -> HTTP 404
https://experience.basf.com/_jcr_content.xml -> HTTP 404
https://experience.basf.com/libs/granite/ui/content/dumplibs.html -> HTTP 404
https://experience.basf.com/system/sling/monitoring -> HTTP 404
https://experience.basf.com/system/console/bundles -> HTTP 404
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid+profile+refresh_token&acr_values=3IAM/Login/External -> 200 len=684

## 2026-09-11 09:29:29 UTC
https://agriculture.basf.com/graphql2 -> 200 len=348114
https://agriculture.basf.com/.restful -> 200 len=348114
https://agriculture.basf.com/docurl/ -> 200 len=404917
https://north-america.intranet.basf.com/index.php/login -> 200 len=?
https://repfinder.basf.com/bin/repfinder.json -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.search.json -> HTTP 404
https://north-america.intranet.basf.com/index.php/api -> 200 len=43206
https://north-america.intranet.basf.com/index.php/tools/activated_packages -> 200 len=43173

## 2026-09-11 13:43:06 UTC
https://agriculture.basf.com/graphql2 -> 200 len=335626
https://agriculture.basf.com/.restful/content -> HTTP 404
https://agriculture.basf.com/.rest/content -> HTTP 404
https://agriculture.basf.com/adminCentral -> 200 len=335626
https://agriculture.basf.com/.cache -> 200 len=335626
https://agriculture.basf.com/.imaging -> 200 len=335626
https://agriculture.basf.com/dam -> 200 len=335626
https://north-america.intranet.basf.com/index.php/ccm/system/block/types -> 200 len=43173
https://north-america.intranet.basf.com/index.php/ccm/system/page/types -> 200 len=43176
https://north-america.intranet.basf.com/index.php/dashboard -> 200 len=43206
https://north-america.intranet.basf.com/ccm/system/block/types -> 200 len=43191
https://north-america.intranet.basf.com/api/blocks -> 200 len=43203

## 2026-09-11 17:24:07 UTC
https://agriculture.basf.com/.graphql -> 200 len=348163
https://agriculture.basf.com/admin -> 200 len=348163
https://agriculture.basf.com/.admin -> 200 len=348163
https://north-america.intranet.basf.com/ccm/system/block/types -> 200 len=43277
https://agriculture.basf.com/graphql2 -> 200 len=348163
https://agriculture.basf.com/.restful -> 200 len=348163
https://agriculture.basf.com/docurl/ -> 200 len=404935
https://north-america.intranet.basf.com/index.php/login -> 200 len=?
https://repfinder.basf.com/bin/repfinder.json -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.search.json -> HTTP 404
https://north-america.intranet.basf.com/index.php/api -> 200 len=43206
https://north-america.intranet.basf.com/index.php/tools/activated_packages -> 200 len=43280

## 2026-09-11 19:55:22 UTC
https://my.basf.com/.auth -> 200 len=204991
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid%20profile%20refresh_token&acr_values=3IAM%2FLogin%2FExternal -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/users/me%00 -> 200 len=246

## 2026-09-11 22:30:27 UTC
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/users/me%00 -> 200 len=246
https://my.basf.com/.auth -> 200 len=204991
https://federation.basf.com/nidp/oauth/nam/authz?response_type=code&client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&redirect_uri=https://my.basf.com/.auth&scope=openid%20profile%20refresh_token&acr_values=3IAM%2FLogin%2FExternal -> 200 len=684
https://federation.basf.com/nidp/oauth/nam/token -> HTTP 405
https://agriculture.basf.com/graphql2 -> 200 len=348125
https://agriculture.basf.com/.restful -> 200 len=348127
https://agriculture.basf.com/docurl/ -> 200 len=404933
https://north-america.intranet.basf.com/index.php/login -> 200 len=?
https://repfinder.basf.com/bin/repfinder.json -> HTTP 404
https://repfinder.basf.com/content/repfinder/en.search.json -> HTTP 404
https://north-america.intranet.basf.com/index.php/api -> 200 len=43179

## 2026-09-12 00:44:26 UTC
https://repfinder.basf.com/bin/basf/repfindertool -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?lat=52.52&lon=13.41 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?lat=999&lon=999 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?format=xml -> 200 len=?
https://agriculture.basf.com/graphql2 -> 200 len=348111
https://agriculture.basf.com/.graphql -> 200 len=348111
https://agriculture.basf.com/.restful/node-types -> HTTP 404
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/users/me%00 -> 200 len=246
https://bsh.secsys.basf.com/api/users/me -> 200 len=246
https://secsys-visitor.basf.com/api/users/me -> 200 len=246

## 2026-09-12 05:23:03 UTC
https://repfinder.basf.com/bin/basf/repfindertool -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?lat=52.52&lon=13.41 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?lat=999&lon=999 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?format=xml -> 200 len=?
https://north-america.intranet.basf.com/index.php/api -> 200 len=43206
https://north-america.intranet.basf.com/index.php/tools/activated_packages -> 200 len=43203
https://north-america.intranet.basf.com/ccm/system/block/types -> 200 len=43173
https://north-america.intranet.basf.com/api/blocks -> 200 len=43182
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/users/me%00 -> 200 len=246
https://bsh.secsys.basf.com/api/users/me -> 200 len=246
https://secsys-visitor.basf.com/api/users/me -> 200 len=246

## 2026-09-12 09:31:23 UTC
https://repfinder.basf.com/bin/basf/repfindertool -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?country=US&productServiceId=1&lat=41.8781&lng=-87.6298&distance=250&limitResults=10&repType=1 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?lat=999&lon=999 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?format=xml -> 200 len=?
https://north-america.intranet.basf.com/index.php/api -> 200 len=43176
https://north-america.intranet.basf.com/index.php/tools/activated_packages -> 200 len=43194
https://north-america.intranet.basf.com/ccm/system/block/types -> 200 len=43176
https://north-america.intranet.basf.com/api/blocks -> 200 len=43283
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/users/me%00 -> 200 len=246
https://bsh.secsys.basf.com/api/users/me -> 200 len=246
https://secsys-visitor.basf.com/api/users/me -> 200 len=246

## 2026-09-12 13:15:40 UTC
https://repfinder.basf.com/bin/basf/repfindertool -> 200 len=80
https://repfinder.basf.com/bin/basf/repfindertool?country=US&productServiceId=1&lat=41.8781&lng=-87.6298&distance=250&limitResults=10&repType=1 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?repType=BR&productServiceId=2&country=DE -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?lat=999&lon=999 -> 200 len=?
https://repfinder.basf.com/bin/basf/retailfindertool?country=US&productServiceId=1&lat=41.8781&lon=-87.6298&distance=250&limitResults=10&repType=1 -> HTTP 404
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/users/me%00 -> 200 len=246
https://bsh.secsys.basf.com/api/users/me -> 200 len=246
https://secsys-visitor.basf.com/api/users/me -> 200 len=246
https://my.basf.com/.auth` -> 200 len=204906
https://my.basf.com/ -> 200 len=204906
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920

## 2026-09-12 16:28:13 UTC
https://repfinder.basf.com/bin/basf/repfindertool -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?country=US&productServiceId=1&lat=41.8781&lng=-87.6298&distance=250&limitResults=10&repType=1 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?repType=BR&productServiceId=2&country=DE -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?lat=999&lon=999 -> 200 len=?
https://repfinder.basf.com/bin/basf/retailfindertool?country=US&productServiceId=1&lat=41.8781&lon=-87.6298&distance=250&limitResults=10&repType=1 -> HTTP 404
https://my.basf.com/.auth` -> 200 len=204916
https://my.basf.com/ -> 200 len=204916
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
https://federation.basf.com/nidp/oauth/nam/authorize?client_id=86cc4bf9-cfdf-4215-bd7c-e9fbbbe626d4&response_type=code&redirect_uri=https%3A%2F%2Fmy.basf.com%2F.auth&scope=openid%20profile%20refresh_token&acr_values=3IAM%2FLogin%2FExternal -> HTTP 404
https://my.basf.com/.auth?code=INTERCEPTED -> 200 len=204993
https://secsys.basf.com/api/users/me -> 200 len=246
https://secsys.basf.com/api/users/me%00 -> 200 len=246

## 2026-09-12 18:51:28 UTC
https://repfinder.basf.com/bin/basf/repfindertool -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?country=US&productServiceId=1&lat=41.8781&lng=-87.6298&distance=250&limitResults=10&repType=1 -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?repType=BR&productServiceId=2&country=DE -> 200 len=?
https://repfinder.basf.com/bin/basf/repfindertool?lat=999&lon=999 -> 200 len=?
https://repfinder.basf.com/bin/basf/retailfindertool?country=US&productServiceId=1&lat=41.8781&lon=-87.6298&distance=250&limitResults=10&repType=1 -> HTTP 404
https://ap-eupf.api.basf.com/api/health?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01 -> HTTP 403
https://ap-eupf.api.basf.com/api/HttpTrigger1?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01 -> HTTP 403
https://ap-eupf.api.basf.com/ -> 200 len=150093
https://ap-eupf.api.basf.com/api/?url=http://169.254.169.254/metadata/instance?api-version=2021-02-01 -> HTTP 403
https://my.basf.com/.auth` -> 200 len=204906
https://my.basf.com/ -> 200 len=204916
https://federation.basf.com/nidp/.well-known/openid-configuration -> 200 len=14920
