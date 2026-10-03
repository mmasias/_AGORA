# ASISTE

## ¿Por qué?

Una guía docente planifica 30 sesiones de clase. ¿Se impartieron realmente esas 30 sesiones? ¿Cuántas asistió el alumno? ¿El profesor cubrió la totalidad del temario planificado?

Hoy esas preguntas no tienen respuesta sistemática. La asistencia, cuando se registra, vive en listas en papel o en hojas de cálculo personales de cada profesor. Secretaría académica no tiene forma de acreditar la actividad docente real frente a la planificada sin perseguir a cada profesor individualmente.

ASISTE cierra ese hueco: registro de presencia vinculado al calendario académico real y a la planificación docente de CELDA.

## ¿Qué?

Una aplicación de control de presencia y asistencia a eventos académicos: clases, tutorías, seminarios. Cada registro de asistencia está vinculado a una sesión de la guía docente de CELDA, lo que permite comparar directamente lo planificado con lo impartido.

Genera el informe de trazabilidad de eventos que permite a secretaría académica acreditar la actividad docente real.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Secretaría académica | Informe de actividad docente real vs. planificada, sin depender de la buena voluntad de cada profesor |
| Profesor | Registro automatizado de asistencia sin listas en papel, y evidencia de cumplimiento de su planificación |
| Director de programa | Visión del grado de cumplimiento de la planificación docente en su titulación |
| Alumno | Registro de su asistencia, consultable y reclamable si hay discrepancias |
| PRISMA | Indicadores de cumplimiento docente para dashboards institucionales |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🔴 Alta | El registro de asistencia en tiempo real en una sala con 30 alumnos es un problema de UX e infraestructura no trivial. QR dinámico, NFC, app móvil, lista manual digitalizada: cada mecanismo tiene sus propias implicaciones técnicas y de fiabilidad. |
| Complejidad de dominio | 🟡 Media | Las reglas de asistencia varían por asignatura (algunas exigen un mínimo para presentarse a examen), por modalidad (presencial, semipresencial, online) y por tipo de evento (clase obligatoria vs. tutoría voluntaria). El modelo tiene que ser flexible sin ser caótico. |
| Dependencias | 🟡 Media | Depende de CELDA para la planificación de sesiones y del catálogo de asignaturas y grupos. Depende de SIGHOR para saber qué grupo está en qué aula en cada franja. |
| **Índice combinado** | 🔴 **Alta** | La complejidad técnica del mecanismo de registro es el reto principal. El dominio es manejable, pero elegir mal el mecanismo de captura (algo que los alumnos no usan o que el profesor no puede operar en los primeros 5 minutos de clase) hace que el sistema no se adopte, independientemente de lo bien construido que esté. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Cuál es el mecanismo de registro de asistencia?** QR dinámico (renovado cada N minutos para evitar suplantación), NFC, código de clase, o lista manual digitalizada. La elección condiciona toda la arquitectura.
- **¿Se registra la asistencia del alumno o la presencia del profesor?** Son dos problemas distintos con modelos distintos. El informe de secretaría académica necesita los dos.
- **¿Cómo se maneja la asistencia en modalidad online o híbrida?** El registro de presencia física no funciona para clases síncronas por videoconferencia.
- **¿Qué ocurre con las discrepancias?** Un alumno que alega haber asistido sin que conste en el registro necesita un flujo de reclamación. Sin él, el sistema genera más problemas de los que resuelve.

### Cómo abordarlo

1. Validar el mecanismo de registro con una prueba piloto real antes de construir la infraestructura completa. Un prototipo de QR dinámico en papel durante dos semanas revela más problemas que seis meses de diseño teórico.
2. Construir primero el registro de presencia del profesor (más simple, menos polémico) y después el del alumno.
3. Integrar con CELDA para que cada registro de asistencia apunte a una sesión concreta de la guía docente.
4. El informe de secretaría como primera salida concreta: define el formato con secretaría académica antes de construir el modelo de datos.
