# Análisis de Riesgos — Plataforma de Votación Digital

**Tarea 2 · Ingeniería en Sistemas de Información · UTU · 2026**

Este repositorio contiene la documentación de un trabajo universitario de ciberseguridad enfocado en el análisis de riesgos de una plataforma de votación digital basada en cifrado homomórfico y arquitectura de microservicios sobre Blockchain.

El objetivo del proyecto es identificar los activos más importantes del sistema, analizar amenazas, priorizar riesgos y proponer controles de mitigación de forma clara y ordenada.

## Objetivo del trabajo

El análisis busca estudiar los principales riesgos de seguridad de una plataforma de votación digital que incluye:

- Registro de votantes.
- Autenticación de identidad.
- Emisión del voto.
- Almacenamiento cifrado.
- Escrutinio.
- Resultados.
- Auditoría.

## Temas trabajados

La documentación del proyecto fue organizada en varias etapas:

1. Inventario de activos.
2. Arquitectura del sistema y trust boundaries.
3. Análisis de amenazas con STRIDE.
4. Priorización de riesgos con DREAD.
5. Relación con MITRE ATT&CK.
6. Plan de mitigación.
7. Riesgos residuales.

## Metodologías aplicadas

- **STRIDE** — Identificación de amenazas por categoría (Spoofing, Tampering, Repudiation, Information Disclosure, DoS, Elevation of Privilege)
- **DREAD** — Puntuación cuantitativa de severidad (Daño, Reproducibilidad, Explotabilidad, Usuarios afectados, Descubribilidad)
- **MITRE ATT&CK** — Mapeo de técnicas a tácticas del framework (Navigator layer incluido)
- **RFC 6973** — Evaluación de anonimato y privacidad del votante
- **AGESIC NTCSI** — Marco normativo de referencia (regulación uruguaya)

## Estructura del repositorio

### Carpeta `docs`

Contiene la documentación principal del análisis:

- `01-inventario-activos.md`
- `02-arquitectura-trust-boundaries.md`
- `03-analisis-stride.md`
- `04-priorizacion-dread.md`
- `05-mapa-attack.md`
- `06-plan-mitigacion.md`
- `07-riesgos-residuales.md`

### Carpeta `diagrams`

Incluye diagramas e imágenes de apoyo, como arquitectura y visualizaciones relacionadas con el análisis.

### Carpeta `prompts`

Contiene material de apoyo para mejorar la redacción, revisar secciones del trabajo y organizar ideas para la entrega.

## Riesgos residuales aceptados

| Riesgo                             | Justificación                                                   |
| ---------------------------------- | --------------------------------------------------------------- |
| Coerción física del votante        | Fuera del perímetro técnico; requiere mitigación legal/policial |
| Zero-Day en HSM certificado        | Probabilidad extremadamente baja en hardware FIPS 140-2         |
| Malware en dispositivo del usuario | La disociación de red impide afectar el escrutinio general      |

## Enfoque del trabajo

Este proyecto fue desarrollado con un enfoque académico, buscando mantener un equilibrio entre claridad, contenido técnico y facilidad de comprensión. No se planteó como una auditoría profesional completa, sino como un análisis universitario bien estructurado y defendible.

## Resultado esperado

El resultado final es una documentación que permite:

- Entender el sistema analizado.
- Identificar amenazas relevantes.
- Priorizar riesgos.
- Proponer controles de seguridad.
- Reconocer que siguen existiendo riesgos residuales.

## Nota final

La idea principal del trabajo es mostrar que la seguridad en una plataforma de votación digital no depende solo de la tecnología, sino también de la privacidad, la integridad del voto, la auditoría y la confianza en el proceso.
