# ACTIVITAT

## ¿Por qué?

Cada curso académico, cada profesor tiene que entregar a secretaría académica un informe de su carga docente: qué asignaturas impartió, en qué programa, cuántas horas. Ese informe se elabora hoy de memoria o revisando el correo, porque no hay ningún sistema que lo genere automáticamente.

CELDA ya tiene esa información: sabe quién impartió qué asignatura en qué curso académico, porque esos datos se registran cuando el profesor firma y aprueba su guía docente. Lo único que falta es extraerlos y darles el formato que secretaría académica necesita.

ACTIVITAT hace exactamente eso: genera el informe anual de carga docente de cada profesor a partir de los datos ya existentes en CELDA, sin que nadie tenga que introducir nada nuevo.

## ¿Qué?

Un gestor de actividad docente del profesorado. A partir de los datos de impartición registrados en CELDA - quién impartió qué asignatura en qué curso académico - genera el informe anual de carga docente que cada profesor entrega a secretaría académica y que la institución usa para los procesos de evaluación del profesorado.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Profesor | El informe anual de carga docente generado automáticamente, sin trabajo manual |
| Secretaría académica | Informes normalizados de toda la plantilla, comparables entre sí y verificables contra el catálogo |
| Director de programa | Carga docente de su programa (el departamento se deduce del programa en que se imparte cada asignatura), para decisiones de contratación y asignación |
| MERITOS | Datos de actividad docente para complementar el perfil del profesor |
| PRISMA | Indicadores de carga docente por programa y por área para dashboards institucionales |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟢 Baja | El núcleo del proyecto es una consulta agregada sobre datos ya existentes en CELDA y un generador de informes. Técnicamente es el proyecto más simple del roadmap junto con VITRINA. |
| Complejidad de dominio | 🟡 Media | El informe de carga docente tiene un formato institucional específico que varía por universidad y por normativa. Además, la carga real puede ser distinta de la planificada si hubo sustituciones o impartición compartida. Modelar esas casuísticas requiere trabajo previo con secretaría académica. |
| Dependencias | 🟢 Baja | Depende casi exclusivamente de CELDA. Los datos de quién impartió qué ya están ahí. La única dependencia adicional es el formato de salida que necesita secretaría académica. |
| **Índice combinado** | 🟢 **Baja-media** | El proyecto con mejor ratio valor/esfuerzo del roadmap. Los datos están, el procesamiento es simple, y el resultado es inmediatamente útil para un proceso real que hoy se hace a mano. El reto es el formato de salida: sin validación previa con secretaría académica, el informe generado puede no encajar con lo que esperan. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Cuál es el formato exacto del informe?** Cada institución tiene su propio formato para el informe de carga docente. La primera tarea es conseguir una plantilla real y validar que los datos de CELDA son suficientes para rellenarla.
- **¿Cómo se modela la impartición compartida?** Una asignatura con dos profesores: ¿se divide la carga por horas reales de cada uno, o se asigna completa a ambos? La respuesta depende de la normativa institucional.
- **¿Planificada o real? (decidido)** CELDA da la carga planificada; ASISTE, cuando exista, la real: sesiones impartidas de verdad (con sustituciones) y horas de tutoría, un dato que hoy no se mide. Sin ASISTE, solo cuenta lo planificado en CELDA.
- **¿El informe lo genera el profesor, secretaría o es automático?** Un informe que se genera automáticamente al cerrar el curso académico y se envía al profesor para que lo revise y firme es el flujo más eficiente, pero requiere validar el dato de impartición antes de enviar.

### Cómo abordarlo

1. Obtener el formato real del informe de carga docente de secretaría académica y verificar que los datos de CELDA son suficientes para generarlo.
2. Construir el generador como consulta + exportación PDF/Excel sobre los datos existentes en CELDA. Sin modelo de datos nuevo.
3. Validar el informe generado con dos o tres profesores reales antes de distribuirlo a toda la plantilla.
4. Integrar con ASISTE en una segunda fase para incluir la carga real: sesiones impartidas y tutorías.
