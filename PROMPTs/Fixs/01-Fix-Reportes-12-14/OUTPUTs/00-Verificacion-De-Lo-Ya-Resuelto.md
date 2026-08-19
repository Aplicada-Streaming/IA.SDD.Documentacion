# 00 — Verificación de lo ya resuelto, antes de proponer nada

**Paso 2 del prompt de intervención.** Cada reporte declara la versión que evaluó y el framework
siguió publicando. Acá se comprueba, propuesta por propuesta, si ya entró — **contra los archivos
vivos, no contra el `CHANGELOG`**.

**Vigente al correr esta verificación:** SDD **9.19**, árbol limpio, `main` al día.

---

## 1. Reporte 13 — **YA RESUELTO. No hay trabajo.**

**Evaluó SDD 9.18. La 9.19 lo trató explícitamente y lo nombra.**

`CHANGELOG.md`, entrada 9.19, texto literal:

> *«Es la pregunta previa contestada sin que nadie clasifique nada — por eso **se incorpora el
> criterio y no el eje de estratos** que `Reportes/13` proponía. Un concepto más que mantener, en un
> método que declara que un procedimiento que crece deja de leerse, sólo se justifica si hace falta,
> y no hizo falta.»*

| Qué proponía el 13 | Qué pasó |
|---|---|
| El **eje de estratos** del hallazgo | **Rechazado con fundamento escrito.** No es un olvido |
| Que se distinga qué puede cerrar el agente y qué es del humano | **Incorporado** como *la pregunta previa* de `Master-Prompt.md` §8.1 — verificado sobre el archivo vivo, `Master-Prompt` está en **8.7** |
| Encargo al auditor: refutar, cita literal, «no concluyente» admitido | **Incorporado** en `Master-Prompt.md` §10 |

**Conclusión: el reporte 13 se cierra sin intervención.** Aplicarle algo sería reabrir una decisión
tomada con fundamento hace un día.

---

## 2. Reporte 12 — **NO ES UN FIX. Es una decisión, y no es del agente.**

**El propio reporte lo declara en su cabecera:**

> *Naturaleza: **Una propuesta de alcance, no un defecto.** El framework funciona; la pregunta es si
> debe incorporar código ejecutable.*

Y tiene dos secciones que lo confirman: **§5 «Lo que hay que decidir antes, y no es de
implementación»** y **§7 «Qué haría falta para decidir»**.

**Por la pregunta previa de `Master-Prompt.md` §8.1 esto es una detención legítima**, y no trabajo
propio: *«se detiene lo que no tiene cita posible: lo que requiere intención de producto, autoridad,
o una preferencia que el árbol no contiene»*. Si el `Framework SDD` debe pasar de ser un corpus de
reglas a distribuir un verificador ejecutable **es intención de producto pura**. Ningún archivo del
árbol la contesta.

**Conclusión: el reporte 12 se eleva al Product Owner y no se aplica.** Se declara en el plan como
decisión pendiente, con su §5 y su §7 como insumo.

---

## 3. Reporte 14 — **PENDIENTE ENTERO. Ninguna de sus cinco propuestas entró.**

Evaluó SDD 9.19, que es la vigente, así que no hubo publicaciones intermedias. Verificado igual,
propuesta por propuesta, contra los archivos vivos:

| # | Propuesta | Estado | Evidencia sobre el archivo vivo |
|---|---|---|---|
| 6.1 | El ítem diferido es una figura declarada | **No existe** | `Root-Rules.md` §12 declara la *referencia pendiente*, que es **otra cosa**: gobierna referencias a artefactos, no ítems de contenido de una §4.x |
| 6.2 | El evento de cierre nombra un artefacto y una sección | **No** | §12 punto 3 pide *«cuándo se cierra: qué evento obliga a volver»*, y **no exige que el evento sea observable**. «El punto de control de la etapa `a`» satisface esa redacción |
| 6.3 | La compuerta mecánica los cuenta | **No** | `Master-Prompt.md` §10.0 tiene **cinco** comprobaciones transversales —enlaces y anclas, recuentos anclados, idempotencia, identificadores, anclaje de referencias— y **ninguna mira ítems diferidos ni eventos vencidos** |
| 6.4 | Los ítems que empaquetan dos decisiones se separan | **No** | `Rules-Devops.md` §4.3 punto 3 sigue pidiendo *«Herramienta de versionado … Configuración base **y** prefijo de tag»* en una sola línea |
| 6.5 | El orquestador de reanudación lo mira | **No** | `Master-Prompt-Reanudacion.md` §1 resuelve **seis** dimensiones y ninguna pregunta *«¿qué se difirió y ya venció?»* |

---

## 4. Y el hallazgo que orienta el diseño de la corrección

**`Root-Rules.md` §12 ya resuelve el caso hermano, y funciona. Conviene ver por qué.**

Su cierre lo dispara un evento **interno a la orquestación del método**: *«cuando la categoría
referenciada se emite, las categorías que la referenciaban vuelven a la cola»*. **El orquestador
observa ese evento porque él mismo lo produce.**

**El caso del reporte 14 es el mismo mecanismo con el evento afuera.** «El punto de control de la
etapa `a`» ocurre en el ciclo de construcción del producto, que **el método declara que no
gobierna**. Nadie del método lo ve pasar, y por eso el diferimiento sobrevivió ocho etapas.

**Consecuencia de diseño, y es la que evita inventar un concepto nuevo:** no hace falta una figura
paralela a §12. Hace falta que **§12 alcance también a los ítems de contenido** y que, cuando el
evento de cierre es externo al método, **algo lo comprueba en lugar de confiar en que alguien vuelva**.

**Es además lo que el propio framework pide.** La 9.19 rechazó el eje de estratos del reporte 13 con
este argumento: *«un concepto más que mantener, en un método que declara que un procedimiento que
crece deja de leerse, sólo se justifica si hace falta»*. **Acá tampoco hace falta.**

---

## 5. Alcance real de la intervención

| Reporte | Desenlace |
|---|---|
| **12** | **Elevado al Product Owner.** Decisión de alcance, no defecto |
| **13** | **Cerrado sin trabajo.** Resuelto en 9.19 con fundamento escrito |
| **14** | **Único aplicable.** Sus cinco propuestas, ninguna implementada |

**La agrupación que el prompt pedía verificar no se sostiene como estaba planteada.** Los tres son
del mismo eje —el método declara algo y no comprueba que haya ocurrido—, pero **están en tres
estados distintos**: uno resuelto, uno que es una pregunta y uno pendiente. Tratarlos como un solo
lote habría reabierto el `13` y habría hecho que un agente decidiera el `12`.
