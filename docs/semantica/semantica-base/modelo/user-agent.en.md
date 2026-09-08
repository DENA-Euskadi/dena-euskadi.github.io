# :material-cellphone-link: UserAgent

## Description

The [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/User-Agent) is a `String` included as an HTTP header in requests, providing data about the **client** (browser, application, etc.) making the request.

---

## General format

```
<product>/<version> (<system-information>) <platform> (<platform-details>) <extensions>
```

---

## Format by message origin

### Message initiated by the client (mobile app / web app)

=== "Mobile App (Flutter)"

    ```
    DenaApp/<DenaApp ver> (<system> <System ver>; <device> <device ver>) Dart/<dart ver> Flutter/<flutter ver>
    ```

    **Example:**
    ```
    DenaApp/1.0 (Android 13; Pixel 6 Pro) Dart/3.0 Flutter/3.16
    ```

=== "Web App (browser)"

    The User-Agent will be whatever the web browser determines.

    **Example (Chrome on Mac):**
    ```
    Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
    ```

### Message initiated by DENA-CORE

```
DENA-CORE/<dena-core ver> <module>/<module ver> (support@dena.eus)
```

**Example** (person-data module notifying a signup):
```
DENA-CORE/2.1.0 person-data/1.0.1 (support@dena.eus)
```

### Message initiated by an administration

```
AdminX/<AdminX ver> <module>/<module ver> (support@adminX.eus)
```

**Example** (administration sending records SRMD):
```
AdminX/1.1.0 expedientes/1.0.0 (support@adminX.eus)
```

!!! info "Not mandatory for administrations"
    The User-Agent header in messages initiated by an administration's data source is **not mandatory**.

---

## User-Agent components (Chrome Android example)

| Component | Value | Description |
|---|---|---|
| `<product>/<version>` | `Mozilla/5.0` | Historical compatibility |
| `<system-information>` | `Linux; Android 16; Pixel 9` | OS and device |
| `<platform>` | `AppleWebKit/537.36` | Rendering engine |
| `<platform-details>` | `KHTML, like Gecko` | Engine details |
| `<extensions>` | `Chrome/143.0.12.45 Mobile Safari/537.36` | Browser and mode |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
