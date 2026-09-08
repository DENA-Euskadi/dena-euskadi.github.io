# :material-shield-lock: Segurtasuna eta Autentifikazioa

Atal honek DENAko komunikazioak nola babesten diren eta sistemaren zati bakoitza nola autentifikatzen den azaltzen du. Lehenik kontzeptuak, gero gida praktikoa.

---

## Segurtasun-helburuak

DENAko segurtasunak lau helburu ditu:

1. **Eskariaren jatorri legitimoa:** Zerbitzura soilik baimendutako eta egiaztatutako alderdiek sartzen direla bermatzea.
2. **Eskariaren osotasuna:** Trukatutako datuak garraioan zehar aldatzen ez direla bermatzea.
3. **Datuen konfidentzialtasuna:** Datuak zifratuta eta baimendu gabeko hirugarrenei ikusezinak mantentzen direla bermatzea.
4. **Replay erasoak prebenitzea:** Hirugarrenek eskariak harrapatu eta erreproduzitzea prebenitzea.

| Helburua | HTTPS | JWT | Security Header |
|----------|:-----:|:---:|:---------------:|
| Jatorri legitimoa | | :material-check: | :material-check: |
| Osotasuna | | :material-check: | :material-check: |
| Konfidentzialtasuna | :material-check: | :material-check: | |
| Anti-replay | | :material-check: | :material-check: |

---

## Autentifikazio-eremuak

Lau eremu daude non alderdi desberdinak autentifikatzen diren:

![Security Areas](../adjuntos/imagenes/arquitectura/security-areas.png)

| Eremua | Nork autentifikatzen du | Norekin | Mekanismoa |
|------|-------------------|-----------|-----------|
| **1** | Pertsona | DENA-APP | GILTZA OAuth token |
| **2** | DENA-APP | DENA-CORE | DENA-IdP OAuth token |
| **3** | Administrazioa | DENA-CORE | DENA-IdP OAuth token (client_credentials) |
| **4** | DENA-CORE | Administrazioa | Administrazioak nahiago duena (OAuth, X.509, usr/pwd...) |

!!! info "Administrazioentzat"
    Administrazio gisa eragiten dizuten eremuak **3** (zure administrazioa DENAn autentifikatzen da) eta **4** (DENA zure administrazioan autentifikatzen da) dira. 1 eta 2 eremuak pertsonaren apparen barnekoak dira.

---

## 1. Fluxua: Pertsona → DENA-APP (GILTZA)

![Auth GILTZA](../adjuntos/imagenes/arquitectura/auth-giltza-flow.png)

1. Pertsona DENA-APP-en sartzen da eta **GILTZA**ren login orrira birbideratzen da (web-view)
2. Pertsonak saioa hasten du
3. GILTZAk OAuth token bat itzultzen du

---

## 2. Fluxua: DENA-APP → DENA-CORE (DENA-IdP)

![Auth DENA-APP CORE](../adjuntos/imagenes/arquitectura/auth-dena-app-core.png)

DENA-APP DENA-CORE-rekin identifikatzen da GILTZA tokena erabiliz. Trukean **DENA-IdP OAuth token** bat jasotzen du, ondorengo dei guztietan erabiliko duena (init, sync SRMD, retrieve data...).

!!! warning "Iraungitzea"
    DENA-IdP tokena **iraungitzen da**. DENA-APP-ek aldiro freskatu behar du.

---

## 3. Fluxua: Zure Administrazioa → DENA-CORE (client_credentials)

![Auth Admin CORE](../adjuntos/imagenes/arquitectura/auth-admin-core.png)

Zure administrazioa DENAn autentifikatzen da **client_credentials** OAuth2 erabiliz:

1. `client_id` + `client_secret` eskatzen diozu DENA taldeari
2. Kredentzialak DENA-IdP-ren token endpoint-era bidaltzen dituzu
3. OAuth token bat jasotzen duzu
4. Token hori erabiltzen duzu DENA-CORE-rako dei guztietan (SRMD bidali, pertsonak kontsultatu, etab.)

**Hau da inplementatu behar duzun fluxua:**

- Metadata-Sync bidaltzeko
- Pertsonen datuak kontsultatzeko (Person-Sync Pull)
- Zure administraziotik DENArako edozein deirentzat

[:octicons-arrow-right-24: Inplementazioaren gida praktikoa](../autenticacion/administracion-core-dena/index.md)

---

## 4. Fluxua: DENA-CORE → Zure Administrazioa

![Auth CORE Admin](../adjuntos/imagenes/arquitectura/auth-core-admin.png)

DENA-CORE-k zure administraziora deitzen duenean (Data-Retrieve-rako), autentifikatu egin behar da. DENAk **EZ du** mekanismorik ezartzen; administrazio bakoitzak berea aukeratzen du:

- **OAuth**: zure administrazioak client credentials ematen dizkio DENAri
- **X.509 Ziurtagiriak**: TLS autentifikazio elkarrekikoa
- **Erabiltzailea/pasahitza**: oinarrizko kredentzialak
- Beste batzuk

Zure administrazioak DENA taldeari beharrezko kredentzialak ematen dizkio aukeratutako mekanismorako.

[:octicons-arrow-right-24: Inplementazioaren gida praktikoa](../autenticacion/core-dena-administracion/index.md)

---

## Inplementazioaren gida praktikoak

Hurrengo orrialdeek inplementazioaren xehetasun teknikoak dituzte:

| Fluxua | Orrialdea |
|-------|--------|
| Zure administrazioak DENAri deitzen dio | [:octicons-arrow-right-24: get-token endpoint-a + adibideak](../autenticacion/administracion-core-dena/index.md) |
| DENAk zure administrazioari deitzen dio | [:octicons-arrow-right-24: Konfigurazioa + eredua](../autenticacion/core-dena-administracion/index.md) |

---

**Hurrengoa:** [:octicons-arrow-right-24: Operatibak](../operativas/index.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
