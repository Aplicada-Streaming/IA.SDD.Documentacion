# G5 — El vocabulario del método no tiene glosario

**Documento:** 14-G5-Vocabulario-Del-Metodo.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Reporte de la familia:** 11
**Estado:** Vigente

---

## 1. Dónde se produce el fallo

Tres hechos encadenados, los tres verificados sobre el árbol vigente:

1. **`Vocabulario-Rules.md` gobierna seis términos** —producto, proyecto de código, unidad de
   entrega, módulo, proyecto y solución de código— y el framework acuña bastante más vocabulario
   propio que ninguna regla gobierna.
2. **Ese vocabulario no está en ningún glosario del framework.** `sonda` es el caso que más pesa: es
   la unidad elemental del sensado de deriva, cada fila de cada `Matriz-Sensado-Deriva.md` es una
   sonda, y en un solo proyecto de código del destino hay 376.
3. **El criterio que exige declarar vocabulario está replicado en once reglas y manda a nueve
   destinos distintos**, casi todos glosarios del producto.

La comprobación del punto 2, reproducible:

```bash
grep -rci "sonda" Rules/*.md Orchestrator/*.md | grep -v ":0"
#  Rules-Examples.md:8   Deriva-Rules.md:14   Rules-Calidad-Y-Pruebas.md:4
#  Rules-Documentacion.md:1   Master-Prompt.md:2
grep -n "sonda" Orchestrator/Master-Prompt.md
#  512 y 825: las dos apariciones la usan; ninguna la define
```

`Master-Prompt.md` §15 tiene cuarenta y un términos. Define `Sensado de deriva` —cuya definición no
usa la palabra— y define `Contrato de verificación (VER-XX)`, que **es** una sonda. No define
`sonda`. `Deriva-Rules.md`, que es la regla dueña del mecanismo, la usa por primera vez ya en uso.

La comprobación del punto 3:

```bash
grep -n "Todo término que esta categoría acuña" Rules/*.md
#  once archivos: Root-Rules, Rules-Contexto, Rules-Necesidades-Negocio, Rules-Arquitectura-Tecnica,
#  Rules-Backlog-Tecnico, Rules-Plan-Sprint, Rules-Calidad-Y-Pruebas, Rules-Devops, Rules-Examples,
#  Rules-Documentacion, Rules-Prompts-AI
#  Rules-Especificacion-Funcional y Rules-UX-UI-DX NO lo tienen, y la primera es la que emite
#  el Glosario-Funcional.md al que nueve de las once apuntan
```

## 2. Por qué la normativa vigente no lo atrapa

**La primera oración del criterio es la misma en las once; la segunda dice dónde, y no hay dos
iguales.** Y en dos casos las segundas oraciones se contradicen sobre la misma clase de término.
Estas son las dos filas que importan, verificadas textualmente:

`Rules-Plan-Sprint.md` §6, línea 302, **contesta bien**:

> El vocabulario de proceso de esta categoría —sprint, incremento, velocidad, Definition of Done— es
> del framework y vive en el glosario operativo de `Master-Prompt.md` §15, **no en un glosario de
> producto**.

`Rules-Calidad-Y-Pruebas.md` §6, línea 317, ante la misma clase de término, **contesta otra cosa**:

> Los términos de prueba propios de la categoría —nivel, **sonda**, umbral, fixture— se definen en el
> plan de pruebas la primera vez que aparecen.

`sonda`, `umbral` y `fixture` son tan del método como `sprint` y `velocidad`. Acá van en línea, en el
cuerpo de un documento del producto, en su primera aparición: es un noveno destino y el único que no
es un glosario. Y contradice lo que el propio glosario operativo declara en su entrada «Glosario de
categoría»: «la regla de no duplicación manda referenciar el término ya declarado por otra categoría
en lugar de redefinirlo».

**La asimetría que explica por qué nadie lo notó**, y que es la observación que más orienta la
corrección: la regla que contesta bien lo hace **sobre términos que además ya están en el glosario
operativo** —`Definition of Done` y las demás figuran ahí—, mientras que la que contesta mal lo hace
sobre términos que **no están**. La política correcta se enunció donde ya estaba resuelta, y no donde
hacía falta.

**El problema de fondo, que no tiene salida buena para un destino.** El criterio dice «todo término
que **esta categoría** acuña o precisa». `sonda` y `pasada de diseño` no los acuña la categoría 10 de
un producto: los acuña el framework, y la categoría los usa porque el método se los impone. Las dos
lecturas dan dos malos resultados:

| Lectura | Qué produce |
|---|---|
| **Sí aplica**: el producto los declara | El glosario funcional de un producto de videovigilancia, que lo lee el Product Owner y que declara qué es un eje de motor, pasa a declarar qué es una sonda. Vocabulario del método mezclado con el del negocio, en el artefacto donde menos corresponde |
| **No aplica**: son del método | Los términos quedan sin declarar en ningún lado que el lector del producto pueda alcanzar, que es exactamente el estado que el criterio existe para evitar. Y el criterio no dice esto: hay que deducirlo de una oración que vive en otra regla |

**Y la inconsistencia se replica en el destino.** No es una discrepancia que un destino resuelve una
vez: cada categoría obedece a su regla. En la corrida real, la categoría 08 de la Fase E definió
`sonda` en una tabla dentro de `Plan-Pruebas.md` siguiendo a su regla, y la categoría 10 de la Fase G
usó la misma palabra sin definirla, porque `Rules-Examples.md` §6 manda a dos glosarios de los cuales
uno no existe todavía y el otro es del dominio del negocio. **La misma palabra quedó definida en la
08 y sin definir en la 10, con las dos categorías en cumplimiento.**

## 3. Correcciones propuestas

### G5-A · Generalizar la frase que ya está escrita

**Archivos:** §6 de las once reglas que llevan el criterio.
**Severidad:** minor.

La primera cláusula del criterio de gobierno de glosario pasa a distinguir las dos clases de
vocabulario, con la frase que `Rules-Plan-Sprint.md` §6 ya tiene:

> El vocabulario **del método** —el que el framework acuña e impone a la categoría— vive en el
> glosario operativo de `Master-Prompt.md` §15 y se cita sin redefinir. El vocabulario **del
> producto** —el que esta categoría acuña o precisa sobre el dominio— va al glosario que le
> corresponde.

Es la intervención de menor costo y mayor alcance del reporte 11: no hay que redactar una política,
hay que generalizar una que existe. Y resuelve la ambigüedad del §2 sin que ningún destino tenga que
decidir si el vocabulario del método va en el glosario que lee el Product Owner.

### G5-B · Completar el glosario operativo

**Archivo:** `Master-Prompt.md` §15.
**Severidad:** minor.

Sin esto, G5-A manda a un glosario que no tiene los términos. Entran, como mínimo, los tres que la
corrida necesitó y no encontró: **`sonda`**, **`pasada de diseño`** y **`pasada de ejecución`**. Se
suma **`arnés`**, que hoy es una metáfora usada una sola vez en `Rules-Examples.md` §0.1 —«arista B
de arnés de autovalidación»— y que el destino adoptó como término: o se define o se deja de usar, y
definirla es más barato que reescribir la regla que la introdujo.

`sonda` es el caso que decide, y conviene que su definición diga de qué es unidad: cada fila de una
matriz de sensado de deriva es una sonda, y las seis poblaciones —`SUP-XX`, `CMP-XX`, `EST-XX`,
`NAV-XX`, `DM-XX` y `VER-XX`— son tipos de sonda.

**Por qué no alcanza con que se entienda.** Un mecanismo cuya unidad se define por repetición en cada
destino se transmite por imitación. Que hasta ahora todos hayan entendido lo mismo no es una
propiedad del método: es suerte, sostenida por lo transparente que resulta la metáfora.

### G5-C · Retirar el noveno destino

**Archivo:** `Rules-Calidad-Y-Pruebas.md` §6.
**Severidad:** minor.

La cláusula «los términos de prueba propios de la categoría —nivel, sonda, umbral, fixture— se
definen en el plan de pruebas la primera vez que aparecen» se reemplaza por la distinción de G5-A:
`sonda` y `umbral` de método se citan del glosario operativo; `nivel` y `fixture` **no son del
método** —los fija cada proyecto de código en su estrategia de testing y no tienen valor único a
nivel producto— y siguen declarándose en la categoría. Esa distinción no es una invención de esta
intervención: es la que el destino hizo al construir su remedio local, y funcionó.

Ninguna regla manda definir un término en línea, en el cuerpo de un documento, como alternativa a un
glosario. Es uno de los criterios de verificación del reporte 11 §10.

### G5-D · Cerrar los dos huecos

**Archivos:** `Rules-Especificacion-Funcional.md` §6 y `Rules-UX-UI-DX.md` §6.
**Severidad:** minor.

Las dos reglas ganan el criterio de gobierno de glosario en su forma nueva. La primera es la que
emite el `Glosario-Funcional.md` al que apuntan nueve de las once restantes y es la única que no
tiene criterio sobre él.

### G5-E · Declarar el alcance real de `Vocabulario-Rules.md`

**Archivo:** `Vocabulario-Rules.md` §1 y §8.
**Severidad:** minor.

La regla se presenta como la regla de vocabulario y gobierna seis términos. Se declara explícitamente
lo que hoy hay que deducir: gobierna **los términos que colisionan con el dominio del cliente**, y el
resto del vocabulario del framework vive en el glosario operativo de `Master-Prompt.md` §15, que pasa
a ser su destino declarado. Su §8, que ya registra un pendiente declarado sobre la unidad de entrega,
es el lugar natural para registrar el alcance.

## 4. Cómo se verifica

| Caso | Comprobación | Resultado esperado |
|---|---|---|
| V1 | `grep -n "sonda" Orchestrator/Master-Prompt.md Rules/Deriva-Rules.md` | Su definición aparece antes que su primer uso |
| V2 | Las once reglas que llevan el criterio | Dan **la misma** respuesta a «¿dónde va un término que el método acuñó?», y esa respuesta no es un glosario del producto |
| V3 | Búsqueda de «se definen … la primera vez que aparecen» en las reglas | Cero coincidencias: ninguna regla manda definir en línea como alternativa a un glosario |
| V4 | Las trece reglas de categoría | Todas llevan el criterio de gobierno de glosario, incluidas las dos que hoy no lo tienen |
| V5 | Dos categorías del mismo producto que usen el mismo término del método | Lo referencian al mismo lugar, y ninguna lo redefine |
| V6 | Un destino que tiene que cumplir el criterio | No tiene que decidir si el vocabulario del método va en el glosario que lee el Product Owner |
| V7 | El inventario de vocabulario propio del framework | Existe, o su ausencia está declarada con su alcance |

## 5. Qué queda fuera de esta corrección, y por qué

**El inventario completo del vocabulario propio del framework** (propuesta 3 del reporte 11).
Recorrer las diecisiete reglas y el master-prompt buscando términos usados como técnicos que no estén
ni en los seis de `Vocabulario-Rules.md` §2 ni en las entradas del glosario operativo. G5-B incorpora
los cuatro que la corrida encontró; la pasada completa produce la lista real y queda declarada como
trabajo siguiente. Es la diferencia entre cerrar el incidente y cerrar el patrón, y hay que
declararla en vez de dejar que parezca cerrado.

**Unificar los nueve destinos** (propuesta 4). G5-A resuelve la pregunta que producía la
contradicción —dónde va el vocabulario del método— y con eso los destinos restantes dejan de
contradecirse sobre la misma clase de término. Lo que queda sin resolver es una dependencia que no
es de esta familia: si el `Glosario-Tecnico.md` de la 11 es el destino del vocabulario técnico del
producto, **seis categorías lo referencian desde fases anteriores a la suya**, que es exactamente el
patrón del reporte 07. Se resuelve con la referencia pendiente de **G3-E**, y por eso G5 depende de
G3 y no al revés.

**El «glosario de categoría» como artefacto real.** `Master-Prompt.md` §15 lo **define** como
concepto —«artefacto propio de una categoría»— y ninguna regla de categoría lo emite: es vocabulario
del framework sin materialización. Decidir si se emite o si se retira del glosario operativo excede
esta intervención y queda registrado.
