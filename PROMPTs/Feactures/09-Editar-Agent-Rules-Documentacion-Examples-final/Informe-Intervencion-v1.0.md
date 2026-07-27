# Informe de intervención — Reordenamiento de categorías 10 ↔ 11 y cuerpo documental de entrega

**Proyecto:** Framework SDD
**Documento:** Informe-Intervencion-v1.0.md
**Versión:** 1.1
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD
**Repositorio intervenido:** `/IA/IA.SDD/`

---

## 1. Resumen ejecutivo

La intervención se ejecutó completa, en nueve etapas, todas con veredicto **CONFORME**. Tocó veintiséis archivos: veinticuatro modificados, uno renombrado y uno que pasó de existir vacío a tener contenido. El framework quedó en la versión **3.0** de su changelog.

Las ocho etapas de la segmentación original (E0 a E7) cubrieron las solicitudes del prompt. La novena (E8) se abrió después, para resolver las siete decisiones que este informe había dejado abiertas en su versión 1.0. **No quedan decisiones pendientes.**

Los tres déficits que motivaban el trabajo quedaron corregidos:

| Déficit | Corrección |
| --- | --- |
| El framework modelaba un solo rol de intervención externo, y ningún artefacto servía al mantenedor ni al operador | La categoría 11 se reorganiza en tres cuerpos por rol de intervención. El cuerpo mantenedor pasa a ser **obligatorio para los ocho tipos D8** |
| Los criterios de calidad exigían verificaciones imposibles en una fase pre-código | La generación se parte en pasadas: el criterio se declara antes, se verifica después. Fases G, I y J |
| Los ejemplos tenían una sola arista, ilustrativa | Doble arista: cada sample declara qué ilustra y **qué verifica**, con contrato `VER-XX` de aserción evaluable |

No quedó ninguna solicitud sin ejecutar. Se tomaron siete decisiones que se elevaron al responsable del framework y se resolvieron durante la corrida, registradas como DEC-01 a DEC-07 en `Estado-Intervencion.md`.

La etapa E8 destapó además un defecto que las etapas anteriores no habían detectado: E2 dejó a `Rules-Calidad-Y-Pruebas.md` contradiciendo a `Deriva-Rules.md`, lo que anulaba en la práctica la extensión del sensado a proyectos sin interfaz visual. Está corregido y documentado en §7.1.

---

## 2. Archivos creados, renombrados, modificados y eliminados

### 2.1 Renombrado

| Antes | Después | Método | Etapa |
| --- | --- | --- | --- |
| `SDD/Devs/Rules/Rules-Developer-Guide.md` | `SDD/Devs/Rules/Rules-Documentacion.md` | `git mv`, historial preservado | E1 |

### 2.2 Creado con contenido

| Archivo | Estado anterior | Versión nueva | Etapa |
| --- | --- | --- | --- |
| `SDD/Guides/SDD-Development-Guide.md` | Archivo vacío de 0 bytes, nunca commiteado | 1.1 — nueve ejes de extensión | E6, E8 |

### 2.3 Modificados

| Archivo | Versión | Cambio principal | Etapas |
| --- | --- | --- | --- |
| `SDD/Devs/Rules/Rules-Documentacion.md` | 1.2 → **2.0** | Reescritura completa como cuerpo documental de entrega | E1, E3 |
| `SDD/Devs/Rules/Rules-Examples.md` | 1.2 → **2.0** | Doble arista y contrato de verificación | E1, E2 |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 3.4 → 3.6 | Intercambio de categorías y Fases I y J | E1, E4 |
| `SDD/Devs/Rules/Root-Rules.md` | 1.3 → 1.4 | Layout canónico y flujo de lectura | E1 |
| `SDD/Devs/Rules/Deriva-Rules.md` | 1.0 → 1.1 | Sondas `VER-XX` en el sensado de deriva | E2 |
| `SDD/Devs/Rules/Rules-Arquitectura-Tecnica.md` | 1.2 → 1.4 | Frontera con 11 y tabla de contenido | E3, E5 |
| `SDD/Devs/Rules/Rules-Calidad-Y-Pruebas.md` | 1.3 → **1.7** | Frontera con 11, vocabulario, tabla de contenido y corrección de la contradicción con `Deriva-Rules.md` | E1, E3, E5, E8 |
| `SDD/Devs/Rules/Rules-Devops.md` | 1.3 → 1.6 | Frontera con 11, vocabulario y tabla de contenido | E1, E3, E5 |
| `SDD/Devs/Rules/Rules-Plan-Sprint.md` | 1.2 → 1.4 | Definition of Done y tabla de contenido | E4, E5 |
| `SDD/Devs/Rules/Rules-Contexto.md` | 1.3 → 1.5 | Vocabulario, trazabilidad y tabla de contenido | E1, E5 |
| `SDD/Devs/Rules/Rules-Necesidades-Negocio.md` | 1.2 → 1.4 | Vocabulario y tabla de contenido | E1, E5 |
| `SDD/Devs/Rules/Rules-UX-UI-DX.md` | 1.6 → 1.8 | Vocabulario y tabla de contenido | E1, E5 |
| `SDD/Devs/Rules/Rules-Prompts-AI.md` | 1.2 → 1.4 | Vocabulario y tabla de contenido | E1, E5 |
| `SDD/Devs/Rules/Rules-Especificacion-Funcional.md` | 1.2 → 1.3 | Tabla de contenido | E5 |
| `SDD/Devs/Rules/Rules-Backlog-Tecnico.md` | 1.2 → 1.3 | Tabla de contenido | E5 |
| `SDD/Devs/Rules/Intake-Rules.md` | 1.0 → 1.1 | Vocabulario de cabecera | E1 |
| `SDD/Devs/Rules/Maqueta-Rules.md` | 1.0 → 1.1 | Referencia a `10-Examples` | E1 |
| `SDD/Devs/Intake/SOLUTION-INTAKE-template.md` | 1.1 → 1.3 | Destinos de samples y documentación, versión de plantilla en cabecera | E1, E8 |
| `SDD/Devs/Guides/Marco-Teorico-SDD-v1.0.md` | 1.6 → 1.8 | Mapa visual, catálogo de especialidades, tabla Diátaxis, nomenclatura y referencia muerta | E1, E8 |
| `SDD/Guides/SDD-User-Guide.md` | 1.3 → 1.5 | Numeración, Fases I y J, paso 7, seis FAQ nuevas, mapa de carpetas | E1, E7 |
| `SDD/Guides/SDD-Getting-Started-Guide.md` | 1.0 → 1.2 | Limpieza de autosuficiencia y despersonalización del ejemplo aplicado | E1, E8 |
| `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md` | 2.1 → 2.2 | Nombre real de la carpeta de prompts | E1 |
| `README.md` | Sin versión (índice) | Reescrito como superficie de entrada | E6 |
| `CHANGELOG.md` | 2.5 → **3.0** | Entrada de la intervención | E7 |
| `SDD/Devs/References/Design/Design-Rules-Blazor-Mudblazor-v1.0.md` | Sin cambio de versión | Nomenclatura de invariantes | E8 |

### 2.4 Eliminados

Ninguno.

---

## 3. Resultado del barrido de propagación (S1.5)

Conteos medidos con `grep -o` sobre todos los `.md` del árbol. «Línea de base» es el estado en E0.

| Cadena | Línea de base | Estado final | Corregidas | En congelados | En control de cambios |
| --- | --- | --- | --- | --- | --- |
| `10-Developer-Guide` | 25 | 10 | 15 | 7 | 3 |
| `11-Examples` | 38 | 14 | 24 | 11 | 3 |
| `Rules-Developer-Guide` | 13 | 10 | 3 | 9 | 1 |
| `categoría 10` / `categoría 11` | 14 / 9 | — | 23 | 0 | 0 |
| `categoria 10` / `categoria 11` (sin tilde) | 0 / 0 | 0 | — | — | La variante sin tilde no existe en el repositorio |
| `Fase F` / `Fase G` | 5 / 4 | 5 / 4 | 9 | 0 | 0 |

**Cero ocurrencias en archivos vivos** de las tres primeras cadenas. Todo lo que resta está en material congelado o en filas de control de cambios.

Reasignación de subagentes (S1.6): `AG-10` pasa de Technical Writer a Developer Advocate y `AG-11` de Developer Advocate a Technical Writer / Documentation Lead. Los conteos totales suben respecto de la línea de base —`AG-10` de 25 a 31 y `AG-11` de 33 a 46— porque las filas de control de cambios nuevas citan ambos identificadores al describir la reasignación.

### 3.1 Omisiones deliberadas, con su motivo

| Qué no se tocó | Cuántas ocurrencias | Motivo |
| --- | --- | --- |
| `SDD/Devs/Bootstrap/` (7 archivos) | 27 | **DEC-01.** Auditorías de la construcción original del framework. Corregirlas falsearía el registro de lo que realmente se auditó |
| `SDD/Devs/Reformulacion/` (4 archivos) | — | **DEC-01.** Ídem, para la reformulación a modelo de solución |
| `SDD/Devs/Intake/_legacy/` (2 archivos) | — | **DEC-01.** Plantillas del modelo anterior, conservadas como referencia histórica |
| Filas de control de cambios ya escritas | 7 | Una fila de changelog describe lo que se hizo en una fecha; reescribirla haría que el registro mienta. Cada archivo afectado incorpora una fila nueva que declara el renombre |
| Entrada nueva del `CHANGELOG.md` | 3 | Describe el intercambio y necesita nombrar las carpetas viejas para que el lector entienda qué cambió |

**Criterio ampliado y declarado.** S1.5 pide corregir siempre las cuatro primeras cadenas. Las filas de control de cambios quedaron fuera de esa corrección, por el mismo principio que S1.7 aplica al vocabulario. Es una decisión de criterio que se declara acá para que no se lea como una omisión.

---

## 4. Resultado de la normalización de vocabulario (S1.7)

Conteos medidos con `grep -oi` sobre la raíz del término, de modo que `consumidor` incluye a `consumidores`.

| Término | Línea de base | Estado final | Sustituidas | Conservadas y por qué |
| --- | --- | --- | --- | --- |
| `consumidor` | 109 | 50 | 60 | Usos técnicos: proyecto consumidor del grafo de dependencias, US consumidora de una BT, consumer group de mensajería, componente que consume la salida de un prompt, superficie que consume un contrato de configuración o de identidad de versión |
| `constructor` | 1 | 2 | 1 | La única ocurrencia real se sustituyó por «mantenedor». Las dos actuales son citas del término dentro de filas de control de cambios |
| `audiencia` | 57 | 51 | 19 | Se conserva donde designa al público del producto (secciones UX, visión de producto) y donde nombra el contrato de doble audiencia de la categoría 11, que es su nombre propio |
| `implementador` | 12 | 15 | **0** | **DEC-04.** Designa la categoría de stakeholder del modelo RACI del intake (propietario / implementador / beneficiario), no un rol de intervención documental. El conteo sube porque tres filas de control de cambios explican por qué se conserva |

**Un hallazgo de criterio.** La tabla de correspondencia del prompt trataba «audiencia» como un término que cubría dos ejes ortogonales. En el repositorio cubre **tres**: rol de intervención, naturaleza del lector y público del producto. El tercero no es un actor documental y se conservó. Cuatro sustituciones se revirtieron durante E1 al detectarlo.

---

## 5. Resultado de la verificación de autosuficiencia

| Cadena | Línea de base (E0) | Estado final |
| --- | --- | --- |
| `IA.Prompts` | 0 | **0** |
| `PromptFramework` | 0 | **0** |
| `IA.SDD.Documentacion` | 0 | **0** |
| `PROMPTs/` | **12** | **0** |
| `Rule-Dual-Audience` | 0 | **0** |
| `Rule-Narrative-Voice` | 0 | **0** |
| `Rule-Markdown` | 0 | **0** |
| `Rule-Documentation` | 0 | **0** |
| `Rule-Evidences` | 0 | **0** |
| `Rule-Indexing` | 0 | **0** |
| `Study-Guide` | 0 | **0** |

**La restricción no se cumplía antes de esta intervención.** `PROMPTs/` tenía doce ocurrencias: diez en `SDD-Getting-Started-Guide.md`, una en `PROMPT-Agente-Bootstrap-SDD.md` y una en `Marco-Teorico-SDD-v1.0.md`. Además, la guía de arranque citaba en cinco lugares rutas absolutas a un repositorio de documentación nominado. Por **DEC-02** se amplió el alcance de E1 para limpiarlas: las rutas pasaron al placeholder `<RUTA-DOCUMENTACION>`, la carpeta se nombra `Prompts/`, y en los otros dos archivos se corrigió el nombre real de la carpeta de este repositorio, que es `PROMPTS/`.

### 5.1 URLs en `/IA/IA.SDD/`

Veintisiete URLs distintas, todas preexistentes. **Cero URLs externas introducidas.**

| URL | Estado |
| --- | --- |
| `https://docs.google.com/document/d/…/preview` | Preexistente. Es el único enlace externo del `README.md` y S9 mandaba conservarlo |
| `https://github.com/Aplicada-Streaming/IA.SDD.git` | Preexistente. Es el propio repositorio |
| `https://semver.org/`, `https://conventionalcommits.org/`, `https://cyclonedx.org/`, `https://slsa.dev/`, `https://diataxis.fr/`, `https://keepachangelog.com/…` | Preexistentes |
| `https://www.w3.org/TR/WCAG22/`, `https://www.w3.org/TR/wai-aria/`, `https://www.designcouncil.org.uk/` | Preexistentes |
| `https://scrumguides.org/`, `https://agilemanifesto.org/`, `https://dora.dev/`, `https://opentelemetry.io/` | Preexistentes |
| `https://opentelemetry.io/`, `https://csrc.nist.gov/…`, `https://datatracker.ietf.org/…rfc7807` | Preexistentes |
| Cinco URLs de `arxiv.org`, una de `agentfactory.panaversity.org`, una de `docs.anthropic.com` | Preexistentes |
| `http://localhost:8080`, `http://localhost:<puerto>` | Placeholders preexistentes, reutilizados |

El diff de la intervención muestra dos líneas agregadas con `http`: el placeholder `http://localhost:<puerto>` en `Rules-Documentacion.md`, que reutiliza una forma ya presente en `Maqueta-Rules.md`, y la URL de Google Docs en el `README.md`, que aparece como línea nueva solo porque el archivo se reescribió completo. **Ninguna URL externa nueva.**

### 5.2 Premisa incorrecta del prompt, para registro

Las Restricciones afirman que SemVer, Conventional Commits, CycloneDX y SLSA «aparecen nombrados y nunca enlazados». Es falso: están enlazados desde antes de esta intervención. La regla de nombrar sin enlazar sigue siendo aplicable a todo contenido nuevo y se respetó —Diátaxis, C4, arc42, `AGENTS.md`, Game Day, postmortem sin culpa, Living Documentation, Docs as Code, Continuous Documentation y Definition of Done se nombran sin URL—, pero su justificación no se sostiene.

---

## 6. Inconsistencias detectadas que exceden el alcance de este prompt

Se distinguen hechos verificados de interpretaciones.

### 6.1 Hechos

1. **Existe una invariante D9** que el prompt no menciona. `Deriva-Rules.md` §1 define D9, evidencia verificable, incorporada con el sensado de deriva y registrada en el changelog. El prompt habla de «invariantes D1–D8» en el Contexto, en las Reglas y en la lista de comprobación de las notas de coherencia. Se adoptó verificar **D1 a D9** en todas las etapas.
2. **El framework se refería a sus invariantes como «D1-D8» de forma mayoritaria** (corregido en E8), pese a que son nueve. La nomenclatura quedó desactualizada al incorporarse D9 y no se normalizó entonces.
3. **La sección de anti-patrones no tiene numeración uniforme.** Va de §4.4 a §4.10 según el archivo de reglas. El master-prompt la citaba como «§4.5», numeración que solo coincidía en siete de los trece archivos. **Corregido en E4**: ahora se la ubica por título.
4. **`SOLUTION-INTAKE-template.md` no declaraba versión en cabecera** (corregido en E8, 1.3), solo en su tabla de control de cambios. Su campo `| Versión | 1.0 |` pertenece al documento que la plantilla genera, no a la plantilla. Es una aplicación incompleta de D6 a las plantillas.
5. **`Rules-Necesidades-Negocio.md` §7 contiene un enlace ilustrativo sin destino real**, a un archivo del repositorio destino generado. Es correcto en su contexto de ejemplo, pero un verificador automático de enlaces lo marca.
6. **`SDD-Getting-Started-Guide.md` conservaba el nombre de una solución concreta** (corregido en E8, DEC-07) en su §6 de ejemplo aplicado. Está fuera de la restricción de autosuficiencia, que prohíbe rutas a otros repositorios y no ejemplos nominados, pero roza D7.
7. **No existe la solicitud S8.** El prompt salta de S7 a S9. Las ocho etapas cubren S1–S7 y S9–S11 sin huecos.
8. **La nota de coherencia de referencia declara fin de línea CRLF.** En este checkout todos los archivos están en LF y no hay `.gitattributes`. La observación §5.1 de `Coherencia-Auditoria-Marco-v1.0.md` está desactualizada.

### 6.2 Interpretaciones

1. **La matriz de sensado de deriva no alcanzaba a los proyectos sin interfaz visual.** Se emitía solo desde la Fase B2, que corre con `requiere_maqueta` en `true`. Un `worker-service`, una `library` o un `cli-tool` quedaban sin instrumento de sensado, cubiertos únicamente por los dos mecanismos que `Deriva-Rules.md` §0 declara insuficientes por sí solos. La extensión con sondas `VER-XX` lo corrige como efecto lateral de S2, no como objetivo declarado.
2. **`Rules-Calidad-Y-Pruebas.md` contradecía a `Deriva-Rules.md`** (corregido en E8, DEC-06). La matriz vive en la categoría 08 y AG-08 la incorpora a la estrategia de testing, así que su archivo de reglas sería el lugar natural para nombrar la clase de sonda nueva. No se lo tocó por la restricción que limita los cambios en 00–09 a cuatro casos tasados. La mecánica funciona igual porque 08 delega el sensado por referencia.
3. **Los pisos de cantidad de samples por tipo D8 se expresan en cantidad, no en cobertura.** Un proyecto puede cumplir el piso y dejar casos de uso críticos sin ninguna sonda que los ejercite. Se cubrió con un criterio de aceptación que exige justificación, pero reexpresar §2.2 en cobertura excede lo que S2 pide.
4. **`AGENTS.md` abre un precedente sobre D3 y D4.** Es la primera excepción declarada a la convención de nombres del framework, admitida por razón funcional: un archivo que las herramientas no encuentran no cumple su función. Conviene que el precedente sea deliberado y no accidental.

---

## 7. Decisiones abiertas y su resolución

Las siete decisiones que este informe registró como abiertas en su versión 1.0 se resolvieron en la etapa **E8**, abierta a tal efecto. **No quedan decisiones pendientes.**

| # | Decisión | Resolución | Resultado |
| --- | --- | --- | --- |
| A-1 | Normalizar «D1-D8» a «D1-D9» | **Hecha** | 18 ocurrencias normativas en 7 archivos, con las enumeraciones completadas a nueve invariantes. Las 21 de notas de coherencia ya emitidas se conservan: verificaron contra el conjunto vigente entonces |
| A-2 | Mencionar `VER-XX` en `Rules-Calidad-Y-Pruebas.md` | **Hecha, y reencuadrada** | No era una mención faltante sino una **contradicción**: la categoría 08 condicionaba la matriz de sensado a la Fase B2, anulando en la práctica la extensión de E2. Corregido en §0, §2.1 y §6. Ver §6.3 |
| A-3 | Pisos de samples en cobertura | **Cerrada sin acción** | El criterio de aceptación de E2 ya exige que todo CU crítico tenga sonda o justificación. El piso de cantidad cubre la progresión didáctica, que es otra cosa |
| A-4 | Control de cambios en el `README.md` | **Cerrada sin acción** | El framework trata a los `README.md` como índices sin versión |
| A-5 | Reponer la tabla de tiempo a primer éxito | **Cerrada sin acción** | La tabla no se había perdido: vive en `Rules-UX-UI-DX.md`, dueña de las métricas DX. Reponerla en 11 duplicaría contenido entre categorías |
| A-6 | Despersonalizar el ejemplo aplicado | **Hecha** | 18 ocurrencias al placeholder `<Nombre-Solucion>`, con dominio y flujos de usuario genéricos |
| A-7 | Dos ejes más en la guía de desarrollo | **Hecha** | §III.8 regla transversal y §III.9 flag de gating. La guía pasa a 1.1 con nueve ejes |

### 7.1 Hallazgo de proceso

Tres de las siete decisiones se cerraron sin acción porque, al medirlas, el problema que enunciaban no existía o ya estaba cubierto. Una —A-2— estaba **mal enunciada**: describía una omisión cuando lo real era una contradicción que anulaba una corrección de E2.

La nota de coherencia de E2 había declarado cumplida su comprobación 5, «sin contradicción con etapas anteriores». Era cierto en su literalidad: E2 no contradecía a E1. Lo que no verificó fue que el archivo transversal que modificaba no contradijera a la categoría que lo opera. El anti-patrón «declarar la frontera de un solo lado», que la guía de desarrollo documenta y que DEC-05 ya había corregido en E3, cubre exactamente este caso y volvió a ocurrir.

Es la clase de defecto que un informe de cierre existe para exponer, y la razón por la que conviene que registre lo que queda abierto en lugar de darlo por cerrado.

## 8. Etapas ejecutadas

| Etapa | Solicitudes | Veredicto | Nota |
| --- | --- | --- | --- |
| E0 — Reconocimiento | ninguna | CONFORME | §4 de `Estado-Intervencion.md` |
| E1 — Renombrado estructural | S1 | CONFORME | `Nota-Coherencia-E1.md` |
| E2 — Categoría 10 | S2 | CONFORME | `Nota-Coherencia-E2.md` |
| E3 — Categoría 11 | S3, S5, S4 (DEC-03), S3.5 recíproco (DEC-05) | CONFORME | `Nota-Coherencia-E3.md` |
| E4 — Documentación viva | S4 (orquestador y DoD) | CONFORME | `Nota-Coherencia-E4.md` |
| E5 — Navegabilidad 00–09 | S6 | CONFORME | `Nota-Coherencia-E5.md` |
| E6 — Superficie de entrada | S9, S10 | CONFORME | `Nota-Coherencia-E6.md` |
| E7 — Cierre | S7, S11 | CONFORME | `Nota-Coherencia-E7.md` |
| E8 — Cierre de decisiones abiertas | A-1 a A-7 de este informe (DEC-06, DEC-07) | CONFORME | `Nota-Coherencia-E8.md` |

Las siete decisiones tomadas durante la corrida (DEC-01 a DEC-07) están registradas con su motivo y su consecuencia operativa en §2 de `Estado-Intervencion.md`.

---

## 9. Estado de entrega

Los cambios quedan en el **working tree**, sin commitear ni pushear, según la restricción del prompt. El repositorio no tiene commits nuevos.

`SDD/Guides/SDD-Development-Guide.md` figura como untracked: existía como archivo vacío nunca commiteado, y esta intervención le dio contenido.

---

## 10. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.1 | 2026-07-26 | Cierre de las siete decisiones abiertas en la etapa E8: cuatro resueltas con acción y tres cerradas sin acción. Se reencuadra A-2, que estaba mal enunciada: no era una mención faltante sino una contradicción entre la categoría 08 y la regla de sensado que anulaba en la práctica la extensión de E2. Se agrega §7.1 con el hallazgo de proceso. Se marcan como corregidos cuatro de los hechos e interpretaciones de §6. | Reformulación SDD |
| 1.0 | 2026-07-26 | Informe inicial de la intervención: inventario de veinticinco archivos con su cambio de versión, resultado de los tres barridos de verificación con sus omisiones deliberadas justificadas, doce inconsistencias detectadas separadas en hechos e interpretaciones, siete decisiones abiertas con recomendación, y estado de entrega. | Reformulación SDD |
