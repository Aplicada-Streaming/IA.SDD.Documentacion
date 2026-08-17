# Reporte 08 — La iteración es del equipo y la categoría que la documenta es del proyecto de código

| Campo | Valor |
|---|---|
| Reporte | 08 |
| Fecha | 2026-08-11 |
| Origen | Corrida real del orquestador sobre el destino `Repos-RPIs/RPI.VidelControl`: Fase D, categoría 07 de los cinco proyectos de código, 2026-08-11 |
| Versión del framework evaluada | SDD 6.0 (`Rules-Plan-Sprint` cabecera, §2.1, §2.2, §4.2, §4.6 y §6; `Rules-Contexto` §4.2; `Master-Prompt` §4 y §6) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Rules-Plan-Sprint.md`; por extensión, `Vocabulario-Rules.md` §4 R3, que es donde se declara el nivel de aplicación de cada regla |
| Naturaleza | Un artefacto declarado a nivel proyecto de código cuyo referente —la iteración, la capacidad y la velocidad— es del equipo, que es un nivel producto |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |
| Reportes relacionados | `06-Obligatoriedad-Por-Tipo-Sin-Condicion.md`, que documenta el mismo error de nivel en otro atributo; `01-Ambito-De-Unicidad-De-Identificadores.md`, que lo documenta para el ámbito de un identificador |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. El incidente](#2-el-incidente)
- [3. Lo que la normativa dice, con precisión](#3-lo-que-la-normativa-dice-con-precisión)
- [4. Las cuatro consecuencias concretas](#4-las-cuatro-consecuencias-concretas)
- [5. La causa raíz](#5-la-causa-raíz)
- [6. El patrón, enunciado](#6-el-patrón-enunciado)
- [7. Qué hizo el destino](#7-qué-hizo-el-destino)
- [8. Propuestas de intervención](#8-propuestas-de-intervención)
- [9. Cómo verificar que la corrección funcionó](#9-cómo-verificar-que-la-corrección-funcionó)
- [10. Anexo — evidencia reproducible](#10-anexo--evidencia-reproducible)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

La categoría 07 declara en su cabecera que su nivel de aplicación es el **proyecto de código**, y emite por cada uno un plan por iteración, dos plantillas de ceremonia y un documento de velocidad. Todo lo que esos artefactos describen, en cambio, es del **equipo**: la iteración es una ventana de calendario, la capacidad es de las personas, la velocidad es la de un equipo, y las tres ceremonias las hace un grupo, no un ensamblado.

En un producto de un solo proyecto de código la diferencia no se nota y el reparto es correcto. En uno de cinco, con un equipo que los construye a todos dentro de la misma ventana, produce cinco documentos de velocidad de los cuales **ninguno mide una magnitud estable**, y deja sin lugar al único que sí lo sería. La propia regla lo insinúa sin sacar la consecuencia: condiciona la forma de sus artefactos a `equipo_n`, que es un atributo del producto y no del proyecto de código.

## 2. El incidente

El destino tiene cinco proyectos de código y un equipo de tres personas. El roadmap del producto, emitido en la Fase A, ya reparte el trabajo en diez iteraciones numeradas de `S0` a `S9` y le asigna a cada una sus épicas.

Al llegar a la Fase D, la categoría 07 pide emitir por cada proyecto de código un plan por iteración y un `Velocidad-Equipo.md`. Las preguntas que no tienen respuesta en la normativa aparecieron todas juntas:

- La iteración `S01` de `VideoControl-Web` y la `S01` de `VideoControl-Domain`, ¿son la misma iteración o dos? El calendario dice que es una sola. La numeración de la regla, que pide «mínimo Sprint 0 y Sprint 1» por proyecto de código, sugiere que cada uno cuenta las suyas.
- `VideoControl-PinMap` no tiene ningún trabajo comprometido en las iteraciones `S01` a `S03`, porque la cabecera de pines recién hace falta cuando aparece el alta de actuadores. ¿Su `Sprint 1` es el primero del producto, y entonces está vacío, o es el primero suyo, y entonces el número `01` significa una cosa distinta que en los otros cuatro documentos?
- La capacidad del equipo, ¿se declara cinco veces repartida, cinco veces completa, o una sola vez en algún lugar que la categoría no tiene?
- La velocidad de `VideoControl-Domain`, ¿qué es? El equipo no tiene una velocidad por proyecto de código: tiene una, y la reparte según lo que cada iteración pida.

Ninguna de las cuatro tiene respuesta en `Rules-Plan-Sprint.md`.

## 3. Lo que la normativa dice, con precisión

| Dónde | Qué dice | Qué nivel presupone |
|---|---|---|
| Cabecera de `Rules-Plan-Sprint.md` | «Nivel de aplicación: Proyecto de código» | Proyecto de código |
| §2.1 | `Velocidad-Equipo.md` obligatorio «para todos los tipos D8 con equipo mayor a 1 dev» | El artefacto es por proyecto de código; el nombre y la condición son del equipo |
| §2.2 | La forma de la categoría se decide por el **tamaño del equipo**, en prosa; la regla no nombra el flag | Producto |
| `Master-Prompt.md` §4 y §6 | El flag `equipo_n`, leído del documento de entrada, decide qué emite la categoría 07 | Producto |
| §4.2 punto 1 | «composición del equipo, capacidad disponible» en cada plan | Producto |
| §4.6 | Velocidad por iteración con promedio móvil de tres | Equipo |
| §6 | «Existe `Plan-Iteracion-Sprint-XX.md` para cada sprint planificado, mínimo Sprint 0 y Sprint 1» | Numeración por proyecto de código |
| `Rules-Contexto.md` §4.2 | El roadmap y el acuerdo de equipo, en la categoría 00 | Producto |

**Lo que el framework sí resuelve bien y no hay que reescribir.** La decisión de que el trabajo se planifique por proyecto de código es correcta y no es lo que se cuestiona: el alcance técnico, la trazabilidad a casos de uso y el orden entre tareas son propios de cada proyecto de código, y mezclarlos en un plan único produciría un documento que nadie puede usar. El acuerdo de equipo y el roadmap ya están en el nivel producto, que es donde corresponden. Y `Vocabulario-Rules.md` §4 R3 —la regla que obliga a declarar el nivel de aplicación en la cabecera— es exactamente el mecanismo correcto: es la que hace visible el problema en lugar de esconderlo.

**Lo que el framework no dice.** Que dentro de una categoría puede haber artefactos de dos niveles distintos. La cabecera declara **un** nivel para toda la regla, y la categoría 07 tiene, en el mismo cajón, artefactos que son del proyecto de código —el plan, con su alcance técnico— y artefactos que son del equipo —la velocidad, la capacidad, las ceremonias—.

## 4. Las cuatro consecuencias concretas

**Cinco velocidades que no son magnitudes.** «La velocidad de `VideoControl-Domain`» no mide el rendimiento de nada: mide cuánto de la iteración se gastó en ese proyecto de código, que depende de qué épica tocaba. Una velocidad baja no dice que el equipo rindió poco; dice que la iteración pidió poco de ahí. Las cinco cifras no son comparables entre sí, y la única que tiene interpretación estable —su suma— no tiene dónde vivir, porque la categoría 07 no tiene nivel producto.

**La regla de capacidad se vuelve inaplicable.** §4.6 punto 3 fija «no comprometer más del 110 % del promedio móvil de tres iteraciones». Aplicada sobre cinco series parciales da cinco topes que, sumados, no guardan ninguna relación con la capacidad real del equipo. Aplicada sobre la suma, funciona, pero esa suma no existe en ningún artefacto.

**Dos categorías numeran lo mismo en niveles distintos.** El roadmap de la 00 numera las iteraciones a nivel producto, `S0` a `S9`, y les asigna épicas. La 07 pide «mínimo Sprint 0 y Sprint 1» por proyecto de código. Si se respeta el roadmap, hay proyectos de código cuyo primer plan es `Sprint 04` y el criterio de aceptación falla. Si se respeta el criterio, el número `01` significa una iteración distinta en cada uno de los cinco documentos, y el calendario deja de ser reconstruible. Es la misma clase de conflicto que el reporte `03` documenta para los conjuntos cerrados, con el agravante de que acá lo que colisiona es un **identificador**.

**Una iteración con sentido funcional puede quedar partida en porciones que no lo tienen.** `Rules-Plan-Sprint.md` §3.4 dice que «un sprint sin trazabilidad a CU es un sprint sin sentido funcional», y §6 lo hace criterio de aceptación. En la iteración `S01` del destino el producto entrega la puesta en marcha completa y avanzan seis casos de uso repartidos en tres proyectos de código —`CU-08`, `CU-09` y `CU-28` en la interfaz, `CU-71` y `CU-80` en la infraestructura, `CU-35` en el dominio—; el plan de `VideoControl-Application` para esa misma iteración declara **ninguno**, porque lo que aporta esa capa son la exigencia de identidad y la compuerta del mapa, que son la condición de esos seis casos de uso y no ninguno de ellos. El criterio es correcto sobre la iteración y falso sobre la porción, y la categoría sólo documenta porciones.

## 5. La causa raíz

El framework asigna un nivel a la **categoría** y no a cada **artefacto**. Es una simplificación que funciona en las once categorías restantes, porque en ellas todos los artefactos comparten el nivel de su categoría, y falla en la 07 porque ahí conviven dos.

Debajo hay algo más de fondo, y es lo mismo que el reporte `06` documenta para la obligatoriedad: **el nivel de un artefacto se derivó de dónde está y no de qué habla**. La 07 quedó a nivel proyecto de código porque planifica construcción, y la construcción es por proyecto de código; nadie preguntó de qué habla `Velocidad-Equipo.md`, que lo dice en el nombre.

Un indicio de que el framework estuvo cerca de verlo: qué forma toma la categoría 07 se decide por el **tamaño del equipo**, que es un dato del producto. Conviene ser preciso sobre dónde está escrito, porque la imprecisión debilitaría el hallazgo: `Rules-Plan-Sprint.md` **no menciona `equipo_n` ni una vez** —§2.2 condiciona en prosa por «equipo de 2 o más devs» y «equipo de 1 dev»—, y el nombre del flag aparece en `Master-Prompt.md` §4 y §6, que es donde el orquestador lo lee del documento de entrada. Es decir: **el atributo que decide la forma de la categoría es del producto y ni siquiera vive en la regla de la categoría**, sino en el plan que la despacha. El framework ya sabe que el equipo es uno solo para todo el producto, y aun así emite un documento de velocidad por cada proyecto de código.

## 6. El patrón, enunciado

> El framework declara el nivel de aplicación por categoría, presuponiendo que todos los artefactos de una categoría hablan del mismo nivel. Cuando una categoría contiene artefactos de dos niveles —los que describen el trabajo, que es por proyecto de código, y los que describen a quien lo hace, que es del producto—, los del nivel equivocado se replican una vez por proyecto de código y dejan de medir lo que su nombre dice. La señal de que está ocurriendo es que la propia regla condicione su forma a un atributo del nivel superior.

## 7. Qué hizo el destino

Se emitieron los artefactos que la regla pide, sin apartarse de su forma, y se declaró la lectura correcta dentro de cada uno:

1. **Numeración del producto.** Los diez planes usan la numeración del roadmap: el `Sprint 01` de un proyecto de código es el mismo `S01` que el de los demás. Cada plan lo dice en §1, con la frase «este plan describe una porción, no una iteración».
2. **Se emite el plan de la iteración de arranque y el de la primera con capacidad**, que en `VideoControl-PinMap` es la `S04`. El README de la categoría explica por qué no están las intermedias, y declara además que ese proyecto de código pasa tres iteraciones sin comprometer capacidad y después concentra todo el suyo en una, que es una concentración de riesgo real y quedó registrada como tal en el plan.
3. **Capacidad declarada una vez y sin repartir.** Los cinco planes declaran la misma capacidad del equipo, y la declaran **sin determinar**: el documento de entrada fija que el equipo es de tres personas (SUP-01) y el acuerdo de equipo §2 declara que sus integrantes no trabajan a tiempo completo en el producto, y ninguno de los dos fija cuántas horas aporta cada quien. Repartirla entre cinco proyectos de código habría producido cinco cifras inventadas en lugar de una desconocida.
4. **Cada `Velocidad-Equipo.md` tiene una sección §5 que no está en la regla**, titulada «Qué mide y qué no mide esta tabla». Declara que mide la porción de la velocidad del equipo gastada en ese proyecto de código, que las cinco tablas no son comparables entre sí, y que sumarlas sí da la velocidad del equipo —que es la única de las seis cifras con interpretación estable, y no tiene dónde vivir—.

## 8. Propuestas de intervención

Ninguna está decidida; son punto de partida.

1. **Declarar el nivel por artefacto y no por categoría.** Es la corrección de fondo y alcanza a `Vocabulario-Rules.md` §4 R3 y a la cabecera de las doce reglas. La tabla maestra §2.1 de cada regla gana una columna de nivel, y en las once categorías donde hoy todos coinciden el cambio es una columna con el mismo valor repetido.
2. **Mover al nivel producto los artefactos de la 07 que hablan del equipo.** `Velocidad-Equipo.md` y la capacidad pasan a `SDD/Docs/Producto/07-Plan-Sprint/`, y el plan por proyecto de código deja de repetirlos y los referencia. Las plantillas de ceremonia también son del equipo: hoy se emiten cinco copias idénticas.
3. **Declarar dónde se numeran las iteraciones.** Si el roadmap ya las numera —y en este framework lo hace, en la 00—, esa numeración es la del producto y la 07 la consume. El criterio «mínimo Sprint 0 y Sprint 1» pasa a ser «la iteración de arranque y la primera con trabajo comprometido en este proyecto de código», que es lo que se quería decir.
4. **Convertir la sección §5 que el destino inventó en sección obligatoria** del documento de velocidad mientras siga estando a nivel proyecto de código. Es la intervención más barata y la única que se puede aplicar sin tocar la estructura: si la cifra va a seguir sin ser una magnitud, que el documento lo diga.
5. **Regla de detección para el resto del framework.** Una regla cuya forma se condiciona a un atributo del nivel superior —`equipo_n` acá, y conviene buscar si hay otros— es candidata a tener este mismo problema. Es una comprobación mecánica sobre las doce reglas.

## 9. Cómo verificar que la corrección funcionó

- Cada artefacto del framework declara su nivel, y en la categoría 07 no todos declaran el mismo.
- Existe una sola declaración de capacidad del equipo en todo el árbol de un producto, y los planes la referencian.
- Existe una sola serie de velocidad por equipo, y sumar series parciales deja de ser necesario para obtener una cifra interpretable.
- El número de una iteración identifica la misma ventana de calendario en los planes de todos los proyectos de código, verificable comparando dos planes del mismo número.
- El criterio de aceptación sobre los planes mínimos se puede satisfacer en un proyecto de código cuya primera iteración con trabajo es la cuarta del producto.

## 10. Anexo — evidencia reproducible

```bash
cd IA/IA.SDD/SDD/Devs

# 1. El nivel declarado de la categoría, y los artefactos que no le corresponden.
head -6 Rules/Rules-Plan-Sprint.md | grep "Nivel de aplicación"
grep -n "Velocidad-Equipo.md\|capacidad disponible\|composición del equipo" Rules/Rules-Plan-Sprint.md

# 2. La forma de la categoría decidida por un atributo del nivel producto.
grep -n "equipo_n" Orchestrator/Master-Prompt.md Rules/Rules-Plan-Sprint.md Rules/Rules-Contexto.md

# 3. El criterio que presupone numeración por proyecto de código.
grep -n "mínimo Sprint 0 y Sprint 1" Rules/Rules-Plan-Sprint.md

# 4. La numeración de nivel producto que ya existe en la categoría 00 del destino.
grep -n "^| E[0-9] |" ../../../../Repos-RPIs/RPI.VidelControl/SDD/Docs/00-Contexto/Roadmap-Producto.md | cut -c1-90

# 5. Las cinco copias idénticas de las plantillas de ceremonia en el destino.
for f in ../../../../Repos-RPIs/RPI.VidelControl/SDD/Docs/Proyectos/*/07-Plan-Sprint/Template-Sprint-Retrospectiva.md
do tail -n +9 "$f" | md5sum; done | awk '{print $1}' | sort -u | wc -l
#   Devuelve 1: descontada la cabecera, que sólo cambia el nombre del proyecto de código,
#   los cinco archivos son byte a byte el mismo documento.
```

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-11 | Reporte inicial, emitido al cerrar la Fase D. Documenta que la categoría 07 declara un solo nivel de aplicación para artefactos de dos niveles distintos, y que en un producto multiproyecto eso produce cinco series de velocidad sin interpretación estable, una regla de capacidad inaplicable y una colisión de numeración de iteraciones con el roadmap de la categoría 00. Enuncia el patrón, señala que la propia regla condiciona su forma a un atributo del nivel superior, y propone cinco intervenciones. |
| 1.1 | 2026-08-11 | Correcciones del audit independiente de la Fase D. **Se corrige la fuente de una cita**: que el equipo no trabaja a tiempo completo lo dice el acuerdo de equipo §2, no el documento de entrada, que sólo fija `equipo_n` = 3 en SUP-01. **Se corrige una atribución**: `Rules-Plan-Sprint.md` no menciona `equipo_n` —el flag vive en `Master-Prompt.md` §4 y §6—, lo que refuerza el hallazgo en vez de debilitarlo, porque el atributo que decide la forma de la categoría ni siquiera está en la regla de la categoría. Y se agrega una cuarta consecuencia que el audit hizo visible: el criterio «un sprint sin trazabilidad a CU es un sprint sin sentido funcional» es correcto sobre la iteración y falso sobre la porción por proyecto de código, que es lo único que la categoría documenta. |
| 1.2 | 2026-08-11 | Corrección de la segunda ronda del audit: §4 decía que en la iteración `S01` avanzan cuatro casos de uso y son seis. El recuento se desactualizó por la propia corrección de la ronda anterior, que agregó el plan de `S01` de `VideoControl-Domain` con su caso de uso. Es el defecto que el reporte `04` documenta, ocurriendo dentro de un reporte. |
| 1.4 | 2026-08-11 | Corrección de la ronda 3: la frase de §7 quedó a medio camino entre las dos redacciones —«sin trabajo de capacidad comprometido», que no significa ninguna de las dos— y describía el README del destino diciendo tres cuando ese README dice cuatro. Se corrige la frase y se corrige el destino, que era donde estaba el error. |
| 1.3 | 2026-08-11 | Correcciones de la ronda 2 del audit. La corrección anterior de §4 se aplicó a media oración: pasó «cuatro» a «seis» en la enumeración y lo dejó en «esos cuatro» doce palabras después. Y §7 decía que `VideoControl-PinMap` pasa cuatro iteraciones sin trabajo comprometido cuando son tres, y su iteración de arranque sí compromete once puntos: lo que pasa tres iteraciones sin comprometer es **capacidad**, no trabajo. |
| 1.4 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** El sprint es del equipo y la categoría era del proyecto de código.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `Vocabulario-Rules.md` §4 R3 | El **nivel de aplicación por artefacto**, que es la abstracción que faltaba |
| `Rules-Plan-Sprint.md` | Los artefactos del equipo pasan a **nivel producto**, con la numeración de iteraciones corregida |

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
