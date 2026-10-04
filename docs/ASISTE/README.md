# ASISTE

## ¿Por qué?

Una guía docente planifica 30 sesiones de clase. ¿Se impartieron realmente esas 30 sesiones? ¿Cuántas asistió el alumno? ¿El profesor cubrió la totalidad del temario planificado?

Hoy esas preguntas no tienen respuesta sistemática. La asistencia, cuando se registra, vive en listas en papel o en hojas de cálculo personales de cada profesor. Secretaría académica no tiene forma de acreditar la actividad docente real frente a la planificada sin perseguir a cada profesor individualmente.

ASISTE cierra ese hueco: registro de presencia vinculado a la planificación docente de CELDA.

No parte de cero: cuenta con un primer esbozo, [pySesion](https://github.com/mmasias/pySesion), con Requisitos y Análisis cerrados, que hay que revisar para rescatar lo trabajado. Entre sus decisiones: control por QR rotatorio, sesión generalizada (clase, laboratorio o tutoría) siempre vinculada a la guía docente, y sin censo de matriculados (el rol se infiere del dominio de correo).

## ¿Qué?

Un **fedatario de asistencia** a eventos académicos: clases, tutorías, seminarios. Registra quién asistió a cada sesión y da fe de ello. Cada registro está vinculado a una sesión de la guía docente de CELDA y, cuando existan, a los grupos que crea SIGHOR, lo que permite comparar lo planificado con lo impartido.

ASISTE no valida si quien asiste está matriculado ni aplica reglas académicas (por ejemplo, un mínimo de asistencia para presentarse a examen). Esas reglas son de otros sistemas, que le preguntan y obtienen respuesta: "¿ha asistido el alumno X a todas las sesiones de la asignatura Y?". Eso sí lo contesta ASISTE.

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
| Complejidad de dominio | 🟢 Baja-media | Al no aplicar reglas académicas ni validar matrícula, el dominio se reduce a registrar y consultar asistencias. Lo que queda es la variedad de modalidades (presencial, semipresencial, online) y de tipos de evento (clase, laboratorio, tutoría). |
| Dependencias | 🟡 Media | Depende de CELDA para las sesiones y el catálogo, y de SIGHOR para los grupos, su aula y su franja. No depende del censo de alumnos del ERP de la universidad. Como SIGHOR irá previsiblemente al final del roadmap, una primera versión puede registrar la asistencia por sesión de guía, sin grupos, como ya hace pySesion. |
| **Índice combinado** | 🟡 **Media-alta** | La complejidad técnica del mecanismo de registro es el reto principal. El dominio es acotado, pero elegir mal el mecanismo de captura (algo que los alumnos no usan o que el profesor no puede operar en los primeros 5 minutos de clase) hace que el sistema no se adopte, independientemente de lo bien construido que esté. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Cuál es el mecanismo de registro de asistencia?** pySesion eligió QR rotatorio; la alternativa sería NFC, código de clase o lista manual digitalizada. Revisar esa decisión con lo aprendido en su análisis, no reabrirla de cero.
- **¿Se registra la asistencia del alumno o la presencia del profesor?** Son dos problemas distintos con modelos distintos. El informe de secretaría académica necesita los dos.
- **¿Cómo se maneja la asistencia en modalidad online o híbrida?** El registro de presencia física no funciona para clases síncronas por videoconferencia.
- **¿Qué ocurre con las discrepancias?** Un alumno que alega haber asistido sin que conste en el registro necesita un flujo de reclamación. Sin él, el sistema genera más problemas de los que resuelve.

### Cómo abordarlo

1. Revisar pySesion (Requisitos, Análisis y discussions) y rescatar lo trabajado.
2. Validar el mecanismo de registro con una prueba piloto real antes de construir la infraestructura completa. Un prototipo de QR dinámico en papel durante dos semanas revela más problemas que seis meses de diseño teórico.
3. Construir primero el registro de presencia del profesor (más simple, menos polémico) y después el del alumno.
4. Integrar con CELDA para que cada registro de asistencia apunte a una sesión concreta de la guía docente; con SIGHOR, además, a su grupo.
5. El informe de secretaría como primera salida concreta: define el formato con secretaría académica antes de construir el modelo de datos.
