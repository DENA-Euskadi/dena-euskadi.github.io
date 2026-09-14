# :material-history: Changelog

Historial de versiones de la documentación DENA.

---

## v0.5.2 <small>— 2026-09-07</small> { #v052 }

!!! success "Actual"

**Sincronización de la documentación con la revisión de código 0.4.16 del API común DENA.**

Contenido nuevo:

- :material-plus: `arquitectura/arquitectura-servicios.md`: documentado el proxy REST base del cliente (`DN00ClientAPIRESTServiceProxyBase`) y la interfaz de marca de proxies CORE (`DN00IsDENACOREServiceProxy`), con su comportamiento (marshalling JSON, reintentos configurables)
- :material-plus: Nueva página del modelo de usuarios del security context (`seguridad/modelo-usuarios.md`): documentado el nuevo usuario de sistema de administración (`DN00DENAAdminSystemUser`) junto a los usuarios de persona y de gestión

Correcciones frente al código 0.4.16:

- :material-refresh: Modelo de refs (object-ref, person-ref, org-admin-ref, data-type-ref): campos reales `oid`/`id`/`dir3Id`; eliminados campos inexistentes (`objectOid`, `createTS`, `lastUpdateTS`, `deleteTS`, `url`, `personId`, `orgId`, `officialId`)
- :material-refresh: Modelo de interop message: estructura real `context` + `protocol` + `payload`; `context.message.{type,correlationId,interopRouteData}`; `flowDirection` derivado; simplificación (eliminada `__DN00IsInteropMessagePayload`)
- :material-refresh: Modelo de datos data-retrieve: `originAdmin`/`aboutPerson` (antes `*Ref`), añadido `lastChangedAt`
- :material-refresh: Semántica base: `status` (code/errorId/details), `consentOid`, `sync-types` (`DN00SyncMetaDataFromAdminToCOREItem`), http-headers y language-texts alineados con el código
- :material-refresh: Enlaces de código y ejemplos JSON (ES/EN/EU) actualizados al modelo real
- :material-refresh: `semantica/data-retrieve/data/pago.md`: añadida sección "Esquema del formato (`schema`)" con los 14 formatos soportados y aviso de validación DC no implementada
- :material-refresh: `semantica/data-retrieve/data/persona.md`: corregido el `entity` de ejemplo a `CAIXESBB` (BIC válido)
- :material-refresh: `semantica/data-retrieve/data/notificacion.md`: aclarado el campo `type` (enum `OFFICIAL_NOTICE`/`COMMUNICATION`, `DN00AdministrativeNoticeType`) frente al discriminador polimórfico `administrativeNotice`
- :material-refresh: `semantica/semantica-base/modelo/data-type-ref.md`: reescrita la explicación de `dataType` orientada a administraciones — separadas las tres piezas (`id`=`DN00DataTypeID`, `oid`=`DN00DataTypeOID`, contenedor `DN00DataTypeRef`) frente al catálogo `DN00DataTypeEnum`, dejando claro que basta con leer `dataType.id`
- :material-refresh: Corregidos ejemplos JSON donde el `oid` de `dataType` repetía el valor del `id` (ahora un GUID de ejemplo), evitando confusión

Correcciones de contenido (ejemplos verificados campo a campo contra el código 0.4.16):

- :material-bug: `pago.md`: valores del enum de frecuencia de domiciliación corregidos a los reales (`DAILY`/`WEEKLY`/`MONTHLY`/`QUARTERLY`/`YEARLY`, `r01f.types.datetime.TimeFrequency`); eliminados valores inexistentes (`BIWEEKLY`/`BIMONTHLY`/`BIANNUAL`/`ANNUAL`)
- :material-bug: `person-sync` (person-hashes, endpoint-person-push-to-admin): campo `allNamesHash` → `fullNameHash` (`DN00PersonHashes`)
- :material-bug: `metadata-sync` (endpoint-sync-metadata): tipo de mensaje `ADMIN_SYNC_METADATA` → `ADMIN_SRMD_SYNC_REQ`/`ADMIN_SRMD_SYNC_RESP` (`DN00InteropMessageType`)
- :material-bug: Campo `fromDataOrigin` → `fromDataOriginInstance` en ejemplos de SRMD (`DN00SyncMetaDataFromAdminToCOREItem`): metadata-sync, ejemplo-end-to-end, arquitectura, ejemplos-código, autenticación
- :material-bug: `denaComponentId` con valores inexistentes (`apiGateway`, `DENA_POSTMAN`, `DENA_INTEROP_ADMIN_SYNC`) corregidos a valores reales del enum `DN00InteropComponent` (`CLIENT_INSTALLMENT`/`DENA_CORE`/`DENA_ADMIN_CONNECTOR`/`ADMIN`) en endpoints de data-retrieve, metadata-sync y person-sync
- :material-bug: `endpoint-person-push-to-admin.md`: reescrito el modelo del push a la estructura real (`DN00PersonSyncPushToAdminFromCOREToConnectorInternalSide` → `notification` con `syncData` y `person`); eliminado el envelope interop y el tipo `PERSON_PUSH_TO_ADMIN` (inexistente en 0.4.16); respuesta descrita por código HTTP según `DN01PersonPushToAdminJobProcessor`
- :material-plus: Cada ficha de tipo de dato (expediente, notificación, registro-oficial, pago, cita, persona) indica ahora en su cabecera el ID de Tipo (DataTypeID) correspondiente, con el valor del enum `DN00DataTypeEnum`

Verificación:

- :material-check-all: Verificados los 92 enlaces a código fuente (`{{ repos.*_blob }}`) contra el tag `PUBLIC-v0.4.16` de los mirrors públicos: todos resuelven correctamente
- :material-check-all: Contrastados campo a campo los ejemplos JSON/Java y tablas de todas las páginas de semántica, endpoints, metadata-sync y person-sync contra las clases reales 0.4.16
- :material-bug: `endpoint-sync-metadata.md` (SRMD): el `payload` de la petición es un **array** directo de items (`Collection<DN00SyncMetaDataFromAdminToCOREItem>`); eliminado el envoltorio `payload.items`. Respuesta: `code`/`errorId`/`details` a nivel raíz y `payload` = `DN00SyncMetaDataFromAdminCOREProcessingInfo` directo (transactionOid/receivedItemsCount/processedOK/processedNOK), sin el envoltorio `processingInfo`
- :material-bug: Ejemplos de código alineados al formato real de RETRIEVE (snippets-codigo en 5 lenguajes, guia-implementacion, ejemplos-codigo, ejemplo-end-to-end, operativas/data-retrieve, troubleshooting, errores-troubleshooting): petición reducida (`context` con `subjectPerson`/`dataType`/`administration`) y respuesta con `code` a nivel raíz y `dataItems[].data`
- :material-bug: `endpoint-data-retrieve.md` (RETRIEVE): reescrita la petición al formato real que el conector (Spring Boot 3) envía a la administración — `context` con `subjectPerson.id`, `dataType.id` y `administration.id` — en lugar del envoltorio interop completo (`message`/`protocol`/`payload`) que no llega a la administración. Corregida también la respuesta (`DN00DataRetrieveResponseFromAdmin`): cada elemento de `dataItems` envuelve el objeto en un campo `data`, con `proposedScheduleItems` e `itemsPagingContext`; `code` a nivel raíz. Verificado el pipeline del conector (transformer semántico `standard` = passthrough)

Traducciones:

- :material-translate: Traducidas a EN y EU las páginas que solo existían en ES (arquitectura-servicios, configuracion, tipos-dato-base, data-retrieve/index)

---

## v0.5.1 <small>— 2026-09-08</small> { #v051 }

**Documentación completa de Person-Sync extraída de DENA-Architecture.docx y DENA-CORE-Services_for_admins.docx.**

Contenido nuevo de arquitectura:

- :material-plus: Sección "Person-Sync" ampliada con Push, Pull On-line y Pull Off-line
- :material-plus: Diagramas de flujo Mermaid para Bespoke job flow
- :material-plus: Ejemplos de payloads JSON (Push, Pull On-line, Bespoke requests)
- :material-plus: Estados del job documentados: REGISTERED, BEING_PROCESSED, FINISHED_OK, FINISHED_NOK
- :material-plus: Ejemplos de API Java para arquitectura-servicios.md
- :material-plus: Imagen extraída de Word: image18.png (Person-Sync Overview), image7.png (DENA Push)

Traducciones:

- :material-translate: arquitectura/index.md: contenido completo traducido a EN y EU
- :material-translate: arquitectura/arquitectura-servicios.md: ejemplos Java traducidos

Limpieza:

- :material-minus: Eliminado image8.png de Admin Pull (no correspondía al contexto)
- :material-minus: Fichero temporal архитектура-dena-completa.md consolidado en arquitectura/index.md

---

## v0.5.0 <small>— 2026-08-24</small> { #v050 }

**Mejora integral de documentacion: estructura, contenido, consistencia y traducciones.**

Inconsistencias corregidas:

- :material-bug: Seccion "Mas informacion" triplicada en arquitectura/index.md
- :material-bug: `REGISTRY` corregido a `REGISTER` (valor canonico del enum) en 6 ficheros
- :material-bug: Leyenda de colores incorrecta en semantica/index (violeta → amarillo)
- :material-bug: Fecha obsoleta en sistema-versionado.md
- :material-bug: TRANSLATION_TRACKER con contadores contradictorios

Contenido nuevo:

- :material-plus: Ejemplo End-to-End completo (token + SRMD + Data-Retrieve + script bash)
- :material-plus: Pagina de limites y restricciones de la API (timeouts documentados)
- :material-plus: Ejemplos de codigo Java Spring Boot (controller, entity, metadata-sync service, token service)
- :material-plus: Glosario ampliado con 6 terminos: Cold-Start, Conector, Data Origin Instance, DENA-APP, DENA-CORE, SRMD

Reestructuracion de seguridad y autenticacion:

- :material-refresh: "Tu Sistema Llama a DENA" integra get-token + cabeceras de seguridad (generacion de campos)
- :material-refresh: "DENA Llama a Tu Sistema" integra modelo JWT + servicios + mecanismos alternativos (OAuth, mTLS, CAS, API Key, Basic Auth, WS-Security)
- :material-minus: Eliminados modelo.md, servicios.md y endpoint-get-token.md (integrados en paginas principales)

Mejoras de contenido existente:

- :material-refresh: Data-Retrieve: JSON de ejemplo con estructura interop completa (colapsable)
- :material-refresh: Data-Retrieve: seccion "Codigo de referencia" con enlaces a tests y DN00InteropHeaders
- :material-refresh: Metadata-Sync: matizacion "que cambio y cuando" (no el quien)
- :material-refresh: Metadata-Sync: documentacion de fromDataOrigin, IDs vs OIDs, error clasico data origin
- :material-refresh: index ES/EN/EU unificados con estructura narrativa identica

Lenguaje inclusivo:

- :material-refresh: Filtrado completo: "usuario" → "persona", "ciudadano/a" → "persona ciudadana/ciudadania"
- :material-refresh: Aplicado en ES, EN y EU (citizen → person, herritarra → pertsona)

Limpieza:

- :material-minus: ADRs vacios eliminados (0001, 0002)
- :material-minus: Ficheros legacy no publicados eliminados (vision-general-detallada, seguridad, servicios-core-admins)
- :material-minus: Traducciones obsoletas eliminadas y regeneradas

Traducciones:

- :material-translate: Traducciones EN+EU nuevas o actualizadas para todas las paginas modificadas
- :material-translate: operativas/data-retrieve, metadata-sync, person-sync (EN+EU)
- :material-translate: autenticacion/administracion-core-dena, core-dena-administracion (EN+EU)
- :material-translate: ejemplo-end-to-end, limites-api (EN+EU)

---

## v0.4.0 <small>— 2026-08-24</small> { #v040 }

**Incorporación del documento Semántica BASE v2.0:**

- :material-plus: HTTP Headers: cabeceras request/response, versionado API, digest de seguridad
- :material-plus: Consentimientos: principios, marco normativo (RGPD), ciclo de vida, formato y API
- :material-plus: DenaObjectRef: tipo base con OID, timestamps y URL
- :material-plus: DENAProtocol: URLs de callback, timeouts, HATEOAS
- :material-plus: DENAConsent: referencia a base habilitante en mensajes Data-Retrieve
- :material-plus: Status: códigos de respuesta, DENAClientErrDetails, DENAServerErrDetails, DENAAsyncQueueData
- :material-plus: Tipos de Mensaje: DenaFlowDirection, DenaMessageType, DenaInteropRouteDataItem, DenaPersonAndConsentGiven
- :material-plus: UserAgent: formato por origen (app móvil, web, CORE, administración)
- :material-plus: Tipos Sync: DenaInteroperableDataTypeSync, DenaPersonDataSync, DenaPersonMetaDataSyncItem
- :material-refresh: OrgAdminRef ampliado con orgId, officialId, objectOid, timestamps, url, estructura organizativa
- :material-refresh: PersonRef ampliado con personId, objectOid, timestamps, url
- :material-refresh: tipos-dato-base: añadidos Hash (SHA-256) y UserAgent
- :material-refresh: semantica-base/index: estructura completa del mensaje (context, protocol, consent, status, data)
- :material-refresh: mkdocs.yml: navegación actualizada con nuevas páginas
- :material-translate: Traducciones EN y EU para todas las páginas nuevas y actualizadas (76/76)

---

## v0.3.38 <small>— 2026-07-04</small> { #v0338 }

**Mejoras de documentación:**

- :material-palette: CSS reorganizado en ficheros numerados `01`–`06` por responsabilidad
- :material-refresh: `extra.css` migrado a ficheros numerados; fichero legacy vaciado
- :material-image: Logos de administraciones añadidos al footer (Ayto. Bilbao, Donosti, Vitoria, DFA, DFB, DFG, EUDEL)
- :material-format-size: Tamaño de logos del footer aumentado a 60px
- :material-bug: Corrección de variables dark mode faltantes en `01-variables.css`, `02-material-overrides.css` y `06-mermaid-diagrams.css`
- :material-bug: Corrección masiva `registry` → `register` en `registro-oficial.md` (es/en/eu)
- :material-web: Tipografía Manrope migrada de fuente local a Google Fonts

---

## v0.3.32 <small>— 2026-06-26</small> { #v0332 }

**Mejoras de documentación:**

- :material-palette: Aplicación completa de colores corporativos DENA (#1D3328)
- :material-image: Logo DENA redimensionado a 64px para mejor visibilidad
- :material-view-grid: Página de entrada "Operativas DENA" con navegación visual
- :material-cogs: Consolidación de secciones operativas en menú principal
- :material-email: Integración de contacto de soporte (admin-digital-data-dena@ejie.eus)
- :material-alert: Avisos de versión de repositorio en toda la documentación
- :material-star: Consistencia visual con iconos Material Design
- :material-navigation: Navegación simplificada sin emojis
- :material-help: Página de contacto y soporte dedicada

---

## v0.3.31 <small>— 2026-06-25</small> { #v0331 }

**Mejoras de documentación:**

- :material-refresh: Variables centralizadas actualizadas en `mkdocs-vars.yaml`
- :material-refresh: Todos los footers migrados a variables `{{ dena.version }}` y `{{ dena.date }}`
- :material-refresh: Headers de versión en documentación semántica usando variables
- :material-plus: Mantenibilidad mejorada: cambios de versión desde un solo archivo
- :material-bug: Corrección de inconsistencias de versionado en documentación

---

## v0.3.26 <small>— 2026-06-11</small> { #v0326 }

**Mejoras de documentación:**

- :material-plus: Página de entrada rediseñada con Material for MkDocs
- :material-plus: Sección "Guía de inicio" con instalación, comunicaciones y mock
- :material-plus: FAQ (Preguntas frecuentes)
- :material-plus: Glosario de términos
- :material-plus: Custom CSS con colores corporativos DENA
- :material-plus: Tooltips en siglas técnicas (OID, DIR3, SIA...)
- :material-plus: Variables reutilizables (`mkdocs-vars.yaml`)
- :material-refresh: Todas las páginas actualizadas con features de Material (admonitions, tabs, mermaid, grid cards)
- :material-refresh: Navegación reorganizada con tabs y secciones expandibles

---

## v0.3.25 <small>— 2026-06-10</small> { #v0325 }

**Contenido:**

- :material-plus: Documentación DATA-RETRIEVE completa (endpoint, objetos, validaciones, errores, snippets)
- :material-plus: Documentación PERSON-SYNC (Pull + Push, endpoints, modelos)
- :material-plus: Documentación METADATA-SYNC (endpoint)
- :material-plus: Guía de implementación Data-Retrieve
- :material-plus: Diagramas mermaid interactivos en semántica
- :material-plus: Colecciones Postman actualizadas

---

## v0.2.0 <small>— 2026-03-15</small> { #v020 }

**Contenido:**

- :material-plus: Documentación de autenticación (Cliente ↔ DENA, Admin ↔ DENA)
- :material-plus: Diagramas de flujo OAuth2
- :material-plus: Endpoint get-token
- :material-plus: Semántica base (estructura REST Message)

---

## v0.1.0 <small>— 2025-12-01</small> { #v010 }

**Contenido:**

- :material-plus: Estructura inicial del repositorio
- :material-plus: Arquitectura general (diagrama draw.io)
- :material-plus: Primeros modelos de datos (DataTypeRef, PersonRef, OrgAdminRef)
- :material-plus: DevTools - DENA Admin Connection Test

---

## Convención de versiones

| Formato | Significado |
|---|---|
| `v0.X.Y` | Fase de desarrollo pre-release |
| :material-plus: | Contenido nuevo |
| :material-refresh: | Contenido actualizado |
| :material-minus: | Contenido eliminado |
| :material-bug: | Corrección de errores |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
