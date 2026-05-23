
══════════════════════════════════════════════════════════

5. ANÁLISIS DE RIESGOS - METODOLOGÍA DREAD 

══════════════════════════════════════════════════════════

5.1 Criterios DREAD

| Criterio | Descripción | Peso |
| --- | --- | --- |
| Damage | Daño potencial si se explota (1-10) | 1.0 |
| Reproducibility | Facilidad para reproducir el ataque (1-10) | 1.0 |
| Exploitability | Facilidad técnica para ejecutar el ataque (1-10) | 1.0 |
| Affected Users | Proporción de usuarios o componentes afectados (1-10) | 1.0 |
| Discoverability | Facilidad para encontrar la vulnerabilidad (1-10) | 1.0 |

5.2 Matriz de Riesgos

| ID | Amenaza | D | R | E | A | D | TOTAL | Nivel |
| -------- | -------------------------------------------- | --- | -- | -- | ---- | ---- | -------- | ------------- |
| TH01 | DDoS en Perímetro                    | 8   | 9 | 8 | 10 | 10 | 45/50 | CRÍTICO |
| TH02 | Tampering del Voto                   | 10 | 6 | 4 | 10 | 5   | 35/50 | ALTO       |
| TH04 | Correlación (Info. Disclosure) | 10 | 3 | 3 | 10 | 4    | 30/50 | ALTO      |
| TH03 | Spoofing de Identidad              | 9   | 7 | 5 | 2   | 6    | 29/50 | MEDIO   |

Justificación Técnica de las Puntuaciones (Auditoría de Riesgo):
TH01 (TOTAL: 45 - CRÍTICO): Se califica con el máximo puntaje en "Affected Users" (10) y "Discoverability" (10) porque el portal de votación debe ser de conocimiento y acceso público por definición. Su "Exploitability" es alta (8) dado que adquirir ataques volumétricos como servicio (DDoS-for-hire) no requiere conocimientos avanzados. Es el riesgo más inminente a nivel operativo.
TH02 (TOTAL: 35 - ALTO): El "Damage" es absoluto (10), ya que alterar el voto destruye el propósito del sistema. Sin embargo, su "Exploitability" se reduce significativamente (4) gracias a la presencia del HSM y el cifrado planificado en la arquitectura, lo que obliga al atacante a vulnerar controles criptográficos complejos o tener acceso interno privilegiado.
TH04 (TOTAL: 30 - ALTO): Al igual que la anterior, el daño de romper el secreto del sufragio es catastrófico (D=10) y afecta a todo el padrón (A=10). Su puntuación total disminuye debido a la baja "Reproducibility" (3) y "Exploitability" (3); lograr vincular las bases de datos requiere vulnerar el "Trust Boundary #2" y poseer permisos profundos de administrador de bases de datos, mitigando el riesgo frente a atacantes externos.
TH03 (TOTAL: 29 - MEDIO): El robo de sesión (ej. mediante phishing) es fácil de reproducir (R=7). Sin embargo, la calificación general cae a "Medio" porque los "Affected Users" se evalúan en 2. El robo de credenciales suele ser un ataque "uno a uno"; un atacante tendría que vulnerar a los votantes de forma masiva e individualizada para alterar el resultado global de la elección, lo que escala logísticamente de forma ineficiente.


5.3 Escala de Severidad

    CRÍTICO (40-50): Remediar inmediatamente. (Ej. Implementar protección Anti-DDoS en TH01).
    ALTO (30-39): Remediar ASAP. (Ej. Reforzar aislamiento de red en TH02).
    MEDIO (20-29): Remediar en siguiente sprint. (Ej. Añadir validación multifactor MFA en TH03).
    BAJO (10-19): Monitorear.
    MÍNIMO (1-9): Aceptar riesgo.

