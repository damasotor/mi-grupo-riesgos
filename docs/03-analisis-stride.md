     4. ANÁLISIS DE AMENAZAS - METODOLOGÍA STRIDE

4.1 Aplicación de STRIDE 

┌────────────────────────────────────────────────────────┐
│ CATEGORÍAS STRIDE                                                                                                   │
├───────────────┬────────────────────────────────────────┤
│ S - Spoofing              │ Suplantación de identidad                                                  │
│ T - Tampering           │ Manipulación de datos o código                                       │
│ R - Repudiation        │ Negación de acciones realizadas                                      │
│ I - Info. Disclosure   │ Divulgación de información sensible                               │
│ D - DoS                      │ Denegación de servicio                                                        │
│ E - Elevation of P.   │ Elevación de privilegios                                                       │
└───────────────┴────────────────────────────────────────┘

4.2 Matriz de Amenazas por Componente

| ID | Componente | Categoría | Descripción de la Amenaza | CVE/CWE |
| -------- | -------------------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ |
| TH01 | API Gateway / WAF            | DoS                      | Saturación de red y recursos para impedir el acceso de los votantes durante la jornada electoral.                              | CWE-400 |
| TH02 | Servicio de Emisión            | Tampering          | Interceptación del flujo de red para alterar el contenido del voto antes de su cifrado y registro en la Blockchain. | CWE-353 |
| TH03 | Servicio de Autenticación | Spoofing             | Un atacante utiliza credenciales robadas o tokens de sesión falsificados para emitir un voto ilegítimo.                    | CWE-287 |
| TH04 | Base de Datos / Backend | Info. Disclosure | Correlación maliciosa en memoria o base de datos que vincula la identidad del padrón con el voto emitido.           | CWE-200 |

4.3 Detalle de Amenazas Principales

--- AMENAZA TH01: Denegación de Servicio Distribuido (DDoS) en Perímetro ---

    Categoría STRIDE: D (DoS)
    Descripción: Un actor externo coordina una botnet para inundar el API Gateway con peticiones malformadas, agotando los recursos de procesamiento e impidiendo que los votantes legítimos accedan a la plataforma.
    Activos Afectados: T01 (API Gateway), Confianza Ciudadana (Activo Intangible).
    Probabilidad: Alta (Los ataques volumétricos son frecuentes y de bajo costo de ejecución).
    Impacto: Alto (Invalida el principio de disponibilidad del sufragio).
    Justificación de la Amenaza: Se identifica en el *Trust Boundary #1* porque es la única interfaz obligatoriamente pública del sistema. Sin acceso, el sistema de votación carece de utilidad.
    Técnicas ATT&CK relacionadas:
_ [T1498] Network Denial of Service.
_ [T1499] Endpoint Denial of Service.



--- AMENAZA TH02: Manipulación del Voto en Tránsito (Man-in-the-Middle) ---

    Categoría STRIDE: T (Tampering)
    Descripción: Un atacante que logra comprometer la red interna (o un empleado malintencionado) intercepta la comunicación entre el Servicio de Autenticación y el Servicio de Emisión, modificando la selección del votante antes de que el HSM aplique la firma criptográfica.
    Activos Afectados: A01 (Votos Cifrados), T03 (Repositorio Blockchain).
    Probabilidad: Baja/Media (Requiere superar las barreras perimetrales o ser un *insider*).
    Impacto: Crítico (Compromete la integridad total de la elección).
    Justificación de la Amenaza: Ocurre cruzando el *Trust Boundary #2*. Una vez que el voto se cifra con el HSM y llega a la Blockchain, es inmutable. Por ende, la única ventana del atacante para alterar el resultado es en este tránsito específico de la capa de aplicación.
    Técnicas ATT&CK relacionadas:
_ [T1557] Adversary-in-the-Middle.
_ [T1565] Data Manipulation.


