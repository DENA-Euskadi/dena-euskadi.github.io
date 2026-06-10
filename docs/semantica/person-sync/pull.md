# PERSON-SYNC — PULL

Para ello, todos los días se generan cada hora exportaciones de los usuarios nuevos o modificados, que se pueden descargar mediante el siguiente endpoint: [Fetch Persons Pregen Export Asset](../endpoints/pull/fetch-persons-pregen-export-asset.md)

Ademas, es posible crear exportaciones a medida, que serán procesadas asincronamente, y se hara posible su descarga tras finalizar el procesamiento.

El proceso a seguir es el siguiente:

1. Creación de la solicitud de exportación, mediante el endpoint: [Create Pull From Admin Bespoke Job](../endpoints/pull/create-pull-from-admin-bespoke-job.md)

2. Comprobación del estado de la solicitud de exportación, mediante el endpoint: [Get Pull From Admin Bespoke Job](../endpoints/pull/get-pull-from-admin-bespoke-job.md)

3. Una vez la solicitud se ha completado, se descarga el fichero con los datos exportados, mediante el endpoint: [Fetch Persons Bespoke Export Asset](../endpoints/pull/fetch-persons-bespoke-export-asset.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
