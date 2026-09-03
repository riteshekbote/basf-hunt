# Deeper findings — session 2026-09-03

_Generated 2026-09-03 | read-only passive enumeration | no active testing_

## 1. alfaview — OpenAPI spec exposed (HIGH VALUE)

**Location:** `https://apis.alfaview.com/v2/docs/openapi.yaml` (159KB)
**Discovery:** Stoplight Elements UI at `https://apis.alfaview.com/docs` loads the spec via `apiDescriptionUrl="/v2/docs/openapi.yaml"`.

### Endpoints (35+)

**Authentication:**
- `/v2/auth/api-key` — API key auth (companyId + clientId + key)
- `/v2/auth/password` — Password auth
- `/v2/auth/group-link` — Guest auth via group link (accessKey)
- `/v2/auth/guest-link` — Guest auth via guest link
- `/v2/auth/token-info` — Token introspection

**Rooms & Meetings:**
- `/v2/rooms` — List rooms
- `/v2/rooms/{id}` — Get room
- `/v2/rooms/{roomId}/participants` — List participants
- `/v2/rooms/{roomId}/permissions` — Room permissions
- `/v2/rooms/{roomId}/permissions/{userId}` — Per-user permissions (IDOR potential)
- `/v2/rooms/{roomId}/passcode` — Room passcode
- `/v2/rooms/{roomId}/features` — Room features
- `/v2/rooms/{roomId}/group-links` / `guest-links` — Guest access management
- `/v2/rooms/{roomId}/file-share-settings` / `file-share-limits`
- `/v2/rooms/{roomId}/subrooms` — Subroom management
- `/v2/meetings` — List/Create meetings
- `/v2/meetings/{id}` — Get/Update meeting
- `/v2/meetings/{id}/cancellation` — Cancel meeting

**Users & Admin:**
- `/v2/users` — List users
- `/v2/users/{id}` — Get user
- `/v2/users/me` — Current user
- `/v2/users/invitation` — Invite user
- `/v2/permission-groups` — Permission groups
- `/v2/room-types` — Room types
- `/v2/stats` — Statistics
- `/v2/languages` — Available languages

### Auth model
- API keys: `companyId` + `clientId` + `key` (in request body)
- Bearer tokens: base64-encoded `accessToken` in `Authorization` header
- Guest access: `accessKey` from group-link or guest-link

### Attack-relevant observations
- `/v2/rooms/{roomId}/permissions/{userId}` — classic IDOR pattern (room ID + user ID in path)
- Guest auth via `accessKey` — if keys are predictable/brute-forceable, guest impersonation possible
- `/v2/users/invitation` — user invitation endpoint (potential user enumeration or abuse)
- `/v2/stats` — statistics endpoint (potential info disclosure)
- `/v2/rooms/{roomId}/passcode` — passcode management (potential bypass)

### Additional alfaview observations
- All hosts behind `edge-proxy` (reverse proxy fingerprint)
- CSP on `app.alfaview.com` allows `omega-lectures.com` as frame-ancestor (third-party integration)
- No X-Frame-Options on ANY alfaview host (potential clickjacking, mitigated by CSP frame-ancestors on app)
- `internal.alfaview.com` and `beta-app.alfaview.com` use HTTP Basic auth

---

## 2. BASF — Azure Function Apps exposed (MEDIUM VALUE)

**Hosts:** `ap-digitalconnect.api.basf.com`, `ap-eupf.api.basf.com`
**Fingerprint:** Azure Function Apps (default page: "Your Azure Function App is up and running")

### Exposed admin endpoints
- `/admin/functions` → 401 (Bearer auth required)
- `/admin/host/status` → 401 (Bearer auth required)
- `/admin/host/keys` → 401 (Bearer auth required)
- `/.env` → 403 (exists, blocked)

### Attack surface
- Azure Functions admin API requires function master key or system key
- If key is weak/default/leaked, full admin access to Function App
- Common Azure Functions key patterns: `<hex>-<hex>`, default keys in `host.json`
- Dev endpoints (`dev-clientcert-sap`, `dev-ext001`) require client certificates (400)

---

## 3. daimlertruck — Lockdown confirmed (NO UNAUTH FINDING)

- APIM gateways: all paths return `OperationNotFound` (no unauth surface)
- Developer portals: Next.js, login-gated (307 → callbackUrl)
- Authz services: empty 404 at root
- Management/capacitor hosts: unreachable (NAT64/IPv6 only)

**Honest assessment: surface is properly locked down against unauthenticated attackers.**

---

## 4. betpanda — Vite SPA with affiliate portal

- `affiliates.betpanda.io` — Vite-built SPA (main.1ae50aab.js)
- Cloudflare-fronted
- No API routes found in JS bundle
- Standard affiliate portal structure

---

## 5. elringklinger — Third-party integrations

- `go.events.elringklinger.com` — 302 redirect (event management platform)
- `ir.elringklinger.com` — Apache, investor relations page

---

## 6. avatarux — Atlassian + cPanel

- `help.desk.avatarux.com` — Atlassian Edge (Jira/Confluence help desk)
- `autoconfig.avatarux.com` — Apache autoconfig
- `cpanel.avatarux.com` — cPanel (if accessible)

---

_Honest framing: all observations from read-only passive enumeration. No vulnerability claimed. Scope must be confirmed before active testing on any of these live production assets._
# Deeper finding re-verification — 2026-09-03

_Appended to DEEPER-FINDINGS.md | read-only re-check, no active testing_

## Re-verification results

### alfaview API — all endpoints auth-gated (CONFIRMED, no unauth vuln)

Live checks against `https://apis.alfaview.com`:
- `/v2/languages` → 401 "No access token was provided in the Authorization header."
- `/v2/room-types` → 401 (same)
- `/v2/users/me` → 401 (same)
- `/v2/rooms` → 401 (same)
- `/v2/auth/token-info` → 422 "invalid access token format" — **endpoint works and validates token format**; could act as an oracle distinguishing valid vs malformed tokens, but no unauth info disclosure
- `/v2/stats` → 422 "validation failed" (requires query params)

**Conclusion:** The alfaview API is properly auth-gated. No unauthenticated vulnerability. The exposed OpenAPI spec is a **roadmap for authenticated testing** — the IDOR-shaped operations (`/v2/rooms/{roomId}/permissions/{userId}`, `DELETE /v2/users/{id}`, `/v2/users/invitation`) only become testable with a valid access token obtained via one of the 4 auth methods.

### Auth model (documented in spec, verified structure)
- All requests require `Authorization: Bearer <base64 accessToken>`
- Tokens valid for 1 hour
- Auth methods: API-key (companyId+clientId+key), password (username+password), group-link (companyId+roomId+accessKey), guest-link (companyId+accessKey)
- Group/guest-link auth accepts only a companyId + roomId + accessKey — **potential guest-auth brute force IF access keys are low-entropy**, but companyId+roomId are long random IDs raising the bar. Not exploitable without scope-authorized testing.

### BASF Azure Functions — admin API exposed, Bearer-auth-gated (CONFIRMED)
- `/admin/host/status` → 401 with `WWW-Authenticate: Bearer`
- `/admin/functions` → 401 Bearer
- `/admin/host/keys` → 401 Bearer
- `/.env` → 403 (blocked)
- Requires Azure Functions master/system key. No default/weak key confirmed (no auth testing done). This is a legitimate **documented-observation**: the admin endpoints are reachable but properly key-authenticated.

### daimlertruck developer portal — login-gated (CONFIRMED still)
- `developer.eu.api.daimlertruck.com/api` → 307 to `/?callbackUrl=%2Fapi` (login gate intact)

## Overall honest conclusion

Each deeper dig confirmed the same reality: **the 6 live targets are all auth-gated at the application/API layer.** The genuinely valuable output of this session:

1. **alfaview OpenAPI spec** (159KB, 60 operations) recovered and stored — the single highest-value artifact; enables targeted authenticated testing later.
2. **BASF Azure Functions admin surface** documented — reachable admin API requiring keys.
3. Honest negative results recorded for daimlertruck and the 22 subdomain-thin targets.

No unauthenticated compromise was achieved, and per triage discipline **no vulnerability is claimed.** The path to real findings is **authenticated testing on alfaview** (obtain a legitimate developer account + written scope confirmation).
