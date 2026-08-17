# Reporte 02 — La Fase B2 itera, y su propagación es un solo evento al final

| Campo | Valor |
|---|---|
| Reporte | 02 |
| Fecha | 2026-08-10 |
| Origen | Corrida real de la Fase B2 sobre el destino `Repos-RPIs/RPI.VidelControl`, proyecto de código `VideoControl-Web`, tres iteraciones de maqueta entre el 2026-08-09 y el 2026-08-10 |
| Versión del framework evaluada | SDD 6.0 (`Master-Prompt` 5.2, `Maqueta-Rules` 3.1, `Deriva-Rules`, invariantes D1 a D9) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Maqueta-Rules.md` §3.6, §8 y §9; `SDD/Devs/Orchestrator/Master-Prompt.md` §441 y §442 |
| Naturaleza | Dos huecos de la matriz de propagación, verificados con tres incidentes de una misma corrida |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |
| Reportes relacionados | `03-Conjuntos-Cerrados-Entre-Categorias.md`, que documenta un tercer incidente de la misma corrida cuya causa es distinta |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**. Cada afirmación sobre la normativa cita el archivo y la línea donde se verifica.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. Lo que la normativa sí resuelve](#2-lo-que-la-normativa-sí-resuelve)
- [3. Los tres incidentes](#3-los-tres-incidentes)
- [4. La causa raíz](#4-la-causa-raíz)
  - [4.1 Hueco A — la propagación es un evento y la fase es un bucle](#41-hueco-a--la-propagación-es-un-evento-y-la-fase-es-un-bucle)
  - [4.2 Hueco B — la matriz no contempla que la fase cree un proyecto de código](#42-hueco-b--la-matriz-no-contempla-que-la-fase-cree-un-proyecto-de-código)
- [5. Por qué no es culpa del orquestador](#5-por-qué-no-es-culpa-del-orquestador)
- [6. El patrón, enunciado](#6-el-patrón-enunciado)
- [7. Propuestas de intervención](#7-propuestas-de-intervención)
- [8. Cómo verificar que la corrección funcionó](#8-cómo-verificar-que-la-corrección-funcionó)
- [9. Anexo — evidencia reproducible](#9-anexo--evidencia-reproducible)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

`Maqueta-Rules` §3.5 declara el ciclo de corrección como **iterativo**, y §3.6 declara la propagación como **un paso posterior, disparado por la aprobación**. Los dos son correctos por separado. Juntos producen un intervalo, tan largo como dure la validación, en el que la documentación describe un producto y la maqueta aprobada describe otro, sin que nada lo señale.

En esta corrida ese intervalo duró dos días y tres iteraciones. Al final, la documentación estaba **catorce estados, dos representaciones y un proyecto de código entero** por detrás de lo que el humano ya había aprobado. Ningún control lo detectó: la omisión de la propagación es hallazgo P0 del audit (`Maqueta-Rules.md:451`), pero ese audit corre al cerrar la fase, es decir después del intervalo.

El segundo hueco es más específico y más caro: la iteración 2 **creó un proyecto de código nuevo**, y la matriz de propagación no tiene ninguna fila para eso. Sus ocho filas propagan a categorías de proyectos de código que ya existen.

## 2. Lo que la normativa sí resuelve

Conviene decirlo primero, porque acota el hallazgo y evita que un prompt de intervención reescriba lo que está bien:

| Qué | Dónde |
|---|---|
| La propagación es obligatoria y su omisión es P0 | `Maqueta-Rules.md:221` y `:451` |
| Hay matriz explícita, con ocho filas y dirección declarada | `Maqueta-Rules.md:229-238` |
| Hay orden: primero 03, después hacia atrás, después hacia adelante | `Maqueta-Rules.md:225-227` |
| Hay regla de corte para el intake y para 00 y 01 | `Maqueta-Rules.md:240`, `Master-Prompt.md:442` |
| Hay criterio de aceptación que exige versión y control de cambios en cada documento retroalimentado | `Maqueta-Rules.md:436` |

**El problema no es la ausencia de propagación.** Es cuándo se dispara y qué no está en la matriz.

## 3. Los tres incidentes

| # | Qué pasó | Cuándo se detectó | Cómo |
|---|---|---|---|
| I-1 | La iteración 2 incorporó a la maqueta un componente empaquetado, `VideoControl.PinMap`, que es **un proyecto de código nuevo del producto**. El `PRODUCT-MANIFEST` siguió declarando cuatro proyectos y el árbol `SDD/Docs/Proyectos/` siguió teniendo cuatro carpetas | Dos días después | El humano preguntó si lo trabajado en la maqueta se había tomado en la documentación |
| I-2 | La iteración 2 obligó al documento de mapa del equipo a ganar una sección nueva —la geometría de la cabecera—, con su validación, su código de error y su efecto sobre la exportación. El `PRODUCT-INTAKE` §20.E-7 no cambió | Ídem | Ídem |
| I-3 | Las iteraciones 2 y 3 sumaron catorce estados de superficie que los wireframes de 03 no declaraban. La maqueta demostraba 191 estados y los trece wireframes declaraban 177 | Ídem | Ídem |

Los tres se detectaron por la misma vía —una pregunta del humano— y ninguno por un control del método. Nótese que I-1 e I-2 son de la iteración 2, cuya validación el humano **ya había aprobado**: por la letra de §3.6 la propagación no estaba vencida, porque la maqueta como tal todavía no estaba aprobada. La regla se cumplió y el resultado igual fue una documentación que mentía durante dos días.

## 4. La causa raíz

### 4.1 Hueco A — la propagación es un evento y la fase es un bucle

`Maqueta-Rules.md:221` abre el paso 6 con «**Con la maqueta aprobada**, el orquestador propaga lo aprendido». El disparador es la aprobación final. Pero §3.5 declara el paso 5 como «Ciclo de corrección y validación (detención, **iterativo**)», y en la práctica cada iteración termina con una aprobación del humano que no es la aprobación de la fase.

De ahí salen dos consecuencias que el framework no nombra:

1. **La deriva se acumula.** Cada iteración agrega distancia entre lo aprobado y lo documentado, y la distancia sólo se paga al final, cuando ya nadie recuerda de qué iteración salió cada cambio. En esta corrida hubo que reconstruirlo leyendo la maqueta contra los wireframes.
2. **Un audit que corra en el intervalo reporta divergencias legítimas como defectos.** No hay forma de distinguir «esto todavía no se propagó porque la fase está abierta» de «esto no se propagó y nadie se dio cuenta», porque la documentación no lleva ninguna marca de que hay una fase abierta sobre ella.

El anti-patrón de `Maqueta-Rules.md:451` describe exactamente el efecto —«queda una documentación que describe un producto distinto del aprobado; es la deriva que la fase venía a evitar»— y lo atribuye a no ejecutar el paso 6. Acá el paso 6 no estaba vencido y el efecto ocurrió igual.

### 4.2 Hueco B — la matriz no contempla que la fase cree un proyecto de código

Las ocho filas de la matriz (`Maqueta-Rules.md:229-238`) tienen una propiedad común que no está declarada y que las limita: **todas propagan a categorías de proyectos de código que ya existen**. La fila más ambiciosa, «alcance funcional que la maqueta mostró faltante», llega hasta 00, 01, 02, 06 y 07 — todas categorías de un árbol ya creado.

Ninguna fila contempla el caso de esta corrida: la validación decidió que una pieza no era de la maqueta sino un **componente empaquetado con contrato propio**, y con eso creó un proyecto de código. Los destinos de esa propagación no son categorías: son el `PRODUCT-INTAKE` §13, §16, §17 y §18, el `PRODUCT-MANIFEST` §2, §3 y §4, y un árbol `SDD/Docs/Proyectos/<nuevo>/` completo que exige **volver a correr la Fase B para ese proyecto**.

Hay además un destino que la matriz no menciona en ninguna de sus ocho filas y que sin embargo es obligatorio en cuanto se toca §13 del intake: **el `PRODUCT-MANIFEST`**. La regla de corte de `Maqueta-Rules.md:240` nombra el intake y nombra 00 y 01, y no nombra el manifiesto, que es documento derivado del intake y queda desincronizado en silencio.

En esta corrida, la incorporación del quinto proyecto de código además **invalidó una afirmación del manifiesto**: §3 declaraba «la cadena es lineal, de modo que no hay proyectos de código paralelizables: cada nivel tiene exactamente uno». Con el proyecto nuevo el nivel 0 pasa a tener dos. Es el tipo de afirmación derivada que nadie revisa porque nadie la marcó como derivada.

## 5. Por qué no es culpa del orquestador

Porque el orquestador cumplió la regla. §3.6 se dispara con la aprobación de la maqueta, la maqueta no estaba aprobada, y no hay ninguna instrucción que le pida propagar entre iteraciones ni que le pida detectar que una iteración creó un proyecto de código.

Tampoco es culpa del subagente maquetador: `Maqueta-Rules.md:32` le prohíbe explícitamente corregir la especificación por su cuenta y le pide emitir el hallazgo para que la corrección se aplique en el paso 6. Hizo eso.

Es un hueco del método: la fase que más descubre es la única cuya salida hacia el resto del árbol depende de un único evento al final.

## 6. El patrón, enunciado

Enunciado en forma general, para que la intervención no se limite a este caso:

> **Cuando una fase es iterativa y su propagación es puntual, la distancia entre lo aprobado y lo documentado crece sin que nada la mida.** Y cuando una matriz de propagación enumera destinos, lo que no está en la lista no se propaga aunque sea consecuencia directa de lo aprobado: una matriz cerrada sin regla de escape convierte cada caso no previsto en una omisión silenciosa.

El patrón tiene una segunda cara que conviene registrar: **la Fase B2 es la única fase del framework que puede descubrir cosas que las fases anteriores no podían saber**, porque es la única donde el humano ve el producto antes de que exista. Que su salida hacia atrás sea un solo paso al final es desproporcionado respecto de lo que produce.

## 7. Propuestas de intervención

No se aplican acá. Se enumeran para el prompt que intervenga el framework.

| # | Intervención | Artefacto | Efecto esperado |
|---|---|---|---|
| P-1 | Que §3.6 declare la propagación **por iteración** y no sólo por aprobación de la fase: al cerrar cada iteración, propagar lo aprobado en ella, o registrar explícitamente qué queda diferido y por qué | `Maqueta-Rules.md` §3.5 y §3.6 | Elimina el intervalo. Cada iteración deja la documentación consistente o deja escrito que no lo está |
| P-2 | Que la matriz gane una fila para **«la validación creó un proyecto de código»**, con sus destinos: intake §13, §16, §17, §18; manifiesto §2, §3, §4; y una corrida de Fase B para el árbol nuevo | `Maqueta-Rules.md` §3.6 | El caso deja de ser invisible |
| P-3 | Que la matriz gane una **regla de escape**: todo hallazgo que no encaje en ninguna fila se declara explícitamente, con su destino propuesto, en lugar de no propagarse | `Maqueta-Rules.md` §3.6 | Una matriz cerrada deja de comportarse como una lista de lo único que existe |
| P-4 | Que la regla de corte nombre el **`PRODUCT-MANIFEST`** junto al intake: tocar §13 del intake obliga a rederivarlo | `Maqueta-Rules.md` §3.6, `Master-Prompt.md` §442 | El derivado deja de desincronizarse en silencio |
| P-5 | Que los documentos alcanzados por una fase abierta lleven una **marca de fase en curso**, para que un audit intermedio distinga deriva legítima de omisión | `Maqueta-Rules.md` §3.5, criterios de audit del `Master-Prompt` §10 | Un audit en el intervalo deja de producir falsos positivos y de ocultar los verdaderos |

## 8. Cómo verificar que la corrección funcionó

1. Correr una Fase B2 con al menos dos iteraciones y comprobar que al cerrar la primera existe propagación aplicada o diferimiento declarado.
2. Contar los estados demostrados por la maqueta y los declarados en la sección 5 de los wireframes: los dos números tienen que coincidir al cerrar cada iteración, no sólo al cerrar la fase.
3. Introducir deliberadamente en una iteración un hallazgo que no encaje en ninguna fila de la matriz y comprobar que se declara en lugar de perderse.
4. Comprobar que tocar §13 del intake produce una versión nueva del manifiesto en la misma corrida.

## 9. Anexo — evidencia reproducible

```bash
# Hueco A: el disparador de la propagación es la aprobación de la fase
grep -n "Con la maqueta aprobada" SDD/Devs/Rules/Maqueta-Rules.md
# → 221

# ...y el paso anterior es iterativo
grep -n "Ciclo de corrección y validación" SDD/Devs/Rules/Maqueta-Rules.md
# → 201 (título del §3.5, con «(detención, iterativo)»)

# Hueco B: las ocho filas de la matriz, ninguna con proyecto de código nuevo
sed -n '231,238p' SDD/Devs/Rules/Maqueta-Rules.md

# La regla de corte nombra intake, 00 y 01; no nombra el manifiesto
sed -n '240p' SDD/Devs/Rules/Maqueta-Rules.md

# I-3, medido sobre el destino antes de la corrección:
#   maqueta 191 estados / trece wireframes 177
```

Sobre el destino, después de la propagación aplicada a mano, los dos números coinciden en 191. La corrección se aplicó al producto y **no al framework**: este reporte existe para que la corrección del framework se decida con evidencia.

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-10 | Reporte inicial: dos huecos de la matriz de propagación de la Fase B2 —el disparador puntual sobre una fase iterativa, y la ausencia de fila para el caso en que la validación crea un proyecto de código— verificados con tres incidentes de una corrida real, con cinco propuestas de intervención. |
| 1.1 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** La propagación de los hallazgos de la Fase B2 hacia atrás y hacia adelante.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `Maqueta-Rules.md` §3.5 y §3.6 | La propagación se declara **por iteración**, con su regla de escape para que no cicle, y la regla de corte nombra el manifiesto: si alcanza §13 del intake, obliga a **rederivar el `PRODUCT-MANIFEST` en la misma corrida** |

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
