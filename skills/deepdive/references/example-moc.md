# Example MOC Proposal

This is a concrete example of what a good Map of Content proposal looks like. It uses IAM/OAuth as the topic — the same topic as the original vault. Use this as a reference for the level of detail expected in the Phase 0 MOC.

---

## Topic Analysis: OAuth 2.0, OIDC, SAML, and IAM with Keycloak

**Fictional Company:** Acme Books — an online bookstore founded in 2018. Grows from 50 employees to 500+ across 10 evolution phases.

**Primary Tool:** Keycloak (open-source IAM platform)

**Tags:** oauth, oidc, saml, iam, keycloak, security

## Proposed Vault Structure

```
oauth-iam-vault/
├── 00-Meta/
│   ├── Glossary.md                                      (beginner)
│   ├── Cheat-Sheets.md                                  (beginner)
│   ├── Learning-Path.md                                 (beginner)
│   ├── Interview-Prep.md                                (intermediate)
│   ├── Troubleshooting-Guide.md                          (intermediate)
│   ├── Architecture-Patterns.md                          (advanced)
│   └── Resources.md                                     (beginner)
├── 01-Foundations/
│   ├── Identity-vs-Authentication-vs-Authorization.md    (beginner)
│   ├── Sessions-Cookies-Tokens.md                        (beginner)
│   ├── Federation.md                                     (intermediate)
│   ├── Trust-Models.md                                   (intermediate)
│   ├── Authentication-Factors.md                         (beginner)
│   └── Cryptography-Basics.md                            (beginner)
├── 02-OAuth2/
│   ├── OAuth2-Overview.md          ← HUB                (beginner)
│   ├── OAuth2-Roles.md                                   (beginner)
│   ├── OAuth2-Scopes.md                                  (beginner)
│   ├── OAuth2-Tokens.md                                  (beginner)
│   ├── OAuth2-Authorization-Code-Flow.md                 (beginner)
│   ├── OAuth2-PKCE.md                                    (intermediate)
│   ├── OAuth2-Client-Credentials.md                      (intermediate)
│   ├── OAuth2-Device-Flow.md                             (intermediate)
│   ├── OAuth2-Token-Exchange.md                          (advanced)
│   ├── OAuth2-Refresh-Tokens.md                          (beginner)
│   └── OAuth2-Security.md                                (intermediate)
├── 03-OIDC/
│   ├── OIDC-Overview.md            ← HUB                (beginner)
│   ├── OIDC-ID-Token.md                                  (beginner)
│   ├── OIDC-Claims.md                                    (beginner)
│   ├── OIDC-UserInfo.md                                  (beginner)
│   ├── OIDC-Discovery.md                                 (intermediate)
│   ├── OIDC-Logout.md                                    (intermediate)
│   └── OIDC-Session-Management.md                        (advanced)
├── 04-JWT/
│   ├── JWT-Fundamentals.md         ← HUB                (beginner)
│   ├── JWT-Structure.md                                  (beginner)
│   ├── JWT-Signing.md                                    (intermediate)
│   ├── JWT-Validation.md                                 (intermediate)
│   └── JWT-Security.md                                   (advanced)
├── 05-SAML/
│   ├── SAML-Overview.md            ← HUB                (intermediate)
│   ├── SAML-Assertions.md                                (intermediate)
│   ├── SAML-IdP-vs-SP.md                                 (beginner)
│   ├── SAML-Metadata.md                                  (intermediate)
│   └── SAML-Enterprise-SSO.md                            (intermediate)
├── 06-Keycloak/
│   ├── Keycloak-Core-Concepts.md    ← HUB                (beginner)
│   ├── Keycloak-Realms.md                                (beginner)
│   ├── Keycloak-Clients.md                               (beginner)
│   ├── Keycloak-Roles.md                                 (beginner)
│   ├── Keycloak-Mappers.md                               (intermediate)
│   ├── Keycloak-Identity-Providers.md                    (intermediate)
│   ├── Keycloak-Authentication-Flows.md                  (intermediate)
│   ├── Keycloak-User-Federation.md                       (intermediate)
│   └── Keycloak-Admin-REST-API.md                        (advanced)
├── 07-Security/
│   ├── CSRF-XSS-Replay-Attacks.md                        (intermediate)
│   ├── Token-Theft.md                                    (intermediate)
│   ├── MFA.md                                            (beginner)
│   ├── Zero-Trust.md                                     (advanced)
│   └── Secure-Token-Storage.md                           (intermediate)
├── 08-Integration/
│   ├── Spring-Boot-Resource-Server.md                    (intermediate)
│   ├── Spring-Boot-OAuth-Client.md                       (intermediate)
│   ├── React-Frontend-Login.md                           (intermediate)
│   └── Microservices-Authentication.md                   (advanced)
├── 09-Enterprise-IAM/
│   ├── Federation-Patterns.md                            (advanced)
│   ├── Multi-Tenancy.md                                  (advanced)
│   ├── B2B-SSO.md                                        (advanced)
│   └── Identity-Brokering.md                             (advanced)
└── 10-Case-Study/
    ├── Acme-Books-Overview.md                            (beginner)
    ├── Acme-Books-Phase-1-Simple-Login.md                (beginner)
    ├── Acme-Books-Phase-2-API.md                         (beginner)
    ├── Acme-Books-Phase-3-Mobile.md                      (intermediate)
    ├── Acme-Books-Phase-4-Microservices.md               (advanced)
    ├── Acme-Books-Phase-5-Third-Party.md                 (intermediate)
    ├── Acme-Books-Phase-6-Enterprise-SSO.md              (intermediate)
    ├── Acme-Books-Phase-7-SAML.md                        (intermediate)
    ├── Acme-Books-Phase-8-Multi-Tenant.md                (advanced)
    ├── Acme-Books-Phase-9-MFA.md                         (intermediate)
    └── Acme-Books-Phase-10-Architecture.md               (advanced)
```

**Total:** 74 files

## Case Study Phases

| Phase | Title | Business Trigger | Key Concept |
|-------|-------|-----------------|-------------|
| 1 | Simple Login | First web app needs user accounts | OIDC Authorization Code Flow |
| 2 | REST API | Frontend needs async data loading | Bearer token validation |
| 3 | Mobile App | iOS/Android apps launched | PKCE for public clients |
| 4 | Microservices | Monolith split into 3 services | Token Exchange + Client Credentials |
| 5 | Third-Party APIs | Partner bookstores want inventory access | Scoped OAuth clients |
| 6 | Enterprise SSO | Penguin Publishing wants corporate login | OIDC identity brokering |
| 7 | SAML Federation | HarperCollins uses legacy ADFS | SAML-to-OIDC protocol bridge |
| 8 | Multi-Tenant SaaS | 50+ enterprise customers need isolation | Realm-per-tenant architecture |
| 9 | Adaptive MFA | Credential stuffing attack on customers | Conditional auth flows |
| 10 | Zero Trust at Scale | 200+ microservices, global deployment | Clustered Keycloak + DPoP + mTLS |

## Real-World Incidents Referenced

- Credential stuffing attack on a major retailer (Phase 9 trigger)
- JWT `alg:none` vulnerability (CVE-2018-0114) — referenced in JWT-Security
- Refresh token theft via mobile malware — referenced in Token-Theft
- SAML XML Signature Wrapping attacks (CVE-2017-11427) — referenced in SAML-Assertions

---

**This is the level of detail expected.** Every file is named. Every difficulty level is assigned. Every case study phase has a clear business trigger and concept mapping. The total file count is stated.
