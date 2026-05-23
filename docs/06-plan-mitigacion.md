══════════════════════════════════════════════════════════

     7. PLAN DE MITIGACIÓN

══════════════════════════════════════════════════════════

7.1 Controles de Seguridad

| ID | Estado | Amenaza | Control de Seguridad | Prioridad |
| --- | --- | --- | --- | --- |
| C01 | Pend. | TH01 (DDoS) | Desplegar módulo Anti-DDoS y Rate Limiting en el API Gateway. | Alta |
| C02 | Pend. | TH02 (Tampering) | Aislar el Servicio de Escrutinio en VLAN dedicada sin acceso a Internet. | Alta |
| C03 | Pend. | TH04 (Info. Disc.) | Implementar protocolo de disociación de logs (Tokenización de PII). | Media |
| C04 | Pend. | TH03 (Spoofing) | Forzar autenticación robusta (ej. CI Digital o MFA biométrico) en la App. | Media |

7.2 Controles por Categoría

CONTROLES PREVENTIVOS:

* [x] Autenticación multifactor (Para administradores y auditores).
* [x] Cifrado de datos en tránsito y en reposo (mTLS, Cifrado Homomórfico).
* [x] Validación de entrada (En el API Gateway).
* [x] Principio de mínimo privilegio (Control de acceso basado en roles - RBAC).
* [x] Segmentación de red (Aislamiento del HSM y Servicio de Escrutinio).

CONTROLES DETECTIVOS:

* [x] Logging de eventos de seguridad (Centralizado e inmutable).
* [x] Monitorización con SIEM (Para detectar T1078 Valid Accounts).
* [x] Alertas de anomalías (Desviaciones en el volumen de emisión de votos).

CONTROLES CORRECTIVOS:

* [x] Plan de respuesta a incidentes (Procedimientos ante caída del API Gateway).
* [x] Procedimientos de backup (Para configuraciones, excluyendo claves del HSM).

══════════════════════════════════════════════════════════

     8. MATRIZ DE CONTROLES (NIST / NTCSI)

══════════════════════════════════════════════════════════


*Nota: Los controles se mapean contra NIST SP 800-53 Rev. 5, cumpliendo con las exigencias de Buenas Prácticas de AGESIC.* 

| ID | Control | Descripción | Referencia |
| --------- | -------------------------------------- | -------------------------------------------------------------------------------------------------- | -----------------|
| SC-13 | Cryptographic Protection | Uso de HSM FIPS 140-2 para protección de claves maestras.            | NIST SC-13 |
| SC-5 | Denial of Service Prot.         | Controles de mitigación volumétrica en el perímetro.                         | NIST SC-5   |
| AC-4 | Information Flow Enf.          | Control estricto de flujo entre los *Trust Boundaries*.                      | NIST AC-4   |
| AU-9 | Protection of Audit Info     | Uso de Blockchain para garantizar el "no repudio" de la auditoría.  | NIST AU-9   |
| IA-2 | Identification and Auth.       | Verificación unívoca del ciudadano contra el padrón electoral.        | NIST IA-2    |

