# VITRINA

## ¿Por qué?

CELDA gestiona el ciclo de vida completo de las guías docentes: redacción, revisión y aprobación. Pero una vez aprobadas, esas guías no tienen salida pública. Un alumno que quiere saber qué se enseña en una asignatura antes de matricularse, o un organismo externo como ANECA que audita la titulación, no tienen ningún punto de acceso sin pasar por la institución.

El portal de consulta pública cierra ese hueco: convierte el trabajo ya hecho en CELDA en algo visible hacia fuera, sin que nadie tenga que exportar, publicar manualmente ni mantener una versión paralela de los documentos.

## ¿Qué?

Un portal de solo lectura que expone las guías docentes aprobadas y vigentes del curso académico activo, organizadas por titulación y asignatura. Sin autenticación, sin escritura, sin lógica de negocio propia.

La fuente de datos es CELDA, consumida via API. VITRINA no tiene base de datos propia: lee, renderiza y sirve.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Alumno prospecto | Consulta el temario, evaluación y bibliografía de cualquier asignatura antes de matricularse |
| Alumno matriculado | Accede a la guía oficial de sus asignaturas sin necesidad de cuenta ni contraseña |
| ANECA / organismo externo | Comprueba la coherencia y vigencia de las guías de un programa en un momento dado |
| Institución | Cumple con la obligación de publicidad activa de las guías docentes sin infraestructura adicional |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟢 Baja | Consumidor puro de la API de CELDA. Sin base de datos propia, sin escritura, sin autenticación. El mayor reto técnico es el render fiel de la guía, que CELDA ya resuelve con su plantilla Jinja2/WeasyPrint. |
| Complejidad de dominio | 🟢 Baja | El dominio es exactamente el de CELDA, sin extensión. No introduce entidades nuevas ni reglas de negocio propias. |
| Dependencias | 🟡 Media | Dependencia directa y única de la API de CELDA. Si CELDA no expone los endpoints necesarios (guías aprobadas por curso activo, filtradas por programa), VITRINA no puede construirse. El trabajo previo está en CELDA, no en VITRINA. |
| **Índice combinado** | 🟢 **Baja** | El proyecto más sencillo del roadmap. La dificultad real no está en construirlo sino en decidir qué expone y qué no: qué campos de la guía son públicos, si se muestra el nombre del profesor, cómo se maneja una guía que se revoca después de publicada. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Qué campos de la guía son públicos?** El temario y las ponderaciones de evaluación son obvios. El nombre del profesor puede tener implicaciones de privacidad según la institución.
- **¿Qué ocurre si una guía aprobada se revoca?** VITRINA debería dejar de mostrarla o marcarla como retirada, no servir una versión obsoleta.
- **¿Se genera HTML estático o es una app dinámica?** Una generación periódica de HTML estático (cada vez que se aprueba una guía) elimina la dependencia en tiempo real con CELDA y hace el portal más robusto. Una app dinámica es más simple de construir pero introduce una dependencia de disponibilidad.
- **¿Se necesita un motor de búsqueda?** Para una institución con 16 programas y ~800 asignaturas, una búsqueda básica por nombre o código es suficiente. No se necesita indexación compleja.

### Cómo abordarlo

1. Definir con la institución qué campos son públicos y cuál es la política ante revocaciones.
2. Extender la API de CELDA con los endpoints necesarios: guías aprobadas del curso activo, filtradas por programa, con solo los campos públicos.
3. Decidir arquitectura: estático generado vs. app dinámica. Recomendación: estático generado, regenerado automáticamente via webhook cuando CELDA aprueba una guía.
4. Construir el portal como aplicación de una sola página o sitio estático, sin autenticación, sin estado.
