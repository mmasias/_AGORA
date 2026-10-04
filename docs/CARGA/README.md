# CARGA

## ¿Por qué?

Un alumno que cursa cinco asignaturas en el mismo semestre puede encontrarse con tres entregas y un examen parcial en la misma semana, sin que ningún profesor ni ningún director de programa lo haya detectado. Cada guía docente planifica sus sesiones de forma independiente, sin visión del conjunto.

CELDA tiene la planificación de sesiones de cada asignatura: cuántas sesiones por semana, en qué orden, y cuáles son de evaluación, marcadas por su tipo. Y, por norma de la universidad, todas las asignaturas empiezan la misma semana. Con esas piezas se puede inferir, antes de que el curso empiece, en qué semana cae cada evaluación y detectar las semanas donde se acumula carga.

CARGA convierte esa detección en un proceso automático y sistemático.

## ¿Qué?

Un detector de sobrecarga de evaluación a partir de la planificación docente real. Para cada asignatura de un programa y semestre, toma de CELDA las sesiones de evaluación (tipo `EVALUACION_CONTINUA` o `EVALUACION_PARCIAL`) su número de orden y las sesiones por semana; con la semana de inicio común calcula la semana de cada evaluación e identifica las semanas donde la carga supera umbrales razonables para el alumno.

El cruce es indirecto -- tipo y orden de sesión, sesiones por semana, inicio común --, no una lectura de fechas: CELDA no guarda fechas de sesión. El cruce se hace por asignatura del programa, no por grupo: la planificación por sesiones es la misma para todos los grupos de una asignatura.

No toma decisiones: informa. La decisión de redistribuir evaluaciones es del director de programa.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Director de programa | Detección temprana de acumulación de evaluaciones antes de que el curso empiece, con tiempo para redistribuir |
| Alumno | Una distribución de evaluaciones más equilibrada a lo largo del semestre |
| Gabinete de calidad | Evidencia de que el programa supervisa la carga de trabajo del alumno, requisito de algunos marcos de acreditación |
| PRISMA | Indicador de equilibrio de carga evaluativa por programa y semestre |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟡 Media | El cálculo en sí es sencillo (semana = inicio común + posición de la sesión según las sesiones por semana), pero depende de que los tres datos sean coherentes: una planificación con sesiones de más o de menos desplaza todas las evaluaciones posteriores. |
| Complejidad de dominio | 🟡 Media | Definir qué es "sobrecarga" es una decisión institucional: ¿cuántas evaluaciones en una semana son demasiadas? ¿Se cuenta por alumno individual o por grupo? ¿Se ponderan por peso en la nota? Los umbrales son configurables pero alguien tiene que definirlos. |
| Dependencias | 🟢 Baja | Depende solo de CELDA: planificación docente (sesiones de evaluación, orden, sesiones por semana) y calendario lectivo (semana de inicio de cada semestre, hoy no modelada). |
| **Índice combinado** | 🟡 **Media** | Cálculo sencillo sobre datos que ya tiene CELDA. Lo que queda es institucional (qué es sobrecarga, umbrales) y un dato por modelar en CELDA: la semana de inicio de cada semestre. |

</div>

### Decisiones de diseño a tomar antes de construir

- **Unidad de análisis (decidido): la semana**, con todas las asignaturas empezando la misma semana por norma de la universidad, y el cruce por asignatura del programa, no por grupo.
- **¿De dónde sale la semana de inicio de cada semestre?** El calendario lectivo es de CELDA, pero hoy solo tiene inicio, fin y semestre activo del curso.
- **¿Se pesan las evaluaciones por ponderación en la nota?** Un examen del 50% no es lo mismo que una entrega del 5%, aunque ambos sean "evaluaciones" en el calendario.
- **¿Es CARGA un proceso bajo demanda o un proceso automático?** Ejecutarlo cada vez que un profesor guarda su planificación o ejecutarlo periódicamente (cada noche, por ejemplo) son dos arquitecturas distintas con implicaciones muy diferentes en carga de sistema.

### Cómo abordarlo

1. Asegurar los datos de entrada en CELDA: la planificación docente (ya existe) y la semana de inicio del semestre en el calendario lectivo.
2. Definir con los directores de programa los umbrales que consideran razonables - sin esa conversación, el detector genera alertas que nadie atiende.
3. Construir CARGA como proceso de análisis bajo demanda en la primera versión: el director lo ejecuta cuando quiere, no en tiempo real.
4. Evolucionar hacia detección automática y notificación proactiva en versiones posteriores, una vez validado que los umbrales son correctos.
