# :material-account-sync: Person-Sync (Pertsonak sinkronizatu)

## Kontzeptua

Person-Sync zure administrazioari **DENAn zer pertsona inskribatuta dauden jakiteko** aukera ematen dion operatiba da. Hau beharrezkoa da zure admin-ak soilik SRMD (aldaketa-jakinarazpenak) bidal ditzan benetan DENA kontua duten pertsonentzat.

Gainera, DENAk pertsonaren oinarrizko datuak partekatzen ditu zure admin-arekin: NIF, izena, abizenak, harremanetarako datuak.

![Person-Sync Overview](../../adjuntos/imagenes/image18.png)

### Pertsonen Datuak Sinkronizatuta

DENAn inskribatutako pertsona bakoitzak kontu bat du honako hauek gordetzen dituena:

- **Pertsona OID**: DENAk sortutako identifikadore bakarra
- **Pertsona ID**: NIF
- **Izen-Osoa**: izena, abizena1, abizena2
- **Harremanetarako Datuak**: helbidea, telefonoa, e-posta...
- **Gai Nahiagoak**: zerbitzu proaktiboetan erabili edo UI pertsonalizatzeko erabil daitezke

### Zertarako balio du?

- **SRMD norentzat bidali jakin**: zentzua du DENAn dauden pertsonen aldaketak jakinaraztea soilik
- **Zure DB sinkronizatuta mantendu**: pertsona bat DENAn inskribatzen denean edo baja ematen duenean, zure admin-a jakinaren gainean jartzen da
- **Oinarrizko datuetara sartu**: izena, kontaktua, gai-lehentasunak

---

## Mekanismo erabilgarriak

DENAk bi mekanismo osagarri eskaintzen ditu:

### Push: DENAk jakinarazten dizu

DENAk mezu bat bidaltzen dio zure admin-eko zerbitzu bati honako hauek gertatzen diren bakoitzean:

- Pertsona berri bat DENAn inskribatzen da
- Pertsona batek bere kontua ezabatzen du
- Pertsona batek bere oinarrizko datuak aldatzen ditu (izena, kontaktua...)

Zure admin-ak endpoint bat erakusten du eta DENAk automatikoki deitzen du aldaketak daudenean.

### Pull: zure admin-ak kontsultatzen du

Zure admin-ak DENAri kontsulta egiten dio nahi duenean, bi modutan:

| Modalitatea | Deskribapena |
|-----------|-------------|
| **On-line** | Zure admin-ak DENAko REST zerbitzu bati deitzen dio pertsonak denbora errealean kontsultatzeko |
| **Off-line (aurrez sortua)** | DENAk aldizkako fitxategiak sortzen ditu pertsonen zerrendarekin |
| **Off-line (bespoke)** | Zure admin-ak fitxategi pertsonalizatu bat eskatzen du eta DENAk eskaeraz sortzen du |

![Person-Sync Pull Flow](../../adjuntos/imagenes/person-sync-pull.png)

---

## Zein aukeratu?

| Egoera | Gomendioa |
|-----------|---------------|
| Momentuan jakin behar duzu norbait inskribatzen denean | **Push** |
| Gaueko batch prozesu bat duzu sinkronizatzeko | **Pull off-line** |
| Pertsonak eskaeraz kontsultatu nahi dituzu | **Pull on-line** |
| Biak nahi dituzu (gomendatua) | **Push** + **Pull off-line** babeskopia gisa |

---

## Zehaztapen osoa

Endpoint-en, ereduen eta fitxategien zehaztapen xehatua ikusteko:

| Mekanismoa | Dokumentazioa |
|-----------|---------------|
| Push | [:octicons-arrow-right-24: Push Endpoint-a](../semantica/person-sync/push.md) |
| Pull | [:octicons-arrow-right-24: Pull Endpoint-a](../semantica/person-sync/pull.md) |
| Ikuspegi orokorra | [:octicons-arrow-right-24: Person-Sync Semantika](../semantica/person-sync/index.md) |

---

**Hurrengoa:** [:octicons-arrow-right-24: Semantika (datuen zehaztapen teknikoa)](../semantica/index.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
