# Evaluación del framework SDD — Política de deprecación y archivado

**Documento:** `Evaluacion-SDD.md`
**Fecha:** 2026-07-28
**Autor:** Orquestador SDD (Claude Code), durante la ejecución que se describe en §1
**Destinatario:** un agente de IA sin contexto previo, encargado de revisar o corregir el framework `IA.SDD`
**Alcance:** cuatro defectos del framework y un vacío de fondo, todos alrededor de la política de deprecación (D5) y del archivado en `_legacy/`
**Estado:** hallazgos verificados con evidencia citada; ninguna corrección aplicada

> Este documento no modificó ningún archivo de `IA.SDD`. Todo lo que propone en §5 está sin aplicar.

---

## Tabla de contenido

- [§1 Contexto en que se produjeron los hallazgos](#1-contexto-en-que-se-produjeron-los-hallazgos)
- [§2 Resumen de hallazgos](#2-resumen-de-hallazgos)
- [§3 Hallazgos con su evidencia](#3-hallazgos-con-su-evidencia)
- [§4 Lo que NO es defecto del framework](#4-lo-que-no-es-defecto-del-framework)
- [§5 Arreglos propuestos](#5-arreglos-propuestos)
- [§6 Cómo verificar que el arreglo funciona](#6-cómo-verificar-que-el-arreglo-funciona)
- [§7 Anexo — Inventario de pérdidas concretas](#7-anexo--inventario-de-pérdidas-concretas)

---

## §1 Contexto en que se produjeron los hallazgos

### §1.1 Qué se estaba ejecutando

Una corrida completa del orquestador SDD sobre una solución real, no un ejercicio. La invocación fue la declarada por el prompt de entrada:

```text
Leer y Ejecutar /IA/IA.SDD/PROMPTS/PROMPT-Agente-Bootstrap-SDD.md
en el repositorio: /DEV/SelfHosted.Service.Core
```

| Elemento | Valor |
|---|---|
| Repositorio fuente | `IA/IA.SDD` |
| Repositorio destino | `DEV/SelfHosted.Service.Core` |
| Solución | SelfHosted.Service.Core, cuatro proyectos: un `web-monolith` principal y tres `library` |
| Fases ejecutadas | Validación de intake y Fase A completa (00-Contexto y 01-Necesidades-Negocio) |
| Fecha de la corrida | 2026-07-27 y 2026-07-28 |

### §1.2 Versiones de los artefactos del framework en el momento de la evaluación

| Artefacto | Versión |
|---|---|
| `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md` | 2.2 |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 3.6 |
| `SDD/Devs/Rules/Intake-Rules.md` | 1.1 |
| `SDD/Devs/Rules/Rules-Contexto.md` | 1.5 |
| `SDD/Devs/Rules/Rules-Necesidades-Negocio.md` | 1.4 |

Las diez reglas de categoría restantes se citan en §3.2 con la línea exacta relevante.

### §1.3 Qué ocurrió que motivó la evaluación

Durante la corrida se perdieron **cinco estados archivados**. El inventario completo está en §7. La secuencia relevante para entender el mecanismo es esta:

1. El intake pasó por tres versiones (1.0, 1.1, 1.2) en dos días, y por cinco rondas de corrección post-auditoría dentro de la 1.2.
2. Los documentos de `00-Contexto` y `01-Necesidades-Negocio` pasaron por tres versiones cada uno (1.0, 1.1, 1.2), con rondas de corrección dentro de cada una.
3. Los `README.md` de sección perdieron su estado v1.0 en las dos categorías, por la misma causa estructural.

Esa cadencia —múltiples correcciones sobre el mismo documento en el mismo día, dentro del ciclo de auditoría que el propio master-prompt exige en §10— es la condición en la que los defectos se manifiestan. Con la cadencia que el framework parece suponer (`v1.0 → v2.0`, cambios espaciados) ninguno de los cuatro habría aparecido.

### §1.4 Advertencia sobre el alcance de esta evaluación

Los hallazgos surgen de **una** corrida, de las fases de validación de intake y A. Las fases B a J no se ejecutaron, de modo que:

- No se ejercitaron las reglas de las categorías 02 a 11 en su parte de versionado, sólo se leyeron.
- No se ejercitó la Fase B2 ni `Maqueta-Rules.md`.
- No se ejercitaron las Fases I y J ni el modelo de documentación viva de `Rules-Documentacion.md`, donde §7.2 del master-prompt declara un criterio de re-ejecución que podría tener el mismo problema y no fue verificado.

Un revisor debería contrastar los hallazgos contra esas fases antes de darlos por completos.

---

## §2 Resumen de hallazgos

| # | Hallazgo | Severidad | Produjo pérdida real | Archivos a tocar |
|---|---|---|---|---|
| H-1 | El `README.md` de sección es imposible de archivar sin colisión | Alta | Sí, dos veces | `Rules-Contexto.md`, `Rules-Necesidades-Negocio.md`, y toda regla que mande un artefacto sin versión en el nombre |
| H-2 | Dos convenciones de ruta de archivado que no coinciden | Media | No | `Master-Prompt.md` §5 y las diez reglas de categoría |
| H-3 | Las reglas describen un flujo de edición distinto del que el master-prompt produce | Alta | Sí, tres veces | `Master-Prompt.md` §8 |
| H-4 | Requisitos de archivado que viven sólo en las reglas y no llegan a los subagentes | Baja | No | `Master-Prompt.md` §5 |
| H-5 | El framework no cubre la cadencia de corrección dentro del ciclo de auditoría | Alta | No, pero obligó a inventar una regla | `Master-Prompt.md` §5 |

H-5 es el vacío de fondo: los otros cuatro son manifestaciones o consecuencias suyas.

---

## §3 Hallazgos con su evidencia

### §3.1 H-1 · El `README.md` de sección es imposible de archivar sin colisión

**Enunciado.** El framework manda emitir un artefacto **sin sufijo de versión en su nombre** y, a la vez, archivar las versiones anteriores identificándolas por nombre de archivo. Los dos requisitos son incompatibles para ese artefacto: el segundo archivado del mismo día sobrescribe al primero.

**Evidencia 1 — el artefacto se manda sin versión.**

`SDD/Devs/Rules/Rules-Contexto.md`, línea 90, §3.4:

> Recomendado. La carpeta `SDD/Docs/00-Contexto/` lleva un `README.md` (sin versión) que enumera los 5 documentos con su propósito, su estado y el orden de lectura sugerido.

`SDD/Devs/Rules/Rules-Necesidades-Negocio.md`, línea 103, §3.4:

> El `README.md` de `01-Necesidades-Negocio/` se recomienda cuando hay más de 5 NB.

Ambos lo nombran `README.md`, sin sufijo. Es deliberado y correcto para el archivo vivo: un índice de sección no debería obligar a actualizar enlaces cada vez que sube de versión.

**Evidencia 2 — el archivado identifica por nombre.**

`SDD/Devs/Orchestrator/Master-Prompt.md`, línea 267, §5:

> | Política de deprecación | Una sola versión vigente, las anteriores se archivan en `_legacy/<categoria>/<fecha>/` | Heredado D5. |

**Evidencia 3 — el resultado observado.**

En la corrida, `SDD/Docs/00-Contexto/` archivó dos veces el mismo día, 2026-07-28: primero el estado v1.0 y después el v1.1. Los cinco documentos versionados conviven sin problema en `_legacy/2026-07-28/`, porque su nombre lleva la versión:

```text
Vision-Producto-v1.0.md
Vision-Producto-v1.1.md
Alcance-Proyecto-v1.0.md
Alcance-Proyecto-v1.1.md
...
```

El README no. El segundo snapshot escribió `_legacy/2026-07-28/README.md` sobre el primero, y el estado v1.0 dejó de existir. Lo mismo ocurrió, de forma independiente, en `SDD/Docs/01-Necesidades-Negocio/`.

**Por qué importa.** Es el único de los cinco hallazgos que produce pérdida de información irrecuperable, y la produce de forma silenciosa: ningún actor recibe error, y el directorio de archivado se ve correcto. Un auditor que verifique "existe el snapshot" lo da por bueno.

**Alcance más amplio del que parece.** El defecto no es del README en particular sino de **todo artefacto que el framework mande sin versión en el nombre**. Un revisor debería barrer las trece reglas buscando otros casos antes de corregir sólo estos dos.

---

### §3.2 H-2 · Dos convenciones de ruta de archivado que no coinciden

**Enunciado.** El master-prompt y las reglas de categoría declaran rutas de archivado distintas para la misma política.

**Evidencia — el master-prompt usa un eje de categoría y otro de fecha.**

`Master-Prompt.md` línea 267: `_legacy/<categoria>/<fecha>/`
`Master-Prompt.md` línea 704, §13.6: `SDD/Intake/_legacy/<YYYY-MM-DD>/`

**Evidencia — las diez reglas de categoría usan `_legacy/` a secas.**

Verificado programáticamente sobre `SDD/Devs/Rules/`: de los diez archivos de regla que mencionan `_legacy`, **ninguno** usa un eje de fecha ni de categoría. Ocurrencias representativas:

| Archivo | Línea | Cita |
|---|---|---|
| `Rules-UX-UI-DX.md` | 180 | «La versión anterior se mueve a `_legacy/` con estado `Superado`…» |
| `Rules-Especificacion-Funcional.md` | 115 | «La versión `v1.0` se mueve a `Casos-De-Uso/_legacy/` con estado `Superado`…» |
| `Rules-Prompts-AI.md` | 121 | «La versión `v1.0` se mueve a `_legacy/` con estado `Superado`…» |
| `Rules-Calidad-Y-Pruebas.md` | 123 | «…la versión anterior se mueve a `_legacy/` con estado `Superado`.» |
| `Rules-Devops.md` | 130 | ídem |
| `Rules-Arquitectura-Tecnica.md` | 149 | ídem |
| `Rules-Backlog-Tecnico.md` | 122 | ídem |
| `Rules-Documentacion.md` | 400 | ídem |

**Consecuencia.** Un subagente que siga su regla de categoría produce una ruta; el orquestador que siga §5 produce otra. En la corrida no generó un defecto visible porque el orquestador impuso su propia convención al despachar, pero es una ambigüedad latente: dos agentes distintos pueden archivar el mismo artefacto en dos lugares.

**Observación sobre cuál es la correcta.** El eje de fecha del master-prompt es el que **causa** la colisión de H-1, porque agrupa por día y dos rondas del mismo día caen juntas. El eje sin fecha de las reglas no colisiona **siempre que el nombre lleve la versión**, que es justo lo que H-1 señala que no ocurre para el README. Es decir: las dos convenciones fallan para el mismo artefacto, por razones distintas. Corregir H-1 resuelve el problema con cualquiera de las dos rutas.

---

### §3.3 H-3 · Las reglas describen un flujo de edición distinto del que el master-prompt produce

**Enunciado.** Las reglas de categoría describen el archivado presuponiendo que se **crea un archivo nuevo y se mueve el viejo**. El master-prompt despacha subagentes que **editan documentos existentes en su lugar**. En el primer flujo la pérdida es imposible; en el segundo es el resultado por defecto si nadie copia antes.

**Evidencia — el flujo que las reglas presuponen.**

`Rules-UX-UI-DX.md` línea 405:

> ¿Existe alguna versión anterior en la carpeta principal? Si la respuesta es sí, archivarla en `_legacy/` antes de publicar la nueva.

`Rules-Especificacion-Funcional.md` línea 277: idéntico.
`Rules-Prompts-AI.md` línea 297: idéntico.

El verbo es **mover**, y el momento es **antes de publicar la nueva**. Eso describe una operación donde el archivo viejo y el nuevo son dos objetos distintos y coexisten.

**Evidencia — el flujo que el master-prompt produce.**

`Master-Prompt.md` §8 define el esqueleto de despacho de subagentes. Sus secciones son: rol asignado, contexto, invariantes, insumos a leer, documentos a producir, trazabilidad, criterios de aceptación, path de salida, prohibiciones, prompt-snippet y devolución.

**Ninguna de esas secciones menciona el archivado, ni el momento de tomarlo, ni distingue entre producir un documento nuevo y corregir uno existente.** El esqueleto está escrito para la emisión inicial. Cuando el mismo subagente vuelve a ser convocado para corregir un hallazgo de auditoría —que es lo que §10 exige que ocurra— el esqueleto no dice nada, y editar en el lugar es lo que un agente hace naturalmente.

**Evidencia — el resultado observado.** Tres de las cinco pérdidas de §7 se produjeron exactamente así: un subagente recibió la instrucción de corregir, editó el archivo en su lugar, y el estado previo dejó de existir sin que nadie lo hubiera archivado.

**Nota.** Para el intake, el master-prompt **sí** declara el momento, en §13.6: «se archivan… **antes de sobrescribir**». Esa instrucción existe y es correcta. Que igualmente se haya perdido el intake v1.1 es un error de ejecución del orquestador, no un defecto del framework; se registra como tal en §4.

---

### §3.4 H-4 · Requisitos de archivado que viven sólo en las reglas y no llegan a los subagentes

**Enunciado.** Las reglas de categoría exigen dos cosas al archivar que el master-prompt no recoge en su política de §5 y que, por lo tanto, no llegan al bloque de invariantes que se inyecta a los subagentes.

**Evidencia.** Las citas de §3.2 muestran el patrón completo, que es:

> La versión anterior se mueve a `_legacy/` **con estado `Superado`** y **una nota al inicio que apunte a la versión vigente**.

Los dos requisitos en negrita no aparecen en `Master-Prompt.md` §5, línea 267, que sólo declara la ruta.

**Consecuencia observada.** En la corrida se archivaron 33 artefactos. **Ninguno** quedó con estado `Superado` ni con nota apuntando a la versión vigente. Un lector que abra un archivo de `_legacy/` no tiene forma de saber, desde el propio archivo, que está superado ni dónde está el vigente.

**Severidad baja** porque no produce pérdida y es recuperable en cualquier momento, pero degrada la utilidad del archivado, que es justamente poder leer un estado anterior sabiendo qué es.

---

### §3.5 H-5 · El framework no cubre la cadencia de corrección dentro del ciclo de auditoría

**Enunciado.** La política de versionado del framework está escrita para cambios sustantivos y espaciados. No dice qué hacer cuando un artefacto se corrige varias veces el mismo día, dentro del ciclo de auditoría que el propio framework exige.

**Evidencia — cómo está escrita la política.**

`Master-Prompt.md` §5, línea 266:

> | Política de versionado de docs | Inicio en `-v1.0`, subir minor en cambios no breaking, major en breaking | Heredado D5. |

Las reglas de categoría son más explícitas y confirman el supuesto. `Rules-Calidad-Y-Pruebas.md` línea 123: «Cuando se pasa de `v1.0` a `v2.0`…». `Rules-Devops.md` línea 130: idéntico. El caso que tienen en mente es el salto mayor, no la corrección.

**Evidencia — la cadencia que el framework exige.**

`Master-Prompt.md` §10 obliga a un audit independiente al cierre de cada fase, con veredicto bloqueante: «RECHAZADO obliga a corrección y re-audit». Y §7 declara: «Detención obligatoria entre fases: el orquestador no inicia la siguiente fase sin que el audit de la fase previa haya devuelto APROBADO».

Es decir: **el framework garantiza que va a haber rondas de corrección sobre documentos ya emitidos**, y no dice si cada una sube de versión.

**Las dos lecturas posibles, ambas malas.**

- Si cada corrección sube minor, un documento que pasó por cinco rondas de auditoría llega a `v1.5` con cuatro versiones archivadas que ningún lector vio nunca. El archivado se llena de estados que no fueron consumidos por nadie.
- Si ninguna sube, se pierde el rastro de qué se corrigió y cuándo, y dos artefactos distintos pueden llamarse igual.

**Qué se hizo en la corrida, a falta de regla.** El orquestador declaró una invariante propia y la aplicó a toda la solución:

> Sube de versión el artefacto que **ya fue consumido** por otro artefacto. El que todavía no fue consumido absorbe sus correcciones dentro de su versión inicial, porque el audit forma parte de su ciclo de emisión y no de una revisión posterior a la publicación.

El auditor independiente la evaluó explícitamente y la endosó, con este argumento: los documentos en estado `Propuesto` no habían sido leídos por ningún consumidor, y subir minor por ronda produciría versiones «cuyas 1.0, 1.1 y 1.2 no existieron para ningún lector». También señaló que el versionado existe para señalizar contra qué estado trabajó un consumidor, y que sin consumidor no hay nada que señalizar.

Esa regla funciona, pero **no está en el framework**: se inventó durante la corrida y se perdería en la siguiente si no se incorpora.

---

## §4 Lo que NO es defecto del framework

Esta sección existe para que un revisor no gaste esfuerzo corrigiendo lo que no está roto. Dos de las cinco pérdidas de §7 fueron errores de ejecución del orquestador contra reglas que existían y eran claras.

| Error | Regla que existía y no se aplicó |
|---|---|
| El intake v1.1 se perdió al despachar la integración de la v1.2 sin archivar antes | `Master-Prompt.md` §13.6: «se archivan… **antes de sobrescribir**». La instrucción era explícita |
| Un archivo de `_legacy/` se renombró sin verificar su contenido, dos veces, y la segunda produjo una etiqueta falsa | Ninguna regla lo cubre porque es higiene elemental de manipulación de archivos, no materia de un framework de documentación |

El segundo merece un comentario para el revisor: la intervención que produjo la etiqueta falsa fue un intento de **prevenir** H-1 en la segunda categoría. Es un ejemplo de que una corrección apresurada sobre un defecto real puede empeorar el estado. Si el revisor corrige H-1, conviene que la corrección incluya cómo tratar los archivados ya existentes, que están mal etiquetados o incompletos.

**El prompt de entrada no está implicado.** `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md` v2.2 menciona `_legacy` una sola vez, en su §2 prerrequisito 4, y sólo para el caso de una carpeta `SDD/Docs/` con contenido previo. Es correcto y no requiere cambios. Su delgadez es deliberada y conviene preservarla: el prompt de entrada fija el modelo de dos repositorios, los prerrequisitos y la invocación, y delega el resto.

---

## §5 Arreglos propuestos

Ninguno está aplicado. Se ordenan por dependencia: A-1 es el único que resuelve una pérdida real y se puede aplicar solo.

### §5.1 A-1 · Declarar el sufijo de versión en el archivado de artefactos sin versión

**Resuelve:** H-1.
**Archivos:** `Rules-Contexto.md` §3.4, `Rules-Necesidades-Negocio.md` §3.4, y toda otra regla que mande un artefacto sin sufijo de versión, tras el barrido que §3.1 recomienda.

**Texto propuesto**, a agregar en cada §3.4 afectado:

> El archivo vivo se llama `README.md`, sin sufijo de versión, porque es el punto de entrada de la carpeta y su nombre debe ser estable. Al archivarse, en cambio, **recibe el sufijo de la versión que se archiva**: `_legacy/<fecha>/README-v<X.Y>.md`. La versión es lo que identifica un snapshot, y sin ella dos archivados del mismo artefacto colisionan y el segundo sobrescribe al primero de forma silenciosa.

**Por qué en las reglas y no en el master-prompt.** Porque la decisión de emitir el README sin versión es de la regla de categoría, y la excepción debe vivir junto a la decisión que la hace necesaria.

**Alternativa descartada.** Cambiar el eje de la carpeta de archivado, por ejemplo a `_legacy/<version>/`. Resuelve la colisión pero rompe la agrupación por fecha, que es útil para saber qué se archivó junto, y obliga a tocar el master-prompt y las diez reglas en lugar de dos.

---

### §5.2 A-2 · Unificar la ruta de archivado

**Resuelve:** H-2.
**Archivos:** `Master-Prompt.md` §5 línea 267, o las diez reglas de categoría, según cuál convención se adopte.

Hay que elegir una y propagarla. La recomendación es **conservar el eje de fecha del master-prompt** y alinear las reglas, por dos razones: agrupa lo archivado en una misma operación, lo que ayuda a reconstruir qué pasó; y con A-1 aplicado, la colisión que el eje de fecha habilitaba deja de existir.

Si en cambio se adopta la forma sin fecha de las reglas, hay que verificar que **todo** artefacto lleve versión en su nombre, porque esa convención depende enteramente de eso.

**Advertencia para el revisor.** Este arreglo toca once archivos y no resuelve ninguna pérdida. Aplicarlo sin A-1 no mejora nada.

---

### §5.3 A-3 · Instruir el snapshot previo en el esqueleto de despacho

**Resuelve:** H-3.
**Archivo:** `Master-Prompt.md` §8.

**Propuesta.** Agregar al esqueleto de despacho una sección que hoy no existe, entre «Path de salida obligatorio» y «Prohibiciones explícitas»:

```text
## Estado previo del entregable

{{VACÍO | EXISTENTE}}

Si el entregable ya existe, antes de editar cualquier archivo tomás el snapshot de su
estado actual en {{PATH_LEGACY}}, según la política de deprecación de las invariantes.
El snapshot se toma antes de la primera edición, no al final: un archivo editado en su
lugar ya no tiene estado previo del cual tomarlo. Verificá que el snapshot esté completo
antes de empezar.
```

Y en las reglas de construcción del despacho, la contraparte para el orquestador:

> - `{{VACÍO | EXISTENTE}}` se resuelve verificando la carpeta de salida antes de construir el despacho. Un despacho de corrección posterior a un audit es siempre `EXISTENTE`.

**Por qué en el master-prompt y no en las reglas.** Porque el problema no es de ninguna categoría en particular: es del modelo de despacho, que es materia del orquestador. Las reglas ya dicen qué archivar; lo que falta es que el subagente sepa cuándo.

---

### §5.4 A-4 · Propagar los requisitos de estado y nota al archivar

**Resuelve:** H-4.
**Archivo:** `Master-Prompt.md` §5, línea 267.

**Texto propuesto**, reemplazando la celda actual:

> | Política de deprecación | Una sola versión vigente. Las anteriores se archivan en `_legacy/<categoria>/<fecha>/` antes de sobrescribir, con estado `Superado` y una nota al inicio que apunte a la versión vigente. Un artefacto cuyo nombre no lleve sufijo de versión lo recibe al archivarse | Heredado D5. |

Con eso los dos requisitos que hoy sólo viven en las reglas entran al bloque de invariantes que §8 inyecta a todos los subagentes, y la excepción de A-1 queda declarada también a nivel solución.

---

### §5.5 A-5 · Incorporar la regla de consumo a la política de versionado

**Resuelve:** H-5.
**Archivo:** `Master-Prompt.md` §5, línea 266.

**Texto propuesto**, reemplazando la celda actual:

> | Política de versionado de docs | Inicio en `-v1.0`, subir minor en cambios no breaking, major en breaking. Las correcciones derivadas del audit de su propia fase se absorben **dentro de la versión en curso**, sin subir, mientras el artefacto no haya sido consumido por otro: el audit forma parte del ciclo de emisión y no de una revisión posterior a la publicación. Sube de versión el artefacto que ya fue consumido, entendiendo por consumido que otro artefacto lo cita como insumo o que fue confirmado como canónico. Cada corrección absorbida deja su fila en el control de cambios citando el hallazgo que la origina | Heredado D5, precisado con la cadencia del audit de §10. |

**Justificación para el revisor.** Sin esta regla, el ciclo de auditoría que §10 exige produce o bien una proliferación de versiones que ningún lector vio, o bien la pérdida del rastro de las correcciones. La regla fue aplicada durante la corrida a 16 documentos, un intake y un manifiesto, y evaluada y endosada por el auditor independiente. Su formulación aquí es la que se usó.

**Punto que el revisor debe decidir.** La regla usa «consumido» como criterio, y su aplicación exige saber si alguien citó el artefacto. En la corrida eso fue evaluable porque la cadena de trazabilidad D6 declara quién consume qué. Conviene verificar que sea igual de evaluable en las Fases I y J, donde `Rules-Documentacion.md` introduce un modelo de documentación viva con tres momentos y una cadencia distinta.

---

## §6 Cómo verificar que el arreglo funciona

Pruebas concretas que un revisor puede correr contra el framework corregido, sin ejecutar una corrida completa.

| # | Verificación | Resultado esperado |
|---|---|---|
| V-1 | Barrer las trece reglas buscando artefactos mandados sin sufijo de versión en el nombre | Todos los encontrados declaran su tratamiento al archivarse |
| V-2 | Buscar `_legacy` en el master-prompt y en las reglas, y comparar las rutas | Una sola convención en los once archivos |
| V-3 | Leer el esqueleto de despacho de §8 poniéndose en el lugar de un subagente convocado para corregir un hallazgo | El esqueleto dice qué hacer con el estado previo antes de editar |
| V-4 | Leer la política de deprecación de §5 y contrastarla con lo que exige cualquier regla de categoría | Ningún requisito de las reglas está ausente de §5 |
| V-5 | Simular: un documento emitido, auditado, rechazado, corregido dos veces y aprobado, sin haber sido citado por nadie | La política dice sin ambigüedad qué versión tiene al final y cuántos snapshots quedaron |
| V-6 | Simular lo mismo, pero con el documento citado por otro entre la emisión y la primera corrección | La política dice sin ambigüedad en qué punto sube de versión |

V-5 y V-6 son las que verifican A-5 y las que el framework hoy no puede responder.

---

## §7 Anexo — Inventario de pérdidas concretas

Registro de lo que efectivamente se perdió durante la corrida, con su causa. Sirve para dimensionar el impacto y para que el revisor pueda contrastar los hallazgos contra hechos.

| # | Artefacto | Estado perdido | Causa | Hallazgo |
|---|---|---|---|---|
| 1 | `SOLUTION-INTAKE-SelfHosted-Service-Core` | v1.1 completa | Edición en el lugar sin snapshot previo, contra la instrucción explícita de §13.6 | Error de ejecución, §4 |
| 2 | `SOLUTION-INTAKE-SelfHosted-Service-Core` | Estado de la v1.2 previo a su primera auditoría | Cinco rondas de corrección absorbidas dentro de la misma versión, sin snapshot intermedio | H-5 |
| 3 | `SDD/Docs/00-Contexto/README.md` | v1.0 | Segundo archivado del mismo día sobrescribió al primero | H-1 |
| 4 | `SDD/Docs/01-Necesidades-Negocio/README.md` | v1.0 | Idéntica a la anterior, en otra categoría | H-1 |
| 5 | `SDD/Docs/01-Necesidades-Negocio/_legacy/2026-07-28/README.md` | Etiqueta correcta | Renombrado sin verificar contenido, en un intento de prevenir el caso 4 | Error de ejecución, §4 |

**Sobre la recuperabilidad.** Ninguno de los cinco es reconstruible con fidelidad. Lo que sobrevive en todos los casos es el control de cambios del artefacto vivo, que describe qué cambió en cada ronda y por qué. Eso permite saber **qué** decía la versión perdida en términos de cambios, pero no reproducir su texto.

**Sobre el caso 2.** Se registra como pérdida aunque el auditor lo consideró aceptable y compartió el criterio de absorber las correcciones. Se lista porque un revisor podría concluir que la regla de A-5 debería incluir un snapshot por ronda de auditoría, y conviene que tenga el dato para decidirlo.

---

## Control de cambios

| Versión | Fecha | Cambios | Autor |
|---|---|---|---|
| 1.0 | 2026-07-28 | Evaluación inicial. Cinco hallazgos sobre la política de deprecación y el archivado, surgidos de la corrida del orquestador sobre `DEV/SelfHosted.Service.Core` durante la validación de intake y la Fase A. Incluye evidencia citada con archivo y línea, separación entre defectos del framework y errores de ejecución del orquestador, cinco arreglos propuestos sin aplicar, seis verificaciones para contrastar el arreglo, y el inventario de las cinco pérdidas concretas con su causa. Ningún archivo de `IA.SDD` fue modificado. | Orquestador SDD (Claude Code) |
