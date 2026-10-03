# SIGHOR

## ¿Por qué?

La planificación docente de CELDA sabe qué se enseña y cuándo, pero no sabe en qué aula ni a qué hora. Los horarios académicos - qué grupo tiene qué asignatura, en qué franja y en qué espacio - viven en sistemas propietarios, hojas de cálculo o en el LMS, desconectados del resto del ecosistema.

Esa desconexión tiene consecuencias concretas: CARGA no puede detectar solapamientos de evaluación sin saber cuándo son realmente los exámenes, ASISTE no puede vincular un registro de presencia a un aula sin saber qué grupo estaba ahí, y ningún alumno tiene una fuente única y fiable para consultar su horario.

SIGHOR es esa fuente: el gestor de horarios académicos del ecosistema.

## ¿Qué?

Un sistema de gestión de horarios que asigna aulas, franjas horarias y grupos a cada asignatura del programa para cada curso académico. Es la fuente de datos de referencia para CARGA (cruce con planificación docente) y ASISTE (registro de presencia en espacio y tiempo).

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Ordenación académica | Herramienta de asignación de espacios y franjas que detecta conflictos antes de publicar el horario |
| Alumno | Fuente única y fiable para consultar su horario, integrada con el ecosistema académico |
| Profesor | Visibilidad de su carga horaria semanal, integrada con su planificación docente en CELDA |
| CARGA | Fechas reales de las sesiones de evaluación para el análisis de sobrecarga |
| ASISTE | Contexto espaciotemporal para cada registro de presencia |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🔴 Alta | La asignación óptima de aulas y franjas es un problema de satisfacción de restricciones (CSP): capacidad de aula, disponibilidad del profesor, franjas no solapadas para el mismo grupo, preferencias de horario. Resolverlo bien requiere un motor de restricciones o una heurística cuidadosamente diseñada. |
| Complejidad de dominio | 🔴 Alta | Los horarios académicos tienen un volumen de restricciones institucionales elevado: franjas reservadas, aulas con equipamiento especial, grupos partidos, docencia compartida entre dos profesores. Modelar todas las restricciones sin que el sistema sea imposible de usar es el reto de diseño central. |
| Dependencias | 🟡 Media | Depende de CELDA para el catálogo de asignaturas y grupos. Es relativamente independiente del resto de satélites, aunque CARGA y ASISTE dependen de él. |
| **Índice combinado** | 🔴 **Alta** | El proyecto técnicamente más complejo del roadmap. La resolución de restricciones de horarios es un problema clásico de IA/optimización que no se resuelve bien con un CRUD convencional. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Se resuelve la asignación automáticamente o es asistida?** Un asignador automático óptimo es complejo de construir y difícil de explicar cuando genera un resultado que no gusta. Un asignador asistido (el usuario asigna manualmente y el sistema detecta conflictos) es más simple y más transparente.
- **¿Cuál es el modelo de datos de una franja horaria?** Día de la semana, hora de inicio y fin, semana del semestre, o fecha concreta. La elección determina la flexibilidad del sistema frente a cambios puntuales (festivos, recuperaciones).
- **¿Cómo se modela un grupo partido?** Una asignatura con prácticas en grupos de 15 alumnos tiene una franja de teoría y varias de prácticas. El modelo tiene que soportar esa jerarquía.
- **¿Se importa de un sistema existente o se construye desde cero?** Si la institución ya tiene los horarios en un formato exportable, la primera versión puede ser un importador + visualizador, más rápida de construir y validar que un sistema de asignación completo.

### Cómo abordarlo

1. Empezar por el importador: si los horarios existen en algún formato (Excel, CSV, PDF), importarlos y visualizarlos es la primera versión útil y la que genera adopción.
2. Construir el detector de conflictos antes que el asignador automático: detectar solapamientos es más simple y más inmediatamente valioso.
3. El asignador automático como tercera fase, solo si las fases anteriores demuestran que hay demanda real para ello.
4. Exponer la API de horarios que necesitan CARGA y ASISTE desde la primera versión, aunque los datos sean importados y no generados por el sistema.
