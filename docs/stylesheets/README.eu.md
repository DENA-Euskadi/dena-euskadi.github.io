# DENA Stylesheets - Egitura Modularra

Direktorio honek CSS estiloak modu modularrean antolatuta ditu, mantentze-lana eta kodearen ulermena errazteko.

## Fitxategiak eta erantzukizunak

### 01-variables.css
- **DENA kolore-paleta korporatiboa**
- **Tipografia ofizialak (Manrope)**
- **Berrerabilgarriak diren CSS aldagaiak**
- **Google Fonts inportazioa**

### 02-material-overrides.css
- **Material for MkDocs override-ak**
- **Oinarrizko temaren konfigurazioa**
- **Kolore eta tipografia nagusiak**
- **Estekak eta oinarrizko elementuak**

### 03-header-navigation.css
- **Goiburu nagusia DENA logoarekin**
- **Fitxen bidezko nabigazioa**
- **Goiburuaren elementu interaktiboak**
- **Hover efektuak eta egoera aktiboak**

### 04-ui-elements.css
- **Alboko nabigazioa**
- **Botoiak eta formulario-elementuak**
- **Bilatzailea**
- **UI elementu orokorrak**

### 05-content-elements.css
- **Admonition-ak (oharrak, abisuak, etab.)**
- **Egoera-badge-ak eta HTTP metodoak**
- **Grid card-ak hover efektuekin**
- **Edukiari lotutako elementu espezifikoak**

### 06-mermaid-diagrams.css
- **Mermaid diagramentzako estiloak**
- **DENA tipografia diagrametan**
- **Kolore korporatiboak elementu grafikoetarako**
- **Diseinu moldagarria diagrametarako**

## Karga-ordena

Fitxategiak ordena numerikoan kargatzen dira estilo-kaskada zuzena bermatzeko:

1. Aldagaiak eta oinarrizko konfigurazioa
2. Material Design override-ak
3. Goiburua eta nabigazioa
4. UI elementuak
5. Eduki-elementuak
6. Diagrama espezifikoak

## Mantentzea

- **Aldagai zentralizatuak**: Kolore-aldaketak `01-variables.css`-en soilik
- **Erantzukizunen banaketa**: Fitxategi bakoitzak helburu espezifiko bat du
- **Debug erraztasuna**: Erroreak moduluka lokalizatzeko errazagoak
- **Eskalagarritasuna**: Modulu berriak gehitzeko erraza lehendik daudenei eragin gabe

## extra.css-etik migrazioa

Jatorrizko `extra.css` fitxategia zatitu egin da lehendik zegoen funtzionalitate guztia mantenduz, baina antolaketa eta irakurgarritasun hobearekin.
