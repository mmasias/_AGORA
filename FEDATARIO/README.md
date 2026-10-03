# FEDATARIO

## ¿Por qué?

Un alumno que termina su titulación cursó cada asignatura en un curso académico concreto, con una guía docente concreta aprobada para ese año. Ese documento — la guía docente oficial que rigió su formación en esa asignatura ese año — es la evidencia académica de lo que aprendió y cómo fue evaluado.

Hoy, obtener ese documento requiere que secretaría académica localice manualmente las guías aprobadas de cada asignatura en el año en que el alumno la cursó. Con 16 programas, décadas de historial y cientos de alumnos, ese proceso no escala.

CELDA conserva las guías aprobadas año a año y sabe, a través del catálogo, qué asignatura existía en qué programa en qué curso. FEDATARIO usa esa información para compilar automáticamente el libro certificado de la trayectoria académica de un alumno.

## ¿Qué?

Un servicio de generación de documentos oficiales personalizados: dado un alumno y su itinerario real (qué asignaturas cursó, en qué programa y en qué curso académico), FEDATARIO compila las guías docentes aprobadas correspondientes en un único documento certificado.

Es un servicio de valor añadido sobre el ecosistema existente, orientado a alumnos que necesitan acreditar su formación ante empleadores, organismos de homologación o programas de posgrado.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Alumno graduado | Documento oficial con las guías docentes de su itinerario real, certificado por la institución |
| Empleador / organismo externo | Evidencia detallada y verificable de la formación recibida por el candidato |
| Organismos de homologación | Documentación estándar para reconocimiento de títulos |
| Institución | Fuente de ingresos adicional (servicio de pago) sin infraestructura nueva |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟡 Media | La generación del documento en sí reutiliza el render de CELDA (Jinja2/WeasyPrint). El reto técnico está en la firma digital y la verificación de autenticidad del documento generado. |
| Complejidad de dominio | 🔴 Alta | Introduce la entidad **Alumno** y su relación con asignaturas y cursos académicos, que no existe en ningún punto del ecosistema actual. Ese modelo de datos no está definido, no hay datos históricos y depende de sistemas externos (el SGA de la universidad) para obtener el itinerario real de cada alumno. |
| Dependencias | 🔴 Alta | Depende de CELDA para el historial de guías aprobadas. Depende de un sistema externo (SGA, Secretaría académica) para el itinerario real del alumno. Esa integración externa es la dependencia más incierta del roadmap. |
| **Índice combinado** | 🔴 **Alta** | El proyecto con más trabajo de cero: introduce entidades nuevas, depende de integración externa y tiene requisitos de autenticidad documental que ningún otro satélite tiene. El modelo de negocio (servicio de pago) añade además requisitos de gestión que el ecosistema actual no tiene. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Cómo llega el itinerario del alumno al sistema?** ¿Se importa del SGA vía API, se introduce manualmente por secretaría, o el propio alumno lo declara y secretaría lo certifica? Cada opción tiene implicaciones de fiabilidad y carga operativa muy distintas.
- **¿Cómo se certifica la autenticidad del documento?** Un PDF con firma digital verificable, un código QR que enlaza a una versión en línea comprobable, o un sello institucional tradicional. La elección tiene implicaciones legales.
- **¿Qué ocurre si una guía del itinerario fue revocada o corregida después?** CELDA mantiene el historial, pero ¿qué versión de la guía se incluye en el documento: la vigente al finalizar el curso o la última aprobada?
- **¿Cómo se gestiona el modelo de pago?** Pago por documento, por alumno, suscripción institucional. La decisión de negocio condiciona la arquitectura del servicio.

### Cómo abordarlo

1. Resolver primero el modelo de datos del Alumno y su itinerario, en colaboración con secretaría académica. Sin eso, no hay proyecto.
2. Definir el mecanismo de autenticidad documental con el área jurídica de la institución antes de construir nada.
3. Construir el generador de documentos reutilizando el render de CELDA como primera prueba de concepto, con datos introducidos manualmente.
4. Integrar con el SGA como segunda fase, una vez validado el documento generado con secretaría académica.
