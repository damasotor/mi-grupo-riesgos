4. ANALISIS DE AMENAZAS - METODOLOGIA STRIDE

4.1 Que es STRIDE

STRIDE es una metodologia que sirve para identificar amenazas de seguridad en un sistema. Cada letra representa un tipo de amenaza:

- S - Spoofing: suplantacion de identidad.
- T - Tampering: modificacion no autorizada de datos o procesos.
- R - Repudiation: negacion de una accion realizada.
- I - Information Disclosure: exposicion de informacion sensible.
- D - Denial of Service: interrupcion o degradacion del servicio.
- E - Elevation of Privilege: obtencion de permisos mayores a los permitidos.

En este proyecto, STRIDE se utiliza para detectar amenazas sobre los componentes mas importantes del sistema de votacion y sobre los limites de confianza definidos en la arquitectura.

4.2 Matriz de Amenazas por Componente

| ID   | Componente | Categoria STRIDE | Amenaza | Trust Boundary |
| ---- | ---------- | ---------------- | ------- | -------------- |
| TH01 | API Gateway / Aplicacion Web-Movil | D - Denial of Service | Un atacante intenta saturar el acceso al sistema para impedir que los votantes puedan ingresar o votar. | TB1 |
| TH02 | Servicio de Autenticacion | S - Spoofing | Un atacante usa credenciales robadas o una sesion ajena para hacerse pasar por un votante legitimo. | TB1, TB2 |
| TH03 | Servicio de Emision de Voto | T - Tampering | Un atacante intenta modificar el voto antes de que quede cifrado y registrado por el sistema. | TB3, TB4 |
| TH04 | Base de datos / Logs / Servicios internos | I - Information Disclosure | Se produce una relacion indebida entre la identidad del votante y el contenido de su voto. | TB3, TB7 |
| TH05 | Registros de auditoria / Acciones administrativas | R - Repudiation | Un usuario o administrador niega haber realizado una accion porque no existen evidencias suficientes en los registros. | TB7 |
| TH06 | Servicio de Escrutinio / Dashboard de resultados | T - Tampering | Un atacante o usuario no autorizado intenta alterar resultados o acceder antes de tiempo al conteo. | TB6, TB8 |
| TH07 | Consola de administracion / Servicios internos | E - Elevation of Privilege | Un usuario con permisos limitados logra acceder a funciones administrativas o criticas del sistema. | TB5, TB6 |
| TH08 | Logs, monitoreo y auditoria | T - Tampering | Un atacante modifica, elimina o inutiliza los logs para ocultar acciones maliciosas. | TB7 |

4.3 Detalle de Amenazas Principales

TH01 - Denegacion de Servicio en el Acceso al Sistema

- Categoria STRIDE: D - Denial of Service
- Descripcion: un atacante intenta saturar la aplicacion o el API Gateway para impedir el acceso de los votantes durante la eleccion.
- Activos afectados: T01, T08, T09, Confianza de la ciudadania.
- Impacto general: alto, porque afecta la disponibilidad del sistema.
- Trust Boundary relacionado: TB1.

TH02 - Suplantacion de Identidad del Votante

- Categoria STRIDE: S - Spoofing
- Descripcion: un atacante intenta ingresar con credenciales robadas, tokens de sesion o informacion de otra persona para votar en su nombre.
- Activos afectados: A02, A07, T07.
- Impacto general: alto, porque afecta la legitimidad de la identidad del votante.
- Trust Boundary relacionado: TB1 y TB2.

TH03 - Manipulacion del Voto antes del Registro

- Categoria STRIDE: T - Tampering
- Descripcion: un atacante intenta modificar el voto antes de que quede cifrado y almacenado por el sistema.
- Activos afectados: A01, A05, T06.
- Impacto general: alto, porque afecta la integridad del voto.
- Trust Boundary relacionado: TB3 y TB4.

TH04 - Relacion entre Identidad y Voto

- Categoria STRIDE: I - Information Disclosure
- Descripcion: una mala gestion de bases de datos, sesiones o logs podria permitir relacionar al votante con su voto.
- Activos afectados: A01, A04, A06.
- Impacto general: alto, porque rompe el secreto del sufragio.
- Trust Boundary relacionado: TB3 y TB7.

TH05 - Repudio de Acciones o Eventos

- Categoria STRIDE: R - Repudiation
- Descripcion: un votante, operador o administrador podria negar una accion realizada si el sistema no guarda evidencia suficiente de lo ocurrido.
- Activos afectados: A06, A11, T12.
- Impacto general: medio, porque dificulta auditorias e investigaciones.
- Trust Boundary relacionado: TB7.

TH06 - Alteracion del Escrutinio o de los Resultados

- Categoria STRIDE: T - Tampering
- Descripcion: un atacante intenta modificar el conteo final, acceder al resultado antes de tiempo o alterar la informacion mostrada en el dashboard.
- Activos afectados: A09, T02, T13.
- Impacto general: alto, porque compromete la confianza en el resultado electoral.
- Trust Boundary relacionado: TB6 y TB8.

TH07 - Elevacion de Privilegios

- Categoria STRIDE: E - Elevation of Privilege
- Descripcion: un usuario con permisos limitados logra acceder a funciones reservadas para administradores, operadores o servicios criticos.
- Activos afectados: T15, T02, T04.
- Impacto general: alto, porque puede abrir acceso a funciones sensibles del sistema.
- Trust Boundary relacionado: TB5 y TB6.

TH08 - Alteracion de Logs y Evidencia

- Categoria STRIDE: T - Tampering
- Descripcion: un atacante modifica o elimina registros de auditoria para ocultar acciones maliciosas o dificultar una investigacion.
- Activos afectados: A06, A11, T12.
- Impacto general: medio-alto, porque reduce la capacidad de control y auditoria.
- Trust Boundary relacionado: TB7.

4.4 Resumen General del Analisis

El uso de STRIDE permite observar que las amenazas mas importantes del sistema aparecen en los puntos donde cambia el nivel de confianza entre usuarios, aplicaciones y servicios internos. En este proyecto, los limites mas sensibles son el acceso desde Internet, la separacion entre autenticacion y emision del voto, el escrutinio y la auditoria.

Las amenazas que mas afectan al sistema son la suplantacion de identidad, la manipulacion del voto, la exposicion de informacion sensible y la alteracion de resultados. Por ese motivo, estas areas deberan recibir mayor atencion en la priorizacion de riesgos y en el plan de mitigacion.
