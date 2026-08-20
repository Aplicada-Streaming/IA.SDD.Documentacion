# Reporte 15 — El barrido exige enumerar sus exclusiones y no dice cuáles son necesarias

| Campo | Valor |
|---|---|
| Reporte | 15 |
| Fecha | 2026-08-20 |
| Origen | Tres intervenciones consecutivas del framework y de un destino real —la publicación de SDD 10.0, la migración 9.12 → 10.0 y su reparación de nomenclatura—, en las que la misma clase de exclusión se descubrió **después** de que el barrido la levantara |
| Versión del framework evaluada | SDD **10.0** (`SDD-Development-Guide.md` §VI.3 comprobación 8 y §VI.3.2) |
| Artefactos del framework alcanzados | `SDD/Guides/SDD-Development-Guide.md` §VI.3.2 |
| Naturaleza | Un hueco de método, **chico y con daño acotado**. No es un defecto de la regla: es una derivación que la regla no hace y que cada intervención rehace |
| Estado | **Para evaluación.** Ninguna modificación aplicada por este reporte |
| Reportes relacionados | `04-Recuentos-Declarados-En-Prosa.md`, del que hereda la forma —una obligación que se cumple a mano y nadie deriva—; y `14`, cuya intervención produjo dos de los tres incidentes |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**, y
sigue la forma de los reportes `00` a `14`.

---

## 1. Resumen

**`SDD-Development-Guide.md` §VI.3.2 exige que el barrido de una intervención deje residuo cero
«fuera de las exclusiones **enumeradas con su motivo**», y no declara qué exclusiones son
estructuralmente necesarias.** El resultado es que **cada intervención las redescubre**, y siempre en
el mismo orden: se corre el barrido, aparece un residuo que el autor sabe que es legítimo, y la
exclusión se escribe **después**.

**Medido en tres intervenciones consecutivas, y en las tres el residuo era de una de dos clases:**

| Clase | Qué es | Por qué es **necesaria** y no contingente |
|---|---|---|
| **A · La declaración misma** | El documento que **declara el patrón** para poder convertirlo lo contiene, por definición | §VI.3.2 exige que la forma anterior se escriba **como patrón literal**. Un barrido que no pudiera nombrar lo que corrige sería inútil, de modo que **toda** intervención que declare un patrón produce este residuo |
| **B · El registro emitido con fecha** | Informes de auditoría, notas de coherencia y entradas de registro **describen el estado de un momento** | Reescribirlos haría decir a un documento cerrado algo que no dijo. **Toda** intervención sobre un árbol con historia atraviesa esta clase |

**Ninguna de las dos depende del concepto que se esté barriendo.** Son consecuencia de la mecánica
del barrido, no del caso — y por lo tanto se pueden derivar una vez en lugar de redescubrirse cada
vez.

---

## 2. Lo que el framework ya resuelve bien

- **El barrido como patrón existe y funciona.** §VI.3.2 nació de que cinco intervenciones seguidas
  cometieron el defecto que corregían, y **la obligación de escribir la forma anterior como cadena
  literal es la que hace verificable la comprobación 8**. Nada de eso se toca.
- **La exigencia de enumerar con motivo es correcta.** Una exclusión sin motivo es un permiso, y ahí
  el barrido se apaga. El reporte **no propone quitarla**.
- **La regla 4 sobre el texto propio ya está escrita**, y es la que hace que el residuo de clase A
  aparezca en lugar de pasar inadvertido. **El framework ya detecta el caso; lo que falta es
  derivarlo.**

---

## 3. La evidencia: tres incidentes en tres intervenciones consecutivas

| # | Intervención | Patrón barrido | Residuo | Clase | Cuándo se enumeró la exclusión |
|---|---|---|---|---|---|
| 1 | **SDD 10.0**, publicación | `Root-Rules.md §12` sin subsección | **1**, en `Coherencia-Reportes-00-11.md` | **B** | **Después** de correr el barrido |
| 2 | **Migración 9.12 → 10.0** de un destino real | `Root-Rules.md §12` sin subsección | **2**, en el plan de migración y en el bloque de procedencia | **A** | **No se enumeró.** Quedó como hallazgo **P2** (`M-01`) de la auditoría de M6 |
| 3 | **Reparación de nomenclatura** del mismo destino | `Dónde se cierra (artefacto y sección)` | **8**, en el plan de conversión | **A** | **Antes**, y sólo porque el incidente 2 acababa de ocurrir |

**El incidente 2 es el que da la medida.** No es que la exclusión se escribiera tarde: **no se
escribió**, la migración afirmó «superficie CERO» con dos ocurrencias vivas, y el defecto lo levantó
una auditoría posterior como **P2**. La afirmación era sustantivamente correcta y **literalmente
falsa**, que es la peor combinación para un registro que otros van a leer.

**Y el incidente 3 muestra por qué esto no se arregla con atención.** Ahí sí se enumeró de entrada,
**y sólo porque el mismo agente venía de cometer el 2 dos horas antes**. Una corrección que depende
de que la anterior esté fresca no es una corrección: es memoria.

---

## 4. La causa raíz

**§VI.3.2 declara una obligación de resultado —residuo cero fuera de exclusiones enumeradas— sin
declarar su derivación.** Las exclusiones quedan como una lista abierta que cada autor construye
observando su propio residuo, y observar el residuo **es posterior a correr el barrido**. La regla
pide, en los hechos, que se enumere de antemano algo que sólo se ve después.

**Es la misma forma del reporte `04`**: un dato que la regla obliga a escribir y no obliga a derivar,
de modo que se escribe a mano, tarde, o no se escribe.

---

## 5. El patrón, enunciado

> **Cuando una regla exige enumerar las excepciones a una comprobación y no declara cuáles son
> estructuralmente necesarias, la enumeración se vuelve un descubrimiento en lugar de una
> derivación.** Quien interviene sólo puede escribirla después de ver el residuo, de modo que el
> resultado depende de que recuerde mirarlo; y las excepciones que la mecánica de la comprobación
> produce **siempre** —no las del caso— se redescubren una vez por intervención, hasta que alguna las
> omita y la comprobación afirme algo literalmente falso.

---

## 6. Propuestas de intervención

**Punto de partida, no decisión tomada.**

### 6.1 §VI.3.2 declara las dos exclusiones estructurales, y se dan por enumeradas

Que la regla las escriba una vez, con su motivo, y declare que **no hace falta repetirlas** en cada
nota de coherencia:

> **Dos exclusiones son estructurales y se dan por enumeradas.** **La declaración misma** —el
> documento que escribe el patrón para poder convertirlo lo contiene por definición— y **el registro
> emitido con fecha** —informes, notas y entradas cerradas, que describen el estado de un momento—.
> La intervención **no las repite**; sí enumera cualquier otra, con su motivo.

**Costo: tres líneas.** Elimina las dos clases que produjeron los tres incidentes.

### 6.2 La comprobación 8 distingue residuo estructural de residuo real

Que el resultado esperado deje de ser «cero» a secas y pase a **«cero fuera de las estructurales y de
las enumeradas»**, para que un residuo de clase A o B **no obligue a escribir nada** y un residuo
distinto siga siendo hallazgo.

### 6.3 Lo que **no** conviene hacer

**Convertir el barrido en un guion que excluya solo.** Es la tentación obvia y cae en el reporte `12`,
que declara que si el framework debe distribuir código ejecutable **es una decisión de alcance sin
tomar**. Mientras no se tome, la corrección tiene que vivir en la regla.

---

## 7. Cómo verificar que la corrección funcionó

- [ ] [enumerable] §VI.3.2 declara **las dos** exclusiones estructurales con su motivo.
- [ ] [enumerable] La comprobación 8 de §VI.3 expresa su resultado esperado **incluyendo** las estructurales.
- [ ] [enumerable] Ninguna nota de coherencia posterior repite las dos estructurales en su enumeración.
- [ ] [interpretativo] Sobre los tres incidentes de §3, la corrección **los habría evitado a los tres** sin que el autor tuviera que mirar el residuo.

---

## 8. Lo que este reporte no sabe

- **Si hay una tercera clase estructural.** Se observaron dos en tres intervenciones. Un barrido sobre
  un concepto con presencia en código —y no sólo en documentación— podría producir otra, y **este
  reporte no la busca**.
- **Cuánto residuo de clase B hay hoy en destinos con historia larga.** No se midió: el incidente 1
  dio **1**, sobre un árbol de framework, y nadie contó el equivalente en un destino de producto.
- **Si la clase B debería excluirse o resolverse de otro modo.** Un registro emitido no se reescribe,
  pero un registro que cita una sección que ya no existe **queda apuntando a la nada**. Este reporte
  propone excluirlo del barrido; **no resuelve qué pasa con su enlace**, y ésa puede ser una pregunta
  distinta.

## 9. Control de cambios

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 2026-08-20 | Emisión inicial. Nace de **tres intervenciones consecutivas** —la publicación de SDD 10.0, la migración 9.12 → 10.0 de un destino real y su reparación de nomenclatura— en las que el barrido de §VI.3.2 levantó un residuo que su autor sabía legítimo y **enumeró después, tarde o nunca**. Declara **dos clases estructurales de exclusión** —la declaración misma y el registro emitido con fecha—, con el argumento de que **ninguna depende del concepto barrido** y por lo tanto pueden derivarse una vez. La evidencia incluye el incidente en que la exclusión **no se escribió**: la migración afirmó «superficie CERO» con dos ocurrencias vivas y una auditoría posterior lo levantó como **P2**, una afirmación **sustantivamente correcta y literalmente falsa**. Y el incidente en que sí se enumeró de entrada, **sólo porque el anterior estaba fresco** — que es memoria y no corrección. Tres propuestas, y la tercera es una advertencia: **no convertir el barrido en un guion**, porque eso cae en la decisión de alcance que el reporte `12` declara sin tomar. |
