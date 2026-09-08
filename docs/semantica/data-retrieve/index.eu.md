# :material-database-arrow-right: DATA-RETRIEVE

> **Bertsioa:** `v{{ dena.version }}` · **Data:** {{ dena.date }}

## Zer da?

**Data-Retrieve** DENAk administrazio publiko bakoitzari herritar baten datuak eskatzeko erabiltzen duen mekanismoa da. Administrazioak REST endpoint bat eskaintzen du eta datu-objektuak JSON formatu normalizatuan itzultzen ditu.

---

## Testuinguru-diagrama

![Testuinguru-diagrama](images/diagrama-contexto.png)

---

## Eredu Kontzeptuala

![Eredu Kontzeptuala](images/diagrama-modelo-datos.png)

---

## Datu-eredua

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "13px"
    fontFamily: "Manrope, sans-serif"
    edgeLabelBackground: "#FFFFFF"
---
flowchart LR
    IFACE["Data Exchanged Object\n(interfazea)"]
    BASE["Data Exchanged Object BASE\nJatorriko Adm. erref. · Person erref. · Urls"]

    IFACE -.-> BASE

    RECORD["Record\n(espedientea)"]
    NOTICE["Administrative Notice\n(jakinarazpena)"]
    REGISTRY["Official Register\n(erregistroak)"]
    PAYBASE["Payment BASE"]
    SCHEDULE["Schedule\n(egutegia)"]

    BASE -.-> RECORD
    BASE -.-> NOTICE
    BASE -.-> REGISTRY
    BASE -.-> PAYBASE
    BASE -.-> SCHEDULE

    ONEOFF["OneOffPayment\n(likidazioak)"]
    DIRECTDEBIT["DirectDebitPayment\n(helbideratzeak)"]

    PAYBASE --> ONEOFF
    PAYBASE --> DIRECTDEBIT

    SRVBASE["Administrative Service Object BASE\nname · urls · org"]
    SRV["Zerbitzua\nJatorri-erref. · DENA erref. · SIA erref."]
    PROC["Prozedura\nJatorri-erref. · DENA erref. · SIA erref."]

    SRVBASE --> SRV
    SRVBASE --> PROC

    RECORD --> SRV
    RECORD --> PROC
    NOTICE --> PROC
    REGISTRY --> PROC
    PAYBASE --> PROC

    click RECORD "./data/expediente/" "Ver documentación de Expediente"
    click NOTICE "./data/notificacion/" "Ver documentación de Notificación"
    click REGISTRY "./data/registro-oficial/" "Ver documentación de Registro Oficial"
    click PAYBASE "./data/pago/" "Ver documentación de Pagos"
    click ONEOFF "./data/pago/" "Ver documentación de Pago Único"
    click DIRECTDEBIT "./data/pago/" "Ver documentación de Domiciliación"
    click SCHEDULE "./data/cita/" "Ver documentación de Cita"
    click SRV "./data/servicio-administrativo/" "Ver documentación de Servicio"
    click PROC "./data/servicio-administrativo/" "Ver documentación de Procedimiento"
    click BASE "./data/campos-comunes/" "Ver campos comunes heredados"

    style IFACE fill:#e3f2fd,stroke:#1565c0,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style BASE fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px,rx:10,ry:10
    style RECORD fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style NOTICE fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style REGISTRY fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style PAYBASE fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style SCHEDULE fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style ONEOFF fill:#ffcccc,stroke:#d8704a,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style DIRECTDEBIT fill:#e1d5e7,stroke:#9673a6,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style SRVBASE fill:#f0f0f0,stroke:#7a8477,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style SRV fill:#f0f0f0,stroke:#7a8477,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
    style PROC fill:#f0f0f0,stroke:#7a8477,color:#1a4d1f,stroke-width:2px,rx:10,ry:10
```

| Kolorea | Esanahia |
|-------|-------------|
| 🔵 Urdin argia | Oinarrizko interfazea (kontratua) |
| 🟣 Bioleta | Oinarrizko klase abstraktua |
| 🟠 Laranja | Trukatutako domeinu-objektuak |
| 🔴 Gorri argia | Ordainketa bakarra (OneOffPayment) |
| ⚪ Gris argia | Testuinguru hierarkikoa (Zerbitzua / Prozedura) |

| Lerroa | Esanahia |
|-------|-------------|
| Etena | Herentzia ("hedatzen du") |
| Jarraitua | Konposizioa / erreferentzia |

---

## Dokumentazioa

!!! tip "Hasi hemen"

    [:octicons-arrow-right-24: **Urratsez urratseko inplementazio-gida**](./guia-implementacion.md) — Zure administrazioan endpointa inplementatzeko behar duzun guztia.

### Endpoint

| Dokumentua | Edukia |
|---|---|
| [:octicons-arrow-right-24: Endpoint](./endpoint-data-retrieve.md) | REST kontratua: request, response, JSON adibideak eta HTTP kodeak |

### Datu-objektuak

??? note "Ikusi datu-objektu guztiak"

    | Objektua | Deskribapena | Dokumentua |
    |---|---|---|
    | **:material-folder-open: Espedientea** | Izapidetzen ari den administrazio-espedientea | [expediente.md](./data/expediente.md) |
    | **:material-email-open: Jakinarazpena** | Jakinarazpen ofiziala edo komunikazioa | [notificacion.md](./data/notificacion.md) |
    | **:material-book-open-page-variant: Erregistro Ofiziala** | Sarrera/irteera erregistro-idazpena | [registro-oficial.md](./data/registro-oficial.md) |
    | **:material-credit-card: Ordainketa** | Ordainketa bakarra edo banku-helbideratzea | [pago.md](./data/pago.md) |
    | **:material-calendar-clock: Hitzordua** | Aurretiazko hitzordua edo agenda-elementua | [cita.md](./data/cita.md) |
    | **:material-account-circle: Pertsona** | Herritarraren datuak | [persona.md](./data/persona.md) |
    | **:material-cog: Administrazio Zerbitzua** | Zerbitzua eta prozedura | [servicio-administrativo.md](./data/servicio-administrativo.md) |
    | **:material-office-building: Unitate Organikoa** | Parte hartzen duen unitate antolatzailea | [unidad-organica.md](./data/unidad-organica.md) |

### Gida osagarriak

| Dokumentua | Edukia |
|---|---|
| [:material-database-outline: Eremu komunak](./data/campos-comunes.md) | Objektu guztiek heredatutako eremuak (OID, ID, URLak, erref.) |
| [Balidazioak](./validaciones.md) | Balidazio-arauak, formatuak eta murrizketak |
| [Erroreak eta troubleshooting](./errores-troubleshooting.md) | Ohiko erroreen eta konponbidearen gida |
| [Kode-snippetak](./snippets-codigo.md) | Java, C#, Python, Node.js eta PHP inplementazioa |

---

## Fluxuaren laburpena

1. DENAk `POST /api/retrieveData` bidaltzen du pertsonaren identifikatzailearekin eta datu motarekin
2. Administrazioak pertsona identifikatzen du (`context.subjectPerson`)
3. Administrazioak datu motaren arabera iragazten du (`context.dataType`)
4. Administrazioak objektuak `payload.dataItems[]` barruan itzultzen ditu

### Datu motak (`dataType.id`)

`dataType`-ren `id` objektu bakoitzaren marshallTypeId-ari dagokio (`DN00DataTypeEnum` enum-a):

| `dataType.id` | Espero diren datuak |
|--------------|-----------------|
| `administrativeServiceProcedureRecord` | Espedienteak |
| `administrativeNotice` | Jakinarazpenak |
| `administrativeOfficialRegisterRecord` | Erregistro ofizialak |
| `oneOffPayment` / `directDebitPayment` | Ordainketak |
| `scheduleItem` | Hitzorduak |
| `personData` | Pertsonaren datuak |

> **Oharra:** Ikusi zerrenda osoa hemen: [DataTypeRef](../semantica-base/modelo/data-type-ref.md).

---

## Request-aren egitura

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "12px"
    fontFamily: "Manrope, sans-serif"
---
flowchart LR
    ROOT["InteropMessage"] --> CTX["context"]
    ROOT --> PROTOCOL["protocol"]
    ROOT --> PAYLOAD["payload"]

    CTX --> MSG["message"]
    CTX --> ORIGIN_CI["originClientInstallment"]
    CTX --> ORIGIN_ADMIN["originAdmin"]
    CTX --> DEST["destinationAdmin"]
    CTX --> PERSON["subjectPerson"]
    CTX --> DTYPE["dataType"]
    CTX --> UA["userAgent"]

    MSG --> MTYPE["type"]
    MSG --> CORR["correlationId"]
    MSG --> ROUTE["interopRouteData"]

    PROTOCOL --> PURLS["urls"]
    PROTOCOL --> PTO["timeOut"]

    style ROOT fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px,rx:8,ry:8
    style CTX fill:#e3f2fd,stroke:#1565c0,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style PAYLOAD fill:#e3f2fd,stroke:#1565c0,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style PROTOCOL fill:#e3f2fd,stroke:#1565c0,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style MSG fill:#e1d5e7,stroke:#9673a6,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style MTYPE fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style DTYPE fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style CORR fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style ORIGIN_CI fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style ORIGIN_ADMIN fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style DEST fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style PERSON fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style UA fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style ROUTE fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style PURLS fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style PTO fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
```

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": [
        { "denaComponentId": "DENA_CORE", "timestamp": "2026-06-01T10:00:00.000Z" }
      ]
    },
    "originClientInstallment": "8B5AE78A-7D42-4069-A626-959BB07276C5",
    "destinationAdmin": { "id": "ADMIN-001", "oid": "...", "dir3Id": "EA0000001" },
    "subjectPerson": { "id": "12345678A", "oid": "..." },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "..." },
    "userAgent": "DENA-CORE/2.1.0 person-data/1.0.1 (support@dena.eus)"
  },
  "protocol": {
    "urls": [],
    "timeOut": "30s"
  },
  "payload": {
    "person": "PERSON-OID-001"
  }
}
```

| Eremua | Nahitaezkoa | Deskribapena |
|-------|:-----------:|-------------|
| `context.message.type` | ✅ | Mezu mota. Ikusi [`DN00InteropMessageType`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropMessageType.java) |
| `context.message.correlationId` | ✅ | Trazabilitaterako korrelazio UUIDa. Ikusi [`DN00InteropMessageData`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropMessageData.java) |
| `context.message.interopRouteData` | ❌ | Osagaien traza. Ikusi [`DN00IteropRouteDataItem`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00IteropRouteDataItem.java) |
| `context.dataType` | ❌ | Eskatutako datu mota. Ikusi [`DN00DataTypeEnum`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DataTypeEnum.java) |
| `context.originClientInstallment` | ❌ | Jatorriko bezero-instalazioaren OIDa. Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.originAdmin` | ❌ | Jatorriko administrazioa. Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.destinationAdmin` | ❌ | Helmugako administrazioa. Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.subjectPerson` | ❌ | Datuak eskatzen zaizkion pertsona. Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.userAgent` | ❌ | Jatorriaren User Agent-a. Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `protocol.urls` | ❌ | Protokoloaren txantiloi URLak. Ikusi [`DN00InteropProtocol`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropProtocol.java) |
| `protocol.timeOut` | ❌ | Eragiketaren timeout-a (adib.: `"30s"`). Ikusi [`DN00InteropProtocol`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropProtocol.java) |
| `payload` | ✅ | Eragiketaren berariazko payload-a |

### Mezu motak (`message.type`)

| Balioa | Deskribapena | DATA-RETRIEVE-n erabilera |
|-------|-------------|---------------------|
| `PERSON_FETCH_DATA` | Pertsonaren datuen eskaera | ✅ Nagusia |
| `CLIENT_RETRIEVE_REQ` / `CLIENT_RETRIEVE_RESP` | Bezerotik datuak berreskuratzea | Eskaera/Erantzuna |

Ikusi balioen zerrenda osoa hemen: [Mezu Motak](../semantica-base/modelo/message-types.md).

### Fluxuaren norabidea (`flowDirection`)

Norabidea (`REQUEST`/`RESPONSE`) mezu motatik eratortzen da; ez da testuinguruaren beraren eremu bat.

### `protocol` blokea

DENAren eta administrazioaren arteko komunikazio-protokoloaren metadatuak.

| Eremua | Mota | Deskribapena |
|-------|------|-------------|
| `urls` | `Array` | Komunikaziorako txantiloi URLak |
| `timeOut` | `TimeLapse` | Eragiketaren timeout-a (formatua: `"30s"`, `"1m"`, etab.) |

### Ibilbidearen traza (`interopRouteData`)

Mezua DENAren osagai batetik pasatzen den bakoitzean, traza-sarrera bat gehitzen da:

| Eremua | Mota | Deskribapena |
|-------|------|-------------|
| `denaComponentId` | `DN00InteropComponent` | Osagaiaren identifikatzailea (`CLIENT_INSTALLMENT`, `DENA_CORE`, `DENA_ADMIN_CONNECTOR`, `ADMIN`) |
| `timestamp` | `String` (ISO 8601) | Osagaiak mezua prozesatu zuen unea |

---

## Response-aren egitura

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "12px"
    fontFamily: "Manrope, sans-serif"
---
flowchart LR
    ROOT["InteropResponse"] --> CTX["context"]
    ROOT --> PAYLOAD["payload"]
    ROOT --> CODE["code"]
    ROOT --> ERRID["errorId"]
    ROOT --> DETAILS["details"]

    CTX --> PERSON["subjectPerson"]
    CTX --> DTYPE["dataType"]
    CTX --> DEST["destinationAdmin"]

    PAYLOAD --> ITEMS["dataItems"]
    ITEMS --> RECORD["Espedientea"]
    ITEMS --> NOTICE["Jakinarazpena"]
    ITEMS --> REGISTRY["Erregistro Ofiziala"]
    ITEMS --> PAYMENT["Ordainketa"]
    ITEMS --> SCHEDULE["Hitzordua"]

    click RECORD "./data/expediente/" "Ver documentación de Expediente"
    click NOTICE "./data/notificacion/" "Ver documentación de Notificación"
    click REGISTRY "./data/registro-oficial/" "Ver documentación de Registro Oficial"
    click PAYMENT "./data/pago/" "Ver documentación de Pagos"
    click SCHEDULE "./data/cita/" "Ver documentación de Cita"

    style ROOT fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px,rx:8,ry:8
    style CTX fill:#e3f2fd,stroke:#1565c0,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style PAYLOAD fill:#e3f2fd,stroke:#1565c0,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style CODE fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style ERRID fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style DETAILS fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style PERSON fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style DTYPE fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style DEST fill:#e8f5e8,stroke:#2e7d32,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style ITEMS fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px,rx:8,ry:8
    style RECORD fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style NOTICE fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style REGISTRY fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style PAYMENT fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
    style SCHEDULE fill:#ffe6cc,stroke:#d79b00,color:#1a4d1f,stroke-width:2px,rx:8,ry:8
```

```json
{
  "context": {
    "message": {
      "type": "CLIENT_RETRIEVE_RESP",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "subjectPerson": { "id": "12345678A", "oid": "..." },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "..." },
    "destinationAdmin": { "id": "ADMIN-001", "oid": "..." }
  },
  "payload": {
    "dataItems": [ ... ]
  },
  "code": "OK",
  "errorId": null,
  "details": null
}
```

| Eremua | Deskribapena |
|-------|-------------|
| `context` | Jasotako testuinguruaren oihartzuna (mezu motak erantzuna adierazten du) |
| `payload.dataItems` | Datu-objektuen array-a |
| `code` | Erantzunaren egoera (`DN00InteropResponseStatus`) |
| `errorId` | Errore-kode espezifikoa (aukerakoa, erroreetan soilik) |
| `details` | Errorearen mezu deskribatzailea (aukerakoa) |

### Egoera-kodeak (`code`)

| Kodea | Deskribapena |
|--------|-------------|
| `OK` | Mezua behar bezala prozesatuta |
| `CLIENT_ERR` | Bezeroaren errorea (gaizki eratutako eskaera, datu baliogabeak) |
| `SERVER_ERR` | Zerbitzariaren errorea (barne-errorea) |
| `QUEUED` | Mezua ilaran prozesamendu asinkronorako |

### HTTP kodeak

| Kodea | Esanahia |
|--------|-------------|
| `200` | Datuak behar bezala itzulita (zerrenda hutsa izan daiteke) |
| `400` | Gaizki eratutako eskaera |
| `401` | Baimenik gabe |
| `404` | Pertsona ez da aurkitu |
| `500` | Barne-errorea |

---

## Glosarioa

| Terminoa | Deskribapena |
|---------|-------------|
| OID | Identifikatzaile tekniko bakarra (sistemak esleitua) |
| ID | Negozio-identifikatzailea (irakurgarria, administrazioak esleitua) |
| DIR3 | AGEko Unitate Organikoen eta Bulegoen Direktorio Komuna |
| SIA | Administrazio Informazio Sistema (AGEren zerbitzu-katalogoa) |
| LanguageTexts | Hizkuntza anitzeko objektua, `SPANISH`, `BASQUE`, `ENGLISH` gakoekin |
| dataItems | Administrazioak itzultzen dituen domeinu-objektuen array-a |
| interopRouteData | Mezua igaro den DENAren osagaien traza. Ikusi [`DN00IteropRouteDataItem`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00IteropRouteDataItem.java) |
| correlationId | Request eta response korrelazionatzeko aukera ematen duen UUIDa. Ikusi [`DN00InteropMessageData`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropMessageData.java) |










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
