7. PLAN DE MITIGACION

7.1 Introduccion

El plan de mitigacion presenta los controles de seguridad propuestos para reducir las amenazas identificadas en el analisis. Su objetivo es mostrar que medidas pueden aplicarse para proteger la disponibilidad, integridad, confidencialidad y trazabilidad del sistema de votacion.

7.2 Tabla General de Controles

| ID  | Amenaza relacionada | Control propuesto | Prioridad | Estado |
| --- | ------------------- | ----------------- | --------- | ------ |
| C01 | TH01 - Denegacion de servicio en el acceso al sistema | Implementar WAF, rate limiting y proteccion basica contra ataques de denegacion de servicio. | Alta | Pendiente |
| C02 | TH02 - Suplantacion de identidad del votante | Aplicar autenticacion robusta, control de sesiones y vencimiento de tokens. | Alta | Pendiente |
| C03 | TH03 - Manipulacion del voto antes del registro | Aplicar cifrado del voto y controles de integridad antes del almacenamiento. | Alta | Pendiente |
| C04 | TH04 - Relacion indebida entre identidad y voto | Mantener separacion entre autenticacion y emision del voto, evitando registros vinculables. | Alta | Pendiente |
| C05 | TH05 - Repudio de acciones o eventos | Registrar eventos importantes del sistema y conservar evidencia para auditoria. | Media | Pendiente |
| C06 | TH06 - Alteracion del escrutinio o de los resultados | Restringir el acceso al escrutinio y controlar la publicacion de resultados. | Alta | Pendiente |
| C07 | TH07 - Elevacion de privilegios | Definir roles, aplicar minimo privilegio y limitar funciones administrativas. | Alta | Pendiente |
| C08 | TH08 - Alteracion de logs y evidencia | Proteger logs, centralizar monitoreo y evitar eliminacion o modificacion no autorizada. | Media | Pendiente |

7.3 Controles Priorizados

C01 - Proteccion del acceso al sistema

Este control busca reducir el riesgo de que el sistema quede fuera de servicio durante la eleccion. Para ello se propone el uso de WAF, limitacion de peticiones y proteccion perimetral basica.

C02 - Autenticacion y control de sesiones

Este control busca evitar la suplantacion de identidad del votante. Se propone usar mecanismos de autenticacion mas robustos, sesiones temporales y control del uso de tokens.

C03 - Integridad del voto

Este control busca asegurar que el voto no sea modificado antes de quedar registrado. Para ello se propone mantener el cifrado del voto y agregar validaciones de integridad en el proceso.

C04 - Separacion entre identidad y voto

Este control busca proteger el secreto del sufragio. La idea es mantener separados los procesos de autenticacion y emision del voto, y evitar que logs o bases de datos permitan relacionar al votante con su eleccion.

C05 - Trazabilidad y evidencia

Este control busca que el sistema conserve evidencia de acciones importantes. Esto ayuda a auditorias y a investigar incidentes o reclamos.

C06 - Proteccion del escrutinio y resultados

Este control busca evitar accesos no autorizados al conteo o a la publicacion de resultados. Se propone restringir permisos y controlar el momento y forma en que la informacion es mostrada.

C07 - Control de privilegios

Este control busca impedir que usuarios comunes accedan a funciones administrativas o criticas. Se propone trabajar con roles claros y con el principio de minimo privilegio.

C08 - Proteccion de logs y monitoreo

Este control busca proteger la evidencia del sistema. Se propone centralizar logs y monitoreo para dificultar su alteracion o eliminacion.

7.4 Controles por Tipo

Controles preventivos:

- C01 - WAF, rate limiting y proteccion del acceso.
- C02 - Autenticacion robusta y control de sesiones.
- C03 - Cifrado e integridad del voto.
- C04 - Separacion entre identidad y voto.
- C06 - Restriccion de acceso al escrutinio y resultados.
- C07 - Definicion de roles y minimo privilegio.

Controles detectivos:

- C05 - Registros de auditoria y trazabilidad.
- C08 - Monitoreo centralizado y seguimiento de eventos.

Controles correctivos:

- Revision de incidentes detectados en auditoria.
- Recuperacion de informacion mediante backups.
- Aplicacion de cambios de configuracion o bloqueo de accesos si se detecta un problema.

7.5 Referencias Generales

Los controles propuestos se apoyan en ideas generales tomadas de buenas practicas de seguridad como:

- NIST: autenticacion, auditoria, proteccion de informacion y control de accesos.
- OWASP: buenas practicas para seguridad en aplicaciones web.
- CIS Controls: monitoreo, gestion de accesos y configuracion segura.

7.6 Resumen

El plan de mitigacion permite relacionar cada amenaza principal con una respuesta concreta. En este proyecto, la mayor prioridad se encuentra en proteger el acceso al sistema, asegurar la identidad del votante, mantener la integridad del voto, cuidar la privacidad y controlar adecuadamente el escrutinio y la auditoria.
