# PRISMA

## ¿Por qué?

Cada satélite del ecosistema genera datos valiosos de forma aislada: CELDA sabe el estado de las guías, PULSO recoge la opinión de los alumnos, MERITOS tiene el perfil investigador del claustro, ASISTE registra la asistencia real. Pero ninguno de ellos tiene una visión de conjunto.

El gabinete de calidad necesita exactamente esa visión: ¿qué porcentaje de guías están aprobadas a tiempo? ¿Cuál es la satisfacción media por titulación? ¿Qué áreas tienen mayor brecha entre asistencia planificada y real? Hoy esas preguntas se responden con hojas de cálculo ensambladas a mano de fuentes distintas.

PRISMA agrega esos indicadores en un único lugar, sin tocar ninguno de los sistemas fuente.

## ¿Qué?

Un agregador de indicadores de calidad académica que lee datos de todo el ecosistema y los presenta en dashboards institucionales. Lee sin escribir: no tiene base de datos propia de entidades de negocio, solo métricas derivadas y cachés de consulta.

Un prisma descompone la luz en sus componentes. PRISMA descompone los datos del ecosistema en indicadores visibles.

La primera versión solo necesita CELDA (estado y plazos de las guías). El resto llega a medida que existen sus fuentes:

| Indicador | Fuente | Nota |
|---|---|---|
| Estado y plazos de las guías, rechazos | CELDA | Primera versión |
| Indicadores académicos (tasas de rendimiento, éxito, abandono, graduación) | ERP de la universidad | Los que exige ANECA; matrícula y notas |
| Satisfacción docente | PULSO | Solo resultados agregados por encima del umbral de participación |
| Asistencia y alumnos atendidos, también en tutorías | ASISTE | |
| Cumplimiento: planificado frente a impartido | ASISTE | Calculado sobre los eventos asociados a la planificación; la tasa de asociación es en sí un indicador |
| Carga docente, incluidas las horas de tutoría | ACTIVITAT | |
| Equilibrio de carga evaluativa | CARGA | |
| Perfil del claustro (acreditación, sexenios) | MERITOS | |

PRISMA lee el resultado de CARGA y de ACTIVITAT, no lo recalcula: la regla (qué es sobrecarga, cómo se computa una hora docente) tiene un único dueño, y si PRISMA la repitiera habría dos definiciones que podrían divergir.

## ¿Para qué?

| Audiencia | Qué obtiene |
|---|---|
| Gabinete de calidad | Indicadores listos para memorias de acreditación sin ensamblar datos a mano |
| Dirección académica | Visión transversal del estado docente de la institución en tiempo real |
| Directores de programa | Comparativa de su titulación frente al resto de la institución |
| ANECA | Evidencias cuantitativas del sistema de garantía de calidad |

## ¿Cómo?

### Análisis de complejidad

<div align=center>

| Dimensión | Nivel | Justificación |
|---|:-:|---|
| Complejidad técnica | 🟡 Media | Agregar datos de múltiples APIs con modelos distintos requiere una capa de normalización. El reto no es la lógica de cada indicador sino mantener la coherencia cuando cualquiera de los sistemas fuente cambia su API. |
| Complejidad de dominio | 🔴 Alta | Definir qué es un indicador de calidad académica es una decisión institucional, no técnica. Los criterios de ANECA cambian, los pesos de cada indicador son negociables y hay riesgo real de construir métricas que nadie usa o que se interpretan mal. |
| Dependencias | 🔴 Alta | Es el proyecto con más dependencias del roadmap. PRISMA no puede mostrar indicadores de encuestas si PULSO no existe, ni de asistencia si no existe ASISTE. Cada satélite que se construye amplía lo que PRISMA puede ofrecer, pero también lo que puede romper. |
| **Índice combinado** | 🔴 **Alta** | La complejidad no está en el código sino en la coordinación: depende de que los demás satélites estén construidos, expuestos via API y estables. Es el último proyecto que debería construirse, no el primero. |

</div>

### Decisiones de diseño a tomar antes de construir

- **¿Qué indicadores son prioritarios?** No todos los datos del ecosistema generan métricas útiles. La primera versión debe cubrir los indicadores que el gabinete de calidad necesita para un informe real, no todos los que son técnicamente posibles.
- **¿Cómo se cachean los datos?** Consultar en tiempo real todas las APIs del ecosistema para cada carga de dashboard no escala. Se necesita una estrategia de caché o un proceso de ETL periódico.
- **¿Cómo se gestiona la indisponibilidad de un satélite?** Si PULSO está caído, PRISMA debe degradarse con gracia, no fallar.
- **¿Quién define los umbrales?** Un indicador de "porcentaje de guías aprobadas" necesita un umbral para ser rojo/amarillo/verde. Ese umbral es una decisión institucional, no técnica.

### Cómo abordarlo

1. Construir primero los satélites fuente (al menos CELDA, que ya existe, y uno o dos más).
2. Definir con el gabinete de calidad los 5-10 indicadores prioritarios para el primer informe real.
3. Construir PRISMA como agregador de esos indicadores concretos, con arquitectura extensible para añadir más fuentes.
4. Estrategia de caché desde el primer día: los dashboards de calidad no necesitan datos en tiempo real, pero sí datos frescos (actualización diaria o por evento).
