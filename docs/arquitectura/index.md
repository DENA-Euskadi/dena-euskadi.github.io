# Arquitectura

## Visión General

DENA Interop es el sistema que permite la interoperabilidad con las distintas administraciones para facilitar a las personas usuarias, a traves de la aplicación cliente, del acceso a los datos que las administraciones ellas. La arquitectura se basa en distintos modulos que permiten la sincronización de metadatos, y obtención de datos por parte de la aplicación cliente. Los distintos modulos de detallan a continuación:

![Diagrama de arquitectura](../adjuntos/imagenes/DENA_Architecture.png)

## Data Sync

El modulo Data Sync se encarga de recibir las actualizaciones de los distintos tipos de datos por parte de las administraciones. DENA solo almacenará los metadatos de actualización, consistentes en la fecha de ultima actualización para cada combinación de persona, tipo de dato y administración. Gracias a estos metadatos, la aplicación cliente podra saber cuando ha de realizar una nueva consulta a cada administración (a traves de DENA) para sincronizar los ultimos cambios que se hayan producido en la datos de la persona usuaria.

Para mantener estos metadatos actualizados, se provee a las administraciones a las administraciones de los mecanismos detallados en [Semantica Data-Sync](../semantica/data-sync/index.md)

## Data Retrieve

Cuando la aplicación cliente detecte nuevos cambios en algún tipo de dato perteneciente a una administración, a traves de la descarga de los metadatos (Data Sync), esta solicitará a traves de DENA a la administración la descarga de los datos actualizados. Para ello, la administración deberá proveer a DENA una conexión a traves de la cual descargar estos datos.

En [Semantica Data-Retrieve](../semantica/data-retrieve/index.md) se detalla la especificación estandar a implementar por la administración para la obtención de los datos por parte de DENA.

En caso de no ser posible la implementación de este estandar, desde DENA se desarrollará un conector no estandar, que permita la obtención de los datos a traves de los medios facilitados por la administración.

## Person Sync

DENA solo deberá sincronizar los metadatos de las personas registradas que hayan dado su consentimiento para el tratamiento de estos datos. Por este motivo se requiere a las administraciones que creen y mantengan un listado de las personas registradas en DENA. Para ello se proveen dos mecanismos:

- **PULL**: La administración solicitará a DENA los datos de personas que requiera mantener. Los medios de obtención de estos datos se detallan en [Semantica PULL](../semantica/person-sync/modelo/pull.md)
- **PUSH**: DENA notificará proactivamente a la administración cuando se registre o se produzcan cambios en una persona. Las conexiones a implementar por la administración para la recepción de estas actualizaciones se detallan en [Semantica PUSH](../semantica/person-sync/modelo/push.md)
