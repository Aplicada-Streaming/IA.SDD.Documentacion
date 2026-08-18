# Reporte 13 — El método clasifica cómo se verifica un hallazgo, y no quién puede cerrarlo

| Campo | Valor |
|---|---|
| Reporte | 13 |
| Fecha | 2026-08-18 |
| Origen | El cierre de la migración normativa de un destino real, fase M6, y la discusión con el Product Owner que le siguió: si un auditor independiente bien perfilado produce un resultado mejor, y qué gana con eso el humano |
| Versión del framework evaluada | SDD 9.18 (`Master-Prompt.md` §8.1 y §10; `Master-Prompt-Migracion.md` §10; `Catalogo-De-Criterios.md` 1.4) |
| Artefactos del framework alcanzados | `SDD/Devs/Orchestrator/Master-Prompt.md` §8.1 y §10; `Master-Prompt-Migracion.md` §10; `SDD/Devs/Rules/Catalogo-De-Criterios.md` §4.1 |
| Naturaleza | Un hueco de método con daño medido en una corrida real. No es un defecto de ninguna regla escrita: es una clasificación que falta |
| Estado | **Para evaluación.** Ninguna modificación aplicada por este reporte |
| Reportes relacionados | `09-El-Audit-Como-Unica-Compuerta.md` y `12-La-Compuerta-Declarada-Y-La-Compuerta-Ejecutada.md`, que tratan el otro extremo del mismo eje: qué puede resolver un guion |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**, y
sigue la forma de los reportes `00` a `12`: evidencia primero, propuesta después, y lo que no se sabe
declarado como tal.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. Lo que el framework ya clasifica, y lo que no](#2-lo-que-el-framework-ya-clasifica-y-lo-que-no)
- [3. La evidencia: cinco detenciones de una corrida real](#3-la-evidencia-cinco-detenciones-de-una-corrida-real)
- [4. Por qué el agente escala de más, y no es cautela](#4-por-qué-el-agente-escala-de-más-y-no-es-cautela)
- [5. Qué aporta realmente un auditor independiente](#5-qué-aporta-realmente-un-auditor-independiente)
- [6. Propuesta: el estrato del hallazgo](#6-propuesta-el-estrato-del-hallazgo)
- [7. La regla de legitimidad de la detención](#7-la-regla-de-legitimidad-de-la-detención)
- [8. Riesgos de la propuesta, y qué la haría fallar](#8-riesgos-de-la-propuesta-y-qué-la-haría-fallar)
- [9. Lo que este reporte no sabe](#9-lo-que-este-reporte-no-sabe)
- [10. Control de cambios](#10-control-de-cambios)

## 1. Resumen

**El framework clasifica los criterios por cómo se verifican —`[enumerable]` o `[interpretativo]`— y
no clasifica los hallazgos por quién puede cerrarlos.** La consecuencia se midió: en una corrida real,
**tres de cinco decisiones que el agente llevó al humano no eran del humano**. Ninguna requería
criterio de producto, autoridad ni preferencia; las tres tenían respuesta en el árbol.

La causa no es descuido. `Master-Prompt.md` §8.1 declara desde la 9.16 que **un defecto propio lo
corrige el agente y una decisión de diseño la toma el humano**, pero **no declara cómo se establece de
cuál de los dos se trata**. Sin ese paso, la asimetría de costos decide: escalar es barato para el
agente y caro para el humano, así que ante la duda se escala.

Este reporte propone la clasificación que falta —**el estrato del hallazgo**, en tres niveles— y la
regla que se desprende: **una detención al humano sólo es legítima si el hallazgo es del tercer
estrato**. Y ubica al auditor independiente en su función correcta, que no es emitir opinión sino
**establecer el estrato**.

## 2. Lo que el framework ya clasifica, y lo que no

**El eje que existe.** `Master-Prompt.md` §10.0 declara dos naturalezas de propiedad y las trata
distinto, y `Catalogo-De-Criterios.md` §4.1 las inventaria: **97 anti-patrones `[enumerable]` y 105
`[interpretativo]`**, sobre 202 catalogados. El eje responde: *¿esto se decide contando o leyendo?*

**El eje que falta responde otra pregunta:** *¿quién tiene autoridad para cerrarlo?* Son
independientes, y confundirlos es lo que produce el problema. Un hallazgo `[interpretativo]` —se
decide leyendo— **no es por eso un hallazgo del humano**: leer los dos documentos y ver si uno dice lo
que el otro afirma es interpretativo **y no requiere ninguna autoridad**. El propio §10.0 lo dice al
describir para qué existe el auditor:

> «dejarle su presupuesto de atención para lo único que un guion no puede hacer, que es **abrir el
> documento citado y ver si dice lo que el que lo cita afirma**».

**Eso es interpretativo y no es del humano.** Hoy el método no tiene dónde escribir esa distinción, y
el resultado es que la marca `[interpretativo]` se lee de hecho como «esto lo mira una persona».

## 3. La evidencia: cinco detenciones de una corrida real

Las cinco detenciones que un agente presentó al Product Owner durante la consolidación y el cierre de
una migración normativa, clasificadas después de los hechos:

| Detención | ¿Requería criterio de producto, autoridad o preferencia? | ¿Tenía respuesta en el árbol? |
| --- | --- | --- |
| Cómo resolver un par de reglas de negocio con solapamiento de cuerpo del 17,1 % | **Sí.** Requiere al analista funcional | No |
| Dónde vive una lección de forma: en la regla que la originó o en la guía de desarrollo | **Sí.** Es alcance del método, y el humano decidió distinto del agente **y tenía razón** | No |
| **Dos filas de control de cambios fuera de su tabla, preexistentes** | **No** | **Sí** |
| **Trece encabezados sin separar de su cuerpo, en documentos que no pasaron por el emisor** | **No** | **Sí** — se contrastan contra su origen |
| **En qué orden consolidar las dos últimas categorías** | **No** | **Sí** — el propio agente fundamentó la recomendación |

**Tres de cinco no eran del humano.** Y las tres son del mismo tipo: **hallazgos originados por el
trabajo del agente o por el estado del árbol, con respuesta verificable en el árbol**, presentados
como si fueran preferencias.

**La segunda fila merece leerse al revés, porque es la que da la medida.** Ahí el agente **recomendó
mal** —propuso no generalizar una lección, apoyándose en un recuento de evidencia que no había
verificado— y el humano decidió lo contrario. Esa detención **valió**. El problema no es que el agente
detenga: es que el ruido de las tres innecesarias compite por la atención con las dos que importan.

## 4. Por qué el agente escala de más, y no es cautela

**La asimetría de costos.** Escalar cuesta al agente una sección del informe; al humano le cuesta
reconstruir contexto que no tiene para decidir algo que no eligió mirar. Sin una regla que lo prohíba,
**el equilibrio se corre siempre hacia escalar**, y se justifica como prudencia.

**Y hay un segundo efecto, que es el que degrada el mecanismo.** `Master-Prompt.md` §8.1 exige que
toda detención lleve análisis, opciones, impacto y recomendación. Es correcto y es caro. **Aplicado a
un hallazgo que el agente podía cerrar solo, produce tres párrafos de análisis para una decisión que
no había que tomar**, y entrena a leer las detenciones por encima — que es exactamente lo que la
9.8 escribió §8.1 para evitar.

**Es el mismo patrón que el framework ya registró dos veces**: un mecanismo correcto que se degrada
por sobre-aplicación. `Migracion-Rules.md` §6 lo dice sobre el verificador de preservación —«un
verificador que sobre-reporta entrena a ignorarlo»— y §VI.3 lo dice sobre los pasos —«el procedimiento
crece hasta dejar de leerse, que es la forma en que un procedimiento muere»—. **Falta decirlo de la
detención.**

## 5. Qué aporta realmente un auditor independiente

`Master-Prompt-Migracion.md` §10 exige **auditor independiente, invocado desde cero**. Conviene separar
qué compra y qué no, porque la respuesta cambia lo que hay que pedirle.

**Lo que sí compra, y es lo importante: ausencia de compromiso.** El agente que decidió una
transposición tiene interés en que la respuesta sea que estuvo bien. **Eso no se corrige con más
contexto ni con mejor prompt**: es estructural de quien decidió.

**Lo que no compra: independencia de criterio.** Dos instancias del mismo modelo, leyendo las mismas
reglas, tienden a coincidir en una pregunta abierta. Una confirmación correlacionada **se lee como
verificación sin serlo**, y cierra el hallazgo peor que no haberlo mirado.

**De ahí se sigue cómo hay que preguntarle, y es la parte accionable:**

| Forma de la pregunta | Qué produce |
| --- | --- |
| «¿Estuvo bien resolver esto por S4?» | Pregunta abierta. **Correlaciona**: el auditor tiende a confirmar |
| «Acá está la afirmación y acá los dos documentos: ¿el texto la sostiene? Citá la línea» | Pregunta cerrada con respuesta en el árbol. **No correlaciona**: hay contra qué contrastar |

**Y una consecuencia que conviene declarar como beneficio y no como efecto secundario.** Un auditor
sin memoria de las decisiones es también **la prueba de que el árbol se sostiene solo**, que es la
propiedad sobre la que se apoya todo el método —el estado vive en el árbol y no en la conversación—.
**Si el auditor no puede resolver con el árbol a la vista, el hallazgo es sobre el árbol**: una
decisión se tomó y no se dejó escrita.

## 6. Propuesta: el estrato del hallazgo

Un eje nuevo, ortogonal a `[enumerable]`/`[interpretativo]`, que responde **quién puede cerrarlo**:

| Estrato | Qué lo caracteriza | Quién resuelve | Qué ya existe |
| --- | --- | --- | --- |
| **E1 · Mecánico** | Se decide contando o comparando | La compuerta mecánica | `Master-Prompt.md` §10.0. **Ya existe** |
| **E2 · Verificable por lectura** | Tiene respuesta **en el árbol**: se abre lo citado y se contrasta | **El auditor lo establece; el agente lo corrige** bajo la autocorrección de §8.1 | **No existe.** Es el hueco |
| **E3 · Decidible sólo por el humano** | Requiere intención de producto, autoridad, o una preferencia que el árbol no contiene | El humano, con la forma de §8.1 F1 a F4 | §8.1. **Ya existe** |

**El estrato E2 es todo el aporte de este reporte.** Hoy el método tiene el primero y el tercero, y lo
que cae en el medio se resuelve por el hábito del agente. Las tres detenciones innecesarias de §3 son
E2.

**El auditor cambia de función y conviene decirlo explícito.** Deja de ser *un segundo opinante* y
pasa a ser **el instrumento que establece el estrato de un hallazgo**. No dice «esto está mal»: dice
«esto tiene respuesta en el árbol y la respuesta es ésta, con su cita» o «esto no la tiene».

## 7. La regla de legitimidad de la detención

> **Una detención al humano es legítima sólo si el hallazgo es E3.** Si es E2, el auditor lo establece
> y el agente lo corrige en la misma unidad. **Escalar un E2 no es prudencia: es delegar trabajo
> propio**, y el costo lo paga quien no tiene el contexto para decidirlo.

**Y su contraparte, que evita el defecto simétrico** —la lección que la 9.18 dejó escrita en la Parte
IV de `SDD-Development-Guide.md`—:

> **Ante la duda sobre el estrato, E3.** Clasificar mal hacia E2 es peor que hacia E3: el agente
> resuelve por su cuenta algo que era del humano, y el humano se entera después. Es el mismo criterio
> con que §6 resuelve la duda entre enumerable e interpretativo, y por el mismo motivo: **el error
> barato y el error caro no son simétricos**.

**Qué obliga en el cierre de unidad.** El bloque de entrega de §8.1 pasa a declarar, de cada hallazgo
que no se escala, **su estrato y quién lo estableció**. Sin eso la regla es inauditable: no se
distingue un E2 correctamente resuelto de un E3 que el agente se guardó.

## 8. Riesgos de la propuesta, y qué la haría fallar

| Riesgo | Por qué es real | Mitigación propuesta |
| --- | --- | --- |
| **El agente clasifica como E2 lo que le conviene** | Es el mismo compromiso que motiva al auditor externo. Un agente que se autoclasifica el estrato reproduce el problema | El estrato de un hallazgo **propio** lo establece el auditor, no el agente |
| **El auditor confirma por correlación** | §5 | Framing adversarial obligatorio: se le pide **refutar**, y la respuesta exige **cita literal**, no juicio |
| **Se resuelven en silencio cosas que el humano quería ver** | Un E2 correctamente resuelto puede igual ser de interés | El cierre de unidad **los lista con su estrato**; no desaparecen, cambian de lugar |
| **Un estrato más es un concepto más que mantener** | El método ya tiene dos ejes de clasificación | Es el argumento en contra más serio de este reporte, y **no está resuelto**: ver §9 |

## 9. Lo que este reporte no sabe

**No sabe si tres de cinco es representativo.** Es una corrida, de un destino, con un agente. El
patrón es consistente con la asimetría de costos de §4, pero **una muestra no es una tasa**.

**No sabe si la clasificación se puede establecer de forma estable.** «¿Tiene respuesta en el árbol?»
es más nítido que «¿es del humano?», pero no es enumerable: sigue siendo un juicio, y este reporte
propone que ese juicio lo haga un agente.

**No sabe si el costo conceptual se justifica.** Sumar un eje a un método que ya tiene dos, y que
además declara en su propia guía que un procedimiento que crece deja de leerse, es exactamente el
movimiento contra el que esa guía advierte. **La alternativa mínima existe y hay que evaluarla contra
ésta:** no crear el eje, y limitarse a agregar a §8.1 una pregunta previa a toda detención —*«¿esto
tiene respuesta en el árbol?»*—. Resuelve el caso medido con una oración en lugar de un concepto.

**No sabe cuál de las dos conviene**, y no lo decide un reporte.

## 10. Control de cambios

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 2026-08-18 | Reporte inicial. Registra que el método clasifica los criterios por **cómo se verifican** y no los hallazgos por **quién puede cerrarlos**, con la medición de una corrida real: **tres de cinco detenciones al humano no eran del humano**. Ubica la causa en que §8.1 declara la frontera desde la 9.16 pero no cómo se establece de qué lado cae un hallazgo, y en la asimetría de costos que decide en su lugar. Propone el **estrato del hallazgo** en tres niveles —E1 mecánico, **E2 verificable por lectura**, E3 del humano—, la **regla de legitimidad de la detención** con su «ante la duda, E3», y redefine al auditor independiente como **el instrumento que establece el estrato** en vez de un segundo opinante. Declara la alternativa mínima —una pregunta previa en §8.1, sin eje nuevo— como competidora seria y no la descarta. |
