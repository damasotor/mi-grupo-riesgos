5. ANALISIS DE RIESGOS - METODOLOGIA DREAD

5.1 Que es DREAD

DREAD es una metodologia que permite priorizar amenazas de seguridad segun su impacto y la facilidad con la que pueden ser explotadas. Su objetivo es ayudar a identificar cuales riesgos deben atenderse primero.

5.2 Criterios DREAD

| Criterio | Descripcion |
| --- | --- |
| Damage | Que tan grave seria el dano si la amenaza se concretara. |
| Reproducibility | Que tan facil seria repetir el ataque. |
| Exploitability | Que tan facil seria ejecutar el ataque. |
| Affected Users | A cuantos usuarios o componentes podria afectar. |
| Discoverability | Que tan facil seria encontrar la debilidad. |

Cada criterio se valora de 1 a 10. Un valor mas alto indica un mayor nivel de riesgo.

5.3 Matriz de Priorizacion

| ID   | Amenaza | D | R | E | A | D | Total | Nivel |
| ---- | ------- | - | - | - | - | - | ----- | ----- |
| TH01 | Denegacion de servicio en el acceso al sistema | 8 | 8 | 8 | 9 | 9 | 42 | Critico |
| TH02 | Suplantacion de identidad del votante | 8 | 7 | 6 | 7 | 7 | 35 | Alto |
| TH03 | Manipulacion del voto antes del registro | 10 | 5 | 5 | 9 | 5 | 34 | Alto |
| TH04 | Relacion indebida entre identidad y voto | 10 | 4 | 4 | 9 | 4 | 31 | Alto |
| TH05 | Repudio de acciones o eventos | 6 | 6 | 5 | 5 | 6 | 28 | Medio |
| TH06 | Alteracion del escrutinio o de los resultados | 10 | 5 | 5 | 10 | 5 | 35 | Alto |
| TH07 | Elevacion de privilegios | 9 | 5 | 5 | 8 | 5 | 32 | Alto |
| TH08 | Alteracion de logs y evidencia | 7 | 6 | 5 | 6 | 5 | 29 | Medio |

5.4 Justificacion General de la Priorizacion

- TH01 se considera critico porque puede impedir que los votantes accedan al sistema y ejerzan su voto.
- TH02 se considera alto porque una suplantacion de identidad afecta una funcion central del sistema: que vote la persona correcta.
- TH03 y TH06 se consideran altos porque afectan directamente la integridad del voto y del resultado electoral.
- TH04 se considera alto porque compromete el secreto del sufragio, que es uno de los objetivos principales del sistema.
- TH07 se considera alto porque un acceso con privilegios mayores podria impactar servicios criticos.
- TH05 y TH08 se consideran medios porque afectan la trazabilidad, la auditoria y la capacidad de investigacion, aunque no necesariamente cambian por si solos el resultado final.

5.5 Escala de Severidad

- Critico (40 a 50): debe atenderse de forma inmediata.
- Alto (30 a 39): debe corregirse con prioridad alta.
- Medio (20 a 29): debe planificarse su correccion.
- Bajo (10 a 19): requiere seguimiento y monitoreo.
- Minimo (1 a 9): riesgo aceptable o de bajo impacto.

5.6 Resumen del Analisis

La priorizacion con DREAD muestra que los riesgos mas importantes del sistema estan relacionados con la disponibilidad del acceso, la integridad del voto, la proteccion de los resultados y la privacidad del sufragio. Esto permite enfocar las mitigaciones en los puntos mas sensibles del proyecto.
