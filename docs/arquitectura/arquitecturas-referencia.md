# :material-sitemap: Arquitecturas de referencia de integración

> Contenido del apartado *"Integration reference architectures"* del documento [DENA-CORE — Services for Admins](../adjuntos/documentos/DENA-CORE-Services_for_admins.pdf) (:material-download: PDF).

Muchas administraciones tienen decenas de tipos de dato y orígenes de datos que integrar en DENA, y un enfoque uno por uno no es la mejor solución, porque lleva a tener múltiples componentes distintos haciendo lo mismo (por ejemplo, múltiples componentes enviando metadatos de sincronización SRMD a DENA).

Por eso, un enfoque más centralizado puede ayudar a la integración de los múltiples orígenes de datos en DENA.

La siguiente imagen representa un enfoque en el que se muestran las tres áreas de integración de cualquier administración:

1. Person sync
2. Envío de metadatos de sincronización SRMD
3. Servicios de recuperación de datos (*data provider*)

![Arquitectura de referencia de integración DENA](../adjuntos/imagenes/documentos/arquitectura-referencia-integracion.png)

---

## 5.1 Person Sync

El *person sync* se realiza normalmente una vez por administración, de modo que cada administración integrada en DENA tiene una única réplica de la base de datos de personas.

Esta integración es directa usando los artefactos proporcionados por DENA, que se pueden desplegar en el datacenter de cualquier administración.

---

## 5.2 Envío de metadatos de sincronización SRMD

Lo primero que un origen de datos tiene que hacer para integrarse en DENA es enviar los cambios. Para ello, la administración puede desplegar un colector de cambios centralizado usando [Apache NiFi] que simplemente monitoriza una [DB view] de cada origen de datos en busca de cambios y, cuando los detecta, envía un [mensaje] a un [Apache Kafka] con los cambios, que a su vez los envía a DENA.

---

## 5.3 Data Retrieve

Para la recuperación de datos, hay que acceder directamente a los datos del origen de datos (o a una copia de ellos).

La forma más sencilla es pedir al área de negocio que cree una [DB view] de los datos que se van a mostrar en DENA (que normalmente son solo unas pocas columnas del total de los datos de negocio) y desplegarla. Esta [DB view] es el origen de un servicio [data provider].

Muchas veces, una manera cómoda de desplegar cada servicio [data provider] es agrupar todos los [data providers] de un origen de datos en un único servicio común y distribuir el acceso a los datos usando rutas de URL:

```
https://dena-internal.my_admin.eus/dena-data-provider/data-type1/byperson/{personId}
https://dena-internal.my_admin.eus/dena-data-provider/data-type2/byperson/{personId}
https://dena-internal.my_admin.eus/dena-data-provider/data-type3/byperson/{personId}
…
```

---

## Documento completo

[:material-download: DENA-CORE — Services for Admins (PDF)](../adjuntos/documentos/DENA-CORE-Services_for_admins.pdf) · [Todos los documentos (PDF)](../adjuntos/documentos.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
