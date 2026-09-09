# :material-history: Changelog

DENA documentation version history.

---

## v0.5.2 <small>— 2026-09-07</small> { #v052 }

!!! success "Current"

**Documentation synchronised with code revision 0.4.16 of the DENA common API.**

New content:

- :material-plus: `arquitectura/arquitectura-servicios.md`: documented the client REST proxy base (`DN00ClientAPIRESTServiceProxyBase`) and the CORE proxy marker interface (`DN00IsDENACOREServiceProxy`), with their behaviour (JSON marshalling, configurable retries)
- :material-plus: New security context user model page (`seguridad/modelo-usuarios.md`): documented the new administration system user (`DN00DENAAdminSystemUser`) alongside the person and management users

Fixes against code 0.4.16:

- :material-refresh: Refs model (object-ref, person-ref, org-admin-ref, data-type-ref): actual fields `oid`/`id`/`dir3Id`; removed non-existent fields (`objectOid`, `createTS`, `lastUpdateTS`, `deleteTS`, `url`, `personId`, `orgId`, `officialId`)
- :material-refresh: Interop message model: actual structure `context` + `protocol` + `payload`; `context.message.{type,correlationId,interopRouteData}`; derived `flowDirection`; simplification (removed `__DN00IsInteropMessagePayload`)
- :material-refresh: Data-retrieve data model: `originAdmin`/`aboutPerson` (previously `*Ref`), added `lastChangedAt`
- :material-refresh: Base semantics: `status` (code/errorId/details), `consentOid`, `sync-types` (`DN00SyncMetaDataFromAdminToCOREItem`), http-headers and language-texts aligned with the code
- :material-refresh: Code links and JSON examples (ES/EN/EU) updated to the actual model
- :material-refresh: `semantica/data-retrieve/data/pago.md`: added "Format schema (`schema`)" section with the 14 supported formats and a notice that DC validation is not implemented
- :material-refresh: `semantica/data-retrieve/data/persona.md`: fixed the example `entity` to `CAIXESBB` (valid BIC)
- :material-refresh: `semantica/data-retrieve/data/notificacion.md`: clarified the `type` field (enum `OFFICIAL_NOTICE`/`COMMUNICATION`, `DN00AdministrativeNoticeType`) versus the polymorphic discriminator `administrativeNotice`

Translations:

- :material-translate: Translated to EN and EU the pages that only existed in ES (arquitectura-servicios, configuracion, tipos-dato-base, data-retrieve/index)

---

## v0.5.1 <small>— 2026-09-08</small> { #v051 }

**Complete Person-Sync documentation extracted from DENA-Architecture.docx and DENA-CORE-Services_for_admins.docx.**

New architecture content:

- :material-plus: "Person-Sync" section expanded with Push, Pull On-line and Pull Off-line
- :material-plus: Mermaid flow diagrams for Bespoke job flow
- :material-plus: JSON payload examples (Push, Pull On-line, Bespoke requests)
- :material-plus: Documented job states: REGISTERED, BEING_PROCESSED, FINISHED_OK, FINISHED_NOK
- :material-plus: Java API examples for arquitectura-servicios.md
- :material-plus: Images extracted from Word: image18.png (Person-Sync Overview), image7.png (DENA Push)

Translations:

- :material-translate: arquitectura/index.md: full content translated to EN and EU
- :material-translate: arquitectura/arquitectura-servicios.md: Java examples translated

Cleanup:

- :material-minus: Removed image8.png from Admin Pull (did not match the context)
- :material-minus: Temporary file arquitectura-dena-completa.md consolidated into arquitectura/index.md

---

## v0.5.0 <small>— 2026-08-24</small> { #v050 }

**Comprehensive documentation improvement: structure, content, consistency and translations.**

Inconsistencies fixed:

- :material-bug: "More information" section tripled in arquitectura/index.md
- :material-bug: `REGISTRY` corrected to `REGISTER` (canonical enum value) in 6 files
- :material-bug: Incorrect colour legend in semantica/index (violet → yellow)
- :material-bug: Obsolete date in sistema-versionado.md
- :material-bug: TRANSLATION_TRACKER with contradictory counters

New content:

- :material-plus: Complete End-to-End example (token + SRMD + Data-Retrieve + bash script)
- :material-plus: API limits and restrictions page (documented timeouts)
- :material-plus: Java Spring Boot code examples (controller, entity, metadata-sync service, token service)
- :material-plus: Glossary expanded with 6 terms: Cold-Start, Connector, Data Origin Instance, DENA-APP, DENA-CORE, SRMD

Security and authentication restructuring:

- :material-refresh: "Your System Calls DENA" integrates get-token + security headers (field generation)
- :material-refresh: "DENA Calls Your System" integrates JWT model + services + alternative mechanisms (OAuth, mTLS, CAS, API Key, Basic Auth, WS-Security)
- :material-minus: Removed modelo.md, servicios.md and endpoint-get-token.md (integrated into main pages)

Improvements to existing content:

- :material-refresh: Data-Retrieve: example JSON with complete interop structure (collapsible)
- :material-refresh: Data-Retrieve: "Reference code" section with links to tests and DN00InteropHeaders
- :material-refresh: Metadata-Sync: clarification of "what changed and when" (not who)
- :material-refresh: Metadata-Sync: documentation of fromDataOrigin, IDs vs OIDs, classic data origin error
- :material-refresh: ES/EN/EU index unified with identical narrative structure

Inclusive language:

- :material-refresh: Full filtering: "user" → "person", "citizen" → "citizen person/citizenship"
- :material-refresh: Applied in ES, EN and EU (citizen → person, herritarra → pertsona)

Cleanup:

- :material-minus: Empty ADRs removed (0001, 0002)
- :material-minus: Unpublished legacy files removed (vision-general-detallada, seguridad, servicios-core-admins)
- :material-minus: Obsolete translations removed and regenerated

Translations:

- :material-translate: New or updated EN+EU translations for all modified pages
- :material-translate: operativas/data-retrieve, metadata-sync, person-sync (EN+EU)
- :material-translate: autenticacion/administracion-core-dena, core-dena-administracion (EN+EU)
- :material-translate: ejemplo-end-to-end, limites-api (EN+EU)

---

## v0.4.0 <small>— 2026-08-24</small> { #v040 }

**Base Semantics v2.0 document integration:**

- :material-plus: HTTP Headers: request/response headers, API versioning, security digests
- :material-plus: Consents: principles, regulatory framework (GDPR), lifecycle, format and API
- :material-plus: DenaObjectRef: base type with OID, timestamps and URL
- :material-plus: DENAProtocol: callback URLs, timeouts, HATEOAS
- :material-plus: DENAConsent: legal basis reference in Data-Retrieve messages
- :material-plus: Status: response codes, DENAClientErrDetails, DENAServerErrDetails, DENAAsyncQueueData
- :material-plus: Message Types: DenaFlowDirection, DenaMessageType, DenaInteropRouteDataItem, DenaPersonAndConsentGiven
- :material-plus: UserAgent: format by origin (mobile app, web, CORE, administration)
- :material-plus: Sync Types: DenaInteroperableDataTypeSync, DenaPersonDataSync, DenaPersonMetaDataSyncItem
- :material-refresh: OrgAdminRef extended with orgId, officialId, objectOid, timestamps, url, organisational structure
- :material-refresh: PersonRef extended with personId, objectOid, timestamps, url
- :material-refresh: tipos-dato-base: added Hash (SHA-256) and UserAgent
- :material-refresh: semantica-base/index: full message structure (context, protocol, consent, status, data)
- :material-refresh: mkdocs.yml: navigation updated with new pages
- :material-translate: EN and EU translations for all new and updated pages (76/76)

---

## v0.3.38 <small>— 2026-07-04</small> { #v0338 }

**Documentation improvements:**

- :material-palette: CSS reorganised into numbered files `01`–`06` by responsibility
- :material-refresh: `extra.css` migrated to numbered files; legacy file emptied
- :material-image: Administration logos added to footer (Ayto. Bilbao, Donosti, Vitoria, DFA, DFB, DFG, EUDEL)
- :material-format-size: Footer logo size increased to 60px
- :material-bug: Fixed missing dark mode variables in `01-variables.css`, `02-material-overrides.css` and `06-mermaid-diagrams.css`
- :material-bug: Mass correction `registry` → `register` in `registro-oficial.md` (es/en/eu)
- :material-web: Manrope typography migrated from local font to Google Fonts

---

## v0.3.32 <small>— 2026-06-26</small> { #v0332 }

**Documentation improvements:**

- :material-palette: Full application of DENA corporate colours (#1D3328)
- :material-image: DENA logo resized to 64px for better visibility
- :material-view-grid: "DENA Operations" entry page with visual navigation
- :material-cogs: Consolidation of operational sections in the main menu
- :material-email: Integration of support contact (admin-digital-data-dena@ejie.eus)
- :material-alert: Repository version notices throughout the documentation
- :material-star: Visual consistency with Material Design icons
- :material-navigation: Simplified navigation without emojis
- :material-help: Dedicated contact and support page

---

## v0.3.31 <small>— 2026-06-25</small> { #v0331 }

**Documentation improvements:**

- :material-refresh: Centralised variables updated in `mkdocs-vars.yaml`
- :material-refresh: All footers migrated to `{{ dena.version }}` and `{{ dena.date }}` variables
- :material-refresh: Version headers in semantic documentation using variables
- :material-plus: Improved maintainability: version changes from a single file
- :material-bug: Fixed versioning inconsistencies in documentation

---

## v0.3.26 <small>— 2026-06-11</small> { #v0326 }

**Documentation improvements:**

- :material-plus: Entry page redesigned with Material for MkDocs
- :material-plus: "Getting started" section with installation, communications and mock
- :material-plus: FAQ (Frequently Asked Questions)
- :material-plus: Glossary of terms
- :material-plus: Custom CSS with DENA corporate colours
- :material-plus: Tooltips on technical acronyms (OID, DIR3, SIA...)
- :material-plus: Reusable variables (`mkdocs-vars.yaml`)
- :material-refresh: All pages updated with Material features (admonitions, tabs, mermaid, grid cards)
- :material-refresh: Navigation reorganised with tabs and expandable sections

---

## v0.3.25 <small>— 2026-06-10</small> { #v0325 }

**Content:**

- :material-plus: Complete DATA-RETRIEVE documentation (endpoint, objects, validations, errors, snippets)
- :material-plus: PERSON-SYNC documentation (Pull + Push, endpoints, models)
- :material-plus: METADATA-SYNC documentation (endpoint)
- :material-plus: Data-Retrieve implementation guide
- :material-plus: Interactive mermaid diagrams in semantics
- :material-plus: Updated Postman collections

---

## v0.2.0 <small>— 2026-03-15</small> { #v020 }

**Content:**

- :material-plus: Authentication documentation (Client ↔ DENA, Admin ↔ DENA)
- :material-plus: OAuth2 flow diagrams
- :material-plus: get-token endpoint
- :material-plus: Base semantics (REST Message structure)

---

## v0.1.0 <small>— 2025-12-01</small> { #v010 }

**Content:**

- :material-plus: Initial repository structure
- :material-plus: General architecture (draw.io diagram)
- :material-plus: First data models (DataTypeRef, PersonRef, OrgAdminRef)
- :material-plus: DevTools - DENA Admin Connection Test

---

## Version convention

| Format | Meaning |
|---|---|
| `v0.X.Y` | Pre-release development phase |
| :material-plus: | New content |
| :material-refresh: | Updated content |
| :material-minus: | Removed content |
| :material-bug: | Bug fix |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
