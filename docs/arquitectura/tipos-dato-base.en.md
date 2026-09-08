# :material-format-list-bulleted-type: Base Data Types

DENA uses a set of [**model objects**] that constitute one of the cornerstones of the system. This section describes the basic [model objects].

---

## Foundational types

### Boolean

`true` / `false` (always in lowercase).

### Numbers

| Type | Description | Example |
|------|-------------|---------|
| **Integer** | Signed integer number. 64-bit representation (Java Long). Range: [-9,223,372,036,854,775,808 .. 9,223,372,036,854,775,807] | `25` |
| **FloatingPoint** | Double-precision floating-point number. 64-bit representation (Java Double). Range: [4.9E-324 .. 1.8E+308] | `12.5` (dot as decimal separator) |

### Enums

They represent values in a predefined finite spectrum. They are serialized as text always in UPPERCASE.

```
ACTIVE / INACTIVE
```

### Language / Country

[Language] is a specialized [Enum] with the languages supported by DENA:

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

A plain text.

```
Sample text string
```

### LanguageTexts

A `Map<Language, String>` whose [key] is the [language] and the [value] a text in that [language]:

```json
{
  "SPANISH": "Expediente",
  "BASQUE": "Espediente",
  "ENGLISH": "Administrative Record"
}
```

### Date Time

| Type | Description | Format | Example |
|------|-------------|---------|---------|
| **LocalDate** | Date without time or time zone | `yyyy-MM-dd` | `2024-12-15` |
| **UTCDateTime** | Date and time with millisecond precision. Always UTC | `yyyy-MM-ddTHH:mm:ss.SSSZ` | `2023-12-16T13:00:00.000Z` |
| **OffsetDateTime** | Date and time with time zone | `yyyy-MM-ddTHH:mm:ss.SSSXXX` | `2026-02-05T14:30:15.123+01:00` |
| **TimeStamp** | An instant, always in UTC. EPOCH (seconds since 1970-01-01T00:00:00Z) | number | `1670374400` |

**Auxiliary types:**

| Type | Description | Example |
|------|-------------|---------|
| **TimeLapse** | Period of time: `{number}{d|h|m|s|ms|mus|ns}` | `5d` = 5 days, `3s` = 3 seconds |
| **TimeFrequency** | Enum: DAILY, WEEKLY, MONTHLY, QUARTERLY, YEARLY | `MONTHLY` |

### Ranges

They represent a discrete interval of numbers or dates with two bounds that may or may not be included:

| Notation | Meaning | Equivalence |
|----------|-------------|--------------|
| `[1..20]` | Between 1 and 20, both included | X >= 1 && X <= 20 |
| `[1..20)` | 1 included, 20 excluded | X >= 1 && X < 20 |
| `(1..20]` | 1 excluded, 20 included | X > 1 && X <= 20 |
| `(1..20)` | Both excluded | X > 1 && X < 20 |
| `[1..]` | Greater than or equal to 1 | X >= 1 |
| `(1..]` | Greater than 1 | X > 1 |
| `[..20]` | Less than or equal to 20 | X <= 20 |
| `[..20)` | Less than 20 | X < 20 |

!!! note "By default"
    In the DENA interoperability services, **inclusive bounds are used by default**.

**Common examples:**

| Type | Representation |
|------|---------------|
| Local Date Range | `[2023-01-01..2023-12-31]` |
| DateTime Range | `[2023-01-01T00:00:00.000Z..2023-01-01T13:00:00.000Z]` |
| Integer Range | `[12..15]` |
| Floating Point Range | `[1.5..5.5]` |

---

### UID / OID

A UID (Unique Identifier) and its specialization OID (Object Identifier) are a **universal unique identifier with no business meaning**, generated randomly.

**UIDs / OIDs are not repeated.**

Format: 36-character hexadecimal string organized into 5 groups separated by `-`.

```
db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb
```

!!! info "Short UIDs"
    Sometimes **SHORT UIDs** are used (shorter, e.g.: `db761b72`). They are used in special situations where the set of objects is very small or to name short-lived objects (e.g.: a message that expires).

### ID

IDs are **identifiers with business meaning**:

- A [person] identifier (e.g.: Spanish NIF)
- An [administrative record] number (e.g.: `EXP-2023-0004232`)
- The [IBAN] that identifies a [bank account]

**Format examples:**

- **IBAN**: `ES21999911110099999999`

![IBAN Format](../adjuntos/imagenes/arquitectura/iban-format.png)

- **DNI**: 8 digits + 1 letter
- **NIE**: 1 letter (X/Y/Z) + 7 digits + 1 letter
- **CIF**: 1 letter + 7 digits + 1 control character

### Token

Tokens are similar to [ID] but generally related to security.

### URLs

#### Url

Models a URL with: protocol, port, host, urlPath, queryString, anchor.

```
https://dena.eus/payments#concepts?param1=a&param2=b
```

#### UrlCollection

A collection of [Url]s where each item can have: ID, language, tags.

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

Models a [web link] with: url text, language, presentation attributes.

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

A collection of [WebLinks].

### Money

Type to represent monetary values (currency + amount).

### Hash (digest)

Digest of a text obtained by applying the **SHA-256** algorithm. Characteristics:

| Property | Description |
|---|---|
| **Fixed length** | From any input text a hash of fixed length and format is obtained |
| **Consistency** | The same input text always generates the same hash |
| **One-way** | It is practically impossible to obtain the original text from the hash |
| **Collision-resistant** | It is practically impossible for two different texts to generate the same hash |
| **Extreme sensitivity** | Any small change in the input generates a completely different hash |

**Example:**

| Original text | Hash (SHA-256) |
|---|---|
| `This is a random text to be hashed` | `2251f5ac60e4fc24a62914bf92a9adf3af1abbf72d649eceec9ed7c3a2e29b8f` |

### UserAgent

String that identifies the client making an HTTP request. It follows a structured pattern according to the type of origin:

| Origin | Format | Example |
|---|---|---|
| DENA mobile app | `DenaApp/<ver> (<OS>; <device>) Dart/<ver> Flutter/<ver>` | `DenaApp/1.0 (Android 13; Pixel 6 Pro) Dart/3.0 Flutter/3.16` |
| DENA-CORE | `DENA-CORE/<ver> <module>/<ver> (support@dena.eus)` | `DENA-CORE/2.1.0 person-data/1.0.1 (support@dena.eus)` |
| Administration | `AdminX/<ver> <module>/<ver> (support@adminX.eus)` | `AdminX/1.1.0 expedientes/1.0.0 (support@adminX.eus)` |
| Web browser | Standard browser format | `Mozilla/5.0 (Macintosh; ...) Chrome/143.0.0.0 ...` |

For more detail: [:octicons-arrow-right-24: UserAgent](../semantica/semantica-base/modelo/user-agent.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
