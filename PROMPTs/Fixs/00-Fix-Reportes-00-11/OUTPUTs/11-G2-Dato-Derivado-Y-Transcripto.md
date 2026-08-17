# G2 — El dato que se copia o se deriva no tiene regla

**Documento:** 11-G2-Dato-Derivado-Y-Transcripto.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Reportes de la familia:** 00 (huecos A y B), 04
**Estado:** Vigente

---

## 1. Dónde se produce el fallo

Los dos reportes describen el mismo dato en dos lugares, por dos vías distintas. El reporte 04 §5 lo
enuncia, y es la frase que define la familia:

> La transcripción y la derivación son dos formas de tener el mismo dato en dos lugares, y el
> framework no tiene regla para ninguna de las dos.

| Vía | Qué pasa | Incidente que lo evidencia |
|---|---|---|
| **Transcripción** | Una fuente enuncia un conteo y la transcripción arroja otro. Quien transcribe elige uno en silencio, o inventa un tercero | El escenario `§20.E-6` del intake del destino: la prosa decía «nueve», el payload enumeraba once, y el número correcto era diez. Los tres convivían en un mismo escenario, separados por veinte líneas (reporte 00 §2.2) |
| **Derivación** | Un número en prosa cuenta una colección del mismo documento o de uno vecino. La colección cambia y el número no | «Veintiún artefactos: el marco, el glosario, trece wireframes y cinco representaciones» sobre una tabla de veinte filas, cuya propia enumeración suma 1 + 1 + 13 + 5 = 20 (reporte 04, I-2) |

El defecto no se comete al escribir: se comete al modificar. El reporte 04 §7 lo dice con precisión y
es lo que decide la forma de la corrección:

> Pedirle a un agente que verifique los recuentos que escribe no atrapa nada, porque el agente que
> escribe el número lo escribe bien. El que lo rompe es el que agrega una fila tres versiones
> después, y ese ni siquiera sabe que el número existe.

## 2. Por qué la normativa vigente no lo atrapa

**Lo que sí valida el intake.** `Intake-Rules.md` §5, viñeta de la Parte D, exige que cada escenario
declare procedencia, un `Estado` del enum cerrado —`medido`, `declarado`, `derivado`,
`reconstruido`— y sus cuatro bloques: contexto, qué ejercita, JSON completo y qué verificar. Suma la
regla de resolución de identificadores en las dos direcciones y la regla de autocontención.

**Lo que no valida.** Nada compara los cuatro bloques entre sí. La validación verifica que los cuatro
**existan**; no que **digan lo mismo**. Un escenario con «nueve» en un bloque y once entradas en el
siguiente cumple los cuatro requisitos, y así pasó.

**Por qué la regla de no invención no lo cubre.** Porque nada se inventó: la tabla se copió
fielmente. Lo que se derivó mal fue el **conteo sobre la tabla**, que es una operación intermedia
que la normativa no nombra. El enum de `Estado` tampoco lo cubre, y el motivo es estructural: el
escenario era legítimamente `medido` en cuanto a sus filas y su conteo era `derivado`, y hoy un
escenario declara **un solo `Estado` para todo su contenido**.

**Lo que dice D9 y lo que no alcanza.** `README.md` D9 exige que toda afirmación sobre el estado del
sistema cite evidencia. Un número en prosa no se trata como afirmación sujeta a D9: se trata como
redacción. Y `Master-Prompt.md` §10 acota D9 explícitamente a «afirmaciones sobre el estado del
sistema», con la nota de `Deriva-Rules.md` §1 de que no aplica a afirmaciones de diseño, de
especificación ni de contexto. Un recuento sobre una tabla del propio documento no es ninguna de esas
cosas: cae en el hueco.

**Ningún criterio de audit suma.** Los criterios de `Master-Prompt.md` §10 verifican conformidad D1 a
D9, cumplimiento de §6 de la regla, coherencia cross-doc, gobierno del glosario, trazabilidad y
estructura de carpetas. Ninguno recalcula un número.

## 3. La evidencia que decide la forma de la corrección

Es la que el reporte 04 incorporó en su versión 1.1 y la que hay que respetar, porque acota la
corrección obvia:

1. **Tres rondas de audit independiente no agotan el patrón.** Sobre la categoría 08 de la Fase E,
   las tres rondas produjeron 33 hallazgos, de los cuales **22 son recuentos derivados o prosa de
   plantilla aplicada donde no era cierta**. El rendimiento bajó 15, 9, 9 y no llegó a cero.
2. **El patrón ocurre dentro de los propios reportes.** El reporte 08 corrigió «cuatro» a «seis» en
   la primera mitad de una oración y lo dejó en «esos cuatro» doce palabras después. Un reporte que
   documenta un patrón y lo comete es la prueba de que la propuesta correcta es **eliminar el dato
   derivado, no verificarlo**.
3. **Verificar sin acotar produce ruido, y el ruido es peor que la ausencia.** La primera versión del
   verificador del destino comprobaba todo par «número + sustantivo» y produjo 94 avisos casi todos
   falsos. Se rehízo con **afirmaciones ancladas**: una frase concreta cuyo número no admite otro
   referente. Dos anclas se retiraron por no cumplir ese criterio.

Y el mismo límite aparece en el reporte 00, en su caso V4: una validación que compare todos los
números de la prosa contra todos los números del payload produce ruido y se desactiva sola. La
comparación tiene que acotarse a **conteos y enumeraciones del propio payload**.

## 4. Correcciones propuestas

### G2-A · Regla de transcripción fiel en la plantilla de intake

**Archivo:** `PRODUCT-INTAKE-template.md` §20, bloque de formato por escenario y bloque «Lo que NO
va en esta sección».
**Severidad:** minor.

Tres obligaciones, tomadas del reporte 00 §4:

1. Si la fuente enuncia un conteo, un total o una cardinalidad, el escenario lo reproduce **tal como
   la fuente lo enuncia**.
2. Si la transcripción arroja un valor distinto, el escenario declara **los dos valores y la razón de
   la diferencia**, en un bloque propio, en lugar de elegir uno.
3. Toda magnitud que el escenario **derive** de la fuente en lugar de copiarla se marca como
   derivada, con su regla de cálculo, aunque el escenario sea `medido` en su conjunto. Es la
   corrección del hueco estructural de §2: el `Estado` deja de ser necesariamente uniforme para todo
   el contenido del escenario.

Más el bloque de conteo como forma sugerida, que es la que la corrección real del caso terminó
adoptando, y un anti-patrón nuevo: «conteo derivado presentado como transcripto».

La regla se enuncia en una sola línea, que es lo que la hace barata de cumplir y de verificar:

> Si la fuente enuncia un número y tu transcripción arroja otro, declarás los dos y por qué difieren,
> en lugar de elegir uno.

**Por qué es normativa y no buena práctica.** Porque el bloque «qué verificar» del escenario es lo
que `08-Calidad-Y-Pruebas` convierte en criterio de aceptación y `10-Examples` en contrato de
verificación. El conteo de un escenario no es un dato ornamental: es un criterio de aceptación en
camino (reporte 00 §4.1).

### G2-B · Coherencia intra-escenario en la validación de intake

**Archivos:** `Intake-Rules.md` §5, viñeta de la Parte D, y §7.
**Severidad:** minor.

Toda magnitud, conteo o enumeración que la prosa de un escenario enuncie tiene que coincidir con lo
que su payload contiene, o el escenario tiene que declarar explícitamente por qué difieren. La
discrepancia **no declarada** es bloqueante en §7; la declarada es un dato más del escenario.

Con su acotación explícita, que es lo que evita que la validación se convierta en ruido y se
desactive sola: **la comparación alcanza a conteos y enumeraciones del propio payload, y no a
cualquier número que aparezca en el texto**. Una fecha, una versión o una medición mencionadas en la
prosa no disparan la validación.

**Por qué importa más de lo que parece.** Los cuatro bloques de un escenario tienen consumidores
declarados distintos —`02` toma el modelo, `08` toma «qué verificar» como criterio de aceptación,
`10` lo convierte en contrato—. Si los bloques se contradicen, cada consumidor aguas abajo cree una
cosa distinta y ninguno tiene forma de saber que el otro leyó otra.

### G2-C · El dato derivado, declarado como tal

**Archivo:** `Root-Rules.md`, sección transversal nueva **§11 Datos derivados en la prosa**.
**Severidad:** minor.

Cuatro reglas, ordenadas de la que más lejos llega a la que más verifica:

1. **Preferir la forma que no cuenta.** «Los artefactos de la tabla» en lugar de «los veintiún
   artefactos», salvo que el número aporte algo que la tabla no da. Es la más barata y la única que
   elimina el dato en vez de verificarlo: un documento que dice «los wireframes de la tabla de
   cobertura» no puede desincronizarse nunca.
2. **Todo recuento que se escriba nombra su fuente.** «Los veinte artefactos de la tabla de §2».
   Un recuento sin fuente declarada es irrecalculable; con fuente, lo verifica cualquiera.
3. **Anclaje que no admite otro referente.** Un recuento anclado se escribe de modo que su número no
   pueda leerse como referido a otra colección. «Cuatro capas» no es anclable si el documento habla
   de capas de la condición de terminado y de capas de cobertura; «las cuatro capas de la matriz de
   cobertura de §3» sí. Un recuento que no se puede anclar se reescribe con la regla 1, no se
   verifica.
4. **El control de cambios registra el recuento cuando cambia**, del mismo modo que registra el
   cambio que lo produjo. Un recuento que cambió sin entrada en el control de cambios es señal de que
   alguien tocó la colección y no el número.

**Métrica de éxito, declarada.** Tomada del reporte 04 §9.4 y no es la obvia: no es cuántos recuentos
se verifican, es **cuántos dejaron de existir**.

### G2-D · Control de recuentos anclados en el audit

**Archivo:** criterios de audit de `Master-Prompt.md` §10.
**Severidad:** minor.

Por cada recuento anclado —con fuente declarada, según G2-C regla 2— el audit recalcula y compara.
Nivel del hallazgo: **P2**, o **P1** si el número está en un índice, en un manifiesto o en un
criterio de aceptación, porque desde ahí se propaga.

Con dos restricciones que vienen de la evidencia y sin las cuales el control produce el ruido del
§3:

- El control alcanza **solo** a los recuentos anclados. Un número sin ancla no es hallazgo del audit:
  es hallazgo contra G2-C regla 1, y su corrección es reescribir la frase, no discutir el número.
- El control es de naturaleza **enumerable** y por lo tanto es candidato de la compuerta mecánica de
  la familia G4. Ése es su lugar natural; el criterio de audit es la red que queda mientras la
  compuerta no exista.

## 5. Cómo se verifica

| Caso | Entrada | Resultado esperado | Origen |
|---|---|---|---|
| V1 | Escenario cuya prosa dice «nueve» y cuyo payload enumera once, sin declaración | **Bloqueante**. La batería de `Intake-Rules.md` §6 reporta el escenario, los dos valores y los dos bloques donde aparecen | 00 V1 |
| V2 | Escenario cuya prosa dice «nueve», su payload enumera diez y declara por qué difieren | **Pasa**. La diferencia declarada es un dato del escenario | 00 V2 |
| V3 | Escenario cuya fuente enuncia un total que el escenario no reproduce | **Recomendado**, no bloqueante | 00 V3 |
| V4 | Escenario cuya prosa menciona un número que **no es un conteo del payload** —una fecha, una versión, una medición— | **Pasa sin observación**. Es el caso que decide si la corrección es aplicable | 00 V4 |
| V5 | Un árbol generado | Todo recuento en prosa declara su fuente, o no existe | 04.1 |
| V6 | Una fila agregada a una tabla contada | El audit reporta el recuento desactualizado | 04.3 |
| V7 | Dos versiones del mismo árbol | La cantidad de recuentos derivados baja | 04.4, que es la métrica de éxito real |
| V8 | Una frase con un número cuyo referente es ambiguo | No se ancla: se reescribe. El verificador no la evalúa | 04, evidencia posterior |

## 6. Qué queda fuera de esta corrección, y por qué

**Que un recuento en prosa sea una afirmación bajo D9.** Es la propuesta P-1 del reporte 04 y toca
una invariante, de modo que cae bajo la misma restricción que G1: `README.md` §Reglas de intervención
exige decisión explícita del responsable. Además es evitable: G2-C obtiene el mismo efecto —el número
deja de ser redacción y pasa a ser dato— declarándolo en una regla transversal, sin ampliar el
alcance de D9, que hoy está deliberadamente acotado a afirmaciones sobre el estado del sistema. Se
propone la vía barata y se deja registrada la profunda.

**Reprocesar los recuentos de la documentación ya emitida.** Fuera de alcance por la misma razón por
la que D9 no es retroactiva (`README.md`): reauditar documentación previa contra una regla nueva
produce un volumen de hallazgos que ahoga a los reales. La regla rige hacia adelante.

**La severidad de la plantilla de intake.** El reporte 00 §7 deja abierto si G2-A es minor o major,
según si un intake ya emitido con un conteo transcripto sin declarar deja de cumplir. Se resuelve
como **minor**, por coherencia con la severidad de G2-B: el reporte 00 §3 ya declara para el hueco A
que «un intake ya emitido no deja de cumplir por su forma, aunque pueda fallar la validación nueva,
que es exactamente el efecto buscado». La misma lógica aplica a la plantilla: lo que cambia es qué
valida el orquestador la próxima vez que corra, no la forma del artefacto ya emitido.
