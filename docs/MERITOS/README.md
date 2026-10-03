# MERITOS

## ¿Por qué?

CELDA sabe quién imparte qué asignatura y en qué curso académico. Pero el perfil académico de un profesor — sus publicaciones, acreditaciones, sexenios, quinquenios, cargos institucionales — no tiene cabida natural en un sistema de guías docentes sin pervertir su propósito.

Sin embargo, esa información existe y necesita un lugar: los procesos de acreditación ANECA, las memorias de titulación y los informes de calidad la exigen periódicamente. Hoy vive dispersa en CVs en Word, formularios de Google o sistemas externos desconectados del resto del ecosistema.

MERITOS centraliza esa información en un repositorio propio, sin contaminar CELDA, y la mantiene vinculada al mismo profesor que ya existe en el ecosistema.

## ¿Qué?

Un repositorio personal por profesor donde cada uno vuelca y mantiene su trayectoria académica e investigadora: publicaciones, cargos, acreditaciones, sexenios y quinquenios. No es un CV libre — es un formulario estructurado que facilita la extracción de datos para procesos institucionales.

El profesor de CELDA y el de MERITOS son el mismo: mismo `email`, mismo identificador. MERITOS extiende el perfil sin duplicar la identidad.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Profesor | Un único lugar donde mantener su trayectoria, sin rellenar el mismo dato en tres sistemas distintos |
| Gabinete de calidad | Datos estructurados para memorias de titulación e informes de acreditación, sin perseguir a cada profesor por email |
| PRISMA | Indicadores de actividad investigadora y docente del claustro, listos para agregar |
| Dirección académica | Visión del claustro: quién está acreditado, quién tiene sexenios activos, qué perfil investigador tiene cada área |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟢 Baja | CRUD estructurado sobre un modelo de datos relativamente plano. Sin flujos de aprobación, sin estados complejos. La autenticación reutiliza el mismo mecanismo OAuth de CELDA. |
| Complejidad de dominio | 🟡 Media | Los criterios de acreditación (ANECA, ACADEMIA, CNEAI) son cambiantes y específicos por área de conocimiento. El modelo debe ser lo suficientemente flexible para adaptarse sin convertirse en texto libre inútil. |
| Dependencias | 🟡 Media | Depende de CELDA para la identidad del profesor. PRISMA depende de MERITOS para los indicadores de claustro. La secuencia importa: MERITOS antes de PRISMA. |
| **Índice combinado** | 🟡 **Media-baja** | Técnicamente simple, pero el modelado del dominio requiere trabajo previo de definición con el gabinete de calidad para evitar construir un formulario que nadie rellene o que no sirva para los procesos reales. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Qué campos son obligatorios y cuáles opcionales?** Un formulario demasiado exigente no se rellena. Uno demasiado laxo no es útil para acreditación.
- **¿Cómo se modelan las publicaciones?** Texto libre por entrada vs. campos estructurados (DOI, revista, año, índice de impacto). La segunda opción permite cruzar con bases externas (ORCID, Scopus); la primera es más fácil de rellenar.
- **¿Quién valida los datos?** En CELDA el director aprueba la guía. En MERITOS no hay un validador natural — o se confía en el autoservicio del profesor, o se introduce un rol de validación que no existe hoy.
- **¿Cómo se integra con ORCID?** Si el profesor tiene ORCID, importar sus publicaciones desde ahí elimina el trabajo manual más tedioso.

### Cómo abordarlo

1. Sesión de definición con el gabinete de calidad: qué datos necesitan exactamente, en qué formato y para qué procesos concretos.
2. Modelar el dominio a partir de esa sesión, no antes. El riesgo es construir campos que nadie usa.
3. Integración con ORCID como primera extensión natural, una vez que el modelo básico esté validado con datos reales.
4. Exponer vía API los datos que PRISMA necesita agregar.
