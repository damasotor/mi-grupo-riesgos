══════════════════════════════════════════════════════════

     9. RIESGOS RESIDUALES

══════════════════════════════════════════════════════════

| ID | Riesgo Residual | Prob. | Imp. | Justificación / Aceptación |
| --- | --- | --- | --- | --- |
| R01 | Coerción física del votante | M | A | Se acepta técnicamente. La plataforma no puede controlar si el usuario está siendo amenazado físicamente al momento de usar su dispositivo. Requiere mitigación externa (legal/policial). |
| R02 | Vulnerabilidad Zero-Day en HSM | B | C | Se acepta. La probabilidad de que exista un exploit público desconocido (Zero-Day) para hardware criptográfico certificado es extremadamente baja. |
| R03 | Malware en dispositivo del usuario | A | M | Se acepta parcialmente. El malware podría capturar la pantalla, pero la disociación de red impide alterar el escrutinio general. |

══════════════════════════════════════════════════════════

     10. CONCLUSIONES Y RECOMENDACIONES

══════════════════════════════════════════════════════════

    # 10.1 Resumen Ejecutivo

El análisis de modelado de amenazas sobre el Sistema de Votación Electrónica revela una arquitectura robusta por diseño, sustentada en la descentralización de la confianza (Blockchain) y criptografía avanzada (HSM). Las vulnerabilidades más críticas se concentran en la capa de disponibilidad perimetral (ataques DoS) y en los puntos de cruce de los *Trust Boundaries*, donde la segregación de red es vital para garantizar la integridad y el anonimato.

    # 10.2 Recomendaciones Prioritarias

1. Despliegue perimetral robusto: Implementar de inmediato una solución WAF y Anti-DDoS comercial para absorber picos de tráfico malicioso.
2. Aislamiento del Escrutinio: Asegurar mediante reglas de firewall a nivel de hardware que el Servicio de Escrutinio permanezca totalmente incomunicado hasta el cierre formal de la elección.
3. Gestión de Identidades: Implementar un mecanismo de autenticación de doble factor para los votantes (ej. validación por SMS o App gubernamental) para elevar la complejidad de los ataques de suplantación.

    # 10.3 Próximos Pasos

* [ ] Implementar controles de alta prioridad (C01 y C02).
* [ ] Desarrollar y probar el Plan de Respuesta a Incidentes (Ejercicios de simulación DoS).
* [ ] Ejecutar prueba de penetración (Pentest) sobre el *Trust Boundary #1* antes del pase a producción.

