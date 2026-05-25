Parte 1

Antes de pedir soluciones, dale contexto a la IA para que entienda el alcance.

    Prompt: "Actúa como un experto en ciberseguridad. Estoy realizando un análisis de riesgos para [menciona el sistema o aplicación]. Tengo un inventario de activos y he identificado amenazas usando STRIDE. ¿Qué información adicional necesitas de mi parte para realizar una evaluación de riesgos precisa?"

Parte 2 Análisis (STRIDE / DREAD)

Si necesitas ayuda para completar tus tablas de análisis:

    Prompt: "Tengo este componente: [describe un componente]. Según la metodología STRIDE, ¿cuáles son las amenazas potenciales más críticas para este elemento y qué controles compensatorios recomendarías implementar para cada una?"

    Prompt para DREAD: "Ayúdame a calcular la severidad según DREAD para esta amenaza: [describe amenaza]. Asigna una puntuación del 1 al 10 a cada parámetro (Daño, Reproducibilidad, Explotabilidad, Usuarios afectados, Descubribilidad) y justifica brevemente cada nota."

Parte 3: Integración de MITRE ATT&CK

Dado que tienes un layer.json (que es un archivo de exportación de MITRE ATT&CK Navigator), puedes usar la IA para analizarlo:

    Prompt: "Analiza el siguiente archivo JSON de MITRE ATT&CK que adjunto. Identifica las tácticas y técnicas predominantes. ¿Qué medidas de mitigación o detección prioritarias debería implementar basándome en estas técnicas?"

Parte 4: Documentación y Plan de Mitigación

Para redactar la parte final de tu informe:

    Prompt: "Basado en los riesgos de prioridad ALTA identificados en mi análisis, redacta un plan de mitigación estructurado. El plan debe incluir: nombre del control, descripción, nivel de esfuerzo (bajo/medio/alto) y el impacto esperado en la reducción del riesgo."
