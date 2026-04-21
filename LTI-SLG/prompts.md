# Prompt 1 (Gemini 3 (via Gemini UI)): Adjunto: Guía que se encuentra en el apartado de historias de usuario

```
En base a esta guía de creación de historias de usuario, genérame un template.md para reflejar la plantilla en un documento. Añade 2 apartados: Historias de Usuario Relacionadas y Evaluación INVEST
```

# Prompt 2 (Claude Sonnet 4.6 (via Copilot)):

```
Actúa como un Product Owner senior con experiencia en metodologías ágiles.

A partir del PRD @LTI-SLG.md, genera 3 User Stories
que cumplan los criterios definidos en @user-story-template.md. En @user-story-example.md tienes un ejemplo de una historia definida.

Después de generar las historias, sugiere un orden de priorización
para el MVP y justifica tu decisión. Documentalo todo en @file:UserStories-SLG.md
```

# Prompt 3: Generación de backlog

## Prompt 3.1 (Claude Sonnet 4.6 (via Copilot)): Ejemplo adaptado de 'Historias de usuarios'

```
Considera el backlog de producto creado anteriormente para el sistema LTI. Analiza las historias de usuario de @UserStories-SLG.md. Estima por cada item en el backlog (genera una tabla markdown):
- Impacto en el usuario y valor del negocio
- Urgencia basada en tendencias del mercado y feedback de usuarios
- Complejidad y esfuerzo estimado de implementación
- Riesgos y dependencias entre tareas
```

#### Resultado:

El prompt generó la sección "Estimación del Backlog" con dos tablas: una a nivel de User Story (impacto, urgencia, complejidad/esfuerzo, riesgos) y otra granular por tarea con Story Points en escala Fibonacci. Incluye un resumen ejecutivo con total de puntos (~63 pts) y semanas estimadas por sprint.

#### Conclusiones:

El prompt es directo y orientado a cuantificación: produce estimaciones numéricas concretas (story points, semanas, equipo de referencia) que son útiles para la planificación de sprints.
Sin embargo, su estructura es reactiva: responde exactamente a las 4 dimensiones pedidas sin añadir organización estratégica adicional (no hay MoSCoW, no hay justificación valor/costo, no hay clasificación por sprint de forma explícita en la tabla principal).
La ausencia de un rol/persona en el prompt limita la profundidad del razonamiento: el modelo no adopta ninguna perspectiva estratégica, solo responde como "generador de tablas".
Fortaleza principal: las estimaciones cuantitativas son accionables para un equipo de desarrollo que necesita planificar velocidad de sprint.

## Prompt 3.2.1 (Gemini 3 (via Gemini UI)): Copiar buenas prácticas de 'Priorización del Backlog de Producto' y generar meta prompt a través de Gemini 3

```
Dadas las siguientes buenas prácticas de Priorización del Backlog, dame un prompt de un experto de Product Owner senior con experiencia en metodologías ágiles:



Priorización del Backlog de Producto

Para priorizar eficazmente el backlog de un proyecto de desarrollo de software, especialmente en un contexto Agile, se deben considerar múltiples factores que afectan tanto la entrega del proyecto como la satisfacción del usuario final. Aquí se detallan los elementos y consideraciones esenciales para una priorización efectiva:

Factores para la Priorización del Backlog



Valor del Negocio: Identificar las user stories y mejoras que aportan el mayor valor al negocio o a los usuarios finales. Esto incluye impacto en la retención de usuarios, atractivo para nuevos usuarios y potencial de ingresos.

Urgencia: Determinar qué características son más urgentes en términos de necesidades del mercado o compromisos con los stakeholders.

Dependencias: Reconocer y priorizar tareas que otras tareas dependen para su implementación. Esto asegura un flujo de trabajo lógico y eficiente.

Coste de implementación: Considerar el esfuerzo, los recursos y el tiempo necesarios para cada tarea. Priorizar aquellas con la mejor relación costo-beneficio.

Riesgos y obstáculos potenciales: Evaluar los riesgos asociados con cada user story o mejora y su impacto potencial en el proyecto.

Feedback del usuario: Integrar las opiniones y preferencias de los usuarios, especialmente en áreas críticas de la interfaz de usuario y la experiencia del usuario.

Madurez tecnológica: Considerar la madurez y la viabilidad de las soluciones tecnológicas propuestas para cada tarea.

Proceso de priorización



Revisión de Stakeholders: Involucrar a los stakeholders clave en el proceso de priorización para asegurar que el backlog refleje los objetivos del negocio.

Sesiones de Priorización con el Equipo: Utilizar técnicas como Planning Poker o la matriz de Eisenhower para consensuar la importancia de cada tarea.

Iteración y Reevaluación: El backlog debe ser dinámico, reevaluando y ajustando las prioridades regularmente a medida que avanza el proyecto y surgen nuevos datos.
```

```
# Prompt: Senior Product Owner & Agile Strategy Expert

**Contexto:** Actúa como un Product Owner Senior con amplia experiencia en la gestión de productos digitales y metodologías ágiles. Tu objetivo es ayudarme a priorizar mi Backlog de Producto de forma estratégica, asegurando el máximo retorno de inversión (ROI) y la eficiencia operativa.

**Instrucciones de Análisis:**
Para cada ítem o User Story que te proporcione, debes realizar una evaluación basada en los siguientes pilares:

1. **Valor y Urgencia:** Evalúa el impacto en el negocio (retención, ingresos) y la urgencia respecto al mercado o compromisos con stakeholders.
2. **Eficiencia Técnica:** Analiza la relación costo-beneficio. Identifica dependencias críticas (qué tareas bloquean a otras) y la madurez tecnológica de la solución.
3. **Gestión de Riesgos:** Identifica posibles obstáculos técnicos o funcionales y considera el feedback del usuario para mitigar errores de UX.
4. **Viabilidad:** Evalúa si la tarea es "Estimable" y "Pequeña" (siguiendo criterios INVEST).

**Metodología de Salida:**
Presenta los resultados en una tabla organizada con las siguientes columnas:
- **Prioridad (Alta/Media/Baja)**
- **Ítem / User Story**
- **Justificación Estratégica (Valor vs. Costo)**
- **Dependencias/Riesgos detectados**
- **Clasificación MoSCoW (Must have, Should have, Could have, Won't have)**

Genera el backlog a partir de las historias de usuario de @file:UserStories-SLG.md y añadelo como seccion a UserStories-SLG.md
```

#### Resultado:

El meta-prompt (generado por Gemini a partir de buenas prácticas) produjo la sección "Backlog Priorizado del Producto" con tres niveles de detalle: tabla a nivel de épica, tablas granulares por sprint con clasificación MoSCoW, y resumen ejecutivo con dimensiones de riesgo y cuello de botella crítico identificado.

#### Conclusiones:

El enfoque de meta-prompting (usar un LLM para generar el prompt óptimo a partir de buenas prácticas) produce un prompt estructurado con rol explícito (Senior PO), metodología definida (4 pilares: Valor/Urgencia, Eficiencia Técnica, Gestión de Riesgos, Viabilidad INVEST) y formato de salida prescrito.
El resultado es cualitativamente superior: incluye justificación estratégica valor/costo por ítem, clasificación MoSCoW accionable, organización por sprint y un resumen ejecutivo con cuello de botella crítico identificado (US-001 · T4 + T2).
La instrucción de contexto ("actúa como Senior PO") activa en el modelo un marco de razonamiento más rico que va más allá de rellenar columnas.
Limitación: al priorizar la justificación cualitativa, el output no incluye story points numéricos, lo que lo hace menos útil para velocity planning sin combinarlo con el resultado del Prompt 3.1.

## Prompt con mejores resultados

El Prompt 3.2.1 fue más efectivo por tres razones técnicas de ingeniería de prompts:

Asignación de rol experto: La instrucción "Actúa como Product Owner Senior" establece un marco cognitivo que orienta todas las inferencias del modelo hacia razonamiento estratégico de producto, no solo generación de contenido estructurado.

Meta-prompting como técnica de mejora: Usar Gemini para sintetizar buenas prácticas en un prompt optimizado elimina la ambigüedad y maximiza la cobertura de los criterios relevantes para la tarea. El prompt resultante es más denso en contexto útil que el prompt escrito manualmente.

Metodología de salida explícita: Al prescribir los 4 pilares de análisis y el formato exacto de la tabla (incluyendo MoSCoW), el modelo no necesita inferir qué dimensiones son relevantes, lo que reduce la varianza del output y alinea el resultado con estándares de gestión de producto reales.

# Prompt 4:

```
Dado el backlog generado en @UserStories-SLG.md, genera únicamente el ticket de trabajo de 'US-001 – Pipeline visual de candidatos' con criterios de aceptación, estimación en story points y sugiere prioridad. Aterrízalos técnicamente, tal y como se hace en las reuniones de planificación
```

# Prompt 5:

```

```

# Prompt 6:

```

```

# Prompt 7:

```

```

# Prompt 8:

```

```

# Prompt 9:

```

```

# Prompt 10:

```

```

# Prompt 11:

```

```

# Prompt 12:

```

```

# Prompt 13:

```

```

# Prompt 14:

```

```

# Prompt 15:

```

```

# Prompt 16:

```

```

# Prompt 17:

```

```

# Prompt 18:

# Prompt 19:

# Prompt 20:

# Prompt 21:

# Prompt 22:

# Prompt 23:

# Prompt 24:

# Prompt 25:

# Prompt 26:

# Prompt 27:

# Prompt 28:

# Prompt 29:

```

```

```

```
