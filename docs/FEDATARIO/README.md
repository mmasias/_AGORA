# FEDATARIO

## ¿Por qué?

Un alumno que termina su titulación cursó cada asignatura en un curso académico concreto, con una guía docente concreta aprobada para ese año. Ese documento - la guía docente oficial que rigió su formación en esa asignatura ese año - es la evidencia académica de lo que aprendió y cómo fue evaluado.

Hoy, obtener ese documento requiere que secretaría académica localice manualmente las guías aprobadas de cada asignatura en el año en que el alumno la cursó. Con 16 programas, un histórico que crece cada curso y cientos de alumnos, ese proceso no escala.

CELDA conserva las guías aprobadas año a año y sabe, a través del catálogo, qué asignatura existía en qué programa en qué curso. Horizonte real: CELDA tiene guías desde el curso 2026-27, así que el primer itinerario completo de un grado de cuatro años es certificable hacia 2030; los itinerarios anteriores no tienen guías en CELDA. FEDATARIO usa esa información para compilar automáticamente el libro certificado de la trayectoria académica de un alumno.

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
| Complejidad de dominio | 🔴 Alta | No modela el censo de alumnos - el itinerario real se consulta al ERP de la universidad (GUIAA o AGORA) por curso académico y asignatura - pero sí tiene que congelarlo: un documento certificado no puede sellarse contra una consulta viva, así que FEDATARIO persiste su propio snapshot del itinerario en el momento de la certificación, con firma digital y verificación de autenticidad. |
| Dependencias | 🔴 Alta | Depende de CELDA para el historial de guías aprobadas. Depende del ERP de la universidad (GUIAA o AGORA) para el itinerario real de cada alumno: un contrato de integración con un sistema que el proyecto no controla, la dependencia más incierta del roadmap. |
| **Índice combinado** | 🔴 **Alta** | El proyecto con más trabajo de cero: introduce entidades nuevas, depende de integración externa y tiene requisitos de autenticidad documental que ningún otro satélite tiene. El modelo de negocio (servicio de pago) añade además requisitos de gestión que el ecosistema actual no tiene. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Cómo llega el itinerario del alumno al sistema?** El ERP de la universidad lo suministra por consulta (qué alumno cursó qué asignatura en qué programa y curso). La decisión restante es cuándo se congela: el snapshot que se certifica debe capturarse en el momento de la generación y quedar inmutable, sin reconsultas posteriores.
- **¿Cómo se certifica la autenticidad del documento?** Un PDF con firma digital verificable, un código QR que enlaza a una versión en línea comprobable, o un sello institucional tradicional. La elección tiene implicaciones legales.
- **¿Qué versión de la guía se incluye? (decidido en CELDA)** El contenido de la guía es propio de cada guía y no cambia retroactivamente. Los resultados de aprendizaje, los requisitos previos y las actividades formativas se leen vigentes por diseño: cambiarlos es excepcional, y si se cambian es porque siempre debieron ser así -- las guías anteriores eran las erróneas. CELDA no archiva el PDF: lo genera en cada petición con la plantilla vigente. Si el documento certificado debe ser inmutable, el congelado es responsabilidad de FEDATARIO (su snapshot del itinerario y el PDF que emite), no de CELDA.
- **¿Cómo se gestiona el modelo de pago?** Pago por documento, por alumno, suscripción institucional. La decisión de negocio condiciona la arquitectura del servicio.

### Cómo abordarlo

1. Resolver primero el contrato con el ERP de la universidad (consulta del itinerario) y el formato del snapshot certificado, en colaboración con secretaría académica. Sin eso, no hay proyecto.
2. Definir el mecanismo de autenticidad documental con el área jurídica de la institución antes de construir nada.
3. Construir el generador de documentos reutilizando el render de CELDA como primera prueba de concepto, con itinerarios introducidos manualmente.
4. Integrar con el ERP de la universidad como segunda fase, una vez validado el documento generado con secretaría académica.
