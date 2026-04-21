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
