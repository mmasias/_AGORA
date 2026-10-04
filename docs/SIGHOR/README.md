# SIGHOR

## ¿Por qué?

La planificación docente de CELDA sabe qué se enseña y cuándo, pero no sabe en qué aula ni a qué hora. Los horarios académicos - qué grupo tiene qué asignatura, en qué franja y en qué espacio - viven en sistemas propietarios, hojas de cálculo o en el LMS, desconectados del resto del ecosistema.

Esa desconexión tiene consecuencias concretas: ningún alumno tiene una fuente única y fiable para consultar su horario.

SIGHOR es esa fuente: el gestor de horarios académicos del ecosistema.

No parte de cero: es una reingeniería de SigHor, el generador de horarios desarrollado en 1998 para la Universidad de Piura (Visual Basic 3.0), cuyo código y análisis están documentados en [pySigHor](https://github.com/mmasias/pySigHor): el algoritmo de cuatro fases en `src/Horario.bas` y sus lecciones en `extraDocs/000-ingenieria-inversa/reflexionesAlgoritmo.md`. La principal: el horario matemáticamente óptimo concentraba las clases en los primeros días y las primeras horas, y resultaba impracticable para las personas.

## ¿Qué?

Un sistema de gestión de horarios que asigna aulas, franjas horarias y grupos a cada asignatura del programa para cada curso académico.

Es el **dueño de los grupos**: los infiere por afinidad de asignatura (asignaturas de distintos programas que comparten la misma asignatura del catálogo pueden impartirse como un único grupo) y permite ajustarlos cuando la inferencia no es adecuada. CELDA no modela grupos: su planificación por sesiones es la misma para todos los grupos de una asignatura.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Ordenación académica | Herramienta de asignación de espacios y franjas que detecta conflictos antes de publicar el horario |
| Alumno | Fuente única y fiable para consultar su horario, integrada con el ecosistema académico |
| Profesor | Visibilidad de su carga horaria semanal, integrada con su planificación docente en CELDA |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🔴 Alta | La asignación óptima de aulas y franjas es un problema de satisfacción de restricciones (CSP): capacidad de aula, disponibilidad del profesor, franjas no solapadas para el mismo grupo, preferencias de horario. Resolverlo bien requiere un motor de restricciones o una heurística cuidadosamente diseñada. |
| Complejidad de dominio | 🔴 Alta | Los horarios académicos tienen un volumen de restricciones institucionales elevado: franjas reservadas, aulas con equipamiento especial, grupos partidos, docencia compartida entre dos profesores. Modelar todas las restricciones sin que el sistema sea imposible de usar es el reto de diseño central. |
| Dependencias | 🟡 Media | Depende de CELDA (catálogo de asignaturas, profesorado, horas presenciales) y del ERP de la universidad (número de matriculados, para dimensionar aulas); los grupos los crea el propio SIGHOR. |
| **Índice combinado** | 🔴 **Alta** | El proyecto técnicamente más complejo del roadmap. La resolución de restricciones de horarios es un problema clásico de IA/optimización que no se resuelve bien con un CRUD convencional. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Se resuelve la asignación automáticamente o es asistida?** Un asignador automático óptimo es complejo de construir y difícil de explicar cuando genera un resultado que no gusta. Un asignador asistido (el usuario asigna manualmente y el sistema detecta conflictos) es más simple y más transparente. El precedente de 1998 inclina la balanza: un óptimo sin restricciones humanas explícitas produjo horarios impracticables.
- **¿Cuál es el modelo de datos de una franja horaria?** Día de la semana, hora de inicio y fin, semana del semestre, o fecha concreta. La elección determina la flexibilidad del sistema frente a cambios puntuales (festivos, recuperaciones).
- **¿Cómo se modela un grupo partido?** Una asignatura con prácticas en grupos de 15 alumnos tiene una franja de teoría y varias de prácticas. El modelo tiene que soportar esa jerarquía.
- **¿Se importa de un sistema existente o se construye desde cero?** Si la institución ya tiene los horarios en un formato exportable, la primera versión puede ser un importador + visualizador, más rápida de construir y validar que un sistema de asignación completo.

### Cómo abordarlo

1. Empezar por el importador: si los horarios existen en algún formato (Excel, CSV, PDF), importarlos y visualizarlos es la primera versión útil y la que genera adopción.
2. Construir el detector de conflictos antes que el asignador automático: detectar solapamientos es más simple y más inmediatamente valioso.
3. El asignador automático como tercera fase, solo si las fases anteriores demuestran que hay demanda real para ello.
4. Exponer la API de horarios desde la primera versión, aunque los datos sean importados y no generados por el sistema.

Por su complejidad, es previsiblemente de los últimos proyectos del roadmap, si no el último.
