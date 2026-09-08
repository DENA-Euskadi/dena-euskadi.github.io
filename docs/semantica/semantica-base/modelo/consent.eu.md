# :material-shield-check: Baimena (consentOid)

## Deskribapena

Eskaera baten oinarri den baimena (edo oinarri gaitzailea) bere **OID**aren bidez erreferentziatzen da. Kode-ereduan, baimena `consentOid` eremu bakar gisa (`DN00ConsentOID` mota) transmititzen da eskaeren oinarrian (`DN00InteropRequestMessageBase`).

!!! info "Eskaera guztietan present"
    `consentOid` elkarreragingarritasun-eskaeren oinarriaren parte da, ez soilik Data-Retrieve-rena. Bere presentzia eraginkorra eragiketak oinarri gaitzailea behar duen ala ez arabera dago.

---

## JSON atributuak

| Eremua | Mota | Beharrezkoa | Deskribapena |
|---|---|:---:|---|
| `consentOid` | `OID` (`DN00ConsentOID`) | :material-close: | Baimenaren identifikatzaile bakarra baimenen biltegian |

---

## Adibidea

```json
{
  "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb"
}
```

---

## Egiaztapena

Uneko ereduak baimenaren OIDa soilik garraiatzen du. Baimenaren xehetasuna (noiz eman zen, indarraldia, etab.) DENAren baimenen biltegian kudeatzen da; biltegi horren kontsulta-APIa definitzeke dago.

Baimenaren bizi-zikloari buruzko testuinguru gehiagorako, ikus: [:octicons-arrow-right-24: Baimenak](../consentimientos.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
