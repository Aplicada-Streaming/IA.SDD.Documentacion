# Reporte 01 — Ámbito de unicidad de los identificadores

| Campo | Valor |
|---|---|
| Reporte | 01 |
| Fecha | 2026-08-10 |
| Origen | Corrida real del orquestador de generación sobre el destino `Repos-RPIs/RPI.VidelControl`, un producto de **cuatro proyectos de código** |
| Versión del framework evaluada | SDD 6.0 (`Master-Prompt` 5.2, `Rules-Especificacion-Funcional` 4.0, `Rules-UX-UI-DX` 4.0, invariantes D1 a D9) |
| Artefactos del framework alcanzados | `README.md` (invariante D3), `SDD/Devs/Rules/Rules-Especificacion-Funcional.md`, `SDD/Devs/Rules/Rules-Necesidades-Negocio.md`, `SDD/Devs/Orchestrator/Master-Prompt.md` |
| Naturaleza | Un hueco normativo con dos consecuencias, verificado con cinco incidentes de una misma corrida |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |
| Reporte relacionado | `00-Regla-Transcripción.md`, que documenta otro hueco de la misma clase: el framework exige un artefacto y no declara una de sus propiedades |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**. Cada afirmación sobre la normativa cita el archivo y la línea donde se verifica, y el anexo trae los comandos que reproducen cada verificación.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. Los cinco incidentes](#2-los-cinco-incidentes)
- [3. Lo que la normativa dice y lo que no dice](#3-lo-que-la-normativa-dice-y-lo-que-no-dice)
- [4. La causa raíz](#4-la-causa-raíz)
  - [4.1 Hueco A — el ámbito de unicidad no está declarado](#41-hueco-a--el-ámbito-de-unicidad-no-está-declarado)
  - [4.2 Hueco B — hay una familia de identificadores sin forma declarada](#42-hueco-b--hay-una-familia-de-identificadores-sin-forma-declarada)
  - [4.3 Hueco C — el control existe pero llega al final](#43-hueco-c--el-control-existe-pero-llega-al-final)
- [5. Por qué no es culpa del orquestador ni de los subagentes](#5-por-qué-no-es-culpa-del-orquestador-ni-de-los-subagentes)
- [6. Por qué recién aparece ahora](#6-por-qué-recién-aparece-ahora)
- [7. Propuestas de intervención](#7-propuestas-de-intervención)
- [8. Cómo verificar que la corrección funcionó](#8-cómo-verificar-que-la-corrección-funcionó)
- [9. Anexo — evidencia reproducible](#9-anexo--evidencia-reproducible)

---

## 1. Resumen

El framework declara la **forma** de sus identificadores y no declara su **ámbito de unicidad**. Al mismo tiempo, una categoría de nivel producto cita por identificador desnudo a artefactos de nivel proyecto de código, lo que sólo funciona si el ámbito es el producto, mientras el layout de salida ubica esos artefactos en carpetas por proyecto, lo que sugiere que el ámbito es el proyecto. Las dos lecturas son defendibles, el framework no elige, y en un producto de más de un proyecto de código alguien tiene que decidir sin que nada le indique que hay una decisión que tomar.

A eso se suma que existe una familia entera de identificadores que las propias reglas exigen —los códigos de error, y los que cada categoría acuñe para catalogar lo suyo— sobre la que la normativa no declara **ni siquiera la forma**.

El resultado, en una corrida de cuatro proyectos de código con cinco agentes trabajando en paralelo: cinco incidentes de numeración, treinta y nueve archivos renumerados a posteriori, y dos hallazgos bloqueantes producidos por la propia corrección.

| Id | Hueco | Artefacto a intervenir | Severidad propuesta |
|---|---|---|---|
| A | El ámbito de unicidad de un identificador no está declarado, y dos partes del framework exigen ámbitos incompatibles | `README.md` (D3) y `Rules-Necesidades-Negocio.md` §3.3 y §4.2 | minor en las reglas; evaluar si D3 se toca |
| B | Los códigos de error y los identificadores que cada categoría acuña no tienen forma ni ámbito declarados | `Rules-Especificacion-Funcional.md` §3.2 y §4.2, y las demás reglas que exijan catalogar | minor |
| C | El único control de duplicación corre al cierre de fase, cuando renumerar ya es caro | `Master-Prompt.md` §6 y §10 | minor |

---

## 2. Los cinco incidentes

Todos en la misma corrida, sobre el destino `Repos-RPIs/RPI.VidelControl`.

| # | Incidente | Evidencia |
|---|---|---|
| 1 | El orquestador tuvo que **inventar** la convención de rangos de casos de uso y reglas —Web 01 a 28, Domain 31 a 39, Application 51 a 69, Infrastructure 71 a 89— porque nada la fijaba, y declararla él mismo | Sección «Numeración de identificadores» de `SDD/Docs/Proyectos/*/02-Especificacion-Funcional/Especificacion-Funcional.md`, escrita por el orquestador y no derivada de ninguna regla |
| 2 | **Cinco prefijos de código de error colisionaban entre proyectos de código.** `E-SET-01` a `04` existía en Domain y en Application con las mismas cuatro situaciones **numeradas al revés**; `E-PUB-` coincidía parcialmente entre tres proyectos, que es peor que la divergencia total porque invita a suponer correspondencia | Detectado por la categoría 03 y registrado en `SDD/Docs/Audit/Audit-Fase-B-03-UX-UI-DX.md`, hallazgos H-01 y H-02, nivel P0 |
| 3 | La renumeración correctiva **dejó afuera `E-PERSIST`**, cuyo prefijo excedía la longitud que la expresión de búsqueda contemplaba, y esos códigos quedaron en el rango de otro proyecto de código | `Audit-Fase-B-02-Especificacion-Funcional.md` H-01 (P1) y `Audit-Fase-B-03-UX-UI-DX.md` H-07 (P2) |
| 4 | La categoría 03 de un proyecto de código **acuñó identificadores `EQ-01` a `EQ-04`** para catalogar equivocaciones de integración, en el rango que la convención reservaba a otro proyecto | `Audit-Fase-B-03-UX-UI-DX.md` H-03 (P1) |
| 5 | La categoría 03 **acuñó códigos `R-XXX`** para los flujos alternativos de su categoría 02, porque la 02 no les había puesto identificador, y lo elevó al orquestador declarando que la propiedad no era suya | Resumen de cierre del subagente de la categoría 03 de `VideoControl-Domain` |

Los incidentes 2 y 3 tuvieron un costo medible: **treinta y nueve archivos renumerados** después de emitidos, y **dos hallazgos P0** producidos por la propia corrección, al no propagarse a las dos secciones que describían el problema resuelto.

---

## 3. Lo que la normativa dice y lo que no dice

### Sobre la forma, dice todo

`README.md` línea 109, invariante **D3**:

> Los identificadores llevan prefijo y dos dígitos uniformes (`NB-XX`, `CU-XX`, `ADR-XX`, `US-XX` y equivalentes)

`Rules-Especificacion-Funcional.md` línea 84, §3.1:

> `CU-XX-<Nombre>.md`, con dos dígitos en `XX`, Título-Con-Guiones en el slug y guion medio antes de la versión.

### Sobre el ámbito, no dice nada

Búsqueda de las expresiones «identificador único», «identificadores únicos», «único en el producto», «único en el proyecto» y «unicidad» sobre los dieciocho archivos de `SDD/Devs/Rules/` y los dos orquestadores de `SDD/Devs/Orchestrator/`: **cero coincidencias**.

### Y sí valida colisiones, pero de otra cosa

`Intake-Rules.md` línea 90 y `Master-Prompt.md` línea 179, entre las validaciones **bloqueantes** de la derivación del manifiesto:

> No hay colisión de `Nombre-Proyecto-Codigo` ni de `Identidad-Codigo`.

El framework se ocupa explícitamente de que dos proyectos de código no colisionen en su nombre. No dice nada equivalente sobre los identificadores de los documentos que esos proyectos producen.

### La contradicción, verificable en dos cabeceras

| Archivo | Línea | Declara |
|---|---|---|
| `Rules-Necesidades-Negocio.md` | 4 | **Nivel de aplicación: Producto** |
| `Rules-Especificacion-Funcional.md` | 4 | **Nivel de aplicación: Proyecto de código** |

Y `Rules-Necesidades-Negocio.md` §4.2 punto 7 obliga a que cada necesidad —artefacto de nivel producto— lleve una sección de trazabilidad a casos de uso, cuya tabla tipo de §4.4 es:

```text
| NB | CU prevista | Estado |
| NB-01 | CU-01 reservar turno por especialidad | a generar |
```

**Sin columna de proyecto de código.** Un artefacto de nivel producto cita por identificador desnudo a artefactos de nivel proyecto de código, y eso sólo resuelve si el identificador es único en el producto.

En sentido contrario, `Master-Prompt.md` §3.5 ubica cada categoría 02 bajo `SDD/Docs/Proyectos/<Nombre-Proyecto-Codigo>/02-Especificacion-Funcional/`, lo que sugiere que el espacio de nombres es la carpeta y el ámbito es el proyecto de código.

---

## 4. La causa raíz

### 4.1 Hueco A — el ámbito de unicidad no está declarado

**Enunciado.** El framework fija la forma del identificador y no su ámbito, y tiene dos partes que exigen ámbitos incompatibles: la trazabilidad de la categoría 01 requiere unicidad de producto, y el layout de salida sugiere unicidad de proyecto de código.

**Por qué importa.** No es una ambigüedad teórica: es una decisión que **hay que tomar antes de escribir el primer caso de uso** y que nadie señala como decisión. Quien la toma no tiene forma de saber que la está tomando, y si la toma tarde, renumerar cuesta lo que costó acá.

**El único indicio que el framework ofrece apunta al lado equivocado.** `Rules-Especificacion-Funcional.md` línea 260 lista como anti-patrón:

> Numeración no contigua de CU sin justificación · Huecos confusos en el catálogo · Documentar la causa o renumerar

Ese anti-patrón obliga a justificar el salto entre el 28 y el 31, pero no dice por qué habría un salto. Llega después de la decisión, no antes.

**Consecuencia adicional sobre el formato.** Con ámbito de proyecto de código, los dos dígitos de D3 dan noventa y nueve identificadores por prefijo y por proyecto, que sobra. Con ámbito de producto, esos noventa y nueve se reparten entre todos los proyectos de código: en esta corrida, cuatro proyectos consumieron los rangos 01–28, 31–39, 51–69 y 71–89. El formato de dos dígitos deja de ser holgado exactamente cuando el ámbito es el producto, y la normativa no relaciona las dos cosas.

### 4.2 Hueco B — hay una familia de identificadores sin forma declarada

**Enunciado.** D3 enumera cuatro prefijos «y equivalentes». Los identificadores que causaron tres de los cinco incidentes no están en esa lista ni en ninguna otra, y sobre ellos la normativa no declara ni forma ni ámbito.

**Evidencia.** La única mención de los códigos de error en toda la regla de la categoría 02 es su §4.2 punto 6, línea 160:

> Excepciones y errores. Cada error con código, causa y respuesta del sistema.

Exige que haya un código. No dice qué forma tiene, ni qué prefijo, ni en qué ámbito es único, ni quién lo asigna. La expresión «código de error» aparece una sola vez en tres de los dieciocho archivos de reglas, y en ninguno con una regla de nomenclatura.

**Por qué produce colisiones con certeza y no por azar.** Un identificador cuya forma no está declarada lo inventa quien lo necesita primero. Con un único agente redactando, el resultado es consistente por accidente. Con cinco agentes en paralelo sobre cuatro proyectos de código, cada uno inventa el suyo, y como todos parten del mismo dominio los prefijos naturales coinciden —`E-SET` para un set, `E-PUB` para la publicación—. La colisión no es un descuido: es el resultado esperable.

**El incidente 5 muestra la misma raíz desde otro ángulo.** La categoría 03 necesitó citar los flujos alternativos de la 02 y descubrió que no tenían identificador, así que se los puso. Un identificador que una categoría necesita y que su categoría de origen no emite termina inventado aguas abajo, que es el peor lugar posible: el que lo acuña no es su dueño.

### 4.3 Hueco C — el control existe pero llega al final

**Enunciado.** El framework tiene un criterio de duplicación de identificadores, y está ubicado donde ya es caro corregir.

**Evidencia.** `Master-Prompt.md` línea 714, entre los criterios del audit de §10:

> Coherencia cross-doc **dentro de la fase**: referencias entre archivos resuelven, IDs no duplicados y gobierno del glosario en sus cuatro criterios

El criterio funcionó: las dos auditorías de la Fase B detectaron las colisiones. El problema es **cuándo**. El audit corre al cierre de la fase, con los identificadores ya escritos en ciento dos y treinta y seis artefactos y ya citados de forma cruzada entre proyectos de código. Renumerar en ese punto costó treinta y nueve archivos y produjo dos hallazgos P0 nuevos.

Nada equivalente corre **antes** de la fase, cuando la corrección sería declarar una convención en un solo lugar.

---

## 5. Por qué no es culpa del orquestador ni de los subagentes

**El orquestador no puede inyectar lo que la normativa no tiene.** `Master-Prompt.md` §6, «Procedimiento de lectura de las reglas», fija que cada despacho se construye copiando §1.1 y §1.2, §2.1 filtrada, §3.1 a §3.3, §4, §5, §6 y §8 del archivo de reglas de la categoría. Si esas secciones no declaran el ámbito de unicidad, el despacho no lo lleva. Es además coherente con el principio rector de su §1: el orquestador **lee** la especialidad, no la asigna, y mantenerse delgado es una propiedad buscada.

**Los subagentes se comportaron correctamente, y conviene dejarlo escrito.** El de la categoría 03 de dos proyectos de código detectó la colisión de códigos, la declaró, adoptó una forma calificada para citarla desde fuera y **la escaló al orquestador en lugar de renumerar** artefactos que no le pertenecían. El de la categoría 02 del proyecto principal hizo lo mismo con dos enunciados que consideraba mal recortados: los emitió como estaban, porque los fija la categoría 01, y los elevó. Ninguno excedió su frontera de autoridad.

El defecto no está en cómo llevaron la tarea. Está en que la tarea no traía la regla.

---

## 6. Por qué recién aparece ahora

Con **un solo proyecto de código**, las dos lecturas del ámbito coinciden y el problema no existe: la carpeta del proyecto y el producto son el mismo espacio de nombres. El framework admite ese caso explícitamente como el degenerado.

La cronología de los propios controles de cambios sugiere por qué el hueco quedó abierto. Las invariantes D1 a D8 «vienen del bootstrap del framework» (`README.md` línea 117), y las reglas de bootstrap llevan fecha **2026-05-17**. La capacidad multi-proyecto aparece después: `Root-Rules.md` registra en sus versiones 1.1 y 1.2, ambas del **2026-06-09**, la reformulación del README raíz «a documento de solución» con «la tabla de proyectos».

Es decir: **D3 se escribió cuando un producto era un proyecto de código**, y cuando el framework incorporó la composición multi-proyecto, la invariante que gobierna los identificadores no se revisó. No es un error de D3: es una consecuencia de un cambio de alcance que no propagó hasta ella.

Esta corrida es, hasta donde el destino permite verificar, la primera de cuatro proyectos de código con generación en paralelo. El hueco estaba desde junio y no tenía cómo manifestarse.

---

## 7. Propuestas de intervención

| # | Archivo | Sección | Qué agregar | Severidad |
|---|---|---|---|---|
| 1 | `README.md` | Invariante D3 | Declarar el **ámbito de unicidad** junto con la forma. La opción coherente con la trazabilidad existente es el producto, porque es la que hace resolver las citas de la categoría 01 sin cambiarlas. Si se elige el proyecto de código, hay que corregir la tabla tipo de `Rules-Necesidades-Negocio.md` §4.4 para que la cita lleve el proyecto | Tocar una invariante es el cambio de mayor impacto del framework: exige decisión explícita del responsable y nota de coherencia |
| 2 | `Rules-Especificacion-Funcional.md` | §3.2 | Declarar la nomenclatura y el ámbito de los **códigos de error**, que hoy §4.2 punto 6 exige sin regular. Incluir el criterio de asignación cuando el producto tiene más de un proyecto de código | minor |
| 3 | Todas las reglas que exijan catalogar algo con identificador | §3.2 de cada una | Regla general: **toda categoría que acuñe un identificador declara su prefijo, su forma y su ámbito**; una categoría no acuña identificadores para artefactos de otra, y si los necesita, los escala | minor por regla |
| 4 | `Master-Prompt.md` | §3, fase de validación previa | Que el orquestador **derive y publique el mapa de rangos de identificadores** junto con el bloque informativo de §3.4, cuando el manifiesto declare más de un proyecto de código, y lo incluya en cada despacho. Es el lugar natural: ya deriva nombres, orden topológico y flags | minor |
| 5 | `Master-Prompt.md` | §10 | Que el criterio «IDs no duplicados» declare explícitamente que alcanza **a los identificadores que las categorías acuñan**, y no sólo a los enumerados en D3 | minor |
| 6 | `CHANGELOG.md` | Entrada nueva | Registrar la intervención. Si se toca D3, el conjunto sube major y la entrada lleva el bloque «Impacto sobre destinos existentes» | según 1 |

**Nota de coherencia.** La intervención alcanza a más de un archivo y, si toca D3, a los dieciocho archivos de reglas y a los dos orquestadores. El `README.md` del framework exige emitir una nota de coherencia con el patrón de `Coherencia-Auditoria-Marco.md`.

**Sobre la elección de ámbito.** Este reporte no la toma: es decisión del responsable del framework. Lo que sí puede afirmarse con evidencia es que **la elección existe, hoy no está declarada, y las dos partes citadas en §3 exigen lecturas distintas**. Cualquiera de las dos, declarada, resuelve el hueco; ninguna declarada lo deja abierto.

---

## 8. Cómo verificar que la corrección funcionó

| Caso | Entrada | Resultado esperado |
|---|---|---|
| V1 | Un producto de un solo proyecto de código | La corrección no cambia nada: es el caso degenerado y las dos lecturas coinciden |
| V2 | Un producto de tres o más proyectos de código, antes de despachar la primera categoría por proyecto de código | El orquestador publica el mapa de rangos de identificadores junto al bloque informativo, y cada despacho lo lleva |
| V3 | Dos subagentes generando en paralelo la categoría 02 de dos proyectos de código, cada uno con un caso de uso sobre el mismo concepto del dominio | Los códigos de error resultantes no colisionan, sin que ninguno de los dos haya tenido que consultar al otro |
| V4 | Una categoría que necesita citar un elemento de otra categoría que no tiene identificador | Lo escala en lugar de acuñarlo, y la regla de la categoría de origen dice qué identificador emitir |
| V5 | Un destino ya generado bajo la versión anterior, con identificadores repetidos entre proyectos de código | La migración normativa lo detecta como documento potencialmente invalidado, y no como coincidencia casual |

El caso V3 es el que decide si la corrección sirve: es el escenario real de una generación en paralelo, y es el que hoy falla con certeza y no por azar.

---

## 9. Anexo — evidencia reproducible

Los comandos se ejecutan desde la raíz del workspace que contiene `IA/IA.SDD` y `Repos-RPIs/RPI.VidelControl`.

```bash
# D3: la forma del identificador, declarada
grep -n "D3" IA/IA.SDD/README.md

# El ámbito de unicidad: sin declarar en ninguna regla ni en los dos orquestadores
grep -rniE "identificador(es)? (único|únicos)|único en (el|la) (producto|proyecto)|unicidad" \
  IA/IA.SDD/SDD/Devs/Rules/*.md IA/IA.SDD/SDD/Devs/Orchestrator/*.md

# Dónde sí se valida colisión, y de qué
grep -rn "colisión" IA/IA.SDD/SDD/Devs/Rules/Intake-Rules.md \
  IA/IA.SDD/SDD/Devs/Orchestrator/Master-Prompt.md

# La contradicción de niveles entre las dos categorías
grep -n "Nivel de aplicación" IA/IA.SDD/SDD/Devs/Rules/Rules-Necesidades-Negocio.md \
  IA/IA.SDD/SDD/Devs/Rules/Rules-Especificacion-Funcional.md

# La tabla tipo que cita el caso de uso sin proyecto de código
sed -n '/Tabla C: Trazabilidad a CU/,/^$/p' IA/IA.SDD/SDD/Devs/Rules/Rules-Necesidades-Negocio.md

# Los códigos de error: exigidos, no regulados
sed -n '160p' IA/IA.SDD/SDD/Devs/Rules/Rules-Especificacion-Funcional.md
grep -rc "código de error" IA/IA.SDD/SDD/Devs/Rules/*.md | grep -v ":0"

# El anti-patrón que llega después de la decisión
sed -n '260p' IA/IA.SDD/SDD/Devs/Rules/Rules-Especificacion-Funcional.md

# El control de duplicación, acotado al cierre de fase
sed -n '714p' IA/IA.SDD/SDD/Devs/Orchestrator/Master-Prompt.md

# Cronología: D3 viene del bootstrap; la composición multi-proyecto llega después
grep -n "D1 a D8 vienen" IA/IA.SDD/README.md
grep -n "1.0 | 2026-05-17" IA/IA.SDD/SDD/Devs/Rules/Root-Rules.md
grep -n "2026-06-09" IA/IA.SDD/SDD/Devs/Rules/Root-Rules.md

# Los incidentes, en el destino
grep -n "H-01\|H-02\|H-03\|H-07" Repos-RPIs/RPI.VidelControl/SDD/Docs/Audit/Audit-Fase-B-03-UX-UI-DX.md
grep -n "H-01" Repos-RPIs/RPI.VidelControl/SDD/Docs/Audit/Audit-Fase-B-02-Especificacion-Funcional.md

# La convención que el orquestador tuvo que inventar y declarar él mismo
sed -n '/Numeración de identificadores/,/Control de cambios/p' \
  Repos-RPIs/RPI.VidelControl/SDD/Docs/Proyectos/VideoControl-Domain/02-Especificacion-Funcional/Especificacion-Funcional.md
```

## Control de cambios

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 2026-08-10 | Reporte inicial. Un hueco normativo con tres facetas —ámbito de unicidad sin declarar, familia de identificadores sin forma declarada, y control de duplicación ubicado al cierre de fase—, verificado con cinco incidentes de la Fase B del destino `RPI.VidelControl` bajo el framework 6.0, con su evidencia reproducible y su propuesta de intervención. |
| 1.1 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** El ámbito de unicidad de los identificadores, que no estaba declarado.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `README.md` D3 y `Root-Rules.md` §9 | El ámbito de unicidad pasa a ser el **producto** y no la categoría, con su sistema de identificadores completo |
| `Master-Prompt.md` §3.4 | El **mapa de rangos** se deriva y se publica antes de despachar la primera categoría, porque varios subagentes numeran en paralelo |
| `Rules-Especificacion-Funcional.md` §3.2 | Códigos de error y titularidad del identificador |

**Después de la 7.0.** La unicidad **dentro de la familia** —que exista un `NB-00014` no vuelve ambiguo a un `CU-00014`— se precisó en la **8.2**, cuando apareció un destino que había acuñado su propia familia con **166 ocurrencias**.

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
