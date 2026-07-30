# Alternativa de fondo — Eliminar el concepto de `_legacy/` del framework

**Documento:** `Alternativa-Eliminar-Legacy-v1.0.md`
**Versión:** 1.1
**Estado:** Evaluado, no adoptado
**Fecha:** 2026-07-28
**Autor:** Revisión SDD (Claude Code)
**Documento padre:** `Revision-Hallazgos-SDD-v1.0.md`
**Origen:** pregunta del responsable del framework: si `_legacy/` concentra la mayoría de los defectos, ¿no conviene limpiar el concepto en lugar de repararlo?

> **Resolución.** El responsable del framework evaluó esta alternativa y eligió la **Opción A**: reparar `_legacy/` y conservarlo. Lo que se aplicó sobre `IA.SDD` son las ocho reparaciones del documento padre, no lo que este documento propone. La evaluación se conserva porque su análisis sigue siendo válido y porque la condición que la habilitaría —DEC-04, el control de versiones como prerrequisito duro— puede resolverse más adelante.

> Ningún archivo de `IA.SDD` fue modificado **por este documento**. La única de sus observaciones que sí se llevó a la intervención es la referencia colgada de `SDD-Development-Guide.md` §2, corregida como errata.

---

## §1 Respuesta corta

**Sí, conviene.** Seis de los nueve hallazgos de la revisión desaparecen en lugar de repararse, y desaparecen porque deja de existir el mecanismo que los produce, no porque se los parchee.

El costo es real y hay que declararlo: la eliminación toca el enunciado de **D5**, que es la clase de cambio que `README.md` línea 122 declara de mayor impacto del framework. Pero ese costo está calibrado para el caso contrario al nuestro, y §5 explica por qué.

La condición que hace viable la eliminación es una sola, y hoy no se cumple: **el repositorio destino tiene que ser un repositorio con control de versiones, y eso tiene que ser un prerrequisito duro**. Hoy no lo es.

---

## §2 Qué hace `_legacy/`, y qué de eso hace falta

La política no nació de un diseño: nació de un diagnóstico concreto de la auditoría del fuente. `SDD/Devs/Bootstrap/Audit-SDD1.md` línea 103:

> Coexistencia de v1.0 y v1.1 / v2.0 sin marcar deprecacion. […] Definir politica: solo la version vigente queda en el arbol; las anteriores se archivan en `_legacy/` con sello `Estado: Superado`.

La cita tiene dos mitades separables, y ahí está la clave del asunto:

| Mitad | Qué es | ¿Hace falta? |
|---|---|---|
| «solo la versión vigente queda en el árbol» | El **requisito**. Resuelve la ambigüedad de qué documento leer, que es el problema que se había observado | **Sí.** Es el valor real de D5 |
| «las anteriores se archivan en `_legacy/`» | El **mecanismo** elegido para cumplirlo | **No necesariamente.** Es una de varias formas |

`Marco-Teorico-SDD-v1.0.md` línea 1668 repite la misma estructura: el problema declarado es «v1.0 y v2.0 en paralelo confunde al lector», y la solución declarada mezcla el requisito con el mecanismo.

El requisito se cumple con **borrar la versión anterior**. Que el estado previo se pueda recuperar es un segundo propósito, distinto, que se le colgó al mismo mecanismo. La revisión de los hallazgos muestra que ese acople es la fuente de los defectos: cinco de los nueve hallazgos son sobre cómo nombrar, dónde poner, cuándo tomar y cómo etiquetar la copia, no sobre la vigencia.

---

## §3 Evidencia de que la copia no es el mecanismo de preservación

### §3.1 El framework se aplicó la política a sí mismo, y la copia se borró sin que nadie lo notara

| Hecho | Evidencia |
|---|---|
| Existió `SDD/Devs/Intake/_legacy/2026-06-10/` con dos plantillas | `git log --diff-filter=A`: alta en `11b4091`, 2026-07-17 |
| Se borró | `git log --diff-filter=D`: baja en `568a44d`, 2026-07-26, el commit más reciente |
| Hoy no existe | `SDD/Devs/Intake/` contiene únicamente `SOLUTION-INTAKE-template.md` y `SOLUTION-MANIFEST-template.md` |
| Un documento vigente seguía afirmando que existe | `SDD-Development-Guide.md` línea 125: «El subdirectorio `_legacy/` conserva las plantillas del modelo anterior». Corregido en la intervención |
| La baja sí estaba registrada donde correspondía | `CHANGELOG.md` línea 12 la declara bajo «### Eliminado» de la entrada [3.1], con el detalle de los dos archivos |
| No se perdió nada | Las dos plantillas son recuperables con `git show 11b4091:<path>` |

Es el experimento hecho. El archivado desapareció, nadie lo notó porque la historia real estaba en otro lado, y lo que quedó del mecanismo fue **una afirmación falsa** en un documento vigente. Una copia en el árbol es un artefacto más que hay que mantener sincronizado con lo que se dice de ella; el historial de versiones no lo es.

> **Corrección respecto de la versión 1.0 de este documento.** La 1.0 afirmaba que eran *dos* las referencias colgadas, contando a `CHANGELOG.md` línea 12. Es incorrecto: esa línea registra la eliminación bajo el encabezado «### Eliminado», que es exactamente lo que un changelog debe hacer. La referencia colgada era una sola, la de `SDD-Development-Guide.md`. El argumento de fondo no cambia —la copia se borró sin pérdida y dejó una afirmación desincronizada— pero el dato sí, y queda corregido.

### §3.2 La corrida sobre `SelfHosted.Service.Core` dice lo mismo

`INPUTs/Evaluacion-SDD.md` §7, sobre la recuperabilidad de las cinco pérdidas:

> Lo que sobrevive en todos los casos es el control de cambios del artefacto vivo, que describe qué cambió en cada ronda y por qué. Eso permite saber **qué** decía la versión perdida en términos de cambios, pero no reproducir su texto.

O sea: lo que efectivamente sirvió no fue el archivado sino la sección de control de cambios, que D5 ya exige en todo documento y que vive en el archivo vigente. El archivado aportó, en la práctica, cero información recuperada.

### §3.3 El framework ya tiene el modelo alternativo, aplicado y con su rationale escrito

`Maqueta-Rules.md` línea 92:

> Archivos HTML de superficie: Título-Con-Guiones, **sin sufijo de versión**. […] La maqueta **se versiona con el repositorio, no con sufijo de archivo**: hay una sola maqueta vigente por proyecto.

`Root-Rules.md` línea 69, sobre el README raíz:

> El archivo es `README.md` literal, sin versión en el nombre. **El versionado vive en la cabecera del documento** mediante el campo `Versión` […] siguiendo la regla D5.

Hay **dos modelos conviviendo** en el framework: uno con la versión en el nombre y archivado en `_legacy/`, y otro con nombre estable, versión en la cabecera e historia en el repositorio. El segundo cumple D5 sin archivar nada, y su rationale está escrito. H-1 no es más que el punto donde los dos modelos chocan.

### §3.4 El framework ya declara la práctica que hace innecesario el archivado

`Rules-Documentacion.md` línea 119 ancla la categoría 11 en *Docs as Code*: «la documentación versionada y entregada por el mismo flujo que el código, de modo que no pueda derivar de él». La línea 193 lo repite en el perfil del subagente. `Rules-Devops.md` línea 384 da por sentado que el proyecto usa Conventional Commits y tags.

El framework, entonces, ya asume que la documentación vive versionada junto al código. `_legacy/` es una segunda capa de versionado, manual, encima de la que ya se asume.

---

## §4 Qué desaparece y qué sobrevive

De los nueve hallazgos de la revisión:

| # | Hallazgo | Con `_legacy/` eliminado |
|---|---|---|
| H-1 | Colisión al archivar artefactos sin sufijo de versión | **Desaparece.** No hay archivado que colisione |
| H-2 | Cuatro convenciones de ruta de archivado | **Desaparece.** No hay ruta que unificar |
| H-3 | El despacho no dice cuándo tomar el snapshot | **Desaparece la causa.** Editar en el lugar pasa a ser lo correcto; el punto de captura es el commit, y es responsabilidad del orquestador, no del subagente |
| H-4 | Estado `Superado` y nota no llegan a los subagentes | **Desaparece.** No hay copia que etiquetar |
| H-6 | La ruta de §5 pierde el eje de proyecto | **Desaparece** |
| H-9 | El layout no declara `_legacy/` | **Desaparece** la mitad; queda declarar `SDD/Docs/Audit/` |
| H-5 | Cadencia de corrección dentro del ciclo de auditoría | **Sobrevive**, y se abarata: sin archivado, la lectura «cada corrección sube minor» pierde su peor consecuencia (la proliferación de snapshots que nadie leyó) y queda solo la cuestión de identidad del documento |
| H-7 | El re-audit sobrescribe el informe de auditoría | **Sobrevive intacto.** No es un defecto de `_legacy/` sino de un nombre de archivo fijo. R-6 sigue siendo necesaria |
| H-8 | La política no cubre las Fases I y J | **Sobrevive**, y se simplifica: el modelo de `last_review` de §7.2 pasa a ser el modelo general en lugar de una excepción no declarada |

**Seis de nueve desaparecen.** De las ocho reparaciones propuestas en el documento padre, quedan sin objeto R-1, R-2, R-3, R-4 y R-7; siguen vigentes R-5, R-6 y R-8, con R-5 simplificada.

---

## §5 Costo real, y por qué es menor de lo que aparenta

### §5.1 Sí toca D5, y eso normalmente es caro

`README.md` línea 104 enuncia D5: «Un nombre lógico tiene una única versión vigente. **Las superadas se archivan en `_legacy/`**». Eliminar el concepto obliga a reescribir esa segunda oración, y `README.md` línea 122 declara que modificar una invariante «alcanza a los dieciséis archivos de reglas, al orquestador y a toda la documentación ya emitida. Requiere decisión explícita del responsable y nota de coherencia».

### §5.2 Pero la advertencia está calibrada para el caso contrario

`SDD-Development-Guide.md` línea 367 explica por qué el cambio de invariante es caro, con un ejemplo: «Cambiar D3, por ejemplo, invalida el nombre de cada archivo que el framework generó alguna vez».

Ese razonamiento vale para un cambio **restrictivo**: se agrega o se endurece una exigencia, y lo ya emitido pasa a incumplirla. El cambio que se evalúa acá es **permisivo**: se elimina una exigencia. La documentación ya emitida no pasa a incumplir nada; a lo sumo conserva carpetas `_legacy/` que ya no hacen falta, y que se pueden dejar donde están sin que nada quede inconsistente.

Esa asimetría es la que hace que esta modificación de invariante no tenga el costo que la regla general anticipa. Conviene que la nota de coherencia lo argumente explícitamente, porque de otro modo un revisor futuro va a leer la intervención como una violación del procedimiento.

### §5.3 Archivos alcanzados

Casi todos los cambios son **borrado de cláusula**, no reescritura:

| Archivo | Cambio |
|---|---|
| `README.md` línea 104 | Reenunciar D5: una sola versión vigente en el árbol; la historia vive en el control de versiones y en el control de cambios del documento |
| `Master-Prompt.md` líneas 266-267, §8, §13.6 línea 704, §3.5, línea 30 | Reemplazar la política de deprecación por la de retención; quitar el archivado del intake; decidir el caso de `SDD/Docs/` con contenido previo (ver §6) |
| Las diez reglas de categoría | Borrar la cláusula «se mueve a `_legacy/` con estado `Superado` y nota…». Es una a tres líneas por archivo |
| `Root-Rules.md`, `Index-Modelos-UX-UI.md` línea 47 | Ídem |
| `CHANGELOG.md` línea 12, `SDD-Development-Guide.md` línea 125 | Corregir las dos referencias colgadas a la carpeta inexistente. Hay que hacerlo de todos modos |
| `SDD-Getting-Started-Guide.md` línea 387, `PROMPT-Agente-Bootstrap-SDD.md` línea 65 | Alinear con la decisión de §6 |

Son unos trece archivos, contra los seis de la reparación conservadora, pero con una diferencia cualitativa: acá se borra texto y allá se agrega. El framework queda **más chico** y con una regla menos que sostener.

### §5.4 Lo que se pierde, dicho sin adornos

`_legacy/` tiene una virtud que el control de versiones no tiene: **es legible sin herramientas**. Alguien que recibe `SDD/Docs/` en un zip, o que la lee en un portal, ve la carpeta; con git hay que saber usar git y tener el repositorio.

Tres consideraciones sobre ese punto:

1. El lector no técnico no quiere el **texto** anterior: quiere saber **qué cambió y por qué**. Eso vive en la sección de control de cambios del documento vigente, que D5 ya exige en todos, y que la corrida demostró que es lo único que efectivamente sirvió (§3.2).
2. El lector que sí quiere el texto anterior —un desarrollador reconstruyendo una decisión— tiene git y sabe usarlo.
3. Si en algún caso hace falta un estado congelado y distribuible, la respuesta correcta es un **tag** o una release, no una carpeta con copias, porque el tag congela el árbol entero de forma coherente y la carpeta congela archivos sueltos.

---

## §6 Lo que hay que decidir si se elimina

| # | Decisión | Opciones | Recomendación |
|---|---|---|---|
| DEC-04 | Control de versiones como prerrequisito duro del orquestador. Hoy **no** figura: ni `PROMPT-Agente-Bootstrap-SDD.md` §2 ni `Master-Prompt.md` §0 lo mencionan | (a) Prerrequisito bloqueante, verificado antes de la Fase A, con el patrón de la precondición dura de §7.1; (b) recomendado no bloqueante | **(a).** Sin esto, eliminar `_legacy/` sí destruye historia. Es la condición que hace viable todo lo demás, y el framework ya tiene el patrón de precondición dura verificada con evidencia |
| DEC-05 | Punto de captura del estado previo | (a) El orquestador commitea antes de cada despacho de corrección, con mensaje que cite la fase y el hallazgo; (b) se delega al usuario | **(a).** Es el reemplazo directo de R-3 y cuesta lo mismo: una operación por despacho, hecha por el mismo actor. Además produce mejor rastro que el snapshot, porque lleva autor, fecha, mensaje y diff |
| DEC-06 | Qué hacer cuando `SDD/Docs/` del destino ya tiene contenido de una corrida anterior. Es el caso de `Master-Prompt.md` línea 30 y del prompt de entrada línea 65, y **no es la misma operación** que archivar una versión superada | (a) Commitear el estado previo y limpiar el árbol; (b) exigir rama nueva; (c) conservar ahí el único `_legacy/` del framework | **(a).** Con DEC-04 resuelto, es equivalente y no reintroduce el concepto |
| DEC-07 | Qué pasa con D4, el sufijo de versión en el nombre | (a) Se conserva; (b) se elimina también, con la versión viviendo solo en la cabecera, como ya hacen el README raíz y la maqueta | **(a) por ahora.** Es una discusión legítima y de mayor alcance —cada bump renombra el archivo y rompe los enlaces entrantes, que es un costo que `_legacy/` no causaba—, pero mezclarla con esta la vuelve intratable. Una cosa por vez |
| DEC-08 | Qué se hace con los `_legacy/` ya existentes en los repositorios destino | (a) Se dejan donde están, congelados, y se documenta que la política que los produjo se descontinuó; (b) se borran | **(a).** Borrarlos no aporta y arriesga; la evaluación ya registra un caso donde tocar lo archivado empeoró el estado |

---

## §7 Comparación de las dos opciones

| | Opción A — reparar `_legacy/` | Opción B — eliminarlo |
|---|---|---|
| Hallazgos que resuelve | 9 de 9, cada uno con su parche | 6 de 9 por desaparición, 3 con las reparaciones que sobreviven |
| Reparaciones necesarias | R-1 a R-8 | R-5 simplificada, R-6, R-8, más DEC-04 y DEC-05 |
| Archivos con cambio | 6 | ~13, casi todos borrando texto |
| Toca una invariante | No | **Sí, D5** |
| Documentación ya emitida | No se invalida | No se invalida (cambio permisivo, §5.2) |
| Reglas que el framework debe sostener | Una más, más detallada | Una menos |
| Riesgo residual | Alto: la política sigue teniendo cuatro puntos de fallo que dependen de que cada actor la aplique bien en cada corrida | Bajo: no hay operación manual que aplicar mal |
| Condición previa | Ninguna | Control de versiones como prerrequisito duro (DEC-04) |

**Recomendación: Opción B**, condicionada a DEC-04. El argumento decisivo no es el conteo de hallazgos sino la naturaleza del mecanismo: `_legacy/` es una operación manual, repetitiva, sin verificación automática y con fallo silencioso, que duplica algo que la herramienta de abajo ya hace de forma confiable. Los hallazgos H-1, H-3, H-6 y H-7 son cuatro formas distintas del mismo fallo silencioso. Reparar los cuatro deja el mecanismo intacto y la próxima corrida va a encontrar el quinto.

**Si se prefiere Opción A**, la razón defendible sería no querer depender del control de versiones del destino. En ese caso conviene decir eso explícitamente en el framework, porque es la premisa que sostiene toda la política y hoy no está escrita en ninguna parte.

---

## §8 Verificaciones para la Opción B

| # | Verificación | Resultado esperado |
|---|---|---|
| V-11 | `grep -rn "_legacy" /IA/IA.SDD/` después de la intervención | Cero ocurrencias normativas. Solo quedan las históricas del `CHANGELOG.md` y de `Bootstrap/`, marcadas como tales |
| V-12 | Leer D5 y preguntarse qué hace un agente si encuentra dos versiones del mismo nombre lógico en el árbol | La regla dice qué borrar y qué preservar, sin nombrar ninguna carpeta |
| V-13 | Simular el arranque sobre un destino sin control de versiones | El orquestador se detiene y lo informa, como en §7.1 |
| V-14 | Simular una fase rechazada y corregida dos veces | El historial tiene un commit por ronda, cada uno citando su hallazgo, y el árbol tiene un solo archivo por nombre lógico |

---

## Control de cambios

| Versión | Fecha | Cambios | Autor |
|---|---|---|---|
| 1.1 | 2026-07-28 | Se registra la resolución del responsable: eligió la Opción A, reparar y conservar `_legacy/`, y el documento pasa a estado «Evaluado, no adoptado». Se corrige un dato de §3.1: las referencias colgadas a la carpeta eliminada eran una y no dos, porque `CHANGELOG.md` línea 12 registra su baja bajo «### Eliminado», que es lo correcto. El argumento de fondo no cambia. Se deja constancia de que la referencia colgada de `SDD-Development-Guide.md` §2 sí se corrigió como errata dentro de la intervención de la Opción A. | Revisión SDD (Claude Code) |
| 1.0 | 2026-07-28 | Evaluación de la alternativa de eliminar `_legacy/` en lugar de repararlo. Se separa el requisito de D5 (una sola versión vigente en el árbol) del mecanismo elegido para cumplirlo, con la cita del diagnóstico de origen en `Audit-SDD1.md`. Se documenta que el propio framework borró su `_legacy/` en `568a44d` sin pérdida y dejó dos referencias colgadas. Seis de los nueve hallazgos desaparecen; tres sobreviven. Se cuantifica el costo, se argumenta por qué la advertencia de `README.md` línea 122 está calibrada para el caso inverso, y se elevan cinco decisiones nuevas, con DEC-04 —control de versiones como prerrequisito duro— como condición de viabilidad. Recomendación: Opción B. | Revisión SDD (Claude Code) |
