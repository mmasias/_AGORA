# _AGORA

Pensando el futuro.

Mapa de proyectos del ecosistema académico construido sobre [CELDA](https://github.com/mmasias/pyCelda).

## Ecosistema

### Infraestructura base

| Proyecto | Expansión | Descripción |
|---|---|---|
| PANAL | Puerta de Acceso Normalizada a Aplicaciones LMS | Intermediario entre campus virtuales (LMS) y servicios académicos y administrativos |
| COLMENA | Componentes Ligeros para el Modelado de ERPs de Naturaleza Académica | Framework con el que se construyen los ERPs del ecosistema |
| AGORA | Aplicación de Gestión y ORdenación Académica | ERP académico sobre COLMENA |
| GUIAA | Gestión Unificada de Investigación, Academia y Administración | ERP ampliado sobre COLMENA |

### Roadmap CELDA

| Fase 0 | Fase I | Fase II | Fase III |
|---|---|---|---|
| **CELDA v0.0.1**<br><sub>Catálogo Electrónico Ligero de Documentación Académica (el proyecto original, agosto/septiembre 2026)</sub> | **CELDA v0.10.1**<br><sub>El proyecto en el punto en el que lo tenemos hoy</sub> | **MERITOS**<br><sub>Modelo de Expedientes y Registros de la Información de Trayectoria Ocupacional</sub> | **ASISTE**<br><sub>Aplicación de Seguimiento e Informe Sobre la Trazabilidad de Eventos</sub> |
| | | **PRISMA**<br><sub>Plataforma de Representación e Integración de Señales y Métricas Académicas</sub> | **FEDATARIO**<br><sub>Generación de documento oficial personalizado por alumno con las guías de su itinerario (servicio de pago)</sub> |
| | | **PULSO**<br><sub>Plataforma Unificada de Levantamiento y Seguimiento de Opiniones</sub> | **SIGHOR**<br><sub>Sistema de Gestión de Horarios</sub> |
| | | **VITRINA**<br><sub>Visibilizador Institucional de Titulaciones, Rubros e Información Normativa Académica</sub> | **ACTIVITAT**<br><sub>Gestor de actividad docente de los profesores</sub> |
| | | **CITA**<br><sub>Catálogo e Indexador de Textos Académicos</sub> | |
| | | **CARGA**<br><sub>Control y Análisis del Reparto y Gestión de Actividades</sub> | |

### Expansión de acrónimos

| Proyecto | Expansión | Descripción |
|---|---|---|
| CELDA | Catálogo Electrónico Ligero de Documentación Académica | <sub>Gestión del ciclo de vida completo de las guías docentes universitarias: redacción, revisión y aprobación con flujo de estados controlado, planificación por sesiones, ponderaciones de evaluación y bibliografía sobre un catálogo institucional validado contra memorias ANECA.</sub> |
| MERITOS | Modelo de Expedientes y Registros de la Información de Trayectoria Ocupacional | <sub>Repositorio personal del profesor donde vuelca su actividad investigadora y académica: publicaciones, cargos, acreditaciones, sexenios y quinquenios. Complementa el perfil docente de CELDA sin pervertir su propósito principal.</sub> |
| PRISMA | Plataforma de Representación e Integración de Señales y Métricas Académicas | <sub>Agregador de indicadores extraídos de todo el ecosistema en un único lugar. Lee sin escribir: consume datos de CELDA y del resto de satélites para ofrecer métricas institucionales de calidad académica sin contaminar ninguno de los sistemas fuente.</sub> |
| PULSO | Plataforma Unificada de Levantamiento y Seguimiento de Opiniones | <sub>Gestor de encuestas docentes. Permite diseñar, distribuir y agregar encuestas dirigidas a alumnos, profesores o directores de programa, con seguimiento longitudinal por asignatura y curso académico.</sub> |
| VITRINA | Visibilizador Institucional de Titulaciones, Rubros e Información Normativa Académica | <sub>Portal público de consulta de las guías docentes aprobadas y vigentes del curso académico activo. Acceso sin autenticación, orientado a alumnos, futuros alumnos y organismos externos como ANECA.</sub> |
| CITA | Catálogo e Indexador de Textos Académicos | <sub>Normalizador de referencias bibliográficas institucional. Resuelve el problema estructural de CELDA donde cada referencia vive como texto libre por guía: centraliza el catálogo, deduplica por cita normalizada y permite que una corrección se propague a todas las guías que la usan.</sub> |
| CARGA | Control y Análisis del Reparto y Gestión de Actividades | <sub>Detector de sobrecarga de evaluación a partir de la planificación docente real de CELDA. Cruza las sesiones planificadas con los horarios de SIGHOR para identificar acumulación de entregas o exámenes en ventanas críticas del calendario académico.</sub> |
| ASISTE | Aplicación de Seguimiento e Informe Sobre la Trazabilidad de Eventos | <sub>Control de presencia y asistencia a eventos académicos: clases, tutorías, seminarios. Genera el informe de trazabilidad de eventos que permite a secretaría académica acreditar la actividad docente real frente a la planificada en la guía.</sub> |
| FEDATARIO | (nombre propio, sin expansión) | <sub>Generación del documento oficial personalizado por alumno con las guías docentes de su itinerario real. Dado que CELDA conserva las guías aprobadas año a año y conoce qué asignatura cursó el alumno en qué curso, puede compilar un libro certificado de su trayectoria académica. Servicio de pago.</sub> |
| SIGHOR | Sistema de Gestión de Horarios | <sub>Gestor de horarios académicos: asignación de aulas, franjas y grupos para cada asignatura del programa. Fuente de datos para CARGA y punto de entrada para la planificación de ASISTE.</sub> |
| ACTIVITAT | (nombre propio en valenciano, sin expansión) | <sub>Gestor de actividad docente del profesorado. A partir de los datos de impartición registrados en CELDA (quién impartió qué asignatura en qué curso), genera el informe anual de carga docente que cada profesor entrega a secretaría académica.</sub> |
