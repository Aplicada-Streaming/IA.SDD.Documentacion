# Estado de la intervención — Reordenamiento de categorías 10 ↔ 11 y cuerpo documental de entrega

**Prompt de origen:** `Editar-Agent-Rules-Documentacion-Examples-final.md`
**Repositorio intervenido:** `/IA/IA.SDD/`
**Fecha de apertura:** 2026-07-26
**Estado global:** **CERRADA** — nueve etapas ejecutadas con veredicto CONFORME el 2026-07-26, sin decisiones abiertas. Cambios en el working tree, sin commitear.

Este archivo fue el mecanismo de reanudación entre sesiones. La intervención está cerrada: no quedan etapas pendientes. El resultado completo, con los tres reportes de verificación, las inconsistencias detectadas y las decisiones abiertas, está en `Informe-Intervencion-v1.0.md`.

---

## 1. Tablero de etapas

| Etapa | Solicitudes | Estado | Fecha | Archivos tocados | Nota de coherencia |
| --- | --- | --- | --- | --- | --- |
| E0 — Reconocimiento | ninguna | Conforme | 2026-07-26 | ninguno (etapa de solo lectura) | §4 de este archivo |
| E1 — Renombrado estructural | S1 | Conforme | 2026-07-26 | 1 renombrado + 17 modificados (detalle en §2 de la nota) | `Nota-Coherencia-E1.md` |
| E2 — Categoría 10 | S2 | Conforme | 2026-07-26 | `Rules-Examples.md` (2.0), `Deriva-Rules.md` (1.1) | `Nota-Coherencia-E2.md` |
| E3 — Categoría 11 | S3, S5, S4 (por DEC-03), S3.5 recíproco (por DEC-05) | Conforme | 2026-07-26 | `Rules-Documentacion.md` (2.0), `Rules-Arquitectura-Tecnica.md` (1.3), `Rules-Calidad-Y-Pruebas.md` (1.5), `Rules-Devops.md` (1.5) | `Nota-Coherencia-E3.md` |
| E4 — Documentación viva | S4 (orquestador y DoD) | Conforme | 2026-07-26 | `Master-Prompt.md` (3.6), `Rules-Plan-Sprint.md` (1.3) | `Nota-Coherencia-E4.md` |
| E5 — Navegabilidad 00–09 | S6 | Conforme | 2026-07-26 | Los diez archivos de reglas de 00 a 09, cada uno con su bump de versión | `Nota-Coherencia-E5.md` |
| E6 — Superficie de entrada | S9, S10 | Conforme | 2026-07-26 | `README.md` (7 → 132 líneas), `SDD-Development-Guide.md` (0 bytes → 1.0, 504 líneas) | `Nota-Coherencia-E6.md` |
| E7 — Cierre | S7, S11 | Conforme | 2026-07-26 | `SDD-User-Guide.md` (1.5), `CHANGELOG.md` (3.0), `Informe-Intervencion-v1.0.md` (nuevo) | `Nota-Coherencia-E7.md` |
| E8 — Cierre de decisiones abiertas | A-1 a A-7 del informe (DEC-06, DEC-07) | Conforme | 2026-07-26 | 12 archivos; el principal es `Rules-Calidad-Y-Pruebas.md` (1.7) | `Nota-Coherencia-E8.md` |

No existe una solicitud S8 en el prompt de origen: la numeración salta de S7 a S9. Las ocho etapas cubren S1–S7 y S9–S11 sin huecos.

---

## 2. Decisiones tomadas por el responsable del framework

Resuelven las ambigüedades que E0 detectó entre el prompt y el estado real del repositorio. Rigen para todas las etapas.

| # | Cuestión | Decisión | Consecuencia operativa |
| --- | --- | --- | --- |
| DEC-01 | Material histórico (`SDD/Devs/Bootstrap/`, `SDD/Devs/Reformulacion/`, `SDD/Devs/Intake/_legacy/`) frente al barrido de propagación de S1.5 | Congelar y declarar | No se corrigen. Describen el estado anterior y corregirlos falsearía el registro auditado. Se listan en el informe de cierre como omisiones deliberadas con su motivo |
| DEC-02 | Las 12 ocurrencias preexistentes de `PROMPTs/`, que contradicen el criterio de cierre de autosuficiencia | Limpiar también `SDD-Getting-Started-Guide.md` | Se despersonalizan las rutas de repositorio destino concreto y se eliminan las menciones a `PROMPTs/` en los tres archivos vivos que las contienen. Amplía el alcance de E1 sobre la guía de arranque |
| DEC-03 | Titularidad de las secciones de S4 dentro de `Rules-Documentacion.md`, que E3 escribe completo | Todo en E3 | E3 escribe `Rules-Documentacion.md` íntegro, incluidos los tres momentos, la cadencia, el ensayo de entrega y la bitácora. E4 queda como dueña exclusiva de `Master-Prompt.md` y de la línea de Definition of Done en `Rules-Plan-Sprint.md` |
| DEC-04 | `implementador` como término a normalizar por S1.7 | No sustituir en el intake | Se conserva donde designa la categoría RACI de stakeholder (propietario / implementador / beneficiario). Se sustituye solo donde designe realmente al rol que monta y opera el servicio. Excepción documentada en el informe |
| DEC-06 | La categoría 08 contradecía a `Deriva-Rules.md` sobre cuándo se emite la matriz de sensado | Corregir la contradicción | Se toca `Rules-Calidad-Y-Pruebas.md` fuera de los cuatro casos que la restricción admite sobre 00-09. Sin la corrección, AG-08 seguiría omitiendo la matriz en proyectos sin interfaz visual, que es lo que S2 vino a corregir |
| DEC-07 | El ejemplo aplicado de la guía de arranque nombraba una solución concreta en 18 lugares | Despersonalizar con placeholder | Pasan a `<Nombre-Solucion>`, con el dominio y los flujos enunciados en términos genéricos. La fila 1.0 del control de cambios conserva el nombre por ser registro histórico |
| DEC-05 | Cuándo escribir las notas recíprocas de frontera con la categoría 11 en `Rules-Arquitectura-Tecnica.md`, `Rules-Calidad-Y-Pruebas.md` y `Rules-Devops.md` | Escribirlas en E3, no en E5 | El orquestador despacha a cada subagente con un solo archivo de reglas, de modo que una frontera declarada de un solo lado no frena al agente que puede cruzarla. E3 escribe la frontera en §0 de los tres archivos; E5 los vuelve a abrir para la tabla de contenido en §4 y §6, y no reescribe la frontera |

---

## 3. Titularidad de archivos por etapa

La regla «ningún archivo es escrito por dos etapas» resultó inalcanzable con las asignaciones del prompt de origen (ver hallazgo A-1 en §4.3). Se adopta en su lugar **titularidad por archivo y sección**: cada sección tiene una única etapa dueña, y las etapas que no son dueñas solo aplican sustitución de cadenas sin alterar semántica.

| Archivo | Etapa dueña del contenido | Otras etapas y su alcance acotado |
| --- | --- | --- |
| `SDD/Devs/Rules/Rules-Documentacion.md` | E3 (cuerpo completo) | E1: renombre físico desde `Rules-Developer-Guide.md` y cabecera (carpeta target, subagente) |
| `SDD/Devs/Rules/Rules-Examples.md` | E2 (doble arista, contrato de verificación) | E1: cabecera y dependencia declarada |
| `SDD/Devs/Rules/Deriva-Rules.md` | E2 (sondas `VER-XX`) | — |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | E4 (Fases I y J, precondiciones, auditoría) | E1: renumeración de categorías y AG-XX |
| `SDD/Devs/Rules/Rules-Plan-Sprint.md` | E4 (Definition of Done) | E5: tabla de contenido |
| `SDD/Devs/Rules/Root-Rules.md` | E1 (layout canónico y numeración) | — |
| Las diez reglas de 00 a 09 | E5 (tabla de contenido, §4 y §6) | E1: sustitución de cadenas de vocabulario y AG-XX. E3: frontera recíproca con 11 en §0 de 05, 08 y 09 (DEC-05) |
| `SDD/Guides/SDD-Getting-Started-Guide.md` | E1 (renumeración y limpieza de autosuficiencia, DEC-02) | — |
| `SDD/Guides/SDD-User-Guide.md` | E7 (S7) | E1: renumeración de categorías |
| `SDD/Devs/Guides/Marco-Teorico-SDD-v1.0.md` | E1 (solo cadenas desactualizadas por el intercambio) | Prohibido reescribir, resumir o duplicar |
| `README.md` raíz | E6 (S9) | — |
| `SDD/Guides/SDD-Development-Guide.md` | E6 (S10) | — |
| `CHANGELOG.md` raíz | E7 | Entrada única de la intervención completa |
| `SDD/Devs/Rules/Intake-Rules.md`, `Maqueta-Rules.md`, `SOLUTION-INTAKE-template.md` | E1 (verificación de referencias cruzadas) | — |
| `SDD/Devs/Bootstrap/`, `SDD/Devs/Reformulacion/`, `SDD/Devs/Intake/_legacy/` | **Ninguna — congelados** (DEC-01) | — |

---

## 4. Nota de coherencia E0 — Reconocimiento

**Alcance.** Verificación en lectura del estado real de `/IA/IA.SDD/` contra los supuestos declarados en el Contexto del prompt de origen, línea de base cuantitativa de los dos barridos que exige S1, línea de base de la restricción de autosuficiencia, y Control de coherencia A sobre el diseño de las ocho etapas. Etapa de solo lectura: no se modificó ningún archivo.

### 4.1 Inventario verificado

`/IA/IA.SDD/` contiene 62 archivos versionados. Estructura de primer nivel: `SDD/Devs/` (reglas, orquestador, intake, guías internas, referencias de diseño, modelos UX-UI, bootstrap, reformulación), `SDD/Guides/` (tres guías de cara al usuario), `PROMPTS/`, `Templates/`, `README.md`, `CHANGELOG.md`. Los archivos están en UTF-8 con fin de línea LF y el repositorio no tiene `.gitattributes`.

| Supuesto del Contexto | Verificación |
| --- | --- |
| Dieciséis archivos de reglas en `SDD/Devs/Rules/` | Cumple — exactamente 16 |
| `Rules-Developer-Guide.md` gobierna la categoría 10, subagente AG-10, se genera en Fase F | Cumple — 33.135 B, estructura canónica §0–§9 completa |
| `Rules-Examples.md` gobierna la categoría 11, subagente AG-11, se genera en Fase G, versión 1.2 | Cumple — 31.604 B; su §0 declara upstream de 02, 05 y 10 |
| Dependencia vigente «10 explica y referencia, 11 demuestra con código completo» | Cumple — `Rules-Developer-Guide.md` §1 y `Rules-Examples.md` §0 |
| `Marco-Teorico-SDD-v1.0.md` de ~151 KB con catorce secciones | Cumple — 151.315 B |
| `SDD-User-Guide.md` de ~115 KB | Cumple — 115.685 B |
| `SDD-Getting-Started-Guide.md` de ~31 KB | Cumple — 31.265 B |
| `Coherencia-Auditoria-Marco-v1.0.md` disponible como patrón de nota | Cumple — siete secciones: alcance, inventario, invariantes, trazabilidad, observaciones, veredicto, control de cambios |
| `SDD-Development-Guide.md` vacío | Cumple — 0 bytes, y además nunca fue commiteado (untracked) |
| `README.md` raíz de siete líneas, título más tres enlaces | Cumple — un único enlace externo, preexistente |
| Orden de fases A → H con Fase F = categoría 10 y Fase G = categoría 11 | Cumple — `Master-Prompt.md` §6 y §7 |
| El caso degenerado produce layout aplanado | Cumple — `Master-Prompt.md` §3.5 |
| Working tree limpio | Cumple — el único elemento pendiente es el archivo vacío untracked |

### 4.2 Líneas de base de los barridos

Barrido de propagación de S1.5, medido en ocurrencias sobre archivos `.md` del árbol completo:

| Cadena | Ocurrencias | Cadena | Ocurrencias |
| --- | --- | --- | --- |
| `10-Developer-Guide` | 25 | `categoría 11` | 9 |
| `11-Examples` | 38 | `categoria 10` / `categoria 11` (sin tilde) | 0 / 0 |
| `Rules-Developer-Guide` | 13 | `AG-10` | 25 |
| `Developer-Guide` (total) | 38 | `AG-11` | 33 |
| `categoría 10` | 14 | `Fase F` / `Fase G` | 5 / 4 |

Dieciocho archivos contienen al menos una de las cuatro cadenas de corrección obligatoria; de ellos, siete pertenecen al material congelado por DEC-01.

Normalización de vocabulario de S1.7: `consumidor` 109 más `consumidores` 42; `audiencia` 57 más `audiencias` 7; `implementador` 12 más `implementadores` 1; `constructor` 1. Veinticinco archivos contienen al menos uno de los cuatro términos.

Restricción de autosuficiencia, línea de base: diez de las once cadenas prohibidas dan cero ocurrencias. `PROMPTs/` da 12, distribuidas en `SDD-Getting-Started-Guide.md` (10), `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md` (1) y `Marco-Teorico-SDD-v1.0.md` (1). El árbol contiene 26 URLs distintas preexistentes.

### 4.3 Control de coherencia A — diseño de las ocho etapas

| Comprobación exigida | Resultado |
| --- | --- |
| Ningún archivo es escrito por dos etapas distintas | **No cumple.** Siete archivos quedan con dos o tres etapas dueñas bajo las asignaciones del prompt: `Rules-Documentacion.md` (E1, E3, E4), `Rules-Examples.md` (E1, E2), `Rules-Plan-Sprint.md` (E4, E5), las reglas de 05, 08 y 09 (E3, E5), `Master-Prompt.md` (E1, E4) y `SDD-User-Guide.md` (E1, E7). Resuelto por DEC-03 y por la titularidad por sección de §3 |
| Ninguna etapa referencia un artefacto que solo existirá después | Cumple. E2 define `VER-XX` y E3 lo referencia; E4 se apoya en artefactos de E2 y E3; E6 describe el framework ya modificado. Sin ciclos |
| Cada etapa tiene criterio de terminación observable | **No cumple en origen.** El prompt describe entregables, no condiciones verificables. Subsanado en §5 |
| El corte sigue siendo correcto a la luz del inventario | Cumple. El inventario no contradice la segmentación de fondo |

### 4.4 Observaciones

1. **La restricción de autosuficiencia no se cumplía antes de esta intervención** (hallazgo preexistente). El prompt exige cero ocurrencias de once cadenas y ya había 12 de `PROMPTs/`. Además, `SDD-Getting-Started-Guide.md` cita en cinco lugares rutas de un repositorio destino concreto y nominado. Resuelto por DEC-02, que amplía el alcance de E1 sobre esa guía.
2. **Premisa fáctica incorrecta en las Restricciones del prompt** (informativa, sin impacto en las acciones). El prompt afirma que SemVer, Conventional Commits, CycloneDX y SLSA «aparecen nombrados y nunca enlazados». Están enlazados, junto con otras 22 URLs. La regla de nombrar sin enlazar sigue siendo aplicable a todo contenido nuevo; su justificación es la que no se sostiene. Las URLs preexistentes se conservan.
3. **Existe una invariante D9.** `Deriva-Rules.md` §1 define D9, evidencia verificable, incorporada al framework en una intervención anterior y registrada en el `CHANGELOG.md`. La lista de comprobación del Control de coherencia B habla solo de D1–D8. Se adopta el criterio de verificar **D1–D9** en todas las notas de etapa.
4. **`implementador` designa mayoritariamente una categoría de stakeholder**, no un rol de intervención documental: aparece como parte de la terna RACI propietario / implementador / beneficiario en `Rules-Contexto.md`, `Rules-Necesidades-Negocio.md` y `SOLUTION-INTAKE-template.md`. La tabla de excepciones de S1.7 no contemplaba este caso. Resuelto por DEC-04.
5. **`constructor` tiene una sola ocurrencia**, en `Rules-Developer-Guide.md` §1, archivo que E3 reescribe por completo. La normalización de ese término no requiere trabajo propio.
6. **La nota de coherencia de referencia declara fin de línea CRLF** (observación §5.1 de `Coherencia-Auditoria-Marco-v1.0.md`). En este checkout todos los archivos están en LF. Se escribe en LF y se preserva el estilo del archivo intervenido en cada caso.
7. **El material congelado por DEC-01 concentra parte sustancial del barrido.** Siete de los dieciocho archivos con cadenas a corregir son auditorías históricas. El informe de cierre debe distinguir explícitamente entre ocurrencias en archivos vivos y en archivos congelados, para que el conteo no se lea como cobertura incompleta.

### 4.5 Veredicto

**CONFORME.** El estado real del repositorio respalda los supuestos del Contexto sin desvíos estructurales. Las siete observaciones son de dos clases: hallazgos preexistentes al inicio de la intervención (1, 2, 3, 6) y precisiones de alcance que las cuatro decisiones DEC-01 a DEC-04 resuelven (4, 5, 7). El Control de coherencia A arrojó dos incumplimientos de diseño, ambos subsanados antes de escribir la primera línea.

---

## 5. Criterios de terminación observables por etapa

Reemplazan la descripción de entregables del prompt de origen por condiciones verificables.

| Etapa | Criterio de terminación |
| --- | --- |
| E1 | `Rules-Developer-Guide.md` no existe y `Rules-Documentacion.md` sí. Sobre archivos vivos: cero ocurrencias de `10-Developer-Guide`, `11-Examples` y `Rules-Developer-Guide`; cero de `PROMPTs/`. Toda ocurrencia restante está en el material congelado por DEC-01 y figura en el reporte de la etapa |
| E2 | `Rules-Examples.md` declara las dos aristas, la sección `Contrato de verificación` con sus cinco campos, las dos pasadas de generación y el prefijo `VER-XX`; `Deriva-Rules.md` §2 y §4 admiten sondas `VER-XX` |
| E3 | `Rules-Documentacion.md` tiene las nueve secciones canónicas, las tres tablas de artefactos (solución, integrador, mantenedor, operador), la tabla de gating por cuerpo para los ocho tipos D8, las cinco fronteras, los cinco prefijos de identificador, los tres momentos con sus cuatro mecanismos, y las cuatro secciones de reglas de redacción transcriptas íntegras |
| E4 | `Master-Prompt.md` declara las Fases I y J con su precondición dura, su criterio de re-ejecución y sus hallazgos P0; `Rules-Plan-Sprint.md` incorpora la condición de Definition of Done |
| E5 | Los diez archivos de reglas de 00 a 09 exigen tabla de contenido en §4 y en §6. La frontera recíproca de 05, 08 y 09 ya quedó escrita en E3 por DEC-05 y no se retoca |
| E6 | `README.md` responde correctamente a cinco intenciones distintas de su matriz de ruteo leído en aislamiento; `SDD-Development-Guide.md` tiene sus seis partes y deja de ser un archivo vacío |
| E7 | `SDD-User-Guide.md` refleja las Fases I y J y la numeración nueva; existe `Informe-Intervencion-v1.0.md` con los tres reportes de verificación; `CHANGELOG.md` tiene su entrada |

---

## 6. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.9 | 2026-07-26 | Apertura y cierre de E8, etapa no prevista en la segmentación original, para resolver las siete decisiones que el informe dejó abiertas. Cuatro se resolvieron con acción (A-1 nomenclatura de invariantes, A-2 contradicción de la categoría 08, A-6 despersonalización del ejemplo aplicado, A-7 dos ejes de extensión) y tres se cerraron sin acción tras verificar que el problema enunciado no existía o ya estaba cubierto (A-3, A-4, A-5). Se registran DEC-06 y DEC-07. Se corrigen además dos referencias muertas y el versionado de la plantilla de intake. No quedan decisiones abiertas. | Reformulación SDD |
| 1.8 | 2026-07-26 | Cierre de E7 y de la intervención completa, con veredicto CONFORME en las ocho etapas. `SDD-User-Guide.md` sube a 1.5 con la renumeración de fases, el paso 7 del usuario en §4.8, la reformulación de §4.7, seis entradas de FAQ (F-24 a F-29) y el mapa de carpetas actualizado. `CHANGELOG.md` sube a 3.0 con la entrada de la intervención y su impacto sobre documentación ya emitida. Se emite `Informe-Intervencion-v1.0.md` con el inventario de veinticinco archivos, los tres reportes de verificación medidos, doce inconsistencias separadas en hechos e interpretaciones y siete decisiones abiertas con recomendación. | Reformulación SDD |
| 1.7 | 2026-07-26 | Cierre de E6 con veredicto CONFORME. `README.md` reescrito como superficie de entrada: qué es SDD, modelo de tres repositorios, anatomía del repositorio, matriz de ruteo con dieciséis intenciones, mapa de las doce categorías, invariantes D1 a D9 enunciadas y reglas de intervención, sin agregar URLs externas. `SDD-Development-Guide.md` escrito desde cero en versión 1.0, con anatomía, los seis contratos internos, siete ejes de extensión con ejemplo trabajado, criterios, once anti-patrones y procedimiento de cambio, referenciando el marco teórico sin duplicarlo. Dos decisiones abiertas: si el README debe llevar control de cambios propio, y si conviene un barrido que normalice «D1-D8» a «D1-D9» en todo el árbol. | Reformulación SDD |
| 1.6 | 2026-07-26 | Cierre de E5 con veredicto CONFORME. Los diez archivos de reglas de las categorías 00 a 09 exigen tabla de contenido en los documentos que generan, declarada en §4.1 y en §6, con umbral de tres secciones de primer nivel, anclas de primer y segundo nivel y excepción para documentos breves. Se respetó el alcance negativo de S6: sin artefactos nuevos, sin cambios de estructura obligatoria y sin tocar gating. La frontera con la categoría 11 escrita en E3 sobre 05, 08 y 09 quedó intacta, verificada una por una. | Reformulación SDD |
| 1.5 | 2026-07-26 | Cierre de E4 con veredicto CONFORME. `Master-Prompt.md` sube a 3.6 con las Fases I y J, la precondición dura de la Fase I, el criterio de re-ejecución que preserva las correcciones manuales del usuario, los diez hallazgos P0 de las fases nuevas, el plan documental incorporado a la Fase H, el reordenamiento de F y G, y el handoff redefinido como cierre del tramo de especificación. `Rules-Plan-Sprint.md` sube a 1.3 con la actualización de la categoría 11 incorporada a la Definition of Done, en §4.5 nueva y con renumeración de tres subsecciones. Se corrige además la referencia preexistente incorrecta a la sección de anti-patrones, que el orquestador citaba como «§4.5» y que solo coincidía en siete de los trece archivos de reglas: ahora se la ubica por título. Con eso queda resuelta la decisión abierta que había dejado E3. | Reformulación SDD |
| 1.4 | 2026-07-26 | DEC-05: las notas recíprocas de frontera con la categoría 11 se incorporan a E3 en lugar de E5, porque el orquestador despacha a cada subagente con un solo archivo de reglas y una frontera de un solo lado no frena a quien puede cruzarla. `Rules-Arquitectura-Tecnica.md` sube a 1.3, `Rules-Calidad-Y-Pruebas.md` y `Rules-Devops.md` a 1.5. Se actualizan la titularidad por archivo y el criterio de terminación de E5. | Reformulación SDD |
| 1.3 | 2026-07-26 | Cierre de E3 con veredicto CONFORME. `Rules-Documentacion.md` reescrito por completo y elevado a 2.0: dos ejes con prohibición de bifurcar por tipo de lector, tres cuerpos por rol de intervención con el cuerpo mantenedor obligatorio para los ocho tipos D8, cinco fronteras, cinco identificadores estables, modelo de documentación viva en tres momentos con cadencia, ensayo de entrega y bitácora de eventualidades, y las cuatro secciones de reglas de redacción transcriptas íntegras. Dos decisiones de diseño registradas: S4 ubicado en §0 para respetar la estructura canónica, y anti-patrones en §4.9. Dos decisiones abiertas elevadas al responsable: alcance de §4.5 como anti-patrones en todos los archivos de reglas, y reposición de la tabla consolidada de tiempo a primer éxito. | Reformulación SDD |
| 1.2 | 2026-07-26 | Cierre de E2 con veredicto CONFORME. `Rules-Examples.md` sube a 2.0 con las dos aristas del sample, el contrato de verificación `VER-XX` con sus cinco campos y su regla de aserción evaluable, las dos pasadas de generación, y diez secciones obligatorias por markdown explicativo. `Deriva-Rules.md` sube a 1.1 extendiendo el sensado a contratos y comportamiento, con la consecuencia de que los proyectos sin interfaz visual dejan de quedar sin instrumento de sensado. Dos decisiones abiertas elevadas al responsable: mención de `VER-XX` en `Rules-Calidad-Y-Pruebas.md` y expresión de los pisos de samples en cobertura de casos de uso. | Reformulación SDD |
| 1.1 | 2026-07-26 | Cierre de E1 con veredicto CONFORME. Renombre de `Rules-Developer-Guide.md` a `Rules-Documentacion.md` por `git mv`, intercambio 10 ↔ 11 propagado a diecisiete archivos vivos, carpetas target `10-Examples/` y `11-Documentacion/`, dependencia invertida en ambas direcciones, subagentes reasignados a AG-10 Developer Advocate y AG-11 Technical Writer / Documentation Lead, vocabulario de actores normalizado con sus excepciones declaradas, y las once cadenas de la restricción de autosuficiencia llevadas a cero por primera vez. Se adopta la verificación de invariantes D1–**D9** en todas las notas de etapa. | Reformulación SDD |
| 1.0 | 2026-07-26 | Apertura del tablero. Cierre de E0 con veredicto CONFORME: inventario verificado, líneas de base de los dos barridos y de la restricción de autosuficiencia, Control de coherencia A con dos incumplimientos subsanados, siete observaciones y cuatro decisiones tomadas. Titularidad por archivo y sección, y criterios de terminación observables para las siete etapas restantes. | Reformulación SDD |
