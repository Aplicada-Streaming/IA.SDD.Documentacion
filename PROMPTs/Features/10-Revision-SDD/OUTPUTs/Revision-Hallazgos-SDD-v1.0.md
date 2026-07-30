# Revisión de los hallazgos sobre el Framework SDD

**Documento:** `Revision-Hallazgos-SDD-v1.0.md`
**Versión:** 2.0
**Estado:** Vigente
**Fecha:** 2026-07-28
**Autor:** Revisión SDD (Claude Code)
**Insumo revisado:** `INPUTs/Evaluacion-SDD.md` v1.0
**Repositorio intervenido:** `/IA/IA.SDD/`, `Master-Prompt.md` v3.6 → **v3.7**, changelog del framework **[3.2]**
**Anexos:** `Anexo-Barrido-Artefactos-Sin-Version-v1.0.md`, `Alternativa-Eliminar-Legacy-v1.0.md`, `Nota-Coherencia-Reparacion-Legacy-v1.0.md`

> **Las ocho reparaciones de §6 están aplicadas.** La versión 1.0 de este documento las proponía sin aplicar; esta versión registra la intervención y su verificación. El detalle por archivo está en §0 y la verificación de implantación en la nota de coherencia.

> **Sobre la alternativa de fondo.** `Alternativa-Eliminar-Legacy-v1.0.md` evalúa eliminar el concepto de `_legacy/` en lugar de repararlo, y recomienda esa vía. **El responsable del framework eligió la Opción A**, reparar y conservar, que es lo que se aplicó. La alternativa queda registrada como evaluación disponible si en el futuro se decide declarar el control de versiones como prerrequisito duro (DEC-04 de ese documento).

---

## Tabla de contenido

- [§0 Estado de aplicación](#0-estado-de-aplicación)
- [§1 Qué hizo esta revisión y qué no](#1-qué-hizo-esta-revisión-y-qué-no)
- [§2 Veredicto sobre los cinco hallazgos](#2-veredicto-sobre-los-cinco-hallazgos)
- [§3 Verificación hallazgo por hallazgo](#3-verificación-hallazgo-por-hallazgo)
- [§4 Hallazgos nuevos](#4-hallazgos-nuevos)
- [§5 Evaluación de los cinco arreglos propuestos](#5-evaluación-de-los-cinco-arreglos-propuestos)
- [§6 Reparaciones propuestas](#6-reparaciones-propuestas)
- [§7 Impacto de las reparaciones](#7-impacto-de-las-reparaciones)
- [§8 Orden de aplicación y conjunto mínimo eficaz](#8-orden-de-aplicación-y-conjunto-mínimo-eficaz)
- [§9 Verificaciones](#9-verificaciones)
- [§10 Decisiones que requieren al responsable del framework](#10-decisiones-que-requieren-al-responsable-del-framework)
- [§11 Observaciones menores](#11-observaciones-menores)

---

## §0 Estado de aplicación

Las ocho reparaciones se aplicaron el 2026-07-28 sobre `/IA/IA.SDD/`, en el orden de dependencia de §8. La intervención toca ocho archivos, con 90 líneas agregadas y 16 eliminadas.

### §0.1 Qué se aplicó

| Reparación | Resuelve | Dónde quedó |
|---|---|---|
| R-7 | H-9 | `Master-Prompt.md` §3.5: el layout declara `SDD/Docs/Audit/` como carpeta fija y explica que `_legacy/` puede ser hija de cualquier carpeta con artefactos versionados, creada al primer archivado |
| R-2 | H-2, H-6 | `Master-Prompt.md` §5 y §5.1: ruta única `<carpeta-del-artefacto>/_legacy/<YYYY-MM-DD>/`, con la lectura de las abreviaturas de las diez reglas y la distinción del caso de `SDD/Docs/_legacy/` del prerrequisito 4 |
| R-1 | H-1 | `Master-Prompt.md` §5.1 con la regla general y la tabla de cinco exenciones, más puntero en las cinco reglas que emiten artefactos sin sufijo |
| R-3 | H-3 | `Master-Prompt.md` §8: sección «Estado previo del entregable» en el esqueleto, más cuatro reglas de construcción que asignan el snapshot al orquestador y exceptúan las Fases I y J |
| R-4 | H-4 | `Master-Prompt.md` §5: la política de deprecación incorpora estado `Superado` y nota a la vigente, en bloque antepuesto que no modifica el cuerpo del snapshot |
| R-6 | H-7 | `Master-Prompt.md` §10: eje de ronda `-r<N>` en el path del informe, en la línea de política y en el esqueleto de despacho del auditor |
| R-5 | H-5 | `Master-Prompt.md` §5: política de versionado con el criterio de estado de cabecera |
| R-8 | H-8 | `Master-Prompt.md` §5.1 (exención de `evidencia` de `VER-XX`) y §7.2 (versionado por corte de cadencia y exención del snapshot previo) |

### §0.2 Archivos intervenidos

| Archivo | Antes | Después |
|---|---|---|
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 3.6 | **3.7** |
| `SDD/Devs/Rules/Root-Rules.md` | 1.4 | **1.5** |
| `SDD/Devs/Rules/Rules-Contexto.md` | 1.5 | **1.6** |
| `SDD/Devs/Rules/Rules-Necesidades-Negocio.md` | 1.4 | **1.5** |
| `SDD/Devs/Rules/Rules-Examples.md` | 2.0 | **2.1** |
| `SDD/Devs/Rules/Rules-Documentacion.md` | 2.0 | **2.1** |
| `SDD/Guides/SDD-Development-Guide.md` | — | errata, sin bump |
| `CHANGELOG.md` | [3.1] | **[3.2]** |

**No se modificó ninguna invariante D1-D9.** `README.md` línea 104, donde vive D5, quedó intacto, según lo decidido en DEC-03. La intervención precisa la política operativa de §5, no el enunciado del principio.

### §0.3 Erratas preexistentes corregidas de paso

Dos tablas markdown estaban partidas por una línea en blanco que las rompía como tabla:

| Errata | Dónde | Estado |
|---|---|---|
| La fila D9 estaba fuera de la tabla de invariantes de solución | `Master-Prompt.md` §5 | Corregida |
| La fila 3.4 estaba fuera de la tabla de control de cambios | `Master-Prompt.md` §16 (era O-2) | Corregida |
| Referencia a `SDD/Devs/Intake/_legacy/`, carpeta eliminada en el changelog [3.1] | `SDD-Development-Guide.md` §2 | Corregida |

### §0.4 Lo que quedó sin hacer, y por qué

| Punto | Motivo |
|---|---|
| O-1, el «trece archivos de reglas» del changelog del master-prompt | Vive dentro de una fila histórica de control de cambios. Reescribir un registro pasado falsearía lo que esa intervención dijo en su momento |
| O-3, el README de proyecto sin regla que lo gobierne | Excede el alcance de la política de archivado. Requiere decidir qué categoría lo adopta, que es una decisión de diseño distinta |
| Las diez reglas que escriben `_legacy/` a secas | No se tocaron a propósito: §5.1 declara que se leen como abreviatura de la ruta canónica, con lo cual dejan de contradecirla sin necesidad de once ediciones |

---

## §1 Qué hizo esta revisión y qué no

**Qué hizo.** Contrastar cada uno de los cinco hallazgos y cada uno de los cinco arreglos de `Evaluacion-SDD.md` contra el estado actual del framework, releyendo en su contexto cada archivo y línea que la evaluación cita, y ejecutando las dos verificaciones que la evaluación dejó pendientes y son estáticas: el barrido V-1 (anexo aparte) y el contraste de los hallazgos contra las Fases I y J, que la evaluación declaró no ejercitadas en su §1.4.

**Qué no hizo.** No ejecutó una corrida del orquestador. La verificación es documental: lectura del framework con citas de archivo y línea, reproducibles con `grep`. Eso alcanza para confirmar o refutar defectos de especificación, que es de lo que tratan los cinco hallazgos, y no alcanza para descubrir defectos que solo se manifiestan en ejecución.

**Resultado de conjunto.** Los cinco hallazgos se confirman. Dos de ellos están subdimensionados en la evaluación de origen. De los cinco arreglos propuestos, dos se adoptan con precisiones, dos se adoptan cambiando su formulación y uno se reemplaza. Aparecen cuatro hallazgos nuevos, uno de ellos de severidad alta y del mismo mecanismo silencioso que H-1.

**Sobre la prioridad declarada en la evaluación.** `Evaluacion-SDD.md` §5 abre diciendo que A-1 «es el único que resuelve una pérdida real y se puede aplicar solo». Es inexacto en su segunda mitad: A-1 corrige el **nombre** del snapshot, y tres de las cinco pérdidas del inventario de §7 se produjeron porque **no se tomó ningún snapshot**, que es H-3. A-1 aplicado solo habría evitado las pérdidas 3 y 4, no las 1 ni la 2. El conjunto mínimo eficaz es {A-1, A-3} junto, y así se ordena en §8.

---

## §2 Veredicto sobre los cinco hallazgos

| # | Hallazgo | Veredicto | Ajuste respecto de la evaluación |
|---|---|---|---|
| H-1 | El `README.md` de sección es imposible de archivar sin colisión | **Confirmado** | Subdimensionado: son nueve clases de artefacto sin versión, no dos. Ver anexo |
| H-2 | Dos convenciones de ruta de archivado que no coinciden | **Confirmado** | Subdimensionado: son cuatro convenciones, no dos, y la causa raíz es que el layout canónico no declara `_legacy/` (H-8) |
| H-3 | Las reglas describen un flujo de edición distinto del que el master-prompt produce | **Confirmado** | Precisión: el defecto es del **momento y del actor** del snapshot, no de que el subagente ignore la política. La ruta sí le llega |
| H-4 | Requisitos de archivado que viven solo en las reglas y no llegan a los subagentes | **Confirmado con matiz** | El enunciado «no llegan a los subagentes» es correcto para los dos requisitos, no para la política entera |
| H-5 | El framework no cubre la cadencia de corrección dentro del ciclo de auditoría | **Confirmado** | Se resuelve la pregunta abierta sobre Fases I y J: ahí el framework **sí** tiene modelo, y es incompatible con el criterio de «consumido» |

---

## §3 Verificación hallazgo por hallazgo

### §3.1 H-1 — Confirmado, con alcance mayor

Las tres evidencias se verifican en el estado actual: `Rules-Contexto.md` línea 90, `Rules-Necesidades-Negocio.md` línea 103 y `Master-Prompt.md` línea 267 dicen literalmente lo que la evaluación cita.

La evaluación acierta también en su propia advertencia: «el defecto no es del README en particular sino de todo artefacto que el framework mande sin versión en el nombre». El barrido del anexo lo confirma y lo cuantifica: **nueve clases**, de las cuales seis deben archivarse con sufijo y tres deben quedar exentas con la razón declarada. Entre las que la evaluación no vio está `SDD/Docs/README.md`, el README raíz de la salida (`Root-Rules.md` líneas 69 y 358), que es el punto de entrada de todo el árbol y por lo tanto el que más veces se reescribe.

**Un matiz sobre el mecanismo.** La evaluación atribuye la colisión al eje de fecha. Es más general que eso: la colisión ocurre siempre que dos snapshots del mismo artefacto caen en la misma carpeta, y con `_legacy/` a secas —la forma de las diez reglas— la colisión no requiere ni siquiera que sean del mismo día. El eje de fecha no causa el defecto: lo acota a un día.

### §3.2 H-2 — Confirmado, y son cuatro convenciones

Verificado, y con una convención más de las que la evaluación reporta. Las formas en uso hoy son:

| Forma | Dónde |
|---|---|
| `_legacy/<categoria>/<fecha>/` | `Master-Prompt.md` línea 267 (§5, política de deprecación) |
| `SDD/Docs/_legacy/<fecha>/` | `Master-Prompt.md` línea 30 y `PROMPT-Agente-Bootstrap-SDD.md` línea 65 |
| `SDD/Intake/_legacy/<YYYY-MM-DD>/` | `Master-Prompt.md` línea 704 (§13.6) |
| `_legacy/` a secas, relativo a la carpeta del artefacto | Diez reglas de categoría. `Rules-Especificacion-Funcional.md` línea 115 la usa con carpeta explícita: `Casos-De-Uso/_legacy/` |
| `_legacy/<fecha>/` relativo a la carpeta del artefacto | `Index-Modelos-UX-UI.md` línea 47 |

La forma de `Index-Modelos-UX-UI.md` es la que ninguna de las dos partes en conflicto declara, y es —según §6— la correcta.

**Corrección a la recomendación de A-2.** La evaluación recomienda conservar el eje de `Master-Prompt.md` §5 y alinear las diez reglas a él. Esa recomendación debe rechazarse: `_legacy/<categoria>/<fecha>/` es defectuoso por una razón que la evaluación no detectó, y que se registra como hallazgo nuevo H-6 en §4.1.

### §3.3 H-3 — Confirmado, con una precisión sobre el mecanismo

El esqueleto de despacho de `Master-Prompt.md` §8 (líneas 452-527) se leyó completo. Sus secciones son exactamente las que la evaluación enumera, y **ninguna** menciona el archivado, el estado previo del entregable ni la distinción entre emitir y corregir. Confirmado.

La precisión: la evaluación dice que el esqueleto «no dice nada» sobre el archivado. Es cierto para el momento y para el actor, y no lo es para la política. El esqueleto inyecta `{{BLOQUE_INVARIANTES_DE_SECCION_5}}` (línea 470), y §5 contiene la fila «Política de deprecación» con su ruta. El subagente **sí recibe** dónde archivar; lo que no recibe es que tiene que hacerlo antes de su primera edición, ni si le toca a él hacerlo. Ese matiz importa porque cambia el arreglo: no hay que agregar la política al despacho, sino el momento y el responsable.

**Contraste contra las Fases I y J, que la evaluación no pudo hacer.** `Master-Prompt.md` §7.2 (líneas 431-447) declara un modelo distinto para la re-ejecución de la Fase I: los documentos afectados por el incremento «se actualizan al estado real del sistema, con `last_review` nuevo», el campo `evidencia` de los contratos `VER-XX` «se sobrescribe» y «la evidencia anterior no se conserva», y las correcciones manuales del usuario no se pisan. Es un modelo de estado y fecha de revisión, no de snapshot por edición. Una instrucción de snapshot incondicional en el despacho, como la que propone A-3, entra en conflicto directo con esa sección: la Fase I se re-ejecuta una vez por incremento y produciría un `_legacy/` que crece con cada corte sin lector que lo consuma. El arreglo tiene que declarar su alcance por fase.

### §3.4 H-4 — Confirmado

`Master-Prompt.md` línea 267 declara la ruta y nada más. Las diez reglas exigen además estado `Superado` y nota al inicio apuntando a la versión vigente (`Rules-UX-UI-DX.md` línea 180, `Rules-Especificacion-Funcional.md` línea 115, `Rules-Prompts-AI.md` línea 121, entre otras). Los dos requisitos no están en §5 y por lo tanto no viajan en el bloque de invariantes.

Vale agregar que `Superado` pertenece al enum cerrado de estados del framework (`Root-Rules.md` línea 110 y `Rules-Necesidades-Negocio.md` línea 128), así que el requisito es coherente con el modelo de estados y no introduce vocabulario nuevo. La severidad baja que la evaluación le asigna es correcta.

**Una objeción de diseño al arreglo, no al hallazgo.** Poner estado `Superado` en el archivo archivado implica **editar el snapshot**. Un snapshot editado ya no es el estado previo: es el estado previo con una modificación. Para un archivado cuyo propósito es poder leer qué decía antes, eso es una contradicción menor pero real. Se resuelve en R-4 sin renunciar al requisito.

### §3.5 H-5 — Confirmado, y con la pregunta abierta resuelta

El vacío es real y está bien caracterizado. `Master-Prompt.md` línea 266 escribe la política para cambios espaciados; §7 y §10 (línea 613: «RECHAZADO obliga a corrección y re-audit») garantizan que va a haber rondas de corrección sobre documentos ya emitidos; y nada dice si cada ronda sube versión.

La regla que el orquestador inventó durante la corrida —sube de versión el artefacto ya consumido— es razonable y su rationale es correcto. Tiene, sin embargo, un problema operativo que la evaluación no evalúa: **«consumido» no es verificable desde el documento**. Para saber si un artefacto fue consumido hay que recorrer las cabeceras de trazabilidad de todos los demás artefactos buscando quién lo cita como insumo. Un auditor de §10, que se invoca desde cero y «lee solo los entregables de la fase, los insumos upstream que cita y los archivos de reglas correspondientes», no tiene ese grafo a mano.

El framework ya tiene un criterio equivalente y sí verificable: el **estado en la cabecera**, del enum cerrado `Borrador / Propuesto / Aprobado / Vigente / Superado / Archivado` (`Root-Rules.md` línea 110). El propio auditor de la corrida usó el argumento en esos términos, según cita la evaluación en §3.5: «los documentos en estado `Propuesto` no habían sido leídos por ningún consumidor». El estado es la forma declarada de lo mismo, y se lee en la primera pantalla del archivo. R-5 reformula A-5 sobre ese criterio.

**La pregunta abierta que la evaluación deja en §5.5** —si el criterio es igual de evaluable en las Fases I y J— tiene respuesta, y es que **no**. En el modelo de documentación viva de `Rules-Documentacion.md` §0.3 y §0.4, un documento del Momento 2 se actualiza en cada corte de sprint, es leído por el equipo entre corte y corte, y su estado es `Vigente` desde la primera publicación. Bajo el criterio de «consumido», cada corte subiría minor: quince incrementos producen un `v1.15` con catorce snapshots. Bajo el criterio de estado, lo mismo. Ninguno de los dos criterios sirve ahí, y §7.2 no lo dice. Se registra como hallazgo nuevo H-8 en §4.3.

---

## §4 Hallazgos nuevos

Cuatro hallazgos que la evaluación no reporta, todos verificados con cita.

| # | Hallazgo | Severidad | Familia |
|---|---|---|---|
| H-6 | La ruta `_legacy/<categoria>/<fecha>/` pierde el eje de proyecto y colisiona entre proyectos | Alta | Extiende H-2 |
| H-7 | El informe de auditoría tiene ruta fija `-v1.0` y el re-audit lo sobrescribe | Alta | Mismo mecanismo que H-1 |
| H-8 | La política de versionado no cubre el modelo de documentación viva de las Fases I y J | Media | Extiende H-5 |
| H-9 | El layout canónico no declara `_legacy/` ni `SDD/Docs/Audit/` | Media | Causa raíz de H-2 |

### §4.1 H-6 — La ruta de §5 pierde el eje de proyecto

**Enunciado.** `Master-Prompt.md` línea 267 declara `_legacy/<categoria>/<fecha>/`. La unidad de trabajo del framework es la solución, que agrupa N proyectos, y el layout de §3.5 ubica las categorías 02 a 11 bajo `Proyectos/<Nombre-Proyecto>/`. La ruta de §5 no tiene eje de proyecto.

**Las dos lecturas posibles, ambas defectuosas.**

- Si `_legacy/` cuelga de `SDD/Docs/`, entonces `SDD/Docs/_legacy/05-Arquitectura-Tecnica/2026-07-28/` recibe los snapshots de **todos** los proyectos que archiven esa categoría ese día. Dos proyectos con un `Arquitectura-Solucion-v1.0.md` cada uno colisionan, y el segundo sobrescribe al primero. Es la misma pérdida silenciosa de H-1, agravada porque los nombres de artefacto sí llevan versión y por lo tanto la colisión no se puede prevenir con el arreglo A-1.
- Si `_legacy/` cuelga de la carpeta de la categoría, entonces el segmento `<categoria>` de la ruta es redundante: `Proyectos/X/05-Arquitectura-Tecnica/_legacy/05-Arquitectura-Tecnica/2026-07-28/`.

**Por qué importa.** Eleva H-2 de «ambigüedad latente que no produjo defecto» a «defecto que produce pérdida en cuanto la solución tenga más de un proyecto». La corrida que originó la evaluación tuvo cuatro proyectos, pero solo ejecutó las Fases de validación de intake y A, cuyas dos categorías son de nivel solución y por eso no ejercitaron el eje de proyecto. Es el hallazgo que la evaluación no podía ver por el alcance que ella misma declara en su §1.4.

### §4.2 H-7 — El re-audit sobrescribe el informe de auditoría

**Enunciado.** `Master-Prompt.md` línea 599 fija el path del informe: `SDD/Docs/Audit/<fase>-<categoria>[-<proyecto>]-v1.0.md`. La versión está **hardcodeada en `v1.0`** y no hay eje de ronda. La línea 613 declara que «RECHAZADO obliga a corrección y re-audit». El segundo auditor escribe el mismo path que el primero.

**Evidencia de que la omisión es específica.** La misma línea 599 sí resuelve el caso análogo de la Fase I: «Las corridas repetidas de la Fase I se distinguen por incremento: `SDD/Docs/Audit/I-<incremento>-<categoria>[-<proyecto>]-v1.0.md`». El framework previó la repetición por incremento y no la repetición por rechazo, que es la que su propio ciclo de auditoría garantiza. El esqueleto de despacho del auditor (línea 629) repite el path fijo.

**Por qué es alta.** El informe de audit es el artefacto que documenta **por qué** se hizo cada corrección. La regla que A-5 propone —y que R-5 conserva— exige que cada corrección absorbida deje su fila en el control de cambios «citando el hallazgo que la origina». Si el informe que contiene ese hallazgo fue sobrescrito por el re-audit que lo declaró resuelto, la cita queda colgada: el rastro apunta a un documento que ya no dice lo que se citó. La política de versionado de R-5 depende de que este hallazgo esté corregido.

Es además el mismo mecanismo silencioso de H-1: nadie recibe error, la carpeta `Audit/` se ve poblada y correcta, y un verificador que compruebe «existe el informe de la fase» lo da por bueno.

### §4.3 H-8 — La política de versionado no cubre las Fases I y J

**Enunciado.** `Master-Prompt.md` §7.2 declara qué se preserva y qué se regenera en cada re-ejecución de la Fase I, y no declara si el documento actualizado sube de versión ni si su estado anterior se archiva. `Rules-Documentacion.md` §0.4 fija la cadencia —cierre de sprint, cierre de incremento, o cambio que altera un contrato público— y `Rules-Documentacion.md` línea 400 remite a la regla general: «Al pasar de `v1.0` a `v2.0`, la anterior se archiva en `_legacy/`». La regla general está escrita para el salto mayor y la cadencia produce actualizaciones por corte.

**Evidencia de que el framework ya decidió algo distinto ahí, sin declararlo como excepción.** §7.2 dice del campo `evidencia` de los contratos `VER-XX`: «Se sobrescribe con la salida real de la corrida en curso. La evidencia anterior no se conserva: lo que importa es el estado presente». Es una excepción explícita y razonada a D5, dentro del master-prompt, que no está reflejada en la política de deprecación de §5 ni declarada como excepción de la invariante. Es correcta en el fondo y está en el lugar equivocado.

**Consecuencia.** Los dos criterios candidatos para H-5 —«consumido» y «estado en cabecera»— dan el mismo resultado indeseable en el Momento 2: cada corte sube minor. El framework necesita declarar explícitamente que en el tramo de documentación viva el eje de identidad es `last_review` y el estado, no el sufijo de versión, o bien que el corte de cadencia es el evento de publicación y sube minor una vez por corte. Es una decisión, no un defecto de redacción; se eleva en §10.

### §4.4 H-9 — El layout canónico no declara las carpetas que la política usa

**Enunciado.** `Master-Prompt.md` §3.5 (líneas 174-198) es el layout de salida, y `Root-Rules.md` §2.1 (líneas 43-46) es el inventario de archivos de la raíz de la salida. Ninguno de los dos declara `_legacy/`, en ninguna de sus formas, ni `SDD/Docs/Audit/`, pese a que §5 depende de la primera y §10 escribe en la segunda.

**Consecuencia.** Es la causa raíz de que existan cuatro convenciones de ruta: no hay un lugar canónico donde la carpeta esté declarada, así que cada archivo que la necesita la inventa. Y afecta al audit: §10 exige verificar «filename y estructura de carpetas correctos», y el auditor no tiene contra qué contrastar esas dos carpetas.

Es también el arreglo más barato del conjunto —dos bloques de layout— y el que hace que R-2 sea una consecuencia en lugar de una convención elegida por decreto.

---

## §5 Evaluación de los cinco arreglos propuestos

| Arreglo | Veredicto | Motivo |
|---|---|---|
| A-1 · Sufijo de versión al archivar | **Adoptar cambiando su formulación** → R-1 | El enunciado es correcto. El alcance es de seis archivos y no de dos, faltan tres exenciones, y ubicarlo «en cada §3.4» duplica la cláusula seis veces sin cubrir los casos futuros |
| A-2 · Unificar la ruta de archivado | **Reemplazar** → R-2 | Su recomendación explícita —conservar el eje de §5— consagra una ruta defectuosa (H-6). La convención correcta es la tercera, que ninguna de las dos partes declaraba |
| A-3 · Snapshot previo en el esqueleto de despacho | **Adoptar con dos precisiones** → R-3 | El diagnóstico y la ubicación son correctos. El actor debe ser el orquestador, no el subagente, y el alcance debe excluir las Fases I y J |
| A-4 · Propagar estado y nota al archivar | **Adoptar con una precisión** → R-4 | Correcto. Falta declarar que el estado y la nota van en un bloque de archivado antepuesto, no reescribiendo el cuerpo del snapshot |
| A-5 · Regla de consumo en la política de versionado | **Adoptar cambiando su criterio** → R-5 | La sustancia es correcta y está bien justificada. «Consumido» no es verificable desde el documento; el estado de la cabecera sí, y expresa lo mismo |

---

## §6 Reparaciones propuestas

**Las ocho están aplicadas.** El texto propuesto de cada una es literal y es el que se pegó en el archivo indicado, salvo los ajustes de redacción que se señalan en cada caso. §0.1 dice dónde quedó cada una.

### §6.1 R-1 · Sufijo de versión al archivar, enunciado una vez y con tabla de exenciones

**Resuelve:** H-1. **Sustituye a:** A-1.
**Archivo principal:** `Master-Prompt.md` §5. **Archivos secundarios:** puntero de una línea en `Root-Rules.md` §3.1, `Rules-Contexto.md` §3.4, `Rules-Necesidades-Negocio.md` §3.4, `Rules-Examples.md` §3.1 y `Rules-Documentacion.md` §3.1.

**Texto propuesto**, a agregar en §5 debajo de la tabla de invariantes de solución:

```text
**Artefactos sin sufijo de versión en el nombre.** Algunos artefactos se emiten sin sufijo
porque son puntos de entrada y su nombre debe ser estable: los `README.md` de sección y el
README raíz de la salida. Al archivarse reciben el sufijo de la versión que se archiva:
`_legacy/<fecha>/README-v<X.Y>.md`. La versión es lo que identifica un snapshot; sin ella,
dos archivados del mismo artefacto colisionan y el segundo sobrescribe al primero sin error.

Quedan exentos del archivado, cada uno por su razón:

| Artefacto | Razón de la exención |
| --- | --- |
| `AGENTS.md` | Se regenera completo desde `Contrato-Agentes-v<X.Y>.md` en cada corrida (§7.2). El artefacto versionado y archivable es el contrato |
| `CHANGELOG.md` | Es acumulativo: su historia es su propio contenido y no tiene estado superado |
| Superficies y assets de la maqueta | Se versionan con el repositorio, no con sufijo de archivo (`Maqueta-Rules.md` §2.3) |
| ADR | Nunca se versionan en el mismo archivo; la anterior queda en `Adrs/` con estado `Superado por ADR-YY` (`Rules-Arquitectura-Tecnica.md` §3.6) |
```

**Por qué en el master-prompt y no en cada regla, contra lo que argumenta A-1.** A-1 razona que «la excepción debe vivir junto a la decisión que la hace necesaria». La decisión de emitir sin sufijo es de cada regla, cierto, pero **el tratamiento al archivar es idéntico en los seis casos**, y es una consecuencia de la política de deprecación, no de la categoría. Duplicar el enunciado en seis archivos reproduce el mecanismo que causó H-2. Además, una regla nueva que declare un índice sin sufijo no hereda nada de lo escrito en las otras seis; enunciado en §5, sí lo hereda. El puntero de una línea en cada regla preserva el valor que A-1 buscaba, que es que quien lee la regla se entere.

**Sobre el archivado ya existente.** La evaluación advierte en su §4 que una corrección apresurada sobre H-1 ya empeoró un estado. La reparación debe incluir la instrucción de **no renombrar retroactivamente** lo ya archivado: un archivo de `_legacy/` cuyo contenido no se verificó no puede recibir una etiqueta de versión, porque la etiqueta sería una afirmación sin evidencia y violaría D9.

### §6.2 R-2 · Ruta de archivado local a la carpeta del artefacto, con eje de fecha

**Resuelve:** H-2 y H-6. **Sustituye a:** A-2.
**Archivos:** `Master-Prompt.md` línea 267 y §3.5; `Root-Rules.md` §2.1.

**Convención propuesta:** `<carpeta-del-artefacto>/_legacy/<YYYY-MM-DD>/`.

**Por qué esta y no la de A-2.** Resuelve H-6 por construcción, porque el eje de proyecto viene dado por la carpeta y no hay que declararlo. Conserva la agrupación por fecha que A-2 quería preservar. Y es la que **menos archivos toca**: ya la cumplen `Master-Prompt.md` §13.6 para el intake (línea 704), `Index-Modelos-UX-UI.md` línea 47 y `Rules-Especificacion-Funcional.md` línea 115 con su `Casos-De-Uso/_legacy/`; y las diez reglas que dicen `_legacy/` a secas dejan de contradecirla, porque pasan a ser un enunciado parcial de la misma ruta en lugar de una convención rival. Solo `Master-Prompt.md` línea 267 cambia de contenido.

**Caso distinto que conviene no confundir.** `Master-Prompt.md` línea 30 y `PROMPT-Agente-Bootstrap-SDD.md` línea 65 mandan archivar en `SDD/Docs/_legacy/<fecha>/` el contenido previo de una corrida anterior completa. No es la misma operación: ahí el artefacto archivado es el árbol entero, y su carpeta es `SDD/Docs/`, con lo cual la ruta ya cumple la convención local. Conviene declararlo explícitamente para que nadie lo lea como una quinta convención.

### §6.3 R-3 · El orquestador toma el snapshot antes de despachar

**Resuelve:** H-3. **Precisa a:** A-3.
**Archivo:** `Master-Prompt.md` §8, esqueleto y reglas de construcción del despacho.

**Texto propuesto**, entre «Path de salida obligatorio» y «Prohibiciones explícitas»:

```text
## Estado previo del entregable

{{VACIO | EXISTENTE, snapshot tomado en PATH_LEGACY}}

Si el bloque dice EXISTENTE, el orquestador ya archivó el estado previo del entregable en
la ruta indicada, antes de construir este despacho. No lo archives de nuevo: editás el
archivo vivo. Si al abrir el entregable encontrás contenido que el snapshot no refleja,
detenete y devolvelo como ambigüedad según §9, sin editar.
```

Y en las reglas de construcción del despacho:

```text
- `{{VACIO | EXISTENTE}}` se resuelve verificando la carpeta de salida antes de construir
  el despacho. Un despacho de corrección posterior a un audit es siempre EXISTENTE.
- Cuando resuelve EXISTENTE, el orquestador toma el snapshot en ese momento, según la
  política de deprecación de §5, y verifica que esté completo antes de despachar. El
  snapshot es responsabilidad del orquestador y no del subagente.
- Esta regla no rige en las Fases I y J, cuyo criterio de re-ejecución vive en §7.2.
```

**Por qué el actor cambia respecto de A-3.** Cuatro razones. El subagente puede fallar o abortar después de haber editado y antes de haber archivado, y el snapshot se pierde igual. Una fase despacha varios subagentes y cada uno archivando produce carpetas `_legacy/<fecha>/` parciales de distintos momentos. Archivar es manipulación de archivos del repositorio, no autoría documental, que es lo que el subagente sabe hacer. Y el framework ya asigna esa responsabilidad al orquestador para el intake, en §13.6. Concentrarla en un solo actor es lo que hace la operación verificable.

**Por qué el alcance excluye I y J.** Porque §7.2 declara un modelo distinto y deliberado —preservar, actualizar con `last_review`, no pisar correcciones manuales— y una instrucción de snapshot incondicional lo contradice. Ver H-8.

### §6.4 R-4 · Estado y nota de archivado, en bloque antepuesto

**Resuelve:** H-4. **Precisa a:** A-4.
**Archivo:** `Master-Prompt.md` §5, línea 267.

**Texto propuesto**, reemplazando la celda actual:

```text
| Política de deprecación | Una sola versión vigente. Las anteriores se archivan en
`<carpeta-del-artefacto>/_legacy/<YYYY-MM-DD>/` antes de sobrescribir, con un bloque de
archivado antepuesto al documento que declara estado `Superado` y enlaza a la versión
vigente. El cuerpo del snapshot no se modifica. Un artefacto cuyo nombre no lleve sufijo
de versión lo recibe al archivarse | Heredado D5. |
```

**Por qué el bloque antepuesto.** Un snapshot cuyo cuerpo se editó para cambiarle el estado ya no es el estado previo, y el propósito del archivado es poder leer qué decía. El bloque antepuesto satisface los dos requisitos que las reglas exigen —que el lector sepa que está superado y dónde está el vigente— sin tocar lo que se quiso preservar. Es además autoevidente para quien abre el archivo: lo primero que lee es la advertencia.

### §6.5 R-5 · Política de versionado con criterio de estado

**Resuelve:** H-5. **Sustituye a:** A-5.
**Archivo:** `Master-Prompt.md` §5, línea 266.

**Texto propuesto**, reemplazando la celda actual:

```text
| Política de versionado de docs | Inicio en `-v1.0`, subir minor en cambios no breaking,
major en breaking. Las correcciones derivadas del audit de la propia fase de emisión se
absorben dentro de la versión en curso, sin subir, mientras el documento esté en estado
`Borrador` o `Propuesto`: el audit forma parte del ciclo de emisión y no de una revisión
posterior a la publicación. Desde que el documento pasa a `Aprobado` o `Vigente` —lo que
ocurre en el corte de fase con confirmación humana, o cuando otro artefacto lo cita como
insumo, lo que suceda primero— toda corrección sube versión y archiva el estado anterior.
Cada corrección absorbida deja su fila en el control de cambios citando el hallazgo del
informe de audit que la origina | Heredado D5, precisado con la cadencia del audit de §10. |
```

**Qué conserva de A-5 y qué cambia.** Conserva la sustancia, el rationale y la exigencia de dejar rastro en el control de cambios, que es lo que hace que absorber correcciones no equivalga a perderlas. Cambia el criterio operativo: donde A-5 dice «consumido», R-5 dice «estado en la cabecera», que es un campo del propio documento, verificable por un auditor que solo tiene los entregables de la fase delante, y que ya pertenece a un enum cerrado del framework (`Root-Rules.md` línea 110). El criterio de consumo se conserva como segunda condición —«o cuando otro artefacto lo cita como insumo»— porque cubre el caso en que un documento es citado antes del corte de fase.

**Dependencia.** La cláusula «citando el hallazgo del informe de audit que la origina» solo es sostenible si el informe de audit sobrevive al re-audit. Requiere R-6.

### §6.6 R-6 · Eje de ronda en el informe de auditoría

**Resuelve:** H-7. **Nueva.**
**Archivo:** `Master-Prompt.md` §10, líneas 599 y 629.

**Texto propuesto** para la línea 599:

```text
Path del informe: `SDD/Docs/Audit/<fase>-<categoria>[-<proyecto>]-r<N>-v1.0.md`, donde
`<N>` es el número de ronda de auditoría de esa fase, empezando en 1. Un veredicto
RECHAZADO produce una ronda nueva: el re-audit escribe su propio informe y no toca el
anterior. Las corridas repetidas de la Fase I se distinguen además por incremento:
`SDD/Docs/Audit/I-<incremento>-<categoria>[-<proyecto>]-r<N>-v1.0.md`.
```

La misma ruta se propaga al esqueleto de despacho del auditor de la línea 629.

**Por qué un informe por ronda y no un informe que se amplía.** Porque cada auditoría es un acto independiente de un agente invocado desde cero, que es la garantía de mirada externa que §10 declara. Un informe único que cada ronda amplía obliga al segundo auditor a editar un documento que no escribió, y le da el contexto del anterior, que es justamente lo que la invocación desde cero busca evitar.

### §6.7 R-7 · Declarar `_legacy/` y `Audit/` en el layout canónico

**Resuelve:** H-9. **Nueva.**
**Archivos:** `Master-Prompt.md` §3.5; `Root-Rules.md` §2.1.

Agregar al bloque de layout de §3.5 las dos carpetas, con una línea que declare que `_legacy/` puede aparecer como hija de cualquier carpeta que contenga artefactos versionados, y que `SDD/Docs/Audit/` recibe los informes de auditoría de todas las fases.

Es el arreglo más barato del conjunto y el que le da fundamento a R-2: con las carpetas declaradas en el layout, la convención de ruta deja de ser una elección entre archivos que se contradicen y pasa a ser una lectura del layout.

### §6.8 R-8 · Declarar la excepción de `VER-XX` y el versionado de la documentación viva

**Resuelve:** H-8. **Nueva.**
**Archivos:** `Master-Prompt.md` §5 y §7.2.

Dos cosas, que se pueden aplicar juntas:

1. Registrar en §5, junto a la política de deprecación, que el campo `evidencia` de los contratos `VER-XX` está exento del archivado, con la razón que §7.2 ya da: lo que importa es el estado presente. Hoy es una excepción a D5 declarada en §7.2 y ausente de la política.
2. Declarar en §7.2 qué pasa con la versión de un documento de 11 actualizado en un corte. La recomendación es que **el corte de cadencia sea el evento de versionado**: el documento sube minor una vez por corte en el que fue tocado, no una vez por edición, y archiva el estado con el que cerró el corte anterior. Es coherente con `Rules-Documentacion.md` §0.4, que ya define el corte como la unidad de actualización, y con que la documentación forme parte de la Definition of Done del incremento.

El punto 2 es una decisión de diseño y se eleva en §10.

---

## §7 Impacto de las reparaciones

### §7.1 Archivos alcanzados

| Reparación | Archivos con cambio de contenido | Archivos con puntero de una línea |
|---|---|---|
| R-1 | `Master-Prompt.md` | `Root-Rules.md`, `Rules-Contexto.md`, `Rules-Necesidades-Negocio.md`, `Rules-Examples.md`, `Rules-Documentacion.md` |
| R-2 | `Master-Prompt.md` | Las diez reglas que dicen `_legacy/` a secas, opcional |
| R-3 | `Master-Prompt.md` | — |
| R-4 | `Master-Prompt.md` | — |
| R-5 | `Master-Prompt.md` | — |
| R-6 | `Master-Prompt.md` | — |
| R-7 | `Master-Prompt.md`, `Root-Rules.md` | — |
| R-8 | `Master-Prompt.md` | — |

**El conjunto completo toca seis archivos con cambio de contenido**, contra los once que A-2 sola requería en la formulación de la evaluación. La concentración en `Master-Prompt.md` no es un defecto de la propuesta: es la consecuencia de que los ocho hallazgos son de política de archivado y de mecánica de despacho, que son materia del orquestador.

### §7.2 Bumps de versión que la intervención exige

Contra la tabla de reglas de intervención de `README.md` líneas 118-123:

| Archivo | Bump | Regla que lo determina |
|---|---|---|
| `Master-Prompt.md` (v3.6) | **minor → 3.7** | Precisa políticas existentes y agrega un bloque al esqueleto de despacho. No agrega categoría ni fase, que es lo que exigiría major |
| `Root-Rules.md` (v1.4) | **minor → 1.5** | Agrega un criterio a una regla existente |
| `Rules-Contexto.md` (v1.5), `Rules-Necesidades-Negocio.md` (v1.4), `Rules-Examples.md` (v2.0), `Rules-Documentacion.md` (v2.0) | **minor** | Ídem, si se aplican los punteros |
| `CHANGELOG.md` del framework | Entrada nueva | Bitácora por intervención |

**No hace falta tocar D5.** Es el punto de impacto que conviene evitar: `README.md` línea 122 declara que modificar una invariante «alcanza a los dieciséis archivos de reglas, al orquestador y a toda la documentación ya emitida», y exige decisión explícita del responsable. Las ocho reparaciones se formulan como precisiones de la **política operativa** de §5, que es donde vive la ruta y el nombre, y no del enunciado de D5, que declara el principio: una sola versión vigente, las superadas se archivan. Ese principio no cambia. Ninguna documentación ya emitida deja de cumplir, porque ningún artefacto existente pasa a estar mal nombrado: lo que cambia es qué hay que hacer de acá en adelante al archivar.

Si el responsable prefiere que D5 también lo diga, el costo sube al máximo del framework. La recomendación es no hacerlo.

### §7.3 Nota de coherencia

`README.md` línea 123 exige nota de coherencia para «cualquier intervención sobre varios archivos», siguiendo el patrón de `Coherencia-Auditoria-Marco-v1.0.md`. La intervención toca seis archivos como mínimo, así que la nota es obligatoria. El precedente de formato más cercano son las notas `Nota-Coherencia-E1.md` a `E8.md` de la intervención anterior, en `09-Editar-Agent-Rules-Documentacion-Examples-final/OUTPUTs/`.

### §7.4 Riesgos de la intervención

| Riesgo | Mitigación |
|---|---|
| Renombrar retroactivamente lo ya archivado en corridas existentes produce etiquetas falsas. Ya pasó una vez, según `Evaluacion-SDD.md` §4 | R-1 incluye la prohibición explícita. Lo ya archivado se deja como está y se documenta su estado |
| R-3 aplicado sin acotar el alcance rompe el modelo de documentación viva | El alcance por fase está en el texto propuesto de R-3, y R-8 lo declara del otro lado |
| R-2 aplicado sin R-7 vuelve a ser una convención por decreto, sin fundamento en el layout | Aplicar R-7 primero. Es de dos bloques |
| R-5 aplicado sin R-6 produce citas a informes de audit sobrescritos | Orden de §8 |

---

## §8 Orden de aplicación y conjunto mínimo eficaz

**Conjunto mínimo eficaz: {R-1, R-3}.** Es el par que detiene la pérdida real. Ninguno de los dos sirve solo: R-1 sin R-3 nombra bien un snapshot que nadie toma, y evita únicamente las pérdidas 3 y 4 del inventario de `Evaluacion-SDD.md` §7; R-3 sin R-1 toma un snapshot que el segundo archivado del día sobrescribe. Esto corrige la prioridad declarada por la evaluación, que presenta a A-1 como aplicable solo.

**Orden propuesto:**

| Orden | Reparación | Por qué en ese lugar |
|---|---|---|
| 1 | R-7 | Es la más barata y es prerrequisito conceptual de R-2. Declara las carpetas que todo lo demás usa |
| 2 | R-2 | Fija la ruta única. Sin ella, R-1 y R-3 escriben en rutas ambiguas |
| 3 | R-1 | Fija el nombre del snapshot. Con R-2 aplicado, resuelve H-1 y H-6 juntos |
| 4 | R-3 | Fija el momento y el actor. Cierra el mecanismo de las tres pérdidas por edición en el lugar |
| 5 | R-4 | Completa el contenido del archivado. No detiene pérdida; mejora la utilidad de lo archivado |
| 6 | R-6 | Preserva el rastro de auditoría del que depende R-5 |
| 7 | R-5 | Cierra el vacío de fondo. Va última porque su cláusula de trazabilidad depende de R-6 |
| 8 | R-8 | Extiende lo anterior al tramo posterior al handoff. Requiere las decisiones de §10 |

Las ocho caben en una sola intervención sobre `Master-Prompt.md`, que es la forma más barata de ejecutarlas: un solo bump, una sola nota de coherencia. Si se prefiere partirlas, el corte natural es {R-7, R-2, R-1, R-3} en una primera intervención y {R-4, R-6, R-5, R-8} en una segunda.

---

## §9 Verificaciones

Las seis de `Evaluacion-SDD.md` §6 siguen siendo válidas. V-1 ya está ejecutada en el anexo. Se agregan cuatro.

Las diez se ejecutaron contra el framework ya intervenido, el 2026-07-28. Todas pasan.

| # | Verificación | Resultado | Evidencia |
|---|---|---|---|
| V-1 | Barrer las **dieciséis** reglas buscando artefactos sin sufijo de versión | **Pasa.** Las seis clases archivables declaran su tratamiento y las tres exentas su razón | Anexo v1.1 §4 |
| V-2 | Buscar `_legacy` en el master-prompt y en las reglas, y comparar rutas | **Pasa.** Una sola convención. Las diez reglas que escriben `_legacy/` a secas quedan cubiertas por la cláusula de abreviatura de §5.1; `Casos-De-Uso/_legacy/` y `SDD/Intake/_legacy/<fecha>/` ya la cumplían | `Master-Prompt.md` §5.1 |
| V-3 | Leer §8 en el lugar de un subagente convocado para corregir | **Pasa.** El esqueleto tiene la sección «Estado previo del entregable», declara que el snapshot ya fue tomado y qué hacer si no refleja lo que el subagente encuentra | `Master-Prompt.md` §8 |
| V-4 | Contrastar la política de §5 con lo que exige cualquier regla de categoría | **Pasa.** Los tres requisitos de las reglas —ruta, estado `Superado`, nota a la vigente— están en §5, más el sufijo de versión que ninguna regla exigía | `Master-Prompt.md` §5 |
| V-5 | Simular un documento emitido, auditado, rechazado, corregido dos veces y aprobado, sin ser citado | **Pasa.** Queda en `v1.0` con cero snapshots: las tres correcciones se absorben mientras el estado es `Propuesto` | `Master-Prompt.md` §5, política de versionado |
| V-6 | Lo mismo, con el documento citado por otro entre la emisión y la primera corrección | **Pasa.** Sube en la primera corrección posterior a la cita, y archiva el estado citado | ídem |
| V-7 | Simular una fase con veredicto RECHAZADO y su re-audit | **Pasa.** `…-r1-v1.0.md` y `…-r2-v1.0.md` coexisten y son distinguibles; el despacho del auditor prohíbe tocar los de rondas anteriores | `Master-Prompt.md` §10 |
| V-8 | Simular una solución de dos proyectos que archivan la misma categoría el mismo día | **Pasa.** Los snapshots caen en `Proyectos/<A>/05-…/_legacy/<fecha>/` y `Proyectos/<B>/05-…/_legacy/<fecha>/`, sin intersección | `Master-Prompt.md` §5.1 |
| V-9 | Contrastar el layout de §3.5 contra las carpetas que el orquestador escribe | **Pasa.** `Audit/` está en el bloque de layout y `_legacy/` en la nota que lo acompaña | `Master-Prompt.md` §3.5 |
| V-10 | Simular tres cortes de la Fase I sobre el mismo documento de 11 | **Pasa.** Sube minor una vez por corte tocado, archiva el estado del corte anterior y `last_review` se renueva en cada uno | `Master-Prompt.md` §7.2 |

### §9.1 Verificación estructural adicional

Al aplicar las reparaciones se corrió además un chequeo de integridad markdown sobre los ocho archivos tocados, buscando filas de tabla separadas de su tabla por una línea en blanco. Detectó tres casos: los dos de §0.3, preexistentes, y cinco filas de control de cambios que la propia intervención había insertado mal. Los cinco se corrigieron antes de cerrar. El chequeo final no devuelve ninguna fila huérfana, y los ocho archivos siguen en UTF-8 con LF, conforme a D2.

---

## §10 Decisiones que requieren al responsable del framework

Las tres quedaron resueltas. Se registran con su resolución y dónde quedó aplicada.

| # | Decisión | Resolución | Dónde quedó |
|---|---|---|---|
| DEC-01 | Versionado en el tramo de documentación viva (Fases I y J) | **(a)** El corte de cadencia sube minor una vez, aunque haya varias ediciones. Aplicada según la recomendación: es coherente con `Rules-Documentacion.md` §0.4, que ya define el corte como la unidad de actualización, y evita las versiones que ningún lector vio. Es la única de las tres que se aplicó por recomendación y no por elección explícita del responsable, así que se señala: revertirla o cambiarla a (b) o (c) es una edición de una fila de tabla | `Master-Prompt.md` §7.2 |
| DEC-02 | Alcance de la corrección sobre lo ya archivado en los repositorios destino existentes | **(a)** No tocar nada. La prohibición de renombrar retroactivamente quedó escrita en la política, con su razón: etiquetar con una versión un archivo cuyo contenido no se verificó viola D9 | `Master-Prompt.md` §5.1, «Sobre lo ya archivado» |
| DEC-03 | Si la precisión entra también en el enunciado de D5 en `README.md` línea 104 | **(a)** No. D5 quedó intacto. La intervención precisa la política operativa de §5, con lo cual no dispara el cambio de mayor impacto de `README.md` línea 122 y ninguna documentación ya emitida deja de cumplir | `README.md` sin cambios |

**Decisión de fondo, resuelta por el responsable.** Entre la Opción A (reparar y conservar `_legacy/`) y la Opción B (eliminar el concepto), evaluadas en `Alternativa-Eliminar-Legacy-v1.0.md`, el responsable eligió la **Opción A**, que es la que se aplicó. La consecuencia que conviene tener presente es la que ese documento anticipa: el mecanismo de archivado sigue siendo una operación manual de fallo silencioso, y las reparaciones lo hacen correcto pero no lo vuelven verificable automáticamente. La condición que habilitaría la Opción B más adelante —el control de versiones como prerrequisito duro, DEC-04— sigue sin cumplirse y sin declararse.

---

## §11 Observaciones menores

Defectos verificables que aparecieron durante la revisión, ajenos al alcance de los hallazgos. Se registran para que quien intervenga los levante de paso.

| # | Observación | Evidencia | Estado |
|---|---|---|---|
| O-1 | El changelog del master-prompt habla de «los trece archivos de reglas». Son dieciséis desde la incorporación de `Maqueta-Rules.md` y `Deriva-Rules.md`. La evaluación heredó el número en su V-1 | Fila 3.6 de `Master-Prompt.md` §16, contra `README.md` línea 29 y `SDD-Development-Guide.md` línea 123 | **Sin corregir**, deliberadamente: vive dentro de una fila histórica de control de cambios, y reescribirla falsearía lo que esa intervención declaró en su momento |
| O-2 | La tabla de control de cambios de §16 del master-prompt estaba partida por una línea en blanco entre las filas 3.3 y 3.4, lo que la rompía como tabla markdown | `Master-Prompt.md`, antes línea 795 | **Corregida** en la intervención, junto con un segundo caso del mismo tipo que apareció al verificar: la fila D9 de §5 estaba fuera de su tabla |
| O-3 | `SDD/Docs/Proyectos/<Nombre-Proyecto>/README.md` está declarado en el layout y ninguna regla lo gobierna: sin contenido especificado, sin criterios de aceptación y sin tratamiento de archivado | `Master-Prompt.md` §3.5, contra `Root-Rules.md` línea 4, que apunta a `SDD/Docs/README.md` | **Sin corregir**: excede el alcance de la política de archivado. Requiere decidir qué categoría lo adopta |
| O-4 | `SDD-Development-Guide.md` §2 afirmaba que `SDD/Devs/Intake/_legacy/` conserva las plantillas del modelo anterior. Esa carpeta se eliminó en la entrada [3.1] del changelog del framework | `SDD-Development-Guide.md` línea 125 | **Corregida** en la intervención |

---

## Control de cambios

| Versión | Fecha | Cambios | Autor |
|---|---|---|---|
| 2.0 | 2026-07-28 | Aplicación de las ocho reparaciones sobre `/IA/IA.SDD/` y verificación del resultado. §0 es nueva y registra qué se aplicó, en qué archivo quedó cada reparación, los bumps de versión, las tres erratas preexistentes corregidas de paso y lo que quedó sin hacer con su motivo. §9 pasa de plan de verificación a resultado: las diez verificaciones se ejecutaron contra el framework intervenido y las diez pasan, más un chequeo estructural de integridad markdown que detectó y corrigió cinco filas de tabla que la propia intervención había insertado mal. §10 registra las tres decisiones como resueltas, señalando que DEC-01 se aplicó por recomendación y no por elección explícita. §11 suma O-4 y marca el estado de cada observación. Se registra que el responsable eligió la Opción A sobre la alternativa de eliminar `_legacy/`. Sube major porque el documento cambia de naturaleza: deja de ser una propuesta sin aplicar y pasa a ser el registro de una intervención ejecutada. | Revisión SDD (Claude Code) |
| 1.0 | 2026-07-28 | Revisión inicial de `INPUTs/Evaluacion-SDD.md` v1.0 contra el estado actual de `IA.SDD`. Los cinco hallazgos se confirman, dos de ellos subdimensionados. Se agregan cuatro hallazgos nuevos: la ruta de §5 pierde el eje de proyecto, el re-audit sobrescribe el informe de auditoría, la política de versionado no cubre las Fases I y J, y el layout canónico no declara `_legacy/` ni `Audit/`. Se evalúan los cinco arreglos propuestos y se sustituyen por ocho reparaciones con texto literal, su orden de aplicación, su impacto en versiones y tres decisiones elevadas al responsable. Se ejecuta la verificación V-1 en anexo y se agregan V-7 a V-10. Ningún archivo de `IA.SDD` fue modificado. | Revisión SDD (Claude Code) |
