# Reporte 06 — La obligatoriedad de un artefacto se decide por el tipo del proyecto, y el tipo no dice lo que el proyecto hace

| Campo | Valor |
|---|---|
| Reporte | 06 |
| Fecha | 2026-08-11 |
| Origen | Corrida real del orquestador sobre el destino `Repos-RPIs/RPI.VidelControl`: Fase C, categoría 05 de `VideoControl-Web`, 2026-08-11. Segunda instancia en la Fase G, categoría 10 de los cuatro proyectos de código de tipo `library`, 2026-08-12 |
| Versión del framework evaluada | SDD 6.0 (`Rules-Arquitectura-Tecnica` §2.1, §2.2, §6 y §7; invariante D8) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Rules-Arquitectura-Tecnica.md` y `SDD/Devs/Rules/Rules-Examples.md` §0, §2.1 y §2.2; por extensión, toda regla de categoría que condicione un artefacto obligatorio al `tipo_proyecto_codigo` |
| Naturaleza | Un criterio de obligatoriedad que resuelve sobre el tipo cuando lo que determina el caso es el reparto de responsabilidades dentro del producto |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |
| Reportes relacionados | `01-Ambito-De-Unicidad-De-Identificadores.md` y `05-Ancho-De-Los-Identificadores.md`, que documentan otros dos atributos que el framework fija sin declarar de qué dependen |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. El incidente](#2-el-incidente)
- [3. Lo que la normativa dice, con precisión](#3-lo-que-la-normativa-dice-con-precisión)
- [4. Por qué las dos salidas disponibles son malas](#4-por-qué-las-dos-salidas-disponibles-son-malas)
- [5. La causa raíz](#5-la-causa-raíz)
- [6. El patrón, enunciado](#6-el-patrón-enunciado)
- [7. Qué hizo el destino](#7-qué-hizo-el-destino)
- [8. Propuestas de intervención](#8-propuestas-de-intervención)
- [9. Cómo verificar que la corrección funcionó](#9-cómo-verificar-que-la-corrección-funcionó)
- [10. Anexo — evidencia reproducible](#10-anexo--evidencia-reproducible)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

`Rules-Arquitectura-Tecnica.md` declara `Modelo-Datos-Logico.md` **obligatorio para el tipo `web-monolith`**. El proyecto de código `VideoControl-Web` es `web-monolith` y **no persiste nada**: la persistencia entera del producto vive en otro proyecto de código del mismo producto, que la modela completa.

El criterio de aceptación del framework parece contemplarlo —«si el tipo D8 exige persistencia»— pero al leerlo con cuidado no lo contempla: la condición es **sobre el tipo**, y el tipo `web-monolith` sí exige persistencia. Lo que no persiste es *este proyecto*.

El resultado es que un proyecto correctamente tipado, con su persistencia correctamente modelada en el lugar que corresponde, **incumple la normativa por una obligación de forma**, y las dos salidas que el framework deja disponibles son peores que el incumplimiento.

## 2. El incidente

| Momento | Qué pasó |
|---|---|
| Fase A y B | El producto se deriva en **cinco proyectos de código**. `VideoControl-Web` queda tipado `web-monolith`, que es correcto: es un monolito web y el punto de entrada único. `VideoControl-Infrastructure` queda tipado `library` y **se lleva toda la persistencia**, que es la decisión que hace posible probar el resto sin base ni hardware |
| Fase C | La 05 de Infrastructure emite `Modelo-Datos-Logico.md` con trece tablas, ocho índices y sus restricciones. La 05 de Web **no lo emite**, y declara el motivo en prosa en dos lugares |
| Audit ronda 1 | Evalúa la ausencia y **no la marca como bloqueante**, apoyándose en la condición de §6. La deja como observación de nivel medio: la omisión no está sostenida por un ADR |
| Audit ronda 2 | Insiste con el mismo argumento |
| Resolución | El destino emite un ADR que **declara el apartamiento**, con sus alternativas descartadas y sus disparadores de revisión |

Nótese que **ningún agente se equivocó**. El tipado es correcto, el reparto de la persistencia es correcto, el modelo lógico existe y está completo, y el auditor evaluó bien. Lo que falla es que la regla no tiene forma de expresar este caso.

## 3. Lo que la normativa dice, con precisión

Tres menciones al mismo artefacto, y ninguna resuelve el caso:

| Dónde | Texto | Qué condiciona |
|---|---|---|
| §2.1, tabla maestra | Obligatorio para «web-monolith, web-microservices, rest-api, worker-service, mobile-app-maui **con almacenamiento**» | El calificativo «con almacenamiento» está pegado al último de una lista de cinco. Leído de forma natural, alcanza a `mobile-app-maui` y no a los otros cuatro |
| §2.2, tabla por tipo | `web-monolith` → Modelo lógico: **Sí** | Nada. Es incondicional |
| §6, criterios de aceptación, y §7 | «Si **el tipo D8** exige persistencia, existe `Modelo-Datos-Logico.md`…» | El **tipo**, no el proyecto. Y `web-monolith` como tipo sí la exige |

**Dice** que la obligatoriedad depende del `tipo_proyecto_codigo`.

**No dice** qué pasa cuando un proyecto de un tipo que exige persistencia no persiste **porque otro proyecto del mismo producto la tiene**.

**No dice** que la obligatoriedad pueda depender de un flag del proyecto —como sí hace, por ejemplo, `requiere_maqueta` para la Fase B2— en vez del tipo.

**No tiene ningún mecanismo declarado de apartamiento**: no existe la figura de «este artefacto no aplica acá, y acá está por qué», con la excepción del propio ADR, que el destino terminó usando para eso.

## 4. Por qué las dos salidas disponibles son malas

El framework deja exactamente dos formas de cumplir la letra, y las dos empeoran la documentación:

| Salida | Qué produce |
|---|---|
| Emitir el modelo lógico copiando el de Infrastructure | **Dos fuentes de verdad para el mismo esquema.** Es el defecto que el propio framework persigue con D6 y con su regla de transcripción del reporte `00`: la copia se desincroniza y nadie sabe cuál manda |
| Emitir un documento que remita a Infrastructure | **Un artefacto vacío que existe para satisfacer un checklist.** Y algo peor que su vacío: enseña que se puede cumplir sin decir nada, y el siguiente artefacto vacío ya tiene precedente |

La tercera —no emitirlo— incumple la letra.

Que las tres opciones sean malas es el síntoma de que la regla no está preguntando lo que corresponde.

## 5. La causa raíz

El framework decide la obligatoriedad de un artefacto por el **tipo** del proyecto de código, y el tipo es una descripción de **la forma** del proyecto: qué clase de cosa es. Lo que determina si hace falta un modelo lógico no es la forma, es **una responsabilidad**: si este proyecto guarda algo o no.

En un producto de un solo proyecto de código, forma y responsabilidad coinciden y la regla funciona. Un `web-monolith` de un solo proyecto tiene su base adentro.

**En un producto de varios proyectos, la responsabilidad se reparte y deja de derivarse de la forma.** Este producto tiene cinco proyectos y la persistencia está en uno solo; el resto son del tipo que son sin serlo del todo.

Y hay una segunda mitad de la causa que conviene ver: **el framework tiene el mecanismo y no lo usa acá**. `requiere_maqueta` es exactamente esto —una capacidad del proyecto que decide si una fase corre— y está declarado en el manifiesto por proyecto, no derivado del tipo. La Fase B2 sabe preguntarle al proyecto; la categoría 05 le pregunta al tipo.

## 6. El patrón, enunciado

> **El framework decide qué artefactos son obligatorios a partir del `tipo_proyecto_codigo`, que describe la forma del proyecto, cuando lo que determina la necesidad de un artefacto es una responsabilidad que en un producto multiproyecto se reparte y deja de derivarse de la forma. Un proyecto correctamente tipado, con la responsabilidad correctamente ubicada en otro proyecto, incumple la normativa, y las dos salidas que el framework deja para cumplirla producen documentación peor que el incumplimiento.**

Dos corolarios que conviene escribir aparte:

> **Una condición mal ubicada es peor que ninguna.** El criterio de §6 parece contemplar el caso —«si el tipo exige persistencia»— y no lo contempla, porque condiciona sobre el tipo y no sobre el proyecto. Un auditor apurado lee la condición, la da por satisfecha y no encuentra el hueco; uno cuidadoso encuentra que la condición no dice lo que parece decir. Las dos lecturas producen informes distintos sobre el mismo hecho.

> **El framework ya tiene la forma correcta de preguntar y no la aplica acá.** `requiere_maqueta` pregunta por una capacidad del proyecto y está en el manifiesto. Que un mecanismo exista en una fase y no en otra no es una decisión: es que nadie lo generalizó.

### 6.1 Segunda instancia, en otra regla y con el dato a la vista

La Fase G del mismo destino produjo el mismo patrón en `Rules-Examples.md`, y en una forma que agrega algo al enunciado.

La regla declara la categoría 10 **obligatoria para `library`**, y en §2.2 le exige **tres samples** —básico, intermedio y avanzado—. El motivo está escrito y es explícito: §0 dice «porque el integrador del artefacto necesita ejemplos reproducibles para arrancar», y §1.2 describe la variante de `library` como «apps consumidoras progresivas que invocan la librería **vía package manager**».

Este producto tiene cuatro proyectos de código de tipo `library`. **Ninguno se distribuye.** El manifiesto lo declara, campo por campo: la columna `redistribuible` dice `false` en los cinco. Los cuatro viajan adentro de la publicación autocontenida del `web-monolith`, y no existe ningún integrador que los consuma por gestor de paquetes, ni lo va a haber sin cambiar la decisión de empaquetado del producto entero.

Tres cosas hacen a esta instancia distinta de la de `Modelo-Datos-Logico.md`:

1. **La regla sí tiene una condición, y está en el lugar equivocado.** §0 dice «es omisible cuando el proyecto de código es estrictamente interno y no hay nuevos integradores previsibles». Es exactamente el caso. Pero la condición vive en la prosa de §0, mientras §2.1 y §2.2 —las tablas que el orquestador lee para filtrar documentos, por instrucción de `Master-Prompt.md` §6— dicen `library: obligatorio` y `library: 3 samples` sin condición alguna. Dos partes de la misma regla dan respuestas distintas, y la que el orquestador ejecuta es la incondicional.
2. **El dato que resolvería el caso ya está declarado, en el manifiesto, y la regla no lo lee.** No hace falta inventar un campo nuevo: `redistribuible` existe. El corolario del §6 decía que el framework «ya tiene la forma correcta de preguntar y no la aplica acá», refiriéndose a `requiere_maqueta`. Acá es más fuerte que eso: el campo no sólo existe, sino que responde exactamente la pregunta de la que depende la obligación, y está a un renglón de distancia.
3. **La cantidad no tiene ninguna válvula.** La obligatoriedad tiene su cláusula de omisión mal ubicada; el piso de tres samples por `library` no tiene ni eso. Un proyecto de código que legítimamente necesita un sample tiene que emitir tres o incumplir.

El destino resolvió emitiendo los catorce samples y declarando el apartamiento respecto del intake —que pedía cinco— en la sección 6 del README de cada categoría. La decisión no fue de cumplimiento formal: repartir los casos de uso en tres niveles resultó **mejor** que el sample único del intake en dos de los cuatro casos, y el ejemplo más claro es `VideoControl-Infrastructure`, cuyo sample único exigía la Raspberry para todo y al repartirse dejó dos tercios corriendo en cualquier máquina. Eso no vuelve correcta la regla: significa que en este producto la obligación incondicional acertó por casualidad, y una obligación que acierta por casualidad sigue siendo una obligación sin condición.

## 7. Qué hizo el destino

Emitió `ADR-07` en la 05 de `VideoControl-Web`. La decisión **declara el apartamiento en vez de ampararse en una excepción que la normativa no da**, con cuatro alternativas descartadas —incluidas las dos que cumplirían la letra— y tres disparadores concretos que la superarían: que el proyecto guarde algo propio, que la sesión deje de vivir en la cookie, o que consulte la base directamente.

Es lo mejor disponible y **no es suficiente**, por un motivo que importa para la intervención: un ADR del destino no puede resolver una regla del framework. Lo que hace es que el apartamiento sea rastreable, tenga estado y obligue a que cambiar de idea sea otro ADR. Lo que no hace es que la próxima corrida sobre otro producto multiproyecto no vuelva a chocarse con lo mismo.

## 8. Propuestas de intervención

No se aplican acá. Se enumeran para el prompt que intervenga el framework.

| # | Intervención | Artefacto | Efecto esperado |
|---|---|---|---|
| P-1 | Condicionar la obligatoriedad sobre **el proyecto y no sobre el tipo**: «si este proyecto de código persiste» en lugar de «si el tipo D8 exige persistencia» | `Rules-Arquitectura-Tecnica.md` §6 y §7 | La condición pasa a preguntar lo que decide el caso |
| P-2 | Derivar la obligatoriedad de **flags del proyecto declarados en el manifiesto**, como ya hace `requiere_maqueta`: `persiste`, `expone_api`, `tiene_pipeline` | `Intake-Rules.md` §13, `PRODUCT-MANIFEST`, reglas de categoría | Generaliza un mecanismo que el framework ya tiene y usa bien en la Fase B2 |
| P-3 | Declarar una **figura de apartamiento** con forma fija: un artefacto obligatorio puede no emitirse si existe un ADR que lo declare, con sus alternativas y sus disparadores de revisión | `Root-Rules.md`, criterios de audit del `Master-Prompt` §10 | Lo que el destino tuvo que inventar pasa a ser parte del método, con la misma forma en todas las corridas |
| P-4 | Alinear las tres menciones del mismo artefacto: §2.1, §2.2 y §6 dicen tres cosas distintas sobre la misma obligación | `Rules-Arquitectura-Tecnica.md` | Una obligación con tres enunciados distintos produce tres auditorías distintas |
| P-5 | Sumar a los audits un control de **coherencia interna de la propia regla**: que la tabla maestra, la tabla por tipo y los criterios de aceptación digan lo mismo sobre cada artefacto | criterios de audit del `Master-Prompt` §10 | El desacuerdo entre secciones de una misma regla deja de descubrirse en una corrida |

La P-2 es la que más lejos llega, y la P-4 es la más barata: hoy la misma obligación está enunciada tres veces con tres alcances distintos, y eso solo ya produce hallazgos que no son del destino.

## 9. Cómo verificar que la corrección funcionó

1. Generar la 05 de un producto multiproyecto donde la persistencia esté en un proyecto distinto del `web-monolith`, y comprobar que **ninguna de las dos salidas malas** es necesaria.
2. Comprobar que la obligatoriedad se resuelve consultando el proyecto y no el tipo: cambiar el flag y ver que la lista de artefactos obligatorios cambia.
3. Correr el audit sobre un artefacto obligatorio ausente **con** su ADR de apartamiento, y comprobar que lo evalúa como decisión y no como omisión.
4. Comparar las tres menciones de cada artefacto obligatorio en cada regla de categoría y comprobar que dicen lo mismo.

## 10. Anexo — evidencia reproducible

```text
[EV-01 | artefacto | SDD/Devs/Rules/Rules-Arquitectura-Tecnica.md | §2.1, tabla maestra, fila de Modelo-Datos-Logico.md | 2026-08-11]
[EV-02 | artefacto | ídem | §2.2, tabla por tipo, fila web-monolith, columna «Modelo lógico»: «Sí» | 2026-08-11]
[EV-03 | artefacto | ídem | §6, criterio «Si el tipo D8 exige persistencia…», y §7 con el mismo texto | 2026-08-11]
[EV-04 | artefacto | SDD/Intake/PRODUCT-INTAKE-Videocontrol-De-Camaras-Y-Actuadores.md | §17.4 P.4: «No aplica directamente: delega en Infrastructure» | 2026-08-11]
[EV-05 | artefacto | SDD/Docs/Proyectos/VideoControl-Infrastructure/05-Arquitectura-Tecnica/Modelo-Datos-Logico.md | trece tablas, ocho índices; es el único del producto | 2026-08-11]
[EV-06 | audit | SDD/Docs/Audit/Audit-Fase-C-Arquitectura-Tecnica.md | hallazgo P2-01, y su repetición en la ronda 2 | 2026-08-11]
[EV-07 | artefacto | SDD/Docs/Proyectos/VideoControl-Web/05-Arquitectura-Tecnica/Adrs/ADR-07-Este-Proyecto-No-Emite-Modelo-De-Datos-Logico.md | la decisión de apartamiento con sus tres disparadores | 2026-08-11]
```

```bash
# Comprobación de la incoherencia interna de la regla, reproducible:
grep -n "Modelo-Datos-Logico\|Modelo lógico" \
  IA/IA.SDD/SDD/Devs/Rules/Rules-Arquitectura-Tecnica.md
#   §2.1  -> obligatorio para web-monolith, sin condición
#   §2.2  -> web-monolith: «Sí», sin condición
#   §6,§7 -> «si el tipo D8 exige persistencia»
```

El apartamiento se aplicó sobre el destino y está registrado como decisión. Ninguna corrección se aplicó al framework: este reporte existe para que esa corrección se decida con evidencia.

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-11 | Reporte inicial: el framework decide la obligatoriedad de un artefacto por el tipo del proyecto de código, que describe su forma, cuando lo que la determina es una responsabilidad que en un producto multiproyecto se reparte. Con las tres menciones desalineadas de la misma obligación, las dos salidas malas que el framework deja para cumplirla, y la observación de que el mecanismo correcto ya existe en `requiere_maqueta` y no se generalizó. |
| 1.1 | 2026-08-12 | Se incorpora una segunda instancia del patrón, encontrada al ejecutar la Fase G: `Rules-Examples.md` declara la categoría obligatoria para `library` y le exige tres samples, con el motivo escrito de que el integrador los necesita para arrancar, y los cuatro `library` de este producto tienen `redistribuible` en `false` en el manifiesto. La instancia agrega tres cosas al enunciado de §6: que la regla sí tiene la condición pero fuera de las tablas que el orquestador ejecuta, que el campo que resolvería el caso ya está declarado en el manifiesto y la regla no lo lee, y que el piso de cantidad no tiene ninguna válvula. Nueva §6.1; se actualizan el origen y los artefactos alcanzados de la cabecera. El enunciado de §6 y el incidente original no se tocan. |
| 1.2 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** La obligatoriedad de un artefacto declarada por tipo D8 sin la condición que la justifica.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `Rules-Arquitectura-Tecnica.md` y `Rules-Examples.md` | La obligatoriedad pasa a depender del **flag**, no del tipo |
| `Root-Rules.md` §11 | El **apartamiento declarado**: un artefacto obligatorio ausente **con** ADR se evalúa como decisión y no como omisión |

**Después de la 7.0.** **El caso concreto se disolvió en la 8.0.** Con `tiene_persistencia` evaluado en la **unidad de entrega**, un monolito cuya persistencia vive en una de sus capas compiladas **sí persiste**, y su modelo lógico es uno solo: el conflicto que el reporte describía deja de poder ocurrir.

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
