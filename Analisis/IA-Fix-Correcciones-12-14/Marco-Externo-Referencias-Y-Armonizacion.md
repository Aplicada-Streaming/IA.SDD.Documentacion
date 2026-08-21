# Marco externo — Cómo resuelven estos dos problemas la industria y la academia

**Documento:** Marco-Externo-Referencias-Y-Armonizacion.md
**Versión:** 1.0
**Fecha:** 2026-08-20
**Para:** `Plan-12-14.md` 0.3, partes 1 y 2
**Regla aplicada:** toda afirmación de esta nota lleva su fuente. Lo que es razonamiento propio y no
cita está marcado como tal.

---

# Parte 1 — Referencias entre documentos sin depender de rutas

## 1.1 El problema tiene nombre y tres estandarizaciones independientes

**El patrón se llama *direccionamiento indirecto*: se cita un identificador lógico y un catálogo lo
resuelve a una ubicación física.** Tres estándares lo resolvieron por separado y llegaron a la misma
forma:

| Estándar | Qué define | Fuente |
|---|---|---|
| **OASIS XML Catalogs** 1.1 (2005) | *«La entrada lógica de un procesador de catálogo es un identificador externo o una referencia URI, y la salida lógica es una referencia URI»*. El mapeo vive en **archivos de catálogo**, y permite mapear identificadores abstractos a URIs concretas **sin tocar los documentos** | [xmlcatalogs.org](https://xmlcatalogs.org/catalogs-1.1) · [OASIS](https://www.oasis-open.org/committees/entity/spec-2001-08-06.html) |
| **DITA `keyref`** (OASIS DITA 1.2+) | En vez de referir la dirección del recurso, se refiere una **clave**, y la clave refiere la dirección. Las claves se definen **en el mapa**, no en cada tópico | [OASIS DITA](https://docs.oasis-open.org/dita/v1.2/cd03/spec/archSpec/keyref.html) · [dita-lang.org](https://dita-lang.org/dita/archspec/base/using-keys-for-addressing) |
| **Antora Resource ID** | Cinco coordenadas —versión, componente, módulo, familia, archivo—. *«No hace falta preocuparse por la ubicación física del archivo en disco»*, y **elimina explícitamente el problema de las rutas `../../`** | [Antora Docs](https://docs.antora.org/antora/latest/page/resource-id-coordinates/) · [Resource IDs](https://docs.antora.org/antora/latest/page/resource-id/) |

## 1.2 Los tres beneficios que las fuentes declaran, contra el estado de SDD

| Beneficio declarado | Fuente | SDD hoy |
|---|---|---|
| **Gestión centralizada**: la ubicación se define en un solo lugar | DITA | **41** rutas `../IA.SDD/` escritas a mano, **22** en normativos |
| **Si el recurso se mueve, la ruta se actualiza sólo en el mapa** | DITA | Se actualizarían 41 lugares |
| **Validación en tiempo de construcción** contra un sistema de archivos virtual | Antora | **No existe**: el subagente recibe la ruta como texto y nadie comprueba que la haya leído |

## 1.3 Y SDD ya tiene la mitad del mecanismo de DITA

DITA define un **fallback**: *«si la clave referida no está definida en el mapa, el destino vuelve al
valor del atributo `href`»* ([Adobe/DITA 1.3](https://help.adobe.com/en_US/framemaker/using/using-framemaker/dita-1.3-source/archSpec/base/key-based-addressing.html)).

**`../IA.SDD/` es exactamente ese `href` de reserva**, y está declarado como tal:
`PROMPT-Agente-Bootstrap-SDD.md:52` dice *«la forma `../IA.SDD/` es el caso particular en que fuente y
destino son hermanas; **no es un requisito**»*.

**Lo que falta es la otra mitad: la clave y el mapa.** *(Razonamiento propio.)*

## 1.4 Con código y sin código, que es la pregunta concreta

**La indirección no necesita código; la validación sí.** Es la línea que separa los tres estándares:

- **Sin código.** Un catálogo es **un archivo de datos**, no un programa — XML Catalogs es un formato,
  y las claves de DITA se definen en un mapa que es un documento. Un `Catalogo-De-Referencias.md` con
  la tabla `clave → ruta`, más la regla de que toda cita usa claves, **es implementable hoy y con cero
  ejecutables**.
- **Con código.** Lo que ningún catálogo da por sí solo es **comprobar que las referencias resuelven**.
  Antora lo hace en tiempo de construcción contra un sistema de archivos virtual. Sin programa, esa
  comprobación **la ejecuta un agente y no es garantía**. *(Razonamiento propio: es la misma frontera
  que el reporte `12` eleva al Product Owner.)*

**Sobre generar el resolver con un agente en cada corrida** *(razonamiento propio, sin fuente)*: la
industria **pinnea** su herramienta de construcción, no la regenera en cada build —Antora se instala
como dependencia versionada—. Un programa generado de nuevo cada vez es **un artefacto sin versión, sin
control de cambios y sin auditoría**, es decir, exactamente lo que el framework le prohíbe a todo lo
demás. Si se decide ir con código, corresponde **un resolver versionado en el árbol**, con su fila en
el control de cambios, y no uno generado al vuelo.

---

# Parte 2 — Reglas contradictorias, armonización y propagación

## 2.1 La literatura no busca consistencia permanente: busca inconsistencia gestionada

**Nuseibeh, Easterbrook y Russo**, *«Making inconsistency respectable in software development»*,
Journal of Systems and Software 58(2):171-180, 2001:

- Definen inconsistencia como *«cualquier situación en la que dos descripciones no obedecen alguna
  relación que se prescribe que debe existir entre ellas»*.
- Y sostienen que **mantener consistencia todo el tiempo es contraproducente**: *«en muchos casos puede
  ser deseable tolerar o incluso alentar la inconsistencia»* para no comprometer decisiones de diseño
  prematuramente. La inconsistencia pasa a ser **una actividad de gestión**, no un error a eliminar.

Fuente: [PDF](https://static.aminer.org/pdf/PDF/000/996/952/making_inconsistency_respectable_in_software_development.pdf) ·
[ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S016412120100036X)

**Consecuencia para el ciclo de armonización** *(razonamiento propio)*: el ciclo **no termina cuando no
hay inconsistencias, sino cuando cada una está resuelta o declarada como tolerada con su motivo**. Es
exactamente la forma que `Master-Prompt.md` §10.1 ya tiene —*«cerró por decisión y no por criterio, con
la lista de lo que quedó abierto»*— y que la nota de coherencia todavía no tiene.

## 2.2 Cuando dos reglas se contradicen, hay principios estándar para decidir cuál gana

**LegalRuleML** (OASIS) formaliza corpus normativos sobre **lógica defeasible**, donde *«una regla
defeasible es una regla cuya conclusión puede ser derrotada por otras reglas»*, y declara los tres
principios de resolución:

| Principio | Qué dice |
|---|---|
| **Lex superior** | Ante conflicto entre normas de distinta fuente, gana la de **jerarquía superior** |
| **Lex specialis** | La norma que gobierna **la materia específica** desplaza a la general |
| **Lex posterior** | La norma **emitida después** desplaza a la anterior |

Fuentes: [LegalRuleML Core Spec 1.0](https://docs.oasis-open.org/legalruleml/legalruleml-core-spec/v1.0/cs02/legalruleml-core-spec-v1.0-cs02.html) ·
[Lex specialis](https://en.wikipedia.org/wiki/Lex_specialis)

**Y esto tiene una consecuencia incómoda para lo ya aplicado.** El conflicto entre
`Rules-Prompts-AI.md` §4.2 punto 9 y `Root-Rules.md` §12.2 se resolvió a favor de §12.2 por dos
motivos que son, sin nombrarlos, **lex superior** —§12.2 es transversal, la otra es de categoría— y
**lex posterior** —§12.2 se publicó después—. **La resolución fue correcta y el corpus no declara
ninguno de los dos principios.** El próximo conflicto se resuelve otra vez por intuición.

## 2.3 Un sistema que combina reglas declara su algoritmo de combinación

**XACML 3.0** (OASIS) define **doce operadores estándar** de combinación —`deny-overrides`,
`permit-overrides`, `first-applicable`, `only-one-applicable`, `deny-unless-permit`,
`permit-unless-deny`, y sus variantes ordenadas—. El punto no es cuál se elige: es que **el algoritmo
se declara**, en lugar de resolverse caso por caso.

Fuente: [XACML 3.0 core spec](https://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-cd-03-en.html) ·
[Introducción](https://is.docs.wso2.com/en/5.9.0/learn/introduction-to-xacml-3.0-policies/)

## 2.4 La propagación de cambios explota si las relaciones no tienen semántica

La literatura de **trazabilidad y análisis de impacto** identifica como problema central la
**explosión de impactos**: propagar por los enlaces sin distinguir qué tipo de relación es cada uno
produce falsos positivos, y las técnicas que funcionan **usan semántica formal de las relaciones y
tipos de cambio** para podarlos.

Fuente: [Requirement-Centric Traceability for Change Impact Analysis](https://link.springer.com/chapter/10.1007/978-3-540-79588-9_10) ·
[Automating impact analysis through event-based traceability](https://link.springer.com/article/10.1007/s00766-003-0175-z)

**Contra el barrido de SDD** *(razonamiento propio)*: el barrido propaga por **coincidencia léxica**,
sin tipo de relación. Por eso la partición de §12 en §12.1 y §12.2 se aplicó como reemplazo masivo y
**una de las trece ocurrencias cayó del lado equivocado**. Y el reporte `02` ya había medido la otra
cara: una **matriz de propagación cerrada** convierte cada caso no previsto en omisión silenciosa.

---

# Parte 3 — Qué se lleva SDD de todo esto

| # | Del marco externo | A SDD | ¿Código? |
|---|---|---|---|
| **E1** | Catálogo de identificadores lógicos (XML Catalogs, `keyref`, Resource ID) | Un **catálogo de referencias** en el árbol; las citas usan claves; `../IA.SDD/` queda como **fallback declarado**, que es la figura de DITA | **No** |
| **E2** | Validación de resolución en tiempo de construcción (Antora) | Comprobar que toda clave resuelve, y que el subagente **leyó** lo que se le entregó | **Sí** — es el reporte `12` |
| **E3** | Principios de resolución de conflictos (LegalRuleML) | Declarar **lex superior, specialis y posterior** en el corpus. Funda lo que hoy se resuelve por intuición | **No** |
| **E4** | Algoritmo de combinación declarado (XACML) | Que el corpus diga **cómo se combinan** dos reglas aplicables, en vez de decidirlo caso por caso | **No** |
| **E5** | Inconsistencia gestionada, no eliminada (Nuseibeh et al.) | El ciclo de armonización cierra con **cada inconsistencia resuelta o tolerada con su motivo** — la forma de §10.1 | **No** |
| **E6** | Semántica de las relaciones para podar la propagación | Tipar la relación entre reglas —**cita, deriva, refina, deroga**— y propagar según el tipo | **No** |

**Cinco de las seis no necesitan una línea de código.** La única que sí es **E2**, y es exactamente la
decisión que el reporte `12` tiene elevada.

---

# Parte 4 — La convergencia entre varios agentes, y el rol integrador

## 4.1 El rol existe, tiene tres nombres y la misma función en las tres tradiciones

| Tradición | Cómo se llama | Qué hace, según la fuente |
|---|---|---|
| **Inspección de software** (Fagan 1976, base de **IEEE 1028**) | **Moderador** | *«Gestiona todo el proceso y la reunión»*, es técnicamente competente y **estimula la participación de todos**. En el **seguimiento**, es **responsable de verificar que los defectos se corrigieron** |
| **Método Delphi** | **Facilitador** / equipo monitor | Compila las respuestas del panel en un estadístico de resumen y **las devuelve a cada panelista junto con su propia respuesta previa**, sin atribución individual |
| **Sistemas multiagente con LLM** | **Integrador** | *«Consolida la retroalimentación de múltiples auditores y sintetiza los hallazgos en recomendaciones coherentes de mejora»* |

Fuentes: [Fagan / peer review](https://grokipedia.com/page/Software_peer_review) ·
[IEEE 1028-1988](https://ieeexplore.ieee.org/document/29123/) ·
[Delphi: rondas y facilitador](https://casrai.org/guides/delphi-method) ·
[Iterative Audit Convergence in LLM-Managed Multi-Agent Systems](https://arxiv.org/pdf/2605.12280)

**El paper de sistemas multiagente nombra los tres roles por separado y les da funciones distintas:**
**Auditor** —examina y detecta—, **Integrador** —consolida y sintetiza— y **Orquestador** —coordina el
flujo y **decide cuándo se cumplió el criterio de convergencia**—.

**Lo que esto fija sobre el papel del integrador** *(razonamiento propio, apoyado en las tres fuentes)*:
**el integrador no audita**. No produce hallazgos nuevos: consolida los de varios auditores, detecta
**contradicciones entre ellos** y prepara la decisión. Mezclar los dos roles destruye lo que el
auditor aporta —la mirada que no participó de lo que revisa—.

## 4.2 El criterio de corte: estabilidad, no exactitud

**Delphi.** El proceso *«continúa hasta alcanzar un criterio de detención predefinido: completar un
número de rondas, alcanzar consenso, o **establecer estabilidad en los resultados**»*. La
**estabilidad** —la consistencia de las respuestas **entre rondas sucesivas**— se considera *«un
criterio necesario para evaluar el consenso»*. El consenso se define con un umbral **fijado de
antemano**, y **dos a tres rondas suelen alcanzar**.

**Fagan / IEEE 1028.** Define **criterios objetivos de entrada y de salida**, y una asimetría útil:
los defectos triviales **se aceptan sin verificación**, y los no triviales **exigen re-inspección
completa**.

**Multiagente con LLM.** Detiene cuando *«todos los problemas identificados tienen resolución, las
rondas sucesivas muestran defectos nuevos mínimos, las métricas se estabilizan, o se alcanza el
máximo de iteraciones»*, y evalúa típicamente **3 a 5 iteraciones** para observar **rendimientos
decrecientes**.

**Y esto es exactamente lo que `Master-Prompt.md` §10.1 ya declara para los destinos:** *«dos rondas
seguidas sin ningún hallazgo de la clase interpretativa»*, **máximo cuatro rondas**, y si no se
alcanza, *«la fase cerró **por decisión y no por criterio**, con la lista de lo que quedó abierto»*.
**Es estabilidad + tope de rondas + no-consenso declarado, que son las tres piezas de Delphi.**
Lo que falta no es el criterio: **es que alcance a las intervenciones sobre el framework, y que exista
el integrador.**

## 4.3 El modelo de la legislatura tiene nombre en ingeniería de requisitos: **WinWin**

**Boehm**, modelo **WinWin** / Theory W. Cuatro elementos:

| Elemento | Qué es |
|---|---|
| **Win condition** | Lo que cada parte declara que necesita |
| **Issue** | **Cuando no hay acuerdo, el conflicto se registra como issue**, con las condiciones en conflicto |
| **Option** | Las resoluciones posibles de ese issue |
| **Agreement** | **Adopta la opción elegida** y reconcilia las condiciones involucradas |

Fuentes: [Requirement negotiation (overview)](https://www.sciencedirect.com/topics/computer-science/requirement-negotiation) ·
[EasyWinWin (ACM)](https://dl.acm.org/doi/pdf/10.1145/503271.503265) ·
[WinWin Spiral, caso de estudio](https://dl.acm.org/doi/10.1109/2.689675)

**Es el escenario de la legislatura, formalizado:** asesores que proponen, conflictos que se
**registran en vez de resolverse en el momento**, opciones que se debaten, y un **acuerdo que deja
escrito cuál opción se adoptó**.

**Contra SDD** *(razonamiento propio)*: los conflictos que esta corrida encontró —§4.2.9 contra §12.2,
la comprobación 2, la 4 mal enunciada— **se resolvieron sin dejar issue, sin opciones enumeradas y sin
acuerdo registrado**. Quedó el resultado y se perdió la deliberación, que es lo que el próximo
necesita para no rediscutirlo.

## 4.4 Qué se lleva SDD

| # | Del marco externo | A SDD | ¿Código? |
|---|---|---|---|
| **E7** | **El integrador consolida, no audita** (Fagan, Delphi, multiagente) | Un rol nuevo en el orquestador: recibe los informes de los auditores, **detecta contradicciones entre ellos**, y emite un acta. **No produce hallazgos propios** | **No** |
| **E8** | **Auditor especializado por materia** — Fagan coordina *«equipos pequeños y especializados»* | Que el orquestador elija el auditor **por la materia del diagnóstico**, y no uno genérico | **No** |
| **E9** | **Corte por estabilidad, con umbral declarado de antemano** (Delphi) | Extender §10.1 a las intervenciones sobre el framework. **El margen aceptable se declara antes de la primera ronda**, no después | **No** |
| **E10** | **Entrada/salida objetivas y re-inspección sólo de lo no trivial** (Fagan/IEEE 1028) | Que la ronda siguiente **no revise todo**: sólo lo no trivial | **No** |
| **E11** | **Issue → Option → Agreement** (WinWin) | Todo conflicto entre reglas deja **issue con sus opciones** y **acuerdo con la opción adoptada**. Es lo que hoy se pierde | **No** |
| **E12** | **Inconsistencia tolerada con su motivo** (Nuseibeh et al.) | El acta cierra con lo resuelto **y lo tolerado**, cada uno con su fundamento | **No** |

**Ninguna de las seis necesita código.**
