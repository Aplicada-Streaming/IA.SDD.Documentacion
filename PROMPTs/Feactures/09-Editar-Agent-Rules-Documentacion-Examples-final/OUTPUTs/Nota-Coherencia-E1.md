# Nota de coherencia — E1 Renombrado estructural (intercambio de categorías 10 ↔ 11)

**Proyecto:** Framework SDD
**Documento:** Nota-Coherencia-E1.md
**Versión:** 1.0
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD

---

## 1. Alcance

Verificación de implantación de la etapa E1, que ejecuta la solicitud S1: renombre del archivo de reglas de la categoría de documentación, intercambio de las categorías 10 y 11 en todo el árbol `/IA/IA.SDD/`, fijación de las carpetas target nuevas, inversión de la dependencia declarada entre ambas categorías, reasignación de los identificadores de subagente, barrido de propagación y normalización del vocabulario de actores. Incluye la limpieza de autosuficiencia decidida en DEC-02.

E1 no redefine contenido: la ampliación de la categoría 10 corresponde a E2 y la reescritura de la categoría 11 a E3. Sobre los dos archivos que esas etapas reescriben, E1 se limitó a la cabecera, a la renumeración y a la dependencia declarada.

## 2. Inventario de archivos

### 2.1 Renombrado

| Antes | Después | Método |
| --- | --- | --- |
| `SDD/Devs/Rules/Rules-Developer-Guide.md` | `SDD/Devs/Rules/Rules-Documentacion.md` | `git mv`, historial preservado |

### 2.2 Modificados

| Archivo | Versión | Cambio de E1 |
| --- | --- | --- |
| `SDD/Devs/Rules/Rules-Examples.md` | 1.2 → 1.3 | Título, carpeta target `10-Examples/`, subagente AG-10, §0 con la dependencia invertida y downstream hacia 11, §1.3 y §3.3 reorientadas, vocabulario |
| `SDD/Devs/Rules/Rules-Documentacion.md` | 1.2 → 1.3 | Título, carpetas target por proyecto y de solución, subagente AG-11 Technical Writer / Documentation Lead, §0 con upstream desde 10, renumeración completa y vocabulario |
| `SDD/Devs/Rules/Root-Rules.md` | 1.3 → 1.4 | Mapa de documentación (`10-Examples`, `11-Documentacion`), flujo de lectura del integrador invertido a 10 → 11 → 02, subagentes de §1.3, vocabulario |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 3.4 → 3.5 | §3.5 layout, §6 filas F y G, §7 ejecución por fases, §14 tabla de adaptabilidad D8 con sus dos últimas columnas intercambiadas |
| `SDD/Devs/Rules/Rules-Contexto.md` | 1.3 → 1.4 | Trazabilidad downstream a `10-Examples`, vocabulario |
| `SDD/Devs/Rules/Rules-Calidad-Y-Pruebas.md` | 1.3 → 1.4 | Multi-especialidad AG-11, vocabulario |
| `SDD/Devs/Rules/Maqueta-Rules.md` | 1.0 → 1.1 | Analogía de §3.7 hacia `10-Examples` |
| `SDD/Devs/Rules/Intake-Rules.md` | 1.0 → 1.1 | Campo de cabecera «Consumidor» a «Lector» |
| `SDD/Devs/Rules/Rules-Necesidades-Negocio.md` | 1.2 → 1.3 | Vocabulario |
| `SDD/Devs/Rules/Rules-UX-UI-DX.md` | 1.6 → 1.7 | Vocabulario en las secciones DX |
| `SDD/Devs/Rules/Rules-Prompts-AI.md` | 1.2 → 1.3 | Vocabulario |
| `SDD/Devs/Rules/Rules-Devops.md` | 1.3 → 1.4 | Vocabulario |
| `SDD/Devs/Intake/SOLUTION-INTAKE-template.md` | 1.1 → 1.2 | Destinos de samples y de documentación, vocabulario |
| `SDD/Devs/Guides/Marco-Teorico-SDD-v1.0.md` | 1.6 → 1.7 | Mapa visual §1.5, catálogo de especialidades §4.2, tabla Diátaxis §8.5, vocabulario |
| `SDD/Guides/SDD-User-Guide.md` | 1.3 → 1.4 | §2, §4 fases F y G, §5, §10 mapa de carpetas, vocabulario |
| `SDD/Guides/SDD-Getting-Started-Guide.md` | 1.0 → 1.1 | Limpieza de autosuficiencia (DEC-02) |
| `PROMPTS/PROMPT-Agente-Bootstrap-SDD.md` | 2.1 → 2.2 | Nombre real de la carpeta de prompts en el árbol de §1 |

Diecisiete archivos modificados y uno renombrado. Ninguna eliminación.

### 2.3 Deliberadamente no tocados

`SDD/Devs/Bootstrap/` (7 archivos), `SDD/Devs/Reformulacion/` (4) y `SDD/Devs/Intake/_legacy/` (2), congelados por DEC-01 como registro auditado del estado anterior. `CHANGELOG.md` de la raíz: sus entradas ya escritas son registro histórico; la entrada de esta intervención la emite E7.

## 3. Resultado del barrido de propagación (S1.5)

Conteos medidos con `grep -o` sobre todos los `.md` del árbol. «Línea de base» es el conteo de E0; «restantes» es el conteo tras esta etapa.

| Cadena | Línea de base | Restantes | Corregidas | Restantes en congelados | Restantes en filas de control de cambios |
| --- | --- | --- | --- | --- | --- |
| `10-Developer-Guide` | 25 | 9 | 16 | 7 | 2 |
| `11-Examples` | 38 | 13 | 25 | 11 | 2 |
| `Rules-Developer-Guide` | 13 | 9 | 4 | 9 | 0 |
| `categoría 10` / `categoría 11` | 14 / 9 | — | 14 / 9 | 0 | 0 |
| `categoria 10` / `categoria 11` (sin tilde) | 0 / 0 | 0 | — | — | La variante sin tilde no existe en el repositorio |
| `Fase F` / `Fase G` | 5 / 4 | 5 / 4 | 5 / 4 | 0 | 0 |

Las etiquetas `Fase F` y `Fase G` no cambian: se corrigió el contenido asociado a cada una, que es lo que S1.5 pide. Ambas conservan su letra y cambia qué categoría produce cada una.

Reasignación de subagentes (S1.6). Los conteos totales de `AG-10` y `AG-11` **suben** respecto de la línea de base porque las filas de control de cambios nuevas citan ambos identificadores al describir la reasignación. El desglose real:

| Identificador | Línea de base | Restantes totales | En archivos vivos | En congelados | En filas nuevas |
| --- | --- | --- | --- | --- | --- |
| `AG-10` | 25 | 28 | 20 | 3 | 5 |
| `AG-11` | 33 | 41 | 26 | 9 | 6 |

Las 46 ocurrencias en archivos vivos corresponden al mapeo nuevo —AG-10 Developer Advocate para ejemplos, AG-11 Technical Writer / Documentation Lead para documentación— más las referencias colectivas «AG-00 a AG-11» y «AG-02 a AG-11», que siguen siendo correctas porque el rango de identificadores no cambió.

Verificación de cierre: cero ocurrencias de las tres primeras cadenas en archivos vivos, excluidas las filas de control de cambios.

**Decisión de criterio, para registro.** S1.5 pide corregir siempre las cuatro primeras cadenas. Las filas de control de cambios ya escritas quedaron fuera de esa corrección, por el mismo principio que S1.7 aplica al vocabulario: una fila de changelog describe lo que se hizo en una fecha determinada, y reescribirla haría que el registro mienta. Cada archivo afectado incorpora una fila nueva que declara el cambio de nombre de la carpeta, de modo que un lector no queda sin la información.

## 4. Resultado de la normalización de vocabulario (S1.7)

Conteos medidos con `grep -oi` sobre la raíz del término, de modo que `consumidor` incluye a `consumidores` y `audiencia` a `audiencias`.

| Término | Línea de base | Restantes | Sustituidas | Restantes en vivos | En congelados | En filas nuevas |
| --- | --- | --- | --- | --- | --- | --- |
| `consumidor` | 109 | 49 | 60 | 33 | 5 | 11 |
| `audiencia` | 57 | 38 | 19 | 27 | 5 | 6 |
| `constructor` | 1 | 1 | 1 | 0 | 0 | 1 |
| `implementador` | 12 | 15 | 0 | 12 | 0 | 3 |

Criterio aplicado a cada término:

- **`consumidor` → `integrador`** donde designa a quien consume el producto o el artefacto publicado. Se conserva en sentido técnico, que es el grueso de las 33 restantes en archivos vivos: proyecto consumidor del grafo de dependencias, US consumidora de una BT, consumer group de mensajería, componente que consume la salida de un prompt, superficie que consume un contrato de configuración o de identidad de versión.
- **`constructor` → `mantenedor`**. Tenía una sola ocurrencia real, en §1.1 de la regla de documentación. La ocurrencia restante es la cita del término dentro de su propia fila de control de cambios.
- **`implementador`: sin sustituciones, excepción DEC-04.** Designa la categoría de stakeholder del modelo RACI del intake (propietario / implementador / beneficiario), no un rol de intervención documental. Sustituirlo habría alterado el modelo de stakeholders de las categorías 00 y 01. El conteo sube de 12 a 15 porque tres filas de control de cambios nuevas explican por qué se conserva.
- **`audiencia` → `rol de intervención`** donde designa a quien lee o interviene sobre la documentación. Se conserva donde designa al público del producto: secciones UX, visión de producto, campo `audience` del frontmatter.

Cuatro sustituciones de «audiencia» se revirtieron durante la etapa al detectarse que designaban al público del producto y no a un lector de documentación. La distinción se declara acá porque no estaba en la tabla de correspondencia del Contexto, que trataba el término como un caso único.

## 5. Resultado de la limpieza de autosuficiencia (DEC-02)

`SDD-Getting-Started-Guide.md`: las cinco rutas absolutas a un repositorio de documentación nominado pasan al placeholder `<RUTA-DOCUMENTACION>`, la carpeta de tool-prompts se nombra `Prompts/`, y el bloque `traces` del frontmatter deja de citar dos archivos que viven fuera de este repositorio. El modelo de tres repositorios y el ejemplo aplicado de §6 se conservan íntegros: describen el método, no crean una dependencia de ruta.

`Marco-Teorico-SDD-v1.0.md` y `PROMPT-Agente-Bootstrap-SDD.md`: el árbol de carpetas nombraba la carpeta de prompts de entrada como `PROMPTs/`. Su nombre real en el repositorio es `PROMPTS/`. La corrección es fáctica además de normativa.

Resultado: las once cadenas prohibidas dan **cero ocurrencias** en todo el árbol. Era 12 para `PROMPTs/` antes de esta etapa.

## 6. Lista de comprobación

| # | Comprobación | Resultado | Evidencia |
| --- | --- | --- | --- |
| 1 | Invariantes D1–D9 intactas en todo archivo tocado | Cumple | D1 español rioplatense técnico en todas las adiciones. D2 UTF-8 sin BOM, LF. D3 y D4: los nombres nuevos `Rules-Documentacion.md`, `10-Examples/` y `11-Documentacion/` respetan Título-Con-Guiones y el sufijo con guion medio. D5: cada archivo sube versión in situ, sin copias paralelas. D6: cabeceras y control de cambios actualizados. D7: no se introdujo ningún literal de dominio; se retiraron cinco rutas con nombre de solución concreta. D8: el conjunto sigue teniendo ocho valores, verificado en `Root-Rules.md`. D9: no se afirmó nada sobre el estado del sistema sin verificarlo con `grep` |
| 2 | Autosuficiencia: cero referencias fuera de `/IA/IA.SDD/` | Cumple | Las once cadenas dan cero. Cero URLs nuevas: el diff de la etapa no agrega ninguna línea con `http` |
| 3 | Referencias internas: todo archivo, carpeta y sección citada existe | Cumple | Barrido de enlaces relativos sin roturas atribuibles a E1. Ver observación 3 |
| 4 | Vocabulario normalizado según la decisión 5 del Contexto | Cumple con excepciones declaradas | §4 de esta nota. Excepciones: DEC-04 y los usos técnicos de «consumidor» |
| 5 | Sin contradicción con etapas anteriores | Cumple | E0 no escribió |
| 6 | Control de cambios actualizado en cada archivo modificado | Cumple | Una fila por archivo, fechada 2026-07-26, sin citar el prompt de origen ni su repositorio |
| 7 | El caso degenerado sigue produciendo el layout aplanado | Cumple | `Master-Prompt.md` §3.5 intacto; solo cambiaron los nombres de las dos carpetas que enumera |
| 8 | Nada fuera del alcance declarado de la etapa fue modificado | Cumple con una ampliación autorizada | La única ampliación es la limpieza de `SDD-Getting-Started-Guide.md`, autorizada por DEC-02 |

## 7. Observaciones

1. **El orden de generación quedó consistente pero provisorio.** E1 dejó la Fase F produciendo 09 y 10, y la Fase G produciendo 11, que es la aplicación directa del intercambio y respeta la dependencia nueva (los ejemplos antes que la documentación que los referencia). El corte definitivo en cinco fases —F, G, H, I y J— es de E4. Hasta entonces el framework es coherente y utilizable, que es lo que exige el principio de segmentación.
2. **La categoría 11 sigue declarada opcional para cuatro tipos D8.** El cambio de gating que la vuelve obligatoria para los ocho, con granularidad por cuerpo, es de E3. Registrado acá para que no se lea como omisión de E1.
3. **Enlace ilustrativo sin destino real** (informativa, preexistente). `Rules-Necesidades-Negocio.md` §7 contiene un ejemplo con un enlace a `Necesidades-De-Negocio/NB-01-…-v1.0.md`, que es un archivo del repositorio destino generado y no existe en `IA.SDD`. Es correcto en su contexto de ejemplo. No lo introdujo esta etapa.
4. **`SOLUTION-INTAKE-template.md` no declara versión en cabecera**, solo en su tabla de control de cambios; el campo `| Versión | 1.0 |` de su cabecera pertenece al documento que la plantilla genera, no a la plantilla. Se agregó la fila 1.2 sin tocar ese campo. Discrepancia menor y preexistente en la aplicación de D6 a las plantillas.
5. **`SDD-Getting-Started-Guide.md` conserva el nombre de una solución concreta** en su §6 de ejemplo aplicado. Está fuera del alcance de la restricción de autosuficiencia, que prohíbe rutas a otros repositorios, no ejemplos nominados. Se señala porque roza D7 y porque, si el responsable del framework quisiera despersonalizarlo del todo, es una decisión propia y no una consecuencia de esta intervención.

## 8. Veredicto

**CONFORME.**

El intercambio 10 ↔ 11 quedó propagado en los diecisiete archivos vivos que lo requerían, con las carpetas target nuevas, los subagentes reasignados y la dependencia invertida en las dos direcciones. La normalización de vocabulario se aplicó con criterio declarado y sus excepciones están justificadas una por una. La restricción de autosuficiencia, que el repositorio incumplía antes de esta intervención, pasó a cumplirse. Las cinco observaciones son dos diferimientos hacia etapas posteriores, ya previstos por la segmentación, y tres hallazgos preexistentes sin impacto sobre la coherencia.

Corresponde detenerse y esperar confirmación humana antes de arrancar E2.

## 9. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.0 | 2026-07-26 | Nota de coherencia inicial de la etapa E1: inventario de dieciocho archivos afectados, resultado del barrido de propagación y de la normalización de vocabulario con sus excepciones, resultado de la limpieza de autosuficiencia, lista de comprobación de ocho puntos con verificación D1–D9, cinco observaciones y veredicto CONFORME. | Reformulación SDD |
