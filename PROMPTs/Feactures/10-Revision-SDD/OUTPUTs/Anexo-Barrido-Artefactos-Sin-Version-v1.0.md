# Anexo — Barrido de artefactos mandados sin sufijo de versión

**Documento:** `Anexo-Barrido-Artefactos-Sin-Version-v1.0.md`
**Versión:** 1.1
**Estado:** Vigente
**Fecha:** 2026-07-28
**Autor:** Revisión SDD (Claude Code)
**Repositorio revisado:** `/IA/IA.SDD/`, antes de la intervención en `HEAD` `568a44d` y después con `Master-Prompt.md` v3.7
**Documento padre:** `Revision-Hallazgos-SDD-v1.0.md`

> Este anexo ejecuta la verificación **V-1** que `INPUTs/Evaluacion-SDD.md` §6 dejó pendiente: barrer las reglas buscando artefactos que el framework manda emitir sin sufijo de versión en el nombre. Las secciones §1 a §3 registran el barrido previo a la intervención, que es el que la fundamentó. **§4 es nueva y re-ejecuta el barrido contra el framework ya reparado.**

---

## §1 Alcance y método

La evaluación de origen enuncia V-1 como «barrer las **trece** reglas». El repositorio tiene **dieciséis** archivos en `SDD/Devs/Rules/`, que es lo que declaran `README.md` línea 29 y `SDD-Development-Guide.md` línea 123. El número «trece» proviene del changelog del master-prompt (`Master-Prompt.md` línea 798) y quedó desactualizado tras la incorporación de `Maqueta-Rules.md` y `Deriva-Rules.md`. El barrido de este anexo cubre los dieciséis, más `Master-Prompt.md`, `Root-Rules.md`, `Index-Modelos-UX-UI.md` y `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md`.

Método: búsqueda de las cadenas `sin versión`, `sin sufijo`, `README.md`, `AGENTS.md` y `_legacy` sobre el árbol, con lectura de cada ocurrencia en su sección. Es una verificación estática y reproducible; no requiere ejecutar una corrida.

---

## §2 Resultado

Se encontraron **nueve clases** de artefacto que el framework manda sin sufijo de versión en el nombre. La evaluación de origen identificó dos de ellas (filas 3 y 4).

| # | Artefacto | Dónde se manda sin versión | ¿Está en el alcance de `_legacy/`? | Tratamiento propuesto |
|---|---|---|---|---|
| 1 | `SDD/Docs/README.md` (README raíz de la salida) | `Root-Rules.md` línea 69 y línea 358 | Sí. Es el artefacto de una categoría transversal y cambia en cada corrida | Sufijo al archivar: `README-v<X.Y>.md` |
| 2 | `SDD/Docs/CHANGELOG.md` | `Root-Rules.md` líneas 44 y 69 | Discutible: es acumulativo, no tiene estado superado | **Exento**, con razón declarada: su historia es su propio contenido |
| 3 | `SDD/Docs/CONTRIBUTING.md`, `SDD/Docs/LICENSE.md` | `Root-Rules.md` líneas 45-46 y 69 | Sí | Sufijo al archivar |
| 4 | `README.md` de la sección `00-Contexto/` | `Rules-Contexto.md` línea 90 (§3.4) y línea 354 | Sí. **Produjo pérdida real** | Sufijo al archivar |
| 5 | `README.md` de la sección `01-Necesidades-Negocio/` | `Rules-Necesidades-Negocio.md` línea 103 (§3.4) | Sí. **Produjo pérdida real** | Sufijo al archivar |
| 6 | `README.md` de la sección `10-Examples/` | `Rules-Examples.md` líneas 17 y 140 | Sí | Sufijo al archivar |
| 7 | `README.md` índice de los cuerpos de `11-Documentacion/` | `Rules-Documentacion.md` línea 370 | Sí | Sufijo al archivar |
| 8 | `AGENTS.md` en la raíz del repositorio destino | `Rules-Documentacion.md` líneas 371 y 287 | No | **Exento**, con razón declarada: `Master-Prompt.md` línea 441 (§7.2) manda regenerarlo completo desde `Contrato-Agentes-v<X.Y>.md` en cada corrida, y ese contrato sí es versionado y archivable |
| 9 | Superficies HTML y assets de maqueta, incluido `index.html` | `Maqueta-Rules.md` líneas 92 y 95 | No | **Exento**, con razón declarada: la propia regla dice que «la maqueta se versiona con el repositorio, no con sufijo de archivo» |

### §2.1 Caso ya resuelto por el framework, que sirve de precedente

`Rules-Arquitectura-Tecnica.md` línea 151 (§3.6) declara una exención explícita y razonada para los ADR: «Ambos archivos coexisten en `Adrs/` sin moverse a `_legacy/`, para preservar la historia decisional». Es la prueba de que el framework ya admite el patrón «artefacto exento del archivado, con la razón declarada junto a la exención». Las filas 2, 8 y 9 deben resolverse de la misma forma, y no por omisión.

### §2.2 Artefacto declarado en el layout que ninguna regla gobierna

`Master-Prompt.md` línea 194 declara `SDD/Docs/Proyectos/<Nombre-Proyecto>/README.md` en el layout canónico. Ninguno de los dieciséis archivos de reglas lo gobierna: `Root-Rules.md` apunta a `SDD/Docs/README.md` (su línea 4 declara ese archivo target y solo ese), y ninguna regla de categoría reclama el README de proyecto. Queda sin contenido especificado, sin criterios de aceptación y sin tratamiento de archivado.

No es un caso de artefacto sin versión: es un caso de artefacto **sin regla**. Se registra acá porque aparece en el mismo barrido y porque cualquier corrección de la política de archivado tiene que decidir qué hacer con él.

### §2.3 Carpetas que la política usa y el layout no declara

`Master-Prompt.md` §3.5 (líneas 174-198) enumera el layout de salida. No aparece en él:

- `_legacy/`, en ninguna de sus formas, pese a que la política de deprecación de §5 línea 267 depende de esa carpeta.
- `SDD/Docs/Audit/`, pese a que §10 línea 599 escribe ahí el informe de cada auditoría de fase.

`Root-Rules.md` §2.1 (líneas 43-46), que es la otra fuente de estructura canónica, tampoco las declara. Un auditor que verifique «filename y estructura de carpetas correctos», criterio que §10 exige, no tiene contra qué contrastar esas dos carpetas.

---

## §3 Consecuencia para el arreglo A-1 de la evaluación

A-1 propone agregar el texto de la excepción «en cada §3.4 afectado», y nombra dos archivos. Con el barrido completo, el alcance real es de seis archivos de reglas para las filas archivables (1, 3, 4, 5, 6, 7), más tres exenciones a declarar (2, 8, 9). Repetir la misma cláusula en seis archivos tiene dos costos:

1. Seis copias del mismo texto que hay que mantener sincronizadas, que es exactamente el mecanismo que produjo H-2 (dos convenciones de ruta que divergieron).
2. Ninguna cobertura para el artefacto sin versión que se agregue en el futuro: una regla nueva que declare un índice sin sufijo no hereda nada.

La alternativa que el documento padre propone en R-1 es enunciar la regla **una sola vez** en la política de deprecación de `Master-Prompt.md` §5, con la tabla de exenciones al lado, y dejar en cada regla de categoría un puntero de una línea. Eso cubre los nueve casos, cubre los futuros y no duplica el enunciado.

---

## §4 Re-ejecución del barrido después de la intervención

Barrido repetido sobre el framework con `Master-Prompt.md` v3.7 y las cinco reglas actualizadas. **V-1 pasa**: las seis clases archivables declaran su tratamiento y las tres exentas declaran su razón.

| # | Artefacto | Tratamiento declarado | Dónde quedó |
|---|---|---|---|
| 1 | `SDD/Docs/README.md` | Sufijo al archivarse | `Root-Rules.md` §3.1 |
| 2 | `SDD/Docs/CHANGELOG.md` | **Exento**, por acumulativo | `Root-Rules.md` §3.1 y `Master-Prompt.md` §5.1 |
| 3 | `CONTRIBUTING.md`, `LICENSE.md` | Sufijo al archivarse, con la regla del README | `Root-Rules.md` §3.1 |
| 4 | `README.md` de `00-Contexto/` | Sufijo al archivarse | `Rules-Contexto.md` §3.4 |
| 5 | `README.md` de `01-Necesidades-Negocio/` | Sufijo al archivarse | `Rules-Necesidades-Negocio.md` §3.4 |
| 6 | `README.md` de `10-Examples/` | Sufijo al archivarse | `Rules-Examples.md` §3.1 |
| 7 | `README.md` índices de `11-Documentacion/` | Sufijo al archivarse | `Rules-Documentacion.md` §3.1 |
| 8 | `AGENTS.md` | **Exento**, se regenera del contrato versionado | `Rules-Documentacion.md` §3.1 y `Master-Prompt.md` §5.1 |
| 9 | Superficies y assets de maqueta | **Exento**, se versiona con el repositorio | `Master-Prompt.md` §5.1 |

Se sumó a la tabla de exenciones de `Master-Prompt.md` §5.1 un caso que este barrido no había clasificado como tal: el campo `evidencia` de los contratos `VER-XX`. No es un artefacto sin sufijo sino un campo, pero era una excepción a D5 declarada en §7.2 y ausente de la política, así que quedó registrada junto a las otras cuatro.

### §4.1 Los dos casos de §2.2 y §2.3, al cierre

| Caso | Estado |
|---|---|
| `SDD/Docs/Proyectos/<Nombre-Proyecto>/README.md` sin regla que lo gobierne | **Sigue abierto.** Es la observación O-3 del documento padre. Excede el alcance de la política de archivado: requiere decidir qué categoría lo adopta |
| El layout canónico no declaraba `_legacy/` ni `SDD/Docs/Audit/` | **Resuelto.** `Master-Prompt.md` §3.5 declara `Audit/` en el bloque de layout y explica dónde aparece `_legacy/` y por qué no ocupa posición fija |

### §4.2 Una corrección a §1 de este anexo

§1 observa que la evaluación de origen enuncia V-1 sobre «las trece reglas» cuando son dieciséis, y atribuye el número al changelog del master-prompt. Eso se verificó y es correcto, pero **no se corrigió** en la intervención: la frase vive dentro de una fila histórica de control de cambios, y reescribir un registro pasado falsearía lo que esa intervención declaró en su momento. Queda como observación O-1 del documento padre.

---

## Control de cambios

| Versión | Fecha | Cambios | Autor |
|---|---|---|---|
| 1.1 | 2026-07-28 | Se agrega §4 con la re-ejecución del barrido contra el framework ya reparado: V-1 pasa, las seis clases archivables declaran su tratamiento y las tres exentas su razón, con el archivo y la sección donde quedó cada una. Se registra la exención del campo `evidencia` de los contratos `VER-XX`, que este barrido no había clasificado. §4.1 cierra los dos casos abiertos de §2.2 y §2.3, uno resuelto y otro no. §4.2 explica por qué el «trece reglas» de §1 no se corrigió. | Revisión SDD (Claude Code) |
| 1.0 | 2026-07-28 | Ejecución de la verificación V-1 sobre los dieciséis archivos de reglas más el master-prompt, `Root-Rules.md`, el índice de modelos UX-UI y el prompt de entrada. Nueve clases de artefacto sin sufijo de versión, seis archivables y tres a eximir. Se registran además el README de proyecto sin regla que lo gobierne y las dos carpetas que la política usa sin que el layout canónico las declare. | Revisión SDD (Claude Code) |
