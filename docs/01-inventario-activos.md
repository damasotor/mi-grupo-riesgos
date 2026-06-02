1.1 Descripcion del Sistema

El proyecto consiste en una plataforma de votacion electronica pensada para permitir la emision remota del sufragio. El sistema utiliza cifrado homomorfico, servicios separados por funciones y un registro de auditoria basado en blockchain. Su objetivo es proteger la identidad del votante, mantener la integridad del voto y permitir un conteo confiable.

1.2 Alcance del Analisis

Componentes incluidos:

1. Frontend (Web/App): interfaz de usuario para la emision del voto.
2. API Gateway / WAF: punto de entrada y filtrado de trafico.
3. Servicio de Autenticacion: modulo de validacion de identidad contra el padron electoral.
4. Servicio de Emision y Cifrado: modulo de procesamiento y cifrado del voto.
5. Repositorio Cifrado / Blockchain: almacenamiento de votos cifrados y registro de auditoria.

Componentes fuera de alcance:

- El hardware personal del votante, como laptop o celular, no forma parte del control directo de la organizacion.
- Las preferencias politicas del votante no se analizan como contenido funcional, ya que el voto se trata como un dato cifrado.
- Los datos de candidatos y partidos se asumen como provistos por la autoridad electoral.

Nota de alcance:

Aunque el dispositivo personal del votante queda fuera del control directo de la organizacion, la aplicacion web y la aplicacion movil si se consideran activos del sistema, ya que forman parte de la plataforma de votacion.

1.3 Supuestos y Marco Normativo

Marco de referencia: este analisis toma como base las buenas practicas de seguridad de la informacion y la normativa nacional aplicable.

Estandares criptograficos: se considera como buena practica el uso de proteccion reforzada para las claves criptograficas del sistema.

Entorno de red: se asume que Internet es un entorno no confiable, por lo que deben aplicarse controles de cifrado en transito y autenticacion.

Continuidad: se asume que la autoridad electoral entrega el padron actualizado en tiempo y forma.

---

2. IDENTIFICACION DE ACTIVOS

El inventario de activos permite identificar los elementos mas importantes del sistema y definir que debe protegerse para mantener la confidencialidad, integridad y disponibilidad de la plataforma de votacion.

2.1 Activos de Informacion

| ID  | Activo                          | Tipo                     | Nivel | Motivo de proteccion                                                | Propietario         |
| --- | ------------------------------- | ------------------------ | ----- | ------------------------------------------------------------------- | ------------------- |
| A01 | Votos cifrados                  | Informacion electoral    | Alta  | Representan la voluntad del votante y no deben ser alterados        | Ciudadania          |
| A02 | Datos de autenticacion          | Datos personales         | Alta  | Permiten validar la identidad de quien vota                         | Autoridad Electoral |
| A03 | Claves criptograficas del sistema | Informacion sensible   | Alta  | Se usan para cifrar, firmar y proteger procesos criticos            | Seguridad TI        |
| A04 | Base de datos del padron        | Datos personales         | Alta  | Contiene informacion de los votantes                                | Autoridad Electoral |
| A05 | Repositorio de votos cifrados   | Informacion electoral    | Alta  | Almacena los votos emitidos                                         | Ciudadania          |
| A06 | Registros de auditoria          | Informacion sensible     | Media | Permiten revisar eventos y acciones del sistema                     | Seguridad TI        |
| A07 | Tokens de sesion                | Informacion sensible     | Alta  | Permiten mantener sesiones autenticadas                             | Seguridad TI        |
| A08 | Claves de firma digital         | Informacion sensible     | Alta  | Se utilizan para validar operaciones importantes                    | Seguridad TI        |
| A09 | Resultados electorales          | Informacion electoral    | Alta  | Deben reflejar correctamente el conteo de votos                     | Autoridad Electoral |
| A10 | Configuracion de la eleccion    | Configuracion critica    | Alta  | Define candidatos, fechas y reglas del proceso                      | Autoridad Electoral |
| A11 | Reportes de auditoria           | Documentacion sensible   | Media | Sirven para verificar el funcionamiento del sistema                 | Auditores           |

Justificacion tecnica:

- A01: aunque el voto este cifrado, sigue siendo uno de los activos principales del sistema y su alteracion afecta directamente la eleccion.
- A02: los datos de autenticacion del votante deben protegerse porque un uso indebido podria permitir la suplantacion de identidad.
- A03: si estas claves son comprometidas, un atacante podria afectar la confidencialidad e integridad del sistema.
- A04: el padron contiene informacion personal que debe resguardarse.
- A05: este repositorio guarda los votos emitidos y debe mantenerse integro.
- A06: los registros de auditoria ayudan a revisar lo ocurrido en el sistema y a investigar incidentes.
- A07: si un token de sesion es robado, un atacante podria actuar como si fuera un usuario legitimo.
- A08: estas claves permiten validar operaciones importantes y por eso deben protegerse.
- A09: los resultados son un activo sensible porque reflejan el resultado final o parcial del proceso.
- A10: una modificacion no autorizada en la configuracion podria alterar el desarrollo de la eleccion.
- A11: los reportes de auditoria son importantes para documentar controles y revisiones.

2.2 Activos Tecnologicos

| ID  | Activo                     | Tipo           | Nivel | Motivo de proteccion                                      |
| --- | -------------------------- | -------------- | ----- | --------------------------------------------------------- |
| T01 | API Gateway / WAF          | Software       | Alta  | Controla y filtra el acceso al sistema                    |
| T02 | Servicio de Escrutinio     | Software       | Alta  | Realiza el conteo de votos                                |
| T03 | Repositorio Blockchain     | Infraestructura| Alta  | Guarda trazabilidad y evidencia de auditoria              |
| T04 | HSM                        | Hardware       | Alta  | Protege las claves criptograficas                         |
| T05 | Nodos Blockchain           | Infraestructura| Alta  | Validan y almacenan bloques                               |
| T06 | Base de datos de votos     | Infraestructura| Alta  | Almacena votos cifrados                                   |
| T07 | Base de datos de autenticacion | Infraestructura| Alta | Gestiona acceso e identidad                               |
| T08 | Aplicacion movil           | Software       | Alta  | Permite votar desde celular                               |
| T09 | Aplicacion web             | Software       | Alta  | Permite votar desde navegador                             |
| T10 | Pipeline CI/CD             | Software       | Media | Participa en despliegues y actualizaciones                |
| T11 | Red interna                | Infraestructura| Alta  | Conecta servicios internos                                |
| T12 | SIEM / monitoreo           | Software       | Media | Ayuda a detectar incidentes                               |
| T13 | Dashboard de resultados    | Software       | Alta  | Publica resultados y estado del proceso                   |
| T14 | Sistema de backups         | Infraestructura| Media | Permite recuperacion ante fallos                          |
| T15 | Consola de administracion  | Software       | Alta  | Permite gestionar funciones criticas del sistema          |
| T16 | Servicio DNS               | Infraestructura| Media | Hace accesible la plataforma a los usuarios               |

Justificacion tecnica:

- T01: es la entrada principal al sistema y ayuda a filtrar trafico no deseado.
- T02: participa en el conteo de votos, por lo que su correcto funcionamiento es clave.
- T03: aporta trazabilidad a las operaciones registradas.
- T04: protege el material criptografico utilizado por el sistema.
- T05: permiten sostener el funcionamiento del registro blockchain.
- T06: almacena votos cifrados y debe mantenerse disponible e integro.
- T07: controla procesos de autenticacion y acceso.
- T08: es parte de la plataforma y debe protegerse contra fallos y manipulaciones.
- T09: es una interfaz publica del sistema y debe mantenerse segura.
- T10: si se compromete, podria afectar actualizaciones o despliegues.
- T11: soporta la comunicacion entre servicios internos.
- T12: ayuda a detectar comportamientos anormales o incidentes.
- T13: muestra informacion sensible del proceso y de sus resultados.
- T14: permite recuperar informacion en caso de fallo o incidente.
- T15: si se usa indebidamente, podria afectar funciones criticas del sistema.
- T16: si falla o es alterado, los usuarios podrian no poder acceder a la plataforma.

2.3 Activos Intangibles

- Confianza de la ciudadania.
- Reputacion del organismo electoral.
- Legitimidad del proceso electoral.
