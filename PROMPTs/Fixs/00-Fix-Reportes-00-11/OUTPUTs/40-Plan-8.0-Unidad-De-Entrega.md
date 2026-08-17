# Plan de aplicación 8.0 — El nivel de unidad de entrega

**Documento:** 40-Plan-8.0-Unidad-De-Entrega.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Insumo:** `15-Modelo-De-Dos-Ejes-Y-Unidad-De-Entrega.md`
**Estado:** Vigente
**Versión resultante del framework:** SDD 8.0 (major)

---

## 1. Qué resuelve, en una línea

El framework tiene dos niveles —producto y proyecto de código— y los productos reales tienen tres. El
nivel intermedio se puebla hoy con proyectos de código, y las once categorías que cuelgan de él
producen artefactos que no son de ese nivel. Es el pendiente que `Vocabulario-Rules.md` §8 declara
desde la versión 5.0 y que ninguna intervención ejecutó.

## 2. Alcance medido

Sobre el árbol vigente (7.0), lo que asume que el nivel intermedio es el proyecto de código:

| Qué | Ocurrencias |
|---|---|
| `tipo_proyecto_codigo` | 87 |
| Ruta `Proyectos/<Nombre-Proyecto-Codigo>/` | 36 |
| «por proyecto de código» en prosa normativa | 124 |
| Cabeceras de regla con nivel declarado | 19 |

**Ninguna de esas ocurrencias se corrige con sustitución de texto.** `Vocabulario-Rules.md` §9.5
prohíbe la transformación mecánica de un corpus escrito, con el daño de la 5.0 como prueba —la
sustitución global que produjo la palabra «reproducto»—. Cada ocurrencia se lee y se decide, porque
una parte de ellas **sigue significando la unidad de compilación** y tiene que quedarse como está.

## 3. El modelo

### 3.1 Dos ejes, no tres niveles

```
PRODUCTO
├── Eje de entrega ─────── unidades de entrega (1..N)   → las once categorías y D8
└── Eje de construcción ── soluciones de código (1..N)
                           └── proyectos de código (1..M)  → stack, dependencias, capas

        puente: matriz de composición  (unidad de entrega × proyecto de código)
```

La relación entre ejes es de **muchos a muchos**: una unidad de entrega se compone de varios proyectos
de código, y un proyecto de código puede componer varias unidades de entrega. Los dos grafos son
distintos: el de entrega es de **integración en runtime**, el de construcción es de **dependencia de
compilación**, y no coinciden.

### 3.2 El criterio que decide qué es cada cosa

| Pregunta | Si la respuesta es sí |
|---|---|
| ¿Se despliega o se publica de forma independiente? | Es una **unidad de entrega** |
| ¿Produce un artefacto de compilación propio y declara sus dependencias? | Es un **proyecto de código** |

Las dos condiciones no se excluyen: **un proyecto de código que se publica por separado es también
una unidad de entrega**. Es el caso de una librería redistribuible, que se compila como proyecto de
código y se publica como entrega. Un proyecto de código que viaja adentro de otra entrega, no.

### 3.3 Qué nivel toma cada categoría

| Categoría | Nivel | Por qué |
|---|---|---|
| 00 Contexto, 01 Necesidades | **Producto** | Visión y necesidades atraviesan las entregas |
| 02, 03, 04, 06, 08, 10, 11 | **Unidad de entrega** | Un caso de uso lo ejecuta alguien contra algo desplegado; una superficie es de lo desplegado; un sample lo consume un integrador de lo publicado |
| 05 Arquitectura | **Los tres** | Vista de producto en N1, arquitectura y contratos de la entrega en N2, capas y modelo lógico del proyecto de código en el inventario de N1 |
| 07 Plan-Sprint | **Producto + unidad de entrega** | Ya resuelto en la 7.0: el equipo en N1, el plan en el nivel intermedio |
| 09 DevOps | **Unidad de entrega + producto** | Un pipeline **publica**, y publicar por separado es la definición de unidad de entrega |

### 3.4 Dónde vive el eje de construcción

En `Producto/Vista-Producto.md`, que **ya existe** y que `Master-Prompt.md` §11 ya despacha con «mapa
de proyectos de código con su D8 y rol, contratos inter-proyecto y el grafo de dependencias». Gana la
**matriz de composición** y pierde la columna D8, que se muda a la unidad de entrega.

El proyecto de código **no es un nivel de carpetas**. Se inventaría una sola vez, a nivel producto, y
las unidades de entrega declaran cuáles componen. Anidarlo obligaría a documentar dos veces un
proyecto compartido, o a asignarlo arbitrariamente a una entrega dejando en la otra una referencia
colgada.

## 4. Decisiones de diseño, con su fundamento

| # | Decisión | Fundamento |
|---|---|---|
| D-1 | El campo D8 pasa de `tipo_proyecto_codigo` a **`tipo_unidad_entrega`** | `SDD-Development-Guide.md` línea 442: los ocho tipos «cubren el espacio de **formas de entrega**». El conjunto D8 no cambia: cambia de qué es atributo |
| D-2 | Layout: `SDD/Docs/Unidades-Entrega/<Nombre-Unidad-Entrega>/` | El nivel intermedio se puebla con lo que la categoría documenta |
| D-3 | Los proyectos de código no tienen árbol propio | No tienen qué poner adentro: stack, dependencias, capas y modelo lógico son arquitectura y ya tienen su categoría |
| D-4 | `redistribuible` pasa a la unidad de entrega | Es una propiedad de la publicación, no de la compilación. Un proyecto redistribuible **es** una unidad de entrega |
| D-5 | El ámbito de unicidad de los identificadores **sigue siendo el producto** | Es lo que hace resolver las citas cruzadas entre entregas y desde la categoría 01. Lo que cambia es que el **reparto de rangos** pasa a ser por unidad de entrega |
| D-6 | Gating por nivel, con un flag nuevo: `entrega_diferida` | Una unidad de entrega puede estar en el roadmap y no en esta etapa. Sale del caso de ventas, donde la app es complemento del portal y el panel |
| D-7 | Casos degenerados en cascada | Una sola unidad de entrega aplana como hoy; un solo proyecto de código dentro de ella aplana un nivel más. Es la garantía de no ruptura |
| D-8 | La cardinalidad de soluciones de código es `1..N` | `Vocabulario-Rules.md` §5: «su cardinalidad no es necesariamente uno a uno». No se asume una sola |

## 5. El plan, ítem por ítem

### Ola 1 — La espina: vocabulario, layout y derivación

| Id | Archivo · sección | Qué cambia | Sev. |
|---|---|---|---|
| Q-01 | `Vocabulario-Rules.md` §2, §4, §5, §8 | La unidad de entrega pasa de término definido a **nivel del layout**. R3 admite tres niveles. §8 cierra el pendiente declarado y registra el que quede | major |
| Q-02 | `Root-Rules.md` §9.1 y sección de layout | El reparto de rangos pasa a ser por unidad de entrega, conservando el ámbito producto | minor |
| Q-03 | `Master-Prompt.md` §3.5 | Layout nuevo: `Unidades-Entrega/<Nombre>/` en el nivel intermedio, con los casos degenerados en cascada | major |
| Q-04 | `Master-Prompt.md` §3.2 y §3.4 | Derivación de `Nombre-Unidad-Entrega`; el bloque informativo enumera los dos ejes y la matriz de composición | major |
| Q-05 | `Master-Prompt.md` §4 | Gating por nivel: producto, unidad de entrega y proyecto de código, con `entrega_diferida` | major |
| Q-06 | `Master-Prompt.md` §6 y §7 | El bucle recorre **unidades de entrega** en orden topológico de integración | major |
| Q-07 | `Master-Prompt.md` §8, §11, §14, §15 | Despacho parametrizado por unidad de entrega; la vista de producto gana la matriz de composición; glosario | major |
| Q-08 | `PRODUCT-INTAKE-template.md` §13 | Se parte en §13.1 unidades de entrega, §13.2 proyectos de código y §13.3 composición | major |
| Q-09 | `PRODUCT-INTAKE-template.md` §14 | Distingue contratos de integración entre entregas de contratos de compilación entre proyectos | minor |
| Q-10 | `PRODUCT-MANIFEST-template.md` | Deriva los dos ejes y la matriz | major |
| Q-11 | `Intake-Rules.md` §2.2, §4, §5 | Campos bloqueantes y derivación de los dos ejes, con sus validaciones nuevas | major |

### Ola 2 — Las categorías

| Id | Archivo | Qué cambia | Sev. |
|---|---|---|---|
| Q-12 | Las doce reglas de categoría, cabecera y §2.1 | Nivel declarado por artefacto con tres valores posibles | major |
| Q-13 | `Rules-Arquitectura-Tecnica.md` | La 05 se reparte en tres: vista de producto, arquitectura de la entrega, e inventario de proyectos de código con sus capas y su modelo lógico | major |
| Q-14 | `Rules-Devops.md` | El pipeline publica una unidad de entrega; la orquestación entre entregas queda en producto | major |
| Q-15 | Las nueve reglas restantes | Su prosa pasa de «por proyecto de código» a «por unidad de entrega» donde el referente es el nivel intermedio, y se conserva donde el referente es la unidad de compilación | major |
| Q-16 | `Maqueta-Rules.md`, `Deriva-Rules.md` | La maqueta y la línea de base son de una unidad de entrega | major |

### Ola 3 — Migración y cierre

| Id | Archivo | Qué cambia | Sev. |
|---|---|---|---|
| Q-17 | `Migracion-Rules.md` | Migración estructural 7.0 → 8.0: identificar qué proyectos de código son unidades de entrega, fusionar árboles y mover la arquitectura al inventario. **Con detención obligatoria**: la clasificación la decide el humano | major |
| Q-18 | `SDD-User-Guide.md`, `SDD-Development-Guide.md`, `SDD-Getting-Started-Guide.md` | Lo que el usuario ve: el modelo de dos ejes, el test de tres preguntas y el flujo | minor |
| Q-19 | `Marco-Teorico-SDD.md` | El fundamento del nivel intermedio y su correspondencia con C4 | minor |
| Q-20 | `CHANGELOG.md`, `_legacy/7.0/`, nota de coherencia | Cierre de la intervención | — |

## 6. Lo que la migración no puede decidir sola

**Cuáles de los proyectos de código existentes son unidades de entrega.** El manifiesto de un destino
7.0 no lo declara: hay que leerlo. La migración lo propone con dos señales —quién es el proyecto
principal, y qué proyectos declaran despliegue o publicación propia— y **se detiene** para que el
humano confirme. Es la misma clase de decisión que el test de tres preguntas: el método no la toma.

Sobre los destinos medidos, la propuesta automática sería:

| Destino | Proyectos de código | Unidades de entrega propuestas | Árboles resultantes |
|---|---|---|---|
| Lab-Geometria | 7 | `GeometriaFactory-Api`, `GeometriaFactory-Web` | 7 → 2 |
| RPI.VidelControl | 5 | `VideoControl-Web` | 5 → 1 |
| SelfHosted.Service.Core | 1 | `SelfHosted-Service` | 1 → 1 |

## 7. Riesgos declarados

| Riesgo | Mitigación |
|---|---|
| La sustitución léxica masiva rompe el corpus, como en la 5.0 | Prohibida. Cada ocurrencia se lee y se decide. `Vocabulario-Rules.md` §9.5 es la regla que lo veda |
| El caso degenerado se rompe, que es el que menos se prueba | Los dos casos degenerados en cascada se declaran en §3.5 del master-prompt y se verifican explícitamente |
| Quedan referencias a `Proyectos/<Nombre-Proyecto-Codigo>/` sin migrar | Comprobación mecánica de residuo, con conteo antes y después |
| La 05 se parte mal y el modelo lógico queda sin dueño | Q-13 declara los tres destinos antes de tocar nada |

## 8. Los cinco pendientes de la 7.0, planificados

Se incorporan a esta intervención los que dependen de ella, y se declara fecha para el resto.

| Pendiente | Dónde entra | Por qué |
|---|---|---|
| Correr el grafo de obligaciones sobre las diecisiete reglas | **Ola 1, antes de Q-12** | Con el nivel cambiando, hay que saber qué obligación apunta a qué fase antes de reasignar niveles |
| Inventario del vocabulario propio del framework | **Ola 3, con Q-18** | El vocabulario nuevo —unidad de entrega como nivel, matriz de composición— entra al glosario operativo en la misma pasada |
| Adelantar la condición de terminado a la Fase A | **No entra** | Es ortogonal al nivel y sigue administrada con la referencia pendiente. Se registra para una intervención propia |
| Decidir si el «glosario de categoría» es artefacto real | **Ola 2, con Q-12** | Al reasignar niveles hay que decir de qué nivel sería |
| Ampliar D9 a los recuentos en prosa | **No entra** | `Root-Rules.md` §10 ya consigue el efecto. Se mantiene descartada con su motivo |
