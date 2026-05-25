══════════════════════════════════════════════════════════

     7. PLAN DE MITIGACIÓN Y REQUISITOS EXTRA

══════════════════════════════════════════════════════════

 7.1 Controles de Seguridad

| ID    | Estado | Amenaza                        | Control de Seguridad                                                                                            | Prioridad |
| ------ | ----------- | -------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------- |
| C01 | Pend. | TH01 (DDoS / T1498) | Desplegar módulo Anti-DDoS, WAF y Rate Limiting en el API Gateway. | Alta |
| C02 | Pend. | TH02 (Tampering / T1565) | Aislar el Servicio de Escrutinio en VLAN dedicada y aplicar validaciones criptográficas de integridad (Hashes/Firmas). | Alta |
| C03 | Pend. | TH04 (Info. Disc.) | Implementar protocolo de disociación de logs (Tokenización de PII). | Alta |
| C04 | Pend. | TH03 (Spoofing / T1078) | Forzar autenticación robusta (MFA / CI Digital) y vinculación de tokens de sesión para evitar su robo (T1528/T1557). | Media |

(Nota de trazabilidad: Se ajustaron las prioridades de los controles para reflejar fidedignamente los resultados cuantitativos obtenidos en la matriz DREAD).

---

 7.1.1 Fichas de Implementación Detalladas (Riesgos de Prioridad Alta)

 📋 Control C01: Módulo Anti-DDoS y Rate Limiting Perimetral
 Amenaza Mitigada: TH01 - Denegación de Servicio Distribuido (DDoS - Distributed Denial of Service) / Técnica MITRE T1498.
 Descripción: Desplegar una capa de mitigación volumétrica basada en la nube y un WAF (Web Application Firewall - Cortafuegos de Aplicaciones Web, diseñado para inspeccionar y filtrar tráfico HTTP malicioso) por delante del API Gateway (Application Programming Interface Gateway - el punto de entrada único para todas las peticiones de los clientes). Se configurarán reglas estrictas de Rate Limiting (límite de velocidad, restringiendo la cantidad de peticiones concurrentes permitidas por cada dirección IP) para identificar y bloquear ráfagas malformadas de botnets.
 Nivel de Esfuerzo: Bajo / Medio. La integración perimetral requiere principalmente cambios de enrutamiento DNS y tunelización segura, sin alterar el código core de la aplicación.
 Impacto Esperado: Muy Alto. Reduce drásticamente la probabilidad de éxito de ataques DoS, garantizando la disponibilidad de los servidores durante la jornada electoral.

 📋 Control C02: Segregación Estricta de Red y Validación Criptográfica en Tránsito
 Amenaza Mitigada: TH02 - Manipulación del Voto en Tránsito (Tampering) / Técnica MITRE T1565.
 Descripción: Implementar aislamiento físico/lógico del Servicio de Escrutinio mediante una VLAN dedicada (Virtual Local Area Network - Red de Área Local Virtual que aísla el tráfico a nivel de red, impidiendo que otros servidores se comuniquen directamente con el escrutinio). Todo voto transmitido desde el cliente debe incluir una firma digital o validación criptográfica basada en Hashes (funciones matemáticas unidireccionales que garantizan que el archivo no fue modificado) calculada en origen. El backend validará la integridad del hash antes de consolidar el registro.
 Nivel de Esfuerzo: Alto. Requiere reconfiguración de la infraestructura de red corporativa, definición de políticas de firewall estrictas y desarrollo para la validación de firmas en el backend.
 Impacto Esperado: Crítico. Neutraliza la ventana de exposición, impidiendo que un atacante interno o una cuenta comprometida modifique los datos en tránsito.

 📋 Control C03: Protocolo de Disociación de Logs y Tokenización de PII
 Amenaza Mitigada: TH04 - Correlación de Identidad y Voto (Information Disclosure) / CWE-200.
 Descripción: Implementar un mecanismo de tokenización unidireccional (salado criptográfico) para ocultar las PII (Personally Identifiable Information - Información de Identificación Personal, como nombres, documentos o correos electrónicos). Se prohíbe taxativamente el registro simultáneo de datos del padrón electoral (PII) junto con los identificadores de voto en las bases de datos transaccionales o en los logs de auditoría centralizados.
 Nivel de Esfuerzo: Medio. Exige el rediseño del modelo de datos de persistencia y la configuración de filtros de depuración (data masking) en el sistema de registro de eventos (logging).
 Impacto Esperado: Alto. Elimina el riesgo de quiebre del secreto del sufragio por correlación cruzada de datos en la base de datos.

 📋 Control C04: Control de Autenticación y Vinculación de Sesión (Session Binding)
 Amenaza Mitigada: TH03 - Suplantación de Identidad (Spoofing) / T1078, T1528, T1557.
 Descripción: Imponer MFA (Multi-Factor Authentication - Autenticación Multifactor, exigiendo contraseña más un código SMS/App) o el uso de CI Digital (Cédula de Identidad Digital, estándar en Uruguay). Adicionalmente, implementar Session Binding: los JWT (JSON Web Tokens - estándar seguro para transmitir la sesión del usuario autenticado) deben ligarse criptográficamente a la huella digital del dispositivo o IP de origen. Configurar un tiempo de vida corto (Short-lived tokens) complementado con mecanismos de Refresh Tokens de un solo uso.
 Nivel de Esfuerzo: Medio. Requiere actualización en la lógica del Servicio de Autenticación y validación en el API Gateway.
 Impacto Esperado: Alto. Si un atacante intercepta un token mediante técnicas AitM (Adversary-in-the-Middle - atacante posicionado entre el usuario y el servidor interceptando el tráfico), el sistema rechazará automáticamente el JWT al detectar que es presentado desde una IP o dispositivo diferente al original.

---

 7.2 Entregable Extra: Matriz de Controles de Integridad (Escenario 8)

En cumplimiento de los requerimientos específicos de la plataforma de votación, se detalla cómo el sistema asegura que los resultados reflejen fielmente la voluntad del electorado:

| Componente | Mecanismo de Integridad | Control de Validación | Garantía de No Alteración |
| ------------------- | -------------------------------------- | --------------------------------- | --------------------------------------- |
| Cliente Web / Móvil | Cifrado Homomórfico Local | El dispositivo del votante cifra el voto antes de transmitirlo mediante una clave pública institucional. | El voto viaja de extremo a extremo sin que la red o los servidores intermedios puedan conocer su contenido o alterarlo. |
| API Gateway / Capa de Tránsito | mTLS (Mutual Transport Layer Security) | Autenticación mutua obligatoria: no solo el cliente verifica al servidor (TLS clásico), sino que el servidor exige un certificado válido al microservicio cliente. | Evita ataques de inyección, interceptación (AitM) o falsificación de identidad en los límites de confianza internos. |
| Servicio de Escrutinio | Validación de Firmas | Verificación criptográfica obligatoria contra las claves simétricas almacenadas en el HSM (Hardware Security Module - procesador físico ultra-seguro dedicado a gestionar claves y firmas criptográficas) certificado bajo la norma FIPS 140-2 (estándar federal de seguridad de EE.UU.). | Impide que votos falsificados o inyecciones directas en la base de datos (T1565) se consoliden en el conteo final. |
| Audit Trail | Ledger de Blockchain | Almacenamiento descentralizado de los hashes de los bloques transaccionales. | Convierte el registro de votos en una estructura inmutable. |

---

 7.3 Controles por Categoría

CONTROLES PREVENTIVOS:
 [x] MFA (Autenticación Multifactor) para administradores y auditores.
 [x] Cifrado de datos en tránsito y en reposo (mTLS, Cifrado Homomórfico).
 [x] Validación de entrada en el API Gateway.
 [x] RBAC (Role-Based Access Control - Control de Acceso Basado en Roles, otorgando el principio de mínimo privilegio necesario para cada tarea).
 [x] Segmentación de red mediante VLAN.
 [x] Cifrado TLS 1.3 forzado y HSTS (HTTP Strict Transport Security - política que obliga a los navegadores a conectarse exclusivamente mediante HTTPS seguro).

CONTROLES DETECTIVOS:
 [x] Logging de eventos de seguridad centralizado e inmutable.
 [x] Monitorización con SIEM (Security Information and Event Management - plataforma que centraliza, analiza y correlaciona alertas de seguridad en tiempo real) configurado para detectar uso anómalo de cuentas (T1078).
 [x] Alertas de anomalías en el volumen de emisión de votos.

CONTROLES CORRECTIVOS:
 [x] Plan de respuesta a incidentes.
 [x] Procedimientos de backup inmutables (excluyendo claves del HSM).

══════════════════════════════════════════════════════════

     8. MATRIZ DE CONTROLES (NIST SP 800-53 / NTCSI)

══════════════════════════════════════════════════════════

Nota: Los controles se mapean contra NIST SP 800-53 Rev. 5 (Catálogo de controles de seguridad y privacidad del Instituto Nacional de Estándares y Tecnología de EE.UU.), cumpliendo rigurosamente con las exigencias del Marco Nacional de Ciberseguridad de AGESIC y su NTCSI (Norma Técnica de Ciberseguridad de la Información de Uruguay).

| ID | Control | Descripción | Referencia |
| --------- | -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | -----------------|
| SC-13 | Cryptographic Protection | Uso de HSM FIPS 140-2 para protección de claves maestras y firma de escrutinio.                          | NIST SC-13 |
| SC-5 | Denial of Service Prot.         | Controles de mitigación volumétrica y rate-limiting en el perímetro del sistema.                              | NIST SC-5 |
| AC-4 | Information Flow Enf.          | Control estricto y segmentación de flujos entre los Trust Boundaries definidos.                              | NIST AC-4 |
| AU-9 | Protection of Audit Info     | Uso de infraestructura Blockchain para garantizar el no repudio e inmutabilidad de la auditoría. | NIST AU-9 |
| IA-2 | Identification and Auth.       | Verificación unívoca y robusta del ciudadano contra el padrón electoral centralizado.                    | NIST IA-2 |


══════════════════════════════════════════════════════════

     11. ENTREGABLE EXTRA: EVALUACIÓN DE ANONIMALIDAD (RFC 6973)

══════════════════════════════════════════════════════════

Siguiendo las directrices de privacidad estipuladas por el RFC 6973 (Request for Comments 6973 - documento técnico estándar internacional que define las consideraciones de privacidad para protocolos de Internet), se realiza el análisis sobre las salvaguardas que impiden la correlación de la identidad del votante con su sufragio emitido:

1. Principio de No-Vinculabilidad (Unlinkability): El sistema implementa una separación lógica y física estricta entre el Servicio de Autenticación (que valida la CI Digital o MFA y marca que "ya votó") y el Servicio de Emisión (que recibe el criptograma del voto). El uso de blind signatures (firmas ciegas) asegura que el Servicio de Emisión reciba un voto válido sin posibilidad alguna de conocer qué identidad del padrón lo generó.

2. Mitigación de la Coerción (Voto Coercion):
   La arquitectura contempla el requisito de "Voto Múltiple Reemplazable": un votante coercionado (amenazado físicamente) puede emitir su voto bajo coacción, pero la plataforma le permite ingresar nuevamente más tarde de forma legítima. El sistema procesará el escrutinio basándose únicamente en el último voto registrado, invalidando automáticamente los anteriores sin alertar al coactor.

3. Minimización de Datos en Logs:
   Los logs centralizados de auditoría (mapeados bajo el control NIST AU-9) tienen prohibido recolectar direcciones IP, identificadores de navegador (USER-AGENTS) o metadatos de sesión en el microservicio de escrutinio.
