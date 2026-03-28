```
sequenceDiagram
  autonumber
  participant UA as Browser / Mobile App
  participant FE as Frontend / BFF
  participant CIAM as CIAM / Identity Broker
  participant EIDP as External IdP
  participant API as Application API
  participant EXT as External Platform APIs

  UA->>FE: GET /auth/start?tenant=<tenant>
  FE-->>UA: 302 → CIAM /authorize (policy=SignIn_Router, domain_hint=<tenant>, state, nonce)
  UA->>CIAM: authorize
  CIAM->>EIDP: federation (redirect)
  EIDP-->>CIAM: assertion
  CIAM-->>UA: 302 → FE /auth/callback?code=...&state=...
  UA->>FE: /auth/callback
  FE->>CIAM: /token exchange
  CIAM-->>FE: id_token (+ refresh token if used)
  FE->>FE: map claims → {sub, tenant_id, roles}
  FE->>API: upsert user via User Provisioning Service
  API-->>FE: user linked / provisioned
  FE-->>UA: Set-Cookie session=... (HttpOnly, Secure, SameSite=Lax), 302 app route

  UA->>FE: /api/visits (cookie)
  FE->>FE: mint short-lived App API JWT with {sub, tenant_id, roles}
  FE->>API: GET /visits Authorization: Bearer <App API JWT>
  API->>EXT: acquire/cache downstream token as needed
  EXT-->>API: data
  API-->>FE: data
  FE-->>UA: data
```
