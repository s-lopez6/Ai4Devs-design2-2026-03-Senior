# Historia de Usuario: Búsqueda inteligente de cursos con sugerencias

## 1. Formato estándar

**Como** estudiante de la plataforma de e-learning,
**quiero** buscar cursos por palabra clave y recibir sugerencias relevantes mientras escribo,
**para** que pueda encontrar rápidamente el curso que necesito sin navegar por todo el catálogo.

---

## 2. Descripción

> El usuario necesita un sistema de búsqueda ágil y predictivo que reduzca la fricción al momento de localizar contenido específico dentro de la plataforma.

---

## 3. Criterios de Aceptación

_Condiciones específicas para considerar la historia como "terminada" (Formato **BDD**):_

### Escenario 1: Sugerencias en tiempo real

- **Dado que** estoy en la página principal,
- **Cuando** escribo al menos 3 caracteres en el buscador,
- **Entonces** aparecen sugerencias de cursos en tiempo real (máximo 500ms).

### Escenario 2: Navegación desde sugerencias

- **Dado que** veo los resultados de búsqueda,
- **Cuando** hago clic en un curso,
- **Entonces** navego a la página de detalle del curso.

### Escenario 3: Búsqueda sin coincidencias

- **Dado que** busco un término que no coincide con ningún curso,
- **Cuando** se muestran los resultados,
- **Entonces** veo un mensaje de "Sin resultados" con cursos sugeridos por categoría.

---

## 4. Historias de Usuario Relacionadas

- **Bloquea a:** Ninguna.
- **Bloqueada por:** US-052: Página de detalle del curso.
- **Relacionada con:** US-045: Filtrado de cursos por categoría.

---

## 5. Evaluación INVEST

_Marca con una 'X' si la historia cumple con el criterio de calidad:_

| Criterio        | Descripción                                             | Cumple (X) |
| :-------------- | :------------------------------------------------------ | :--------: |
| **I**ndependent | ¿Es independiente de otras historias?                   |    [X]     |
| **N**egotiable  | ¿Es flexible y abierta a discusión?                     |    [X]     |
| **V**aluable    | ¿Aporta valor claro al cliente o usuario?               |    [X]     |
| **E**stimable   | ¿El equipo tiene información suficiente para estimarla? |    [X]     |
| **S**mall       | ¿Es lo suficientemente pequeña (cabe en un Sprint)?     |    [X]     |
| **T**estable    | ¿Existen criterios claros para ser probada?             |    [X]     |

---

## 6. Notas adicionales

- El buscador debe soportar búsqueda por título, descripción e instructor.
- Considerar integración con algún servicio de búsqueda como Algolia o ElasticSearch.
- **Accesibilidad:** el componente debe ser navegable con teclado.

---

## 7. Tareas

- [ ] Configurar el motor de búsqueda (Algolia/ElasticSearch).
- [ ] Implementar el componente de UI (Search bar) con dropdown de sugerencias.
- [ ] Lógica de Debounce para la llamada a la API (500ms).
- [ ] Manejo de estado para "Sin resultados".
- [ ] Pruebas unitarias y de accesibilidad (Aria-labels).
