# 40 — La auditoría sobre las quince reglas: ítems empaquetados y diferimiento ilegítimo

**Cierra el criterio 4 de `Reportes/14` §7** —*«ningún ítem obligatorio de una §4.x empaqueta dos
decisiones cuando una sola puede estar bloqueada. **Se audita una vez sobre las quince reglas**»*— y
**la solicitud 6 del prompt**, que encarga la lista de ítems donde diferir sea ilegítimo y declara
que *«se construye mirando las quince reglas, no se hereda del reporte»*.

**Corrida sobre SDD 10.1, sobre los archivos vivos.**

---

## 1. Qué se auditó, y por qué son quince

Las quince reglas con §4.x sustantiva, que es donde vive un ítem que un destino contesta:

`Root-Rules` · `Rules-Contexto` · `Rules-Necesidades-Negocio` · `Rules-Especificacion-Funcional` ·
`Rules-UX-UI-DX` · `Rules-Prompts-AI` · `Rules-Arquitectura-Tecnica` · `Rules-Backlog-Tecnico` ·
`Rules-Plan-Sprint` · `Rules-Calidad-Y-Pruebas` · `Rules-Devops` · `Rules-Examples` ·
`Rules-Documentacion` · `Maqueta-Rules` · `Migracion-Rules`

**`Migracion-Rules` entra y sale enseguida:** su §4.x es procedimiento de migración, no una lista de
ítems que un destino contesta, y por eso el patrón no se le aplica. **`Vocabulario-Rules` e
`Intake-Rules` no tienen §4.x** y quedan fuera por construcción.

## 2. El criterio, escrito antes de mirar

El reporte pide separar *«cuando un ítem admite que una mitad se difiera y la otra no»*. Eso deja
afuera la mayoría de las enumeraciones, y la distinción es la que decide:

| | Test | Ejemplo del árbol |
|---|---|---|
| **Empaquetado** | Las dos mitades **se deciden por separado**, y una puede depender de un evento futuro sin que la otra dependa de nada | `Rules-Devops` §4.3 punto 3, ya partido: elegir `v` no exige haber elegido MinVer |
| **No empaquetado** | La segunda mitad **se deriva de la primera**: no se puede decidir antes, así que diferirla no es arrastre sino consecuencia | `Rules-Arquitectura-Tecnica` §4.4 punto 5 (línea 214): el identificador de la migración **no existe** hasta que hay tooling |

**Un ítem con muchos atributos no es un ítem empaquetado.** «Cada gate especifica condición,
herramienta y consecuencia» describe **una** decisión con tres caras.

---

## 3. Resultado: cinco ítems empaquetados, y cuatro están en la misma regla

| # | Ítem | Las dos decisiones | Cuál puede estar bloqueada, y por qué la otra no |
|---|---|---|---|
| **E1** | `Rules-Devops` §4.6 punto 5 (l. 203) · *«**SAST y DAST**. Herramientas de análisis estático y dinámico, stages […] y criterios de bloqueo de PR»* | Dos familias de herramienta en un ítem, unidas por una conjunción literal | **DAST necesita un ambiente desplegado**, que §4.4 punto 1 declara aparte y puede no existir todavía. **SAST corre sobre el código y no espera a nada.** Es el caso más nítido de los cinco |
| **E2** | `Rules-Devops` §4.4 punto 2 (l. 184) · *«Provisión (IaC). **Herramienta declarativa elegida** […] Layout de módulos, política de state y **aprobación de `plan` antes de `apply`**»* | La herramienta y una política de proceso | El **layout** y la **política de state** sí dependen de la herramienta. La **aprobación de `plan` antes de `apply`** no: la regla la escribe en términos neutros y vale para las cuatro herramientas que nombra |
| **E3** | `Rules-Devops` §4.3 punto 5 (l. 176) · *«Canales. Preview, stable y opcionalmente LTS, con criterios de promoción y **semántica de sufijos `-alpha`, `-beta`, `-rc`**»* | El conjunto de canales y la convención de sufijos | **Es el gemelo exacto del caso medido.** La convención está escrita literal en la propia regla, igual que `v<X.Y.Z>` lo estaba en su tabla de canales: no hay nada que decidir y aun así viaja pegada a una decisión que puede estar abierta |
| **E4** | `Rules-Devops` §4.6 punto 1 (l. 199) · *«SBOM. **Formato** (CycloneDX o SPDX), **generador**, formato de salida […] publicación adjunta al release y firma»* | El formato y el generador | El **generador** puede depender del runtime, que se fija más tarde. El **formato** y la **publicación adjunta al release** se eligen hoy, y de hecho la tabla de stages de §4.7 ya escribe «SBOM generado y adjunto» como gate |
| **E5** | `Rules-Backlog-Tecnico` §4.4 punto 5 (l. 174) · *«**Prioridad y estimación**. MoSCoW declarada con justificación, story points […] y técnica usada»* | Dos decisiones **de dueños distintos** | La **prioridad MoSCoW** es del Product Owner; la **estimación** es del equipo y sale del refinamiento. Bloquear una no bloquea la otra, y el ítem las obliga juntas |

**Y el que no entra, para que el criterio se vea funcionando:** `Rules-Calidad-Y-Pruebas` §4.2 punto 4
(l. 158) reparte tres roles QA en un ítem. Parece empaquetado y no lo es en el sentido del reporte:
son tres nombramientos de la misma decisión —quién hace QA—, y si el equipo no está formado, **no hay
mitad decidible**.

**Cuatro de los cinco están en `Rules-Devops`, y no es casualidad.** Es la regla donde un ítem
mezcla, por naturaleza del dominio, *elegir un producto* con *fijar una convención*, que es la
combinación que produjo el incidente medido.

---

## 4. La lista que el reporte no hacía: dónde diferir es ilegítimo

El reporte `14` §8 declara no saber *«si conviene prohibir el diferimiento en algunos ítems»*. La
lista se construye acá, y **no como catálogo de excepciones sino como una propiedad**, porque el
propio framework acaba de declarar que una regla enunciada sobre el caso y no sobre la propiedad es
defecto de forma (`SDD-Development-Guide.md` §VI.3.1, y la observación §5 de
`Coherencia-Item-Diferido.md`).

> **Diferir es ilegítimo cuando el ítem fija la forma de un registro que el producto empieza a
> producir antes del evento de cierre.**

**El motivo es el daño irreversible, y está medido.** En el incidente del reporte `14`, **tres de las
ocho etapas ya no se podían etiquetar sin inventar el punto**: cada acto ocurrido mientras el ítem
estaba diferido nace sin la forma, y la forma no se le puede poner después. Frente a eso, el
diferimiento en forma **no alcanza**: es correcto, contable y aun así el daño se acumula mientras el
evento no llega.

**Los ítems que hoy cumplen la propiedad, sobre las quince reglas:**

| Ítem | Qué registro empieza antes | Reversible después |
|---|---|---|
| `Rules-Devops` §4.3 punto **3.b** — prefijo de tag | Las etiquetas de cada etapa cerrada | **No.** Medido: 3 de 8 |
| `Rules-Devops` §4.3 punto **2** — Conventional Commits | Cada mensaje de confirmación, del que se deriva el bump | **No.** Reescribir la historia no es una opción del método |
| `Rules-Devops` §4.3 punto **1** — SemVer y sus reglas de incremento | La serie de versiones publicadas | **No.** Una versión publicada no se renumera |
| `Rules-Devops` §4.3 punto **7** — registro del avance **con responsable** | El registro de etapas, desde la primera | **No**, y la regla ya lo dice: *«deja una obligación sin sujeto, que es la forma en que estos registros se degradan»* |
| `Rules-Calidad-Y-Pruebas` §4.8 punto **1** — DoD por capa | Cada sprint que cierra sin criterio | **No.** Un sprint cerrado no se vuelve a cerrar |

**Los cinco son de dos reglas, y las dos gobiernan el ciclo de construcción** —que es, exactamente, el
ciclo que el método declara que no gobierna y donde §12.2 dijo que su evento de cierre se le escapa—.
**No es una coincidencia: es la misma frontera vista por segunda vez.**

**Qué se propone hacer con la lista, y son dos cosas distintas:**

1. **Lo barato y sin costo:** que §12.2 declare la propiedad, y que el ítem que la cumple **se
   conteste con un valor provisorio declarado** en lugar de diferirse. Un prefijo `v` provisorio que
   después se cambia cuesta un renombre de etiquetas; el prefijo ausente costó ocho etapas.
2. **Lo caro:** marcar los cinco ítems en sus reglas. Cada marca vuelve **no conforme** a un destino
   que difirió ahí, y eso es **major** en las dos reglas.

---

## 5. Qué se aplica y qué se eleva

| Qué | Costo | Quién decide |
|---|---|---|
| **E1 a E5**, partir los cinco ítems | **Major** en `Rules-Devops` y en `Rules-Backlog-Tecnico`; **todo destino con esos documentos entra en migración** | **Product Owner.** Es la misma bifurcación que `20-Plan` §2 planteó para `A`/`B`, con el mismo argumento a favor de esperar: **la medición sigue siendo de un caso** |
| **La propiedad de §4**, como criterio en §12.2 | **Minor** si se enuncia como criterio, **major** si se marca ítem por ítem | **Product Owner**, por la misma razón |
| **La auditoría en sí** | **Cero.** Es este documento | **Corrida y cerrada**: el criterio 4 del reporte `14` pasa de «no auditado» a «auditado, con cinco hallazgos enumerados» |

**Y hay un dato que conviene esperar antes de gastar un major.** La comprobación 6 de §10.0 y el
bloque de la reanudación **ya están corriendo desde la 10.0**. La primera corrida sobre cada destino
dice **cuántos diferimientos hay y cuántos están vencidos**. Con ese número, partir los cinco ítems se
decide con evidencia en vez de con un caso — que es, palabra por palabra, el orden que
`20-Plan-De-Aplicacion.md` §3 fundó y que la 9.13 ya había usado: **primero medir, después obligar**.

---

## 6. Desenlace: qué se aplicó, y cuándo

**Decisión del Product Owner, 2026-08-20: los cinco ítems se parten ahora.** No se espera la medición
que §5 proponía. Publicado como **SDD 11.0**, con el bloque «Impacto sobre destinos existentes» que el
major obliga y la nota `SDD/Devs/Guides/Coherencia-Items-Empaquetados.md`.

| Qué | Estado |
|---|---|
| **E1 a E5**, los cinco ítems | **Aplicados.** `Rules-Devops.md` 5.0 → **6.0** (cuatro) y `Rules-Backlog-Tecnico.md` 4.4 → **5.0** (uno) |
| El criterio que los distingue | **Registrado** en `Catalogo-De-Criterios.md` 1.8 |
| **La propiedad del §4** —diferir es ilegítimo cuando el ítem fija la forma de un registro que ya empezó— | **No aplicada.** Queda elevada: sin un segundo caso medido, incorporarla es agregar un concepto, que es lo que la 9.19 desaconsejó |

**Con esto el criterio 4 de `Reportes/14` §7 pasa a cumplir**, y los cinco criterios del reporte quedan
cerrados.
