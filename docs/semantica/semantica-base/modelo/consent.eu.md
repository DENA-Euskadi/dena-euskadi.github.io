# :material-shield-check: DENAConsent

## Deskribapena

Elkarreragingarritasun-eskaera bat babesten duen **oinarri gaitzailearen** (baimena edo gaitzapen normatiboa) erreferentzia duen objektua.

!!! warning "Data-Retrieve soilik"
    `consent` blokea **datuen berreskuratze** (Data-Retrieve) mezuetan soilik dago. Administrazioari pertsonaren datuen trukea ahalbidetzen duen oinarri bat existitzen dela egiaztatzea ahalbidetzen dio.

---

## JSON Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `consentOid` | `OID` | :material-check: | Biltegi komuneko baimenaren identifikatzaile bakarra |
| `consentURL` | `URL` | :material-close: | Hartzaileak baimenaren xehetasunak aurkitu eta deskargatu ditzakeen URLa |
| `consentData` | `Object` | :material-close: | Baimenaren zenbait xehetasun (noiz eman zen, zein bitartekoaren bidez, noiz arte, etab.) |

---

## Adibidea

```json
{
  "consent": {
    "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentURL": "https://interop.api.dena.eus/consent/db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentData": {
      "grantedAt": "2025-06-15T10:30:00.000Z",
      "expiresAt": "2026-06-15T10:30:00.000Z",
      "grantedVia": "DENA_APP_ENROLLMENT"
    }
  }
}
```

---

## Administrazioaren egiaztapena

Administrazioak, edozein unetan, `consentURL` atzitu dezake:

1. Baimenaren xehetasun guztiak deskargatzeko
2. Biltegi komunak jaulkitako **sinaturiko justifikantea** lortzeko
3. Baimena oraindik indarrean dagoela egiaztatzeko

!!! tip "Aukerako egiaztapena"
    DENA-CORE-k dagoeneko oinarri gaitzailearen existentzia egiaztatzen du eskaera bidali aurretik. Administrazioak mekanismo honetan konfiantza izan dezake edo, beharrezkotzat jotzen badu, gehigarri egiaztatu.

---

## Baimenaren bizi-zikloarekin erlazioa

DENAn baimenak nola kudeatzen diren xehetasun gehiagorako, ikusi: [:octicons-arrow-right-24: Baimenak](../consentimientos.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
