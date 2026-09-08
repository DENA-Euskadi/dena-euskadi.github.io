# :material-format-list-bulleted-type: Oinarrizko Datu Motak

DENAk sistemaren oinarrietako bat osatzen duten [**model objects**] multzo bat erabiltzen du. Atal honek oinarrizko [model objects] deskribatzen ditu.

---

## Oinarrizko motak

### Boolean

`true` / `false` (beti minuskulaz).

### Numbers

| Mota | Deskribapena | Adibidea |
|------|-------------|---------|
| **Integer** | Zeinudun zenbaki osoa. 64 bit-eko adierazpena (Java Long). Barrutia: [-9.223.372.036.854.775.808 .. 9.223.372.036.854.775.807] | `25` |
| **FloatingPoint** | Doitasun bikoitzeko koma mugikorreko zenbakia. 64 bit-eko adierazpena (Java Double). Barrutia: [4.9E-324 .. 1.8E+308] | `12.5` (puntua hamartar bereizle gisa) |

### Enums

Aurrez definitutako espektro finitu bateko balioak adierazten dituzte. Beti MAIUSKULAZ testu gisa serializatzen dira.

```
ACTIVE / INACTIVE
```

### Language / Country

[Language] DENAk onartzen dituen hizkuntzekin espezializatutako [Enum] bat da:

| Enum Val | Int code | iso639_1 | iso639_2 |
|----------|----------|----------|----------|
| SPANISH | 10 | es | spa |
| BASQUE | 11 | eu | eus |
| ENGLISH | 20 | en | eng |
| FRENCH | 30 | fr | fra |
| DEUTCH | 40 | de | deu |
| ITALIAN | 58 | it | ita |
| PORTUGUESE | 59 | pt | por |

| Country Enum | Val | iso3166_1 | iso3166_2 |
|-------------|-----|-----------|-----------|
| SPAIN | 724 | ES | ESP |
| UNITED_KINGDOM | 826 | GB | GBR |
| FRANCE | 250 | FR | FRA |
| GERMANY | 276 | DE | DEU |
| ITALY | 380 | IT | ITA |
| PORTUGAL | 620 | PT | PRT |

### String

Testu soil bat.

```
Sample text string
```

### LanguageTexts

`Map<Language, String>` bat, non [gakoa] [hizkuntza] den eta [balioa] hizkuntza horretako testu bat:

```json
{
  "SPANISH": "Expediente",
  "BASQUE": "Espediente",
  "ENGLISH": "Administrative Record"
}
```

### Date Time

| Mota | Deskribapena | Formatua | Adibidea |
|------|-------------|---------|---------|
| **LocalDate** | Data ordurik eta ordu-eremurik gabe | `yyyy-MM-dd` | `2024-12-15` |
| **UTCDateTime** | Data eta ordua milisegundoko doitasunarekin. Beti UTC | `yyyy-MM-ddTHH:mm:ss.SSSZ` | `2023-12-16T13:00:00.000Z` |
| **OffsetDateTime** | Data eta ordua ordu-eremuarekin | `yyyy-MM-ddTHH:mm:ss.SSSXXX` | `2026-02-05T14:30:15.123+01:00` |
| **TimeStamp** | Une bat, beti UTC. EPOCH (segundoak 1970-01-01T00:00:00Z-tik) | zenbakia | `1670374400` |

**Mota lagungarriak:**

| Mota | Deskribapena | Adibidea |
|------|-------------|---------|
| **TimeLapse** | Denbora-tartea: `{zenbakia}{d|h|m|s|ms|mus|ns}` | `5d` = 5 egun, `3s` = 3 segundo |
| **TimeFrequency** | Enum: DAILY, WEEKLY, MONTHLY, QUARTERLY, YEARLY | `MONTHLY` |

### Ranges

Sartuta egon daitezkeen edo ez daitezkeen bi mugarekin zenbaki edo dataen tarte diskretu bat adierazten dute:

| Notazioa | Esanahia | Baliokidetasuna |
|----------|-------------|--------------|
| `[1..20]` | 1 eta 20 artean, biak barne | X >= 1 && X <= 20 |
| `[1..20)` | 1 barne, 20 kanpo | X >= 1 && X < 20 |
| `(1..20]` | 1 kanpo, 20 barne | X > 1 && X <= 20 |
| `(1..20)` | Biak kanpo | X > 1 && X < 20 |
| `[1..]` | 1 baino handiagoa edo berdina | X >= 1 |
| `(1..]` | 1 baino handiagoa | X > 1 |
| `[..20]` | 20 baino txikiagoa edo berdina | X <= 20 |
| `[..20)` | 20 baino txikiagoa | X < 20 |

!!! note "Lehenetsita"
    DENA elkarreragingarritasun-zerbitzuetan, **lehenespenez muga inklusiboak erabiltzen dira**.

**Adibide ohikoak:**

| Mota | Adierazpena |
|------|---------------|
| Local Date Range | `[2023-01-01..2023-12-31]` |
| DateTime Range | `[2023-01-01T00:00:00.000Z..2023-01-01T13:00:00.000Z]` |
| Integer Range | `[12..15]` |
| Floating Point Range | `[1.5..5.5]` |

---

### UID / OID

UID (Unique Identifier) bat eta bere espezializazioa OID (Object Identifier) **negozio-esanahirik gabeko identifikatzaile unibertsal bakarra** dira, ausaz sortua.

**UIDak / OIDak ez dira errepikatzen.**

Formatua: 36 karaktereko kate hamaseitarra, `-` bidez bereizitako 5 taldetan antolatua.

```
db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb
```

!!! info "Short UIDs"
    Batzuetan **SHORT UIDs** erabiltzen dira (laburragoak, adib.: `db761b72`). Objektu-multzoa oso txikia denean edo iraupen laburreko objektuak izendatzeko egoera berezietan erabiltzen dira (adib.: iraungitzen den mezu bat).

### ID

IDak **negozio-esanahia duten identifikatzaileak** dira:

- [Pertsona] baten identifikatzailea (adib.: espainiar NIF)
- [Administrazio-espediente] baten zenbakia (adib.: `EXP-2023-0004232`)
- [Banku-kontu] bat identifikatzen duen [IBAN]a

**Formatu-adibideak:**

- **IBAN**: `ES21999911110099999999`

![IBAN Format](../adjuntos/imagenes/arquitectura/iban-format.png)

- **DNI**: 8 digitu + letra 1
- **NIE**: letra 1 (X/Y/Z) + 7 digitu + letra 1
- **CIF**: letra 1 + 7 digitu + kontrol-karaktere 1

### Token

Tokenak [ID] antzekoak dira, baina oro har segurtasunarekin lotuta.

### URLs

#### Url

URL bat modelatzen du honekin: protocol, port, host, urlPath, queryString, anchor.

```
https://dena.eus/payments#concepts?param1=a&param2=b
```

#### UrlCollection

[Url] batzuen bilduma bat, non item bakoitzak izan ditzakeen: ID, language, tags.

```json
[
  {
    "id": "mipago",
    "url": "https://www.euskadi.eus/nireordainketa",
    "lang": "BASQUE"
  },
  {
    "id": "mipago",
    "url": "https://www.euskadi.eus/mipago",
    "lang": "SPANISH"
  }
]
```

#### WebLink

[Web link] bat modelatzen du honekin: url text, language, aurkezpen-atributuak.

```json
{
  "url": "https://www.euskadi.eus/nireordainketa",
  "texts": {
    "lang": "BASQUE",
    "title": "Euskal administrazioen ordainketa-pasabidea",
    "description": "Admin baten edozein likidazio ordaintzeko aukera ematen du"
  }
}
```

#### WebLinkCollection

[WebLinks] bilduma bat.

### Money

Balio monetarioak adierazteko mota (moneta + kopurua).

### Hash (digest)

**SHA-256** algoritmoa aplikatuz lortutako testu baten laburpena. Ezaugarriak:

| Propietatea | Deskribapena |
|---|---|
| **Luzera finkoa** | Edozein sarrera-testutik luzera eta formatu finkoko hash bat lortzen da |
| **Koherentzia** | Sarrera-testu berak beti hash bera sortzen du |
| **Norabide bakarrekoa** | Ia ezinezkoa da jatorrizko testua hashetik lortzea |
| **Talkekiko erresistentea** | Ia ezinezkoa da bi testu desberdinek hash bera sortzea |
| **Sentikortasun muturrekoa** | Sarreran edozein aldaketa txikik erabat desberdina den hash bat sortzen du |

**Adibidea:**

| Jatorrizko testua | Hash (SHA-256) |
|---|---|
| `This is a random text to be hashed` | `2251f5ac60e4fc24a62914bf92a9adf3af1abbf72d649eceec9ed7c3a2e29b8f` |

### UserAgent

HTTP eskaera bat egiten duen bezeroa identifikatzen duen String-a. Jatorri motaren arabera egituratutako patroi bat jarraitzen du:

| Jatorria | Formatua | Adibidea |
|---|---|---|
| DENA mugikor-aplikazioa | `DenaApp/<ver> (<OS>; <device>) Dart/<ver> Flutter/<ver>` | `DenaApp/1.0 (Android 13; Pixel 6 Pro) Dart/3.0 Flutter/3.16` |
| DENA-CORE | `DENA-CORE/<ver> <module>/<ver> (support@dena.eus)` | `DENA-CORE/2.1.0 person-data/1.0.1 (support@dena.eus)` |
| Administrazioa | `AdminX/<ver> <module>/<ver> (support@adminX.eus)` | `AdminX/1.1.0 expedientes/1.0.0 (support@adminX.eus)` |
| Web-nabigatzailea | Nabigatzailearen formatu estandarra | `Mozilla/5.0 (Macintosh; ...) Chrome/143.0.0.0 ...` |

Xehetasun gehiagorako: [:octicons-arrow-right-24: UserAgent](../semantica/semantica-base/modelo/user-agent.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
