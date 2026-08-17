# G1 — Un atributo fijado sin declarar de qué depende

**Documento:** 10-G1-Atributos-Sin-Dependencia-Declarada.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Reportes de la familia:** 01, 05, 06, 08
**Estado:** Vigente

---

## 1. Dónde se produce el fallo

Los cuatro reportes muestran el mismo movimiento: el framework fija la cara visible de una propiedad
y omite la variable que la gobierna. Con un solo proyecto de código las dos coinciden y la omisión no
se nota; con varios, la variable se separa de la cara visible y la regla deja de decidir lo que
tenía que decidir.

| Reporte | Atributo fijado | Variable que lo gobierna y no está declarada | Dónde, verificado sobre el árbol vigente |
|---|---|---|---|
| 01 | La **forma** del identificador: prefijo y dos dígitos | Su **ámbito de unicidad** | `README.md` D3 declara la forma. `grep -rniE "unicidad\|identificador(es)? únicos?"` sobre `Rules/` y `Orchestrator/` devuelve cero coincidencias |
| 05 | El **ancho** del identificador: dos dígitos | La **capacidad**: de qué colección es función el ancho, y qué hacer al agotarse | `Deriva-Rules.md` §2.1 línea 82 y `Rules-Documentacion.md` §… línea 392 fijan dos dígitos «como el resto de los identificadores del template»; ninguna declara el techo |
| 06 | La **obligatoriedad** de un artefacto: por `tipo_proyecto_codigo` | La **responsabilidad**: si este proyecto persiste, si este proyecto se distribuye | `Rules-Arquitectura-Tecnica.md` §2.1 línea 70, §2.2 línea 86, §6 línea 340 y §7 línea 465. `Rules-Examples.md` §0, §2.1 y §2.2 |
| 08 | El **nivel de aplicación**: por categoría | El **referente**: de qué habla cada artefacto | `Rules-Plan-Sprint.md` línea 4 declara «Proyecto de código» para una categoría que emite `Velocidad-Equipo.md` |

La causa común está enunciada en el propio reporte 05 §5, y es la que ordena la corrección:

> allá el framework declaraba la **forma** del identificador y no su **ámbito de unicidad**; acá declara
> la forma y no su **capacidad**. Son dos atributos distintos del mismo sistema, y en los dos casos el
> framework fijó lo visible y omitió lo que gobierna.

## 2. Por qué la normativa vigente no lo atrapa

**Sobre el ámbito (01).** El framework sí valida colisiones, y de otra cosa: `Intake-Rules.md` §4
línea 90 y `Master-Prompt.md` §3.1 línea 179 exigen que «no hay colisión de `Nombre-Proyecto-Codigo`
ni de `Identidad-Codigo`». Se ocupa explícitamente de que dos proyectos de código no colisionen en su
nombre y no dice nada sobre los identificadores de los documentos que esos proyectos producen. El
único indicio disponible apunta al lado equivocado: el anti-patrón «numeración no contigua de CU sin
justificación» de `Rules-Especificacion-Funcional.md` obliga a justificar el salto de rango, y llega
después de la decisión de repartir rangos, no antes.

**Sobre la familia sin forma (01, hueco B).** `Rules-Especificacion-Funcional.md` §4.2 punto 6, línea
160, dice literalmente: «Excepciones y errores. Cada error con código, causa y respuesta del
sistema». Exige que haya un código y no declara su forma, su prefijo, su ámbito ni quién lo asigna.
La expresión «código de error» aparece hoy en un solo archivo de reglas —`Rules-Calidad-Y-Pruebas.md`—
y en ninguno con regla de nomenclatura.

**Sobre la capacidad (05).** `Deriva-Rules.md` §2.1 fija dos dígitos y, en la misma oración, la regla
de estabilidad: «un elemento que se elimina no libera su número; su fila queda con estado
`Retirado`». Las dos reglas son buenas y se agravan mutuamente —el rango se dimensiona por el total
histórico y no por el vigente— y en ningún lado están enunciadas juntas. Y la tabla que el framework
define como derivada de todas las otras, la matriz de sensado de §2.3, es la que con más seguridad
desborda: su tamaño es la suma de las demás.

**Sobre la obligatoriedad (06).** Las tres menciones del mismo artefacto no dicen lo mismo, y es
verificable en una sola pasada:

```bash
grep -n "Modelo-Datos-Logico\|Modelo lógico" IA/SDD/IA.SDD/SDD/Devs/Rules/Rules-Arquitectura-Tecnica.md
#  70 §2.1 tabla maestra: «web-monolith, web-microservices, rest-api, worker-service,
#                          mobile-app-maui con almacenamiento» — el calificativo cuelga del último
#  86 §2.2 tabla por tipo: web-monolith → «Sí», incondicional
# 340 §6 criterio: «Si el tipo D8 exige persistencia…» — condiciona sobre el tipo, no sobre el proyecto
# 465 §7 prompt-snippet: el mismo texto de §6
```

La condición de §6 parece contemplar el caso y no lo contempla: `web-monolith` **como tipo** sí exige
persistencia; lo que no persiste es *ese* proyecto. Y la tabla que el orquestador ejecuta para filtrar
documentos es §2.1, por instrucción de `Master-Prompt.md` §8, que es la incondicional.

**Sobre el nivel (08).** `Vocabulario-Rules.md` §4 R3 obliga a declarar el nivel de aplicación en la
cabecera de la regla, y esa obligación es correcta: es lo que hace visible el problema. Lo que el
framework no dice es que dentro de una categoría puede haber artefactos de dos niveles. La cabecera
declara **un** nivel para toda la regla.

## 3. El dato decisivo: el mecanismo correcto ya existe y no se generalizó

Es lo que hace a esta familia barata de corregir, y está verificado sobre el árbol vigente.
`Master-Prompt.md` §4 declara una tabla de flags por proyecto de código, y entre ellos:

| Flag vigente | Ámbito | Qué pregunta | Qué decide hoy |
|---|---|---|---|
| `tiene_persistencia` | proyecto de código | «true si declara cualquier motor de persistencia distinto a "No aplica"» | Activa `modelo-conceptual` en 02 y `Modelo-Datos-logico` en 05 **del proyecto de código** |
| `requiere_maqueta` | proyecto de código | Capacidad del proyecto, confirmada por el humano | Ejecuta o no la Fase B2 |
| `equipo_n` | **producto** | Cantidad de personas del equipo | Decide la forma de la categoría 07 |

Tres consecuencias directas:

1. **El caso del reporte 06 ya tiene su flag.** `tiene_persistencia` es por proyecto de código y su
   impacto declarado en `Master-Prompt.md` §4 es exactamente activar el modelo lógico en la 05 de ese
   proyecto. La regla de la categoría no lo lee: condiciona sobre el tipo. El orquestador y la regla
   dicen cosas distintas sobre la misma obligación, y no hace falta inventar un flag para
   reconciliarlas.
2. **El caso de `Rules-Examples.md` tiene su campo en el manifiesto.** `redistribuible` está declarado
   por proyecto de código en §13 del intake y se copia al manifiesto (`Intake-Rules.md` §4, tabla de
   mapeo). Es exactamente la pregunta de la que depende la obligación de la categoría 10 —si hay o no
   un integrador que consuma el artefacto por gestor de paquetes— y la regla no lo lee.
3. **El caso del reporte 08 tiene su prueba en la propia tabla.** `equipo_n` es el único flag de
   ámbito **producto** y es el que decide la forma de la categoría 07, declarada a nivel proyecto de
   código. La señal que el reporte 08 §6 enuncia —«que la propia regla condicione su forma a un
   atributo del nivel superior»— es verificable en la tabla de flags del orquestador.

## 4. Correcciones propuestas

### G1-A · Sistema de identificadores como sección transversal

**Archivo:** `Root-Rules.md`, sección nueva **§10 Sistema de identificadores**.
**Severidad:** minor sobre `Root-Rules.md`.

Se declara en un solo lugar lo que hoy está repartido entre D3 y cuatro reglas de categoría:

1. **Ámbito de unicidad.** Todo identificador declara en qué ámbito es único. Cuando el producto
   tiene más de un proyecto de código, el ámbito por defecto es el **producto**, porque es el que
   hace resolver, sin cambiarlas, las citas por identificador desnudo que ya existen en la
   normativa: la tabla de trazabilidad a casos de uso de `Rules-Necesidades-Negocio.md` §4.4 cita
   `CU-01` sin columna de proyecto de código, desde un artefacto de nivel producto.
2. **Ancho como función de la colección.** El ancho se fija al abrir la tabla, se declara en la línea
   que la encabeza y no se cambia después. Dos dígitos hasta noventa y nueve elementos previstos,
   tres a partir de ahí. El ancho es propiedad **de la familia**, no del framework: `SUP-01` y
   `EST-001` conviven legítimamente si cada tabla declara el suyo.
3. **Regla de agotamiento.** Si un rango se agota pese al dimensionamiento, la salida del método es
   ampliar el ancho de esa familia y declarar la ampliación en la tabla, migrando sus filas. No se
   fragmenta el identificador ni se comprime el inventario. Es la salida A del reporte 05 §7, elegida
   una vez por el método y no por cada agente.
4. **Colecciones derivadas.** Una colección que se construye a partir de otras —la matriz de sensado
   de `Deriva-Rules.md` §2.3 es el caso canónico— dimensiona su ancho sobre la **suma** de sus
   fuentes y no sobre su propio conteo inicial.
5. **Estabilidad y capacidad, enunciadas juntas.** Como un identificador retirado no libera su
   número, el rango se dimensiona por el total histórico previsto y no por el vigente.
6. **Titularidad.** Toda categoría que acuñe un identificador declara su prefijo, su forma y su
   ámbito en §3.2 de su regla. Una categoría no acuña identificadores para artefactos de otra: si los
   necesita, los escala, y la regla de la categoría de origen dice qué identificador emitir.

**Lo que queda pendiente de decisión.** Los puntos 1 y 2 tocan la invariante **D3** —y el 2 roza
D4—, y `README.md` §Reglas de intervención declara que modificar una invariante «requiere decisión
explícita del responsable y nota de coherencia». La sección §10 de `Root-Rules.md` se puede escribir
sin tocar D3; que D3 remita a ella es la parte que exige la decisión. Ver §6.

### G1-B · Los códigos de error tienen forma y ámbito

**Archivo:** `Rules-Especificacion-Funcional.md` §3.2, con su criterio en §6.
**Severidad:** minor.

Los códigos de error son la familia que produjo tres de los cinco incidentes del reporte 01, y hoy
se exigen sin regularse. Se declara: prefijo `E-<DOMINIO>-XX`, ámbito de unicidad el del punto 1 de
G1-A, y asignación por la categoría 02 del proyecto de código dueño del error. La colisión no era un
descuido: con cinco agentes en paralelo sobre el mismo dominio, los prefijos naturales coinciden con
certeza y no por azar (reporte 01 §4.2).

### G1-C · La obligatoriedad se condiciona sobre el proyecto, no sobre el tipo

**Archivos:** `Rules-Arquitectura-Tecnica.md` §2.1, §2.2, §6 y §7; `Rules-Examples.md` §0, §2.1, §2.2
y §6.
**Severidad:** minor en las dos. No cambia el conjunto de artefactos de la categoría; cambia la
condición bajo la cual uno de ellos es obligatorio, y la condición nueva es **más** permisiva que la
vigente, de modo que ninguna documentación ya emitida deja de cumplir.

1. Las cuatro menciones del modelo lógico pasan a decir lo mismo, y lo que dicen es «si
   `tiene_persistencia` es true **para este proyecto de código**», que es el flag que
   `Master-Prompt.md` §4 ya deriva y cuyo impacto declarado ya es ése.
2. La categoría 10 condiciona su obligatoriedad sobre `redistribuible` del manifiesto, y su piso de
   tres samples pasa a ser piso de uno con la progresión completa exigida solo cuando
   `redistribuible` es true. La cláusula de omisión que hoy vive en la prosa de §0 sube a las tablas
   §2.1 y §2.2, que son las que el orquestador ejecuta.

### G1-D · Figura de apartamiento declarado

**Archivos:** `Root-Rules.md` §10 (o sección propia), criterios de audit de `Master-Prompt.md` §10.
**Severidad:** minor.

Un artefacto declarado obligatorio puede no emitirse si existe un ADR que declare el apartamiento,
con sus alternativas descartadas y sus disparadores de revisión. Es lo que el destino tuvo que
inventar —`ADR-07` del reporte 06 §7— y lo que `Rules-Documentacion.md` §2.5 ya admite para su
propio gating, según el hallazgo P0 de `Master-Prompt.md` §10: «un artefacto declarado obligatorio
por el gating de `Rules-Documentacion.md` §2.5 está ausente **sin ADR que lo justifique**». La figura
existe en una regla y no está generalizada; esto la generaliza.

### G1-E · El nivel se declara por artefacto

**Archivos:** `Vocabulario-Rules.md` §4 R3; la tabla maestra §2.1 de las doce reglas de categoría;
`Rules-Plan-Sprint.md` §2.1, §2.2, §4.2, §4.6 y §6.
**Severidad:** minor en las once reglas donde la columna repite el mismo valor; **major** en
`Rules-Plan-Sprint.md`, porque cambia dónde viven artefactos que ya se emitieron y la documentación
generada bajo la versión anterior deja de cumplir.

1. R3 pasa a exigir el nivel **por artefacto**, en una columna nueva de la tabla maestra §2.1 de cada
   regla. En once categorías la columna repite el nivel de la cabecera; la cabecera se conserva como
   el nivel predominante de la categoría.
2. En la categoría 07, `Velocidad-Equipo.md`, la declaración de capacidad y las plantillas de
   ceremonia pasan a nivel **producto**, en `SDD/Docs/Producto/07-Plan-Sprint/`. El plan por proyecto
   de código deja de repetirlos y los referencia. La evidencia de que hoy se replican sin aportar es
   del propio reporte 08 §10: descontada la cabecera, las cinco plantillas de retrospectiva del
   destino son byte a byte el mismo documento.
3. La numeración de iteraciones es del **producto** y la fija el roadmap de la categoría 00. El
   criterio «mínimo Sprint 0 y Sprint 1» pasa a «la iteración de arranque y la primera con trabajo
   comprometido en este proyecto de código», que es lo que quería decir y lo que hace satisfacible el
   criterio en un proyecto cuya primera iteración con capacidad es la cuarta del producto.
4. Mientras un documento de velocidad siga emitiéndose por proyecto de código —caso de un destino ya
   generado que no migra—, lleva una sección obligatoria que declara qué mide y qué no: es la
   sección §5 que el destino inventó (reporte 08 §7.4) y la intervención más barata de las cinco que
   ese reporte propone.

## 5. Cómo se verifica

Los casos vienen de las secciones «cómo verificar» de los cuatro reportes, con su criterio negativo
cuando lo tienen.

| Caso | Entrada | Resultado esperado | Origen |
|---|---|---|---|
| V1 | Un producto de un solo proyecto de código | Nada cambia: es el caso degenerado y las dos lecturas del ámbito coinciden | 01 V1 |
| V2 | Un producto de tres o más proyectos, antes de despachar la primera categoría por proyecto | El orquestador publica el mapa de rangos junto al bloque informativo de §3.4, y cada despacho lo lleva | 01 V2 |
| V3 | Dos subagentes generando en paralelo la categoría 02 de dos proyectos, cada uno con un caso de uso sobre el mismo concepto | Los códigos de error no colisionan, sin que ninguno haya consultado al otro | 01 V3, el caso que decide |
| V4 | Una categoría que necesita citar un elemento de otra categoría que no tiene identificador | Lo escala en lugar de acuñarlo | 01 V4 |
| V5 | Una línea de base con más de cien estados | El ancho está declarado en la tabla, no inferido por el agente | 05.1 |
| V6 | La misma línea de base generada dos veces por agentes distintos | Los dos eligen el mismo ancho | 05.3 |
| V7 | La matriz de sensado de un proyecto con cinco tablas fuente | Dimensiona su ancho sobre la suma de las cinco | 05.4 |
| V8 | La 05 de un producto multiproyecto donde la persistencia vive en otro proyecto | Ninguna de las dos salidas malas —copiar el modelo o emitir un documento que remite— es necesaria | 06.1 |
| V9 | Cambiar el flag `tiene_persistencia` de un proyecto | La lista de artefactos obligatorios de ese proyecto cambia | 06.2 |
| V10 | Un artefacto obligatorio ausente **con** su ADR de apartamiento | El audit lo evalúa como decisión y no como omisión | 06.3 |
| V11 | Las tres menciones de cada artefacto obligatorio en cada regla | Dicen lo mismo | 06.4 |
| V12 | Dos planes de iteración del mismo número, de proyectos distintos | Identifican la misma ventana de calendario | 08 |
| V13 | Un proyecto cuya primera iteración con trabajo es la cuarta del producto | El criterio de planes mínimos se satisface | 08 |

## 6. Qué queda fuera de esta corrección, y por qué

**La elección del ámbito de unicidad y la reformulación de D3 y D4.** El reporte 01 §7 declara
explícitamente que no toma la decisión: «es decisión del responsable del framework. Lo que sí puede
afirmarse con evidencia es que la elección existe, hoy no está declarada, y las dos partes citadas
exigen lecturas distintas». `README.md` §Reglas de intervención clasifica tocar una invariante como
«el cambio de mayor impacto del framework: alcanza a los dieciocho archivos de reglas, a los dos
orquestadores y a toda la documentación ya emitida. Requiere decisión explícita del responsable y
nota de coherencia».

Por eso G1-A se propone en dos piezas separables:

| Pieza | Qué hace | Severidad | Requiere decisión |
|---|---|---|---|
| `Root-Rules.md` §10 completa | Declara ámbito, ancho, agotamiento, colecciones derivadas y titularidad | minor | No |
| D3 y D4 remiten a §10 | Hace que un agente que solo lee `README.md` sepa que el ámbito y el ancho están declarados | major del conjunto | **Sí** |

Aplicar la primera sin la segunda deja el hueco cerrado para todo agente que reciba `Root-Rules.md`
en su despacho, y abierto para el que solo tenga el `README.md` en contexto. Es una corrección real
y parcial, y así hay que declararla.

**La renumeración de destinos ya generados.** Fuera de alcance: la corrige la migración normativa,
y el caso V5 del reporte 01 pide precisamente que la migración detecte los identificadores repetidos
entre proyectos como documento potencialmente invalidado, y no como coincidencia casual.
