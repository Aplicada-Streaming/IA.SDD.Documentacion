# Nota de coherencia — Normalización del versionado y del archivado (framework 4.0)

**Proyecto:** Framework SDD
**Documento:** `Nota-Coherencia-Normalizacion-Versionado-v1.0.md`
**Versión:** 1.3
**Estado:** Vigente
**Fecha:** 2026-07-28
**Autor:** Revisión SDD (Claude Code)
**Repositorio intervenido:** `/IA/IA.SDD/`, changelog `[3.1]` → **`[4.0]`**, con `[3.2]` absorbida en la misma sesión
**Plan ejecutado:** `Plan-Normalizacion-Versionado-v1.0.md`
**Patrón seguido:** `Coherencia-Auditoria-Marco.md`, según exige `README.md` para toda intervención sobre varios archivos

---

## 1. Alcance

La versión `[4.0]` tiene **dos partes**, ejecutadas en la misma sesión y consolidadas en una sola entrada de changelog.

**Parte 1 — Normalización del versionado.** Las siete etapas del plan, más una E5b que se abrió al descubrir un defecto preexistente durante E5.

**Parte 2 — Fase de reconciliación normativa.** Seis etapas (F1 a F6) que cierran el hueco declarado como fuera de alcance en la Parte 1: el orquestador ya puede leer con qué versión del framework se generó un destino y proponer qué hacer al respecto.

**Objetivo:** que no convivan dos lógicas de versionado dentro del mismo plano. El criterio lo fijó el responsable del framework y motivó toda la intervención.

**Regla resultante:** en la carpeta de trabajo hay un solo archivo por nombre lógico, sin sufijo de versión; la versión vive en la cabecera; al ser superado, el archivo se copia completo a `_legacy/`, donde la copia sí recibe el sufijo.

Con la Parte 2 ya no queda nada fuera de alcance: el consumidor de la procedencia y de `_legacy/` existe.

---

## 2. Inventario

**41 archivos, 1086 líneas agregadas, 829 eliminadas, 11 renombres con historial preservado.**

### 2.1 Invariantes

| Archivo | Cambio |
| --- | --- |
| `README.md` | **D4 y D5 reformuladas.** D4: el archivo vivo lleva nombre lógico estable y declara su versión en la cabecera; el sufijo identifica a las copias de `_legacy/`. D5: una sola versión vigente **es** un solo archivo por nombre lógico. Se agrega la nota que explica la duplicidad anterior, la fila de `_legacy/` en la anatomía y la fila de intervención para publicar una versión |

### 2.2 Renombrados

Once archivos pasan de `Nombre-v1.0.md` a `Nombre.md`, con `git mv`: `Marco-Teorico-SDD`, `Coherencia-Auditoria-Marco`, `Coherencia-Config-Esquema`, `Coherencia-Incorporacion`, `Coherencia-Panel-Monolitico` y los seis `Design-Rules-*`.

**163 referencias actualizadas** en 23 archivos. Los tres `Design-Rules` previstos del roadmap del índice también pierden el sufijo, porque ese listado existe para fijar la convención de nombre.

### 2.3 Normativos, todos con bump major

| Archivo | Antes | Después |
| --- | --- | --- |
| `Master-Prompt.md` | 3.7 | **4.0** |
| `Rules-Documentacion.md`, `Rules-Examples.md` | 2.1 | **3.0** |
| Los otros catorce archivos de reglas | 1.1 a 1.8 | **2.0** |

Suben major por el criterio de `README.md`: la documentación generada con la nomenclatura anterior deja de cumplir.

**751 nombres de artefacto normalizados** en 24 archivos, incluidas las dos guías de usuario, el prompt de entrada, las plantillas de intake y el marco teórico.

### 2.4 Añadidos

| Archivo | Contenido |
| --- | --- |
| `_legacy/README.md` | Mecánica del archivado por versión del framework: qué contiene, por qué el conjunto entero y no los archivos que cambiaron, para qué sirve, cuándo se crea, qué queda excluido y la regla de intocabilidad |
| `SOLUTION-MANIFEST-template.md` §1.1 | Bloque de procedencia del framework |
| `SDD-Development-Guide.md` §VI.5 | Versionado del framework como conjunto |

### 2.5 Preservado deliberadamente

| Qué | Por qué |
| --- | --- |
| Las entradas anteriores del `CHANGELOG.md` | `SDD-Development-Guide.md` §VI.2: «Las filas ya escritas no se reescriben, aunque un cambio posterior invalide lo que describen». Se habían reescrito durante E2 y se restauraron |
| Los archivos de `SDD/Devs/Bootstrap/` | La tabla I.2 de la misma guía: «Nunca se edita; se cita». Once referencias restauradas |

---

## 3. Verificación de invariantes

| Invariante | Resultado | Evidencia |
| --- | --- | --- |
| D1 idioma y registro | Cumple | Español rioplatense neutro técnico en todo el texto agregado |
| D2 encoding | Cumple | Los 41 archivos siguen en UTF-8 con LF, verificado con `file` |
| D3 nombres | Cumple | Los once renombres respetan Título-Con-Guiones; ningún nombre nuevo introduce acentos ni caracteres especiales |
| **D4 sufijo de versión** | **Modificada** | Pasa a regir sobre las copias archivadas. Ningún archivo del framework lleva sufijo en el nombre: verificado con `find` |
| **D5 una sola versión vigente** | **Modificada** | El principio se conserva y se vuelve estructural. Ningún nombre de archivo afirma una versión que su cabecera pueda contradecir |
| D6 trazabilidad | Cumple | Cero enlaces relativos rotos sobre los 49 archivos, tras 163 reemplazos y once renombres |
| D7 neutralidad de dominio | Cumple | No se introdujo vocabulario ni ejemplo de ningún dominio de cliente |
| D8 tipos de proyecto | No aplica | El conjunto cerrado no se toca |
| D9 evidencia verificable | Cumple | La prohibición de tocar lo ya archivado se funda en D9 y se conserva en la política |

**Se modifican dos invariantes.** `README.md` lo declara el cambio de mayor impacto del framework y exige decisión explícita del responsable, que la dio, y esta nota de coherencia. Tres cosas acotan el costo: D5 se vuelve más fácil de cumplir, no más difícil, porque con un archivo por nombre lógico violarla requiere esfuerzo; no hay documentación previa que migrar, porque el responsable declaró que no se busca compatibilidad con lo anterior; y los árboles de cliente ya generados quedan como están, bajo la normativa con la que se hicieron, que es exactamente lo que el bloque de procedencia permite declarar de ahora en más.

---

## 4. Verificación de implantación

| # | Verificación | Resultado |
| --- | --- | --- |
| N-1 | Buscar `-v<X.Y>` en el árbol | Solo sobrevive en líneas que describen copias archivadas (23) y en registros históricos preservados a propósito (174) |
| N-2 | Listar nombres de archivo | **Cero** archivos con sufijo de versión en el nombre |
| N-3 | Contrastar el campo `Versión` contra el nombre | Ninguna contradicción es posible: el nombre ya no afirma una versión |
| N-4 | Resolver los enlaces internos | **Cero rotos** en 49 archivos |
| N-5 | Leer `_legacy/` como agente sin contexto | La mecánica está documentada; el primer snapshot corresponde a la 4.0 cuando sea superada |
| N-6 | Simular tres subidas de versión de un documento de cliente | La carpeta mantiene un archivo por nombre lógico y ninguna referencia entrante cambia |
| N-7 | Leer un `SOLUTION-MANIFEST` derivado | Declara la versión del conjunto y de cada regla aplicada, según §1.1 del formato |
| — | Integridad markdown | Cero filas de tabla huérfanas tras corregir trece casos introducidos por la propia intervención |

---

## 5. Observaciones

**Sobre el alcance real de E5.** El plan estimó 274 ocurrencias y resultaron 751. La medición del plan contó solo el patrón literal `-v<X.Y>` y no los nombres concretos. La etapa se ejecutó en tres pasadas: una mecánica que excluyó toda línea que mencionara `_legacy`, y dos manuales sobre las 27 líneas de prosa que definían la convención y que un reemplazo automático habría dejado sin sentido.

**Sobre el defecto preexistente que apareció durante E5.** Al menos seis reglas verificaban «Ningún archivo usa el patrón `-v<X.Y>.md`; todos usan `-v<X.Y>.md`», con los dos patrones idénticos, y `Rules-Contexto.md` daba un ejemplo inválido idéntico a los válidos. Verificado contra `HEAD`: es anterior a esta intervención. Una normalización previa había convertido el patrón prohibido `.v<X.Y>.md` en el permitido y había vaciado de sentido toda línea que los contrastaba. Un auditor que corriera esos checklists pasaba siempre. Quedaron reescritos contra la regla nueva.

**Sobre el snapshot inicial.** `_legacy/` arranca vacío y rige desde la 4.0 hacia adelante. No se fabricó un `_legacy/3.2/` porque ese estado ya no existe íntegro en el árbol y reconstruirlo produciría un snapshot que nadie va a consultar. Es el mismo criterio con que se incorporó D9.

**Sobre el reinicio de versiones.** El plan proponía reiniciar cada archivo a 1.0. No se aplicó: habría devuelto el marco teórico de 1.8 a 1.0 y descartado el rationale acumulado en las tablas de control de cambios, sin ganancia, porque el `[4.0]` del conjunto ya declara la discontinuidad. Es la única desviación respecto del plan aprobado y se declara acá para que el responsable la revierta si prefiere el reinicio.

**Sobre la decisión de qué registros históricos se preservan.** Se honraron las dos protecciones que el framework declara explícitamente: las filas del changelog y los archivos de `Bootstrap/`. Las notas de coherencia del catálogo de diseño **sí** quedaron con sus referencias actualizadas, porque viven en el árbol normativo y una de ellas se cita como patrón. Es un juicio de esta intervención y no una regla escrita del framework; conviene que el responsable lo confirme o lo revierta.

**Sobre lo que la intervención no habilita todavía.** El bloque de procedencia y `_legacy/` del framework quedan sin consumidor hasta que exista un flujo de re-evaluación. Hoy `Master-Prompt.md` solo ofrece, ante un `SDD/Docs/` con contenido previo, archivar todo y empezar de cero o abortar.

---

## 5.bis Parte 2 — Fase de reconciliación normativa

### Qué se agregó

`Master-Prompt.md` **§2.1, nueva**. Se dispara únicamente si `SDD/Docs/` del destino tiene contenido. Distingue tres casos —sin procedencia declarada, al día, y desfasado— y en el tercero arma un diff normativo sin despachar ningún subagente: lee la procedencia del manifiesto, lee las versiones vigentes de cabecera, clasifica cada salto por severidad leyéndola de la propia numeración, y para cada salto major enumera los artefactos que esa regla gobierna según su tabla maestra de documentos.

### Las tres salidas

| Salida | Efecto | Detención |
| --- | --- | --- |
| **A — Plan de adecuación** | Emite `SDD/Docs/Audit/Reconciliacion-<origen>-a-<vigente>.md`, documento por documento, con la regla que lo gobierna y el orden sugerido según la cadena D6. **No modifica nada** | Vuelve a detenerse al presentarlo: ejecutar el plan es una decisión aparte |
| **B — Regenerar desde cero** | Comportamiento histórico del prerrequisito 4. Archiva `SDD/Docs/` completo y regenera bajo la versión vigente | — |
| **C — Continuar bajo la versión de origen** | Usa las reglas de `_legacy/<version>/`, no las vigentes. **No se ofrece si ese conjunto no es reconstruible**, porque el orquestador no puede aplicar reglas que no puede leer | Registra la decisión en el manifiesto |

### Archivos alcanzados por la Parte 2

| Archivo | Cambio |
| --- | --- |
| `Master-Prompt.md` | §2.1 nueva; §0 reemplaza el prerrequisito 4; §3.5 declara el informe de reconciliación en `Audit/`; §7 suma la fase al inicio del recorrido |
| `SOLUTION-MANIFEST-template.md` | Tabla de decisiones de reconciliación, para registrar la salida C |
| `PROMPT-Agente-Bootstrap-SDD.md` | Prerrequisito 4 alineado |
| `SDD-Getting-Started-Guide.md` | Entrada de troubleshooting reescrita |
| `SDD-User-Guide.md` | Lista de fases y dos entradas de glosario |

### Verificación específica de la Parte 2

| # | Verificación | Resultado |
| --- | --- | --- |
| 1 | Las tres salidas A, B y C están declaradas y son distinguibles | Pasa |
| 2 | El artefacto de salida de A y la tabla de registro de C existen y se citan entre sí | Pasa |
| 3 | Ningún archivo conserva el prerrequisito 4 viejo («archivar o abortar» como únicas opciones) | Pasa, cero ocurrencias |
| 4 | La fase declara explícitamente que no modifica documentos y que no elige por su cuenta | Pasa |
| 5 | La salida C queda condicionada a que el conjunto de origen sea reconstruible | Pasa |

### Por qué no hay `_legacy/4.0/`

El plan preveía crear el snapshot de la versión superada al publicar la siguiente. Al intentarlo apareció un problema de hecho: `[3.2]` y `[4.0]` se produjeron en la misma sesión sobre el estado `[3.1]`, y ninguna llegó a existir como árbol publicado independiente. Un primer intento de snapshot produjo una copia que declaraba internamente la versión 3.6, o sea un registro falso.

Se descartó, y las dos partes se consolidaron en una sola versión `[4.0]`. El archivado por versión rige desde la 4.0 hacia adelante y su primera subcarpeta se creará cuando la 4.0 sea superada. Fabricar un snapshot reconstruido habría violado la regla de intocabilidad que el propio `_legacy/README.md` declara.

---

## 5.ter Parte 3 — Pasada de estabilización

Barrido de consistencia sobre el framework completo, posterior a las Partes 1 y 2 y dentro de la misma versión `[4.0]`. No introduce capacidades: cierra inconsistencias.

### Notas de coherencia readecuadas a las reglas vigentes

La Parte 1 las había dejado a medio camino: las filas de inventario nombraban los archivos sin sufijo y las filas que verificaban D4 seguían afirmando que esos mismos archivos lo llevaban. La nota se contradecía consigo misma.

Se evaluaron dos salidas —congelarlas como registro histórico, o readecuarlas— y **el responsable del framework eligió readecuarlas**. Las cuatro notas quedan consistentes con las reglas vigentes: referencias sin sufijo, celdas de D4 reexpresadas, y la reexpresión **declarada en la propia celda** con la aclaración de que la verificación original se hizo contra la D4 anterior. No se falsea lo que se verificó: se dice que se reexpresó y por qué.

De paso se corrigieron en esas notas dos defectos preexistentes: se autonombraban con un prefijo de guion bajo que ningún archivo tenía, y citaban `Guia-Usuario-SDD-v1.0.md`, un archivo que nunca existió con ese nombre; la guía real es `SDD-User-Guide.md`.

### Contradicción de política reconciliada

La decisión anterior abría una contradicción con `README.md`, que declara que las notas emitidas antes de D9 «siguen diciendo D1-D8, y es correcto que lo hagan». Se agregó el párrafo que separa los dos ejes. El **alcance** de lo que una nota verificó no se toca nunca: una nota que verificó D1 a D8 sigue diciendo D1 a D8. Una **verificación concreta** se reexpresa cuando, y solo cuando, quedaría falsa contra el árbol vigente o citaría un archivo inexistente; no alcanza con que la invariante haya cambiado de forma.

La versión 4.0 ilustra la diferencia y por eso quedó escrita en el `README.md`: D4 y D5 se reformularon las dos, pero solo las celdas de D4 se reexpresaron. Las de D4 afirmaban que ciertos archivos llevaban sufijo de versión en el nombre, lo que dejó de ser cierto. Las de D5 afirmaban que había un único archivo por nombre lógico sin copias paralelas, lo que sigue siendo cierto bajo la formulación nueva, y quedaron intactas. Sin esta distinción el framework tenía dos prácticas opuestas para el mismo tipo de artefacto.

### Otros defectos preexistentes corregidos

| Defecto | Evidencia | Corrección |
| --- | --- | --- |
| `§6.5` de `Maqueta-Rules.md` no existe. §6 es una lista numerada sin subsecciones, y la verificación de ofuscación es su punto 5. Tres archivos lo citaban | Verificado contra `HEAD`: preexistente | `§6 punto 5` en los tres |
| `Master-Prompt.md` §0 titulaba «Modelo de dos repositorios» mientras el `README.md` y la guía de arranque declaran tres | Contradicción entre documentos vigentes | Reescrito: el framework opera sobre tres, el orquestador sobre dos de ellos, y el tercero no lo toca nunca |
| «aplicando las validaciones de §3.1» era ambiguo: se leía como si fueran del formato del manifiesto | Ambigüedad de lectura | «§3.1 de este master-prompt» |

### Verificación de la pasada

| # | Verificación | Resultado |
| --- | --- | --- |
| 1 | Enlaces relativos en los 50 archivos | **cero rotos** |
| 2 | Tablas markdown | **cero filas huérfanas** |
| 3 | Archivos con sufijo de versión en el nombre | **cero** |
| 4 | Referencias a secciones de otros archivos | Todas resuelven; los dos hits restantes se verificaron uno por uno y son falsos positivos del detector |
| 5 | Texto obsoleto de la convención anterior (cuatro patrones distintos) | **cero ocurrencias** |
| 6 | Encoding y EOL | **cero** archivos fuera de UTF-8 con LF |

Ningún archivo sube de versión por esta pasada: las correcciones son de clase errata según `README.md`, y las notas de coherencia declaran su reexpresión en línea. Todo queda dentro de `[4.0]`, que todavía no se publicó.

---

## 5.quater Parte 4 — Navegabilidad y anexos de datos del intake

Origen distinto del resto: no sale de un defecto sino de contrastar la plantilla contra **dos intakes reales**, de soluciones sin relación entre sí, de 4023 y 2112 líneas.

### Qué ya estaba y qué faltaba

La **Parte D ya existía** en la plantilla 1.3, con la regla de autocontención y la cita por identificador, y los dos intakes la siguieron. No hubo que crearla. Lo que faltaba:

| # | Hueco | Evidencia |
| --- | --- | --- |
| 1 | Sin tabla de contenido, ni en la plantilla ni en los dos intakes reales | Cuatro reglas exigen TOC a los documentos **generados**; el intake, que es el que más agentes leen, quedaba exento. Asimetría sin justificación |
| 2 | El formato por escenario pedía tres piezas; los dos intakes agregaron tres bloques más por su cuenta | **Contexto**, **qué ejercita** y **qué verificar**, los tres en ambos documentos, de forma independiente |
| 3 | El enum de `Estado` divergía | Plantilla: `verificado / propuesto / reconstruido`. Uso real: `medido / declarado / derivado / reconstruido` |
| 4 | `Intake-Rules.md` no mencionaba la Parte D | La regla de autocontención existía declarada desde la 1.3 y **ninguna validación la verificaba** |

El hueco 4 es del mismo tipo que los que motivaron toda esta revisión: una regla escrita que ningún actor comprueba.

### Qué se aplicó

- `SOLUTION-INTAKE-template.md` (1.3 → **1.4**): TOC obligatorio con los escenarios listados por identificador; formato por escenario de tres piezas a cinco; `Estado` como enum cerrado de cuatro valores, que es D9 aplicada a los datos de ejemplo; recomendación de encadenar los escenarios como una línea de tiempo.
- `Intake-Rules.md` (2.0 → **2.1**): §5 valida la Parte D, el TOC, la regla de resolución de identificadores en las dos direcciones y la de autocontención.

### Verificación de D7

Los dos intakes de referencia pertenecen a dominios concretos. Se verificó que ninguno de sus términos se filtrara a los artefactos normativos: **cero ocurrencias** de vocabulario, identificadores o stack de esas soluciones en la plantilla y en la regla. Lo que se incorporó es la forma, no el contenido.

---

## 6. Veredicto

**CONFORME.** Las siete etapas del plan más E5b, y las seis de la Parte 2, están implantadas. Las verificaciones pasan: cero enlaces rotos, cero tablas mal formadas, cero archivos con sufijo de versión en el nombre, las tres salidas de la reconciliación declaradas y consistentes entre los cinco archivos que las citan. Las dos invariantes modificadas lo fueron con decisión explícita del responsable y quedan declaradas acá.

Queda abierta una sola decisión menor: si se confirma el criterio de §5 sobre las notas de coherencia del catálogo de diseño.

---

## Control de cambios

| Versión | Fecha | Cambios | Autor |
|---|---|---|---|
| 1.3 | 2026-07-28 | Se incorpora la Parte 4: navegabilidad y anexos de datos del intake, sintetizados del patrón que dos intakes reales desarrollaron por su cuenta. Tabla de contenido obligatoria, formato por escenario de tres piezas a cinco con el bloque de verificación que convierte el JSON en fixture, `Estado` como enum cerrado alineado con D9, y validación de la Parte D en `Intake-Rules`, que hasta acá no verificaba ninguna de las dos reglas que la plantilla declaraba. D7 verificado: cero filtraciones de los dominios de referencia. | Revisión SDD (Claude Code) |
| 1.2 | 2026-07-28 | Se incorpora la Parte 3, de estabilización: las cuatro notas de coherencia readecuadas a las reglas vigentes por decisión del responsable, con la reexpresión declarada en cada celda; la contradicción de política que eso abría, reconciliada en el `README.md` separando el alcance de una verificación de su forma; y tres defectos preexistentes corregidos, entre ellos el `§6.5` inexistente de `Maqueta-Rules.md` citado desde tres archivos y el «modelo de dos repositorios» del master-prompt contra los tres que declara el README. Seis verificaciones, todas en cero. | Revisión SDD (Claude Code) |
| 1.1 | 2026-07-28 | Se incorpora la Parte 2 de la versión `[4.0]`: la fase de reconciliación normativa de `Master-Prompt.md` §2.1, con sus tres salidas, los cinco archivos alcanzados y sus cinco verificaciones propias. Se documenta por qué no existe `_legacy/4.0/`: `[3.2]` y `[4.0]` se produjeron en la misma sesión sobre `[3.1]` y no hubo árbol publicado intermedio que preservar; un primer intento de snapshot produjo una copia que declaraba la versión 3.6 y se descartó por falso. Las dos partes quedan consolidadas en una sola entrada de changelog. | Revisión SDD (Claude Code) |
| 1.0 | 2026-07-28 | Nota de coherencia de la normalización del versionado y del archivado. Inventario de 41 archivos con once renombres, 163 referencias y 751 nombres de artefacto normalizados. Verificación de las nueve invariantes, con D4 y D5 modificadas. Ocho verificaciones de implantación. Seis observaciones, incluidas la desviación respecto del reinicio de versiones y el defecto preexistente de los checklists de D4. Veredicto CONFORME. | Revisión SDD (Claude Code) |
