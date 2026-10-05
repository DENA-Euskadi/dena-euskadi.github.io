# :material-sitemap: Integrazio-erreferentziako arkitekturak

> [DENA-CORE — Services for Admins](../adjuntos/documentos/DENA-CORE-Services_for_admins.pdf) dokumentuaren *"Integration reference architectures"* atalaren edukia (:material-download: PDF).

Administrazio askok dozenaka datu-mota eta datu-jatorri dituzte DENAn integratzeko, eta banan-banako ikuspegia ez da irtenbiderik onena, gauza bera egiten duten hainbat osagai desberdin izatera baitakar (adibidez, SRMD sinkronizazio-metadatuak DENAra bidaltzen dituzten hainbat osagai).

Beraz, ikuspegi zentralizatuago batek datu-jatorri anitzak DENAn integratzen lagun dezake.

Ondorengo irudiak edozein administraziotako hiru integrazio-arloak irudikatzen dituen ikuspegi bat erakusten du:

1. Person sync
2. SRMD sinkronizazio-metadatuak bidaltzea
3. Datuak berreskuratzeko zerbitzuak (*data provider*)

![DENA integrazio-erreferentziako arkitektura](../adjuntos/imagenes/documentos/arquitectura-referencia-integracion.png)

---

## 5.1 Person Sync

*Person sync* normalean administrazioko behin egiten da, eta horrela DENAn integratutako administrazio bakoitzak pertsonen datu-basearen erreplika bakarra du.

Integrazio hau zuzena da DENAk emandako artefaktuak erabiliz, edozein administrazioren datacenter-ean zabal daitezkeenak.

---

## 5.2 SRMD sinkronizazio-metadatuak bidaltzea

Datu-jatorri batek DENAn integratzeko egin behar duen lehen gauza aldaketak bidaltzea da. Horretarako, administrazioak aldaketa-biltzaile zentralizatu bat zabal dezake [Apache NiFi] erabiliz, datu-jatorri bakoitzaren [DB view] bat aldaketen bila monitorizatzen duena eta, detektatzen dituenean, [mezu] bat bidaltzen diona [Apache Kafka] bati aldaketekin, honek DENAra bidaltzen dituenak.

---

## 5.3 Data Retrieve

Datuak berreskuratzeko, datu-jatorriaren datuak (edo horien kopia bat) zuzenean atzitu behar dira.

Modurik errazena negozio-arloari DENAn erakutsiko diren datuen [DB view] bat sortzeko eskatzea da (normalean negozio-datu osoen zutabe gutxi batzuk soilik) eta zabaltzea. [DB view] hori [data provider] zerbitzu baten iturria da.

Askotan, [data provider] zerbitzu bakoitza zabaltzeko modu erosoa datu-jatorriaren [data provider] guztiak zerbitzu komun bakar batean biltzea eta datu-sarbidea URL bideen bidez banatzea da:

```
https://dena-internal.my_admin.eus/dena-data-provider/data-type1/byperson/{personId}
https://dena-internal.my_admin.eus/dena-data-provider/data-type2/byperson/{personId}
https://dena-internal.my_admin.eus/dena-data-provider/data-type3/byperson/{personId}
…
```

---

## Dokumentu osoa

[:material-download: DENA-CORE — Services for Admins (PDF)](../adjuntos/documentos/DENA-CORE-Services_for_admins.pdf) · [Dokumentu guztiak (PDF)](../adjuntos/documentos.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
