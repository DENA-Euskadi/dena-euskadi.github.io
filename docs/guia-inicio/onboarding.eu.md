# :material-checkbox-multiple-marked: onboardinga Egiaztapen-zerrenda

Urratsez urratseko gida "sarbidea eman didate"-tik "nire integrazioa martxan dago"-ra.

---

## :material-numeric-1-circle: Ingurunea prestatu

- [ ] JDK 21+ instalatu
- [ ] Maven 3.9+ instalatu
- [ ] `settings.xml` konfiguratu DENA biltegiekin
- [ ] Konektibitate-probaren biltegia klonatu

[:octicons-arrow-right-24: Instalazio-gida](./instalacion.md)

---

## :material-numeric-2-circle: Kredentzialak lortu

- [ ] `client_id` eta `client_secret` eskatu DENA taldeari
- [ ] Token endpoint-aren URLa jaso (Keycloak DENA)
- [ ] Esleitutako ingurunea berretsi (PRE/PRO)

!!! info "Kontaktua kredentzialetarako eta laguntzarako"

    **:material-email: DENA Kontaktua:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    Kredentzialak DENA taldeak ematen ditu administrazioaren alta-fasean. onboardinga prozesuari edo arazo teknikoei buruzko edozein zalantzatarako, jarri harremanetan laguntza-taldearekin.

---

## :material-numeric-3-circle: Konektibitatea balioztatu

- [ ] DENA Admin Connection Test zabaldu
- [ ] **Admin → DENA** egiaztatu (PRE-ren aurkako proba)
- [ ] Azpiegiturekin koordinatu **DENA → Admin** funtziona dezan
- [ ] Norabide biko konektibitatearen berrespena lortu

[:octicons-arrow-right-24: Komunikazio-gida](./probar-comunicaciones.md)

---

## :material-numeric-4-circle: Endpoint-a inplementatu

- [ ] Lehenik zer inplementatu aukeratu (normalean Data-Retrieve)
- [ ] Endpoint-aren espezifikazioa irakurri
- [ ] `POST /api/retrieveData` inplementatu zure sisteman
- [ ] Gutxienez datu-mota bat itzuli (adib.: espedienteak)
- [ ] Hizkuntza anitzeko testuak sartu (SPANISH + BASQUE)
- [ ] HTTP kode estandarrak errespetatu

[:octicons-arrow-right-24: Data-Retrieve inplementazio-gida](../semantica/data-retrieve/guia-implementacion.md)

---

## :material-numeric-5-circle: Mock-arekin probatu

- [ ] Espedienteen mock-a zabaldu
- [ ] Demo1 konektoreak zure endpoint-arekin konektatzen duela egiaztatu
- [ ] `personId` eta `dataTypeId` ezberdinekin probatu
- [ ] Erantzuna formatu egokian dagoela egiaztatu

[:octicons-arrow-right-24: Espedienteen Mock-a](./mock-expedientes.md)

---

## :material-numeric-6-circle: Autentifikazioa probatu

- [ ] Zure kredentzialekin token bat lortu
- [ ] Token-a sartu DENAri egiten dizkiozun deietan
- [ ] DENAk zure endpoint-arekin konektatzen duela egiaztatu
- [ ] Dei autentifikatuak funtzionatzen dutela egiaztatu

[:octicons-arrow-right-24: Autentifikazioa](../autenticacion/index.md)

---

## :material-numeric-7-circle: End-to-end balioztatu PRE-n

- [ ] DENAk zure endpoint erreala deitzen du proba-pertsona batekin
- [ ] Erantzuna CORE-ra behar bezala iristen dela egiaztatu
- [ ] Metadata-Sync probatu aplikagarria bada
- [ ] Person-Sync probatu aplikagarria bada

---

## :material-numeric-8-circle: PRO-ra pasatu

- [ ] PRO kredentzialak eskatu
- [ ] PRO endpoint-ekiko konektibitatea egiaztatu
- [ ] End-to-end proba errepikatu PRO-n
- [ ] Produkziora pasatzea berretsi DENA taldearekin

---

!!! success "Integrazioa osatuta!"

    Urrats guztiak gainditutakoan, zure administrazioa DENArekin integratuta egongo da eta erabiltzaileek beren datuak atzitu ahal izango dituzte aplikaziotik.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
