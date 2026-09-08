# :material-cellphone-link: UserAgent

## Deskribapena

[User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/User-Agent) HTTP goiburuetan eskaeretan sartzen den `String` bat da, eskaera egiten duen **bezeroari** (nabigatzailea, aplikazioa, etab.) buruzko datuak ematen dituena.

---

## Formatu orokorra

```
<product>/<version> (<system-information>) <platform> (<platform-details>) <extensions>
```

---

## Formatua mezuaren jatorriaren arabera

### Bezeroak (app mugikorra / web app) hasitako mezua

=== "App Mugikorra (Flutter)"

    ```
    DenaApp/<DenaApp ber> (<sistema> <Sistema ber>; <gailua> <gailu ber>) Dart/<dart ber> Flutter/<flutter ber>
    ```

    **Adibidea:**
    ```
    DenaApp/1.0 (Android 13; Pixel 6 Pro) Dart/3.0 Flutter/3.16
    ```

=== "Web App (nabigatzailea)"

    User-Agent web nabigatzaileak zehaztutakoa izango da.

    **Adibidea (Chrome Mac-en):**
    ```
    Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
    ```

### DENA-CORE-k hasitako mezua

```
DENA-CORE/<dena-core ber> <modulua>/<modulu ber> (support@dena.eus)
```

**Adibidea** (person-data moduluak alta bati buruz jakinaraziz):
```
DENA-CORE/2.1.0 person-data/1.0.1 (support@dena.eus)
```

### Administrazio batek hasitako mezua

```
AdminX/<AdminX ber> <modulua>/<modulu ber> (support@adminX.eus)
```

**Adibidea** (administrazioak espedienteen SRMD bidaltzen):
```
AdminX/1.1.0 expedientes/1.0.0 (support@adminX.eus)
```

!!! info "Administrazioentzat ez da derrigorrezkoa"
    Administrazioaren datu-iturri batetik hasitako mezuetako User-Agent goiburua **ez da derrigorrezkoa**.

---

## User-Agent osagaiak (Chrome Android adibidea)

| Osagaia | Balioa | Deskribapena |
|---|---|---|
| `<product>/<version>` | `Mozilla/5.0` | Bateragarritasun historikoa |
| `<system-information>` | `Linux; Android 16; Pixel 9` | SE eta gailua |
| `<platform>` | `AppleWebKit/537.36` | Errendatze-motorra |
| `<platform-details>` | `KHTML, like Gecko` | Motorraren xehetasunak |
| `<extensions>` | `Chrome/143.0.12.45 Mobile Safari/537.36` | Nabigatzailea eta modua |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
