           1.1 Descripción del Sistema

El proyecto consiste en una plataforma de votación electrónica diseñada para garantizar el ejercicio del sufragio de forma remota, utilizando tecnología de cifrado homomórfico y una arquitectura basada en microservicios sobre una infraestructura inmutable (Blockchain). El sistema permite al usuario emitir su voto, el cual es registrado, cifrado y contabilizado sin revelar la identidad del votante, manteniendo la integridad del resultado final.

           1.2 Alcance del Análisis

    Componentes incluidos:    

1     Frontend (Web/App):     Interfaz de usuario para la emisión del voto.
2     API Gateway / WAF:     Punto de entrada y filtrado de tráfico.
3     Servicio de Autenticación:     Módulo de validación de identidad del padrón electoral.
4     Servicio de Emisión y Cifrado:     Módulo de procesamiento y cifrado homomórfico.
5     Repositorio Cifrado (Blockchain):     Almacenamiento inmutable de los votos.

    Componentes fuera de alcance:    

-Preferencias de los votantes:     Los datos específicos sobre qué candidato elige el votante son tratados como un "payload" cifrado opaco; el análisis de riesgo no audita la veracidad o el contenido de dicha preferencia.
-Entorno físico del votante:     La seguridad del dispositivo personal del usuario (laptop/celular) es responsabilidad del mismo y queda fuera del perímetro de control de la organización.
-Candidatos y Opciones:     Los registros de candidatos, nombres y partidos en la base de datos de configuración se asumen como datos íntegros proporcionados por la autoridad electoral externa, no siendo objeto de este análisis de vulnerabilidades técnicas.

           1.3 Supuestos

1.3 Supuestos y Marco Normativo

    Marco de Referencia: Este análisis se desarrolla tomando como base las Buenas Prácticas de Seguridad de la Información (NTCSI) dictadas por AGESIC.

    Estándares Criptográficos: Aunque la normativa nacional uruguaya no impone una certificación de hardware equivalente a FIPS 140-2, este proyecto adopta dicho estándar como mejor práctica internacional (Best Practice). Esta decisión asegura la debida diligencia en la protección de claves criptográficas y el cumplimiento de los niveles de seguridad requeridos para sistemas críticos de infraestructura pública.

    Entorno de Red: Se asume que el tráfico de Internet es un entorno de confianza cero (Zero Trust), por lo que se implementan controles de cifrado en tránsito y autenticación mutua por defecto.

    Continuidad: Se asume que la autoridad electoral proporciona el padrón actualizado en tiempo y forma, cumpliendo con la Ley Nº 18.331 de Protección de Datos Personales.

---
     2. IDENTIFICACIÓN DE ACTIVOS

2.1 Activos de Información

| ID | Activo | Tipo Dato | Clasificación | Propietario |
| ------ | ----------------------------------------------- | ---------------------------- | ---------- | ------------------------------ |
| A01 | Votos Cifrados                                | Sensible/Electoral | Alta     | Ciudadanía                  |
| A02 | Credenciales de Padrón                | PII (Identificable)   | Alta     | Autoridad Electoral |
| A03 | Claves Criptográficas Maestras | Confidencial             | Crítica | Seguridad TI               |

Justificación Técnica:
    A01 (Alta): Se clasifica como "Alta" porque, aunque está cifrado, representa la voluntad del votante. Su integridad es absoluta; un cambio aquí invalida la elección.
    A02 (Alta - PII): Las credenciales y datos del padrón son *Información Personal Identificable* (PII). Según la Ley N° 18.331 (Uruguay), su divulgación tiene implicaciones legales severas, requiriendo protección estricta.
    A03 (Crítica): Son el "anillo de poder". Si un atacante compromete las llaves en el HSM , puede descifrar votos, falsificar firmas y destruir completamente el anonimato y la integridad del sistema. Es el único activo "Crítico" por su impacto transversal.


2.2 Activos Tecnológicos

| ID | Activo | Tipo | Criticidad | Notas |
| ------ | -------------------------------------------------- | ------------------------ |----------- | -----------------------------------------------------------------------|
| T01 | API Gateway / WAF                            | Software             | Alta      | Filtro de entrada perimetral                                    |
| T02 | Servicio de Escrutinio                       | Software              | Alta      | Lógica de conteo (Trust Boundary #3)                 |
| T03 | Repositorio Blockchain                    | Software/Infra   | Crítica  | Registro de auditoría inmutable                             |
| T04 | HSM (Hardware Security Module) | Hardware             | Crítica  | Gestión de llaves maestras                                      |

Justificación Técnica:
    T01 (Alta): Es la primera línea de defensa contra ataques volumétricos (DDoS) e inyecciones (SQLi/XSS). Si falla, el *backend* queda expuesto.
    T02 (Alta): Ejecuta la lógica de negocio final. Su compromiso podría permitir la manipulación de resultados antes de ser publicados en el Dashboard.
    T03 (Crítica): Garantiza la propiedad de "no repudio" (auditabilidad). Si la Blockchain  se corrompe o se pierden los consensos, no hay forma de auditar la elección matemáticamente. 
	T04 (Crítica): Como hardware físico, es el único punto donde el material criptográfico "toca" memoria en texto plano para operaciones de firma. Su caída anula la capacidad de emitir y escrutar votos.


2.3 Activos Intangibles

    Confianza Ciudadana: El activo más crítico; si se pierde, el sistema es ilegítimo políticamente, independientemente de su robustez técnica.
    Reputación del Organismo Electoral: Impacto directo en la estabilidad institucional.

