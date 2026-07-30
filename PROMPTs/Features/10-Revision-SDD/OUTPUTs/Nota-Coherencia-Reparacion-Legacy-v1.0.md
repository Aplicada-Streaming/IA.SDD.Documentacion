# Nota de coherencia — Reparación de la política de deprecación y del archivado en `_legacy/`

**Proyecto:** Framework SDD
**Documento:** `Nota-Coherencia-Reparacion-Legacy-v1.0.md`
**Versión:** 1.0
**Estado:** Vigente
**Fecha:** 2026-07-28
**Autor:** Revisión SDD (Claude Code)
**Repositorio intervenido:** `/IA/IA.SDD/`
**Patrón seguido:** `SDD/Devs/Guides/Coherencia-Auditoria-Marco-v1.0.md`, según exige `README.md` línea 123 para toda intervención sobre varios archivos

---

## 1. Alcance

Verificación de implantación de las ocho reparaciones R-1 a R-8 de `Revision-Hallazgos-SDD-v1.0.md` §6, que resuelven nueve hallazgos sobre la política de deprecación y el archivado: cinco reportados por `INPUTs/Evaluacion-SDD.md` a partir de una corrida real del orquestador sobre una solución de cuatro proyectos, y cuatro detectados al contrastar esos hallazgos contra el framework.

La intervención **conserva `_legacy/` y lo repara**. La alternativa de eliminar el concepto se evaluó en `Alternativa-Eliminar-Legacy-v1.0.md` y el responsable del framework la descartó en favor de esta vía.

Fuera de alcance: la observación O-1 (conteo de reglas en una fila histórica de changelog) y la O-3 (README de proyecto sin regla que lo gobierne). Ambas quedan registradas en el documento padre con su motivo.

---

## 2. Inventario de archivos

### 2.1 Modificados con cambio normativo

| Archivo | Versión | Cambio |
| --- | --- | --- |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 3.6 → **3.7** | §3.5 declara `SDD/Docs/Audit/` y explica dónde aparece `_legacy/`. §5 reescribe las celdas de política de versionado y de deprecación. **§5.1 es nueva**: detalle operativo con la ruta única, el sufijo de los artefactos sin versión, la tabla de cinco exenciones y la prohibición de renombrar lo ya archivado. §7.2 suma la fila de versionado por corte y dos notas. §8 suma la sección «Estado previo del entregable» al esqueleto y cuatro reglas de construcción. §10 suma el eje de ronda al path del informe, en la política y en el despacho del auditor |
| `SDD/Devs/Rules/Root-Rules.md` | 1.4 → **1.5** | §3.1 declara que el README raíz recibe el sufijo de versión al archivarse, y que `CHANGELOG.md` queda exento |
| `SDD/Devs/Rules/Rules-Contexto.md` | 1.5 → **1.6** | §3.4 declara el sufijo al archivarse del README de la sección 00 |
| `SDD/Devs/Rules/Rules-Necesidades-Negocio.md` | 1.4 → **1.5** | §3.4 declara el sufijo al archivarse del README de la sección 01 |
| `SDD/Devs/Rules/Rules-Examples.md` | 2.0 → **2.1** | §3.1 declara el sufijo al archivarse del README índice |
| `SDD/Devs/Rules/Rules-Documentacion.md` | 2.0 → **2.1** | §3.1 declara el sufijo de los índices y la exención de `AGENTS.md` del archivado |

### 2.2 Modificados sin cambio normativo

| Archivo | Cambio |
| --- | --- |
| `CHANGELOG.md` | Entrada `[3.2] - 2026-07-28` con las secciones Cambiado, Añadido y Corregido |
| `SDD/Guides/SDD-Development-Guide.md` | Errata: §2 afirmaba que `SDD/Devs/Intake/_legacy/` conserva las plantillas del modelo anterior, carpeta eliminada en la entrada [3.1]. Sin bump, según `README.md` línea 118 |

### 2.3 No modificados, deliberadamente

| Archivo | Motivo |
| --- | --- |
| `README.md` | D5 queda intacto. La intervención precisa la política operativa de §5, no el enunciado del principio. Es la decisión DEC-03 |
| Las diez reglas que escriben `_legacy/` a secas | §5.1 declara que se leen como abreviatura de la ruta canónica, con lo cual dejan de contradecirla sin requerir once ediciones |
| `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md`, `SDD-Getting-Started-Guide.md` | Su `SDD/Docs/_legacy/<fecha>/` es el caso del prerrequisito 4, distinto del archivado de una versión superada. §5.1 lo declara explícitamente para que no se lea como una convención rival |

---

## 3. Verificación de invariantes

| Invariante | Resultado | Evidencia |
| --- | --- | --- |
| D1 idioma y registro | Cumple | Español rioplatense neutro técnico en todo el texto agregado. Sin marketing, sin emojis, sin negritas decorativas fuera del énfasis normativo |
| D2 encoding | Cumple | Los ocho archivos siguen en UTF-8 y con LF, verificado con `file` tras la intervención |
| D3 nombres | No aplica | La intervención no crea ni renombra archivos en `IA.SDD` |
| D4 sufijo de versión | Cumple | Todas las rutas de ejemplo agregadas usan `-v<X.Y>.md` con guion medio |
| **D5 una sola versión vigente** | **Cumple y se precisa** | El principio no cambia. Lo que se agrega es la ruta única, el sufijo del snapshot para artefactos sin versión, los requisitos de estado y nota, y la tabla de exenciones. Ninguna documentación ya emitida deja de cumplir |
| D6 trazabilidad | Cumple | Las referencias cruzadas agregadas resuelven: los cinco punteros de las reglas apuntan a `Master-Prompt.md` §5.1, que existe; §5.1 cita `Maqueta-Rules.md` §2.3 y `Rules-Arquitectura-Tecnica.md` §3.6, ambas verificadas |
| D7 neutralidad de dominio | Cumple | No se introdujo vocabulario, ejemplo ni producto de ningún dominio de cliente |
| D8 tipos de proyecto | No aplica | La intervención no toca el conjunto cerrado |
| D9 evidencia verificable | Cumple, y se refuerza | La prohibición de renombrar retroactivamente lo ya archivado se funda explícitamente en D9: etiquetar con una versión un archivo cuyo contenido no se verificó es una afirmación sin evidencia |

**Ninguna invariante fue modificada.** Por lo tanto no se dispara el procedimiento de `README.md` línea 122, que exige decisión explícita del responsable para alcanzar a los dieciséis archivos de reglas y a toda la documentación emitida.

---

## 4. Verificación de implantación

Las diez verificaciones de `Revision-Hallazgos-SDD-v1.0.md` §9 se ejecutaron contra el framework intervenido. **Las diez pasan.** El detalle con su evidencia vive en esa sección; acá se resume lo que cada hallazgo dejó de poder ocurrir.

| Hallazgo | Qué ya no puede pasar |
| --- | --- |
| H-1 | Que el segundo archivado del día de un `README.md` sobrescriba al primero: el snapshot lleva sufijo de versión |
| H-2 | Que dos actores archiven el mismo artefacto en rutas distintas: hay una sola convención y las abreviaturas están declaradas como tales |
| H-3 | Que un subagente convocado para corregir edite en el lugar sin que nadie haya archivado: el orquestador toma el snapshot antes de despachar y el esqueleto se lo declara |
| H-4 | Que un archivo de `_legacy/` no diga que está superado ni dónde está el vigente |
| H-5 | Que la política no responda si una corrección post-audit sube versión |
| H-6 | Que dos proyectos que archivan la misma categoría el mismo día colisionen entre sí |
| H-7 | Que el re-audit obligatorio tras un veredicto RECHAZADO sobrescriba el informe que documenta los hallazgos corregidos |
| H-8 | Que el tramo de documentación viva quede sin regla de versionado y con una excepción a D5 no declarada |
| H-9 | Que la política dependa de una carpeta que ninguna fuente de estructura declara |

### 4.1 Verificación estructural

Se corrió un chequeo de integridad markdown sobre los ocho archivos, buscando filas de tabla separadas de su tabla por una línea en blanco. Detectó tres casos y todos se corrigieron antes de cerrar:

| Caso | Origen |
| --- | --- |
| La fila D9 fuera de la tabla de invariantes de `Master-Prompt.md` §5 | Preexistente |
| La fila 3.4 fuera de la tabla de control de cambios de `Master-Prompt.md` §16 | Preexistente, era la observación O-2 |
| Cinco filas de control de cambios, una por cada regla intervenida | Introducido por esta misma intervención, al insertarlas |

El chequeo final no devuelve ninguna fila huérfana en los archivos tocados.

---

## 5. Observaciones

**Sobre el mecanismo, que sigue siendo manual.** Las reparaciones vuelven correcta la política de archivado, pero no la vuelven verificable automáticamente. Cuatro de los nueve hallazgos —H-1, H-3, H-6 y H-7— eran formas distintas del mismo fallo silencioso: un archivo se sobrescribe, nadie recibe error y el directorio se ve correcto. La intervención cierra las cuatro formas conocidas; no impide que aparezca una quinta, porque el mecanismo sigue dependiendo de que un actor ejecute bien una operación manual en cada corrida. Esa limitación es inherente a la Opción A y está analizada en `Alternativa-Eliminar-Legacy-v1.0.md`.

**Sobre DEC-01.** El versionado por corte de cadencia en las Fases I y J se aplicó siguiendo la recomendación del documento padre, no una elección explícita del responsable, porque sin él las Fases I y J quedaban sin regla tras exceptuarlas del snapshot previo. Revertirlo o cambiarlo por alguna de las dos alternativas evaluadas es la edición de una fila de tabla en §7.2.

**Sobre el alcance de la verificación.** Es documental: se leyó el framework resultante y se contrastó cada verificación contra su texto. No se ejecutó una corrida del orquestador. Eso alcanza para confirmar que la especificación quedó completa y consistente, y no alcanza para confirmar que un orquestador real la aplique bien. La primera corrida posterior a esta intervención es la que lo va a decir.

---

## 6. Veredicto

**CONFORME.** Las ocho reparaciones están implantadas, las diez verificaciones pasan, ninguna invariante fue modificada, ninguna documentación ya emitida deja de cumplir y no quedaron referencias rotas ni tablas mal formadas en los archivos tocados.

Quedan abiertas dos observaciones registradas con su motivo (O-1 y O-3) y una decisión de fondo disponible para el futuro (DEC-04, el control de versiones como prerrequisito duro).

---

## Control de cambios

| Versión | Fecha | Cambios | Autor |
|---|---|---|---|
| 1.0 | 2026-07-28 | Nota de coherencia de la intervención que repara la política de deprecación y el archivado en `_legacy/`. Inventario de seis archivos con cambio normativo, dos sin él y tres no modificados deliberadamente. Verificación de las nueve invariantes, con D5 precisada sin modificar su enunciado. Verificación de implantación de los nueve hallazgos y chequeo estructural que detectó y corrigió tres casos de tablas mal formadas. Veredicto CONFORME. | Revisión SDD (Claude Code) |
