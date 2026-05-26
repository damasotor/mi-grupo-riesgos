     3. ARQUITECTURA DEL SISTEMA

3.1 Diagrama de Arquitectura

(Inserta aquí la imagen exportada de tu archivo `Arquitectura.xml`  - Se recomienda formato .png para el Markdown)

3.2 Flujo de Datos

El flujo de datos se diseñó garantizando que la identidad del votante y el sentido del voto nunca viajen juntos ni persistan en la misma base de datos de forma vinculable:

1. El `Votante` (T01)  envía credenciales a través de Internet.

2. El `API Gateway` (T01) filtra y enruta al `Servicio de Autenticación`.

3. Se valida contra la `Base de Datos de Votantes`  y se emite un token anónimo temporal.

4. El votante utiliza este token para interactuar con el `Servicio de Emisión de Votos`.

5. El voto es cifrado usando llaves del `HSM` (T04).

6. Se registra en el `Repositorio de Votos Cifrados` y se escribe un hash en la `Blockchain` (T03).

7. Al cierre, el `Servicio de Escrutinio` (T02)  accede a los votos y los contabiliza.


3.3 Actores del Sistema

| Actor | Descripción | Privilegios |
|--------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Usuario final                                     | Ciudadano que emite el sufragio.                   | Limitados (Emisión): Solo puede autenticarse y enviar un *payload* de voto. No tiene lectura sobre el repositorio general. |
| Autoridad Electoral / Auditores | Gestores y supervisores de la plataforma. | Segmentados (Auditoría/Conteo): Tienen acceso de lectura a los *Logs* y al *Dashboard* de resultados. El inicio del escrutinio requiere "Secreto Compartido" (múltiples autoridades requeridas). |
| Atacante                                            | Actor malicioso (interno o externo).            | A evaluar: Se busca prevenir la escalada de privilegios a través de los Trust Boundaries. |

3.4 Límites de Confianza (Trust Boundaries)

Límites de Confianza #1 (Internet): Separa el dispositivo no confiable del usuario de la red gestionada por la organización. Justifica la implementación de controles perimetrales estrictos (WAF, Rate Limiting).
Límites de Confianza #2 (Separación Identidad/Voto): Frontera lógica interna. Se justifica para cumplir con el requisito legal del "secreto del sufragio". En esta línea se "rompe" el vínculo entre el "Servicio de Autenticación" y el "Servicio de Emisión".
Límites de Confianza #3 (Escrutinio): Aislamiento profundo. El "Servicio de Escrutinio"  no debe tener acceso directo desde Internet, ni siquiera para las autoridades. Se justifica para prevenir manipulaciones prematuras de los resultados (Tampering) durante la jornada electoral.

