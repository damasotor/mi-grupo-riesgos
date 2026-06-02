8. RIESGOS RESIDUALES

8.1 Introduccion

Los riesgos residuales son aquellos que pueden seguir existiendo incluso despues de aplicar controles de seguridad. En otras palabras, aunque el sistema incorpore medidas de proteccion, no siempre es posible eliminar por completo todos los riesgos.

En una plataforma de votacion digital, esto es especialmente importante porque existen factores externos, tecnicos y humanos que no pueden controlarse totalmente.

8.2 Riesgos Residuales Identificados

| ID  | Riesgo residual | Probabilidad | Impacto | Justificacion |
| --- | --------------- | ------------ | ------- | ------------- |
| R01 | Coaccion al votante fuera del sistema | Media | Alta | El sistema no puede controlar completamente el entorno fisico o social en el que vota la persona. |
| R02 | Problemas de seguridad en el dispositivo del votante | Media | Alta | Aunque la plataforma tenga controles propios, el dispositivo del usuario podria estar comprometido o ser inseguro. |
| R03 | Falla o vulnerabilidad no conocida en componentes criticos | Baja | Alta | Puede existir una vulnerabilidad no detectada en software, servicios o componentes sensibles. |
| R04 | Errores de configuracion o uso por parte de administradores | Media | Media | Un error humano puede afectar configuraciones, accesos o procesos importantes. |
| R05 | Interrupciones externas de conectividad o infraestructura | Media | Media | Factores como caidas de red, DNS o servicios externos pueden afectar la disponibilidad. |

8.3 Explicacion de los Riesgos Residuales

R01 - Coaccion al votante fuera del sistema

Aunque la plataforma aplique controles tecnicos, no puede garantizar completamente que el votante este libre de presiones, amenazas o influencias externas al momento de emitir su voto.

R02 - Problemas de seguridad en el dispositivo del votante

La aplicacion puede estar protegida, pero el celular o la computadora del usuario podria tener malware, configuraciones inseguras o software no confiable. Esto sigue siendo un riesgo importante en sistemas remotos.

R03 - Falla o vulnerabilidad no conocida en componentes criticos

Siempre existe la posibilidad de que aparezca una vulnerabilidad nueva en un componente importante del sistema, incluso si se aplican buenas practicas de seguridad.

R04 - Errores de configuracion o uso por parte de administradores

No todos los riesgos provienen de atacantes externos. Tambien puede haber errores internos al configurar permisos, publicar resultados o administrar servicios.

R05 - Interrupciones externas de conectividad o infraestructura

La disponibilidad del sistema tambien depende de elementos externos, como conectividad, servicios de red o componentes de infraestructura que pueden fallar.

8.4 Tratamiento General

Estos riesgos no se eliminan por completo, pero pueden reducirse mediante:

- capacitacion y procedimientos claros para administradores;
- monitoreo continuo y revision de eventos;
- copias de seguridad y planes de recuperacion;
- mejoras progresivas de seguridad en aplicaciones y servicios;
- comunicacion clara sobre las limitaciones del sistema.

8.5 Resumen

El analisis de riesgos residuales muestra que, aun con controles de seguridad, siguen existiendo amenazas que no pueden eliminarse totalmente. En este proyecto, los riesgos mas relevantes se relacionan con el entorno del votante, fallas no previstas, errores humanos y problemas externos de disponibilidad. Por eso, ademas de controles tecnicos, tambien son importantes la auditoria, la supervision y la mejora continua.
