# Ecosistema CELDA

Mapa de proyectos del ecosistema construido sobre [CELDA](https://github.com/mmasias/pyCelda).

## ¿Por qué?

CELDA ya resolvió la parte cara de cualquier herramienta académica: el modelado curricular. Sabe qué programas hay, qué asignaturas los componen, quién imparte cada una, cómo se planifican sus sesiones y qué guía docente está aprobada en cada curso académico, con los datos validados contra las memorias de verificación.

Muchas funciones que la institución necesita siguen fuera de ese modelo: las sesiones de clase no se miden, las encuestas docentes se recogen con formularios genéricos desconectados del catálogo, el informe de carga docente se elabora de memoria, la misma referencia bibliográfica se teclea en decenas de guías con formatos distintos, y las guías aprobadas no tienen salida pública.

Construir cada una de esas funciones desde cero obligaría a repetir el modelado que CELDA ya tiene. Meterlas todas dentro de CELDA la desnaturalizaría: CELDA es un sistema de guías docentes, y cada función ajena que absorbe la hace más difícil de mantener. Ya ocurrió con el perfil académico del profesor, implantado dentro de CELDA por necesidad y que ahora hay que extraer (ver [MERITOS](docs/MERITOS/README.md)).

## ¿Qué?

Un núcleo, CELDA, y un conjunto de satélites alrededor. Cada satélite añade una función concreta sobre datos que CELDA ya modela, sin volver a modelarlos.

<div align=center>

|![](/images/modelosUML/ecosistemaCelda.svg)
|-:
[Ver más...](docs/MAPA.md)

</div>

Tres principios gobiernan el conjunto:

- **Cada dato tiene un único dueño**, que lo crea y lo modifica; el resto lo consume. Lo mismo vale para las reglas: si un satélite calcula algo (qué es sobrecarga de evaluación, cómo se computa una hora docente), el resto lee su resultado y no lo recalcula.
- **Los satélites consumen CELDA sin cambiar su naturaleza**: CELDA sigue siendo un sistema de guías docentes.
- **Los sistemas existentes de la organización se consumen, no se reconstruyen**: ERPs, campus virtual y su framework común.

## ¿Para qué?

<div align=center>

| Audiencia | Qué obtiene | Satélites |
|---|---|---|
| Profesorado | Un único lugar para su trayectoria académica e investigadora; el informe anual de carga docente generado sin trabajo manual; registro de asistencia sin listas en papel | MERITOS, ACTIVITAT, ASISTE |
| Alumnado y público | Consulta pública de las guías vigentes, antes y después de matricularse; registro de su propia asistencia; documento certificado con las guías de su itinerario | VITRINA, ASISTE, FEDATARIO |
| Calidad y ANECA | Indicadores de guías, satisfacción, asistencia, carga docente, perfil del claustro e indicadores académicos en un solo lugar, sin ensamblar hojas de cálculo; encuestas contextualizadas por asignatura y curso | PRISMA, PULSO |
| Dirección de programa | Detección temprana de semanas con acumulación de evaluaciones; carga docente de su programa; horarios con conflictos detectados antes de publicarse | CARGA, ACTIVITAT, SIGHOR |
| Secretaría académica | Actividad docente real frente a la planificada, incluidas las tutorías; informes normalizados de toda la plantilla | ASISTE, ACTIVITAT |
| Biblioteca y profesorado | Un catálogo de referencias bien especificadas del que copiar, y visibilidad de qué bibliografía se usa realmente | CITA |

</div>

## ¿Cómo?

### Sistemas existentes de la organización

Externos a este mapa: el ecosistema los consume, no los construye.

<div align=center>

| Sistema | Expansión | Descripción |
|---|---|---|
| **SG** | GUIAA / AGORA | ERPs de la organización |
| **PANAL** | Puerta de Acceso Normalizada a Aplicaciones LMS | Servicio intermediario entre campus virtuales y servicios académicos y administrativos |
| **CAMPUS** | | LMS: campus virtual |
| ***COLMENA*** | Componentes Ligeros para el Modelado de ERPs de Naturaleza Académica | Framework con el que se construyen los ERPs de la organización. Al compartir sus componentes, los ERPs exponen la misma interfaz: el ecosistema se integra una sola vez, contra COLMENA |

</div>

> *En el resto del mapa, **"ERP de la universidad"** designa a SG, GUIAA o AGORA según corresponda: es la fuente de la identidad de los alumnos, la matrícula, el itinerario del alumno y el catálogo curricular de alto nivel.*

### Datos maestros

Cada dato compartido tiene un único dueño, que lo crea y lo modifica; el resto lo consume.

<div align=center>

| Dato maestro | Dueño | Notas |
|---|---|---|
| Catálogo curricular de alto nivel | ERP de la universidad | Frontera con CELDA por precisar |
| Programa, Materia, Asignatura (nivel memorias) | CELDA | Elementos de bajo nivel, procedentes de las memorias de verificación |
| Profesor (identidad) | La universidad (externo) | Fuera de todos los servicios del mapa; CELDA asocia el email con sus roles, no da de alta |
| Datos aportados por el profesor | MERITOS | Perfil, acreditaciones, publicaciones, cargos |
| Alumno, matrícula, itinerario | ERP de la universidad | |
| Grupo | SIGHOR | Inferido por afinidad de asignatura y ajustable |
| Curso académico | CELDA | |
| Calendario lectivo | CELDA | Hoy solo inicio, fin y semestre activo del curso; la semana de inicio de cada semestre, por precisar |
| Aula, franja | SIGHOR | |

</div>

### Mapa de dependencias

Qué consume cada satélite, de dónde, y desde cuándo: [docs/MAPA.md](docs/MAPA.md).

### Roadmap CELDA

<div align=center>

| Fase 0 | Fase I | Fase II | Fase III |
|:-:|:-:|:-:|:-:|
| **CELDA <sub>v0.0.1** | **CELDA <sub>v0.10.2** | **MERITOS** | **ASISTE** |
| | | **PRISMA** | **FEDATARIO** |
| | | **PULSO** | **SIGHOR** |
| | | **VITRINA** | **ACTIVITAT** |
| | | **CITA** | |
| | | **CARGA** | |

</div>

### Detalle de proyectos

| Proyecto | Expansión | Descripción |
|---|---|---|
| [**CELDA**](https://github.com/mmasias/pyCelda) | Catálogo Electrónico Ligero de Documentación Académica | <sub>Gestión del ciclo de vida completo de las guías docentes universitarias: redacción, revisión y aprobación con flujo de estados controlado, planificación por sesiones, ponderaciones de evaluación y bibliografía sobre un catálogo institucional validado contra memorias ANECA.</sub> |
| [MERITOS](docs/MERITOS/README.md) | Modelo de Expedientes y Registros de la Información de Trayectoria Ocupacional | <sub>Repositorio de los datos que aporta el profesor sobre su actividad investigadora y académica: publicaciones, cargos, acreditaciones, sexenios y quinquenios. Complementa el perfil docente de CELDA sin pervertir su propósito principal.</sub> |
| [PRISMA](docs/PRISMA/README.md) | Plataforma de Representación e Integración de Señales y Métricas Académicas | <sub>Agregador de indicadores extraídos de todo el ecosistema en un único lugar. Lee sin escribir: consume datos de CELDA y del resto de satélites para ofrecer métricas institucionales de calidad académica sin contaminar ninguno de los sistemas fuente.</sub> |
| [PULSO](docs/PULSO/README.md) | Plataforma Unificada de Levantamiento y Seguimiento de Opiniones | <sub>Gestor de encuestas docentes. Permite diseñar, distribuir y agregar encuestas dirigidas a alumnos, profesores o directores de programa, con seguimiento longitudinal por asignatura y curso académico. Primera versión con código de invitación por asignatura, sin censo de alumnos.</sub> |
| [VITRINA](docs/VITRINA/README.md) | Visibilizador Institucional de Titulaciones, Rubros e Información Normalizada Académica | <sub>Portal público de consulta de las guías docentes aprobadas y vigentes del curso académico activo. Acceso sin autenticación, orientado a alumnos, futuros alumnos y organismos externos como ANECA.</sub> |
| [CITA](docs/CITA/README.md) | Catálogo e Indexador de Textos Académicos | <sub>Catálogo institucional de referencias bibliográficas correctamente especificadas. CELDA se apoya en él para encontrar o construir una referencia y la copia en la guía; las referencias nuevas quedan almacenadas para su reutilización por CELDA o cualquier otro consumidor.</sub> |
| [CARGA](docs/CARGA/README.md) | Control y Análisis del Reparto y Gestión de Actividades | <sub>Detector de sobrecarga de evaluación a partir de la planificación docente real de CELDA. Infiere en qué semana cae cada sesión de evaluación (tipo y orden de la sesión y sesiones por semana según su planificación, inicio común de todas las asignaturas) para identificar acumulación de evaluaciones en una misma semana.</sub> |
| [ASISTE](docs/ASISTE/README.md) | Aplicación de Seguimiento e Informe Sobre la Trazabilidad de Eventos | <sub>Fedatario de asistencia a eventos académicos: clases, tutorías, seminarios. Registra quién asistió a cada evento, asociado opcionalmente a una sesión de la planificación de CELDA, y responde a quien pregunte (¿ha asistido el alumno X a todas las sesiones de la asignatura Y?), sin aplicar reglas académicas ni validar matrícula. Parte del esbozo existente en pySesion.</sub> |
| [FEDATARIO](docs/FEDATARIO/README.md) |  | <sub>Generación del documento oficial personalizado por alumno con las guías docentes de su itinerario real. Dado que CELDA conserva las guías aprobadas año a año y el ERP de la universidad suministra el itinerario real de cada alumno (qué asignatura cursó en qué curso), puede compilar un libro certificado de su trayectoria académica. Servicio de pago.</sub> |
| [SIGHOR](docs/SIGHOR/README.md) | Sistema de Gestión de Horarios | <sub>Gestor de horarios académicos, reingeniería de SigHor (1998): asignación de aulas, franjas y grupos para cada asignatura del programa. Dueño de los grupos.</sub> |
| [ACTIVITAT](docs/ACTIVITAT/README.md) |  | <sub>Gestor de actividad docente del profesorado. A partir de los datos de impartición registrados en CELDA (quién impartió qué asignatura en qué curso), genera el informe anual de carga docente que cada profesor entrega a secretaría académica.</sub> |

## ¿Y ahora qué?

- **Roadmap**: reajustar las fases con las dependencias y la complejidad revisadas ([#9](https://github.com/mmasias/_ECOSISTEMA_CELDA/issues/9)).
- **Frontera con el ERP**: qué parte del catálogo viene del ERP y cuál define CELDA; semana de inicio de cada semestre en el calendario de CELDA ([#10](https://github.com/mmasias/_ECOSISTEMA_CELDA/issues/10)).
- **Mapa de endpoints**: qué expone CELDA a cada satélite, una vez reajustado el roadmap ([#12](https://github.com/mmasias/_ECOSISTEMA_CELDA/issues/12)).
- Razonamiento detrás de las decisiones de este mapa: [discussion #1](https://github.com/mmasias/_ECOSISTEMA_CELDA/discussions/1).
