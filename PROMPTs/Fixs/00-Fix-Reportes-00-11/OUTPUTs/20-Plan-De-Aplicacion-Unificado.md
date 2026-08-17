# Plan de aplicación unificado

**Documento:** 20-Plan-De-Aplicacion-Unificado.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Insumos:** `01-Catalogo-Y-Agrupamiento.md` y los cinco análisis por familia `10-` a `14-`
**Estado:** Vigente
**Versión resultante del framework:** SDD 7.0 (major)

---

## 1. Decisiones tomadas por el responsable

Las tres que el análisis dejó abiertas, resueltas el 2026-08-15. Se registran acá porque cambian la
severidad del conjunto y el alcance de la migración.

| # | Decisión | Qué se eligió | Consecuencia |
|---|---|---|---|
| D-1 | Ancho de los identificadores | **Cinco dígitos uniformes para todas las familias de catálogo**: `CU-00014`, `EST-00015`, `SD-00374` | Contradice el texto vigente de la invariante D3, que fija dos dígitos. **Obliga a tocar D3**, con lo cual el conjunto sube major |
| D-2 | Ámbito de unicidad | **Producto** | Es la lectura que hace resolver, sin tocarla, la tabla de trazabilidad de `Rules-Necesidades-Negocio.md` §4.4, que cita `CU-XXXXX` sin columna de proyecto de código |
| D-3 | Nivel de aplicación de la categoría 07 | **Mover al nivel producto** los artefactos que hablan del equipo | Major sobre `Rules-Plan-Sprint.md`: la documentación de esa categoría ya generada deja de cumplir |
| D-4 | Clasificación de los criterios de aceptación | **Las diecisiete reglas, en esta corrida** | Amplía el alcance de G4-B respecto de lo que el análisis proponía |
| D-5 | Migración | La migración normativa **lleva la renumeración y el renombre de archivos** | Ítem propio del plan sobre `Migracion-Rules.md` |

**Alcance del ensanche a cinco dígitos, con sus dos exclusiones declaradas.** Se aplica a toda familia
que catalogue elementos de una colección de un producto: `NB`, `CU`, `RN`, `RC`, `ADR`, `US`, `BT`,
`EP`, `TC`, `NFR`, `SUP`, `CMP`, `EST`, `NAV`, `DM`, `SD`, `VER`, `EV`, `EVE`, `ISSUE`, `OPS`, `EXT`,
`STAGE`, `ENV`, `DOD` y equivalentes. Quedan fuera, con su motivo escrito en la regla:

- **`AG-XX`**: designa uno de los doce roles del catálogo de especialidades **del framework**, no un
  elemento de una colección de un producto. Su cardinalidad la fija el propio framework.
- **El ordinal de iteración** (`Sprint-XX`, `S0` a `S9`): es una posición de calendario que el
  roadmap de la categoría 00 numera, no un identificador de catálogo.

**Qué no se reescribe.** Los archivos de `SDD/Devs/Bootstrap/`, las notas de coherencia de
`SDD/Devs/Guides/Coherencia-*.md` y todo `_legacy/`. Son registros de lo que se verificó en un
momento dado, y `README.md` ya declara el principio: «el alcance de lo que una nota verificó no se
toca nunca». Reescribirles la notación sería afirmar que verificaron algo que en ese momento no
existía.

## 2. Orden de aplicación

Las correcciones se ordenan por dependencia entre ellas, no por número de reporte. Cada ola solo
depende de las anteriores.

| Ola | Qué contiene | Por qué va acá |
|---|---|---|
| **1** | Reglas transversales nuevas: sistema de identificadores, datos derivados, apartamiento declarado, referencia pendiente | Las olas 2 a 5 las citan. Sin ellas, las citas no resuelven |
| **2** | Cableado: que las reglas nuevas lleguen a los subagentes y al auditor | Una regla que no viaja en el despacho no existe para quien escribe. Es el defecto que el framework ya reparó una vez, en la 5.1, con `Vocabulario-Rules.md` |
| **3** | Correcciones semánticas por regla de categoría | Consumen lo declarado en la ola 1 |
| **4** | El barrido de notación a cinco dígitos | Va después de lo semántico para que un conflicto de contenido no quede escondido debajo de un cambio masivo de texto |
| **5** | Versionado del conjunto: `CHANGELOG.md`, `_legacy/`, nota de coherencia, migración | Cierra la intervención |

## 3. El plan, ítem por ítem

Cada ítem declara su familia de origen, el archivo, la sección, qué cambia y su severidad. La columna
«Ya emitido» dice si la documentación generada con la versión anterior deja de cumplir por ese ítem.

### Ola 1 — Reglas transversales

| Id | Familia | Archivo · sección | Qué cambia | Sev. | Ya emitido |
|---|---|---|---|---|---|
| P-01 | G1 | `Root-Rules.md` §10 (nueva) | Sistema de identificadores: ámbito de unicidad producto, ancho de cinco dígitos, familias alcanzadas y las dos exclusiones, colecciones derivadas, estabilidad y capacidad enunciadas juntas, titularidad por categoría y prohibición de acuñar por otra | minor en el archivo | Sí, por el ancho |
| P-02 | G2 | `Root-Rules.md` §11 (nueva) | Datos derivados en la prosa: preferir la forma que no cuenta, nombrar la fuente, anclaje que no admite otro referente, registro en el control de cambios | minor | No |
| P-03 | G1 | `Root-Rules.md` §12 (nueva) | Figura de apartamiento declarado: un artefacto obligatorio puede no emitirse con un ADR que lo declare, con alternativas y disparadores de revisión | minor | No |
| P-04 | G3 | `Root-Rules.md` §13 (nueva) | Referencia pendiente: forma, obligación de declarar origen provisorio y momento de cierre | minor | No |
| P-05 | G1, D-1 | `README.md` invariante D3 | El ancho pasa a cinco dígitos y D3 remite a `Root-Rules.md` §10 para el ámbito y las familias | **major del conjunto** | Sí |

### Ola 2 — Cableado

| Id | Familia | Archivo · sección | Qué cambia | Sev. | Ya emitido |
|---|---|---|---|---|---|
| P-06 | G1, G2, G3 | `Master-Prompt.md` §8 | Las cuatro secciones transversales nuevas de `Root-Rules.md` entran en los insumos obligatorios de **todo** despacho, con la misma regla de construcción con que la 5.1 sumó `Vocabulario-Rules.md` | minor | No |
| P-07 | G1 | `Master-Prompt.md` §3.4 | Cuando el manifiesto declara más de un proyecto de código, el orquestador deriva y publica el **mapa de rangos de identificadores** junto al bloque informativo, y lo incluye en cada despacho | minor | No |
| P-08 | G4 | `Master-Prompt.md` §10 | Compuerta mecánica antes del audit con sus tres comprobaciones y la obligación de declarar qué no mira; criterio de corte de rondas; clasificación de hallazgos por detectabilidad; control de recuentos anclados; verificación cruzada de conjuntos cerrados como P0; clase de hallazgo aguas arriba | minor | No |
| P-09 | G3 | `Master-Prompt.md` §7 y §12 | Registro único de decisiones pendientes del producto, exhibido al cerrar cada fase; el bloque de §12 pasa a leerse de ese artefacto | minor | No |
| P-10 | G3 | `Master-Prompt.md` §6 | Reapertura obligatoria que trae el insumo, no solo el turno | minor | No |
| P-11 | G5 | `Master-Prompt.md` §15 | Entran `sonda`, `pasada de diseño`, `pasada de ejecución` y `arnés` en el glosario operativo | minor | No |

### Ola 3 — Reglas de categoría

| Id | Familia | Archivo · sección | Qué cambia | Sev. | Ya emitido |
|---|---|---|---|---|---|
| P-12 | G2 | `PRODUCT-INTAKE-template.md` §20 | Regla de transcripción fiel con sus tres obligaciones, bloque de conteo como forma sugerida y anti-patrón nuevo | minor | No |
| P-13 | G2 | `Intake-Rules.md` §5 y §7 | Coherencia intra-escenario, acotada a conteos y enumeraciones del propio payload; bloqueante si no está declarada | minor | No |
| P-14 | G3 | `Maqueta-Rules.md` §3.5 y §3.6 | Propagación por iteración o diferimiento declarado; regla de escape de la matriz; fila para «la validación creó un proyecto de código»; el `PRODUCT-MANIFEST` entra en la regla de corte; distinción entre propagar y contradecir | minor | No |
| P-15 | G1 | `Rules-Especificacion-Funcional.md` §3.2 y §6 | Nomenclatura y ámbito de los códigos de error | minor | No |
| P-16 | G3 | `Rules-Especificacion-Funcional.md` §3.2 y §4.2 | Marcado explícito de conjuntos cerrados | minor | No |
| P-17 | G5 | `Rules-Especificacion-Funcional.md` §6 y `Rules-UX-UI-DX.md` §6 | Las dos reglas que no lo tienen ganan el criterio de gobierno de glosario, en su forma nueva | minor | No |
| P-18 | G1 | `Rules-Arquitectura-Tecnica.md` §2.1, §2.2, §6 y §7 | Las cuatro menciones del modelo lógico condicionan sobre `tiene_persistencia` del proyecto de código, no sobre el tipo | minor | No |
| P-19 | G1 | `Rules-Examples.md` §0, §2.1, §2.2 y §6 | La obligatoriedad condiciona sobre `redistribuible`; la cláusula de omisión sube de la prosa de §0 a las tablas; el piso de tres samples gana válvula | minor | No |
| P-20 | G4 | `Rules-Examples.md` §6 | Trazabilidad recíproca: qué pasos del flujo recorre la salida prometida; y bloque de discriminación por aserción | minor | No |
| P-21 | G3 | `Rules-Calidad-Y-Pruebas.md` §6 | La fuente única de la condición de terminado sube del prompt-snippet §8 a criterio de aceptación, alcanzando a cualquier artefacto | minor | No |
| P-22 | G5 | `Rules-Calidad-Y-Pruebas.md` §6 | Se retira el noveno destino: `sonda` y `umbral` se citan del glosario operativo; `nivel` y `fixture` siguen en la categoría por no ser del método | minor | No |
| P-23 | G3 | `Rules-Contexto.md` §4.2 y §6 | El acuerdo de equipo referencia la condición de terminado y no la enumera; mientras la 08 no exista, con la forma de referencia pendiente | minor | No |
| P-24 | G1, D-3 | `Rules-Plan-Sprint.md` §2.1, §2.2, §4.2, §4.6 y §6 | `Velocidad-Equipo.md`, la capacidad y las plantillas de ceremonia pasan a nivel producto; la numeración de iteraciones es del roadmap; el criterio de planes mínimos se reformula | **major** | Sí |
| P-25 | G1 | `Vocabulario-Rules.md` §4 R3 | El nivel de aplicación se declara **por artefacto**, en una columna nueva de la tabla maestra §2.1 de cada regla | minor | No |
| P-26 | G1 | Tabla maestra §2.1 de las doce reglas de categoría | Columna de nivel por artefacto | minor | No |
| P-27 | G5 | `Vocabulario-Rules.md` §1 y §8 | Alcance declarado: gobierna los términos que colisionan con el dominio del cliente; el resto del vocabulario del framework vive en el glosario operativo | minor | No |
| P-28 | G5 | §6 de las once reglas que llevan el criterio | Primera cláusula que distingue vocabulario del método y del producto | minor | No |
| P-29 | G4, D-4 | §6 de las diecisiete reglas | Cada criterio marcado como enumerable o interpretativo | minor | No |
| P-30 | G1 | `Deriva-Rules.md` §2.1 y §2.3 | Ancho remitido a `Root-Rules.md` §10; la matriz de sensado declarada como colección derivada que dimensiona sobre la suma de sus fuentes | minor | Sí, por el ancho |

### Ola 4 — Notación

| Id | Familia | Alcance | Qué cambia | Sev. | Ya emitido |
|---|---|---|---|---|---|
| P-31 | G1, D-1 | `Rules/`, `Orchestrator/`, `Intake/`, `SDD/Guides/`, `Devs/Guides/Marco-Teorico-SDD.md` | Barrido de notación: las familias de catálogo pasan de `-XX` a `-XXXXX`, y toda mención de «dos dígitos» pasa a «cinco dígitos». Excluye `AG-XX`, el ordinal de iteración, `Bootstrap/`, las notas de coherencia y `_legacy/` | major del conjunto | Sí |

### Ola 5 — Cierre

| Id | Familia | Archivo | Qué cambia | Sev. |
|---|---|---|---|---|
| P-32 | D-5 | `Migracion-Rules.md` | La migración normativa lleva la renumeración de identificadores y el renombre de los archivos que los llevan en el nombre, con su comprobación de que ninguna referencia queda colgada | major |
| P-33 | — | `SDD-User-Guide.md` y `SDD-Development-Guide.md` | Lo que el usuario ve: el ancho, el ámbito, la compuerta mecánica y el registro de decisiones pendientes | minor |
| P-34 | G4 | `SDD-Development-Guide.md` | Regla de redacción de criterios de aceptación: nombrar los dos lados de una relación, y comprobar que una declaración falsa no acerque el criterio a cumplirse | minor |
| P-35 | — | `SDD/Devs/Guides/Coherencia-Reportes-00-11.md` (nueva) | Nota de coherencia con el patrón de `Coherencia-Auditoria-Marco.md`: alcance, inventario, verificación de invariantes, trazabilidad, observaciones y veredicto | — |
| P-36 | — | `CHANGELOG.md` y `_legacy/6.1/` | Entrada de la versión 7.0 con bloque de impacto sobre destinos existentes, y copia del conjunto normativo superado | — |

## 4. Coherencia entre correcciones

Los cruces que había que resolver de forma explícita, y cómo quedaron:

| Cruce | Riesgo | Resolución |
|---|---|---|
| P-02 (recuentos con fuente) y P-08 (audit los verifica) | Que el audit verifique recuentos sin ancla y produzca el ruido que desactivó al verificador del destino | P-08 alcanza **solo** a los recuentos anclados según P-02. Un número sin ancla es hallazgo contra P-02, y su corrección es reescribir la frase |
| P-08 (compuerta mecánica) y P-29 (clasificación de criterios) | Que la compuerta declare cubrir criterios interpretativos | P-29 es insumo de P-08: la compuerta cubre la suma de los enumerables y **declara qué no mira** |
| P-04 (referencia pendiente) y P-28 (vocabulario del método) | Que el vocabulario técnico del producto se mande al `Glosario-Tecnico.md` de la 11, que seis categorías referencian desde fases anteriores | P-28 manda el vocabulario **del método** al glosario operativo, que existe desde el arranque. El del producto sigue yendo a la 11 y usa la referencia pendiente de P-04. Por eso G5 depende de G3 |
| P-18/P-19 (obligatoriedad por flag) y P-03 (apartamiento) | Que el apartamiento se use para evadir una obligación que la condición nueva ya resuelve | P-03 es la salida para lo que la condición **no** contempla. Donde hay flag, la obligación se resuelve por flag y el ADR no aplica |
| P-24 (mover artefactos de la 07) y P-26 (columna de nivel) | Que la columna quede declarando el nivel viejo | P-26 se aplica después de P-24 en la categoría 07 |
| P-31 (barrido) y todo lo demás | Que un conflicto semántico quede escondido bajo un cambio masivo de texto | El barrido va en su propia ola, después de lo semántico, y su verificación es un recuento antes y después |

## 5. Versionado del conjunto

`README.md` §Reglas de intervención: «la versión del conjunto se deriva de la mayor severidad de sus
partes: major si alguna regla o alguna plantilla de intake sube major, o **se toca una invariante**».

Se toca D3 (P-05) y sube major `Rules-Plan-Sprint.md` (P-24) y `Migracion-Rules.md` (P-32). El
conjunto pasa a **SDD 7.0**, con entrada en `CHANGELOG.md`, bloque «Impacto sobre destinos
existentes» y copia del conjunto normativo superado en `_legacy/`.

## 6. Qué queda declarado como trabajo siguiente

No es lo que no se pudo: es lo que esta intervención decidió no hacer, con su motivo escrito.

| Pendiente | De qué familia | Por qué no va en esta intervención |
|---|---|---|
| Correr la comprobación del grafo de obligaciones sobre las diecisiete reglas y tratar cada caso | G3 | Se aplica el mecanismo; cada obligación detectada exige decidir si se administra, se reordena o la regla estaba mal, y eso es una intervención por caso |
| Inventario completo del vocabulario propio del framework | G5 | Se incorporan los cuatro términos que la corrida encontró. La pasada completa produce la lista real |
| Adelantar la condición de terminado a la Fase A | G3 | Mueve contenido entre dos categorías y toca el orden de fases que los tres reportes declaran correcto |
| Decidir si el «glosario de categoría» es un artefacto real | G5 | Hoy está definido en el glosario operativo y no lo emite ninguna regla |
| Que un recuento en prosa sea afirmación bajo D9 | G2 | P-02 obtiene el mismo efecto sin ampliar el alcance de D9, hoy acotado a afirmaciones sobre el estado del sistema |
| Reprocesar los recuentos de la documentación ya emitida | G2 | Misma razón por la que D9 no es retroactiva: ahogaría los hallazgos reales |
