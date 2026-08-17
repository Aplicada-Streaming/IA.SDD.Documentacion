# Reporte 04 — Los recuentos declarados en prosa no se verifican contra lo que cuentan

| Campo | Valor |
|---|---|
| Reporte | 04 |
| Fecha | 2026-08-10 |
| Origen | Corrida real del orquestador sobre el destino `Repos-RPIs/RPI.VidelControl`: Fase A, Fase B y Fase B2, entre el 2026-08-09 y el 2026-08-10 |
| Versión del framework evaluada | SDD 6.0 (`Master-Prompt` 5.2, `Root-Rules` 3.1, reglas de categoría 4.0, invariantes D1 a D9) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Root-Rules.md`, las reglas de categoría, los criterios de audit del `Master-Prompt` §10 |
| Naturaleza | Una clase de dato que el framework produce en todos sus artefactos y no verifica en ninguno |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |
| Reportes relacionados | `00-Regla-Transcripción.md`, cuyo incidente original es un caso particular de este mismo patrón |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. Los cinco incidentes](#2-los-cinco-incidentes)
- [3. Qué tienen en común](#3-qué-tienen-en-común)
- [4. Lo que la normativa dice y lo que no dice](#4-lo-que-la-normativa-dice-y-lo-que-no-dice)
- [5. La causa raíz](#5-la-causa-raíz)
- [6. El patrón, enunciado](#6-el-patrón-enunciado)
- [7. Por qué no alcanza con pedir cuidado](#7-por-qué-no-alcanza-con-pedir-cuidado)
- [8. Propuestas de intervención](#8-propuestas-de-intervención)
- [9. Cómo verificar que la corrección funcionó](#9-cómo-verificar-que-la-corrección-funcionó)
- [10. Anexo — evidencia reproducible](#10-anexo--evidencia-reproducible)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

Los artefactos que este framework produce están llenos de números en prosa: «veintiún artefactos», «los cuatro proyectos de código», «nueve ventanas», «treinta términos referenciados», «catorce estados». Cada uno de esos números es **un dato derivado de una tabla o de una lista que está en el mismo documento o en uno vecino**.

El framework no declara en ninguna parte que esos números tengan que coincidir con lo que cuentan, y ningún audit los verifica. El resultado es una clase de defecto que aparece en cada corrida, sobrevive a todas las revisiones y se detecta por casualidad: alguien suma.

En esta corrida se encontraron **cinco**, en tres fases distintas y en documentos de tres niveles distintos. Ninguno lo detectó un control del método.

## 2. Los cinco incidentes

| # | Documento | Qué decía la prosa | Qué había realmente | Quién lo detectó |
|---|---|---|---|---|
| I-1 | `PRODUCT-INTAKE` §20.E-6 | «once ventanas» en el anexo y «nueve ventanas» en su propio bloque de qué ejercita | Nueve filas en la tabla fuente, que enumeran **diez** ventanas, con un encendido redundante que el anexo contaba como ventana aparte | El audit de la Fase A, y sólo porque el documento se contradecía **consigo mismo** (H-18). Si los dos números hubieran coincidido entre sí y los dos hubieran estado mal, habría pasado |
| I-2 | `VideoControl-Web/03-UX-UI-DX/README.md` | «Veintiún artefactos: el marco, el glosario, trece wireframes y cinco representaciones» | La tabla listaba **veinte**, y la propia enumeración del texto suma veinte: 1 + 1 + 13 + 5 | Nadie, hasta que un agente actualizó la tabla dos meses después y sumó |
| I-3 | `Wireframes-Acceso.md` §5 | Un estado «acuse entrante» | La maqueta demostraba **dos**, con texto y código de resultado distintos | Nadie. Salió de comparar recuentos maqueta contra wireframe, no de leer el documento |
| I-4 | Trece wireframes de `VideoControl-Web` §5 | 177 estados sumados | La maqueta aprobada demostraba **191** | Una pregunta del humano, dos días después. Ver reporte `02` |
| I-5 | `Glosario-UX.md` | «Veintiún términos» en el control de cambios de la versión 1.0 | Correcto en su versión, pero el índice de la categoría siguió citando ese número tres versiones después, cuando ya eran treinta | Nadie, hasta la misma pasada de I-2 |

## 3. Qué tienen en común

Los cinco son **el mismo defecto con distinta piel**:

1. Hay una colección: filas de una tabla, artefactos de una carpeta, entradas de un glosario, estados de una superficie.
2. Alguien escribe en prosa cuántos hay, porque leerlo ayuda.
3. La colección cambia.
4. El número no.

Y los cinco comparten una propiedad que los hace especialmente resistentes: **el número en prosa es plausible**. Nadie lee «veintiún artefactos» y sospecha. Un enlace roto se ve, una sección faltante se ve, un identificador colisionado se ve al cruzar. Un número mal contado se lee sin fricción, y por eso sobrevive a lectores humanos y a agentes por igual.

I-2 lo muestra bien: el texto decía «veintiún artefactos: el marco, el glosario, trece wireframes y cinco representaciones». La propia frase trae los sumandos. Sumaban veinte. Estuvo así desde que se escribió.

## 4. Lo que la normativa dice y lo que no dice

**Dice** que toda afirmación tiene que estar respaldada por evidencia verificable — invariante D9.

**Dice** que cada documento declara su trazabilidad upstream y downstream — invariante D6.

**No dice** que un número enunciado en prosa sea una afirmación sujeta a D9. En la práctica se lo trata como redacción, no como dato.

**No dice** que un recuento tenga que declarar sobre qué colección se calcula. «Veintiún artefactos» no dice de qué tabla sale, y por lo tanto no hay forma mecánica de recalcularlo.

**No tiene ningún criterio de audit** que verifique recuentos. Los audits verifican estructura, trazabilidad, vocabulario, cobertura y anclas. Ninguno suma.

## 5. La causa raíz

La causa raíz no es la falta de cuidado. Es que **el framework no distingue el dato derivado del dato declarado**.

Un dato declarado —el nombre de un caso de uso, el motivo de una regla— es autoridad: está bien porque alguien lo decidió. Un dato derivado —cuántos hay, cuál es el total, cuántos de cada tipo— no es autoridad: es el resultado de una operación sobre otra cosa, y su corrección depende de que esa otra cosa no haya cambiado.

El framework escribe los dos con la misma tinta, en el mismo párrafo, sin marca que los distinga. Y al no distinguirlos, no puede pedir que los segundos se recalculen cuando cambia su fuente, porque no sabe cuáles son.

Nótese que este es exactamente el mismo defecto que el reporte `00` describe para otra clase de dato: allá, una cifra transcripta de una fuente que había sido corregida; acá, una cifra derivada de una colección que había crecido. **La transcripción y la derivación son dos formas de tener el mismo dato en dos lugares**, y el framework no tiene regla para ninguna de las dos.

## 6. El patrón, enunciado

> **Todo número que un documento enuncia en prosa es un dato derivado de una colección, y el framework lo escribe como si fuera un dato declarado. Al no marcarlo como derivado, no puede recalcularlo cuando su fuente cambia, y al ser plausible por naturaleza, ningún lector lo cuestiona. El defecto es silencioso, recurrente y se detecta por casualidad.**

Corolario que conviene escribir aparte, porque es lo que hace al patrón caro:

> Un recuento incorrecto **no rompe nada**, y por eso convive con la documentación durante toda su vida. Su costo aparece más tarde y en otro lado: alguien planifica contra un número, un audit reporta una diferencia que en realidad es la suma equivocada, o un agente aguas abajo genera artefactos para veintiún elementos que son veinte.

## 7. Por qué no alcanza con pedir cuidado

Porque el defecto no se comete por descuido en la escritura: se comete **en la modificación**. Los cinco incidentes tienen la misma forma: el número estaba bien cuando se escribió, y dejó de estarlo cuando la colección cambió, en otra sesión, por otra mano, con otro objetivo.

Pedirle a un agente que verifique los recuentos que escribe no atrapa nada, porque el agente que escribe el número lo escribe bien. El que lo rompe es el que agrega una fila tres versiones después, y ese ni siquiera sabe que el número existe.

Por eso la intervención tiene que ser estructural y no de instrucción.

## 8. Propuestas de intervención

No se aplican acá. Se enumeran para el prompt que intervenga el framework.

| # | Intervención | Artefacto | Efecto esperado |
|---|---|---|---|
| P-1 | Declarar que **un recuento en prosa es una afirmación bajo D9** y por lo tanto tiene que ser verificable contra la colección que cuenta | `README.md` del framework, invariante D9 | El número deja de ser redacción y pasa a ser dato |
| P-2 | Exigir que todo recuento **nombre su fuente**: «los veinte artefactos de la tabla de arriba», «los trece wireframes de §2» | `Root-Rules.md` | Un recuento sin fuente declarada es irrecalculable; con fuente, es verificable por cualquiera |
| P-3 | Sumar a los audits un **control de recuentos**: por cada número en prosa con fuente declarada, recalcular y comparar. Hallazgo P2, o P1 si el número está en un índice o en un manifiesto | criterios de audit del `Master-Prompt` §10 | El defecto deja de depender de que alguien sume por casualidad |
| P-4 | Preferir la **forma que no cuenta**: «los artefactos de la tabla» en lugar de «los veintiún artefactos», salvo que el número aporte algo que la tabla no da | `Root-Rules.md`, guía de redacción | Menos números derivados es menos superficie de falla. Es la intervención más barata y la que más lejos llega |
| P-5 | Que el control de cambios de un documento **registre el recuento cuando cambia**, del mismo modo que registra el cambio que lo produjo | reglas de categoría | Un recuento que cambió sin entrada en el control de cambios es señal de que alguien tocó la colección y no el número |

La P-4 merece énfasis: las otras cuatro agregan verificación, y ésta elimina el dato. Un documento que dice «los wireframes de la tabla de cobertura» no puede desincronizarse nunca.

## 9. Cómo verificar que la corrección funcionó

1. Extraer de un árbol generado todos los números en prosa y comprobar que cada uno declara su fuente.
2. Recalcular cada uno contra su fuente y comprobar que coinciden.
3. Agregar deliberadamente una fila a una tabla contada y comprobar que el audit reporta el recuento desactualizado.
4. Medir cuántos recuentos derivados quedan después de aplicar P-4: la métrica de éxito no es cuántos se verifican, es cuántos dejaron de existir.

## 10. Anexo — evidencia reproducible

```bash
# I-2, reproducible sobre el destino antes de la corrección:
#   el texto decía «Veintiún artefactos», la tabla tenía veinte filas
#   y la propia enumeración del texto suma 1 + 1 + 13 + 5 = 20

# I-4, medido sobre el destino:
#   suma de estados declarados en la sección 5 de los trece wireframes → 177
#   estados demostrados por la maqueta aprobada                        → 191

# Comprobación genérica de recuentos de una tabla de índice:
#   contar filas de la tabla de artefactos vigentes y compararla con la
#   cifra que el párrafo siguiente enuncia
```

Los cinco incidentes se corrigieron sobre el destino. Ninguna corrección se aplicó al framework: este reporte existe para que esa corrección se decida con evidencia.

## Evidencia posterior: el patrón sobrevive al audit

Este reporte se emitió con la evidencia de las fases A, B y B2. Las fases D y E agregaron una medición que conviene incorporar, porque cambia qué hay que hacer con el patrón y no sólo cuánto pesa.

**Tres rondas de audit independiente sobre la misma fase no lo agotaron.** Sobre la categoría 08 de la Fase E, las tres rondas produjeron 33 hallazgos, de los cuales **22 son recuentos derivados escritos a mano o prosa de plantilla aplicada donde no era cierta**. El rendimiento bajó ronda a ronda —15, 9, 9— y no llegó a cero. En las tres hubo hallazgos que eran la corrección de la ronda anterior aplicada a media oración: un número subido en una tabla y la frase de al lado con el valor viejo.

**Y el patrón ocurre dentro de los propios reportes de este directorio.** El `07` corrigió sus recuentos en dos rondas seguidas y en las dos quedó alguno; el `08` corrigió «cuatro» a «seis» en la primera mitad de una oración y lo dejó en «esos cuatro» doce palabras después. Un reporte que documenta un patrón y lo comete es la mejor prueba de que la propuesta de §8 —eliminar el dato derivado en vez de verificarlo— es la correcta.

**Lo que el destino hizo con esto** está en `SDD/Herramientas/Verificacion/`: un verificador que contrasta contra la fuente las afirmaciones ancladas, corriendo como compuerta antes de despachar un audit. Su primera versión comprobaba todo par «número + sustantivo» y produjo 94 avisos casi todos falsos, lo que agrega una precisión a la propuesta de este reporte: **el dato derivado se elimina o se ancla, y anclarlo exige que su frase no admita otro referente**. La consecuencia de no acotarlo es ruido, y el ruido en un instrumento de verificación es peor que su ausencia.

El reporte [09](09-El-Audit-Como-Unica-Compuerta.md) desarrolla la parte que excede a este patrón: que el audit sea el único instrumento de la compuerta de fase.

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-10 | Reporte inicial: el framework escribe datos derivados —los recuentos en prosa— con la misma forma que los datos declarados, no exige que nombren su fuente y no los verifica en ningún audit. Cinco incidentes de una misma corrida, en tres fases y tres niveles de documento, con cinco propuestas de intervención, de las cuales la más barata es eliminar el dato en lugar de verificarlo. |
| 1.1 | 2026-08-12 | Se incorpora la evidencia de las fases D y E: tres rondas de audit independiente sobre la misma fase no agotaron el patrón, y 22 de sus 33 hallazgos son de esta clase. Se agrega además que el patrón ocurre dentro de los propios reportes de este directorio, y una precisión sobre la propuesta de §8: anclar un dato derivado exige que su frase no admita otro referente, porque un verificador sin esa restricción produce ruido. |
| 1.2 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** Los recuentos escritos en la prosa, que envejecen sin avisar.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `Root-Rules.md` §10 | La regla de **datos derivados en la prosa**: todo recuento declarado se ancla a su fuente, y quien la cambia sabe qué prosa revisar |
| `Master-Prompt.md` §10 y §10.0 | La compuerta mecánica verifica los recuentos anclados **antes** del audit |

**Después de la 7.0.** Su patrón siguió apareciendo, y quedó registrado: la **8.6** documenta una corrección aplicada sin revisar la prosa que la citaba, que es exactamente el defecto que este reporte describe.

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
