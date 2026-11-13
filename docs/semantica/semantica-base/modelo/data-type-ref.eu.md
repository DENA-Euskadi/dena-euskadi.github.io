# :material-tag: DataTypeRef

## Deskribapena

`dataType` `context`-eko eremua da eta **zein datu mota** eskatzen edo trukatzen ari den adierazten du. Zure administrazioak zein objektu itzuli behar duen jakiteko irakurtzen duen pieza da.

!!! tip "Administrazio batek behar duen bakarra"

    **`dataType.id` irakurtzea besterik ez da behar**: datu mota identifikatzen duen testu bat da (adib. `SCHEDULE`). Bere balioaren arabera, dagokion objektua itzultzen duzu. `oid` DENA-ren barneko identifikatzaile bat da eta **ez da interpretatu behar**.

---

## JSON atributuak

| Eremua | Nahitaezkoa | Deskribapena |
|---|:---:|---|
| `id` | ✅* | Datu motaren testu-identifikatzailea. Zein objektu itzuli behar duzun jakiteko erabiltzen duzuna da |
| `oid` | ❌* | DENA-ren barneko identifikatzailea (GUID bat). Barneko erabilerarako; ez da interpretatu behar |

<small>*Bietako bat gutxienez. Praktikan `id` beti heltzen da.</small>

---

## Adibidea

```json
{
    "id": "SCHEDULE",
    "oid": "7e0e84e5-4e4e-7e4e-be4e-4e4e4e4e4006"
}
```

---

## Datu moten katalogoa (`dataType.id`)

`dataType.id`-n etorri daitezkeen balio baliodunak eta itzuli beharreko datu-objektua:

| `dataType.id` | Itzuli beharreko datu-objektua | Dokumentazioa |
|---|---|---|
| `ADMINISTRATIVE_RECORD` | Espediente administratiboa | [expediente.md](../../../data-retrieve/data/expediente.md) |
| `ADMINISTRATIVE_NOTICE` | Jakinarazpena | [notificacion.md](../../../data-retrieve/data/notificacion.md) |
| `PAYMENT_ONE_OFF_PAYMENT` | Ordainketa bakarra | [pago.md](../../../data-retrieve/data/pago.md) |
| `PAYMENT_DIRECT_DEBIT_PAYMENT` | Banku-domiziliazioa | [pago.md](../../../data-retrieve/data/pago.md) |
| `SCHEDULE` | Aurretiazko hitzordua | [cita.md](../../../data-retrieve/data/cita.md) |
| `PERSON_DATA` | Pertsonaren datuak | [persona.md](../../../data-retrieve/data/persona.md) |
| `PERSON_CREDENTIAL` | Pertsonaren kredentzialak | [credencial-persona.md](../../../data-retrieve/data/credencial-persona.md) |
| `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE` | Euskarako hizkuntza-profilaren kredentzialak | [credencial-persona.md](../../../data-retrieve/data/credencial-persona.md) |
| `ECONOMIC_BENEFIT` | Onura ekonomikoa | [beneficio-economico.md](../../../data-retrieve/data/beneficio-economico.md) |
| `ADMINISTRATIVE_LEDGER` | Erregistro-liburua | [libro-registro.md](../../../data-retrieve/data/libro-registro.md) |
| `ADMINISTRATIVE_PERSON_REGISTER` | Pertsona-erregistroa | [registro-persona.md](../../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV` | Pertsona-erregistroa — PEV etxebizitza | [registro-persona.md](../../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL` | Pertsona-erregistroa — azken borondateak | [registro-persona.md](../../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV` | Pertsona-erregistroa — PEV berrikuspena | [registro-persona.md](../../../data-retrieve/data/registro-persona.md) |
| `PERSON_ENROLLMENT` | Matrikula | [matricula.md](../../../data-retrieve/data/matricula.md) |
| `PERSON_ACADEMIC_ENROLLMENT` | Matrikula akademikoa | [matricula.md](../../../data-retrieve/data/matricula.md) |
| `PERSON_MEDICAL_DATA` | Datu medikoak | [datos-medicos.md](../../../data-retrieve/data/datos-medicos.md) |

> ⚠️ `ADMINISTRATIVE_OFFICIAL_REGISTER_RECORD` mota **zaharkituta** dago eta katalogotik kendu da. Bere ordezko mota `ADMINISTRATIVE_LEDGER` da.

<!-- DENA-DOC-FOOTER -->
