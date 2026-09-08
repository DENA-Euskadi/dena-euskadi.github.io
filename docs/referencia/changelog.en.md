# :material-history: Changelog

DENA documentation version history.

---

## v0.4.0 <small>— 2026-08-24</small> { #v040 }

!!! success "Current"

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

- :material-palette: CSS reorganised into numbered files `01`–06 by responsibility
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
