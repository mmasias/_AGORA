# PULSO

## ¿Por qué?

La opinión de los alumnos sobre la docencia es un dato que las instituciones recogen obligatoriamente para los procesos de acreditación y mejora continua. Hoy esa recogida se hace con herramientas genéricas (Google Forms, encuestas del LMS) que no están conectadas con el catálogo real de asignaturas, programas y profesores.

El resultado es que los datos llegan descontextualizados: hay que cruzar manualmente la respuesta de un alumno con la asignatura que cursó, el profesor que la impartió y el curso académico en que lo hizo. Ese cruce, cuando se hace, se hace en una hoja de cálculo.

PULSO cierra ese hueco: encuestas diseñadas sobre el catálogo real de CELDA, distribuidas en el momento correcto del calendario académico y con los datos ya contextualizados desde el primer momento.

## ¿Qué?

Un gestor de encuestas docentes vinculado al ecosistema académico. Permite diseñar plantillas de encuesta, distribuirlas a los destinatarios correctos (alumnos de una asignatura, profesores de un programa, directores de titulación) y agregar las respuestas con trazabilidad completa: qué asignatura, qué curso académico, qué programa.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Gabinete de calidad | Encuestas de satisfacción docente con contexto académico completo, sin cruce manual de datos |
| Directores de programa | Resultados por asignatura y por curso, comparables longitudinalmente |
| Profesores | Retroalimentación estructurada sobre su docencia, por asignatura y curso |
| PRISMA | Indicadores de satisfacción listos para agregar en dashboards institucionales |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟡 Media | El motor de encuestas (diseño de preguntas, distribución, recogida de respuestas) es un dominio resuelto, pero integrarlo con el catálogo de CELDA y garantizar el anonimato de las respuestas añade complejidad real. |
| Complejidad de dominio | 🟡 Media | Las encuestas docentes tienen requisitos específicos: anonimato de las respuestas frente a trazabilidad del contexto, umbrales mínimos de participación para publicar resultados, ventanas temporales ligadas al calendario académico. No es texto libre - hay reglas institucionales. |
| Dependencias | 🟢 Baja (v1) | Depende de CELDA para el catálogo de asignaturas, programas y profesores. La primera versión no necesita censo de alumnos: verifica la pertenencia a la asignatura con un código de invitación. El censo del ERP de la universidad (GUIAA o AGORA) llega en una segunda versión, para el control de participación y los umbrales de representatividad. Depende del LMS (via PANAL) para la distribución si se quiere que llegue directamente al campus virtual. |
| **Índice combinado** | 🟡 **Media** | Un proyecto con complejidad real pero acotada. El riesgo principal es el anonimato: garantizar que las respuestas no son trazables hasta el alumno individual mientras se mantiene el contexto académico es un problema de diseño que hay que resolver antes de escribir código. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Cómo se garantiza el anonimato?** Las respuestas deben ser anónimas para el profesor evaluado pero contextualizadas por asignatura y curso. El mecanismo de separación entre identidad del respondente (quiénes ya participaron: en v1, por código de invitación de un solo uso; en v2, contra el censo del ERP de la universidad) y contenido de la respuesta (qué contestaron, anónimo para el análisis) es la decisión más crítica del proyecto.
- **¿Quién distribuye las encuestas y cómo llegan a los alumnos?** En v1 el profesor o el gabinete reparte el código de invitación de la asignatura; en v2 el censo de destinatarios lo suministra el ERP de la universidad. El canal puede ser email, LMS via PANAL, u otro. El canal de distribución condiciona la arquitectura.
- **¿Qué umbral mínimo de participación se exige para publicar resultados?** Una encuesta con 2 respuestas sobre 30 alumnos no es representativa. La política de umbrales es institucional.
- **¿Las encuestas son plantillas reutilizables o se diseñan de cero cada vez?** Un catálogo de plantillas institucionales reduce la carga del gabinete de calidad y garantiza comparabilidad longitudinal.

### Cómo abordarlo

1. Definir la política de anonimato con la institución antes de diseñar el modelo de datos. No hay solución técnica para una política de privacidad que no existe.
2. Construir el motor de encuestas sobre el catálogo de CELDA: cada encuesta nace vinculada a un programa, asignatura y curso académico, no como formulario genérico. Acceso con código de invitación por asignatura, sin censo.
3. Separar en el modelo la identidad del respondente (quién contestó, para saber si ya participó) del contenido de la respuesta (qué contestó, anónimo para el análisis).
4. Integrar con PANAL para distribución via LMS y con el censo del ERP de la universidad como segunda fase, no como requisito de la primera versión.
