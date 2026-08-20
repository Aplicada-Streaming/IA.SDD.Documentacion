# Reporte 15 — El barrido exige enumerar sus exclusiones y no dice cuáles son necesarias

| Campo | Valor |
|---|---|
| Reporte | 15 |
| Fecha | 2026-08-20 |
| Origen | Tres intervenciones consecutivas del framework y de un destino real —la publicación de SDD 10.0, la migración 9.12 → 10.0 y su reparación de nomenclatura—, en las que la misma clase de exclusión se descubrió **después** de que el barrido la levantara |
| Versión del framework evaluada | SDD **10.0** (`SDD-Development-Guide.md` §VI.3 comprobación 8 y §VI.3.2) |
| Artefactos del framework alcanzados | `SDD/Guides/SDD-Development-Guide.md` §VI.3.2 |
| Naturaleza | Un hueco de método **más chico de lo que la primera lectura sugería**. §VI.3.2 **ya enumera seis clases** de exclusión; falta **una**, y el resto de los incidentes son de una clase **ya declarada que nadie consultó** |
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

**Y §VI.3.2 ya hizo la mitad del trabajo.** Enumera **seis clases** de exclusión y declara
explícitamente que están ahí *«para que no se redescubran cada vez»*. **La corrección de este reporte
es por lo tanto mucho más chica de lo que su primera versión afirmaba**, y el hallazgo se parte en dos
mitades de naturaleza distinta:

| | Qué es | Estado en el framework |
|---|---|---|
| **A · La declaración misma** | El documento que **declara el patrón** para poder convertirlo lo contiene, por definición. §VI.3.2 exige escribir la forma anterior **como patrón literal**, de modo que **toda** intervención que declare uno produce este residuo | **NO está entre las seis.** Es el hueco real |
| **B · El registro emitido con fecha** | Informes, notas y entradas cerradas describen el estado de un momento | **YA está**, y por triplicado: «Filas de control de cambios», «Notas de coherencia anteriores» y `_legacy/` |

**La mitad B no es un hueco del framework: es evidencia de que la lista no se consulta.** Fue escrita
para no redescubrirse y se redescubrió igual — no porque falte, sino porque **nada pone la lista
delante de quien corre el barrido** en el momento en que mira su residuo.

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
| 1 | **SDD 10.0**, publicación | `Root-Rules.md §12` sin subsección | **1**, en `Coherencia-Reportes-00-11.md` | **B — ya declarada** | **Después**, y **escrita a mano** en lugar de citada: la fila «Notas de coherencia anteriores» de §VI.3.2 ya la cubría |
| 2 | **Migración 9.12 → 10.0** de un destino real | `Root-Rules.md §12` sin subsección | **2**, en el plan de migración y en el bloque de procedencia | **A — el hueco** | **No se enumeró.** Quedó como hallazgo **P2** (`M-01`) de la auditoría de M6 |
| 3 | **Reparación de nomenclatura** del mismo destino | `Dónde se cierra (artefacto y sección)` | **8**, en el plan de conversión | **A — el hueco** | **Antes**, y sólo porque el incidente 2 acababa de ocurrir |

**El incidente 2 es el que da la medida.** No es que la exclusión se escribiera tarde: **no se
escribió**, la migración afirmó «superficie CERO» con dos ocurrencias vivas, y el defecto lo levantó
una auditoría posterior como **P2**. La afirmación era sustantivamente correcta y **literalmente
falsa**, que es la peor combinación para un registro que otros van a leer.

**Y el incidente 3 muestra por qué esto no se arregla con atención.** Ahí sí se enumeró de entrada,
**y sólo porque el mismo agente venía de cometer el 2 dos horas antes**. Una corrección que depende
de que la anterior esté fresca no es una corrección: es memoria.

---

## 4. La causa raíz, y son dos

**La primera es un faltante simple:** de las clases que la mecánica del barrido produce **siempre**,
§VI.3.2 enumera seis y **le falta la más autorreferencial de todas** — el documento que declara el
patrón. Los incidentes 2 y 3 son exactamente ésa.

**La segunda es más incómoda y explica el incidente 1.** La lista existe, dice de sí misma que está
ahí «para que no se redescubran cada vez», **y las tres intervenciones la escribieron a mano en lugar
de citarla**. El motivo es de ubicación y no de contenido: la lista vive en `SDD-Development-Guide.md`
§VI.3.2, y **la nota de coherencia se escribe mirando el residuo**, no la guía. Nada, en el momento
en que el autor enumera, lo manda a leer lo que ya está enumerado.

**Es una variante del reporte `04`** —un dato que se escribe a mano cuando podría derivarse— con un
agravante propio: acá **el trabajo ya estaba hecho** y aun así se rehízo.

---

## 5. El patrón, enunciado

> **Una lista de excepciones que vive lejos del momento en que se enumera se reescribe a mano aunque
> esté completa.** Enumerarla una vez —que es lo correcto y lo que §VI.3.2 hizo— no alcanza si nada
> pone la lista delante de quien mira su residuo: el autor la reconstruye desde lo que ve, de modo que
> **acierta en lo que le apareció y omite lo que no**. Y cuando la mecánica de la comprobación produce
> una excepción que la lista no tiene, esa omisión no se nota: la comprobación afirma cero y hay
> residuo vivo, que es una afirmación **sustantivamente correcta y literalmente falsa**.

---

## 6. Propuestas de intervención

**Punto de partida, no decisión tomada.**

### 6.1 La séptima clase entra en la tabla de §VI.3.2

Una fila más, con la forma de las seis que ya están:

| Clase | Por qué se excluye |
| --- | --- |
| **La declaración de la propia intervención** | Escribe la forma anterior **como patrón literal** porque §VI.3.2 se lo exige; nombrarla es su función. Un barrido que no pudiera nombrar lo que corrige sería inútil |

**Costo: una fila.** Cubre los incidentes 2 y 3.

### 6.1.b Y la nota de coherencia **cita** la lista en vez de reconstruirla

Que §VI.3 declare que la sección de barrido **enumera sólo las exclusiones propias del caso** y
**cita** §VI.3.2 para las estables. Es lo que cubre el incidente 1, y es la mitad que la fila nueva
no arregla: sin esto, la próxima intervención vuelve a escribir a mano una lista que ya existe.

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

- [ ] [enumerable] §VI.3.2 enumera **siete** clases, y la séptima es la declaración de la propia intervención.
- [ ] [enumerable] La comprobación 8 de §VI.3 expresa su resultado esperado **incluyendo** las estructurales.
- [ ] [enumerable] Ninguna nota de coherencia posterior **reescribe** una clase que §VI.3.2 ya enumera: la cita.
- [ ] [interpretativo] Sobre los tres incidentes de §3, la corrección **los habría evitado a los tres**: la fila nueva cubre el 2 y el 3, y la cita obligatoria cubre el 1.

---

## 8. Lo que este reporte no sabe

- **Si falta alguna clase además de la séptima.** Se observaron tres incidentes y **una sola clase
  ausente**. Un barrido sobre un concepto con presencia en código —y no sólo en documentación— podría
  producir otra, y **este reporte no la busca**.
- **Por qué las tres intervenciones no leyeron la lista.** Se propone una explicación —vive lejos del
  momento en que se enumera— y **no se verificó contra ningún otro autor**: las tres corridas fueron
  del mismo agente, de modo que la muestra es de uno.
- **Cuánto residuo de clase B hay hoy en destinos con historia larga.** No se midió: el incidente 1
  dio **1**, sobre un árbol de framework, y nadie contó el equivalente en un destino de producto.
- **Si la clase B debería excluirse o resolverse de otro modo.** Un registro emitido no se reescribe,
  pero un registro que cita una sección que ya no existe **queda apuntando a la nada**. Este reporte
  propone excluirlo del barrido; **no resuelve qué pasa con su enlace**, y ésa puede ser una pregunta
  distinta.

## 9. Control de cambios

| Versión | Fecha | Cambios |
|---|---|---|
| 1.1 | 2026-08-20 | **Corrección sustantiva antes de aplicarse.** La emisión 1.0 afirmaba que faltaban **dos** clases estructurales de exclusión; **§VI.3.2 ya enumera seis**, y una de las dos —el registro emitido con fecha— **está entre ellas por triplicado**. El reporte se reescribe: el hueco real es **una** clase, la declaración de la propia intervención, y el incidente 1 deja de ser evidencia de un faltante para pasar a ser evidencia de que **la lista existe y no se consulta**. Se agrega la segunda causa raíz —la lista vive lejos del momento en que se enumera—, su propuesta correlativa —que la nota **cite** en vez de reconstruir— y la constancia de que las tres corridas fueron **del mismo agente**, de modo que esa explicación no está verificada contra nadie más. **El defecto de la 1.0 es el mismo que el reporte describe**: enumerar desde lo que uno ve en lugar de leer lo que ya está escrito. |
| 1.0 | 2026-08-20 | Emisión inicial. Nace de **tres intervenciones consecutivas** —la publicación de SDD 10.0, la migración 9.12 → 10.0 de un destino real y su reparación de nomenclatura— en las que el barrido de §VI.3.2 levantó un residuo que su autor sabía legítimo y **enumeró después, tarde o nunca**. Declara **dos clases estructurales de exclusión** —la declaración misma y el registro emitido con fecha—, con el argumento de que **ninguna depende del concepto barrido** y por lo tanto pueden derivarse una vez. La evidencia incluye el incidente en que la exclusión **no se escribió**: la migración afirmó «superficie CERO» con dos ocurrencias vivas y una auditoría posterior lo levantó como **P2**, una afirmación **sustantivamente correcta y literalmente falsa**. Y el incidente en que sí se enumeró de entrada, **sólo porque el anterior estaba fresco** — que es memoria y no corrección. Tres propuestas, y la tercera es una advertencia: **no convertir el barrido en un guion**, porque eso cae en la decisión de alcance que el reporte `12` declara sin tomar. |
