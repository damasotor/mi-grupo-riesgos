══════════════════════════════════════════════════════════

     9. RIESGOS RESIDUALES

══════════════════════════════════════════════════════════

| ID | Riesgo Residual | Prob. | Imp. | Justificación / Aceptación |
| --- | ------------------------ | -------- | ------ | --------------------------------------- |
| R01 | Coerción física del votante | M | A | Se acepta técnicamente. La plataforma no puede controlar si el usuario está siendo amenazado físicamente al momento de usar su dispositivo. Requiere mitigación externa (legal/policial). |
| R02 | Vulnerabilidad Zero-Day en HSM | B | C | Se acepta. La probabilidad de que exista un exploit público desconocido (Zero-Day) para hardware criptográfico certificado es extremadamente baja. |
| R03 | Malware en dispositivo del usuario | A | M | Se acepta parcialmente. El malware podría capturar la pantalla, pero la disociación de red impide alterar el escrutinio general. |

══════════════════════════════════════════════════════════

     10. ENTREGABLE EXTRA: EVALUACIÓN DE ANONIMALIDAD (RFC 6973)

══════════════════════════════════════════════════════════

En cumplimiento estricto de las directrices de privacidad estipuladas por el RFC 6973 (Request for Comments 6973 - el estándar internacional técnico que rige las consideraciones de privacidad en sistemas distribuidos), se realiza el análisis sobre las salvaguardas que impiden la correlación de la identidad del votante con su sufragio:

1. Principio de No-Vinculabilidad (Unlinkability): El sistema implementa una separación lógica y física absoluta entre el Servicio de Autenticación (encargado de validar la CI Digital o el MFA - Autenticación Multifactor - y marcar al ciudadano en el padrón como "Ya votó") y el Servicio de Emisión de Votos (que procesa el criptograma). El uso de blind signatures (firmas ciegas) asegura que el servidor central firme la validez del derecho al voto del ciudadano sin llegar a conocer jamás el contenido del sufragio elegido, rompiendo cualquier lazo de unión de datos entre el votante y su opción política.

2. Mitigación de la Coerción (Voto Coercion):
   Como se identificó en el riesgo residual R01, la aplicación no puede evitar que un coactor amenace físicamente a un ciudadano en su domicilio. Para solucionar este vector de ataque desde el diseño de la arquitectura, se define el requisito de "Voto Múltiple Reemplazable": un votante coercionado puede emitir el voto que le imponen bajo amenaza externa. Posteriormente, al estar seguro y a solas, el sistema le permite ingresar nuevamente y emitir un nuevo sufragio legítimo. La base de datos y el motor de escrutinio procesarán únicamente el último voto registrado, descartando de forma automática los anteriores sin generar ninguna alerta visual que ponga en peligro al usuario.

3. Minimización de Datos en Registros de Auditoría:
   Los logs de auditoría centralizados y los sistemas SIEM (Security Information and Event Management - Gestión de Eventos e Información de Seguridad) mapeados bajo la normativa NIST AU-9 tienen prohibido recolectar direcciones IP, identificadores de navegador (USER-AGENTS) o marcas de tiempo precisas en los microservicios de escrutinio. Al almacenar los registros transaccionales mediante identificadores pseudo-aleatorios y rotativos en la Blockchain, se anulan los ataques de análisis de tráfico que buscan la re-identificación cruzada de datos.

══════════════════════════════════════════════════════════

     11. CONCLUSIONES Y RECOMENDACIONES

══════════════════════════════════════════════════════════

    # 11.1 Resumen Ejecutivo

El análisis de modelado de amenazas sobre el Sistema de Votación Electrónica revela una arquitectura robusta por diseño, sustentada en la descentralización de la confianza (Blockchain) y criptografía avanzada (HSM). Las vulnerabilidades más críticas se concentran en la capa de disponibilidad perimetral (ataques DoS) y en los puntos de cruce de los Trust Boundaries, donde la segregación de red es vital para garantizar la integridad y el anonimato.

    # 11.2 Recomendaciones Prioritarias

1. Despliegue perimetral robusto: Implementar de inmediato una solución WAF y Anti-DDoS comercial para absorber picos de tráfico malicioso.
2. Aislamiento del Escrutinio: Asegurar mediante reglas de firewall a nivel de hardware que el Servicio de Escrutinio permanezca totalmente incomunicado hasta el cierre formal de la elección.
3. Gestión de Identidades: Implementar un mecanismo de autenticación de doble factor para los votantes (ej. validación por SMS o App gubernamental) para elevar la complejidad de los ataques de suplantación.

    # 11.3 Próximos Pasos

 [ ] Implementar controles de alta prioridad (C01 y C02).
 [ ] Desarrollar y probar el Plan de Respuesta a Incidentes (Ejercicios de simulación DoS).
 [ ] Ejecutar prueba de penetración (Pentest) sobre el Trust Boundary #1 antes del pase a producción.

