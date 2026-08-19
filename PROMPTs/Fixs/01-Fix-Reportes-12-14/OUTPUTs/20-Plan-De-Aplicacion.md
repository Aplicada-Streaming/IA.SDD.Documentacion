# 20 — Plan de aplicación, y la decisión que lo bifurca

**Alcance real, según `00-Verificacion-De-Lo-Ya-Resuelto.md`:** el `13` está **resuelto** en 9.19, el
`12` es una **decisión y no un fix**, y el `14` es **el único aplicable**.

---

## 1. El diseño, y por qué no crea un concepto nuevo

`Root-Rules.md` **§12** ya resuelve el caso hermano —la *referencia pendiente*— y funciona porque su
evento de cierre es **interno a la orquestación**: cuando la categoría referenciada se emite, el
orquestador lo sabe **porque él mismo lo produce**.

**El caso del `14` es el mismo mecanismo con el evento afuera.** «El punto de control de la etapa
`a`» ocurre en el ciclo de construcción, que el método **declara que no gobierna**. Nadie del método
lo ve pasar.

**Entonces no hace falta una figura paralela: hace falta que §12 alcance a los ítems de contenido, y
que el evento de cierre sea observable.** Es además lo que el propio framework exige: la 9.19 rechazó
el eje de estratos del `13` con el argumento de que *«un concepto más que mantener… sólo se justifica
si hace falta»*.

---

## 2. La bifurcación, y es del Product Owner

**Las dos opciones corrigen el hueco. Se diferencian en a quién le cuestan.**

### Opción A · **Detección** — el conjunto sube **minor** (9.20)

| | |
|---|---|
| **Qué se toca** | `Master-Prompt.md` §10.0 suma una **sexta comprobación transversal**: barrer los ítems diferidos y **levantar el que nombre un evento de cierre ya ocurrido**. `Master-Prompt-Reanudacion.md` R0 paso 4 suma el mismo barrido a los pendientes declarados |
| **Qué NO se toca** | Ninguna regla de categoría. **Ningún documento generado deja de cumplir** |
| **Qué resuelve** | **El lazo que hoy no cierra nadie**, que es el criterio de aceptación decisivo del reporte |
| **Qué NO resuelve** | El diferimiento sigue escribiéndose en prosa, así que **la detección es heurística**: encuentra lo que se parezca a un diferimiento, no lo que esté marcado como tal |
| **Costo para los destinos** | **Cero.** No hay migración |
| **Efecto en Lab-Geometria** | La próxima corrida habría levantado el prefijo de etiqueta como ítem vencido |

### Opción B · **Forma declarada** — el conjunto sube **major** (10.0)

| | |
|---|---|
| **Qué se toca** | Además de lo anterior: **§12 se extiende a ítems de contenido** con forma obligatoria de cuatro campos, y `Rules-Devops.md` §4.3 punto 3 **se parte en dos** —herramienta y prefijo— |
| **Qué resuelve** | Lo mismo, **y además vuelve el diferimiento contable en vez de heurístico**: con marca, la comprobación es exacta |
| **Costo para los destinos** | **Alto y obligatorio.** Un documento con un diferimiento en prosa **deja de cumplir** (§VI.1), de modo que el conjunto sube **major** y **todo destino existente entra en migración** |
| **Obligación correlativa** | La entrada del `CHANGELOG.md` lleva el bloque «Impacto sobre destinos existentes» con sus tres tablas (§VI.4) |
| **Efecto en Lab-Geometria** | Migración normativa **9.19 → 10.0**, la sexta de ese destino, justo cuando entra en su primer despliegue real |

---

## 3. Recomendación: **A ahora, B cuando haya un segundo caso medido**

**Tres motivos, y el tercero es el que manda.**

1. **La detección es lo que el reporte pide.** Su criterio decisivo es *«alguna compuerta levanta un
   ítem diferido cuyo evento de cierre ya ocurrió»*. **`A` lo cumple entero.** La marca de `B` hace
   la detección más exacta, no la hace posible.
2. **`B` cobra a todos los destinos por un defecto medido en uno.** El propio reporte `14` lo declara
   en «lo que no sabe»: *«la medición es de **uno**»*. Un major obliga a migrar a destinos que quizá
   no tienen ni un diferimiento.
3. **`A` produce la medición que `B` necesita.** Con la comprobación corriendo, la próxima corrida
   sobre cada destino **dice cuántos diferimientos hay y cuántos están vencidos**. Si son pocos, `B`
   nunca hizo falta; si son muchos, `B` se decide **con el dato en la mano** en vez de con un caso.

**Y hay un precedente del propio framework para este orden.** La compuerta mecánica de §10.0 nació
así: el reporte `09` la propuso, entró como comprobaciones transversales, y **recién en la 9.13** —con
97 anti-patrones ya marcados y contados— se le conectó el segundo conjunto de reglas. Primero medir,
después obligar.

---

## 4. Lo que se aplica bajo la opción A

| # | Archivo | Cambio | Versión |
|---|---|---|---|
| 1 | `Orchestrator/Master-Prompt.md` | §10.0 suma la comprobación transversal **6 · ítems diferidos**: se cuentan, y **es hallazgo el que nombre un evento de cierre ya ocurrido** | 8.7 → **8.8** (minor) |
| 2 | `Orchestrator/Master-Prompt-Reanudacion.md` | R0 paso 4 suma el barrido a los pendientes declarados, y R1 lo publica | 1.7 → **1.8** (minor) |
| 3 | `Rules/Catalogo-De-Criterios.md` | Registra la comprobación nueva, por la comprobación 12 de `§VI.3` —«quien toca, registra»— | minor |
| 4 | `CHANGELOG.md` + nota de coherencia + snapshot `_legacy/9.19/` | Conjunto **9.20** | minor |

**Ninguna regla de categoría se toca, y por eso ningún destino migra.**

## 5. Lo que se eleva y no se aplica

| Qué | Por qué |
|---|---|
| **Reporte 12** | Decisión de alcance: si el framework debe distribuir un verificador ejecutable. Su §5 y su §7 son el insumo. **`Master-Prompt.md` §8.1: requiere intención de producto** |
| **La opción B** | Cobra una migración a todos los destinos. Se decide con la medición que `A` produce |
