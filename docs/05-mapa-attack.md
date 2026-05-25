══════════════════════════════════════════════════════════

     6. MAPA DE ATAQUE (ATT&CK)

══════════════════════════════════════════════════════════

6.1 Técnicas Identificadas

| ID | Técnica | Táctica (Tactic) | Mitigación |
| --------- | ------------------------------------------------- | --------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| T1498 | Network Denial of Service            | Impact                                             | Implementación de WAF y mitigación Anti-DDoS en el API Gateway.            |
| T1565 | Data Manipulation                           | Impact                                             | Uso de cifrado homomórfico y registro inmutable en Blockchain.                 |
| T1557 | Adversary-in-the-Middle                | Credential Access / Collection | Autenticación mutua (mTLS) entre microservicios (Trust Boundary #2).      |
| T1528 | Steal Application Access Token   | Credential Access                        | Expiración corta de tokens JWT y validación estricta de origen.                     |
| T1078 | Valid Accounts                                 | Initial Access                                 | MFA obligatorio para administradores y monitoreo de anomalías de login. |

6.2 Visualización
![Mapa de Ataque MITRE ATT&CK Escenario 8](diagrams/Arquitectura.png)
