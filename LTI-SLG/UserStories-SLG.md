# User Stories – LTI ATS (MVP)

> **Contexto:** Historias generadas a partir del PRD de LTI ATS. Se han seleccionado las 3 historias de mayor impacto para el MVP, cubriendo las funcionalidades clasificadas como **Must** en el análisis MoSCoW.

---

# Historia de Usuario US-001: Gestión visual del pipeline de candidatos

## 1. Formato estándar

**Como** recruiter de la plataforma LTI,
**quiero** gestionar candidatos en un pipeline visual con etapas configurables (screening, entrevista, oferta),
**para** tener visibilidad total del proceso de selección y avanzar candidatos sin fricción operativa.

---

## 2. Descripción

> El recruiter necesita una vista centralizada tipo kanban donde pueda ver todos los candidatos de una vacante agrupados por etapa del proceso, moverlos entre etapas con drag & drop y consultar el historial de actividad de cada candidato en tiempo real. Esta funcionalidad es el núcleo del ATS y habilita el resto de flujos del producto.

---

## 3. Criterios de Aceptación (BDD)

### Escenario 1: Visualización del pipeline

- **Dado que** soy un recruiter autenticado con una vacante activa,
- **Cuando** accedo a la vista de pipeline de esa vacante,
- **Entonces** veo todos los candidatos agrupados por etapa en un tablero kanban con nombre, estado y fecha de última actividad.

### Escenario 2: Movimiento de candidatos entre etapas

- **Dado que** estoy en la vista kanban de una vacante,
- **Cuando** arrastro una tarjeta de candidato a una etapa diferente,
- **Entonces** el sistema actualiza el estado del candidato en tiempo real y registra el cambio en el historial de actividad.

### Escenario 3: Registro automático de actividad

- **Dado que** un candidato ha sido movido a una nueva etapa,
- **Cuando** consulto su perfil,
- **Entonces** veo en el historial de actividad el cambio de etapa con fecha, hora y usuario que realizó la acción.

### Escenario 4: Etapas configurables por vacante

- **Dado que** soy recruiter creando o editando una vacante,
- **Cuando** configuro el pipeline,
- **Entonces** puedo añadir, renombrar o eliminar etapas y guardar la configuración sin necesidad de soporte técnico.

---

## 4. Notas adicionales

- El pipeline debe soportar un mínimo de 3 y un máximo de 10 etapas por vacante.
- Los cambios de estado deben persistir en tiempo real (sin recarga de página).
- El historial de actividad es inmutable: no se puede editar ni eliminar.
- El componente kanban debe ser accesible por teclado (WCAG 2.1 AA).

---

## 5. Tareas

- [ ] Tarea 1: Diseño del componente kanban (UI/UX) con estados y tarjetas de candidato.
- [ ] Tarea 2: API endpoint `PATCH /candidates/:id/stage` para actualizar etapa.
- [ ] Tarea 3: Lógica de registro automático de actividad en base de datos.
- [ ] Tarea 4: Configuración de etapas por vacante (CRUD de stages).
- [ ] Tarea 5: Pruebas unitarias del servicio de actualización de estado.
- [ ] Tarea 6: Pruebas E2E del flujo drag & drop y persistencia.
- [ ] Tarea 7: Accesibilidad del tablero kanban (navegación por teclado).

---

## 6. Historias de Usuario Relacionadas

- **Bloquea a:** US-002 (Comunicación automatizada), US-003 (Scorecards de evaluación).
- **Bloqueada por:** Ninguna.
- **Relacionada con:** Gestión de vacantes, perfil de candidato.

---

## 7. Evaluación INVEST

| Criterio        | Descripción + Justificación breve                                                              | Cumple (X) |
| :-------------- | :--------------------------------------------------------------------------------------------- | :--------: |
| **I**ndependent | No depende de otras historias del MVP para ser desarrollada e implantada de forma autónoma.    |    [X]     |
| **N**egotiable  | El número de etapas, la lógica de drag & drop y los campos de la tarjeta son negociables.      |    [X]     |
| **V**aluable    | Es la funcionalidad central del ATS; sin ella el producto no tiene utilidad para el recruiter. |    [X]     |
| **E**stimable   | El equipo puede estimar el esfuerzo: UI kanban + backend CRUD + historial de actividad.        |    [X]     |
| **S**mall       | Acotada a pipeline visual de una vacante; no incluye IA ni analítica avanzada.                 |    [X]     |
| **T**estable    | Los escenarios BDD cubren visualización, movimiento, historial y configuración.                |    [X]     |

---

---

# Historia de Usuario US-002: Comunicación automatizada con candidatos

## 1. Formato estándar

**Como** recruiter de la plataforma LTI,
**quiero** automatizar el envío de emails a candidatos según las reglas del workflow (avance de etapa, rechazo, confirmación de entrevista),
**para** reducir las tareas manuales de comunicación y mejorar la experiencia del candidato sin incrementar mi carga operativa.

---

## 2. Descripción

> El recruiter pierde una parte significativa de su tiempo enviando emails repetitivos de confirmación, rechazo o actualización de estado. Esta historia permite configurar plantillas de email y reglas de disparo automático asociadas a los cambios de etapa del pipeline, garantizando una comunicación consistente y oportuna con todos los candidatos.

---

## 3. Criterios de Aceptación (BDD)

### Escenario 1: Envío automático al cambiar de etapa

- **Dado que** existe una regla de automatización activa para la etapa "Entrevista",
- **Cuando** un candidato es movido a esa etapa,
- **Entonces** el sistema envía automáticamente el email definido en la plantilla asociada en un máximo de 2 minutos.

### Escenario 2: Creación y edición de plantillas de email

- **Dado que** soy recruiter en la configuración de comunicación,
- **Cuando** creo una nueva plantilla con variables dinámicas (nombre del candidato, nombre de la vacante, fecha),
- **Entonces** puedo guardarla y asignarla a una etapa del pipeline sin errores.

### Escenario 3: Envío manual de email desde el perfil del candidato

- **Dado que** estoy en el perfil de un candidato,
- **Cuando** selecciono "Enviar email" y elijo una plantilla o redacto un mensaje libre,
- **Entonces** el email se envía y queda registrado en el historial de comunicación del candidato.

### Escenario 4: Tracking de comunicaciones

- **Dado que** se ha enviado un email a un candidato (manual o automático),
- **Cuando** consulto el historial de comunicación en su perfil,
- **Entonces** veo el asunto, la fecha de envío, el estado (enviado/fallido) y el canal.

---

## 4. Notas adicionales

- Las variables dinámicas mínimas requeridas: `{{candidate_name}}`, `{{job_title}}`, `{{recruiter_name}}`, `{{company_name}}`.
- El servicio de envío de email debe integrarse con un proveedor externo (p. ej. SendGrid o AWS SES).
- Los emails automáticos fallidos deben generar una alerta visible en la plataforma.
- Compliance: incluir enlace de cancelación de comunicación (GDPR).

---

## 5. Tareas

- [ ] Tarea 1: CRUD de plantillas de email con soporte de variables dinámicas.
- [ ] Tarea 2: Motor de reglas de automatización (trigger: cambio de etapa → envío de plantilla).
- [ ] Tarea 3: Integración con proveedor de email (SendGrid / AWS SES).
- [ ] Tarea 4: Registro de comunicaciones en el perfil del candidato.
- [ ] Tarea 5: Manejo de errores y alertas por envío fallido.
- [ ] Tarea 6: Enlace de opt-out en emails (GDPR).
- [ ] Tarea 7: Pruebas unitarias del motor de reglas.
- [ ] Tarea 8: Pruebas de integración con el proveedor de email (modo sandbox).

---

## 6. Historias de Usuario Relacionadas

- **Bloquea a:** Ninguna.
- **Bloqueada por:** US-001 (Pipeline de candidatos — necesario para disparar triggers por etapa).
- **Relacionada con:** Compliance GDPR, configuración de vacantes.

---

## 7. Evaluación INVEST

| Criterio        | Descripción + Justificación breve                                                                        | Cumple (X) |
| :-------------- | :------------------------------------------------------------------------------------------------------- | :--------: |
| **I**ndependent | Independiente de scorecards y analítica; solo requiere que el pipeline exista como precondición técnica. |    [X]     |
| **N**egotiable  | El proveedor de email, las variables disponibles y el número de plantillas son negociables.              |    [X]     |
| **V**aluable    | Reduce directamente las tareas manuales del recruiter y mejora el candidate response rate (KPI del PRD). |    [X]     |
| **E**stimable   | El equipo puede estimar: CRUD plantillas + motor de reglas + integración proveedor email.                |    [X]     |
| **S**mall       | Acotada a email; no incluye SMS, WhatsApp ni otros canales.                                              |    [X]     |
| **T**estable    | Los escenarios BDD cubren envío automático, manual, plantillas y tracking.                               |    [X]     |

---

---

# Historia de Usuario US-003: Scorecards de evaluación y feedback estructurado

## 1. Formato estándar

**Como** hiring manager de una empresa que usa LTI,
**quiero** completar scorecards de evaluación estructurados tras cada entrevista y consultar el feedback consolidado del equipo,
**para** tomar decisiones de contratación objetivas, alineadas con el equipo y basadas en criterios predefinidos.

---

## 2. Descripción

> Sin un sistema de evaluación estructurado, el feedback de las entrevistas queda disperso en emails, chats o notas personales, lo que dificulta la comparación objetiva de candidatos. Esta historia permite que los hiring managers completen scorecards con criterios predefinidos (competencias, cultura fit, nivel técnico) y que el recruiter vea el feedback consolidado de todo el equipo de forma centralizada en el perfil del candidato.

---

## 3. Criterios de Aceptación (BDD)

### Escenario 1: Completar un scorecard tras una entrevista

- **Dado que** soy hiring manager y acabo de realizar una entrevista registrada en LTI,
- **Cuando** accedo al perfil del candidato y selecciono "Añadir evaluación",
- **Entonces** veo el scorecard de la vacante con los criterios predefinidos y puedo puntuar cada uno (escala 1–5) y añadir comentarios libres.

### Escenario 2: Visibilidad del feedback consolidado

- **Dado que** varios entrevistadores han completado sus scorecards para un mismo candidato,
- **Cuando** el recruiter accede al perfil del candidato,
- **Entonces** ve el resumen consolidado con la puntuación media por criterio y el feedback individual de cada entrevistador.

### Escenario 3: Configuración de criterios del scorecard por vacante

- **Dado que** soy recruiter creando una vacante,
- **Cuando** defino el scorecard de la posición,
- **Entonces** puedo añadir, editar o eliminar criterios de evaluación específicos para esa vacante (mínimo 1, máximo 10 criterios).

### Escenario 4: Restricción de visibilidad del feedback

- **Dado que** un hiring manager ha enviado su evaluación,
- **Cuando** otro entrevistador accede al perfil antes de completar su propio scorecard,
- **Entonces** no puede ver las puntuaciones de otros evaluadores hasta haber enviado la suya (para evitar sesgos de anclaje).

---

## 4. Notas adicionales

- El scorecard debe ser editable por el autor hasta que el recruiter cierre la evaluación de la ronda.
- Los scorecards enviados deben quedar auditados (no se pueden eliminar, solo el recruiter puede anularlos).
- Considerar exportación de evaluaciones a PDF para procesos formales de selección.
- La restricción de visibilidad del Escenario 4 es configurable a nivel de vacante por el recruiter.

---

## 5. Tareas

- [ ] Tarea 1: CRUD de criterios de scorecard por vacante.
- [ ] Tarea 2: UI del formulario de evaluación (puntuación 1–5 + comentario por criterio).
- [ ] Tarea 3: Lógica de consolidación y cálculo de puntuación media por criterio.
- [ ] Tarea 4: Vista de feedback consolidado en el perfil del candidato (recruiter view).
- [ ] Tarea 5: Lógica de restricción de visibilidad antes del envío propio (blind review).
- [ ] Tarea 6: Registro de auditoría de scorecards enviados (inmutable).
- [ ] Tarea 7: Pruebas unitarias de consolidación de puntuaciones.
- [ ] Tarea 8: Pruebas de roles y permisos (hiring manager vs recruiter view).

---

## 6. Historias de Usuario Relacionadas

- **Bloquea a:** Analítica básica del hiring funnel (calidad de evaluación por vacante).
- **Bloqueada por:** US-001 (Pipeline de candidatos — el scorecard está vinculado a etapas de entrevista).
- **Relacionada con:** Gestión de roles y permisos, gestión de vacantes.

---

## 7. Evaluación INVEST

| Criterio        | Descripción + Justificación breve                                                                          | Cumple (X) |
| :-------------- | :--------------------------------------------------------------------------------------------------------- | :--------: |
| **I**ndependent | Independiente de la automatización de comunicación; solo requiere el pipeline como base estructural.       |    [X]     |
| **N**egotiable  | La escala de puntuación, el número de criterios y la lógica de blind review son negociables con el equipo. |    [X]     |
| **V**aluable    | Mejora directamente la calidad de contratación y alinea al equipo, uno de los KPIs estratégicos del PRD.   |    [X]     |
| **E**stimable   | El equipo puede estimar: CRUD criterios + formulario UI + consolidación + permisos.                        |    [X]     |
| **S**mall       | Acotada a scorecards de entrevista; no incluye IA de análisis de evaluaciones ni comparador automático.    |    [X]     |
| **T**estable    | Los escenarios BDD cubren creación, visibilidad consolidada, configuración y restricción de blind review.  |    [X]     |

---

---

# Priorización para el MVP

## Orden recomendado

| Prioridad | Historia                                         | Sprint sugerido |
| :-------: | :----------------------------------------------- | :-------------: |
|     1     | US-001: Pipeline visual de candidatos            |    Sprint 1     |
|     2     | US-002: Comunicación automatizada con candidatos |    Sprint 2     |
|     3     | US-003: Scorecards de evaluación y feedback      |   Sprint 2–3    |

---

## Justificación de la priorización

### 1. US-001 primero — El pipeline es el núcleo del producto

El pipeline visual es la funcionalidad vertebral del ATS: sin él, ninguna otra historia tiene sentido técnico ni de negocio. Bloquea directamente a US-002 y US-003, ya que ambas dependen de que existan etapas y candidatos gestionados. Además, es la historia que más rápido valida el producto con usuarios reales (demo-able en sprint 1). Desde la perspectiva del PRD, es el criterio de éxito del MVP más crítico: _"Creación y gestión completa de pipeline sin fricción crítica"_.

### 2. US-002 segundo — Máximo impacto en eficiencia operativa

La comunicación automatizada ataca directamente el problema core del PRD (_"Pérdida de candidatos por falta de seguimiento"_) y el KPI de _"Automatización de al menos 40% de tareas manuales de recruiting"_. Su impacto es inmediatamente percibible por los recruiters desde el primer uso, lo que acelera la adopción activa (KPI: ≥70% usuarios mensuales). Tiene menor complejidad técnica que los scorecards y puede desplegarse en paralelo con el refinamiento del pipeline.

### 3. US-003 tercero — Diferenciador de calidad, pero dependiente de adopción previa

Los scorecards aportan el diferenciador de _calidad de contratación_ frente a la competencia (Greenhouse-style structured hiring), pero su valor solo se materializa cuando el equipo ya usa el pipeline y la comunicación activamente. Técnicamente depende del pipeline (US-001) y su desarrollo es más complejo por la lógica de roles, permisos y blind review. Puede iniciarse en Sprint 2 en paralelo con el cierre de US-002.

---

## Criterios de priorización aplicados

| Criterio                            | US-001  | US-002 | US-003 |
| :---------------------------------- | :-----: | :----: | :----: |
| Valor para el usuario final         |  Alto   |  Alto  | Medio  |
| Dependencias técnicas (bloqueos)    |  Base   | Media  |  Alta  |
| Impacto en KPIs del PRD             |  Alto   |  Alto  | Medio  |
| Complejidad de implementación       |  Media  | Media  |  Alta  |
| Riesgo de adopción si no se entrega | Crítico |  Alto  | Medio  |

---

---

# Backlog Priorizado del Producto – LTI ATS (MVP)

> **Metodología aplicada:** Análisis estratégico por los cuatro pilares (Valor/Urgencia, Eficiencia Técnica, Gestión de Riesgos, Viabilidad INVEST) sobre las User Stories y sus tareas constituyentes. Las tareas de testing y accesibilidad se clasifican como ítems independientes del backlog para permitir su planificación granular en sprint.

---

## Backlog por Historia de Usuario (nivel épica)

| Prioridad | Ítem / User Story                          | Justificación Estratégica (Valor vs. Costo)                                                                                                                                                                                                                   | Dependencias / Riesgos detectados                                                                                                                                 |     MoSCoW      |
| :-------: | :----------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------: |
| **Alta**  | **US-001 – Pipeline visual de candidatos** | Valor máximo: es la funcionalidad vertebral del ATS. Sin ella ninguna otra historia tiene sentido técnico ni de negocio. Costo medio (kanban UI + backend CRUD + historial). ROI crítico: habilita demo en Sprint 1 y valida el producto con usuarios reales. | Bloquea a US-002 y US-003. Riesgo: drag & drop accesible (WCAG 2.1 AA) y etapas configurables añaden complejidad al modelo de datos.                              |  **Must have**  |
| **Alta**  | **US-002 – Comunicación automatizada**     | Valor alto: ataca directamente el problema core del PRD ("pérdida de candidatos por falta de seguimiento") y el KPI de automatización ≥40%. Costo medio. Percepción inmediata por el recruiter desde el primer uso → acelera adopción.                        | Bloqueada por US-001. Riesgo: integración con proveedor externo (SLA, coste por email), compliance GDPR, gestión de errores de envío.                             |  **Must have**  |
| **Media** | **US-003 – Scorecards de evaluación**      | Valor medio-alto: diferenciador estratégico frente a competidores (structured hiring), pero su valor solo se materializa con adopción previa de US-001 y US-002. Costo alto por lógica de roles, permisos y blind review.                                     | Bloqueada por US-001. Riesgo: complejidad de la restricción de visibilidad (blind review), auditoría inmutable, gestión de permisos hiring manager vs. recruiter. | **Should have** |

---

## Backlog granular por tareas

### Sprint 1 — US-001: Pipeline visual de candidatos

| Prioridad | Ítem / Tarea                                          | Justificación Estratégica (Valor vs. Costo)                                                                                                       | Dependencias / Riesgos detectados                                                                                                                   |     MoSCoW      |
| :-------: | :---------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------: |
| **Alta**  | US-001 · T2 – API `PATCH /candidates/:id/stage`       | Fundamento técnico del pipeline. Sin este endpoint no existe persistencia de movimientos. Costo bajo, impacto máximo.                             | Ninguna. Base para T3 (historial) y para los triggers de US-002.                                                                                    |  **Must have**  |
| **Alta**  | US-001 · T4 – CRUD de etapas por vacante              | El modelo de datos de stages debe estar definido antes de cualquier otro desarrollo del pipeline. Costo bajo, desbloquea toda la historia.        | Base para T1 (UI) y T2 (API). Riesgo: definir correctamente el rango 3–10 etapas en el schema.                                                      |  **Must have**  |
| **Alta**  | US-001 · T1 – Componente kanban (UI/UX)               | Funcionalidad visible de mayor impacto para el stakeholder. Sin la UI no hay demo ni validación de producto. Costo medio (drag & drop + estados). | Depende de T4 (modelo de stages) y T2 (API). Riesgo: complejidad del drag & drop en bibliotecas accesibles.                                         |  **Must have**  |
| **Alta**  | US-001 · T3 – Registro automático de actividad        | Requisito de negocio explícito: historial inmutable con fecha, hora y usuario. Costo bajo, alto valor auditor.                                    | Depende de T2 (API de cambio de etapa). Riesgo: asegurar inmutabilidad en el modelo de datos.                                                       |  **Must have**  |
| **Media** | US-001 · T5 – Pruebas unitarias del servicio de etapa | Garantiza la fiabilidad del core antes de avanzar a US-002 y US-003. Costo bajo.                                                                  | Depende de T2 y T3 estar implementadas.                                                                                                             | **Should have** |
| **Media** | US-001 · T6 – Pruebas E2E del flujo drag & drop       | Valida la persistencia end-to-end y el comportamiento del pipeline completo. Costo medio.                                                         | Depende de T1, T2 y T3 estar implementadas. Riesgo: entorno de tests E2E (Playwright/Cypress) debe estar configurado.                               | **Should have** |
| **Baja**  | US-001 · T7 – Accesibilidad WCAG 2.1 AA del kanban    | Requisito de compliance importante, pero paralelizable o abordable en iteración posterior sin bloquear el MVP funcional. Costo alto relativo.     | Depende de T1 (componente kanban finalizado). Riesgo: bibliotecas de drag & drop con soporte de teclado limitado pueden requerir soluciones custom. | **Could have**  |

### Sprint 2 — US-002: Comunicación automatizada con candidatos

| Prioridad | Ítem / Tarea                                                            | Justificación Estratégica (Valor vs. Costo)                                                                               | Dependencias / Riesgos detectados                                                                                                    |     MoSCoW      |
| :-------: | :---------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------- | :-------------: |
| **Alta**  | US-002 · T1 – CRUD de plantillas de email                               | Núcleo de la historia: sin plantillas no hay automatización. Costo bajo, impacto directo en la experiencia del recruiter. | Ninguna dependencia técnica previa dentro de US-002.                                                                                 |  **Must have**  |
| **Alta**  | US-002 · T3 – Integración con proveedor de email (SendGrid / AWS SES)   | Sin el servicio de envío configurado ninguna automatización puede ejecutarse. Costo medio. Desbloquea T2 y T5.            | Depende de T1. Riesgo: elección del proveedor (coste, SLA, límites sandbox); requiere credenciales y configuración de dominio.       |  **Must have**  |
| **Alta**  | US-002 · T6 – Enlace de opt-out GDPR                                    | Requisito legal no negociable en mercado europeo. Costo bajo, riesgo regulatorio alto si se omite.                        | Depende de T3 (integración email). Riesgo: incumplimiento del RGPD puede implicar sanciones.                                         |  **Must have**  |
| **Alta**  | US-002 · T2 – Motor de reglas de automatización (trigger etapa → email) | Componente central de la historia: conecta el evento del pipeline (US-001) con el envío de email. Costo medio-alto.       | Depende de T1 (plantillas) y T3 (integración email). Bloquea T7 y T8. Riesgo: latencia del envío (SLA ≤2 min), manejo de reintentos. |  **Must have**  |
| **Media** | US-002 · T4 – Registro de comunicaciones en perfil del candidato        | Aumenta la trazabilidad y la confianza del recruiter en el sistema. Costo bajo.                                           | Depende de T3 y T2.                                                                                                                  | **Should have** |
| **Media** | US-002 · T5 – Manejo de errores y alertas por envío fallido             | Incrementa la fiabilidad operativa; sin él, los fallos silenciosos dañan la experiencia. Costo bajo.                      | Depende de T3 y T2. Riesgo: definir el canal de alerta (notificación en plataforma vs. email al recruiter).                          | **Should have** |
| **Media** | US-002 · T7 – Pruebas unitarias del motor de reglas                     | Fiabilidad del core de automatización antes de puesta en producción. Costo bajo.                                          | Depende de T2.                                                                                                                       | **Should have** |
| **Media** | US-002 · T8 – Pruebas de integración con proveedor (sandbox)            | Valida el contrato de integración externo sin coste de envíos reales. Costo bajo.                                         | Depende de T3. Riesgo: disponibilidad del entorno sandbox del proveedor elegido.                                                     | **Should have** |

### Sprint 2–3 — US-003: Scorecards de evaluación y feedback estructurado

| Prioridad | Ítem / Tarea                                                                            | Justificación Estratégica (Valor vs. Costo)                                                                                               | Dependencias / Riesgos detectados                                                                                           |     MoSCoW      |
| :-------: | :-------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------- | :-------------: |
| **Alta**  | US-003 · T1 – CRUD de criterios de scorecard por vacante                                | Base del modelo de datos de evaluación. Sin criterios definidos no puede existir ningún formulario ni consolidación. Costo bajo.          | Bloquea a T2, T3, T4 y T6. Riesgo: validar el rango 1–10 criterios y su persistencia por vacante.                           |  **Must have**  |
| **Alta**  | US-003 · T6 – Registro de auditoría de scorecards (inmutable)                           | Requisito de compliance y confianza: los scorecards enviados no se pueden eliminar. Costo bajo, valor jurídico alto.                      | Depende de T2 (formulario de envío). Riesgo: garantizar inmutabilidad a nivel de base de datos (no solo a nivel de API).    |  **Must have**  |
| **Alta**  | US-003 · T8 – Pruebas de roles y permisos (hiring manager vs. recruiter)                | La lógica de permisos es el riesgo técnico mayor de esta historia. Fallar aquí comprometería la integridad del blind review. Costo medio. | Depende de T5. Riesgo: casos límite con múltiples entrevistadores sobre el mismo candidato.                                 |  **Must have**  |
| **Alta**  | US-003 · T2 – UI del formulario de evaluación (puntuación 1–5 + comentario)             | Interfaz principal del hiring manager; sin ella la historia no tiene entregable visible. Costo medio.                                     | Depende de T1 (criterios disponibles). Riesgo: UX del formulario debe evitar fricciones que reduzcan la tasa de completado. |  **Must have**  |
| **Alta**  | US-003 · T3 – Lógica de consolidación y puntuación media                                | Transforma datos individuales en inteligencia de hiring. Sin ella, la vista de recruiter no aporta valor. Costo medio.                    | Depende de T2 (scorecards enviados). Riesgo: cálculo de media con criterios heterogéneos entre evaluadores.                 |  **Must have**  |
| **Media** | US-003 · T4 – Vista de feedback consolidado (recruiter view)                            | Entregable final visible para el recruiter: resume el trabajo de todo el equipo. Costo bajo (consuming T3).                               | Depende de T3.                                                                                                              | **Should have** |
| **Media** | US-003 · T7 – Pruebas unitarias de consolidación de puntuaciones                        | Garantiza la exactitud matemática del cálculo de medias. Costo bajo.                                                                      | Depende de T3.                                                                                                              | **Should have** |
| **Baja**  | US-003 · T5 – Lógica de blind review (restricción de visibilidad antes de envío propio) | Funcionalidad configurable que aporta un diferenciador de calidad de proceso, pero añade complejidad significativa. Costo alto.           | Depende de T2 y T8 (roles). Riesgo: gestión de estado de "has_submitted" por entrevistador puede generar race conditions.   | **Could have**  |

---

## Resumen ejecutivo del backlog

| Dimensión                     | Detalle                                                                                                                        |
| :---------------------------- | :----------------------------------------------------------------------------------------------------------------------------- |
| **Total de ítems en backlog** | 22 tareas distribuidas en 3 épicas (US-001: 7, US-002: 8, US-003: 7)                                                           |
| **Must have**                 | 13 ítems — entrega el MVP funcional y legalmente válido                                                                        |
| **Should have**               | 7 ítems — mejoran la fiabilidad y la observabilidad del sistema                                                                |
| **Could have**                | 2 ítems — diferenciales de calidad (accesibilidad WCAG, blind review) aplazables post-MVP                                      |
| **Won't have (MVP)**          | Exportación PDF de scorecards, canales alternativos (SMS/WhatsApp), IA de análisis de evaluaciones                             |
| **Riesgo principal**          | Integración con proveedor externo de email (US-002 · T3): única dependencia de terceros con impacto en SLA y costes operativos |
| **Cuello de botella crítico** | US-001 · T4 + T2 (modelo de stages + API): toda la cadena de valor del MVP depende de estos dos ítems                          |

---

---

# Ticket de Trabajo – US-001: Pipeline visual de candidatos

> **Contexto de planificación:** Ticket técnico generado para la sesión de Sprint Planning del Sprint 1. Refleja el desglose de tareas acordado en refinamiento, con criterios de aceptación técnicos, estimaciones en Story Points (escala Fibonacci) y dependencias secuenciadas para el equipo de desarrollo.

---

## Metadatos del ticket

| Campo                    | Valor                   |
| :----------------------- | :---------------------- |
| **ID**                   | US-001                  |
| **Épica**                | Pipeline de candidatos  |
| **Sprint**               | Sprint 1                |
| **Prioridad**            | 🔴 Alta — Must have     |
| **Story Points totales** | **21 SP**               |
| **Assignee**             | Por asignar en planning |
| **Bloquea a**            | US-002, US-003          |
| **Bloqueada por**        | —                       |
| **Estado**               | To Do                   |

---

## Historia de usuario

**Como** recruiter de la plataforma LTI,
**quiero** gestionar candidatos en un pipeline visual con etapas configurables,
**para** tener visibilidad total del proceso de selección y avanzar candidatos sin fricción operativa.

---

## Criterios de aceptación técnicos (DoD — Definition of Done)

Un ítem de este ticket se considera **Done** cuando cumple **todos** los criterios siguientes:

### Funcionales

- [ ] El tablero kanban muestra columnas dinámicas generadas desde la configuración de etapas de la vacante (mín. 3 / máx. 10 columnas).
- [ ] Cada tarjeta de candidato expone: nombre completo, avatar/iniciales, etapa actual y timestamp de última actividad (formato `DD/MM/YYYY HH:mm`).
- [ ] El drag & drop entre columnas llama a `PATCH /candidates/:id/stage` y actualiza la UI de forma optimista (sin recarga de página).
- [ ] Si la llamada a la API falla, la tarjeta revierte a la columna original y se muestra un toast de error.
- [ ] Cada cambio de etapa genera un registro de actividad inmutable en base de datos con: `candidateId`, `fromStage`, `toStage`, `changedBy` (userId), `changedAt` (UTC timestamp).
- [ ] El CRUD de etapas permite crear, renombrar y eliminar stages; eliminar una etapa con candidatos asignados mueve éstos a la primera etapa disponible (no los borra).
- [ ] La configuración de etapas se guarda por `vacancyId`; cambiar las etapas de una vacante no afecta a otras vacantes.

### Técnicos (backend)

- [ ] Endpoint `PATCH /candidates/:id/stage` responde `200 OK` con el candidato actualizado o `404` si no existe, `422` si `stageId` no pertenece a la vacante del candidato.
- [ ] Endpoint `GET /vacancies/:id/pipeline` devuelve candidatos agrupados por `stageId` en una única consulta (sin N+1 queries — usar `include` de Prisma o `groupBy`).
- [ ] Los endpoints están protegidos por middleware de autenticación JWT existente.
- [ ] Todos los cambios de etapa pasan por el servicio de dominio `CandidateStageService` (DDD); la capa de presentación no escribe directamente en el repositorio.
- [ ] Los campos nuevos en el schema de Prisma tienen migración versionada (`prisma migrate dev`).
- [ ] Cobertura de tests unitarios del `CandidateStageService` ≥ 80%.

### Técnicos (frontend)

- [ ] El componente `KanbanBoard` está implementado con `react-beautiful-dnd` y es completamente tipado en TypeScript.
- [ ] El estado del board se gestiona con `useState` local o `useReducer`; no se usa estado global para este componente en el MVP.
- [ ] Las llamadas a la API pasan por la capa `services/candidateService.ts` (no se llama a Axios directamente desde el componente).
- [ ] El componente `KanbanBoard` tiene tests de componente con React Testing Library que cubren: render con datos, drag & drop simulado y rollback por error de API.

### Calidad

- [ ] Sin errores de TypeScript (`tsc --noEmit` pasa en CI).
- [ ] Sin errores de ESLint.
- [ ] Código revisado por al menos 1 peer (PR aprobada).

---

## Desglose de tareas y estimación

### T4 – CRUD de etapas por vacante (modelo de datos y API)

**Story Points: 3**
**Prioridad de implementación: 1ª (desbloquea todo lo demás)**

**Descripción técnica:**
Definir el modelo `Stage` en el schema de Prisma y exponer endpoints REST para su gestión.

**Schema Prisma a añadir:**

```prisma
model Stage {
  id          Int           @id @default(autoincrement())
  name        String        @db.VarChar(100)
  order       Int
  vacancyId   Int
  vacancy     Vacancy       @relation(fields: [vacancyId], references: [id])
  candidates  Application[] @relation("ApplicationStage")
  createdAt   DateTime      @default(now())
  updatedAt   DateTime      @updatedAt

  @@unique([vacancyId, order])
}
```

**Endpoints a implementar:**

| Método   | Ruta                    | Descripción                        | Respuesta               |
| :------- | :---------------------- | :--------------------------------- | :---------------------- |
| `GET`    | `/vacancies/:id/stages` | Lista etapas ordenadas por `order` | `200` array de stages   |
| `POST`   | `/vacancies/:id/stages` | Crea nueva etapa                   | `201` stage creado      |
| `PATCH`  | `/stages/:id`           | Renombra una etapa                 | `200` stage actualizado |
| `DELETE` | `/stages/:id`           | Elimina etapa; reasigna candidatos | `204`                   |

**Criterios de aceptación de la tarea:**

- [ ] `DELETE /stages/:id` devuelve `409 Conflict` si es la única etapa de la vacante (mínimo 1 stage requerido para reasignación).
- [ ] La constraint `@@unique([vacancyId, order])` se aplica en base de datos.
- [ ] Migración de Prisma generada y versionada en `/prisma/migrations/`.

---

### T2 – API endpoint `PATCH /candidates/:id/stage`

**Story Points: 3**
**Prioridad de implementación: 2ª (depende de T4)**

**Descripción técnica:**
Implementar el endpoint de cambio de etapa dentro de la capa DDD existente, respetando la arquitectura en capas del proyecto.

**Capas a crear/modificar:**

```
domain/
  models/CandidateStage.ts          ← Value Object nuevo
  services/CandidateStageService.ts ← Servicio de dominio nuevo
infrastructure/
  repositories/StageRepository.ts   ← Repositorio nuevo (interfaz + implementación Prisma)
presentation/
  controllers/candidateController.ts ← Añadir handler patchStage()
routes/
  candidates.ts                      ← Añadir ruta PATCH /:id/stage
```

**Contrato del endpoint:**

```
PATCH /candidates/:id/stage
Authorization: Bearer <token>
Content-Type: application/json

Body:
{
  "stageId": number   // ID de la etapa destino (requerido)
}

Respuestas:
200 OK     → { id, firstName, lastName, currentStageId, updatedAt }
400        → { error: "stageId is required" }
404        → { error: "Candidate not found" }
422        → { error: "Stage does not belong to this candidate's vacancy" }
```

**Criterios de aceptación de la tarea:**

- [ ] El servicio `CandidateStageService.changeStage()` valida que `stageId` pertenece a la misma vacante que el candidato antes de persistir.
- [ ] El cambio de etapa y la creación del registro de actividad ocurren en la misma transacción de Prisma (`prisma.$transaction`).
- [ ] Tests unitarios del servicio con mocks del repositorio (escenarios: éxito, stage inválido, candidato no encontrado).

---

### T3 – Registro automático de actividad

**Story Points: 2**
**Prioridad de implementación: 3ª (incluida en transacción de T2)**

**Descripción técnica:**
Modelo `ActivityLog` y su escritura dentro de la transacción de `CandidateStageService`.

**Schema Prisma a añadir:**

```prisma
model ActivityLog {
  id          Int      @id @default(autoincrement())
  candidateId Int
  candidate   Candidate @relation(fields: [candidateId], references: [id])
  fromStageId Int?
  toStageId   Int
  changedBy   Int      // Employee.id
  changedAt   DateTime @default(now())

  // Sin updatedAt ni campos mutables: inmutabilidad garantizada a nivel de schema
}
```

**Endpoint de lectura a implementar:**

```
GET /candidates/:id/activity
Authorization: Bearer <token>

Respuesta 200:
[
  {
    "id": 1,
    "fromStage": "Screening",
    "toStage": "Entrevista",
    "changedBy": "Ana García",
    "changedAt": "2026-04-22T10:30:00Z"
  }
]
```

**Criterios de aceptación de la tarea:**

- [ ] No existe endpoint `PATCH` ni `DELETE` sobre `ActivityLog` (inmutabilidad forzada a nivel de API).
- [ ] `fromStageId` es `null` para el primer registro de un candidato (entrada al pipeline).
- [ ] Los registros se devuelven ordenados por `changedAt` DESC.

---

### T1 – Componente kanban (UI/UX)

**Story Points: 8**
**Prioridad de implementación: 4ª (depende de T4 y T2)**

**Descripción técnica:**
Componente React con `react-beautiful-dnd` (ya instalado: `react-beautiful-dnd 13.1.1`) y MUI como sistema de diseño.

**Estructura de componentes:**

```
components/
  KanbanBoard/
    KanbanBoard.tsx          ← Componente raíz; gestiona DnD context y estado
    KanbanColumn.tsx         ← Columna individual (Droppable)
    CandidateCard.tsx        ← Tarjeta de candidato (Draggable)
    KanbanBoard.types.ts     ← Tipos e interfaces TypeScript
    KanbanBoard.test.tsx     ← Tests de componente
```

**Tipos clave:**

```typescript
interface Stage {
  id: number;
  name: string;
  order: number;
}

interface CandidateCard {
  id: number;
  firstName: string;
  lastName: string;
  currentStageId: number;
  lastActivityAt: string; // ISO 8601
}

interface KanbanBoardProps {
  vacancyId: number;
  stages: Stage[];
  candidates: CandidateCard[];
  onStageChange: (candidateId: number, newStageId: number) => Promise<void>;
}
```

**Lógica de actualización optimista:**

```typescript
// 1. Actualizar estado local inmediatamente (UX sin latencia)
// 2. Llamar a onStageChange (que internamente llama a PATCH /candidates/:id/stage)
// 3. Si la promesa rechaza → revertir el estado local al valor anterior + mostrar toast error
```

**Criterios de aceptación de la tarea:**

- [ ] `KanbanBoard` recibe `stages` y `candidates` como props; no hace fetch directamente (separation of concerns).
- [ ] El componente padre (`PipelinePage`) orquesta el fetch de datos y pasa las props.
- [ ] Drag & drop entre columnas dispara `onStageChange`; si falla, el estado revierte visualmente.
- [ ] Tests cubren: render del board con N columnas, simulación de drag & drop exitoso, simulación de error de API con rollback.

---

### T5 – Pruebas unitarias del servicio de actualización de etapa

**Story Points: 2**
**Prioridad de implementación: 5ª (paralela a T1, tras T2)**

**Descripción técnica:**
Suite de tests Jest para `CandidateStageService` con mocks de repositorios.

**Casos de test requeridos:**

| Caso                                 | Descripción                                       | Resultado esperado                                          |
| :----------------------------------- | :------------------------------------------------ | :---------------------------------------------------------- |
| `changeStage - success`              | Stage válido, candidato existe, vacante coincide  | Retorna candidato actualizado; `ActivityLog` creado         |
| `changeStage - invalid stage`        | `stageId` no pertenece a la vacante del candidato | Lanza `ValidationError` con código `422`                    |
| `changeStage - candidate not found`  | `candidateId` inexistente                         | Lanza `NotFoundError` con código `404`                      |
| `changeStage - same stage`           | `stageId` igual al stage actual                   | Retorna candidato sin crear registro de actividad duplicado |
| `changeStage - transaction rollback` | El repositorio de `ActivityLog` falla al escribir | Revierte el cambio de etapa; ningún cambio persiste         |

**Criterios de aceptación de la tarea:**

- [ ] Cobertura de líneas ≥ 80% en `CandidateStageService.ts`.
- [ ] Todos los tests usan mocks (`jest.fn()`) de los repositorios; ningún test toca base de datos real.
- [ ] Tests nombrados en inglés siguiendo el patrón `describe('CandidateStageService') > it('should ...')`.

---

### T6 – Pruebas E2E del flujo drag & drop

**Story Points: 2**
**Prioridad de implementación: 6ª (tras T1, T2 y T3 integradas)**

**Descripción técnica:**
Suite Cypress sobre el flujo completo del pipeline.

**Casos de test E2E requeridos:**

| Caso                       | Pasos                                                 | Aserción                                                             |
| :------------------------- | :---------------------------------------------------- | :------------------------------------------------------------------- |
| Visualización del pipeline | Login → navegar a vacante activa                      | Columnas visibles con candidatos correctos por etapa                 |
| Drag & drop exitoso        | Arrastrar candidato de col. A a col. B                | Tarjeta aparece en col. B; historial del candidato refleja el cambio |
| Persistencia tras recarga  | Mover candidato → recargar página                     | Candidato sigue en la nueva etapa                                    |
| Rollback por error de red  | Interceptar `PATCH` con `cy.intercept` → forzar `500` | Tarjeta revierte a columna original; toast de error visible          |

**Criterios de aceptación de la tarea:**

- [ ] Tests en `cypress/e2e/pipeline.cy.ts`.
- [ ] Usan `cy.intercept` para controlar respuestas de la API (no dependen de datos de producción).
- [ ] Pasan en CI (entorno headless `cypress run`).

---

### T7 – Accesibilidad WCAG 2.1 AA del kanban

**Story Points: 1**
**Prioridad de implementación: 7ª (post-MVP si hay restricción de tiempo)**

**Descripción técnica:**
`react-beautiful-dnd` soporta navegación por teclado nativamente. Esta tarea verifica y completa el soporte.

**Checklist técnico:**

- [ ] Drag & drop operable completamente por teclado: `Tab` para foco, `Space` para seleccionar, flechas para mover, `Enter` para soltar, `Escape` para cancelar.
- [ ] Cada `CandidateCard` tiene `aria-label` descriptivo: `"Candidato [nombre], etapa [nombre etapa], última actividad [fecha]"`.
- [ ] Cada columna tiene `role="list"` y `aria-label` con el nombre de la etapa y el recuento de candidatos.
- [ ] Contraste de colores de las tarjetas ≥ 4.5:1 (WCAG AA) verificado con herramienta axe.
- [ ] Audit de axe-core integrado en el test de componente `KanbanBoard.test.tsx` con `jest-axe`.

---

## Secuencia de implementación recomendada

```
Sprint 1 — Semana 1
┌─────────────────────────────────────────────────────────────┐
│  Día 1–2  │  T4: Schema Prisma + CRUD stages (3 SP)         │
│  Día 2–3  │  T2: API PATCH /candidates/:id/stage (3 SP)     │  ← incluye T3 (2 SP)
│  Día 3    │  T3: ActivityLog (dentro de transacción de T2)  │
│  Día 4–5  │  T1: KanbanBoard UI + tests componente (8 SP)   │
└─────────────────────────────────────────────────────────────┘

Sprint 1 — Semana 2
┌─────────────────────────────────────────────────────────────┐
│  Día 1–2  │  T5: Tests unitarios CandidateStageService (2 SP)│
│  Día 2–3  │  T6: Tests E2E Cypress (2 SP)                   │
│  Día 4    │  T7: Accesibilidad (1 SP) — si queda capacidad  │
│  Día 5    │  Buffer: revisiones, PR, merge a main            │
└─────────────────────────────────────────────────────────────┘
```

---

## Estimación total y distribución de carga

| Tarea                           |   SP   | Perfil recomendado | Sprint                      |
| :------------------------------ | :----: | :----------------- | :-------------------------- |
| T4 – CRUD stages (schema + API) |   3    | Backend            | Sprint 1                    |
| T2 – API PATCH stage            |   3    | Backend            | Sprint 1                    |
| T3 – ActivityLog (transacción)  |   2    | Backend            | Sprint 1                    |
| T1 – KanbanBoard UI             |   8    | Frontend           | Sprint 1                    |
| T5 – Tests unitarios servicio   |   2    | Backend / QA       | Sprint 1                    |
| T6 – Tests E2E Cypress          |   2    | QA / Frontend      | Sprint 1                    |
| T7 – Accesibilidad WCAG         |   1    | Frontend           | Sprint 1 (si hay capacidad) |
| **Total**                       | **21** |                    |                             |

> **Velocidad de referencia:** Asumiendo un equipo de 2 desarrolladores (1 backend + 1 frontend) con velocidad histórica de ~20–24 SP/sprint, esta historia encaja en un sprint completo con margen para T7.

---

## Riesgos y mitigaciones

| Riesgo                                                                                    | Probabilidad | Impacto | Mitigación                                                                                                                   |
| :---------------------------------------------------------------------------------------- | :----------: | :-----: | :--------------------------------------------------------------------------------------------------------------------------- |
| `react-beautiful-dnd` en modo estricto de React 18 puede generar warnings de `StrictMode` |    Media     |  Bajo   | Envolver `DragDropContext` fuera del `StrictMode` o usar `@hello-pangea/dnd` (fork activo) si hay problemas en CI            |
| Transacción Prisma entre dos writes puede generar deadlock bajo carga concurrente         |     Baja     |  Alto   | Usar `prisma.$transaction` con timeout explícito; añadir índice en `(candidateId, changedAt)` en `ActivityLog`               |
| El modelo de datos de `Vacancy` puede no existir aún en el schema actual                  |    Media     |  Alto   | **Verificar schema Prisma existente en la primera tarea de T4**; si `Vacancy` no existe, crear spike de 1 día para definirlo |
| Capacidad del sprint insuficiente para T7 (accesibilidad)                                 |    Media     |  Bajo   | T7 clasificado como Could have; se mueve al backlog del siguiente sprint sin riesgo para el MVP                              |
