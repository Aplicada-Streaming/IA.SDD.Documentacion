# Reporte 07 — Cuatro categorías tienen que referenciar un artefacto que se emite en una fase posterior

| Campo | Valor |
|---|---|
| Reporte | 07 |
| Fecha | 2026-08-11 |
| Origen | Corrida real del orquestador sobre el destino `Repos-RPIs/RPI.VidelControl`: Fases D y E, categorías 06, 07 y 08 de los cinco proyectos de código, 2026-08-11 |
| Versión del framework evaluada | SDD 6.0 (`Master-Prompt` §6; `Rules-Contexto` §4.2; `Rules-Backlog-Tecnico` §3.4 y §6; `Rules-Plan-Sprint` §4.2, §4.5 y §6; `Rules-Calidad-Y-Pruebas` §2.1, §6 y §8) |
| Artefactos del framework alcanzados | El plan de generación por categoría de `Master-Prompt.md` §6 y las cuatro reglas de categoría citadas |
| Naturaleza | Obligaciones que apuntan hacia adelante en el orden de fases: una categoría tiene que referenciar, o satisfacer una condición sobre, un artefacto que todavía no existe. Las distancias medidas van de una a cinco fases |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |
| Reportes relacionados | `03-Conjuntos-Cerrados-Entre-Categorias.md`, que documenta otra forma de conflicto entre categorías: dos que afirman cosas incompatibles sobre el mismo conjunto |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. Los incidentes](#2-los-incidentes)
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

El framework ordena las fases por dependencia de contenido: una categoría se genera después de aquellas cuyo contenido consume. Ese orden es correcto y no es lo que este reporte cuestiona.

Lo que este reporte documenta es que **varias reglas de categoría crean obligaciones en la dirección contraria**. La condición de terminado canónica vive en la categoría 08, que se emite en la Fase E, y tres categorías anteriores tienen que referenciarla: la 00 en la Fase A —**cinco fases antes**, contando la B2, que el plan enumera como fila propia— y la 06 y la 07 en la Fase D, una fase antes. La categoría 08 exige, en la Fase E, una matriz que sólo la categoría 10 puede poblar, dos fases más adelante. La categoría 11 se planifica en la Fase H, y la 07 —Fase D— tiene que incluir en la condición de terminado de cada iteración que sus documentos queden revisados, **cuatro fases antes de que exista siquiera su índice**.

La distancia importa y por eso se declara caso por caso: no es la misma situación una referencia que se resuelve en la fase siguiente que una que espera cinco.

En los cuatro casos, el agente que escribe llega a una instrucción que no puede cumplir tal como está redactada, y **el framework no declara qué hacer mientras tanto**. Las dos salidas que quedan disponibles son malas de maneras distintas y las dos se usan: copiar el contenido, que crea una segunda fuente de algo que otra regla declara fuente única, o dejar una referencia colgada.

## 2. Los incidentes

**Incidente A — la condición de terminado, en la Fase A.** `Rules-Contexto.md` §4.2 exige que `Acuerdo-Equipo.md` tenga una sección §5 «Definition of Done (referencia a 08)». El documento se emite en la Fase A. La categoría 08 se emite en la Fase E. En la corrida real, el agente de la Fase A resolvió **enumerando los ocho criterios** en el propio acuerdo de equipo, con la aclaración de que el detalle de la estrategia de verificación vive en la 08. Es una decisión razonable —un acuerdo de equipo sin condición de terminado no sirve de nada— y produce una segunda fuente de la condición de terminado, escrita cinco fases antes que la que la 08 va a declarar única.

Una precisión sobre dónde está esa declaración de fuente única, porque cambia su peso: «`Definition-Of-Done.md` es la única fuente; los sprint plans referencian, no redefinen» está en `Rules-Calidad-Y-Pruebas.md` §8, que es el **prompt-snippet sugerido** para el subagente. No es una sección normativa. Los criterios de aceptación de §6 de esa regla **sí cubren una parte del problema y no la otra**, y la distinción es la que importa acá: §6 exige que «la DoD no se redefine en sprint plans; los sprint plans referencian este documento», de modo que una copia hecha en un plan de sprint está alcanzada. **La copia hecha en la Fase A, dentro del acuerdo de equipo, no la alcanza ningún criterio de ninguna regla**: no es un plan de sprint, y `Rules-Contexto.md` no tiene un criterio equivalente. El incidente A cae exactamente en el hueco entre las dos reglas, y ése es el hallazgo.

**Incidente B — la condición de terminado, en la Fase D.** `Rules-Backlog-Tecnico.md` §3.4 exige que la condición de listo referencie «la Definition of Done de 08 como filtro de salida (sin solaparlas)», y `Rules-Plan-Sprint.md` §4.2 punto 5 exige «referencia explícita a la DoD canónica del proyecto de código (vive en 08)», con su criterio de aceptación correspondiente en §6. Los dos artefactos se emiten en la Fase D. El documento referenciado no existe y su contenido no se puede citar, de modo que la referencia no se puede verificar: no hay forma de comprobar que la condición de listo no se solapa con una condición de terminado que todavía no está escrita.

**Incidente C — la categoría 11, en la Fase D.** `Rules-Plan-Sprint.md` §4.5 es la más dura de las cuatro: «un sprint no se declara cerrado con documentos de la categoría 11 afectados por sus ítems y sin revisar», y §6 lo convierte en criterio de aceptación. La categoría 11 se planifica en la Fase H y se construye en la Fase I. En la Fase D no existe ni su índice, de modo que la condición no se puede enunciar en concreto: no hay conjunto de documentos que enumerar.

**Incidente D — la matriz de sensado, en la Fase E.** `Rules-Calidad-Y-Pruebas.md` §2.1 y §6 exigen que un proyecto de código con categoría 10 emita `Matriz-Sensado-Deriva.md` con una fila `VER-XX` por cada contrato de verificación de sus ejemplos, y agregan que «no existe `Matriz-Sensado-Deriva.md` sin filas: una matriz vacía es un proyecto de código sin instrumento de sensado, no un proyecto de código conforme». La categoría 10 se emite en la **Fase G**, dos fases después de la 08. En la corrida real, los cuatro proyectos de código sin interfaz visual no pudieron emitir la matriz —no hay con qué poblarla— y la ausencia quedó declarada como hueco en su matriz de cobertura. Este incidente tiene una particularidad: **la regla previó la matriz vacía y la prohibió, y no previó la matriz imposible**.

Los cuatro incidentes son del mismo tipo y por eso van juntos. **Ninguno es un error de un agente**: en los cuatro, el agente cumplió la regla que tenía, o la única forma de cumplirla al pie de la letra habría producido un documento peor.

## 3. Lo que la normativa dice, con precisión

El orden de fases está declarado sin ambigüedad en `Master-Prompt.md` §6:

| Fase | Categoría | Qué emite |
|---|---|---|
| A | `00-Contexto` | `Acuerdo-Equipo.md`, cuya §5 debe referenciar la 08 |
| D | `06-Backlog-Tecnico` | `Definition-Of-Ready.md`, que debe referenciar la condición de terminado de la 08 |
| D | `07-Plan-Sprint` | `Plan-Iteracion-Sprint-XX.md`, que debe referenciar la condición de terminado canónica de la 08 y exigir la revisión de los documentos de la 11 |
| E | `08-Calidad-Y-Pruebas` | `Definition-Of-Done.md`, «la DoD canónica del proyecto de código» |
| E | `08-Calidad-Y-Pruebas` | `Matriz-Sensado-Deriva.md`, que la regla exige poblada con sondas que sólo la 10 produce |
| G | `10-Examples` | Los contratos de verificación de los que salen esas sondas |
| H | `11-Documentacion` | El índice del cuerpo documental, con estado `Planificado` |

**Lo que el framework sí resuelve bien y no hay que reescribir.** El orden de fases en sí es correcto: la condición de terminado necesita la estrategia de verificación, la matriz de cobertura y los quality gates, y ninguna de esas cosas existe antes de la Fase E. Adelantar la 08 para resolver este reporte sería cambiar un problema por otro peor. Y la intención de fuente única es la correcta; lo que hay que corregir no es la idea sino dónde está escrita.

**Lo que el framework no dice en ninguna parte.** Qué hace un artefacto que tiene que referenciar algo que todavía no existe. No hay un estado `Pendiente de emisión` para una referencia, no hay una instrucción de reapertura que obligue a volver sobre el artefacto cuando el referenciado aparezca, y no hay un criterio de audit que detecte una referencia que quedó colgada.

## 4. Por qué las dos salidas disponibles son malas

**Copiar el contenido.** Es lo que ocurrió en la Fase A. Produce un artefacto útil y crea una segunda fuente. El costo no se paga al escribirla sino cuando la 08 emita la suya y las dos diverjan: a partir de ahí hay dos listas de criterios de cierre, las dos vigentes, y ninguna regla normativa que declare cuál gana —la que lo diría vive en un prompt-snippet—. Es exactamente el defecto que el reporte `00` documenta para la transcripción y el `03` para los conjuntos compartidos entre categorías.

**Dejar la referencia colgada.** Cumple la letra —hay una referencia— y no cumple nada más. Un lector de la condición de listo no puede verificar que no se solapa con la condición de terminado, porque no la tiene. Y un audit que compruebe «existe referencia explícita a la DoD canónica» la da por satisfecha, con lo cual el hueco queda sellado por el propio mecanismo que debería detectarlo.

La tercera salida —declarar el apartamiento— es la que este destino usó, y funciona sólo porque alguien la inventó: no está en el framework, no hay quien la exija y no hay quien vuelva a mirar el documento cuando el referenciado aparezca.

## 5. La causa raíz

El framework tiene un grafo de dependencias **de contenido** y lo ordena bien. Lo que no tiene es la constatación de que ese grafo no es el único: hay un segundo grafo, de **obligaciones normativas entre artefactos**, que las reglas de categoría van tejiendo una por una, y que nadie comprueba contra el orden de fases.

Cada una de las cuatro reglas involucradas es razonable leída sola. `Rules-Contexto` tiene razón en que un acuerdo de equipo declare su condición de terminado. `Rules-Plan-Sprint` tiene razón en anclar la actualización documental al cierre de iteración, y su §4.5 explica muy bien por qué. `Rules-Calidad-Y-Pruebas` tiene razón en querer una fuente única. **El conflicto no está adentro de ninguna de las cuatro: está entre las cuatro y el plan de generación**, que es un quinto documento que ninguna de ellas mira.

Es la misma causa raíz del reporte `03` vista desde otro ángulo. Allí dos categorías afirmaban cosas incompatibles sobre el mismo conjunto; acá una categoría le exige algo a otra que todavía no corrió. En los dos casos el framework tiene trazabilidad entre categorías y no tiene **arbitraje** entre ellas.

## 6. El patrón, enunciado

> Una regla de categoría puede crear una obligación sobre un artefacto que otra categoría emite en una fase posterior. El framework ordena las fases por dependencia de contenido y no comprueba ese orden contra las obligaciones que sus propias reglas declaran, de modo que la contradicción no la descubre el método sino el agente que llega a la instrucción y no la puede cumplir. Como el framework tampoco declara qué hacer mientras tanto, cada agente resuelve por su cuenta, y las dos resoluciones disponibles —copiar el contenido o dejar la referencia colgada— rompen otra regla del propio framework.

El enunciado es deliberadamente general: los cuatro incidentes de este reporte son los que aparecieron hasta la Fase E de una corrida, y no hay ninguna razón para suponer que son todos. Cualquier regla que diga «referencia a la categoría N» sin comprobar en qué fase corre N es un caso potencial.

## 7. Qué hizo el destino

Las cuatro resoluciones quedaron declaradas en el propio árbol generado, no en un registro aparte:

1. **La condición de terminado.** Mientras la 08 no existió, la condición de listo de los cinco proyectos de código y los diez planes de iteración referenciaron la del acuerdo de equipo como vigente, declararon que la canónica todavía no existía y declararon qué iba a pasar cuando apareciera. **No se copió el contenido en ningún momento.**
2. **La categoría 11.** Cada plan de iteración declara que la 11 no tiene ningún documento emitido, que por lo tanto ningún ítem la alcanza, y que **la condición se cumple por vacío y vuelve a ser exigible en cuanto la 11 emita su índice**. La frase «no es una dispensa, es el estado del árbol en la fecha de este plan» está escrita a propósito, para que nadie la lea después como una excepción concedida.
3. **Al emitirse la categoría 08, la referencia se cerró.** Es lo que la propuesta 3 de §8 pide y esta corrida lo ejecutó: los quince documentos de la Fase D que la referenciaban —las cinco condiciones de listo y los diez planes de iteración— pasaron a apuntar a `08-Calidad-Y-Pruebas/Definition-Of-Done.md`, el acuerdo de equipo quedó declarado como **origen** de la mayor parte de los criterios de su capa de historia y no como fuente, y cada documento registró el cierre en su control de cambios. **El cierre lo disparó una persona que se acordó, no el método**: nada en el framework lo habría exigido, y ésa sigue siendo la parte del hallazgo que no se resuelve declarando bien una referencia.
4. **La matriz de sensado se cerró recién en la Fase G, y no del todo como el reporte anticipaba.** El cuarto incidente decía que la 08 exige una matriz poblada con sondas que sólo produce la 10, y que cuatro proyectos de código habían cerrado la Fase E sin matriz. Al ejecutarse la Fase G ocurrió lo que el reporte esperaba, con una diferencia que conviene registrar: **las cuatro matrices faltantes no las emitió la categoría 08 al reabrirse, las emitió el generador de la categoría 10**, porque es el único que tiene los contratos. El resultado es correcto —los cinco proyectos de código tienen matriz, y las catorce sondas `VER-XX` están—, pero un artefacto de la categoría 08 quedó emitido por la herramienta de la 10. La propuesta 3 de §8, la reapertura obligatoria, no alcanza para este caso: reabrir la 08 sin darle acceso a los contratos de la 10 la deja igual de imposibilitada. Lo que este caso agrega es que **la reapertura tiene que traer consigo el insumo que faltaba**, y no sólo el turno.
5. **La quinta matriz, la de `VideoControl-Web`, sí se reabrió bien**, porque ya existía: su §2 declaraba literalmente «No hay sondas `VER-XX` … esa categoría todavía no se generó», y al generarse pasó a declarar cuántas hay. Esa frase es exactamente la forma de referencia pendiente que propone §8.2, escrita antes de que el reporte la propusiera, y funcionó: el documento avisó de su propia carencia y la carencia se cerró.
6. **La 00 ya había copiado.** El `Acuerdo-Equipo.md` de la Fase A enumera sus ocho criterios y no se modificó: corregirlo ahora sería reescribir un artefacto ya auditado por un defecto que no es suyo.

## 8. Propuestas de intervención

Ninguna está decidida; son punto de partida.

1. **Comprobar el grafo de obligaciones contra el orden de fases.** Recorrer las reglas de categoría buscando toda frase de la forma «referencia a la categoría N» o «según lo que declare N», y contrastar la fase de la categoría que la contiene con la fase de N. Es una comprobación mecánica y de una sola pasada, y produce la lista completa de casos en lugar de los cuatro que una corrida encontró.
2. **Declarar un estado de referencia pendiente.** Un artefacto puede referenciar algo que todavía no existe si declara que no existe, cuál es su origen provisorio y cuándo se resuelve. Convertir en norma lo que este destino improvisó, con la ventaja de que entonces sí hay algo que el audit pueda verificar.
3. **Reapertura obligatoria.** Cuando la categoría N se emite, las categorías que la referenciaban en estado pendiente vuelven a la cola para cerrar la referencia. Sin esto, el estado pendiente se vuelve permanente y el reporte se repite dentro de dos fases.
4. **Adelantar la condición de terminado a la Fase A, y sólo ella.** La condición de terminado es un acuerdo del equipo antes que un producto de la estrategia de calidad: sus criterios hablan de revisión, cobertura y documentación, no de la pirámide de testing. La 08 la refinaría en lugar de crearla. Es la intervención más profunda de las cinco y la que resolvería el incidente A de raíz, en vez de administrarlo.
5. **Revisar la condición de §4.5 sobre la categoría 11.** La regla es buena y su justificación es la mejor escrita de las cuatro. Lo que le falta es decir qué significa antes de la Fase H, que es durante toda la construcción del backlog y los primeros planes.

## 9. Cómo verificar que la corrección funcionó

- La comprobación mecánica de la propuesta 1 corre y devuelve cero obligaciones hacia una fase posterior, o devuelve la lista y cada una tiene su tratamiento declarado.
- Ningún artefacto contiene una copia de la condición de terminado. La búsqueda de sus criterios en las categorías 00, 06 y 07 devuelve referencias y ninguna enumeración.
- Un artefacto con una referencia pendiente lo declara con esa forma, y el audit de su fase falla si no lo hace.
- Al emitirse la categoría referenciada, las referencias pendientes que la apuntaban quedan cerradas, y hay una comprobación que falla si alguna sigue abierta al cierre del producto.
- Ningún artefacto de una categoría queda emitido por la herramienta de otra. Si la reapertura de la categoría N necesita un insumo de la categoría M, la reapertura lo trae; que el resultado sea correcto no vuelve correcta la vía por la que se produjo, porque la próxima vez que ese artefacto haya que regenerarlo no va a estar claro quién lo hace.

## 10. Anexo — evidencia reproducible

```bash
# 1. Las tres reglas que obligan hacia la categoría 08, y la fase de cada una.
cd IA/IA.SDD/SDD/Devs
grep -n "Definition of Done de 08\|DoD canónica\|referencia a 08" Rules/Rules-Backlog-Tecnico.md Rules/Rules-Plan-Sprint.md Rules/Rules-Contexto.md

# 2. El orden de fases que las contradice.
grep -n "^| [A-J][0-9]* |" Orchestrator/Master-Prompt.md | cut -c1-45
#   Devuelve las diecisiete filas del plan. El sufijo [0-9]* del patrón hace falta:
#   sin él la fila «| B2 |» de la validación visual de maqueta no aparece y la salida
#   son dieciséis.
#   Las cinco que importan acá, con su distancia hasta la categoría referenciada:
#   A -> 00-Contexto          (Acuerdo-Equipo.md, §5 referencia a 08)  ... 5 fases (con B2)
#   D -> 06-Backlog-Tecnico   (Definition-Of-Ready.md, referencia a 08) ... 1 fase
#   D -> 07-Plan-Sprint       (planes: referencia a 08, 1 fase; condición sobre la 11, 4 fases)
#   E -> 08-Calidad-Y-Pruebas (Definition-Of-Done.md)
#   H -> 11-Documentacion     (índice, estado Planificado)

# 3. La regla de fuente única que la salida «copiar» rompe.
grep -n "única fuente" Rules/Rules-Calidad-Y-Pruebas.md
#   Devuelve una sola línea, la 453, dentro de «## 8. Prompt-snippet sugerido» (que empieza en la 429):
#   no es una sección normativa ni un criterio de aceptación de §6.

# 4. El incidente D: la matriz que la 08 exige y que sólo la 10 puede poblar.
grep -n "VER-XX\|matriz vacía" Rules/Rules-Calidad-Y-Pruebas.md | cut -c1-120
grep -n "^| [EG] |" Orchestrator/Master-Prompt.md | cut -c1-40
#   E -> 08-Calidad-Y-Pruebas, que exige la matriz
#   G -> 10-Examples, que produce las sondas con que se puebla

# 5. La copia efectivamente producida en la Fase A del destino.
grep -n -A 12 "^## 5. Definition of Done" \
  ../../../../Repos-RPIs/RPI.VidelControl/SDD/Docs/00-Contexto/Acuerdo-Equipo.md
```

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-11 | Reporte inicial, emitido al cerrar la Fase D. Documenta tres incidentes del mismo tipo —dos sobre la condición de terminado de la categoría 08 y uno sobre la categoría 11— en los que una regla de categoría obliga a referenciar o a satisfacer una condición sobre un artefacto que se emite en una fase posterior. Enuncia el patrón, muestra por qué las dos salidas disponibles rompen otra regla del framework, y propone cinco intervenciones, incluida la comprobación mecánica que produce la lista completa de casos. |
| 1.1 | 2026-08-11 | Corrección de dos afirmaciones que el audit independiente de la Fase D encontró mal medidas. **El título y §1 decían «dos fases más tarde» y ninguno de los tres incidentes lo es**: de la Fase D a la E hay una fase, de la A a la E hay cuatro y de la D a la H hay cuatro. Se declara la distancia caso por caso y se corrige el comentario del anexo, que presentaba cinco filas como si fueran la salida completa del comando. Y se precisa dónde vive la declaración de fuente única de la condición de terminado: en el prompt-snippet §8 y no en un criterio de aceptación, lo que **agrava** el hallazgo en vez de atenuarlo, porque ningún audit la puede exigir. |
| 1.2 | 2026-08-11 | Correcciones de la segunda ronda del audit. La corrección anterior sobrepasó el blanco: al dejar de sobrevaluar una cita del prompt-snippet pasó a **negar un criterio de aceptación de §6 que sí existe** —«la DoD no se redefine en sprint plans»—. Se reescribe §2 con la distinción que el hallazgo necesitaba: ese criterio cubre la copia hecha en un plan de sprint y **no alcanza a la del acuerdo de equipo**, que es el incidente A y cae en el hueco entre dos reglas. Se corrige además el patrón del comando 2 del anexo, que omitía la fila `B2` y devolvía dieciséis filas en vez de diecisiete, y el alcance del comando 4, que mostraba siete de los ocho criterios. |
| 1.3 | 2026-08-11 | Se incorpora un cuarto incidente, encontrado al ejecutar la Fase E: la categoría 08 exige una matriz de sensado que sólo puede poblar la categoría 10, dos fases más adelante, y la regla previó la matriz vacía y la prohibió sin prever la matriz imposible. Y se registra en §7 lo que ocurrió cuando el documento referenciado apareció: la referencia del incidente B se cerró en los 75 documentos de la Fase D, que es la intervención 3 ejecutada, con la observación de que la disparó una persona y no el método. |
| 1.4 | 2026-08-11 | Correcciones del audit de la Fase E. Seis recuentos habían quedado con el número anterior al agregar el cuarto incidente —incluido el título—, que es el defecto que el reporte `04` documenta ocurriendo dentro de un reporte por segunda vez. Se corrige además la distancia de la Fase A a la E, que estaba medida **sin contar la B2** pese a que el propio anexo argumenta que hay que contarla; la atribución de nueve criterios al acuerdo de equipo, que tiene ocho; los metadatos de origen y de secciones evaluadas; el tiempo verbal de §7.1, que describía en presente un estado ya superado; y §3, que no tenía fila para las fases E y G. Se agrega al anexo el comando que evidencia el cuarto incidente. |
| 1.5 | 2026-08-11 | Correcciones de la ronda 2 del audit. §7 afirmaba que **los 75 documentos** de la Fase D pasaron a apuntar a la condición de terminado, y son **quince**: los otros sesenta nunca la referenciaron, y el árbol se había corregido retirándoles una fila que decía lo contrario. Y quedaban cinco recuentos más con el número anterior al cuarto incidente, incluido uno que la fila 1.4 declaraba haber corregido. Se retira además el recuento de criterios del acuerdo, que no es el mismo en los cinco proyectos de código. |
| 1.6 | 2026-08-12 | Se registra el desenlace del cuarto incidente, ocurrido al ejecutar la Fase G. Las cuatro matrices de sensado faltantes se emitieron, pero las emitió el generador de la categoría 10 y no la 08 reabierta, porque los contratos que las pueblan viven en la 10: la propuesta 3 de §8 pide devolver el turno y este caso muestra que hay que devolver también el insumo. Se registra además que la quinta matriz, la única que ya existía, había declarado su propia carencia con la forma que propone §8.2 y se cerró sin fricción, lo que es la primera evidencia a favor de esa propuesta. §7 pasa de cuatro resoluciones a seis y §9 suma un criterio de verificación. |
| 1.7 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** Las obligaciones que una fase declara hacia otra que todavía no corrió.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `Root-Rules.md` §12 | La **referencia pendiente**: se declara, no se rellena ni se inventa |
| `Master-Prompt.md` §6 | Reapertura con insumo |
| `Rules-Calidad-Y-Pruebas.md` §6 y `Rules-Contexto.md` §4 | Fuente única exigible |

**Después de la 7.0.** **La comprobación que este reporte proponía se corrió, no sólo se incorporó.** Sobre las doce reglas de categoría encontró **tres obligaciones que ninguna corrida había detectado**, lo cual probó que la lista no estaba cerrada, tal como el reporte advertía. Y la **8.0** mejoró la solución: con el nivel por artefacto disponible, la condición de terminado se administra **en dos capas** en lugar de con una referencia pendiente.

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
