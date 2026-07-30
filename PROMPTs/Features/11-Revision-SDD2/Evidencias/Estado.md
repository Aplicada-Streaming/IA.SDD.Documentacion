# Estado — Definición de solución, producto, solución de código y proyecto de solución

| Campo | Valor |
|---|---|
| Análisis | Cómo quedaron definidos los conceptos de solución, producto, solución de código y proyecto de solución |
| Alcance | `IA.SDD` — plantillas de intake (`SOLUTION-INTAKE-template`, `SOLUTION-MANIFEST-template`) y orquestador (`Master-Prompt`) |
| Versión del framework analizada | 4.0 (entrada vigente del `CHANGELOG.md`) |
| Fecha | 2026-07-29 |
| Estado | Informe de análisis, sin cambios aplicados |

Fuentes leídas: `README.md`, `SDD/Devs/Intake/SOLUTION-INTAKE-template.md` (1.4), `SDD/Devs/Intake/SOLUTION-MANIFEST-template.md` (2.0), `SDD/Devs/Rules/Intake-Rules.md` (2.1), `SDD/Devs/Orchestrator/Master-Prompt.md` (4.0), `SDD/Devs/Rules/Rules-Contexto.md` (2.0), `SDD/Guides/SDD-User-Guide.md`, `SDD/Guides/SDD-Getting-Started-Guide.md`, `CHANGELOG.md`.

Este documento se produjo en pasadas sucesivas. La §1 a la §3 relevan cómo quedaron definidos los conceptos en el árbol vigente. La §4 contrasta ese estado contra el modelo de niveles que plantea el responsable del framework: un producto de software con nombre propio, conocido por el Product Owner, distinto de la solución de código, que se divide en proyectos de código, que se dividen en espacios de nombres. La §5 desarrolla el rol de Product Owner como autor del intake, su reparto entre agente humano y agente de IA, la frontera del framework, y la distinción entre Product Owner y stakeholder con su consecuencia sobre el comportamiento de AG-00. La §6 consolida las decisiones abiertas, la §7 propone el plan de intervención y la §8 evalúa qué especialidad debe intervenir en cada categoría.

---

## §1 Los cuatro conceptos

### §1.1 Solución — definida, con definición canónica única

Es el único de los cuatro que quedó cerrado en un glosario normativo. `Master-Prompt.md` §15 la define como «contenedor raíz del entregable que agrupa una jerarquía de proyectos. No tiene un valor D8 propio». La `SDD-User-Guide.md` §10.1 repite la misma fórmula con N ≥ 1, y el `README.md` la enuncia igual en su segundo párrafo. Las tres formulaciones coinciden.

Lo que la sostiene operativamente es que la solución es la unidad de tres cosas a la vez:

- Un solo negocio. La Parte A del intake (§1 a §12) es de nivel solución: «el negocio es uno» (`SOLUTION-INTAKE-template.md`, descripción de la Parte A).
- Un solo intake y un solo manifiesto derivado de él.
- Un solo árbol de salida, con las categorías 00 y 01 más `Solucion/` y el README raíz a nivel solución (`Master-Prompt.md` §3.5).

La solución no tiene tipo: el valor D8 lo declaran sus proyectos. Ese es el corte conceptual que introdujo la unificación de intake y es coherente en los tres artefactos analizados.

### §1.2 Proyecto de solución — definido, y es el que carga toda la semántica

`Master-Prompt.md` §15 lo define como «nodo de la jerarquía con exactamente un valor D8. Unidad de especialización de los subagentes y de generación de las categorías 02 a 11».

Su identidad tiene seis campos fijos, idénticos en las tres fuentes (intake §13, manifiesto §2 y bloque informativo del orquestador §3.4):

| Campo | Plano | Origen |
|---|---|---|
| `Nombre-Proyecto` | Documentación | Declarado en intake §13 |
| `nombre-proyecto-codigo` | Código | Derivado por el orquestador |
| `project_type` (D8) | Ambos | Declarado en intake §13, conjunto cerrado de 8 |
| Rol en la solución | Documentación | Declarado en intake §13 |
| `redistribuible` | Código | Declarado en intake §13 |
| Dependencias | Ambos | Declaradas en intake §13, validadas |
| Path `/src` | Código | Derivado del nombre de código |

Sobre el conjunto de proyectos rigen cinco validaciones bloqueantes, repetidas idénticas en `SOLUTION-MANIFEST-template.md` §4, `Intake-Rules.md` §4 y `Master-Prompt.md` §3.1:

1. Cada `project_type` pertenece al conjunto cerrado D8.
2. Hay exactamente un proyecto principal.
3. No hay colisión de `Nombre-Proyecto` ni de `nombre-proyecto-codigo`.
4. Cada dependencia referencia un proyecto existente.
5. El grafo de dependencias es acíclico.

De ahí sale el orden topológico, que gobierna tanto la generación de documentación como el orden de build.

El caso de un solo proyecto quedó nombrado como caso degenerado: una sola fila en el manifiesto, layout aplanado sin `Proyectos/` ni `Solucion/`, y se declara explícitamente como la garantía de no ruptura con el template de tipo único anterior.

### §1.3 Solución de código — no quedó definida como concepto

No existe el término en ningún glosario, ni en el manifiesto, ni en el master-prompt. Una búsqueda por `.sln`, «solución de código» o «solution file» sobre todo el repositorio no devuelve ninguna coincidencia.

Lo que sí quedó es un plano de código, definido por derivación y no por declaración. Se nombra una sola vez y en un lugar secundario, `SDD-User-Guide.md` §5.2: «esta convención aplica solo al plano de código en `/src`; el plano de documentación sigue en Título-Con-Guiones». Sus piezas son:

- `NombreSolucionCodigo` en PascalCase, derivado del nombre legible de la solución.
- `<NombreSolucionCodigo>.<Sufijo>` por proyecto, con el sufijo orientado por el `project_type`. El mapa de sufijos del manifiesto §2.1 es explícitamente orientativo, no cerrado.
- Prefijo de organización `Aplicada` para los proyectos con `redistribuible: true`, para darle al paquete un espacio de nombres estable e independiente de la solución que lo consume.
- `src/<nombre-proyecto-codigo>/` como path de cada proyecto.
- El orden topológico como orden de build.

En síntesis: el framework define cómo se llaman las cosas en el plano de código, pero nunca declara que exista un artefacto «solución de código» (un `.sln`, un workspace, un archivo agregador). No hay ninguna regla que lo emita ni que lo valide. Queda como consecuencia implícita del conjunto de paths y nombres derivados.

### §1.4 Producto — no quedó definido, y sobrevive como residuo

El término `producto` no está en ningún glosario del framework, pero aparece con dos sentidos incompatibles entre sí:

1. Sinónimo tácito de solución. La categoría 00 se llama «Contexto del producto» (`Rules-Contexto.md`, título) y emite `Vision-Producto.md` y `Roadmap-Producto.md`, pero su nivel de aplicación declarado en el `README.md` es Solución, y se genera una vez por solución. Es decir: «producto» es la solución mirada desde el eje de valor y visión, pero eso no está dicho en ninguna parte.
2. El sistema real construido, por oposición a la maqueta y a la documentación: «no es el producto ni documentación viva: es la línea de base de un momento» (`Master-Prompt.md` §15, entrada Maqueta).

Es vocabulario heredado de la era pre-solución (proyecto único) que la unificación de intake no barrió. No rompe nada operativamente, porque la categoría 00 tiene su nivel declarado en el `README.md` y en `Rules-Contexto.md` §1.2, pero es el único de los cuatro conceptos que quedó sin dueño normativo.

> **Reconsideración posterior.** Esta sección se escribió en la primera pasada, cuando el análisis daba por sentado que «producto» era un sinónimo redundante de «solución» y la salida natural parecía ser eliminarlo, renombrando la categoría 00 a «Contexto de la solución». **Las pasadas siguientes lo invierten.** Bajo el modelo de §4, el producto no es un residuo a barrer: es el **nivel superior que falta**, con nombre propio, dueño declarado (el Product Owner, §5) y ciclo de vida distinto del de la solución de código. La categoría 00 se llama «Contexto del producto» con razón: produce visión, alcance y roadmap, que son artefactos de producto y no de código.
>
> Lo que sigue siendo cierto de esta sección es el **diagnóstico**: el término no está definido en ningún glosario y se usa con dos sentidos incompatibles. Lo que cambia es la **salida**: no se elimina el término, se lo define como concepto de primer nivel y se desambigua el segundo sentido —el sistema construido, por oposición a la maqueta— con otra expresión. Ver §7.3, intervención F.

---

## §2 Reparto de la definición entre los tres artefactos

| Artefacto | Versión | Qué le quedó como propiedad |
|---|---|---|
| `SOLUTION-INTAKE-template.md` | 1.4 | El origen. Único documento que completa el humano. Parte A negocio (nivel solución), Parte B composición (§13 es la fuente del manifiesto), Parte C técnica repetida por proyecto, Parte D anexos de datos. Declara el perfil de convención de nombres |
| `SOLUTION-MANIFEST-template.md` | 2.0 (según su control de cambios) | El formato del derivado. Dejó de ser plantilla a llenar y pasó a ser referencia de formato: bloque de solución, §1.1 procedencia del framework, §1.2 perfil de nombres, tabla de proyectos, grafo, validaciones bloqueantes y caso degenerado |
| `Master-Prompt.md` | 4.0 | El procedimiento y el vocabulario. §3.2 algoritmo determinista de normalización, §3.3 orden topológico, §3.4 bloque informativo, §3.5 layout de salida, y sobre todo §15, que es la fuente canónica de las definiciones |

El reparto es limpio en intención: el intake declara, `Intake-Rules.md` §4 valida y mapea campo a campo, el orquestador deriva y el manifiesto describe el formato del resultado. La cadena tiene confirmación humana obligatoria entre derivar y tratar el manifiesto como canónico.

---

## §3 Fricciones detectadas

Cinco hallazgos verificados contra el árbol vigente, ordenados por impacto.

### §3.1 Contradicción de caso en `Nombre-Solucion` y `Nombre-Proyecto`

La invariante D3 exige Título-Con-Guiones con «cada palabra capitalizada», y el algoritmo de `Master-Prompt.md` §3.2, paso 4, dice «capitalizar la inicial de cada palabra». Pero todos los ejemplos del framework usan minúsculas: `gestion-de-turnos`, `parser-csv`, `gestion-de-turnos-api`. Y `SDD-User-Guide.md` §4.1 lo formaliza al revés: «Título-Con-Guiones (minúsculas, sin acentos, guion medio como separador)», que es una definición que se contradice a sí misma.

La contradicción se propaga al nombre de archivo. `SOLUTION-MANIFEST-template.md` §5 declara `Nombre-Solucion` = `gestion-de-turnos` y en la fila siguiente cita el intake de origen como `SOLUTION-INTAKE-Gestion-De-Turnos.md`, cuando el patrón declarado es `SOLUTION-INTAKE-<Nombre-Solucion>.md`. Con el valor declarado, el archivo debería llamarse `SOLUTION-INTAKE-gestion-de-turnos.md`. El §6 repite el defecto con `parser-csv` y `SOLUTION-INTAKE-Parser-CSV.md`.

Consecuencia: un orquestador que aplique §3.2 al pie de la letra produce nombres distintos de los que los ejemplos muestran, y distintos de los que la guía de usuario indica escribir a mano en el paso de copia del intake.

### §3.2 La plantilla del manifiesto no declara su versión en cabecera

`SOLUTION-INTAKE-template.md` declara «Versión de la plantilla: 1.4» en su cabecera desde su versión 1.3, y su control de cambios registra que eso «corrige una aplicación incompleta de D6 sobre las plantillas». La corrección se aplicó a una plantilla y no a la otra: `SOLUTION-MANIFEST-template.md` solo tiene su 2.0 en el control de cambios del pie y su cabecera arranca directo en prosa, sin campo `Versión`.

Bajo D4 vigente es la misma clase de defecto que la versión 4.0 vino a cerrar: un archivo cuya versión no es legible desde su cabecera.

### §3.3 Rutas obsoletas `rules/`

Cuatro ubicaciones citan `rules/Intake-Rules.md` cuando la ruta real es `SDD/Devs/Rules/Intake-Rules.md`:

| Archivo | Ubicación |
|---|---|
| `SOLUTION-MANIFEST-template.md` | Párrafo introductorio y fila 2.0 del control de cambios |
| `Master-Prompt.md` | §3, paso 1 de la fase de validación de intake |
| `Marco-Teorico-SDD.md` | Descripción de la fase de validación y diagrama de fases |

La del master-prompt es la más relevante: cae justo en el primer paso de la fase de validación de intake. El mismo defecto afecta la cita a `rules/Maqueta-Rules.md` en el marco teórico.

### §3.4 El árbol de ejemplo del intake §16 no coincide con el layout del orquestador

`SOLUTION-INTAKE-template.md` §16 muestra `docs/` y `devs/Intake/` en minúscula, mientras `Master-Prompt.md` §3.5 manda `SDD/Docs/` y `SDD/Intake/`. Es el ejemplo que el usuario copia para armar su propia §16, así que el defecto se propaga a cada intake real.

### §3.5 Redundancia de la convención de nombres, declarada en cuatro lugares

La regla de derivación de nombres de código está escrita en cuatro archivos: intake §13, manifiesto §1.2 más §2.1, `Intake-Rules.md` §4 y `Master-Prompt.md` §3.2. Las cinco validaciones bloqueantes están escritas tres veces (manifiesto §4 y §7, `Intake-Rules.md` §4, `Master-Prompt.md` §3.1).

Hoy todas las copias dicen lo mismo, así que no es un defecto sino una superficie de deriva. Es exactamente el patrón que produjo el problema que la versión 4.0 corrigió en el versionado: dos lógicas conviviendo, con el defecto apareciendo en la frontera entre ambas.

---

## §4 Contraste contra el modelo de cuatro niveles

### §4.0 El modelo planteado

El responsable del framework plantea esta cadena, que arranca en el rol de Product Owner:

1. El **Product Owner** conoce el producto y redacta el primer documento de intake sobre la plantilla existente.
2. El **producto de software** tiene un nombre propio, y es lo que el PO conoce.
3. El producto se distingue de la **solución de código**, que es otra cosa.
4. Una solución de código se divide en **proyectos de código**.
5. Un proyecto de código se divide en **espacios de nombres**.

Contra eso, lo que el framework tiene hoy es una cadena de dos niveles repartida en dos planos:

| | Plano documental | Plano de código |
|---|---|---|
| Nivel superior | `Nombre-Solucion` | `NombreSolucionCodigo` |
| Nivel inferior | `Nombre-Proyecto` | `nombre-proyecto-codigo` más `src/<x>/` |

Y un único nombre humano, «Nombre de la solución» en la cabecera del intake, del que se derivan los cuatro identificadores de la tabla, más el nombre del repositorio destino, el del repositorio `.Documentacion` y el del archivo de intake.

Los cuatro apartados siguientes son los puntos donde el modelo planteado no entra en el árbol vigente.

### §4.1 El producto no tiene nombre propio

Hoy el nombre del producto y la raíz del plano de código son forzosamente la misma cadena: `NombreSolucionCodigo` se obtiene en PascalCase del nombre legible de la solución (`Master-Prompt.md` §3.2).

Funciona mientras el producto se llame igual que su raíz de código. Se rompe apenas el producto adopta un nombre comercial y la raíz de código tiene que quedarse quieta por compatibilidad con sus consumidores. Son dos nombres con ciclos de vida distintos: el del producto lo cambia el PO cuando el negocio lo pide, el de código no se cambia nunca sin romper referencias.

Consecuencia práctica: hoy no hay dónde escribir el nombre del producto sin arrastrar el renombre a todo el plano de código, al repositorio destino y al nombre del archivo de intake.

### §4.2 `NombreSolucionCodigo` no es una solución de código: es una raíz de espacio de nombres

Lo que el campo hace realmente es prefijar el nombre de cada proyecto (`GestionDeTurnos.WebApi`). El framework no emite en ningún momento un archivo de solución ni ningún artefacto agregador equivalente: no hay regla que lo produzca ni que lo valide.

La prueba está en la excepción de los redistribuibles. `SOLUTION-MANIFEST-template.md` §2.1 justifica el prefijo `Aplicada` porque un paquete reusable «necesita un espacio de nombres estable e independiente de la solución que lo consume». Esa frase es el framework razonando a nivel de espacio de nombres mientras llama «solución» a lo que está manipulando.

Es decir: los niveles 3 y 5 del modelo planteado (solución de código y espacio de nombres) están hoy colapsados en un único campo, y el nombre que lleva ese campo corresponde al nivel equivocado.

### §4.3 El 1:1 entre proyecto SDD y proyecto de código se rompe en el propio mapa de sufijos

`SOLUTION-MANIFEST-template.md` §2.1 declara, para `web-microservices`: «un proyecto por servicio bajo `<NombreSolucionCodigo>.Services.<Servicio>` más `.Gateway` y `.BuildingBlocks`». Eso son N proyectos de código bajo **una sola fila** del manifiesto, y esa fila tiene un único campo `nombre-proyecto-codigo` y un único path `src/`.

El caso `library` tiene el mismo problema: «`.Core`, `.Abstractions`, `.Domain`, `.Infrastructure` u otro rol» describe los proyectos de código que una librería real tiene a la vez, no cuatro alternativas entre las que elegir una.

La tabla de proyectos del manifiesto no puede expresar lo que su propia §2.1 describe. Es la evidencia más fuerte de que falta el nivel de proyecto de código como entidad enumerable, y no como campo derivado de una sola línea.

### §4.4 El proyecto SDD no es un proyecto de código

La definición canónica del proyecto SDD es «unidad de especialización de los subagentes y de generación de las categorías 02 a 11», y lleva exactamente un valor D8.

Un proyecto de código no tiene D8. Lo tiene una unidad entregable y desplegable: `rest-api`, `cli-tool`, `worker-service` describen qué se publica y cómo se opera, no qué se compila. El proyecto SDD es, en rigor, un **componente entregable**, y se proyecta sobre 1..N proyectos de código.

Este es el punto conceptual central del contraste: la cadena planteada es enteramente del plano de código, y el proyecto SDD vive en el plano documental. La corrección no es renombrar el proyecto SDD a «proyecto de código», sino declarar la proyección de uno sobre otro.

### §4.5 Mapeo resultante

| Nivel del modelo planteado | Hoy en SDD | Plano | Estado |
|---|---|---|---|
| Producto de software (nombrado, del PO) | «Nombre de la solución», cabecera del intake | Negocio | Existe el dato, no el concepto ni el nombre propio |
| (sin equivalente en el modelo) | **Proyecto SDD**: unidad D8, categorías 02 a 11 | Documentación | Definido y canónico |
| Solución de código | `NombreSolucionCodigo` | Código | Existe como prefijo de nombres, no como artefacto |
| Proyecto de código | `nombre-proyecto-codigo` más `src/<x>/` | Código | Existe, forzado 1:1 con el proyecto SDD |
| Espacio de nombres | Solo en la justificación del prefijo `Aplicada` | Código | No existe como concepto |

### §4.6 Qué implicaría aplicar el modelo

| Cambio | Archivos afectados | Severidad |
|---|---|---|
| Separar «Nombre del producto» (negocio, mutable) de `Nombre-Solucion` y `NombreSolucionCodigo` (estables), declarando cuál se puede cambiar y cuál no | Cabecera de `SOLUTION-INTAKE-template`, bloque de solución del manifiesto, `Master-Prompt.md` §3.2 y §3.4 | Minor |
| Admitir 1..N proyectos de código por proyecto SDD en la tabla de composición | `SOLUTION-INTAKE-template` §13, `SOLUTION-MANIFEST-template` §2, `Intake-Rules.md` §4, `Master-Prompt.md` §3.1, §3.2 y §3.4 | **Major**: cambia el esquema de la tabla y la documentación derivada con el esquema anterior deja de cumplir |
| Renombrar el concepto de §2.1: lo que hoy se llama raíz de solución es raíz de espacio de nombres | `SOLUTION-MANIFEST-template` §2.1, `SDD-User-Guide.md` §5.2 | Minor |
| Cuatro entradas nuevas de glosario más la aclaración de que el proyecto SDD no es el proyecto de código | `Master-Prompt.md` §15, `SDD-User-Guide.md` §10.1 | Minor |
| Que el árbol de §16 muestre la solución de código y sus proyectos, y no solo `src/<x>/` plano | `SOLUTION-INTAKE-template` §16 | Minor |

El conjunto cerrado D8 **no se toca**: sus ocho valores describen componentes entregables, que es exactamente el nivel al que el proyecto SDD pertenece.

---

## §5 El Product Owner como autor del intake

### §5.1 «Propietario» no es una traducción de Product Owner, y ahí está el problema

La primera lectura de este informe registró tres relatos en competencia sobre el origen del intake. La aclaración del responsable los reduce a uno y reencuadra el hallazgo: **quien redacta el intake es el Product Owner, porque es quien conoce el producto al detalle**, y la pregunta bloqueante de `SOLUTION-INTAKE-template` §2 —«¿Quién es el propietario del problema y aprueba el intake?»— está nombrando a esa misma persona con una traducción.

La verificación contra el árbol vigente muestra que el problema es más específico que una traducción desprolija. `propietario` en SDD **no es** hoy el nombre del rol: es una de las tres etiquetas de una taxonomía de stakeholders —propietario, implementador, beneficiario— que aparece en cuatro archivos (`SOLUTION-INTAKE-template` §2, `Rules-Contexto.md` §5 y §6, `Rules-Necesidades-Negocio.md` §3 y §6). La categoría designa a quien posee el problema, paga o decide el rumbo; el Product Owner cae dentro de ella, pero no la agota: un sponsor que financia también es «propietario» y no es el PO.

Y hay un detalle que agrava la ambigüedad: la pregunta dice «propietario **del problema**», no del producto. Son dos cosas distintas. El Product Owner es dueño del producto y de su backlog; el dueño del problema es una categoría de stakeholder. La pregunta bloqueante los está fusionando en una sola respuesta.

### §5.2 El framework ya usa «Product Owner» sin traducir, salvo en el intake

Este es el dato que cierra la discusión sobre si conviene traducir. Cinco archivos de reglas usan el término en inglés, tal como lo fija la bibliografía de Scrum:

| Archivo | Uso |
|---|---|
| `Rules-Backlog-Tecnico.md` §1.2 | «Scrum Master + Product Owner» (desktop-app), «Scrum Master + API Product Owner» (rest-api) |
| `Rules-Contexto.md` §1.2 | «Product Manager + API Product Owner» (rest-api) |
| `Rules-Plan-Sprint.md` | «AG-06 Product Owner / Backlog»; «feedback del Product Owner» |
| `Rules-Necesidades-Negocio.md` | «Nombrar rol específico (Product Owner, Recepcionista, Auditor)» |
| `Marco-Teorico-SDD.md` | Tabla de correspondencias con Scrum: «Product Owner → AG-00 (Product Manager)» |

O sea: el framework ya decidió no traducir el rol, y lo aplica en todas partes menos en el único documento que el rol escribe. Mantener «Product Owner» en inglés no rompe D1 —el framework conserva sin traducir los nombres de rol y los términos técnicos establecidos: walking skeleton, thin slice, quality gate, handoff, plan-then-confirm—, y además es lo que el propio árbol ya hace.

### §5.3 Hallazgo nuevo: el Product Owner está mapeado a dos agentes distintos

Al verificar el punto anterior aparece una contradicción que no estaba en la primera pasada. El rol de PO se asigna a dos especialidades incompatibles:

| Fuente | Qué afirma |
|---|---|
| `Marco-Teorico-SDD.md`, tabla de correspondencias con Scrum | Product Owner corresponde a **AG-00** (Product Manager) |
| `Marco-Teorico-SDD.md`, catálogo de especialidades | «AG-06 — **Scrum Master** / Agile Coach (Backlog)» |
| `Rules-Backlog-Tecnico.md` §1 | AG-06 es «**Scrum Master** con perfil de Agile Coach orientado al backlog», y en §1.2 lista al Product Owner como rol **acompañante y distinto** de AG-06 |
| `Rules-Plan-Sprint.md` | «AG-06 **Product Owner** / Backlog» |

AG-06 se llama Scrum Master en dos archivos y Product Owner en un tercero, mientras el marco teórico asigna el PO a AG-00. Es la misma confusión de nombres de rol que motiva esta sección, un nivel más abajo, y conviene resolverla en la misma intervención.

### §5.4 El tool-prompt integrador es un agente que actúa en el rol de Product Owner

El tercer relato de la primera pasada —que el intake lo produce el tool-prompt integrador desde `INPUTs/` (`SDD-Getting-Started-Guide.md` §4, PASO-4)— no compite con el PO como autor. La aclaración del responsable precisa la relación: el rol de Product Owner es **compartido entre un agente humano y un agente de IA**, y el reparto no es arbitrario sino por naturaleza del trabajo, en fases sucesivas.

| Fase | Quién | Qué hace | Salida |
|---|---|---|---|
| Conceptualización | Agente **humano** en rol de Product Owner | Junta material bibliográfico, define conceptos, idea conceptos nuevos y los transcribe | Material **no estructurado** en `INPUTs/` |
| Integración | Agente de **IA** (tool-prompt integrador) en rol de Product Owner | Vuelca ese material no estructurado en la plantilla de intake y la completa | `SOLUTION-INTAKE` **estructurado**, en el destino |
| Generación | Orquestador y subagentes AG-00 a AG-11 | Deriva el manifiesto y genera las doce categorías | `SDD/Docs/` |

El criterio del reparto es la estructura. La parte generativa —reunir, definir, idear, transcribir— es del humano y produce material que por naturaleza no es estructurado. La parte de estructuración —mapear ese material a las secciones, las tablas y los identificadores de la plantilla— es del agente. El humano se vale del prompt integrador precisamente porque lo que produjo no tiene forma todavía.

Esto no es la misma relación que el framework tiene con sus otras especialidades. En AG-00 a AG-11 el agente hace el trabajo bajo una especialidad declarada y el humano aprueba; acá el rol se parte en dos y cada mitad la ejerce un agente de naturaleza distinta. Es un modelo de colaboración propio, y hoy no está descripto en ningún archivo.

**Es el único agente de SDD cuya especialidad no está declarada.** El principio rector del orquestador (`Master-Prompt.md` §1) dice que la especialidad «es propiedad del documento que se va a generar»: el master-prompt no la asigna, la lee de la §1.2 del archivo de reglas de cada categoría. Ese principio cubre a AG-00 hasta AG-11 y a AG-ROOT. No cubre al integrador, porque el documento que el integrador genera —el intake— no tiene un `Rules-*.md` que declare quién lo escribe. `Intake-Rules.md` regula cómo se **valida** el intake y cómo se deriva el manifiesto de él; nada dice sobre quién lo **produce**.

La causa es estructural, no un olvido: el tool-prompt integrador vive en el repositorio de documentación, que el framework declara explícitamente que no toca nunca. El resultado es que **el framework declara la especialidad de todos sus agentes salvo la del que produce su propio documento de entrada**.

### §5.5 Consecuencia sobre el mapeo de agentes

Con el modelo aclarado, la tabla de correspondencias con Scrum del marco teórico queda mal en su fila más importante. Hoy afirma que el Product Owner corresponde a AG-00 (Product Manager). Pero AG-00 es el subagente que genera la categoría 00 **a partir del intake ya escrito**: está aguas abajo. El rol de Product Owner, en el modelo aclarado, es compartido y se ejerce enteramente aguas arriba, entre el humano que conceptualiza y el integrador que estructura.

Quedan entonces dos agentes distintos del lado del producto, y hasta ahora estaban fusionados en uno:

| Agente | Posición | Rol | Produce | Declarado en |
|---|---|---|---|---|
| Tool-prompt integrador | Aguas arriba del intake | **Product Owner** | `SOLUTION-INTAKE` | Ninguna parte del framework |
| AG-00 | Aguas abajo del intake | **Product Manager** | `Vision-Producto.md`, `Alcance-Proyecto.md`, `Roadmap-Producto.md` | `Rules-Contexto.md` §1 |

Sumado a la contradicción de §5.3 (AG-06 llamado Scrum Master en dos archivos y Product Owner en un tercero), el resultado es que el término «Product Owner» hoy apunta a tres cosas distintas dentro del framework: a AG-00, a AG-06 y —en el modelo real— al par humano más integrador.

### §5.6 El tramo previo es externo por diseño; lo que falta es declarar la frontera

Una lectura anterior de este informe planteó que el framework «arranca una fase después de donde arranca el trabajo» y trató la relación con `INPUTs/` como una tensión sin resolver. La precisión del responsable corrige ese encuadre, y conviene dejarlo asentado porque cambia la severidad de lo que hay que hacer.

**El tramo previo al intake es externo al framework a propósito, y es una metodología necesaria por derecho propio.** Fuera de SDD se preparan los documentos y materiales que definen la idea del producto. El Product Owner junta todo lo necesario, y mediante el prompt integrador genera el `SOLUTION-INTAKE` siguiendo la plantilla y lo deposita en el destino que el framework demanda. El intake es la **frontera**: todo lo anterior es de otra metodología, y lo único que cruza es el documento.

Con ese encuadre, dos cosas que la pasada anterior había marcado como problema dejan de serlo:

- **La regla de autocontención no está en tensión: es la condición de frontera.** `Intake-Rules.md` §5 prohíbe que el intake deje una referencia a un archivo externo como único respaldo de un dato. Leída como norma de prolijidad, parecía un parche. Leída como condición de frontera, es exactamente lo que hace que el corte funcione: el intake debe absorber todo, porque nada de lo que quedó del otro lado es resoluble por el framework ni por los subagentes. Es lo que convierte al integrador en una pieza necesaria y no en una comodidad.
- **La procedencia de la Parte D es coherente con eso.** `SOLUTION-INTAKE-template` §20 exige «Procedencia: `[archivo fuente]`, líneas [N–M]» además del JSON completo transcripto. No es una referencia externa que reemplace al dato: es una cita de auditoría hacia el otro lado de la frontera, mientras el dato viaja entero. Las dos reglas dicen lo mismo desde ángulos distintos.

**Lo que sí falta es que la frontera esté declarada como frontera.** Hoy no está declarada: está ausente. La diferencia importa. Un tramo «externo, con este contrato en el corte» es una decisión de alcance; un tramo del que el framework no habla se lee como un olvido, y deja tres cosas sin resolver:

| Qué falta | Por qué importa |
|---|---|
| Declarar qué queda afuera y por qué | Sin eso, un lector no distingue alcance deliberado de omisión. El `README.md` declara las exclusiones del framework en otros ejes, no en este |
| Nombrar el prompt integrador como pieza metodológica necesaria, con su contrato en el corte | Hoy aparece solo como PASO-4 de un tutorial de seis pasos. Su contrato —producir un intake conforme a la plantilla, autocontenido, depositado en `SDD/Intake/`— no está escrito en ninguna parte normativa |
| Resolver el upstream del intake bajo D6 | Es el punto que sigue abierto, y se trata abajo |

**El upstream del intake sigue siendo un cabo suelto, pero menor.** D6 exige que «cada documento declara su upstream y su downstream en la cabecera». El `SOLUTION-INTAKE` tiene su trazabilidad downstream completa hacia las doce categorías, y su cabecera no tiene ningún campo que apunte hacia atrás: nombre de la solución, cliente, repositorio, lead técnico, documento, versión, fecha, stack y estado.

Con el tramo previo declarado externo, hay dos salidas honestas, y las dos son baratas:

- **Campo de fuentes informativo.** La cabecera suma un campo que nombra el material de origen y aclara que es una cita de auditoría no resoluble por el framework, coherente con la autocontención: el dato ya está adentro, la cita solo dice de dónde vino.
- **Exención declarada de D6.** El intake se declara documento de frontera y por lo tanto sin upstream dentro del universo del framework. Hay precedente de forma para esto: la política de deprecación de `Master-Prompt.md` §5.1 ya maneja una tabla de cinco exenciones declaradas.

Lo que no corresponde es dejarlo como está, que es el único caso donde una invariante no se cumple sin que ningún archivo diga por qué.

**Consecuencia sobre la severidad.** La pasada anterior proponía agregar una fase al orquestador, lo que habría sido major. Con el tramo declarado externo eso ya no corresponde: el orquestador no debe orquestar lo que está afuera. Alcanza con declarar la frontera, su contrato y la resolución de D6, que es material de `README.md`, `Intake-Rules.md` y las guías. Baja de major a minor.

### §5.7 Qué resuelve esto

La aclaración cierra dos decisiones abiertas y desbloquea una tercera:

- **Quién redacta el intake**: el Product Owner, humano en la recolección y agente en la redacción bajo plantilla. Deja de ser decisión y pasa a ser dato a escribir.
- **Quién es dueño del nombre del producto** (§4.1): el Product Owner humano. El nombre del producto es un dato de entrada declarado, no derivado del material de investigación.
- Con eso resuelto, la separación entre nombre de producto y nombre de código queda habilitada para decidirse por su propio mérito, sin depender de quién la declara.

### §5.8 Cambios que se siguen

| Cambio | Archivo | Severidad |
|---|---|---|
| La cabecera del intake suma un campo de autor: **Product Owner**, distinto de «Cliente / Stakeholder principal» y de «Lead técnico» | `SOLUTION-INTAKE-template`, cabecera | Minor |
| La pregunta bloqueante de §2 se desdobla: quién es el Product Owner que aporta el conocimiento y aprueba, y quién es el propietario del problema como categoría de stakeholder | `SOLUTION-INTAKE-template` §2 | Minor |
| Se declara el reparto: el PO humano reúne la información y aprueba; el tool-prompt integrador la reúne bajo la plantilla actuando en el rol de PO | `SOLUTION-INTAKE-template`, guía de uso; `SDD-Getting-Started-Guide.md` PASO-4 | Minor |
| **Se declara la frontera del framework** (§5.6): qué queda afuera, por qué, y cuál es el contrato en el corte | `README.md`, `Intake-Rules.md`, `SDD-User-Guide.md`, `SDD-Getting-Started-Guide.md` | Minor |
| **Se nombra el prompt integrador como pieza metodológica necesaria**, externa al framework, con su contrato: intake conforme a plantilla, autocontenido, depositado en `SDD/Intake/` | `README.md`, `Intake-Rules.md` | Minor |
| **Se resuelve el upstream del intake bajo D6** (§5.6): campo de fuentes informativo o exención declarada | `SOLUTION-INTAKE-template` cabecera, o tabla de exenciones | Minor |
| Se corrige la fila de la tabla de correspondencias con Scrum: el PO no es AG-00, que es Product Manager aguas abajo | `Marco-Teorico-SDD.md` | Minor |
| Se unifica a qué agente corresponde AG-06, hoy Scrum Master en dos archivos y Product Owner en un tercero (§5.3) | `Marco-Teorico-SDD.md`, `Rules-Plan-Sprint.md`, `Rules-Backlog-Tecnico.md` | Minor |
| Entradas de glosario para Product Owner y para tool-prompt integrador, con su distinción respecto de la categoría «propietario», de AG-00 y de AG-06 | `Master-Prompt.md` §15, `SDD-User-Guide.md` §10.1 | Minor |
| Se conserva «propietario» donde designa la categoría de stakeholder, con el mismo criterio que la normalización de actores de 2026-07-26 conservó «implementador» | Cuatro archivos de la taxonomía | Sin cambio, se declara el criterio |

Hay precedente directo para la parte de vocabulario: la entrada de 2026-07-26 «Normalización del vocabulario de actores» ya hizo una pasada de este tipo —«consumidor» pasó a «integrador» o «lector» según el caso— y **conservó deliberadamente «implementador» donde designaba la categoría de stakeholder**. El mismo criterio aplica acá: no se toca la taxonomía, se agrega el rol que faltaba.

### §5.9 Qué queda por decidir sobre la frontera

Con el tramo previo declarado externo (§5.6), la decisión ya no es si el framework absorbe esa fase, sino **cuánto declara del contrato en el corte**. Tres niveles posibles, de menor a mayor:

1. **Solo la exclusión.** El `README.md` declara que la preparación del material conceptual y su integración en el intake quedan fuera del alcance, y que el intake es la frontera. Resuelve la ambigüedad de lectura y nada más.
2. **La exclusión más el contrato** (recomendado). Suma qué tiene que cumplir lo que cruza: intake conforme a la plantilla, autocontenido según `Intake-Rules.md` §5, depositado en `SDD/Intake/`, con el checklist de §19 tildado. Es lo que ya se exige de hecho en la validación de intake; solo se declara del lado de afuera para que el productor sepa contra qué producir.
3. **La exclusión, el contrato y la especialidad del integrador.** Suma la declaración del rol —qué es, qué insumos toma, con qué criterios—, dejando el tool-prompt concreto en el repositorio de documentación. Esto reabre la tensión con el modelo de tres repositorios: el framework norma sin tocar, el repositorio de documentación instancia. Es coherente con el principio de delegación de la especialidad, pero es el único de los tres niveles que agrega material normativo sobre algo declarado externo, y ahí hay una decisión de criterio, no de forma.

El nivel 2 es el que mejor relación costo/beneficio tiene: cierra la ambigüedad, le da al productor del intake un contrato verificable y no compromete al framework a normar una metodología que declaró afuera. El nivel 3 conviene solo si el prompt integrador se va a estandarizar entre soluciones, y en ese caso la pregunta pasa a ser si vive en `IA.SDD` o en un repositorio de metodología aparte.

### §5.10 Product Owner y stakeholder: dos comportamientos distintos

La distinción entre ambos roles no es de jerarquía sino de naturaleza, y tiene consecuencias operativas sobre cómo se comporta un agente que ejerce cada uno.

**Stakeholder** es una categoría de relación con el producto, no un puesto. Es plural por naturaleza y parcial por definición: cada uno aporta un interés, un dolor o una restricción. Su legitimidad no depende de que tenga razón; un stakeholder puede pedir algo equivocado y sigue siendo una voz válida que hay que registrar.

**Product Owner** es una responsabilidad, y en la bibliografía de Scrum es explícitamente una persona y no un comité. Su trabajo no es tener un interés propio: es arbitrar entre intereses que se contradicen y cerrar. Es dueño de la consecuencia de esa decisión.

En una línea: el stakeholder tiene un interés; el PO tiene autoridad y rinde cuentas. Los stakeholders son muchos y divergen; el PO es uno y tiene que converger. El PO además **es** un stakeholder —cae en la categoría «propietario» de la tríada—, pero eso describe de dónde viene, no qué hace con lo que llega.

#### §5.10.1 Cómo se comportaría cada uno como agente

| | Agente stakeholder | Agente Product Owner |
|---|---|---|
| Perspectiva | Parcial a propósito | Global, obligatoriamente |
| Cardinalidad | N agentes, uno por rol representado | Uno. Dos POs es cero decisiones |
| Frente a una contradicción | La produce | La cierra, con criterio declarado |
| ¿Puede decir «no»? | No. Pide, se queja, juzga | Sí, y es su función principal |
| Criterio de éxito | Fidelidad a la voz que representa | Coherencia y decidibilidad del resultado |
| Falla característica | Volverse equilibrado y razonable: deja de representar | No decidir: todo es Must, ninguna exclusión |
| Salida | Necesidades, restricciones, juicios de aceptación | Prioridades, exclusiones, backlog ordenado |

La asimetría en una frase: la virtud del agente stakeholder es ser parcial; la del agente PO es cerrar. Un agente que intente las dos no hace ninguna bien, que es el riesgo concreto de implementarlas como un solo prompt.

#### §5.10.2 El intake ya separa los dos comportamientos, sin nombrarlos

Las secciones de la Parte A se parten limpio según qué comportamiento las produce:

| Comportamiento | Secciones | Evidencia en la plantilla |
|---|---|---|
| Aportado por stakeholders: parcial, plural | §2 stakeholders, §7 casos límite, §11 riesgos, §12 glosario | §2 exige voces nombradas, «sin genéricos como "los usuarios"»; §11 pregunta «¿qué le quita el sueño al cliente?» |
| Arbitrado por el PO: global, decidido | §4 MoSCoW, §9 exclusiones, §10 restricciones | §4 declara el modo de falla, «si todo es Must, no hay priorización»; §9 pregunta «¿qué se pidió y se **decidió** dejar afuera?» |

La §9 es el caso más limpio de arbitraje registrado: alguien pidió y alguien decidió que no. No puede salir de un stakeholder.

#### §5.10.3 Decisión tomada: AG-00 es Product Manager, no Product Owner

`Rules-Contexto.md` §1.1 declara que la responsabilidad principal de AG-00 es «completar lo que el cliente todavía no dijo: forzar la priorización MoSCoW, declarar exclusiones explícitas, traducir aspiraciones en objetivos SMART». Esa frase mezcla dos comportamientos incompatibles:

| Sentido | Comportamiento | ¿Corresponde a AG-00? |
|---|---|---|
| Formalizar lo implícito: traducir aspiraciones en objetivos SMART, estructurar, explicitar lo dado por sabido | Product Manager | **Sí.** Es su trabajo y se conserva |
| Decidir lo no decidido: forzar MoSCoW, declarar exclusiones | Product Owner, arbitraje | **No.** Corresponde aguas arriba |

Se descarta que AG-00 sea un stakeholder: no tiene interés propio ni representa a nadie, y pedirle parcialidad sería lo contrario de lo que hace.

Se descarta que sea Product Owner por un motivo estructural. Lo que define al PO no es decidir sino rendir cuentas, y AG-00 corre **aguas abajo del punto donde el humano ya confirmó el intake y el manifiesto**, es decir donde ya declaró qué es el producto. Si arbitra ahí, las decisiones de producto entran a la cadena D6 habiendo pasado solo un audit y nunca una aprobación.

**La arbitración además ya es innecesaria, y el texto quedó sin actualizar.** `Intake-Rules.md` §5 valida, antes de que corra cualquier subagente, que §4 tenga MoSCoW con un Must mínimo razonable y que §9 tenga al menos tres exclusiones. Si el intake pasa, no queda nada que forzar; si no pasa, el orquestador se detiene con la batería de preguntas. Las fechas lo confirman: la frase viene de `Rules-Contexto.md` 1.0, del 2026-05-17, generada en el bootstrap, e `Intake-Rules.md` no existió hasta el 2026-06-10 con la unificación de intake. Ninguna versión intermedia revisó ese párrafo.

El marco teórico ya tenía la formulación correcta y la tabla de correspondencias la perdió. El catálogo de especialidades dice: «Alias. PM, Product Owner senior **en contextos donde el rol no existe formalmente**». Esa condición final es la clave: AG-00 hace de PO solo cuando no hay PO. Con el PO declarado como autor del intake (§5), el alias no aplica, y el error está en la tabla que afirma sin condición «Product Owner → AG-00».

#### §5.10.4 Por qué importa: el riesgo no es que invente mal, es que invente bien

Un AG-00 al que le falta la priorización no produce algo absurdo: produce un MoSCoW razonable, coherente con el resto del intake e indistinguible de uno decidido. No queda ninguna marca que diga que lo decidió el agente. De ahí se encadenan tres efectos:

1. **Se vuelve upstream de todo.** Una vez en `Vision-Producto.md`, la cadena D6 lo toma: NB traza a la visión, CU a NB, US a CU. La invención se propaga por once categorías y cada documento aguas abajo cita correctamente su upstream. La trazabilidad, que existe para detectar deriva, termina certificándola.
2. **El audit no lo detecta.** El auditor verifica D1-D9 y los criterios de §6 de cada regla: completitud, forma y coherencia interna. Una prioridad inventada pero coherente pasa todos los checks. El audit no verifica fidelidad a una intención que nunca se expresó.
3. **D9 no protege acá.** El glosario declara que la evidencia verificable «no aplica a afirmaciones de diseño, de especificación ni de contexto». La categoría 00 es exactamente eso: la regla de evidencia exime el lugar donde ocurriría la invención.

Por eso la salida correcta no es que AG-00 escriba menos, sino que **cuestione lo que falta** en lugar de completarlo.

#### §5.10.5 Criterio para distinguir formalización de ambigüedad

Hace falta un test operativo, porque «falta un dato» y «falta una decisión» se parecen:

> Es **formalización** si el dato está en el intake y solo falta darle forma. Es **ambigüedad** si resolverlo constituye una decisión de producto: alguien tendría que elegir entre alternativas todas legítimas, y esa elección compromete al negocio.
>
> Test rápido: ¿la respuesta podría ser otra sin que nada del intake quede contradicho? Si la respuesta es sí, es una decisión y se escala.

#### §5.10.6 Catálogo de ambigüedades de la categoría 00

Cada ítem, al detectarse, dispara el patrón de ambigüedad legítima de `Master-Prompt.md` §9: detención, pregunta, reanudación. Ninguno se resuelve por cuenta del subagente.

| Id | Ambigüedad | Artefacto afectado |
|---|---|---|
| A1 | MoSCoW degenerado: §4 con todo en Must, o sin ningún Must | `Alcance-Proyecto.md` |
| A2 | Capacidad de §4 que no entra al alcance ni figura en las exclusiones de §9 | `Alcance-Proyecto.md` |
| A3 | Exclusión de §9 que contradice una capacidad Must de §4 | `Alcance-Proyecto.md` |
| A4 | Supuesto del equipo sin confirmar, cuando el alcance depende de él | `Alcance-Proyecto.md` |
| B1 | Métrica de §8 sin target numérico o sin plazo: el número es un compromiso de negocio | `Vision-Producto.md` §5, §6 |
| B2 | Métrica sin fuente de dato obtenible, siendo que la trazabilidad de `Rules-Contexto` §7 exige declararla | `Vision-Producto.md` §6 |
| B3 | Dos objetivos que se contradicen entre sí | `Vision-Producto.md` §5 |
| C1 | Categoría de la tríada sin representante, contra el criterio de §6 que exige mínimo uno por categoría | `Vision-Producto.md` §2 |
| C2 | Dos stakeholders con intereses incompatibles y sin arbitraje en el intake | `Vision-Producto.md` §2 |
| C3 | Stakeholder genérico, que el propio intake prohíbe | `Vision-Producto.md` §2 |
| D1 | Fecha objetivo de §10 incompatible con el alcance Must de §4 | `Roadmap-Producto.md` |
| D2 | Criterio de transición entre fases ausente, siendo verificable y exigido | `Roadmap-Producto.md` §5 |
| D3 | Orden de fases no derivable: dos Must sin precedencia declarada ni dependencia técnica que la imponga | `Roadmap-Producto.md` §2 |
| E1 | Plataformas target que se contradicen entre proyectos sin declarar cuál rige | `Compatibilidad-Plataformas.md` |
| E2 | Versión mínima de runtime o SO ausente donde el tipo D8 la exige | `Compatibilidad-Plataformas.md` |
| F1 | `equipo_n` sin dato: gatea la existencia del acuerdo de equipo y la forma de la categoría 07 | `Acuerdo-Equipo.md` |
| G1 | Riesgo de §11 sin mitigación ni responsable: asignar responsable es decisión organizativa | `Vision-Producto.md` §8 |
| G2 | Término del glosario con dos definiciones incompatibles entre fuentes | `Vision-Producto.md` §9 |

**Hallazgo colateral verificado en F1.** `Master-Prompt.md` §4 declara que `equipo_n` se lee de «SOLUTION-INTAKE §2 (stakeholders) o §10 (restricciones del cliente)», y **ninguna de las dos secciones pide ese dato**: §2 pide una tabla de roles con nombre, categoría y responsabilidad, y §10 pide presupuesto, fecha objetivo, normativa e integraciones obligatorias. El flag tiene origen declarado en secciones que no lo contienen, y gatea la emisión de `Acuerdo-Equipo.md` y la forma de la categoría 07. Es un defecto independiente de todo lo demás y se corrige agregando la pregunta al intake.

#### §5.10.7 El punto estructural: el mecanismo existe, el catálogo no

`Intake-Rules.md` §1 describe el mecanismo de §9 del master-prompt como «detección **reactiva**, en runtime, cuando un subagente ya generando detecta un dato faltante». Es correcto que sea reactivo, pero hoy no hay ningún catálogo de qué buscar: cada subagente detecta lo que le llama la atención mientras escribe.

Un catálogo por categoría, como el de §5.10.6, convierte ese mecanismo en una verificación que el subagente puede correr **antes** de redactar. No reemplaza la detección reactiva: le pone un piso. El lugar natural es la §6 de cada `Rules-<Categoria>.md`, junto a los criterios de aceptación, y la categoría 00 sirve de piloto.

#### §5.10.8 Pregunta abierta que hereda el integrador

El mismo criterio de §5.10.5 aplica al tool-prompt integrador, y ahí queda sin resolver. Si el integrador actúa en rol de PO (§5.4), entonces **tiene que arbitrar**; y cuando el material aporte posiciones de stakeholders que se contradicen —el caso normal— tiene dos salidas legítimas: resolver la contradicción, que es decidir sobre el producto, o escalarla al PO humano.

Hoy nada dice cuál, y las dos fallas son reales. Si transcribe ambas posiciones sin resolver, produce un intake que no pasa la validación posterior (§4 con todo en Must, §9 sin exclusiones). Si resuelve en silencio, toma decisiones de producto que ningún humano aprobó y que se propagan por las doce categorías. Esto forma parte de lo que debería declararse en el contrato de frontera de §5.6.

#### §5.10.9 Cambios que se siguen

| Cambio | Archivo | Severidad |
|---|---|---|
| §1.1 parte la responsabilidad de AG-00: formaliza y estructura, no decide. Ante un ítem no decidido escala por §9 | `Rules-Contexto.md` §1.1 | Minor |
| Se incorpora el catálogo de ambigüedades de la categoría 00 como verificación previa a la redacción | `Rules-Contexto.md` §6 | Minor |
| Se corrige la fila de correspondencia con Scrum y se restituye la condición del alias de AG-00 | `Marco-Teorico-SDD.md` | Minor |
| Se agrega al intake la pregunta que da origen a `equipo_n` (hallazgo F1) | `SOLUTION-INTAKE-template` §2 o §10, `Master-Prompt.md` §4 | Minor |
| Entradas de glosario para Product Owner y stakeholder, con la distinción de comportamiento | `Master-Prompt.md` §15, `SDD-User-Guide.md` §10.1 | Minor |
| Se declara qué hace el integrador ante contradicciones entre stakeholders (§5.10.8) | Contrato de frontera de §5.6 | Minor |

---

## §6 Decisiones de fondo que quedan abiertas

Consolidadas de las tres pasadas. La decisión sobre quién redacta el intake quedó **resuelta** por la aclaración de §5 y sale de esta lista: la redacta el Product Owner, humano en la recolección y agente integrador en la redacción bajo plantilla.

1. **Si el nombre de producto se separa del nombre de código** (§4.1). Es la decisión que habilita o descarta el resto del modelo de cuatro niveles. Ya tiene dueño declarado: el PO.
2. **Si el proyecto de código pasa a ser una entidad enumerable** con 1..N por proyecto SDD (§4.3). Es el único cambio major del conjunto y el que cierra la contradicción interna del mapa de sufijos.
3. **Si el espacio de nombres se declara como nivel** o se deja implícito en la convención de nombres (§4.2).
4. **Cuánto se declara del contrato de frontera** (§5.6 y §5.9): solo la exclusión, la exclusión más el contrato de lo que cruza, o además la especialidad del integrador. El tramo previo queda externo en los tres casos; lo que se decide es qué escribe el framework sobre el corte. Incluye resolver el upstream del intake bajo D6, con campo de fuentes informativo o exención declarada.
5. **Con qué expresión se desambigua el segundo sentido de «producto»** —el sistema construido, por oposición a la maqueta y a la documentación— una vez que el primer sentido queda tomado por el concepto de primer nivel (§1.4, reconsideración). La decisión de si «producto» se define como concepto **ya no está abierta**: el modelo de §4 la resuelve por la afirmativa.
6. **Si las cuatro copias de la convención de nombres se reducen a una fuente con punteros** (§3.5), con el mismo criterio que la versión 4.0 aplicó al versionado.
7. **Si el registro de decisión de producto se incorpora** como instrumento análogo al ADR, o si alcanza con que la decisión se escale siempre al PO (§8.3).
8. **Si se incorporan las especialidades faltantes** y con qué gatillo: Product Owner declarado, AppSec y Modelador de Datos (§8.5); y si 05 y 09 se desdoblan por nivel (§8.6).

## §7 Plan de mejoras

Consolida todo lo anterior, incluida la evaluación de especialidades de §8, que se lee como anexo analítico de las intervenciones E.

> **Estado de ejecución al 2026-07-29.** Las intervenciones **A y B están aplicadas** sobre el framework, publicadas como entrada `[4.1]` de su `CHANGELOG.md` y verificadas en `SDD/Devs/Guides/Coherencia-Roles-Y-Defectos-Verificados.md`. Alcanzaron nueve archivos editados más la nota de coherencia. Durante la ejecución aparecieron cuatro defectos adicionales de la misma clase, resueltos en la misma pasada: cuatro referencias al `BRIEF` deprecado, residuos del intercambio 10 ↔ 11 en `Rules-Contexto.md`, un conteo desactualizado en su prompt-snippet y el origen inexistente del flag `equipo_n`. El defecto A5 resultó mayor de lo diagnosticado: las fichas de AG-10 y AG-11 del catálogo §4.2 estaban **completamente intercambiadas**, y la entrada 1.7 del control de cambios del marco teórico declaraba haberlas corregido. Las intervenciones **C, D, E1 a E4 y F siguen pendientes** de las decisiones de §6.

### §7.1 Criterio de ordenamiento

Tres criterios, en este orden de prioridad:

1. **Lo que no requiere decisión va primero.** Un defecto verificado se corrige sin consumir tiempo del responsable. Además baja el ruido: con los defectos afuera, las discusiones de fondo se dan sobre un árbol limpio.
2. **Lo que desbloquea a otros va antes que lo que depende.** La terminología de producto es prerrequisito del modelo de cuatro niveles; el Product Owner declarado es prerrequisito del fin de la triple asignación de la priorización.
3. **Lo major va último.** Un cambio que invalida documentación ya emitida obliga a snapshot en `_legacy/`, entrada de `CHANGELOG.md` y declaración de qué quedó invalidado. Conviene hacerlo una sola vez y con todo lo anterior consolidado.

### §7.2 Las cuatro ondas

| Onda | Intervenciones | Requiere decisión | Severidad |
|---|---|---|---|
| **1 — Higiene** | A, B | No | Patch a minor |
| **2 — Vocabulario y frontera** | C, F, E1 | Decisiones 4 y 5 | Minor |
| **3 — Especialidades** | E2, E3, E4 | Decisiones 7 y 8 | Minor, con un major |
| **4 — Estructura** | D | Decisiones 1 a 3 | Major |

### §7.3 Detalle por intervención

#### Intervención A — defectos verificados

**Qué cierra.** §3.1 a §3.4 y §8.7: el caso de `Nombre-Solucion` y su propagación al nombre de archivo, la plantilla del manifiesto sin versión en cabecera, las rutas `rules/` obsoletas, el árbol de ejemplo del intake §16, el diagrama de trazabilidad con 10 y 11 invertidos, y la tabla del catálogo apuntando a `03-UX-UI/`.

**Prerrequisitos.** Ninguno. Son defectos verificados contra el árbol vigente.

**Archivos.** `SOLUTION-MANIFEST-template.md`, `SOLUTION-INTAKE-template.md`, `Master-Prompt.md`, `Marco-Teorico-SDD.md`, `SDD-User-Guide.md`.

**Severidad.** Patch a minor. Ninguna documentación emitida deja de cumplir.

**Entregable.** Una nota de coherencia según el patrón de `Coherencia-Auditoria-Marco.md` y una entrada de `CHANGELOG.md`.

#### Intervención B — roles y comportamiento de AG-00

**Qué cierra.** §5.8 y §5.10.9: el campo de autor Product Owner en la cabecera del intake, el desdoblamiento de la pregunta bloqueante de §2, el reparto humano/agente declarado, la partición de la responsabilidad de AG-00 entre formalizar y decidir, el catálogo de ambigüedades de la categoría 00, la corrección de la correspondencia con Scrum y del alias de AG-00, la unificación del nombre de AG-06, el origen de `equipo_n` y las entradas de glosario.

**Prerrequisitos.** Ninguno: la decisión sobre AG-00 está tomada (§5.10.3) y el resto lo habilita la aclaración del responsable.

**Archivos.** `Rules-Contexto.md` §1.1 y §6, `SOLUTION-INTAKE-template.md` cabecera y §2, `Marco-Teorico-SDD.md`, `Rules-Plan-Sprint.md`, `Rules-Backlog-Tecnico.md`, `Master-Prompt.md` §4 y §15, `SDD-User-Guide.md` §10.1, `SDD-Getting-Started-Guide.md`.

**Severidad.** Minor. Precedente directo en la normalización de actores de 2026-07-26.

**Dos entregas separadas, por alcance distinto:**

- **B1 vocabulario de roles**: cabeceras, glosarios y tablas de correspondencia.
- **B2 comportamiento de AG-00**: `Rules-Contexto.md` §1.1 y §6. Define un patrón —el catálogo de ambigüedades por categoría— que la categoría 00 estrena como piloto y que después conviene replicar en las once restantes. Esa réplica es trabajo aparte y no entra en esta intervención.

#### Intervención C — declaración de la frontera

**Qué cierra.** §5.6 y §5.9: qué queda fuera del framework y por qué, el contrato de lo que cruza el corte, el prompt integrador como pieza metodológica externa necesaria, qué hace el integrador ante contradicciones entre stakeholders (§5.10.8) y la resolución del upstream del intake bajo D6.

**Prerrequisitos.** Decisión 4 de §6: cuánto se declara del contrato de frontera. Recomendado el nivel 2 de §5.9.

**Archivos.** `README.md`, `Intake-Rules.md`, `SOLUTION-INTAKE-template.md` cabecera, `SDD-User-Guide.md`, `SDD-Getting-Started-Guide.md`.

**Severidad.** Minor. No agrega ninguna fase al orquestador: el tramo es externo y el orquestador no debe orquestar lo que está afuera.

#### Intervención F — terminología de producto

**Qué cierra.** §1.4 con su reconsideración: «producto» deja de ser un término sin dueño y pasa a ser el concepto de primer nivel, con su entrada de glosario, su nombre propio en la cabecera del intake y su dueño declarado. El segundo sentido —el sistema construido, por oposición a la maqueta— se desambigua con otra expresión.

**Prerrequisitos.** Decisión 5 de §6, que es solo elegir la expresión del segundo sentido. La definición del concepto ya está resuelta por el modelo de §4.

**Archivos.** `Master-Prompt.md` §15, `SDD-User-Guide.md` §10.1, `SOLUTION-INTAKE-template.md` cabecera, `Rules-Contexto.md`.

**Severidad.** Minor.

**Por qué va en la onda 2 y no en la 4.** Es prerrequisito de la intervención D: no se puede separar el nombre del producto del nombre de código sin haber definido antes qué es el producto. Hacerla temprano y barata evita que D arrastre la discusión terminológica.

#### Intervención E1 — Product Owner declarado y fin de la triple asignación

**Qué cierra.** §8.2 y §8.5: la priorización MoSCoW deja de estar declarada como responsabilidad de AG-00, AG-01 y AG-06, y pasa a tener un dueño único aguas arriba. Las tres especialidades la derivan y trazan; ninguna la origina, y si falta escalan por §9.

**Prerrequisitos.** Intervención B cerrada, porque comparte archivos y porque el mandato de AG-00 se acota ahí.

**Archivos.** `Rules-Contexto.md`, `Rules-Necesidades-Negocio.md`, `Rules-Backlog-Tecnico.md`, `Master-Prompt.md` §15, glosarios.

**Severidad.** Minor.

**Por qué importa el orden.** Es el cambio de mejor relación costo/beneficio de todo el plan: toca tres archivos de reglas y cierra el modo de falla más caro identificado, que es el de tres agentes inventando la misma decisión por separado sin contradecirse de forma detectable.

#### Intervención E2 — desdoblamiento de 05 y 09 por nivel

**Qué cierra.** §8.6: la arquitectura de solución se separa de la de proyecto (Arquitecto de Soluciones frente a Arquitecto de Software), y la coordinación del build topológico se separa del pipeline de proyecto (Release Engineer frente a DevOps).

**Prerrequisitos.** Decisión 8 de §6.

**Severidad.** **Major.** Toca el gating de dos categorías y la variante de especialidad que el orquestador lee de la §1.2 de cada regla, así que cambia qué subagente se despacha en qué nivel.

#### Intervención E3 — especialidades gatilladas

**Qué cierra.** §8.5: AppSec bajo `requiere_compliance` o `tiene_auth`, y Modelador de Datos bajo `tiene_persistencia`. Ambas siguen el patrón ya resuelto por `usa_llm` para AG-04 y por `requiere_maqueta` para AG-03M.

**Prerrequisitos.** Decisión 8 de §6.

**Severidad.** Minor: agrega variantes de multi-especialidad a categorías existentes, sin cambiar qué artefactos se producen.

**Prioridad relativa.** AppSec primero si algún producto en curso es regulado; el Modelador de Datos es mejora de coherencia y puede esperar.

#### Intervención E4 — registro de decisión de producto

**Qué cierra.** §8.3: la imposibilidad de distinguir una decisión tomada por el PO de una completada por un agente. Instrumento análogo al ADR, aplicable a prioridad, exclusión, recorte de alcance y criterio de transición de fase.

**Prerrequisitos.** Decisión 7 de §6. Si esa decisión resuelve que toda decisión de producto se escala siempre al PO, esta intervención no se hace.

**Severidad.** Minor.

#### Intervención D — modelo de cuatro niveles

**Qué cierra.** §4 completo: producto, solución de código, proyecto de código y espacio de nombres como niveles declarados, con la tabla de composición admitiendo 1..N proyectos de código por proyecto SDD.

**Prerrequisitos.** Decisiones 1 a 3 de §6, más las intervenciones A, B y F cerradas por solapamiento de archivos.

**Severidad.** **Major.** Cambia el esquema de la tabla de composición, obliga a snapshot en `_legacy/`, a entrada de `CHANGELOG.md` y a declarar qué documentación emitida queda invalidada.

**Nota de alcance.** El conjunto cerrado D8 no se toca: sus ocho valores describen componentes entregables, que es el nivel al que el proyecto SDD pertenece.

### §7.4 Dependencias

```text
A ─────────────────────────────┐
                               ├──> D  (onda 4, major)
B ──┬──> E1 ───────────────────┤
    │                          │
    └──> (réplica del catálogo de ambigüedades en las 11 categorías restantes)
                               │
F ─────────────────────────────┘

C  (independiente de todo lo demás)

E2, E3, E4  (dependen solo de las decisiones 7 y 8)
```

Lecturas del grafo: **C es completamente independiente** y puede hacerse en cualquier momento, incluso en paralelo. **A, B y F son prerrequisito de D** por solapamiento de archivos, no por dependencia conceptual. **E1 depende de B**. **E2, E3 y E4 no dependen de ninguna otra intervención**, solo de sus decisiones.

### §7.5 Recorrido recomendado

| Paso | Qué | Por qué en ese momento |
|---|---|---|
| 1 | Intervención A | Sin decisiones, limpia el árbol antes de discutir sobre él |
| 2 | Intervención B | Sin decisiones, y desbloquea E1 |
| 3 | Decisiones 4 y 5 de §6 | Son las dos más baratas de responder |
| 4 | Intervenciones C y F, en paralelo | No se tocan entre sí |
| 5 | Intervención E1 | Cierra el modo de falla más caro del informe |
| 6 | Decisiones 7 y 8 | Ya con el vocabulario de roles consolidado, se discuten mejor |
| 7 | Intervenciones E3 y E4 | Minor, incrementales |
| 8 | Decisiones 1 a 3 | La discusión de fondo del modelo de cuatro niveles |
| 9 | Intervenciones D y E2 | Los dos major, juntos, con un solo snapshot en `_legacy/` |

Los pasos 1 y 2 se pueden ejecutar hoy. El paso 9 conviene tratarlo como una versión mayor del framework, con su entrada de `CHANGELOG.md` y su snapshot, dado que las reglas de intervención del `README.md` exigen para un major actualizar el master-prompt, `Root-Rules.md` y la guía de usuario en la misma intervención.

### §7.6 Lo que este plan deja explícitamente afuera

- **La réplica del catálogo de ambigüedades en las once categorías restantes.** La categoría 00 lo estrena como piloto en B2. Replicarlo es trabajo de volumen y conviene hacerlo cuando el patrón haya probado su valor en una corrida real.
- **La reducción de las cuatro copias de la convención de nombres** (decisión 6 de §6). Hoy todas dicen lo mismo: es superficie de deriva, no defecto. Conviene resolverla dentro de D, que ya toca esos archivos.
- **La emisión efectiva del árbol `/src`.** Este informe evalúa cómo se nombra el plano de código, no propone que el framework lo genere.

---

## §8 Evaluación de la asignación de especialidades por categoría

Evaluación de qué especialidad debe intervenir en cada categoría para que la producción del producto completo sea óptima, contrastando el catálogo vigente (`Marco-Teorico-SDD.md` §4.3, catorce especialidades) contra los hallazgos de §4 y §5.

### §8.1 Criterios de evaluación

Cuatro ejes, derivados de lo que este informe encontró:

| Eje | Pregunta | Origen |
|---|---|---|
| **Autoridad de decisión** | ¿La especialidad decide algo que le corresponde decidir, o arbitra lo que otro debió cerrar? | §5.10.3 |
| **Nivel de aplicación** | ¿El trabajo a nivel solución y a nivel proyecto es el mismo trabajo? Si no, ¿lo hace la misma especialidad? | §4.4, §4.5 |
| **Cobertura** | ¿Hay una capacidad crítica del producto sin especialidad que la sostenga? | §8.5 |
| **Instrumento de registro** | Cuando la especialidad sí decide, ¿queda registrado que hubo una decisión, con alternativas y dueño? | §8.3 |

### §8.2 Hallazgo transversal: la priorización está triple-asignada y ninguno de los tres es su dueño

El defecto que §5.10.3 detectó en AG-00 no es puntual. La priorización MoSCoW aparece como responsabilidad declarada en **tres especialidades distintas**, y ninguna de las tres es el Product Owner:

| Especialidad | Qué declara su §1.1 | Categoría |
|---|---|---|
| AG-00 Product Manager | «forzar la priorización MoSCoW» | 00 |
| AG-01 Analista de Negocio | articular el problema «con qué prioridad relativa» | 01 |
| AG-06 Scrum Master (Backlog) | «que la priorización MoSCoW refleje el valor real de negocio» | 06 |

Tres agentes con mandato sobre la misma decisión, ejecutados en fases distintas y sin ninguno que rinda cuentas. El modo de falla es acumulativo: si el intake trae la priorización decidida, los tres la derivan y no pasa nada; si no la trae, **los tres la inventan por separado**, cada uno coherente consigo mismo, y las tres versiones conviven en la documentación sin contradecirse de forma detectable, porque cada una vive en su categoría.

La corrección es la misma que §5.10.3 aplicó a AG-00, extendida: la priorización se decide una sola vez, aguas arriba, en el intake y por el PO; las tres especialidades la **derivan y trazan**, y ninguna la origina. Si falta, escalan por §9.

### §8.3 El instrumento que falta: no hay registro de decisión de producto

El framework ya sabe cómo manejar a un agente que decide legítimamente: **el ADR**. Cuando AG-05 elige entre alternativas técnicas, no lo hace en silencio: emite un registro con contexto, alternativas evaluadas, decisión, consecuencias y estado. Eso es lo que hace que una decisión de un agente sea auditable en vez de invisible.

Ese instrumento existe solo en la categoría 05. Las decisiones de producto —prioridad, exclusión, recorte de alcance, criterio de transición de fase— no tienen equivalente. Se escriben como si fueran hechos.

Es la raíz de lo que §5.10.4 describe: el problema no es que un agente decida, es que decida sin dejar rastro de que hubo una decisión. Dos salidas posibles, y no son excluyentes:

- **Que la decisión no ocurra ahí** (§8.2): se escala al PO. Es la salida preferente.
- **Que ocurra y quede registrada**: un registro de decisión de producto, análogo al ADR, para los casos en que el PO delega explícitamente. Le daría al framework una forma de distinguir «esto lo decidió el PO» de «esto lo completó el agente», que hoy no tiene.

### §8.4 Evaluación categoría por categoría

| Cat. | Especialidad vigente | Veredicto | Observación |
|---|---|---|---|
| **ROOT** | AG-ROOT Arquitecto de Soluciones | **Adecuada** | Consolida e integra, no origina contenido. Su autoridad es de coherencia, que es exactamente lo que hace |
| **00** | AG-00 Product Manager | **Mal asignada, ya resuelta** | Ver §5.10.3. Formaliza, no arbitra. Alias de PO condicionado a que el rol no exista |
| **01** | AG-01 Analista de Negocio | **Correcta con reserva** | El rol es adecuado: traduce dolores en NB medibles. La reserva es «prioridad relativa» (§8.2): la deriva de §4 del intake, no la origina |
| **02** | AG-02 Analista Funcional | **Adecuada** | Elicitación y formalización con IREB, Cockburn y BDD. Es el caso limpio: traduce, no decide. Su riesgo es otro, ver §8.5 sobre el modelador de datos |
| **03** | AG-03 UX/UI o DX según variante | **Subespecificada** | Su §1.1 declara «investigación de usuarios», y no hay usuarios que investigar: trabaja sobre documentos. O la investigación viene del intake (§5, §6, §7) y entonces es derivación, o es invención con el mismo modo de falla de §5.10.4. Hay que declarar cuál |
| **03M** | AG-03M Maquetador de validación visual | **Adecuada y ejemplar** | Es el modelo a imitar: especialidad sin categoría propia, gatillada por flag (`requiere_maqueta`), con salida verificable por un humano. Precedente para §8.5 |
| **04** | AG-04 Ingeniero de Prompts | **Adecuada** | Gating por `usa_llm` bien resuelto. Trata el prompt como artefacto con contrato y evaluación, que es la respuesta correcta |
| **05** | AG-05 Arquitecto de Software | **Subespecificada por nivel** | Ver §8.6. La arquitectura de solución y la de proyecto son dos trabajos distintos bajo una sola especialidad y un alias |
| **06** | AG-06 Scrum Master (Backlog) | **Mal nombrada y con mandato excedido** | Dos problemas: se lo llama Product Owner en `Rules-Plan-Sprint` (§5.3), y arrastra mandato sobre la priorización (§8.2). Como curador de backlog e INVEST es adecuada |
| **07** | AG-07 Scrum Master (Sprints) | **Adecuada** | Traduce backlog priorizado en iteraciones. No origina prioridad, la recibe. Es la relación correcta con 06 |
| **08** | AG-08 QA / SDET | **Adecuada, y es el mejor caso del catálogo** | Es la única especialidad **adversarial por naturaleza**: su función es no creerle a la especificación. Por eso funciona sin necesitar un contrapeso externo |
| **09** | AG-09 DevOps | **Subespecificada por nivel** | Ver §8.6. El pipeline de un proyecto y la orquestación del build topológico entre proyectos son dos oficios |
| **10** | AG-10 Developer Advocate | **Adecuada** | Produce código que compila y corre, con contrato de verificación `VER-XX`. Su salida es ejecutable, que es la forma más fuerte de evidencia |
| **11** | AG-11 Technical Writer | **Adecuada** | Diátaxis, docs as code y ensayo de entrega. El modelo de documentación viva en tres momentos está bien resuelto |

### §8.5 Especialidades faltantes

Tres capacidades del producto no tienen especialidad que las sostenga, y las tres tienen ya el patrón de gating resuelto por `usa_llm` y `requiere_maqueta`:

| Especialidad propuesta | Gatillo | Hoy quién lo hace | Por qué no alcanza |
|---|---|---|---|
| **Product Owner** (upstream) | Siempre | Nadie declarado; en la práctica el humano más el integrador (§5.4) | Es el dueño de la priorización que hoy está triple-asignada (§8.2). Sin él declarado, no hay a quién escalar |
| **Especialista en Seguridad / AppSec** | `requiere_compliance` o `tiene_auth` | Repartido entre AG-05 (ADR de seguridad), AG-08 (tests) y AG-09 (supply chain) | En un producto regulado la seguridad es transversal y adversarial, como QA. Repartida entre tres constructores, nadie la ataca |
| **Modelador de Datos** | `tiene_persistencia` | AG-02 (modelo conceptual) y AG-05 (modelo lógico) | El modelo conceptual y el lógico son el mismo objeto en dos niveles, hoy partido entre dos especialidades que no comparten mandato |

De las tres, la primera es la más urgente porque resuelve §8.2. La segunda es la de mayor riesgo si el producto es regulado. La tercera es una mejora de coherencia, no un defecto.

### §8.6 Especialidades que deberían desdoblarse por nivel

Tres categorías declaran nivel «Proyecto + Solución» en el `README.md` del framework: 05, 09 y 11. En dos de ellas el trabajo cambia de naturaleza con el nivel y la especialidad no:

| Cat. | Nivel proyecto | Nivel solución | Hoy |
|---|---|---|---|
| **05** | Arquitectura interna: capas, componentes, ADR del proyecto | Contratos inter-proyecto, grafo de dependencias, vista de solución | Un solo AG-05, con «Solution Architect técnico» como alias |
| **09** | Pipeline del proyecto: build, test, gates, publicación | Orden de build topológico, coordinación entre productores y consumidores, versionado lockstep o independiente | Un solo AG-09 |
| **11** | Cuerpo documental del proyecto por rol de intervención | Artefactos de nivel solución, `AGENTS.md`, ensayo de entrega | Un solo AG-11, y acá **sí alcanza**: es el mismo oficio en dos alcances |

Para 05 y 09 la diferencia es de oficio, no de alcance: coordinar contratos entre proyectos es Solution Architect; coordinar el orden de build de un grafo es Release Engineer. Y bajo el modelo de cuatro niveles (§4), aparece un tercer nivel de aplicación —el proyecto de código— que hoy no tiene a quién asignarse.

### §8.7 Defectos verificados del catálogo

Tres, todos corregibles sin decidir nada:

1. **El diagrama de trazabilidad de `Marco-Teorico-SDD.md` §4.4 quedó con 10 y 11 invertidos.** Dice «AG-10 (Developer guide) → AG-11 (Ejemplos)», mientras la tabla §4.3 inmediatamente anterior asigna a AG-10 los ejemplos ejecutables y a AG-11 el cuerpo documental. El intercambio de categorías 10 ↔ 11 del 2026-07-26 actualizó la tabla y no el diagrama.
2. **La tabla §4.3 apunta a `03-UX-UI/`**, y la carpeta real es `03-UX-UI-DX/`.
3. **AG-06 con dos nombres**, ya registrado en §5.3.

### §8.8 Asignación propuesta

Resumen de la evaluación, con el cambio respecto de lo vigente:

| Cat. | Especialidad propuesta | Cambio |
|---|---|---|
| — | **Product Owner** (humano más integrador, upstream del intake) | **Nueva.** Dueño único de la priorización y las exclusiones |
| ROOT | AG-ROOT Arquitecto de Soluciones | Sin cambio |
| 00 | AG-00 Product Manager, sin mandato de arbitraje | Responsabilidad partida (§5.10.3) |
| 01 | AG-01 Analista de Negocio, deriva prioridad, no la origina | Reserva de §8.2 |
| 02 | AG-02 Analista Funcional, con Modelador de Datos si `tiene_persistencia` | Multi-especialidad condicionada |
| 03 | AG-03 UX/UI o DX, con origen de la investigación declarado | Aclaración de §8.4 |
| 03M | AG-03M Maquetador, si `requiere_maqueta` | Sin cambio |
| 04 | AG-04 Ingeniero de Prompts, si `usa_llm` | Sin cambio |
| 05 | AG-05 Arquitecto de Software (proyecto) / **Arquitecto de Soluciones** (solución) | Desdoblado por nivel |
| 06 | AG-06 Scrum Master de backlog, sin mandato de priorización | Nombre unificado y mandato acotado |
| 07 | AG-07 Scrum Master de sprints | Sin cambio |
| 08 | AG-08 QA / SDET, con **AppSec** si `requiere_compliance` o `tiene_auth` | Multi-especialidad condicionada |
| 09 | AG-09 DevOps (proyecto) / **Release Engineer** (solución) | Desdoblado por nivel |
| 10 | AG-10 Developer Advocate | Sin cambio |
| 11 | AG-11 Technical Writer, ambos niveles | Sin cambio |

El instrumento de §8.3 —registro de decisión de producto— es transversal y no pertenece a ninguna especialidad: lo emite quien decida, con el mismo criterio con que AG-05 emite un ADR.

### §8.9 Qué se gana

La evaluación no propone más agentes por gusto. Cada cambio cierra un modo de falla concreto ya identificado en este informe:

| Cambio | Modo de falla que cierra |
|---|---|
| PO declarado como dueño único de la priorización | Tres agentes inventando la misma decisión por separado (§8.2) |
| AG-00 sin mandato de arbitraje | Invención plausible que se propaga por once categorías y que el audit no detecta (§5.10.4) |
| Registro de decisión de producto | Imposibilidad de distinguir lo decidido de lo completado |
| Desdoblamiento de 05 y 09 por nivel | Trabajo de solución hecho con criterios de proyecto: contratos y orden de build tratados como detalle interno |
| AppSec gatillada | Seguridad repartida entre tres constructores y atacada por ninguno |
| Origen de la investigación de AG-03 declarado | El mismo modo de falla de §5.10.4, un nivel más abajo |
