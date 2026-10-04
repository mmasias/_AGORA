# MERITOS

## ¿Por qué?

CELDA sabe quién imparte qué asignatura y en qué curso académico. Pero el perfil académico de un profesor - sus publicaciones, acreditaciones, sexenios, quinquenios, cargos institucionales - no tiene cabida natural en un sistema de guías docentes sin pervertir su propósito.

Sin embargo, esa información existe y necesita un lugar: los procesos de acreditación ANECA, las memorias de titulación y los informes de calidad la exigen periódicamente.

Durante el desarrollo de CELDA, esa presión ganó la partida: a petición del gabinete de calidad, el perfil del profesor acabó implantándose dentro del propio CELDA - perfil por profesor y curso académico, con ORCID, CVN, doctorado, acreditación, sexenios y quinquenios, validación y histórico anual. La necesidad era real y el módulo funciona en producción, pero el lugar es equivocado: cada curso que pasa, el perfil investigador engorda un sistema cuya razón de ser son las guías docentes.

MERITOS corrige el desvío: extrae ese módulo de CELDA a un repositorio propio, sin perder la historización por curso, y lo extiende con lo que todavía no tiene cabida en ninguna parte: publicaciones estructuradas y cargos institucionales.

## ¿Qué?

Un repositorio personal por profesor donde cada uno vuelca y mantiene su trayectoria académica e investigadora. No nace de cero: nace de la extracción del módulo de perfil que hoy vive dentro de CELDA, del que conserva el modelo de datos ya validado con el gabinete de calidad. Tampoco termina en la extracción: lo amplía con publicaciones y cargos. No es un CV libre - es un formulario estructurado que facilita la extracción de datos para procesos institucionales.

MERITOS no gestiona al profesor: gestiona los datos que el profesor aporta. La identidad del profesor la gestiona la universidad, fuera de todos los servicios del mapa; CELDA solo asocia su `email` con sus roles. El profesor de CELDA y el de MERITOS son el mismo: mismo `email`, misma persona. Un profesor es profesor con independencia de las asignaturas que imparta, así que ninguna de las dos aplicaciones necesita a la otra para saber quién es: cada una autentica por su cuenta, con su propio OAuth contra `email`.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Profesor | Un único lugar donde mantener su trayectoria, sin rellenar el mismo dato en tres sistemas distintos |
| Gabinete de calidad | Datos estructurados para memorias e informes de acreditación, con la continuidad del histórico anual que hoy se captura dentro de CELDA |
| Dirección académica | Visión del claustro: quién está acreditado, quién tiene sexenios activos, qué perfil investigador tiene cada área |
| CELDA | Vuelve a ser solo un sistema de guías docentes: el perfil del profesor deja de ser ámbito prestado |
| PRISMA | Indicadores de actividad investigadora y docente del claustro, listos para agregar |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟡 Media | No es un CRUD verde: es una extracción con corte sobre un sistema en producción. Servicio propio, migración del histórico por curso sin perder el estado de validación, y retirada del módulo en CELDA en el mismo movimiento. Las costuras son limpias (una tabla, un formulario, sin entrelazado con las guías), lo que evita que suba más. |
| Complejidad de dominio | 🟡 Media | La mitad del dominio ya está modelada y validada en producción con el gabinete de calidad: perfil por curso, doctorado, acreditación, sexenios, quinquenios. La incertidumbre se concentra en lo no modelado: publicaciones y cargos, con los criterios de acreditación cambiantes y específicos por área como telón de fondo. |
| Dependencias | 🟡 Media | Coordinación bidireccional con CELDA durante el corte (migración + retirada del módulo). Tras el corte no queda dependencia de runtime con CELDA: la autenticación es OAuth propio contra `email` en cada aplicación del ecosistema, y el profesor es una entidad autónoma, no derivada de las asignaturas que imparte. Lo que queda entre las dos es al revés: CELDA consultando la breve biografía a MERITOS, para la guía docente. PRISMA depende de MERITOS para los indicadores de claustro. |
| **Índice combinado** | 🟡 **Media** | Migrar datos validados en producción es riesgo real, pero acotado: módulo aislado, modelo ya estabilizado y una sola contraparte (CELDA). La secuencia importa: MERITOS antes de PRISMA. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Corte único o doble escritura temporal?** Migrar en un solo despliegue concentra el riesgo pero cierra la puerta de golpe; mantener doble escritura durante un curso de transición es más seguro pero conserva el módulo vivo en los dos sitios a la vez.
- **¿Qué pasa con la condición de paso de CELDA? (decidido)** Hoy CELDA no deja acceder a las guías mientras el perfil del curso activo no esté validado. Esa condición sale con el módulo: tras la extracción, comprobar el perfil validado deja de ser responsabilidad de CELDA, y CELDA no consulta a MERITOS para ello.
- **¿Conserva CELDA lectura de algo del perfil tras la extracción?** Hoy lo consumen MiPerfil y la vista de profesores; el PDF de la guía no lo toca (renderiza los nombres del profesorado de la propia guía). **Decidido: sí, la breve biografía del profesor**, que la guía docente va a mostrar. Tras la extracción, CELDA la consulta a MERITOS, no al revés. Como el perfil está historizado por curso, la consulta se hace por (profesor, curso académico): una guía de un curso pasado muestra la biografía que el profesor tenía cuando la impartió.
- **¿Dónde vive la historización por curso?** El contrato `(Profesor, CursoAcademico)` se lleva tal cual: el curso académico es convención institucional compartida por todas las aplicaciones, no una consulta a CELDA. Queda por fijar cómo se alinea el corte de curso entre aplicaciones sin que ninguna mande sobre las demás.
- **¿Cómo se modelan las publicaciones?** Campos estructurados (DOI, revista, año, índice de impacto) permiten cruzar con bases externas (ORCID, Scopus) e importar desde ahí; texto libre es más fácil de rellenar. Primera extensión natural una vez extraído el perfil.

### Cómo abordarlo

1. Inventariar el módulo en CELDA (tabla, formulario, validación) y cerrar el inventario de consumidores: qué queda en CELDA leyendo el perfil y bajo qué contrato.
2. Construir MERITOS con el modelo de perfil actual tal cual - ya está validado con el gabinete de calidad - y la identidad resuelta.
3. Ejecutar la extracción como un solo movimiento: migración del histórico con su estado de validación intacto y retirada del módulo en CELDA en el mismo despliegue, incluida la condición de paso del perfil validado.
4. Extender con publicaciones y cargos una vez el perfil extraído esté estable; ORCID como primera integración.
5. Exponer vía API los datos que PRISMA necesita agregar.
