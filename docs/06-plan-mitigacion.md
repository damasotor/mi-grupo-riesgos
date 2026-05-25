══════════════════════════════════════════════════════════

 7. PLAN DE MITIGACIÓN

══════════════════════════════════════════════════════════

 7.1 Controles de Seguridad

| ID | Estado | Amenaza | Control de Seguridad | Prioridad |
| ------------ | -------- | ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
|     C01     | Pend. | TH01 (DDoS / T1498)          | Desplegar módulo Anti-DDoS, WAF y Rate Limiting en el API Gateway.                                                                                           | Alta    |
|     C02     | Pend. | TH02 (Tampering / T1565) | Aislar el Servicio de Escrutinio en VLAN dedicada y aplicar validaciones criptográficas de integridad (Hashes/Firmas). | Alta    |
|     C03     | Pend. | TH04 (Info. Disc.)                  | Implementar protocolo de disociación de logs (Tokenización de PII).                                                                                              | Media|
|     C04     | Pend. | TH03 (Spoofing / T1078)    | Forzar autenticación robusta (MFA / CI Digital) y vinculación de tokens de sesión para evitar su robo (T1528/T1557).    | Alta    |

---

 Detalle de Implementación de Controles Críticos (Escenario 8)

Para mitigar eficazmente las técnicas de ataque identificadas en el análisis de MITRE ATT&CK, se especifican las siguientes directrices técnicas para los controles pendientes:

A. Mitigación contra Acceso Inicial y Robo de Credenciales/Tokens (T1078, T1557, T1528)

Control de Autenticación de Sesión (C04): Implementar *Session Binding* (vinculación de sesión). Los tokens de acceso (JWT) emitidos deben ligarse criptográficamente a la huella digital del dispositivo o a la dirección IP origen. Si un atacante intercepta un token mediante técnicas *Adversary-in-the-Middle* (AitM), el token será rechazado automáticamente al ser presentado desde una infraestructura externa.
Gestión de Expiración:     Configurar un tiempo de vida corto (*Short-lived tokens*) para los tokens de acceso, complementado con mecanismos de *Refresh Tokens* de un solo uso.

B. Mitigación contra la Alteración de Datos (Data Manipulation - T1565)

Control de Integridad del Escrutinio (C02): Todo voto o registro transmitido desde el cliente hacia el Servicio de Escrutinio debe incluir una firma digital o una validación criptográfica basada en hashes calculada en origen. El backend validará la integridad antes de consolidar el registro en la base de datos para asegurar que ningún intermediario o cuenta comprometida modificó los datos en tránsito.

C. Mitigación contra Ataques de Denegación de Servicio (Network DoS - T1498)

Protección Perimetral (C01): Desplegar reglas de "Rate Limiting" estrictas en el API Gateway para restringir la cantidad de peticiones concurrentes por IP. Complementar con un servicio de mitigación volumétrica (WAF/Anti-DDoS) en la capa perimetral para absorber inundaciones de tráfico directo (*Direct Flood*) antes de que impacten en los servidores de la aplicación.

---

 7.2 Controles por Categoría

    CONTROLES PREVENTIVOS:    

 [x] Autenticación multifactor (Para administradores y auditores).
 [x] Cifrado de datos en tránsito y en reposo (mTLS, Cifrado Homomórfico).
 [x] Validación de entrada (En el API Gateway).
 [x] Principio de mínimo privilegio (Control de acceso basado en roles - RBAC).
 [x] Segmentación de red (Aislamiento del HSM y Servicio de Escrutinio en VLAN dedicada).
 [x] Cifrado TLS 1.3 forzado y HSTS para mitigar intercepciones AitM.    

    CONTROLES DETECTIVOS:    

 [x] Logging de eventos de seguridad (Centralizado e inmutable).
 [x] Monitorización con SIEM (Específicamente configurado para alertar sobre uso anómalo de cuentas legítimas - T1078).
 [x] Alertas de anomalías (Desviaciones en el volumen de emisión de votos o uso simultáneo de un mismo token de sesión desde múltiples IPs).

    CONTROLES CORRECTIVOS:    

 [x] Plan de respuesta a incidentes (Procedimientos ante caída del API Gateway por DoS).
 [x] Procedimientos de backup (Para configuraciones, excluyendo claves del HSM; respaldos en modalidad inmutable para garantizar recuperación ante manipulación de datos).
 
 
 ══════════════════════════════════════════════════════════

     8. MATRIZ DE CONTROLES (NIST / NTCSI)

══════════════════════════════════════════════════════════

| ID        | Control                                  | Descripción                                                                                                     | Referencia |
| --------- | -------------------------------------- | -------------------------------------------------------------------------------------------------- | -----------------|
| SC-13  | Cryptographic Protection | Uso de HSM FIPS 140-2 para protección de claves maestras.            | NIST SC-13 |
| SC-5    | Denial of Service Prot.       | Controles de mitigación volumétrica en el perímetro.                         | NIST SC-5   |
| AC-4   | Information Flow Enf.         | Control estricto de flujo entre los *Trust Boundaries*.                      | NIST AC-4   |
| AU-9   | Protection of Audit Info    | Uso de Blockchain para garantizar el "no repudio" de la auditoría.  | NIST AU-9  |
| IA-2    | Identification and Auth.     | Verificación unívoca del ciudadano contra el padrón electoral.        | NIST IA-2    |


*Nota: Los controles se mapean contra NIST SP 800-53 Rev. 5, cumpliendo con las exigencias de Buenas Prácticas de AGESIC.* 

La publicación NIST SP 800-53 Rev. 5 (titulada originalmente en inglés "Security and Privacy Controls for Information Systems and Organizations") es uno de los estándares de ciberseguridad más importantes y detallados del mundo, desarrollado por el Instituto Nacional de Estándares y Tecnología de los Estados Unidos (NIST).

En términos sencillos, es un catálogo masivo de controles de seguridad y privacidad diseñado para proteger los sistemas de información corporativos y gubernamentales contra todo tipo de amenazas.

A continuación, se detallan sus pilares fundamentales, innovaciones de la Revisión 5 y cómo se estructura:
1. ¿Cómo se organiza? (Las Familias de Controles)

El documento agrupa sus cientos de controles en 20 familias exhaustivas. Cada familia cubre un aspecto operativo, técnico o de gestión específico dentro de una organización:

    AC (Access Control / Control de Acceso): Quién puede entrar al sistema y qué puede hacer (por ejemplo, el control de acceso basado en roles o la vinculación de tokens).

    AT (Awareness and Training / Concienciación y Capacitación): Educación de los usuarios contra el phishing o ingeniería social.

    AU (Audit and Accountability / Auditoría y Rendición de Cuentas): Registro inmutable de logs para saber qué pasó, cuándo y quién lo hizo.

    CM (Configuration Management / Gestión de Configuración): Mantener inventarios de hardware y software y líneas base seguras.

    CP (Contingency Planning / Planificación de Contingencia): Planes de respuesta ante desastres, respaldos (backups) y continuidad de operaciones.

    IA (Identification and Authentication / Identificación y Autenticación): Verificación de identidades mediante MFA, certificados digitales o biometría.

    Incident Response (IR / Respuesta a Incidentes): Cómo reaccionar, contener y recuperarse de un ataque activo.

    RA (Risk Assessment / Evaluación de Riesgos): Metodologías para identificar vulnerabilidades y calcular el impacto (muy conectado con procesos tipo STRIDE o DREAD).

    SC (System and Communications Protection / Protección de Sistemas y Comunicaciones): Cifrado de datos en tránsito (TLS 1.3), protección perimetral (Firewalls/WAF), segmentación de redes y uso de hardware criptográfico (HSM).

    SI (System and Information Integrity / Integridad de Sistemas e Información): Protección contra malware, monitoreo del sistema y parches de seguridad.
