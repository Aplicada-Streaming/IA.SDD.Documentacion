# Plan — Migración normativa de un destino SDD a la versión vigente del framework

| Campo | Valor |
|---|---|
| Documento | `Fix-Orquestador-Sobre-Intake.md` |
| Versión | 1.2 |
| Fecha | 2026-07-29 |
| Estado | **Ejecutado**. El framework se publicó en la 6.0; la nota de coherencia es `Coherencia-Migracion.md` |
| Repositorio a intervenir | `/IA/IA.SDD` (Framework SDD), versión vigente del conjunto: 5.1 |
| Origen | `00-Exploracion.md` (evaluación de las políticas del orquestador) y `01-Planteo.md` (planteo de adecuación y de orquestador de migración) |
| Consumidor | `02-Ejecutar-Fix-Orquestador-Sobre-Intake.md`, que ejecuta este plan en una sesión nueva |

Este documento es el plan. No ejecuta nada. Toda afirmación sobre el estado actual del framework cita archivo y línea.

---

## §1 Qué resuelve este plan

Hoy, cuando el orquestador arranca sobre un destino ya especificado con una versión anterior del framework, la reconciliación normativa (`/IA/IA.SDD/SDD/Devs/Orchestrator/Master-Prompt.md` §2.1, líneas 93 a 157) **compara y ofrece, pero no migra**. Sus tres salidas son: plan sin tocar nada (A), regenerar desde cero archivando (B), o seguir con las reglas viejas (C). No existe salida que lleve el destino de la versión de origen a la vigente preservando su contenido.

Además, la reconciliación **no alcanza a los dos documentos de entrada**. El intake no se reestructura nunca, y el manifiesto solo cambia de forma como efecto colateral de ser un artefacto derivado que se rehace en cada corrida.

Este plan dota al framework de esa capacidad, y responde las tres preguntas del planteo:

1. Que el plan de migración sea ejecutable, y que su alcance incluya intake y manifiesto además de los documentos generados.
2. Si conviene una regla de migración, un playbook por salto de versión, o que el agente use la última versión como principio rector.
3. Si conviene un orquestador de migración contiguo al orquestador de generación.

Las respuestas están en §3, con su fundamento. El resumen es: **sí a un orquestador contiguo, delgado; sí a una regla transversal que tenga la mecánica; no a los playbooks por salto de versión; la última versión como principio rector es el criterio correcto y además es el único que degrada bien cuando falta el conjunto de origen.**

---

## §2 Punto de partida verificado

### §2.1 Lo que ya existe y se reusa

| Pieza | Dónde | Qué aporta a la migración |
|---|---|---|
| Reconciliación normativa | `Master-Prompt.md` §2.1, líneas 93 a 157 | Ya calcula el diff normativo, clasifica saltos por severidad y enumera documentos potencialmente invalidados. Es el diagnóstico; falta el instrumento |
| Bloque de procedencia | `PRODUCT-MANIFEST-template.md` §1.1, líneas 42 a 56 | Declara bajo qué normativa se generó el destino. Es la entrada del diff |
| Archivado por versión | `_legacy/` del framework, con su README | Permite reconstruir el conjunto normativo de origen. Rige desde la 4.0 hacia adelante |
| Estado objetivo declarado | §2.1 (tabla maestra), §2.2 (reglas por tipo), §6 (criterios de aceptación) y anti-patrones de cada `Rules-<Categoria>.md` | Declara exhaustivamente a dónde hay que llegar. No hay que escribir nada nuevo para saberlo |
| Preservación de correcciones manuales | `Master-Prompt.md` §7.2, línea 566, y `SDD-Development-Guide.md` §VI.4 opción 2, línea 538 | El patrón ya resuelto: el agente relee, enumera diferencias, declara cómo las interpretó y espera confirmación |
| Flujo controlado de escritura del intake | `Master-Prompt.md` §13, líneas 837 a 856 | Archivado previo en `SDD/Intake/_legacy/<fecha>/`, entrada en control de cambios, atomicidad por sección, re-derivación del manifiesto |
| Despacho y auditoría | `Master-Prompt.md` §8 (línea 577) y §10 (línea 701) | Esqueleto de despacho y auditor independiente. La migración los cita, no los redefine |
| Prohibición de sustitución global de cadena | `Vocabulario-Rules.md` §9.5, líneas 175 a 183 | La evidencia decisiva contra los playbooks mecánicos por salto de versión |
| Escalera de desambiguación léxica | `Vocabulario-Rules.md` §9.2 a §9.4, líneas 147 a 173 | Gobierna la adopción del término «migración», que ya tiene otros referentes en el framework (§3.5) |

### §2.2 Los cuatro huecos que hay que cerrar antes

Verificados en la exploración. Sin ellos, la migración no tiene sobre qué operar.

| # | Hueco | Evidencia |
|---|---|---|
| H1 | La procedencia no declara la versión de las plantillas de intake y manifiesto. Un cambio estructural de plantilla es invisible para el diff normativo | `PRODUCT-MANIFEST-template.md` §1.1 líneas 46 a 52 lista conjunto, `Master-Prompt`, `Root-Rules`, `Rules-<Categoria>` y transversales, y ninguna fila de plantilla. Las plantillas se versionan aparte: intake 2.1 (`PRODUCT-INTAKE-template.md` línea 3) y manifiesto 3.1 (`PRODUCT-MANIFEST-template.md` línea 3) |
| H2 | El intake y el manifiesto nunca aparecen entre los documentos potencialmente invalidados. El paso 4 del diff enumera artefactos «leyendo su tabla maestra de documentos (§2.1 de la regla)» y `Intake-Rules.md` no tiene §2.1 ni tabla maestra | `Master-Prompt.md` línea 112; `Intake-Rules.md` línea 24, donde §2 son campos bloqueantes. `Intake-Rules.md` subió major (2.1 a 3.0, línea 152) y ningún artefacto quedó marcado |
| H3 | Sobre un destino 4.1 la reconciliación no llega a correr. §2 paso 1 resuelve el producto buscando `PRODUCT-INTAKE-*.md` y §2.1 se evalúa después de esa lectura | `Master-Prompt.md` líneas 81 y 91; `_legacy/4.1/SDD/Devs/Intake/SOLUTION-INTAKE-template.md` línea 7. Falla el prerrequisito 2 de `PROMPT-Agente-Bootstrap-SDD.md` línea 63 y la cadena se detiene antes de §2.1. El framework asume lo contrario en `Coherencia-Vocabulario-Producto-Y-Proyecto-De-Codigo.md` línea 97 |
| H4 | La validación del intake no cubre todo lo que la plantilla agrega. Los campos bloqueantes de cabecera son «nombre del producto, estado» y la plantilla 1.5 agregó Product Owner | `Intake-Rules.md` línea 28; `PRODUCT-INTAKE-template.md` líneas 620 a 623. Es una clase de defecto ya documentada: las reglas de la Parte D vivían en la plantilla desde su 1.3 y ninguna validación las comprobaba hasta `Intake-Rules` 2.1 (línea 151). **Queda fuera de esta intervención por decisión D-4**: se trata en una feature aparte |

---

## §3 Las cinco decisiones de diseño

Las dos preguntas del planteo son ortogonales y conviene decidirlas por separado: **cómo** se calcula la transformación, y **dónde** vive la capacidad. Después se decide **qué alcanza**, **cómo se trata el intake** —que es documento humano— y **cómo se nombra** la capacidad.

### §3.1 Decisión A — cómo se calcula la transformación

| Opción | Descripción |
|---|---|
| **A1 — Playbook por salto de versión** | Un documento de migración por cada par de versiones consecutivas. Los saltos se encadenan: 4.1 a 5.0, 5.0 a 5.1, 5.1 a 6.0 |
| **A2 — Estado objetivo (última versión como principio rector)** | No hay recetas por salto. El agente lee la normativa vigente como especificación del estado al que hay que llegar, lee el documento existente como fuente de contenido, y lo re-expresa. El salto de versión solo sirve para **priorizar**, no para transformar |

**Se adopta A2.** Cinco fundamentos, los cinco verificables:

1. **El framework ya prohíbe la transformación mecánica, por evidencia propia.** `Vocabulario-Rules.md` §9.5 (líneas 175 a 183) prohíbe la sustitución global de cadena y documenta el daño que produjo en la intervención del framework 5.0: treinta ocurrencias de la palabra inexistente «reproducto» en doce archivos, veintitrés cabeceras de tabla de anti-patrones pisadas, catorce etiquetas de cabecera convertidas sobre valores que no correspondían y sesenta filas históricas de control de cambios reescritas. Un playbook por salto es exactamente esa clase de artefacto: describe la migración como operación de texto. Adoptarlo sería reintroducir la práctica que el framework acaba de prohibir con su propio caso como prueba.
2. **El estado objetivo ya está declarado y no hay que escribirlo.** Cada `Rules-<Categoria>.md` declara su tabla maestra en §2.1, sus reglas de inclusión por tipo en §2.2, sus criterios de aceptación en §6 y sus anti-patrones. Eso es la especificación completa del destino. Un playbook la duplicaría, y una duplicación que hay que mantener en paralelo se desincroniza: es el mismo mecanismo de H4, donde la plantilla declaraba una regla y la validación no la comprobaba.
3. **El costo de A1 crece sin techo y su mantenimiento es retroactivo.** Un playbook por salto hay que escribirlo al publicar cada versión, y hay que revisarlo cuando una versión posterior cambia lo que él migraba. Con A2, publicar una versión no genera ninguna deuda de migración.
4. **A2 degrada bien cuando falta el conjunto de origen.** El archivado por versión rige desde la 4.0 hacia adelante y hoy `_legacy/` contiene una sola subcarpeta, `4.1/`. Un destino generado con 3.x no es reconstruible, y §2.1 línea 113 obliga a declararlo como tal. Con A1 no hay migración posible: el playbook necesita conocer el origen. Con A2 la migración sigue siendo posible, porque el objetivo está declarado; lo único que se pierde es la clasificación de saltos, y el efecto es que todo pasa a «revisar» en lugar de discriminarse entre «regenerar» y «sin cambio». Es una degradación de precisión, no una pérdida de capacidad.
5. **El diff que A2 necesita ya está calculado.** §2.1 pasos 1 a 5 (líneas 107 a 113) leen la procedencia, leen las versiones vigentes, clasifican cada salto leyendo la numeración y enumeran los artefactos gobernados por cada regla con salto major. Falta ejecutar, no calcular.

**Lo que A2 no resuelve por sí solo, y cómo se cubre.** El estado objetivo dice a dónde hay que llegar; no dice cómo mapear el contenido viejo cuando una sección desaparece, se parte en dos o se renombra. Para eso no se inventa mecanismo: se reusa el de preservación de correcciones manuales de `Master-Prompt.md` §7.2 línea 566 y de `SDD-Development-Guide.md` §VI.4 opción 2 línea 538. El agente relee, enumera las diferencias, declara cómo interpretó cada una y espera confirmación.

**La única concesión a A1, y es baratísima.** Hay un tipo de cambio que ningún diff de versiones puede inferir: el renombre de un artefacto. Que `SOLUTION-INTAKE` pasó a llamarse `PRODUCT-INTAKE` no se deduce de que `Intake-Rules` haya subido de 2.1 a 3.0. Ese conocimiento vive hoy en prosa en el `CHANGELOG.md` y en las notas de coherencia. La concesión es **un bloque estructurado obligatorio en toda entrada major del `CHANGELOG.md`**, con los renombres de artefacto, las secciones movidas y los campos bloqueantes nuevos. No es un playbook por salto: es un bloque en un archivo que ya existe, que ya es obligatorio en prosa (`SDD-Development-Guide.md` §VI.4 línea 543: «Todo bump major se anota en el `CHANGELOG.md` de la raíz declarando explícitamente el impacto sobre la documentación ya emitida») y que pasa a tener forma legible por el agente.

### §3.2 Decisión B — dónde vive la capacidad

| Opción | Descripción |
|---|---|
| **B1 — Dentro del orquestador de generación** | §2.1 gana una cuarta salida que ejecuta la migración, y el master-prompt absorbe la mecánica |
| **B2 — Regla transversal leída por el orquestador de generación** | La mecánica vive en una regla nueva; el master-prompt la cablea |
| **B3 — Orquestador de migración contiguo** | Prompt de entrada y master-prompt propios, con la mecánica en una regla transversal. §2.1 pasa a ser el ruteador que nombra el instrumento |

**Se adopta B3, con la mecánica en una regla transversal como manda B2.** No son alternativas: B2 es una condición que B3 cumple. Fundamentos:

1. **La mecánica tiene que vivir en una regla, no en un prompt.** Es el principio de delegación de la especialidad, regla rectora del orquestador: «El master-prompt **no asigna** las especialidades de los subagentes. Las **lee** desde la sección §1 de cada `Rules-<Categoria>.md`» (`Master-Prompt.md` líneas 62 a 71). Los cuatro fundamentos que esa sección da —la especialidad es propiedad del documento, un cambio toca un solo archivo, el catálogo evoluciona sin republicar el prompt, el orquestador queda delgado— aplican íntegros a la migración. Esto descarta B1 en su forma fuerte y es no negociable.
2. **Regímenes normativos distintos.** La migración tiene que leer artefactos escritos bajo la normativa de origen y escribir bajo la vigente. El orquestador de generación hoy opera bajo un único régimen, salvo la salida C, que conmuta a `_legacy/`. Meter la migración adentro obliga a que cada una de las fases A a J sepa bajo qué régimen está leyendo y bajo cuál escribiendo. Es superficie condicional nueva en las diez fases.
3. **Orden de arranque: es el argumento decisivo.** La migración del intake y del manifiesto tiene que ocurrir **antes** de que §2 pueda resolver el destino. Sobre un destino 4.1 el orquestador de generación ni encuentra el intake (H3). Un orquestador contiguo que corre primero normaliza el destino y después el de generación arranca bajo una única premisa: la procedencia coincide con la vigente y §2.1 informa «al día» y sigue. B3 **elimina ramas** del orquestador de generación; B1 se las agrega.
4. **Cardinalidad y ciclo de vida distintos.** El orquestador de generación «se ejecuta una sola vez por producto» (`Master-Prompt.md` línea 14). La migración se ejecuta una vez por salto de versión que el destino atraviese, N veces en su vida. Son cardinalidades incompatibles: alojarlas en el mismo prompt obliga a que una de las dos declaraciones mienta.
5. **La objeción a B3 es la duplicación, y se neutraliza.** El riesgo real de un orquestador contiguo es reescribir despacho, auditoría y plan-then-confirm. Se evita por construcción: el master-prompt de migración **cita** §8 (despacho) y §10 (auditoría) del master-prompt de generación en lugar de redefinirlos. Los dos archivos viven en el mismo repositorio, así que la autosuficiencia declarada en el `README.md` línea 144 —ningún archivo referencia otro repositorio— se preserva. El master-prompt de migración queda siendo un archivo de fases, no un segundo orquestador completo.

### §3.3 Decisión C — alcance y orden de la migración

**Alcance: `SDD/Intake/` y `SDD/Docs/`, en ese orden.** El orden no es preferencia: es la cadena D6. El intake es la fuente de verdad (`Master-Prompt.md` línea 839), el manifiesto se deriva de su §13 y los documentos generados se derivan del manifiesto y del intake. Migrar `SDD/Docs/` contra un intake todavía con estructura vieja produce documentación derivada de un upstream superado. El plan de la salida A de §2.1 ya declara que los documentos se ordenan «según la cadena D6» (línea 147); esta decisión extiende esa cadena aguas arriba hasta el intake.

Orden de ejecución: intake, manifiesto derivado, categorías de nivel producto (00, 01), categorías por proyecto de código en orden topológico (02 a 11), consolidación de producto.

**Fuera de alcance, declarado con su razón** — se declara explícitamente para que la migración no se lea como si hubiera cubierto todo:

| Artefacto | Por qué queda afuera |
|---|---|
| `SDD/Maquetas/` | Se versiona con el repositorio y está exento del archivado (`Master-Prompt.md` §5.1, tabla de exenciones, línea 395). Es material ejecutable que el humano edita a mano |
| `/samples/` | Es código, no documentación de especificación |
| `AGENTS.md` | Se regenera completo desde `Contrato-Agentes.md` en cada corrida de la Fase I (`Master-Prompt.md` línea 565). Migrar el contrato alcanza |
| Código fuente del destino | El framework produce documentación de especificación, no código (`PROMPT-Agente-Bootstrap-SDD.md` línea 22) |

**Caso especial: destino posterior al handoff.** Si el destino ya tiene código, la categoría 11 está en el tramo de documentación viva, donde D9 exige que toda afirmación sobre el estado del sistema cite evidencia. Migrar 11 por regeneración plana produciría un cuerpo documental que describe intenciones y se lee como si describiera hechos, que es lo que `Master-Prompt.md` §7.1 línea 553 prohíbe. En ese caso la migración de 11 se enruta por el criterio de re-ejecución de la Fase I (§7.2), no por regeneración.

### §3.4 Decisión D — el intake es documento humano

El Product Owner es el autor responsable del intake y quien lo aprueba; la redacción puede estar asistida por un agente, pero «la autoría del contenido y la aprobación no se delegan» (`PRODUCT-INTAKE-template.md` línea 67). De ahí cuatro restricciones sobre la migración del intake, todas apoyadas en mecánica ya existente:

1. **El agente propone, no sobrescribe.** Emite el intake migrado como propuesta y presenta un diff **de estructura**: qué sección se movió, qué se partió, qué se renombró, qué contenido quedó sin destino. Escribe recién con aprobación explícita.
2. **Nada se inventa.** Si la plantilla vigente exige una sección para la que el intake de origen no tiene contenido, la sección **no se rellena**: se emite como pendiente en la batería consolidada de preguntas de `Intake-Rules.md` §6. Es la conducta que D9 exige y la máquina ya existe.
3. **Escritura por el flujo de §13.** Archivado previo en `SDD/Intake/_legacy/<YYYY-MM-DD>/` (regla 6, línea 853), fila en el control de cambios (regla 3) y re-derivación del manifiesto con nueva confirmación (regla 7, línea 854). El bump es **major**, porque una migración estructural reescribe secciones ya aprobadas y la regla 4 (línea 851) reserva major para ese caso cuando el usuario lo pide.
4. **§13 necesita un tercer caso de escritura autorizado.** Hoy la única escritura permitida es consolidar la respuesta del humano a una ambigüedad de §9 o a la batería de §3 (regla 2, línea 844). La migración estructural es un tercer caso y hay que declararlo, o queda siendo lo que la línea 856 llama error de orquestación que dispara abort.

### §3.5 Decisión E — el término es «migración normativa»

**Decisión tomada:** se adopta «migración» y se renombra también el plan de la salida A, que hoy se llama «plan de adecuación». Esta sección declara el término y su desambiguación, porque el término elegido **ya tiene otros referentes vigentes en el framework** y `Vocabulario-Rules.md` §9.4 (líneas 169 a 173) obliga a verificarlo por ocurrencia antes de declarar cualquier cosa: es afirmación sobre el estado del sistema y cae bajo D9.

**Barrido de ocurrencias de «migra\*» fuera de `_legacy/`.** Tres referentes, dos de ellos preexistentes:

| Referente | Dónde | Naturaleza |
|---|---|---|
| **R1 — La intervención sobre el framework que renombró el vocabulario** | `Vocabulario-Rules.md` líneas 179 y 180 (**prosa normativa vigente**). **Corregido durante la ejecución:** la línea 213, que este plan clasificaba también como prosa vigente, **es la fila 2.0 del control de cambios de §11** y por lo tanto es intocable; contiene dos ocurrencias. Más de veinte filas de control de cambios que dicen «la migración de la 5.0» en `Intake-Rules.md:153`, `PROMPT-Agente-Bootstrap-SDD.md:99`, `PRODUCT-INTAKE-template.md:626`, `PRODUCT-MANIFEST-template.md:242`, `Index-Design-Rules.md:100`, `Index-Modelos-UX-UI.md:84`, `Templates/README.md:100`, `Design-Rules-Identidad-De-Version.md:199`, `Coherencia-Config-Esquema.md:81`, `Coherencia-Roles-Y-Defectos-Verificados.md:145`, `Coherencia-Sustitucion-Lexica-Y-Gobierno-Glosario.md:183, 198, 216` | Preexistente |
| **R2 — Migraciones de datos y de esquema del producto documentado** | `PRODUCT-INTAKE-template.md:147`, `Rules-Arquitectura-Tecnica.md:212` y `:272`, `Rules-Devops.md:504`, `Rules-Documentacion.md:1005`, `Design-Rules-Config-Esquema.md:215`, `Design-Rules-Web-Generico.md:276`, `Design-Rules-Primer-Arranque.md:226`, `Audit-Fase-2.md:238` | Preexistente |
| **R3 — Llevar un destino de la versión de origen a la vigente** | Ninguna: es el referente que esta intervención introduce | Nuevo |

**Por qué se corrió el barrido.** Porque el framework tiene el precedente exacto: la 5.0 descartó «módulo» como término para la unidad de compilación después de que un barrido mostrara que «ya designaba un área funcional del producto en 37 lugares del framework … Adoptarlo habría creado una colisión nueva en el plano de UX y de pruebas en lugar de cerrar una» (`Coherencia-Vocabulario-Producto-Y-Proyecto-De-Codigo.md` línea 93). Omitir el barrido y adoptar el término desnudo repetiría el error que ese precedente evitó.

**Resolución, aplicando la escalera de §9.3 (líneas 159 a 167), que manda usar la forma más barata que resuelva y declarar por qué las anteriores no alcanzaban:**

| Frente | Forma adoptada | Por qué |
|---|---|---|
| **R3 contra R1** | **Forma calificada obligatoria**: el término canónico es **«migración normativa»**, hermano de «reconciliación normativa» que ya existe. La entrada de glosario sola no alcanza porque los dos referentes coexisten en la misma sección: `Vocabulario-Rules.md` §9.5 habla de «la migración del framework 5.0» y es precisamente la sección donde se explica cómo no hacer una migración. El criterio de colisión de §9.2 es la sección, no el documento | Es el segundo escalón, y el primero queda declarado como insuficiente con su evidencia |
| **R1 en prosa vigente** | Se sustituye «migración» por **«intervención»**, término que el framework ya usa para eso en el `README.md` y en `SDD-Development-Guide.md` §VI. Alcanza a las tres ocurrencias de `Vocabulario-Rules.md` (179, 180, 213). **Las filas de control de cambios no se tocan**: `SDD-Development-Guide.md` §VI.2 línea 507 prohíbe reescribir filas ya escritas | El sentido viejo tiene un término mejor y disponible; liberar la palabra es más barato que calificarla en veinte lugares |
| **R3 contra R2** | **Nada.** Los contextos son disjuntos: R2 vive en la documentación técnica del producto (persistencia, devops, config) y R3 en la normativa del framework sobre sus propios destinos. §9.4 prohíbe declarar desambiguación sin colisión verificada, y calificar todas las ocurrencias «empeora el texto sin resolver nada» (§9.5 y el criterio negativo de `Master-Prompt.md` §10) | Se declara explícitamente para que una ronda de auditoría posterior no lo levante como hallazgo |

**Forma desnuda admitida.** Dentro de `Migracion-Rules.md` y del master-prompt de migración, donde no hay otro referente en el contexto de lectura, «migración» puede usarse sin calificar. En todo otro archivo la primera mención de cada sección va calificada. Es el mismo tratamiento que el framework le da a las familias calificadas según §9.2.

**La fase de diagnóstico conserva su nombre.** «Reconciliación normativa» sigue designando la fase §2.1 del orquestador de generación, porque lo que hace es comparar, no transformar. Renombrarla propagaría a ocho archivos sin ganancia conceptual. Lo que sí se renombra, según la decisión, es su salida A: de «plan de adecuación» a **«plan de migración normativa»**.

**Nombres de artefacto:**

| Artefacto | Nombre |
|---|---|
| Regla transversal | `SDD/Devs/Rules/Migracion-Rules.md` |
| Master-prompt de migración | `SDD/Devs/Orchestrator/Master-Prompt-Migracion.md` |
| Prompt de entrada | `PROMPTS/PROMPT-Agente-Migracion-SDD.md` |
| Plan (salida A de §2.1) | `SDD/Docs/Audit/Plan-Migracion-<origen>-a-<vigente>.md`, que reemplaza a `Reconciliacion-<origen>-a-<vigente>.md` |
| Informe de ejecución (fase M6) | `SDD/Docs/Audit/Informe-Migracion-<origen>-a-<vigente>.md` |
| Nota de coherencia | `SDD/Devs/Guides/Coherencia-Migracion.md` |

El nombre corto del archivo de reglas con el término largo en la prosa es el patrón ya establecido: `Deriva-Rules.md` para «sensado de deriva», `Maqueta-Rules.md` para «validación visual de maqueta».

---

## §4 Arquitectura propuesta

### §4.1 Flujo

```text
1. Usuario invoca el orquestador de generación sobre un destino con SDD/Docs/ poblado.
2. Master-Prompt §2 resuelve el destino, tolerando nombres de artefacto legados (fix H3).
3. Master-Prompt §2.1 arma el diff normativo y presenta las tres salidas, ahora
   nombrando el instrumento de la salida A: el orquestador de migración normativa.
4. Salida A: se emite Plan-Migracion-<origen>-a-<vigente>.md y la corrida TERMINA.
   La prohibición vigente se preserva: «ejecutar el plan es una decisión aparte».
5. Usuario invoca PROMPT-Agente-Migracion-SDD.md sobre el destino, con el plan
   como insumo.
6. El orquestador de migración recorre sus fases M0 a M6 y deja el destino
   declarando la versión vigente en su bloque de procedencia.
7. Usuario reinvoca el orquestador de generación. §2.1 informa «al día» y continúa
   a §3 sin preguntar (comportamiento ya existente, línea 104).
```

**Por qué la salida A no encadena automáticamente con la migración.** Porque §2.1 ya declara que al presentar el plan «el orquestador vuelve a detenerse: ejecutar el plan es una decisión aparte» (línea 147). Encadenar automáticamente debilitaría una detención existente. Con este diseño, §2.1 no necesita una cuarta salida: solo necesita **nombrar el instrumento** que ejecuta la salida que ya tiene. Es el cambio más chico que resuelve el problema.

### §4.2 El plan como contrato entre los dos orquestadores

La salida A de §2.1 (línea 147) ya declara un plan con una fila por documento afectado: path, regla que lo gobierna, qué cambió en esa regla, si requiere regeneración o solo revisión, y en qué orden conviene tocarlos según la cadena D6. Ese es exactamente el insumo que el orquestador de migración necesita. Pasa a ser el **contrato** entre los dos: uno lo emite, el otro lo consume. Con tres cambios:

- Nombre: `Plan-Migracion-<origen>-a-<vigente>.md`.
- Filas para el intake y el manifiesto, habilitadas por el fix H2.
- Una columna de **fuente de contenido**: de dónde sale el contenido del documento migrado (el propio documento de origen, un documento hermano, o pendiente de respuesta humana).

Si el usuario invoca la migración sin plan previo, el orquestador de migración lo genera él mismo aplicando §2.1: la dependencia es del artefacto, no de haber corrido el otro prompt.

### §4.3 Reparto de responsabilidades

| Pieza | Responsabilidad | Qué NO hace |
|---|---|---|
| `Migracion-Rules.md` | Toda la mecánica: principio de estado objetivo, clasificación por artefacto, regla de no invención, preservación de contenido, criterios de aceptación, anti-patrones, especialidad de los subagentes en §1.2 | No declara fases ni orden de ejecución |
| `Master-Prompt-Migracion.md` | Fases M0 a M6, detenciones, orden D6, invocación del auditor | No redefine despacho ni auditoría: cita §8 y §10 del master-prompt de generación |
| `Master-Prompt.md` §2.1 | Diagnóstico y ruteo. Nombra el instrumento | No ejecuta la migración |
| `PROMPT-Agente-Migracion-SDD.md` | Modelo de repositorios, prerrequisitos verificables, invocación | No contiene lógica de orquestación, igual que su par de bootstrap |

**Quién re-expresa cada documento.** El subagente de la categoría del documento, leído de §1.2 de su regla, exactamente como en la generación. Es coherente con el fundamento 1 del principio de delegación: «La especialidad es propiedad del documento que se va a generar, no del orquestador» (línea 65). La migración no crea especialidades nuevas.

---

## §5 Fases del orquestador de migración

| Fase | Qué hace | Detención | Salida |
|---|---|---|---|
| **M0 — Reconocimiento del destino** | Resuelve intake y manifiesto tolerando nombres legados. Lee la procedencia. Si no hay procedencia, lo declara y degrada la clasificación de saltos a «revisar todo» (§3.1 fundamento 4) | Sí, si el destino no es reconocible | Bloque informativo de estado del destino |
| **M1 — Diff normativo** | Aplica §2.1 pasos 1 a 5. Consume el plan si existe; si no, lo emite. Sin despachar subagentes | Sí: presenta el plan completo, con intake y manifiesto incluidos, y espera aprobación | `Plan-Migracion-<origen>-a-<vigente>.md` |
| **M2 — Migración del intake** | Propone el intake bajo la plantilla vigente. Diff de estructura. Lo que no tiene fuente va a la batería de `Intake-Rules.md` §6, no se rellena | Sí, doble: aprobación del diff y resolución de la batería | `PRODUCT-INTAKE-<Slug>.md` migrado, major, con archivado previo |
| **M3 — Re-derivación del manifiesto** | Deriva el manifiesto del intake migrado por §3 y §3.1 del master-prompt de generación, con el bloque de procedencia **todavía apuntando al origen** | Sí: confirmación del manifiesto, ya existente en §3 paso 3 | `PRODUCT-MANIFEST-<Slug>.md` migrado |
| **M4 — Migración de `SDD/Docs/`** | Recorre el plan en orden D6: 00, 01, después cada proyecto de código en orden topológico, 02 a 11. Por documento: regenerar contenido si su regla saltó major, revisar si saltó minor, no tocar si no cambió. Preserva correcciones manuales por §7.2 | Sí, por fase, con audit de §10 entre medio | Documentos migrados con su fila de control de cambios y su estado previo archivado |
| **M5 — Cierre de procedencia** | Reescribe el bloque de procedencia con las versiones vigentes, **solo si toda la cadena quedó migrada**. Si algo quedó pendiente, la procedencia no se toca y se declara el estado parcial | Sí | Procedencia actualizada, o declaración de migración parcial |
| **M6 — Auditoría de migración** | Auditor independiente por §10, con los criterios propios de `Migracion-Rules.md` §6 | Sí, bloqueante ante P0 | `Informe-Migracion-<origen>-a-<vigente>.md` |

**Por qué M5 es una fase y no un efecto colateral.** Porque una procedencia que declara la versión vigente sobre un árbol migrado a medias es una afirmación falsa sobre el estado del sistema, y D9 la prohíbe. Reescribirla es el acto que cierra la migración, y tiene que ser condicional y explícito.

**Hallazgos P0 propuestos para el audit de M6:**

- Un documento migrado contiene contenido que no proviene ni del documento de origen ni de una respuesta del humano: es invención.
- Una sección exigida por la normativa vigente quedó rellenada con contenido inferido en lugar de emitida como pendiente.
- La procedencia se reescribió con migración parcial.
- Una corrección manual del usuario fue pisada sin declarar la interpretación y esperar confirmación.
- El estado previo de un documento migrado no quedó archivado en el `_legacy/` de su carpeta.
- Una fila del plan quedó sin resolver y sin declararse como pendiente en el informe.

---

## §6 Prerrequisitos: cinco fixes

Sin estos, la migración no tiene entrada, no tiene alcance sobre el intake y no puede reconocer un destino legado.

| Fix | Qué se hace | Archivo | Bump |
|---|---|---|---|
| **F1 (cierra H1)** | El bloque de procedencia §1.1 suma dos filas obligatorias: versión de `PRODUCT-INTAKE-template` y de `PRODUCT-MANIFEST-template` | `SDD/Devs/Intake/PRODUCT-MANIFEST-template.md` | major, 3.1 a 4.0 |
| **F2 (cierra H2)** | `Intake-Rules.md` gana una §2.1 con la tabla maestra de sus dos artefactos, para que el paso 4 del diff los pueda enumerar | `SDD/Devs/Rules/Intake-Rules.md` | minor, 3.1 a 3.2 |
| **F3 (cierra H3)** | §2 paso 1 tolera nombres de artefacto legados: si no hay `PRODUCT-INTAKE-*.md`, busca los nombres declarados por las versiones archivadas antes de decidir que no hay intake. §2.1 nombra el instrumento de la salida A y renombra su plan. §13 declara el tercer caso de escritura autorizado | `SDD/Devs/Orchestrator/Master-Prompt.md` | minor, 5.1 a 5.2 |
| **F4 (habilita la concesión de §3.1)** | Toda entrada major del `CHANGELOG.md` incluye un bloque «Impacto sobre destinos existentes» con renombres de artefacto, secciones movidas y campos bloqueantes nuevos. Se declara el requisito en la guía | `SDD/Guides/SDD-Development-Guide.md` §VI.4 y §VI.5 | minor, **1.4 a 1.5** (el plan declaraba 1.3 a 1.4; la guía ya estaba en 1.4 al arrancar la ejecución) |
| **F5 (habilita §3.5)** | Entrada de vocabulario para «migración normativa» con sus tres referentes y la regla de forma calificada; y renombre léxico controlado de las ocurrencias de «adecuación» y de «migración» según el inventario de §6.1 | `SDD/Devs/Rules/Vocabulario-Rules.md` más los cuatro archivos del grupo 1 de §6.1 | minor, `Vocabulario-Rules.md` sube minor |

**Sobre el bump de F1.** Sube major porque un manifiesto ya emitido no declara esas filas y por lo tanto deja de cumplir, que es el criterio de `SDD-Development-Guide.md` §VI.1 línea 499: «¿un documento generado con la versión anterior sigue cumpliendo la nueva? Si no, es major».

**Sobre el bump de F3.** Sube minor según las reglas de versionado de `Master-Prompt.md` §16 líneas 961 a 963: no cambia el principio de delegación, ni el flujo plan-then-confirm, ni el conjunto D8, ni los insumos obligatorios, ni la cardinalidad de generación. Es cambio en el flujo de §7 y en la mecánica, que la propia sección clasifica como minor. Queda como decisión abierta (§10, D-2) si el responsable considera que agregar un caso de escritura a §13 amerita major.

### §6.1 Inventario del renombre léxico (obligatorio por §9.5)

`Vocabulario-Rules.md` §9.5 exige, para renombrar un término en un corpus ya escrito: enumerar las ocurrencias, clasificar cada una por sentido, sustituir solo las que cambian de referente, y verificar con un barrido que busque la palabra nueva en contextos donde no puede aparecer. El registro declara cuántas se revisaron y cuántas se cambiaron. El inventario está hecho: **dieciocho ocurrencias de «adecua\*» fuera de `_legacy/`, de las cuales se sustituyen siete.**

**Verificado contra el árbol el 2026-07-29**, antes de sustituir: las dieciocho ocurrencias y su reparto por archivo coinciden exactamente con este inventario, y las siete líneas del grupo 1 son las que la tabla declara.

**Grupo 1 — sentido normativo, se sustituyen (7).** «plan de adecuación» pasa a «plan de migración normativa»:

| Archivo | Línea |
|---|---|
| `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md` | 65 |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 133, 147, 463 |
| `SDD/Guides/SDD-Getting-Started-Guide.md` | 387 |
| `SDD/Guides/SDD-User-Guide.md` | 420, 1450 |

**Grupo 2 — registro histórico, intocable (4).** Filas de control de cambios y entradas de changelog. `SDD-Development-Guide.md` §VI.2 línea 507: «Las filas ya escritas **no se reescriben**, aunque un cambio posterior invalide lo que describen … corregirlas hace que el changelog mienta». Reescribirlas es una de las cuatro clases de daño de la intervención 5.0:

| Archivo | Línea |
|---|---|
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 954 (fila 4.0 de §16) |
| `CHANGELOG.md` | 116, 180, 190 |

**Grupo 3 — sentido no normativo en prosa vigente, no se toca (7).** «adecuado», «adecuada», «Adecuar» como adjetivo o verbo común:

| Archivo | Línea |
|---|---|
| `SDD/Devs/Rules/Rules-Examples.md` | 359 (donde «adecuado» es justamente un ejemplo de palabra vaga prohibida) |
| `SDD/Devs/Rules/Rules-Calidad-Y-Pruebas.md` | 38, 277 |
| `SDD/Devs/Rules/Rules-Devops.md` | 295 |
| `SDD/Devs/Rules/Rules-Documentacion.md` | 63, 244 |
| `SDD/Devs/Guides/Coherencia-Sustitucion-Lexica-Y-Gobierno-Glosario.md` | 146 (cita de un barrido; además es nota de coherencia) |

**Grupo 4 — liberación de «migración» para el referente nuevo (2).** Ocurrencias de R1 en prosa normativa vigente que pasan a «intervención»: `Vocabulario-Rules.md` líneas 179 y 180. Las ocurrencias de R1 en filas de control de cambios quedan intactas por la misma razón que el grupo 2.

**Corregido durante la ejecución (2026-07-29).** Este grupo declaraba **tres** ocurrencias e incluía la línea 213. La verificación contra el árbol que exige §9.5 —y que la Solicitud 5 del tool-prompt obliga a correr antes de sustituir— mostró que **la línea 213 es la fila de la versión 2.0 del control de cambios de §11**, no prosa normativa. Reescribirla habría sido exactamente la cuarta clase de daño de la intervención 5.0 que este plan cita como fundamento. Se sustituyeron dos ocurrencias, no tres.

**Ocurrencia decimonovena, introducida por la propia intervención.** Al declarar el renombre, `Vocabulario-Rules.md` §9.6 menciona el nombre viejo como **objeto** del renombre: «de "plan de adecuación" a "plan de migración normativa"». Es una ocurrencia de «adecua\*» que no se sustituye, porque sustituirla dejaría la oración afirmando que el plan pasó de llamarse igual a como se llama. Queda declarada como grupo propio para que el barrido negativo y las auditorías posteriores no la levanten. El registro final declara **diecinueve revisadas y siete sustituidas**.

**Verificación negativa obligatoria al cerrar.** Barrido de «migración» buscando ocurrencias en contextos donde el referente nuevo no puede aparecer: dentro de artefactos de dominio del producto (R2) y dentro de filas históricas. Si aparece, es una sustitución de más.

### §6.2 Nota sobre H4

H4 —que la validación del intake no cubre todo lo que la plantilla exige— **queda fuera de esta intervención por decisión D-4**. Lo que sí corresponde a este plan es que la migración no la agrave: por eso `Migracion-Rules.md` verifica el intake migrado contra la **plantilla vigente**, no solo contra los campos bloqueantes de `Intake-Rules.md` §2.

Forma recomendada para la feature aparte: **no** un parche puntual que sincronice `Intake-Rules.md` con la plantilla actual, sino una regla que declare el requisito general de que **toda sección obligatoria de una plantilla tenga validación declarada en la regla que la gobierna**. La razón es que la clase de defecto ya ocurrió dos veces de forma independiente —las reglas de la Parte D declaradas en la plantilla 1.3 y sin validación hasta `Intake-Rules` 2.1 (línea 151), y el campo Product Owner agregado en la plantilla 1.5 y todavía sin validar (línea 28)—, así que sincronizar el estado presente deja la puerta abierta a la tercera.

---

## §7 Impacto sobre el framework

### §7.1 Inventario de archivos

| Archivo | Acción | Bump |
|---|---|---|
| `_legacy/5.1/` | Crear: snapshot del conjunto normativo completo antes de la primera modificación | — |
| `SDD/Devs/Intake/PRODUCT-MANIFEST-template.md` | F1: dos filas en §1.1 | major, 3.1 a 4.0 |
| `SDD/Devs/Rules/Intake-Rules.md` | F2: §2.1 nueva con tabla maestra | minor, 3.1 a 3.2 |
| `SDD/Devs/Rules/Vocabulario-Rules.md` | F5: entrada de «migración normativa» con sus tres referentes; grupo 4 del renombre | minor |
| `SDD/Devs/Rules/Migracion-Rules.md` | Crear: decimoctava regla, sexta transversal | 1.0 |
| `SDD/Devs/Orchestrator/Master-Prompt-Migracion.md` | Crear | 1.0 |
| `PROMPTS/PROMPT-Agente-Migracion-SDD.md` | Crear | 1.0 |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | F3 (§2 paso 1, §2.1, §13) y grupo 1 del renombre | minor, 5.1 a 5.2 |
| `SDD/Guides/SDD-Development-Guide.md` | F4, conteo de reglas y transversales, tabla de derivación del conjunto | minor, 1.3 a 1.4 |
| `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md` | Grupo 1 del renombre; prerrequisito 4 nombra el instrumento | minor |
| `README.md` | Anatomía (`PROMPTS/` y `Orchestrator/` con dos archivos), lista de transversales de su línea 92, matriz de ruteo con la fila «Vengo a llevar un destino existente a la versión vigente» | minor |
| `SDD/Guides/SDD-User-Guide.md` | Lista de fases, glosario, árbol de reglas, grupo 1 del renombre | minor |
| `SDD/Guides/SDD-Getting-Started-Guide.md` | Troubleshooting de `SDD/Docs/` con contenido previo (su línea 387), grupo 1 del renombre | minor |
| `SDD/Devs/Guides/Coherencia-Migracion.md` | Crear: nota de coherencia de la intervención | 1.0 |
| `CHANGELOG.md` | Entrada `[6.0]` con su bloque de impacto sobre destinos, formato que F4 introduce | — |

Conteos que quedan desactualizados y hay que corregir en la misma intervención: el `README.md` enumera «diecisiete archivos de reglas … más cinco transversales» (líneas 29 y 92) y pasan a dieciocho y seis. La `SDD-Development-Guide.md` ya arrastra un defecto de este tipo, registrado en su fila 1.3 (línea 576).

### §7.2 Versión del conjunto

**6.0.** Se deriva de la mayor severidad de sus partes, y F1 sube major.

**Ambigüedad detectada y declarada.** La tabla de derivación de `SDD-Development-Guide.md` §VI.5 (líneas 555 a 559) y la fila equivalente del `README.md` (línea 140) hacen subir el conjunto a major «cuando alguna **regla** sube major, o se modifica una invariante». F1 sube major una **plantilla**, no una regla, y ese caso no está contemplado. Se recomienda 6.0 por el criterio sustantivo de §VI.1 —documentación ya emitida deja de cumplir— y **se registra como hallazgo colateral**: la tabla de derivación del conjunto no contempla las plantillas. Corregirla es parte de la etapa E5.

---

## §8 Segmentación en etapas

`SDD-Development-Guide.md` §VI.3 línea 527 exige partir toda intervención de más de tres o cuatro archivos, con nota de coherencia por etapa y confirmación humana entre medio, y con tres condiciones por etapa: dejar el framework consistente y utilizable aunque la intervención se abandone ahí, ser verificable por sí sola, y no definir dos veces el mismo artefacto.

| Etapa | Contenido | Estado consistente al cerrar |
|---|---|---|
| **E0** | Snapshot `_legacy/5.1/`, verificado con `diff -r` como se hizo con la 4.1 | El framework sin cambios, con su conjunto vigente archivado |
| **E1** | F1, F2 y F4. Los tres son mejoras de instrumentación de la reconciliación | La reconciliación existente pasa a ver las plantillas y a enumerar intake y manifiesto. Sin capacidad nueva, pero con diagnóstico completo |
| **E2** | F5: entrada de vocabulario de «migración normativa» y grupo 4 del renombre. Después, `Migracion-Rules.md` 1.0 | El término está declarado antes de usarse, y la regla queda publicada y citable. Ningún prompt la usa todavía: no rompe nada |
| **E3** | `Master-Prompt-Migracion.md` 1.0 y `PROMPT-Agente-Migracion-SDD.md` 1.0 | Capacidad de migración completa e invocable a mano **con M2 detenida**, y la reconciliación todavía sin nombrarla. *Corregido durante la ejecución:* la escritura del intake de M2 depende del tercer caso de escritura de §13, que es F3 y llega en E4; hasta entonces M2 presenta la propuesta, el diff y la batería, y no escribe. El master-prompt de migración lo declara como nota de habilitación en su §1, en lugar de adelantar F3 o de autorizarse a sí mismo |
| **E4** | F3 y grupo 1 del renombre: §2 paso 1, §2.1 y §13 del master-prompt de generación, más las cuatro ocurrencias de «plan de adecuación» en el prompt de bootstrap y las dos guías | Los dos orquestadores integrados: la reconciliación rutea al de migración y reconoce destinos legados |
| **E5** | Propagación: `README.md`, tres guías, conteos, corrección de la tabla de derivación del conjunto de §VI.5, `CHANGELOG.md [6.0]`, `Coherencia-Migracion.md`, verificación negativa del renombre | Framework coherente y documentado en la 6.0 |

El orden E2 antes de E4 no es arbitrario: el término se declara antes de que ningún archivo lo use, para que ninguna etapa intermedia deje el framework citando un término que no existe.

Regla que la guía pide adoptar y este plan adopta (línea 529): un descubrimiento no habilita un cambio. Lo que aparezca durante una etapa y exigiría modificar una etapa cerrada se registra como observación, se reporta y se espera decisión.

---

## §9 Criterios de aceptación de la intervención

Verificables uno por uno al cerrar E5.

- [ ] `_legacy/5.1/` existe, es copia completa del conjunto 5.1 y se verificó con `diff -r` antes de la primera modificación.
- [ ] El bloque de procedencia de `PRODUCT-MANIFEST-template.md` §1.1 declara la versión de las dos plantillas de intake.
- [ ] `Intake-Rules.md` tiene §2.1 con tabla maestra de sus dos artefactos, y el paso 4 del diff de `Master-Prompt.md` §2.1 resuelve contra ella.
- [ ] `Vocabulario-Rules.md` declara «migración normativa» con sus tres referentes, la regla de forma calificada y la constancia de que R2 no se desambigua por contextos disjuntos.
- [ ] `Migracion-Rules.md` declara: principio de estado objetivo, especialidad en §1.2, tabla maestra, regla de no invención, preservación de contenido, criterios de aceptación en §6, anti-patrones y control de cambios en §9. Estructura homóloga a las otras diecisiete reglas.
- [ ] `Master-Prompt-Migracion.md` declara las fases M0 a M6 y **no** redefine despacho ni auditoría: cita §8 y §10 del master-prompt de generación.
- [ ] `Master-Prompt.md` §2 paso 1 resuelve un destino cuyo intake se llama `SOLUTION-INTAKE-*.md` sin detener la cadena, y lo rutea a la migración.
- [ ] `Master-Prompt.md` §2.1 salida A nombra el instrumento que la ejecuta, emite `Plan-Migracion-<origen>-a-<vigente>.md`, y conserva su detención y sus tres prohibiciones intactas.
- [ ] `Master-Prompt.md` §13 declara el tercer caso de escritura autorizado, con su condición de aprobación explícita y su bump major.
- [ ] **Renombre léxico conforme a §9.5**: se revisaron **diecinueve** ocurrencias de «adecua\*» y se sustituyeron siete; ninguna fila histórica de control de cambios fue reescrita; ninguna de las siete ocurrencias no normativas fue tocada; las **dos** ocurrencias de R1 en prosa vigente de `Vocabulario-Rules.md` pasaron a «intervención». El registro declara estos números. *(Cifras corregidas durante la ejecución: ver §6.1.)*
- [ ] **Verificación negativa**: el barrido de «migración» no encuentra el referente nuevo dentro de artefactos de dominio del producto ni dentro de filas históricas.
- [ ] Toda entrada major del `CHANGELOG.md` tiene bloque «Impacto sobre destinos existentes», y la entrada `[6.0]` lo tiene.
- [ ] `README.md` y las tres guías declaran dieciocho reglas y seis transversales, y la matriz de ruteo tiene fila para la migración.
- [ ] La tabla de derivación del conjunto de `SDD-Development-Guide.md` §VI.5 contempla las plantillas.
- [ ] Autosuficiencia preservada: ningún archivo nuevo o modificado de `/IA/IA.SDD` referencia otro repositorio.
- [ ] Ninguna invariante D1 a D9 modificada. Si alguna lo fuera, requiere decisión explícita del responsable y nota de coherencia propia.
- [ ] El caso degenerado sigue produciendo el layout aplanado.
- [ ] `Coherencia-Migracion.md` emitida con alcance, inventario, verificación de invariantes, trazabilidad, observaciones y veredicto, según el patrón de `Coherencia-Auditoria-Marco.md`.
- [ ] Prueba de humo sobre un destino real: un destino 4.1 se reconoce, se le calcula el diff y se le emite el plan de migración con filas para intake y manifiesto.

---

## §10 Decisiones

| Id | Decisión | Estado |
|---|---|---|
| **D-1** | Término de la capacidad | **Resuelta**: «migración», en su forma calificada «migración normativa», y se renombra también el plan de la salida A. La calificación es exigida por la colisión verificada en §3.5, no una reserva sobre la decisión |
| **D-4** | ¿H4 se cierra acá o aparte? | **Resuelta**: aparte. Forma recomendada en §6.2: una regla que exija validación declarada para toda sección obligatoria de una plantilla, en lugar de sincronizar el estado presente |
| **D-2** | ¿F3 sube minor o major? Agrega un caso de escritura a §13 | **Resuelta 2026-07-29: minor**, 5.1 a 5.2. Ninguno de los cinco disparadores de major de `Master-Prompt.md` §16 se toca, y ningún documento generado con la 5.1 deja de cumplir |
| **D-3** | ¿La salida A encadena automáticamente con la migración, o exige invocación nueva? | **Resuelta 2026-07-29: invocación nueva.** Preserva la detención ya declarada en §2.1 y evita la cuarta salida. §2.1 solo nombra el instrumento |
| **D-5** | ¿La migración alcanza destinos sin procedencia declarada? | **Resuelta 2026-07-29: sí**, con clasificación degradada a «revisar todo», declarada, y con prohibición explícita de suponer una versión de origen. Vive en `Migracion-Rules.md` §4.5, y la fila «Sin procedencia» de `Master-Prompt.md` §2.1 se actualizó para ofrecerla |
| **D-6** | ¿Se admite migración parcial como estado final? | **Resuelta 2026-07-29: sí**, con la procedencia intacta y el estado parcial declarado en el informe, sin marca adicional en el manifiesto. Vive en `Migracion-Rules.md` §4.6, y su violación es hallazgo P0 del audit de M6 |

---

## §11 Riesgos

| Riesgo | Mitigación |
|---|---|
| La migración inventa contenido para secciones nuevas | Hallazgo P0 del audit de M6, y regla de no invención en `Migracion-Rules.md`. Lo que no tiene fuente va a la batería de `Intake-Rules.md` §6 |
| La migración pisa correcciones manuales del usuario | Se reusa el patrón de §7.2: releer, enumerar diferencias, declarar interpretación, esperar confirmación. `Master-Prompt.md` línea 569 ya declara por qué importa |
| Los dos master-prompts se desincronizan | El de migración no duplica despacho ni auditoría: los cita. La duplicación que no existe no se desincroniza |
| El destino queda a mitad de camino y nadie lo sabe | M5 es condicional: sin cadena completa no se reescribe la procedencia, y el estado parcial se declara |
| **El renombre léxico se hace por sustitución global de cadena** | Es el riesgo más concreto de esta intervención, porque el término nuevo ya tiene dos referentes vigentes. `Vocabulario-Rules.md` §9.5 lo prohíbe con evidencia propia, y §6.1 de este plan ya provee el inventario clasificado que la regla exige. La verificación negativa es criterio de aceptación |
| El agente que ejecute el plan lea «migración» como transformación por salto | El término va calificado y la regla declara el principio de estado objetivo en su §1. Es la razón por la que la calificación no es cosmética |
| El plan crece y se ejecuta de una sola vez | Segmentación obligatoria de §8, con nota de coherencia y confirmación humana entre etapas |

---

## §12 Control de cambios

| Versión | Fecha | Cambios | Autor |
|---|---|---|---|
| 1.0 | 2026-07-29 | Plan inicial. Sintetiza la exploración de `00-Exploracion.md` y responde el planteo de `01-Planteo.md`: adecuación por estado objetivo en lugar de playbooks por salto de versión, con la evidencia de `Vocabulario-Rules.md` §9.5 como fundamento; orquestador contiguo y delgado, con la mecánica en una regla transversal nueva, por el principio de delegación de la especialidad y por el orden de arranque que hoy impide que la reconciliación corra sobre un destino legado; alcance extendido al intake y al manifiesto en orden D6, con el intake tratado como documento humano; los cuatro huecos de la exploración convertidos en prerrequisitos; segmentación en seis etapas con criterios de aceptación verificables y seis decisiones abiertas | Exploración SDD |
| 1.1 | 2026-07-29 | Resolución de D-1 y D-4. **D-1**: se adopta «migración» y se renombra el plan de la salida A. §3.5 es nueva y documenta el barrido que `Vocabulario-Rules.md` §9.4 exige antes de adoptar un término: «migración» ya tiene dos referentes vigentes en el framework —la intervención que renombró el vocabulario, en prosa normativa de §9.5, y las migraciones de datos del producto documentado—, así que el término canónico pasa a ser la forma calificada «migración normativa», segundo escalón de la escalera de §9.3, con el primero declarado insuficiente y su evidencia. El sentido viejo se libera sustituyéndolo por «intervención», término que el framework ya usa para eso. El sentido de datos no se toca, por contextos disjuntos (§9.4). La fase «reconciliación normativa» conserva su nombre porque compara y no transforma. **D-4**: H4 queda fuera de la intervención, con forma recomendada en §6.2. Se agrega **F5** y el **inventario clasificado del renombre léxico** en §6.1, con las dieciocho ocurrencias de «adecua\*» repartidas en cuatro grupos y la verificación negativa obligatoria, que es lo que §9.5 exige y lo que la intervención 5.0 no hizo. Se renombran los artefactos propuestos y se reordena la segmentación para que el término se declare antes de usarse | Exploración SDD |
| 1.2 | 2026-07-29 | Registro de la ejecución del plan, que publicó el framework en la **6.0**. **Cuatro decisiones resueltas** en §10 antes de tocar ningún archivo: **D-2** minor (5.1 a 5.2), **D-3** invocación nueva, **D-5** sí con clasificación degradada, **D-6** sí con procedencia intacta y estado parcial declarado en el informe. **Tres correcciones al plan detectadas durante la ejecución.** §6.1 y §3.5: el grupo 4 del inventario léxico clasificaba la línea 213 de `Vocabulario-Rules.md` como prosa normativa vigente, y es la fila 2.0 de su control de cambios; el grupo pasa de tres ocurrencias a dos, y se declara además la ocurrencia decimonovena que la propia intervención introduce al mencionar el nombre viejo como objeto del renombre, con lo que el registro final declara diecinueve revisadas y siete sustituidas. §6 y §7.1: el bump de F4 era 1.4 a 1.5 y no 1.3 a 1.4, porque la guía de desarrollo ya estaba en 1.4 al arrancar. §8: la celda de estado consistente de E3 prometía la capacidad «invocable a mano», y la escritura del intake de M2 depende del tercer caso de escritura de §13, que llega en E4; se resolvió con una nota de habilitación declarada en el master-prompt de migración en lugar de adelantar F3. La ambigüedad declarada en §7.2 —la tabla de derivación del conjunto no contemplaba las plantillas— quedó **corregida** en `SDD-Development-Guide.md` §VI.5 y en la fila equivalente del `README.md`. Se cerró además una observación colateral que el plan no había previsto: el bloque de procedencia no podía declarar `Vocabulario-Rules`, pese a que el despacho la inyecta siempre | Framework SDD (migración normativa) |
