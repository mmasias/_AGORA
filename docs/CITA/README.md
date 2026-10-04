# CITA

## ¿Por qué?

En CELDA, cada referencia bibliográfica se teclea dentro de la guía que la contiene. El mismo libro puede aparecer en 40 guías con 40 formatos distintos: con ISBN o sin él, con el año entre paréntesis o sin paréntesis, con el nombre del autor abreviado o completo. Cada profesor vuelve a construir una referencia que otro ya construyó, y la calidad de la cita depende de quién la escribe.

CITA resuelve ese problema en origen: un catálogo institucional de referencias correctamente especificadas, en el que CELDA se apoya para encontrar o construir la referencia antes de guardarla en la guía.

## ¿Qué?

Un servicio contenedor de referencias bibliográficas correctamente especificadas. CELDA consulta el catálogo para encontrar una referencia existente o construir una nueva con los campos correctos, y **copia** la referencia en la guía, donde queda guardada como hasta ahora. Si la referencia construida no existía en el catálogo, CITA la almacena para que CELDA o cualquier otro consumidor la reutilice.

CITA no reescribe guías: la guía conserva su copia. Corregir una referencia en el catálogo mejora las guías que la incorporen a partir de ese momento, no las ya redactadas o aprobadas, coherente con el principio de CELDA de que un cambio en un nivel superior no altera retroactivamente lo materializado en una guía.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Profesor | Añade bibliografía a su guía eligiendo de un catálogo o construyendo la referencia con un formulario estructurado, sin teclear el formato a mano |
| Biblioteca | Visibilidad sobre qué recursos bibliográficos se usan realmente en la docencia, y en qué programas |
| Gabinete de calidad | Referencias bien especificadas y verificables para los informes de acreditación |
| CELDA | Bibliografía de calidad homogénea sin cambiar su modelo: la guía sigue guardando su propia copia |
| Otros consumidores | Un catálogo institucional reutilizable fuera de las guías docentes |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟢 Baja-media | Un catálogo con búsqueda y alta estructurada. La deduplicación sigue siendo necesaria al dar de alta (¿esta referencia ya existe?), pero no hay que reconciliar a posteriori las referencias ya guardadas en las guías. |
| Complejidad de dominio | 🟡 Media | El dominio bibliográfico tiene estándares consolidados (BibTeX, Citation Style Language, DOI, ISBN); el reto es adaptarlos a las necesidades reales de la institución sin sobre-ingeniería. |
| Dependencias | 🟢 Baja | CELDA consulta y copia; CITA no depende de CELDA para funcionar ni CELDA de CITA para renderizar sus guías. El cambio en CELDA se limita a que el alta de una referencia pase por el catálogo. |
| **Índice combinado** | 🟢 **Baja-media** | Sin migración obligatoria de datos en producción: las referencias ya guardadas en las guías siguen siendo válidas. La carga inicial del catálogo a partir de ellas es opcional. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Cuál es el criterio de deduplicación al dar de alta?** Mismo DOI (artículos), mismo ISBN (libros), o... ¿qué criterio para los demás casos? Determina cuándo CITA ofrece una referencia existente en vez de crear una nueva.
- **¿Se siembra el catálogo con las referencias ya guardadas en CELDA?** Hacerlo da un catálogo útil desde el primer día pero exige reconciliar casos ambiguos; empezar vacío es más limpio y el catálogo crece con el uso.
- **¿Se integra con bases externas?** Un DOI resuelve automáticamente los metadatos de un artículo. Integrar con CrossRef o similar elimina el trabajo manual de alta.
- **¿Qué se copia en la guía?** La cita formateada, los campos estructurados, o ambos más el identificador de origen en CITA (útil para estadísticas de uso sin crear dependencia de renderizado).

### Cómo abordarlo

1. Construir el catálogo con búsqueda y alta estructurada.
2. Integrar en el alta de referencias de CELDA: buscar en CITA, elegir o construir, copiar en la guía.
3. Integración con DOI/CrossRef como primera extensión para facilitar el alta.
4. Opcional: sembrar el catálogo con las referencias ya existentes en CELDA, con revisión manual de los casos ambiguos.

Relación con el backlog de CELDA: la normalización de la bibliografía planteada allí (catálogo compartido de referencias) se resuelve consumiendo CITA, sin cambiar el modelo de la guía.
