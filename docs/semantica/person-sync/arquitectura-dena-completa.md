# :material-file-document: Arquitectura DENA - Documentación Completa

> Este documento contiene la información extraída del archivo `DENA-Architecture.docx` y `DENA-CORE-Services_for_admins.docx` para referencia.

---

## 1. Visión General de DENA

### 1.1 Premisas de DENA

DENA es un sistema de interoperabilidad entre **personas** y **administraciones**, no entre administraciones.

**Principios fundamentales:**

- DENA-APP recupera los datos directamente de la administración a través de DENA-CORE actuando como PROXY
- **NO se almacena ningún dato en DENA-CORE**
- Cada vez que una persona recupera datos de una administración, un consentimiento proporcionado por la persona es obligatorio
- DENA-APP almacena los datos recuperados en una caché local en el dispositivo del usuario
- DENA-APP debe conocer:
  - **Procedencia de datos**: qué administraciones tienen información sobre una persona
  - **Cambios en los datos de origen**: permite saber si la caché local requiere actualización

### 1.2 Conceptos Clave de DENA

| Concepto | Descripción |
|----------|-------------|
| **[person]** | Individuo que se registra en DENA (crea una cuenta DENA) |
| **[admins]** | Entidades públicas que exponen datos al ecosistema DENA |
| **DENA-APP** | Aplicación móvil o web usada por las personas para acceder a sus datos |
| **DENA-CORE** | Sistema que proporciona servicios para DENA-APP y la interoperabilidad |
| **[data origin]** | Fuente de datos de una persona en una administración |
| **SRMD** | Sync And Retrieve Meta-Data: metadatos sobre cambios en los datos |
| **[admin connector]** | Componente de DENA-CORE que intermedia con las administraciones |
| **[data provider]** | Servicio expuesto por la administración para proporcionar datos |

---

## 2. Servicios CORE para Administraciones

DENA-CORE ofrece los siguientes servicios para las administraciones:

### 2.1 SRMD Sync: Envío de información sobre cambios

Mecanismo para que las administraciones envíen información sobre cambios en los datos a DENA-CORE.

![SRMD Sync Overview](../../adjuntos/imagenes/image1.png)

**Flujo de alto nivel:**

1. La administración consulta su BD de negocio buscando elementos que han cambiado desde el último ciclo
2. Para cada elemento cambiado, crea una estructura SRMD:
   ```
   {person} | {data type} | {admin} | {last update instant}
   ```
3. La administración envía TODOS los SRMD a DENA-CORE mediante un servicio REST

**Estrategias de implementación:**

- **Un data provider** para un único data type en una administración
- **Data providers agregados** que combinan datos de múltiples orígenes
- **Múltiples data providers** para múltiples orígenes en diferentes instancias

**Cuando una administración tiene múltiples data origins para un mismo data type, el SRMD incluye:**

```
{person} | {admin} | {data type} | {data origin instance} | {last update instant}
```

### 2.2 Data Retrieval: Exposición de servicios

DENA-CORE actúa como intermediario/proxy para que DENA-APP recupere datos de las administraciones.

![Data Retrieval Overview](../../adjuntos/imagenes/image5.png)

**Flujo deRetrieval:**

1. Cliente llama a DENA-CORE solicitando datos (data type, admin, person)
2. DENA-CORE traduce el mensaje a objetos del modelo
3. DENA-CORE determina el connector a usar
4. El connector llama al data provider de la administración
5. Los datos se traducen al formato estándar si es necesario
6. DENA-CORE determina el estado de "ha cambiado"
7. La respuesta llega al cliente

**La administración puede:**
- Implementar el data provider usando semánticas estándar DENA
- Implementar semánticas personalizadas (SOAP, X-509, formato propio)

---

## 3. Mecanismo de Intercambio de Datos

### 3.1 SYNC (Meta-data sobre cambios)

DENA-APP usa una caché local donde se almacenan todos los datos recuperados. Para mantenerla actualizada:

1. Las administraciones envían periódicamente SRMD a DENA-CORE con cambios en los tipos de datos
2. DENA-APP obtiene los SRMD para la persona logueada y los almacena en su BD local
3. Comparando SRMD local con el de DENA-CORE, DENA-APP sabe qué datos han cambiado

**Estructura del SRMD:**
```
[person] | [admin] | [data type] | [data origin instance] | [last update instant]
```

### 3.2 RETRIEVE (Recuperación de datos)

Una vez detectado un cambio, DENA-APP recupera los datos completos:

1. DENA-APP llama a un servicio REST de DENA-CORE
2. DENA-CORE traduce el mensaje a objetos del modelo
3. DENA-CORE determina qué connector usar
4. El connector llama al proveedor de datos de la administración
5. Los datos se traducen al formato estándar si es necesario
6. DENA-CORE determina el estado de "ha cambiado" de cada elemento
7. La respuesta llega a DENA-APP

### 3.3 El Problema COLD-START

Cuando una persona se registra en DENA, no existe SRMD porque las administraciones NO envían SRMD para personas no registradas.

**Solución:**
DENA-CORE inserta entradas SRMD como si las administraciones las hubieran enviado para administraciones clave (EJGV, DFB, DFG, DFA, Bilbao, Gasteiz, Donostia).

---

## 4. Person-Sync Detallado

### 4.1 Datos Almacenados por Persona

Cada persona registrada en DENA tiene:

- **Person OID**: Identificador único generado por DENA
- **Person ID**: El NIF
- **Full Name**: nombre, apellido1, apellido2
- **Contact Data**: dirección, teléfono, email...
- **Preferred Themes**: para personalización

### 4.2 Mecanismos de Sincronización

#### DENA-PUSH: DENA notifica a la administración

DENA envía un mensaje cuando:
- Nueva persona se registra
- Persona elimina su cuenta
- Persona cambia datos básicos (nombre, contacto...)

![DENA Push Overview](../../adjuntos/imagenes/image7.png)

#### ADMIN-PULL: La administración consulta

**Modalidades:**

| Modalidad | Descripción |
|-----------|-------------|
| **On-line** | Consulta REST en tiempo real para obtener datos de personas |
| **Off-line (pre-generado)** | Ficheros periódicos con listados de personas |
| **Off-line (bespoke)** | Ficheros personalizados generados bajo demanda |

**Ejemplo de Pull Online:**

```
POST /persons/search
{
  "orgAdminRef": { "id": "admin1" },
  "personQuery": {
    "personIds": ["40404040H", "12345678Z"]
  }
}
```

**Ejemplo de Pull Offline - Ficheros pre-generados:**

```
POST /pre-generated
{
  "jobType": "ALL_PERSONS",
  "exportType": "DATA",
  "fileFormat": "CSV",
  "hourOfDay": "20"
}
```

**Ejemplo de Pull Offline - Bespoke:**

```
POST /bespokes
{
  "orgAdminRef": { "id": "admin1" },
  "exportSpec": {
    "personExportSpec": "data",
    "lastUpdateRange": "Instant:[2026-08-24T21:19:41.314878600Z..)",
    "exportContentSpec": {
      "exportDefaultContactData": true,
      "exportOtheContactData": true,
      "exportFinData": true
    },
    "exportFileFormat": "CSV"
  }
}
```

!!! warning "Diferencia importante"

    Person-Sync ≠ Contact Data como tipo de dato:
    - **Person-Sync**: Acceso a datos de cuenta de personas en DENA
    - **Contact Data**: Tipo de dato interoperable para que las personas vean sus datos de contacto

---

## 5. Arquitectura de Servicios

### 5.1 Visión General

Los principales bloques de la arquitectura son:

- **Client Device UI**: DENA-APP
- **DENA-CORE**:
  - Módulo de Configuración (estructura organizativa, config de interoperabilidad)
  - Módulo de Personas (cuentas DENA, sincronización)
  - Módulo de Sync and Retrieve (SRMD)
  - Módulo de Consentimientos
  - Módulo de Seguridad
- **Admin Connectors**: Traducción entre semánticas estándar y específicas

![Architecture Overview](../../adjuntos/imagenes/image2.png)

### 5.2 Conectores

Un **conector** traduce las semánticas de DENA usadas internamente a las semánticas específicas del proveedor de datos de la administración.

![Connectors Architecture](../../adjuntos/imagenes/image6.png)

**Dos lados del conector:**
1. **Lado interno**: Usa semánticas estándar de DENA (transporte, seguridad, formato)
2. **Lado externo**: Sabe cómo interactuar con el proveedor remoto (URL, headers, autenticación, formato de datos)

Los conectores son módulos independientes desplegados independientemente de DENA-CORE para hacer el sistema más resiliente.

---

## 6. Tipos de Datos Base

### 6.1 Tipos Fundamentales

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| **Boolean** | Valores true/false | `true` |
| **Numbers** | Enteros y decimales | `42`, `3.14` |
| **Enums** | Valores de un conjunto definido | `LANG: SPANISH \| BASQUE \| ENGLISH` |
| **String** | Cadenas de texto | `"Euskadi"` |
| **LanguageTexts** | Textos multilingües | `{ "es": "Hola", "eu": "Kaixo" }` |
| **DateTime** | Instantes temporales | `2024-01-15T10:30:00Z` |
| **UID/OID** | Identificadores únicos | `urn:uuid:12345678-1234...` |
| **ID** | Identificadores con significado business | NIF, IBAN, número de expediente |
| **Money** | Cantidades monetarias | `{ "base": "CENTS", "currency": "EUR", "amount": 249333 }` |
| **Hash** | Digest SHA-256 | `2251f5ac60e4fc24a62914bf92a9adf3af1abbf72d649eceec9ed7c3a2e29b8f` |
| **UserAgent** | Información del cliente | `DenaApp/1.0 (Android 13; Pixel 6 Pro)...` |

### 6.2 Estructuras Complejas

- **Urls**: URL con componentes parseados
- **WebLink**: Enlace web con textos multilingües
- **WebLinkCollection**: Colección de WebLinks
- **Ranges**: Rangos de valores
- **Token**: Tokens de seguridad

---

## 7. Intercambio de Datos: Ejemplos JSON

### 7.1 SRMD enviado de Admin a DENA-CORE

```json
{
  "admin": { "id": "EJGV" },
  "aboutPerson": { "id": "40404040H" },
  "someDataWasUpdatedAt": "2026-08-17T15:14:07.0369127Z",
  "ofType": { "id": "ADMIN_NOTICE" },
  "fromDataOrigin": "DEFAULT"
}
```

### 7.2 Respuesta de DENA-CORE al SRMD

```json
{
  "transactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
  "receivedItemsCount": 3,
  "processedOK": [
    {
      "oid": "4F4EF685-BA83-4BAB-80CD-6B01BBE7939C",
      "lastTransactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
      "admin": { "id": "EJGV" },
      "dataType": { "id": "ADMIN_NOTICE" },
      "dataOriginInstanceId": "DEFAULT",
      "person": { "id": "40404040H" },
      "dataLastUpdatedAt": "2026-08-17T15:14:07.0324014Z"
    }
  ],
  "processedNOK": []
}
```

---

## 8. Seguridad

### 8.1 Autenticación de Persona

- Login con **GILTZA OAUTH token**
- Posteriormente uso de **Passkeys** para evitar requerir GILTZA nuevamente

### 8.2 Autenticación de DENA-APP con DENA-CORE

- Token OAUTH del **DENA-IdP**

### 8.3 Autenticación de Admin con DENA-CORE

- Token OAUTH del **DENA-IdP**

### 8.4 Autenticación de DENA-CORE con Admin

- Sistema de autenticación preferido por la administración destino
- OAUTH, X-509 certificate, user/passwd...

---

## 9. Configuración de DENA

### 9.1 Sistema de Etiquetado

Define la estructura organizativa de las administraciones.

### 9.2 Configuración de Interoperabilidad

| Elemento | Descripción |
|----------|-------------|
| **Data Type** | Tipos de datos ofrecidos por las administraciones |
| **Data Origin** | Configuración usada por los conectores |
| **AppVersion** | Historial de versiones de la aplicación |

---

## 10. Imágenes de Arquitectura

Las siguientes imágenes están disponibles en `docs/adjuntos/imagenes/`:

| Archivo | Descripción |
|---------|-------------|
| `image1.png` | Diagrama de conceptos SRMD |
| `image2.png` | Arquitectura general de conectores |
| `image3.svg` | Flujo de sincronización |
| `image5.png` | Vista de Data Retrieval |
| `image6.svg` | Arquitectura de conectores detallada |
| `image7.png` | DENA Push Overview |
| `image9.png` | Data interchange flow |
| `image10.png` | High level architecture |
| `image11.png` | Implementación de ejemplo |
| `image12.png` | Datos intercambiados |

---

**Última actualización:** Extraído de DENA-Architecture.docx y DENA-CORE-Services_for_admins.docx

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>