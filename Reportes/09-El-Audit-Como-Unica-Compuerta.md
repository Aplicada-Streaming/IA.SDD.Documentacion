# Reporte 09 — El audit es la única compuerta de fase, y no tiene nada mecánico delante

| Campo | Valor |
|---|---|
| Reporte | 09 |
| Fecha | 2026-08-12 |
| Origen | Corrida real del orquestador sobre el destino `Repos-RPIs/RPI.VidelControl`: tres rondas de audit sobre la Fase E y dos sobre la Fase D, 2026-08-11 y 2026-08-12 |
| Versión del framework evaluada | SDD 6.0 (`Master-Prompt` §6 y §10; los criterios de aceptación §6 de las doce reglas de categoría) |
| Artefactos del framework alcanzados | `SDD/Devs/Orchestrator/Master-Prompt.md`; por extensión, la sección de criterios de aceptación de toda regla de categoría |
| Naturaleza | Una compuerta de fase que sólo admite un instrumento de muestreo, para verificar propiedades que en buena parte son enumerables |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |
| Reportes relacionados | `04-Recuentos-Declarados-En-Prosa.md`, que documenta la clase de defecto que este reporte muestra que el audit no agota |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. La evidencia: tres rondas que no convergen](#2-la-evidencia-tres-rondas-que-no-convergen)
- [3. Lo que la normativa dice, con precisión](#3-lo-que-la-normativa-dice-con-precisión)
- [4. Por qué más rondas no lo resuelven](#4-por-qué-más-rondas-no-lo-resuelven)
- [5. La causa raíz](#5-la-causa-raíz)
- [6. El patrón, enunciado](#6-el-patrón-enunciado)
- [7. Qué hizo el destino](#7-qué-hizo-el-destino)
- [8. Propuestas de intervención](#8-propuestas-de-intervención)
- [9. Cómo verificar que la corrección funcionó](#9-cómo-verificar-que-la-corrección-funcionó)
- [10. Anexo — evidencia reproducible](#10-anexo--evidencia-reproducible)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

El framework cierra cada fase con un audit y no exige nada antes. El audit es un lector: encuentra defectos leyendo, y leer un corpus de cuarenta documentos es un muestreo. Sirve —encontró todo lo importante que se encontró— pero **no agota**, y el framework no le pone delante ninguna comprobación que sí agote.

La consecuencia se midió: tres rondas sobre la misma fase, con auditores independientes y sin defectos inventados, produjeron **rendimiento decreciente y no nulo**, y en las tres el grueso de los hallazgos fue de una clase que un guion detecta en un segundo. Cada ronda gastó su atención en recuentos mal escritos y en accidentes de escapado, y le quedó menos para lo único que un guion no puede hacer: abrir el documento citado y ver si dice lo que el que lo cita afirma.

## 2. La evidencia: tres rondas que no convergen

Reparto de los hallazgos de las tres rondas de la Fase E, categoría 08, por si un guion podría haberlos encontrado:

| Ronda | Hallazgos | Detectables por guion | Sólo por lectura |
|---|---|---|---|
| 1 | 6 P0 + 9 P1 | 9 | 6 |
| 2 | 3 P0 + 6 P1 | 6 | 3 |
| 3 | 2 P0 + 7 P1 | 7 | 2 |
| **Total** | **33** | **22** | **11** |

«Detectables por guion» significa: recuentos que no coinciden con la colección que nombran, prosa de plantilla aplicada a un proyecto donde no es cierta, y accidentes de escapado. Ejemplos de los tres tipos, todos reales:

- **Recuento.** «Los nueve criterios del acuerdo de equipo» sobre un documento con ocho, replicado en ochenta lugares. «Los siete de la capa de iteración» sobre cuatro documentos que declaran seis en su propia sección.
- **Plantilla.** «Los cuatro umbrales son bloqueantes» en proyectos con uno, dos y tres capas. «Este proyecto no ejecutó la Fase B2 por no tener interfaz» sobre un componente de dibujo.
- **Escapado.** Un bloque de texto emitido con `\n` literales que no renderiza. Trescientas setenta y cuatro filas de una tabla con tubos sin escapar, que hacían perder las tres últimas columnas —incluida la de estado, que es la razón de existir del instrumento—.

Los once de la columna derecha son de otra naturaleza y son los que importan: afirmaciones que contradicen al documento que citan. «Al cambiar la contraseña las sesiones abiertas siguen siendo válidas», cuando el caso de uso dice que se cierran. «La precedencia evita que el planificador pise lo que la persona hizo», cuando la regla existe para que sí pise. Ninguna de esas la ve un guion.

**La observación decisiva la hizo el auditor de la ronda 2**, sin que se le pidiera:

> Mientras «siete», «cuatro», «los 75» sigan siendo texto libre en vez de salida de un comando citado, cada ronda va a encontrar los que la anterior no tocó.

## 3. Lo que la normativa dice, con precisión

`Master-Prompt.md` §6 cierra cada fila del plan de generación con una columna **`Audit post-fase`**, y §10 desarrolla la mecánica: el audit devuelve `APROBADO`, `APROBADO CON OBSERVACIONES` o `RECHAZADO`, y ninguna fase empieza sin que la anterior haya cerrado.

**Lo que el framework sí resuelve bien y no hay que reescribir.** El audit como compuerta es correcto y este destino lo comprueba: los once hallazgos de la columna derecha de §2 son defectos graves que ningún otro mecanismo del método habría encontrado, y dos de ellos habrían llegado a las pruebas de la categoría 08 como criterios que verifican lo contrario de lo que el diseño exige. La independencia del auditor también funciona: los tres despachos encontraron conjuntos distintos.

**Lo que el framework no dice en ninguna parte.** Que antes de despachar un audit haya que correr algo. No hay una compuerta mecánica, no hay una lista de propiedades enumerables, y los criterios de aceptación §6 de las doce reglas de categoría están redactados para que los evalúe un lector —«existe X con las cinco secciones», «cada TC referencia al menos un CU»— sin distinguir cuáles de ellos son decidibles por una máquina. Varios lo son.

## 4. Por qué más rondas no lo resuelven

Es la parte que conviene no discutir con intuiciones. Tres rondas dieron 15, 9 y 9 hallazgos. La cuarta habría dado algunos más: el rendimiento baja, no llega a cero, y **cada corrección introduce material nuevo que la ronda siguiente tiene que volver a muestrear**. En las tres rondas hubo hallazgos que eran la corrección anterior aplicada a media oración: se subió un número en una tabla y la frase de al lado quedó con el valor viejo.

Hay además un costo que no se ve en la cuenta de hallazgos: **un audit que gasta su atención en contar no la gasta en leer**. Los dos P0 de la ronda 3 fueron accidentes de escapado; el auditor que los encontró es el mismo que tenía que verificar 309 citas contra su fuente.

## 5. La causa raíz

El framework tiene **un solo instrumento de verificación** y lo aplica a propiedades de dos naturalezas distintas.

Las propiedades **enumerables** —un recuento coincide o no, un enlace resuelve o no, un documento tiene o no la sección que la regla exige, un generador es idempotente o no— se deciden contando. Un lector las decide peor que un guion: se cansa, muestrea, y su hallazgo es una observación en vez de un veredicto.

Las propiedades **interpretativas** —si una frase dice lo que su fuente dice, si un argumento se sostiene, si una decisión está bien tomada— sólo las decide alguien que lea las dos cosas. Ningún guion las alcanza.

El framework las trata igual, y el resultado es previsible: el instrumento caro se gasta en lo barato.

## 6. El patrón, enunciado

> El framework verifica cada fase con un único instrumento —un lector independiente— y lo aplica indistintamente a propiedades enumerables y a propiedades interpretativas. Como las primeras son muchas y baratas de producir, consumen la mayor parte de la atención del instrumento, que es escaso y es el único capaz de decidir las segundas. El efecto no es que las propiedades enumerables queden sin verificar: es que **las interpretativas quedan peor verificadas de lo que el método supone**, y que el número de rondas necesarias para cerrar una fase crece sin que el método declare un criterio de corte.

## 7. Qué hizo el destino

Se construyó una **compuerta mecánica** en `SDD/Herramientas/Verificacion/`, que corre antes de despachar un audit y devuelve código distinto de cero si algo falla. Tres comprobaciones, cada una salida de un defecto real:

1. **Enlaces y anclas** sobre todo el árbol.
2. **Recuentos anclados**: contrasta contra la fuente los números que la prosa declara sobre colecciones contables.
3. **Idempotencia de los generadores**: correr dos veces produce los mismos bytes.

Dos cosas de su construcción valen para la intervención, porque las dos fueron errores antes de ser criterios:

**La primera versión del verificador de recuentos comprobaba todo par «número + sustantivo» y produjo 94 avisos de los que casi todos eran ruido.** «Cuatro capas» puede ser las capas de la condición de terminado o las de cobertura. Se rehízo con **afirmaciones ancladas**: una frase concreta cuyo número no admite otro referente. El criterio para agregar un ancla es ése, y dos anclas se retiraron por no cumplirlo.

**La compuerta declara qué no mira.** Su salida en verde dice, literalmente: «lo mecánico está limpio; lo que afirma cada frase, no lo mira nadie todavía». Una compuerta que se lee como aprobación es peor que ninguna, porque el audit siguiente llega con la guardia baja.

Lo que la compuerta **no** resuelve, y este destino tampoco: el audit sigue sin criterio de corte declarado. La Fase E cerró con tres rondas por decisión del Product Owner, no por un criterio del método.

## 8. Propuestas de intervención

Ninguna está decidida; son punto de partida.

1. **Separar los criterios de aceptación por naturaleza.** En las doce reglas de categoría, marcar cada criterio de §6 como *enumerable* o *interpretativo*. Es una pasada de clasificación sobre texto que ya existe, y produce la lista de lo que una compuerta tendría que cubrir.
2. **Exigir una compuerta mecánica antes del audit**, con su resultado como insumo del despacho. El prompt del auditor recibiría «esto ya está verificado, no lo mires» y podría gastar su presupuesto en lo interpretativo.
3. **Declarar un criterio de corte para las rondas.** Hoy no existe: una fase cierra cuando el audit aprueba, y si nunca aprueba no hay regla. Un criterio posible: se cierra cuando dos rondas seguidas no encuentran ningún hallazgo de la clase interpretativa, con los enumerables en cero por la compuerta.
4. **Pedirle al audit que clasifique sus hallazgos** por si un guion podría haberlos encontrado. Es barato, y produce la métrica que este reporte tuvo que reconstruir a mano: si la proporción no baja ronda a ronda, la compuerta no está cubriendo lo que debería.
5. **Que la compuerta declare su alcance en su propia salida.** Es lo que este destino hizo y conviene volverlo norma: un instrumento que no dice qué no mira se lee como si mirara todo.

## 9. Cómo verificar que la corrección funcionó

- Los criterios de aceptación de las doce reglas están clasificados, y la suma de los enumerables coincide con lo que la compuerta comprueba.
- Un audit despachado recibe el resultado de la compuerta y su prompt excluye explícitamente lo ya verificado.
- La proporción de hallazgos detectables por guion, reportada por el propio audit, baja por debajo de un umbral declarado en la primera ronda de una fase.
- Existe un criterio de corte y una fase cerrada lo cita al cerrar, en vez de cerrar por decisión.

## 10. Anexo — evidencia reproducible

```bash
# 1. La compuerta del destino, con sus tres comprobaciones.
cd Repos-RPIs/RPI.VidelControl/SDD/Herramientas/Verificacion
python3 compuerta.py
#   1. Enlaces y anclas ......... sin roturas
#   2. Recuentos anclados ....... 0 afirmaciones ancladas incorrectas
#   3. Idempotencia ............. los generadores no cambian el árbol

# 2. El verificador de recuentos, solo, con su código de salida.
python3 recuentos.py; echo "código de salida: $?"

# 3. El framework no exige nada antes del audit: la columna existe y no tiene precondición.
cd ../../../../../IA/IA.SDD/SDD/Devs
grep -n "Audit post-fase" Orchestrator/Master-Prompt.md
grep -n "compuerta\|verificaci[óo]n mec[áa]nica\|antes de despachar" Orchestrator/Master-Prompt.md
#   Devuelve cuatro líneas —575, 665, 835 y 922— y **ninguna es una precondición del
#   audit**: hablan del archivado del estado previo, de la resolución de un destino
#   existente, de la precondición de la Fase I y del handoff a codificación.

# 4. Los criterios de aceptación de una regla, sin distinguir naturaleza.
sed -n '/^## 6. Criterios de aceptación/,/^---/p' Rules/Rules-Calidad-Y-Pruebas.md | grep -c "^- \[ \]"
#   Veinte, y varios son decidibles por una máquina: existencia de archivos,
#   presencia de tablas, ausencia de sufijos en los nombres, tabla de contenido.
```

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-12 | Reporte inicial, emitido al cerrar la Fase E tras tres rondas de audit. Documenta que el audit es la única compuerta de fase y no tiene ninguna comprobación mecánica delante, con la medición de las tres rondas: 33 hallazgos, 22 de ellos detectables por un guion. Enuncia el patrón —un solo instrumento para propiedades de dos naturalezas— y propone cinco intervenciones, incluida la que este destino ya construyó y las dos que no resolvió: el criterio de corte de las rondas y la clasificación de los criterios de aceptación. |
| 1.1 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** El audit era la única compuerta, y verificaba con criterio lo que se podía contar.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `Master-Prompt.md` §10.0 | La **compuerta mecánica previa al audit**, que verifica lo enumerable antes de que nadie interprete |
| `Master-Prompt.md` §10 y §10.1 | El criterio de corte de las rondas y la marca de detectabilidad |
| §6 de las diecisiete reglas | Cada criterio de aceptación se clasifica en `[enumerable]` o `[interpretativo]` |

**Después de la 7.0.** La **8.4** la llevó un paso más: de **contar** defectos a **repararlos** cuando la reparación es unívoca.

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
