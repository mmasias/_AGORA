# CITA

## ¿Por qué?

En CELDA, cada referencia bibliográfica vive como texto libre dentro de la guía que la contiene. Eso significa que el mismo libro puede aparecer en 40 guías con 40 formatos distintos: con ISBN o sin él, con el año entre paréntesis o sin paréntesis, con el nombre del autor abreviado o completo. No hay forma de saber que son la misma referencia.

La consecuencia práctica es que corregir un error en una referencia (una edición incorrecta, una URL caducada) exige corregirla en cada una de las guías que la usa, una a una, manualmente.

CITA resuelve ese problema estructural: un catálogo centralizado de referencias bibliográficas donde cada entrada existe una sola vez, y las guías apuntan a esa entrada en vez de copiarla.

## ¿Qué?

Un normalizador de referencias bibliográficas institucional. Centraliza el catálogo, deduplica por cita normalizada y permite que una corrección se propague a todas las guías que usan esa referencia. Las guías de CELDA pasan de contener copias independientes de texto libre a contener punteros a entradas del catálogo.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Profesor | Añade bibliografía a su guía eligiendo de un catálogo, sin teclear manualmente. Una corrección a la referencia la hace una vez y se propaga sola. |
| Biblioteca | Visibilidad sobre qué recursos bibliográficos se usan realmente en la docencia, y en qué programas |
| Gabinete de calidad | Referencias normalizadas y verificables para los informes de acreditación |
| Institución | Catálogo bibliográfico institucional que refleja la realidad docente, no una lista de deseos |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟡 Media | La normalización y deduplicación de referencias bibliográficas es un problema resuelto en biblioteconomía, pero implementarlo correctamente (¿cuándo son la misma referencia dos entradas con formato distinto?) requiere decisiones de diseño no triviales. |
| Complejidad de dominio | 🟡 Media | El dominio bibliográfico tiene estándares consolidados (BibTeX, Citation Style Language, DOI, ISBN), pero adaptarlos a las necesidades reales de una institución pequeña sin sobre-ingeniería es el reto principal. |
| Dependencias | 🔴 Alta | CITA implica una migración no trivial de CELDA: las referencias que hoy son texto libre en cada guía tienen que pasar a ser punteros al catálogo. Esa migración afecta datos en producción y requiere reconciliación manual para los casos ambiguos. |
| **Índice combinado** | 🟡 **Media-alta** | El proyecto más disruptivo para CELDA de toda la Fase II. No es difícil de construir, pero la migración de datos existentes es la operación más arriesgada del roadmap: 775 guías con referencias en texto libre que hay que reconciliar contra un catálogo nuevo. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Cuál es el criterio de deduplicación?** Dos entradas son la misma referencia si tienen el mismo DOI (para artículos), el mismo ISBN (para libros), o... ¿qué criterio para los demás casos? La respuesta determina el algoritmo de reconciliación.
- **¿Se migran las referencias existentes o se empieza de cero?** Empezar de cero es más limpio pero deja las 775 guías existentes con referencias en formato antiguo. Migrar es más seguro para la continuidad pero exige reconciliación.
- **¿Se integra con bases externas?** Un DOI resuelve automáticamente todos los metadatos de un artículo. Integrar con CrossRef o similar elimina el trabajo manual de entrada de datos.
- **¿Cómo se modela la relación entre guía y referencia?** Una guía referencia una entrada del catálogo con un tipo (básica, complementaria, web) y potencialmente con una nota propia. La clase de asociación `(Guia, Referencia)` necesita esos atributos adicionales.

### Cómo abordarlo

1. Construir el catálogo y su CRUD antes de tocar CELDA.
2. Escribir el script de reconciliación como proceso separado, con revisión manual de los casos ambiguos, antes de aplicarlo en producción.
3. Extender la API de CELDA para que las referencias apunten al catálogo de CITA en vez de ser texto libre.
4. Integración con DOI/CrossRef como primera extensión para facilitar el alta de nuevas referencias.
