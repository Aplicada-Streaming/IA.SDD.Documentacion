# Reporte 05 — El ancho de los identificadores está fijado y el tamaño de lo que identifican no

| Campo | Valor |
|---|---|
| Reporte | 05 |
| Fecha | 2026-08-11 |
| Origen | Corrida real del orquestador sobre el destino `Repos-RPIs/RPI.VidelControl`: cierre de la Fase B2 del proyecto de código `VideoControl-Web`, 2026-08-11 |
| Versión del framework evaluada | SDD 6.0 (`Deriva-Rules` 3.1, `Root-Rules` 3.1, invariantes D3 y D4) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Deriva-Rules.md` §2.1 y §2.3; los invariantes D3 y D4 de `Root-Rules.md` |
| Naturaleza | Una convención de forma que en el caso general funciona y en el caso grande es imposible de cumplir, sin ninguna salida declarada |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |
| Reportes relacionados | `01-Ambito-De-Unicidad-De-Identificadores.md`, que documenta el otro atributo no declarado del mismo sistema de identificadores |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. El incidente](#2-el-incidente)
- [3. Por qué la regla es razonable y aun así no se puede cumplir](#3-por-qué-la-regla-es-razonable-y-aun-así-no-se-puede-cumplir)
- [4. Lo que la normativa dice y lo que no dice](#4-lo-que-la-normativa-dice-y-lo-que-no-dice)
- [5. La causa raíz](#5-la-causa-raíz)
- [6. El patrón, enunciado](#6-el-patrón-enunciado)
- [7. Qué hizo el agente y por qué esa salida no es gratis](#7-qué-hizo-el-agente-y-por-qué-esa-salida-no-es-gratis)
- [8. Propuestas de intervención](#8-propuestas-de-intervención)
- [9. Cómo verificar que la corrección funcionó](#9-cómo-verificar-que-la-corrección-funcionó)
- [10. Anexo — evidencia reproducible](#10-anexo--evidencia-reproducible)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

`Deriva-Rules.md` §2.1 exige que los identificadores de la línea de base visual sean **de dos dígitos uniformes**, «como el resto de los identificadores del template (D3, D4)».

Al cerrar la Fase B2 de `VideoControl-Web` había **191 estados aprobados** que inventariar. Dos dígitos alcanzan hasta noventa y nueve.

La regla no admite excepción, no declara qué hacer al agotarse el rango, y no hay ninguna otra parte del framework que lo resuelva. El agente tuvo que elegir entre tres salidas, todas malas, sin ningún criterio del método para preferir una.

## 2. El incidente

| Tabla de la línea de base | Elementos a identificar | ¿Entra en dos dígitos? |
|---|---|---|
| `SUP-XX` superficies | 13 | Sí, con holgura |
| `CMP-XX` componentes | 86 | Sí, con **trece de margen** |
| `NAV-XX` rutas de navegación | 20 | Sí |
| `EST-XX` estados | **191** | **No** |
| `SD-XX` sondas de la matriz de sensado (§2.3) | **374** | **No** |

Dos observaciones sobre esta tabla que importan más que el hecho aislado:

- **No es un producto desmesurado.** Trece superficies para un producto de uso doméstico, con un promedio de menos de quince estados por superficie. El desbordamiento no lo produjo un exceso: lo produjo multiplicar trece por quince.
- **Los componentes zafaron por trece.** Con dos superficies más, o con una descomposición un poco más fina, `CMP-XX` también desbordaba. El margen no es una propiedad del método, es una casualidad de este producto.

Y hay un tercer dato que agrava el diagnóstico: la matriz de sensado del §2.3 se construye **a partir de** las otras tablas, con una sonda por elemento. Su tamaño es la suma de los demás. Es decir: **la tabla que más seguro desborda es la que el framework define como derivada de todas las otras**, y es la única que no podía no desbordar.

## 3. Por qué la regla es razonable y aun así no se puede cumplir

La regla no está mal pensada. El ancho uniforme sirve para tres cosas concretas y todas valiosas:

1. Los identificadores ordenan lexicográficamente igual que numéricamente. `EST-09` antes que `EST-10` sin ninguna lógica especial.
2. Alinean en columna, que es lo que hace legible una tabla de doscientas filas.
3. Un ancho fijo es un ancho reconocible: `CU-14` se lee como identificador de un vistazo, y eso vale en un método que cruza referencias entre documentos todo el tiempo.

Las tres propiedades **se conservan con tres dígitos**. Lo que no se conserva es la uniformidad *entre familias* si unas usan dos y otras tres, que es exactamente el compromiso que hubo que tomar.

El problema no es que dos dígitos sea poco. Es que el framework fijó un ancho sin declarar de qué depende ese ancho.

## 4. Lo que la normativa dice y lo que no dice

**Dice** que los identificadores son de dos dígitos uniformes (`Deriva-Rules.md` §2.1).

**Dice** que son estables: un elemento eliminado no libera su número, su fila queda `Retirado`. Esta regla es importante para el reporte, porque **acelera el agotamiento**: el rango no se recicla, así que el techo no es «cuántos hay» sino «cuántos hubo alguna vez».

**No dice** qué pasa al llegar a noventa y nueve.

**No dice** que el ancho sea una función del tamaño esperado de la colección, ni cómo estimarlo antes de empezar a numerar.

**No dice** si el ancho es propiedad del framework, del producto o de cada tabla. Sin eso, un agente que necesita tres dígitos no sabe si está tomando una decisión local o rompiendo una convención global.

**No tiene ningún criterio de audit** que verifique que un rango de identificadores no está por agotarse. El desbordamiento no se detecta antes: se choca.

## 5. La causa raíz

El framework fijó el ancho de los identificadores **en el momento en que sus colecciones eran chicas**: casos de uso, reglas de negocio, hallazgos de audit. Todas esas colecciones tienen decenas de elementos y el ancho de dos dígitos les sobra.

La Fase B2 introdujo colecciones de otra escala. Un estado no es una unidad de especificación: es una **combinación**. Los estados salen de multiplicar superficies por situaciones, las sondas salen de sumar todas las tablas. Las colecciones combinatorias crecen con el producto de sus factores, no con la suma.

Y ahí está la causa raíz: **el framework trata el ancho del identificador como una convención tipográfica cuando es una decisión de capacidad**. Una convención tipográfica se aplica igual en todos lados. Una decisión de capacidad depende de cuánto hay que contar, y por lo tanto no puede ser la misma para una colección enumerada a mano y para una colección derivada de un producto cartesiano.

Nótese el parentesco con el reporte `01`: allá el framework declaraba la **forma** del identificador y no su **ámbito de unicidad**; acá declara la forma y no su **capacidad**. Son dos atributos distintos del mismo sistema, y en los dos casos el framework fijó lo visible y omitió lo que gobierna.

## 6. El patrón, enunciado

> **El framework fija el ancho de sus identificadores como una convención de forma, sin declarar de qué colección es función ni qué hacer cuando el rango se agota. Mientras las colecciones son enumeradas a mano, el ancho sobra y la omisión no se nota. Cuando aparece una colección derivada —un producto cartesiano, una suma de tablas—, el rango se agota, no hay salida declarada, y el agente tiene que elegir sin criterio entre romper la convención, comprimir el inventario o fragmentar el identificador.**

Dos corolarios que conviene retener aparte:

> **La regla de estabilidad de los identificadores acelera el agotamiento.** Un rango que no se recicla se dimensiona por el total histórico y no por el vigente. Las dos reglas son buenas por separado y se agravan mutuamente, y en ningún lado están enunciadas juntas.

> **La tabla que el framework define como derivada de todas las otras es la que con más seguridad desborda.** Cuando un método construye una colección a partir de otras, hereda su tamaño; y si el método fija el mismo ancho para las dos, está garantizando el choque en la derivada.

## 7. Qué hizo el agente y por qué esa salida no es gratis

Tres salidas posibles, ninguna prevista por el método:

| Salida | Qué costaba |
|---|---|
| **A.** Tres dígitos en las tablas que desbordan | Rompe la uniformidad entre familias: `SUP-01` y `EST-001` conviven |
| **B.** Identificador compuesto, `SUP-04/EST-07` | Rompe la forma del identificador en todo el framework, y las referencias de una sola pieza —«la sonda verifica `EST-XX`»— dejan de resolver |
| **C.** Comprimir el inventario agrupando estados | Rompe lo que la línea de base es: si un estado aprobado no tiene identificador, no tiene sonda, y lo que no tiene sonda no se sensa |

Se eligió **A**, con la desviación declarada en `Linea-Base-Visual.md` §3 y en la matriz de sensado, y este reporte emitido. Es la que menos rompe, y aun así rompe algo: a partir de ahora, en este destino, el ancho del identificador **ya no dice de qué familia es**.

Lo importante para el prompt de intervención no es cuál se eligió: es que **el método no ofreció ninguna razón para preferir una**. Otro agente, en otra corrida, habría elegido otra con el mismo derecho, y las dos líneas de base habrían quedado incomparables.

## 8. Propuestas de intervención

No se aplican acá. Se enumeran para el prompt que intervenga el framework.

| # | Intervención | Artefacto | Efecto esperado |
|---|---|---|---|
| P-1 | Declarar que el ancho del identificador es **función del tamaño esperado de su colección**, con una regla simple: dos dígitos hasta noventa y nueve, tres a partir de ahí, y el ancho se fija al abrir la tabla y no se cambia después | `Root-Rules.md`, D3 y D4 | El ancho deja de ser convención tipográfica y pasa a ser decisión declarada, tomada una vez y no negociada por agente |
| P-2 | Declarar el ancho **por familia y no por framework**, dentro de la propia tabla, en la línea que la encabeza | `Deriva-Rules.md` §2.1 y §2.3, reglas de categoría | `SUP-01` y `EST-001` dejan de ser una incoherencia y pasan a ser dos familias con capacidades distintas y declaradas |
| P-3 | Marcar las colecciones **derivadas** —las que se construyen a partir de otras— y exigir que su ancho se dimensione sobre la suma de sus fuentes | `Deriva-Rules.md` §2.3 | La matriz de sensado deja de ser la tabla que garantizadamente desborda |
| P-4 | Enunciar juntas la regla de estabilidad y la de ancho, con su consecuencia: **el rango se dimensiona por el total histórico** | `Root-Rules.md` | La interacción entre dos reglas buenas deja de ser un descubrimiento de cada corrida |
| P-5 | Sumar a los audits un control de **saturación de rango**: una familia que superó el ochenta por ciento de su capacidad es hallazgo P2, y una que la agotó es P0 | criterios de audit del `Master-Prompt` §10 | El desbordamiento se avisa antes de chocarse |
| P-6 | Declarar, para el caso en que el rango igualmente se agote, **cuál de las tres salidas es la del método** | `Root-Rules.md` | Dos corridas distintas del mismo framework producen líneas de base comparables |

De las seis, la P-1 es la barata y la P-6 la que importa: sin ella, el framework sigue delegando en cada agente una decisión que debería tomar una sola vez.

## 9. Cómo verificar que la corrección funcionó

1. Generar una línea de base de un proyecto de código con más de cien estados y comprobar que el ancho del identificador está declarado en la tabla, y no inferido por el agente.
2. Correr el audit sobre una familia que superó el ochenta por ciento de su rango y comprobar que lo reporta.
3. Generar dos veces la misma línea de base con agentes distintos y comprobar que eligen el mismo ancho. Hoy no está garantizado.
4. Comprobar que la tabla derivada —la matriz de sensado— dimensiona su ancho sobre la suma de sus fuentes y no sobre su propio conteo inicial.

## 10. Anexo — evidencia reproducible

```text
[EV-01 | artefacto | SDD/Devs/Rules/Deriva-Rules.md | §2.1, «Los identificadores son de dos dígitos uniformes» | 2026-08-11]
[EV-02 | linea-base | SDD/Docs/Proyectos/VideoControl-Web/03-UX-UI-DX/Linea-Base-Visual.md | §6, EST-001 a EST-191 | 2026-08-11]
[EV-03 | linea-base | SDD/Docs/Proyectos/VideoControl-Web/08-Calidad-Y-Pruebas/Matriz-Sensado-Deriva.md | §3, SD-001 a SD-374 | 2026-08-11]
[EV-04 | linea-base | SDD/Docs/Proyectos/VideoControl-Web/03-UX-UI-DX/Linea-Base-Visual.md | §5, CMP-01 a CMP-86: trece de margen sobre el techo | 2026-08-11]
```

```bash
# Reproducción del recuento de estados sobre el destino:
#   suma de las filas de la seccion 5.1 de los trece Wireframes-*.md  -> 191
#   estados que la maqueta demuestra, leidos de Datos-Maqueta.js      -> 191
#
# Reproducción del recuento de sondas:
#   13 superficies + 86 componentes + 191 estados + 20 rutas + 64 campos -> 374
```

La desviación se aplicó sobre el destino y está declarada en los dos artefactos que la usan. Ninguna corrección se aplicó al framework: este reporte existe para que esa corrección se decida con evidencia.

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-11 | Reporte inicial: el framework fija el ancho de sus identificadores como convención de forma, sin declarar de qué colección es función ni qué hacer al agotarse el rango. Verificado con el desbordamiento de `EST-XX` a 191 elementos y de `SD-XX` a 374 en el cierre de la Fase B2, con dos corolarios —la estabilidad acelera el agotamiento, y la tabla derivada es la que con más seguridad desborda— y seis propuestas de intervención. |
| 1.1 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** El ancho de los identificadores y las colecciones derivadas.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `Root-Rules.md` §9 | Ancho uniforme de **cinco dígitos** y su fundamento |
| `Deriva-Rules.md` §2.1 y §2.3 | Colecciones derivadas, con la **estabilidad y la capacidad tratadas juntas** |

**Después de la 7.0.** La **8.1** cerró el caso que quedaba: los rangos por familia, cuando el mismo número existe en dos familias distintas.

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
