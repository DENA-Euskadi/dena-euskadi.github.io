# :material-account-key: Security context-aren erabiltzaile-eredua

Eskaera bat DENA-CORErengana iristen denean, eskaera sortzen duen aldea **erabiltzaile** batez adierazten da **security context**-aren barruan. DENAk hainbat erabiltzaile mota bereizten ditu, nork jarduten duen arabera: pertsona bat, kudeaketako barne-erabiltzaile bat, edo sistema gisa jarduten duen administrazio bat.

Guztiek marka-interfaze komun bat partekatzen dute, `DN00IsDENAUser`, security context-aren barruan modu homogeneoan erabili ahal izateko.

---

## Erabiltzaile motak

| Mota | Klasea | Zer adierazten du | Identifikatzaile nagusia |
|------|--------|-------------------|--------------------------|
| Pertsona | `DN00DENAPersonUser` | Identitate-hornitzaile baten bidez autentifikatutako pertsona | Pertsonaren erreferentzia + Identity Broker-eko ID |
| Kudeaketa | `DN00DENAManagementUser` | Kudeaketako barne-erabiltzailea | Lanpostuaren kodea |
| Administrazio-sistema | `DN00DENAAdminSystemUser` | Sistema gisa jarduten duen administrazioa (atzean pertsonarik gabe) | Administrazioaren erreferentzia |

---

## Pertsona erabiltzailea

`DN00DENAPersonUser` (`personUser` JSONen) pertsona baten eta Identity Broker-eko (Keycloak, WSO2...) bere erabiltzaile teknikoaren arteko **identitate-zubi** gisa jarduten du.

Garrantzitsua da kontzeptuak ez nahastea:

- **Pertsona** sisteman kontua duen banakoa da.
- **Erabiltzailea** Identity Broker-ak kudeatzen duen segurtasun-identitate teknikoa da.

Pertsona erabiltzaileak biak lotzen ditu gutxieneko datu-multzo batekin:

| Eremua | JSON | Deskribapena |
|--------|------|--------------|
| Pertsonaren erreferentzia | `personRef` | Pertsonaren OIDa negozio-datu-basean |
| Broker-eko erabiltzaile-IDa | `personUserId` | Identity Broker-ak esleitutako IDa (adib. Keycloak-en `user_id`) |
| Izena | `givenName` | Pertsonaren izena |
| Abizenak | `familyName` | Pertsonaren abizenak |

!!! info "Identitate-fluxua"
    Pertsona identitate-hornitzaile batean autentifikatzen da (adibidez GILTZA bere NANarekin). Identity Broker-ak dagokion erabiltzailea aurkitzen edo sortzen du eta saio bati lotzen dio. Pertsonaren erreferentziarekin (`personRef`), aplikazioak zehazki daki zein pertsona fisikori dagokion erabiltzailea, eta bere rolak eta negozio-datuak kargatu ditzake.

---

## Kudeaketa erabiltzailea

`DN00DENAManagementUser` (`internalUser` JSONen) **kudeaketako barne-erabiltzaile** bat adierazten du.

| Eremua | JSON | Deskribapena |
|--------|------|--------------|
| Lanpostuaren kodea | `workPlaceCode` | Erabiltzaileari lotutako lanpostua |

---

## Administrazio-sistemaren erabiltzailea

`DN00DENAAdminSystemUser` (`adminSystemUser` JSONen) **sistema gisa jarduten duen administrazio** bat adierazten du, hau da, jarduten duena pertsona bat ez baizik eta administrazioa bera denean (adibidez, `client_credentials` erabiliz makinatik makinarako deietan).

| Eremua | JSON | Deskribapena |
|--------|------|--------------|
| Administrazioa | `admin` | Administrazioaren erreferentzia (`DN00OrgAdminRef`) |

Bi sistema-erabiltzaile subjektu bera direla jotzen da administrazio bera erreferentziatzen dutenean (OIDaren edo IDaren arabera). Erabiltzailea bistaratzeko izena erreferentziatutako administrazioarena da.

!!! note "Autentifikazio-eremuekin lotura"
    Erabiltzaile mota hau autentifikazioaren **3. Eremuarekin** bat dator (administrazio bat DENA-COREren aurrean autentifikatzen da `client_credentials` bidez). Ikusi [Segurtasuna eta Autentifikazioa](index.md) eremuen xehetasunetarako.

---

## Balidazioa

Hiru erabiltzaile motak `DN00DENAUserValidator` bidez autobalidatzen dira, erabiltzaile-ereduaren balidazio-arau komunak aplikatuz.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
