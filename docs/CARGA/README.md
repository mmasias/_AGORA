# CARGA

## ¿Por qué?

Un alumno que cursa cinco asignaturas en el mismo semestre puede encontrarse con tres entregas y un examen parcial en la misma semana, sin que ningún profesor ni ningún director de programa lo haya detectado. Cada guía docente planifica sus sesiones de forma independiente, sin visión del conjunto.

CELDA tiene la planificación de sesiones de cada asignatura. SIGHOR tiene los horarios reales de cada grupo. Cruzar esos dos datos permite detectar, antes de que el curso empiece, las ventanas críticas donde se acumula carga de evaluación.

CARGA convierte esa detección en un proceso automático y sistemático.

## ¿Qué?

Un detector de sobrecarga de evaluación a partir de la planificación docente real. Cruza las sesiones de tipo evaluación planificadas en las guías de CELDA con los horarios de SIGHOR para identificar semanas o periodos donde la carga de evaluación supera umbrales razonables para el alumno.

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
| Complejidad técnica | 🟡 Media | El cruce de datos entre dos fuentes (CELDA y SIGHOR) con granularidades distintas (sesiones de guía vs. franjas horarias de grupos) requiere una capa de normalización temporal no trivial. |
| Complejidad de dominio | 🟡 Media | Definir qué es "sobrecarga" es una decisión institucional: ¿cuántas evaluaciones en una semana son demasiadas? ¿Se cuenta por alumno individual o por grupo? ¿Se ponderan por peso en la nota? Los umbrales son configurables pero alguien tiene que definirlos. |
| Dependencias | 🔴 Alta | Depende de CELDA (planificación de sesiones) y de SIGHOR (horarios). Si SIGHOR no existe, CARGA solo puede analizar la planificación temporal sin anclarla a fechas reales del calendario. Es el proyecto con dependencia más directa de otro satélite no construido. |
| **Índice combinado** | 🔴 **Alta** | No por complejidad intrínseca sino por dependencias: CARGA sin SIGHOR es solo la mitad del análisis. La secuencia natural es SIGHOR primero, CARGA después. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Qué unidad de tiempo es la ventana de análisis?** Semana natural, semana lectiva, o periodo configurable. La elección cambia la sensibilidad del detector.
- **¿Se cruza por grupo o por alumno?** Un alumno en un grupo de mañana y otro en uno de tarde pueden tener cargas distintas aunque cursen la misma asignatura.
- **¿Se pesan las evaluaciones por ponderación en la nota?** Un examen del 50% no es lo mismo que una entrega del 5%, aunque ambos sean "evaluaciones" en el calendario.
- **¿Es CARGA un proceso bajo demanda o un proceso automático?** Ejecutarlo cada vez que un profesor guarda su planificación o ejecutarlo periódicamente (cada noche, por ejemplo) son dos arquitecturas distintas con implicaciones muy diferentes en carga de sistema.

### Cómo abordarlo

1. Construir SIGHOR primero, o definir al menos su API de consulta de horarios.
2. Definir con los directores de programa los umbrales que consideran razonables — sin esa conversación, el detector genera alertas que nadie atiende.
3. Construir CARGA como proceso de análisis bajo demanda en la primera versión: el director lo ejecuta cuando quiere, no en tiempo real.
4. Evolucionar hacia detección automática y notificación proactiva en versiones posteriores, una vez validado que los umbrales son correctos.
