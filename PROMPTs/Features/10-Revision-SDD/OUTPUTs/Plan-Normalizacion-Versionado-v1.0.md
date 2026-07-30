# Plan de normalización del versionado y del archivado

**Documento:** `Plan-Normalizacion-Versionado-v1.0.md`
**Versión:** 1.0
**Estado:** Propuesto, sin aplicar
**Fecha:** 2026-07-28
**Autor:** Revisión SDD (Claude Code)
**Repositorio a intervenir:** `/IA/IA.SDD/`, hoy en `Master-Prompt.md` v3.7 y changelog `[3.2]`
**Documentos relacionados:** `Revision-Hallazgos-SDD-v1.0.md`, `Alternativa-Eliminar-Legacy-v1.0.md`

> Ningún archivo fue modificado por este plan. Es una propuesta para evaluar antes de ejecutar.

---

## §1 Objetivo

Que el framework tenga **una sola lógica de versionado y archivado**, aplicada uniformemente dentro de cada plano.

El criterio rector lo fijó el responsable del framework: *no pueden convivir dos lógicas de versionado dentro del mismo objeto*. Que el framework y el cliente tengan cada uno la suya es correcto; que adentro de cualquiera de los dos convivan dos, no.

No es objetivo preservar el historial ni mantener compatibilidad con lo anterior. Se traza una línea nueva y uniforme, con reinicio de versiones donde haga falta.

---

## §2 Por qué hace falta: los dos planos hoy incumplen el criterio

Verificado sobre el árbol actual.

**Dentro del framework:**

| Lógica | Archivos |
|---|---|
| Nombre estable, versión en la cabecera | 34 |
| Versión en el nombre | 11, de los cuales **4 mienten**: `Marco-Teorico-SDD-v1.0.md` contiene la versión 1.8 |

**Dentro del cliente:**

| Lógica | Artefactos |
|---|---|
| Versión en el nombre (`Vision-Producto-v1.0.md`) | La mayoría |
| Nombre estable (README de sección, README raíz, `AGENTS.md`, maqueta) | 9 clases |

**En los dos planos, los defectos aparecieron en la frontera entre las dos lógicas.** H-1 —que produjo dos pérdidas reales— es la colisión entre el README de nombre estable y una política de archivado escrita para nombres versionados. Los cuatro nombres desactualizados del framework son el mismo choque del otro lado. No es coincidencia: es el costo de la duplicidad, medido.

---

## §3 La estrategia

### §3.1 La regla única

> **En la carpeta de trabajo hay un solo archivo por nombre lógico, sin sufijo de versión. La versión vive en la cabecera del documento. Al ser superado, el archivo se copia completo a `_legacy/`, y la copia archivada sí recibe el sufijo de la versión que preserva.**

Aplica a todo artefacto de los dos planos, sin excepciones de nombre.

```
carpeta/
  |- Nombre-Logico.md                    ← único, la cabecera dice qué versión es
  |- _legacy/
       |- <eje>/
            |- Nombre-Logico-v1.0.md     ← copia completa, autocontenida
            |- Nombre-Logico-v1.1.md
```

Propiedades que se obtienen, y que hoy no se tienen todas juntas:

| | |
|---|---|
| Cuál es la vigente | Imposible dudar: hay una sola |
| Contexto que ingiere un agente | Mínimo. `_legacy/` es señal legible de «salteame» |
| Cascada de referencias al subir de versión | **Ninguna.** Los enlaces apuntan al nombre lógico y nunca cambian de destino |
| Cada versión archivada | Autocontenida, no un delta |
| Consultar una versión anterior | Abrir un archivo. Sin git, sin ramas |
| Cantidad de lógicas por plano | **Una** |

### §3.2 El eje de agrupación de `_legacy/`, y por qué difiere entre planos

Es la única diferencia entre los dos planos, y está fundada en qué se necesita reconstruir en cada uno.

| Plano | Eje | Qué se archiva | Por qué |
|---|---|---|---|
| **Framework** | `_legacy/<version-del-framework>/` | El **conjunto normativo completo** | Las reglas son interdependientes. `Rules-Contexto` 1.6 junto a `Master-Prompt` 3.9 puede ser una combinación que nunca existió. Lo que hay que poder reconstruir es el set coherente, no archivos sueltos |
| **Cliente** | `<carpeta-del-artefacto>/_legacy/<YYYY-MM-DD>/` | El **artefacto individual** | Acá se consulta un documento puntual: «qué decía la visión de producto antes de que cambiáramos el alcance». No hay conjunto coherente que reconstruir |

Costo del snapshot del framework, medido: **49 archivos markdown, 1,5 MB**. Una copia completa por versión declarada en el changelog. Diez versiones son 15 MB en un repositorio de documentación. Es barato.

Y no se toma en cada edición: se toma **una vez por entrada del changelog**, o sea por versión publicada del framework.

### §3.3 Git queda afuera de la mecánica de versionado

Es la consecuencia importante, y responde a la objeción de que reconstruir por git obliga a crear ramas.

| | Rol |
|---|---|
| `CHANGELOG.md` | **Es** el mecanismo de versionado del framework. Una entrada = una versión = un snapshot en `_legacy/` |
| `_legacy/<version>/` | Hace cada versión declarada reconstruible **desde el propio árbol** |
| git | Control de código fuente y nada más. No hacen falta tags para esta mecánica |

Un agente que necesite planificar la actualización de un cliente generado con SDD 4.0 hacia el árbol vigente lee `_legacy/4.0/` y tiene el conjunto normativo de entonces, con las mismas herramientas con las que lee cualquier otro archivo. Sin `git checkout`, sin ramas, sin conocimiento de git.

**No hace falta un segundo changelog.** El `CHANGELOG.md` declara versiones del framework; el log de git registra commits. Son dos cosas distintas que no se mezclan y ninguna necesita a la otra.

### §3.4 Criterio de versionado

**Del lado del framework**, se conserva el que ya existe en `README.md` líneas 118-123, sin cambios: errata sin subir versión; agregar artefacto o criterio, minor; cambiar gating o conjunto de artefactos, major; tocar invariante, máximo impacto con nota de coherencia.

La versión del conjunto se deriva de la mayor severidad de sus partes: si alguna regla sube major, el framework sube major; si alguna sube minor y ninguna major, sube minor; si no cambia ninguna regla, sube patch. Es lo que ya se venía aplicando de hecho en las entradas `[3.0]`, `[3.1]` y `[3.2]`.

**Del lado del cliente**, tres reglas:

| Regla | Enunciado |
|---|---|
| **Estado** | En `Borrador` o `Propuesto` el documento absorbe correcciones sin subir de versión: el audit es parte de su ciclo de emisión. Desde `Aprobado` o `Vigente`, toda corrección sube de versión y archiva la anterior. **Ya aplicada** en `Master-Prompt.md` §5 |
| **Minor / major** | La pregunta es una sola: *¿algún documento aguas abajo deja de ser correcto?* Si no, minor. Si sí, major, y los downstream deben revisarse. Es el mismo criterio que `SDD-Development-Guide.md:457` ya usa para el framework, mirado desde el otro lado de la cadena D6 |
| **Errata** | Typos, formato, tabla de contenido: sin subir versión, sin archivar |

La regla de estado es la que vuelve viable todo lo demás: sin ella, las cinco rondas de corrección de un ciclo de auditoría producen cinco versiones archivadas que ningún lector vio.

### §3.5 Procedencia del framework en el cliente

Bloque nuevo en el `SOLUTION-MANIFEST`, escrito por el orquestador al derivarlo en §3:

```markdown
## Procedencia del framework

| Framework SDD | 4.0 |
| Master-Prompt | 1.0 |
| Rules-Contexto | 1.0 |
| … una fila por regla efectivamente usada |
```

El orquestador ya abre cada archivo de reglas para armar el despacho —copia la fila de §1.2 para la especialidad del subagente—, así que leer el campo de versión de la misma cabecera no agrega ninguna lectura.

Con eso, un árbol de cliente declara bajo qué normativa se generó, y `_legacy/<version>/` del framework permite recuperar esa normativa exacta.

---

## §4 Qué implica sobre las invariantes

Es el punto de mayor impacto del plan y hay que declararlo sin rodeos.

| Invariante | Hoy | Después |
|---|---|---|
| **D4 — Sufijo de versión** | «`-v<X.Y>.md` con guion medio» rige para el nombre de todo artefacto versionable | Rige **para las copias archivadas en `_legacy/`**. El archivo vivo lleva nombre lógico estable y su versión en la cabecera |
| **D5 — Una sola versión vigente** | «Un nombre lógico tiene una única versión vigente. Las superadas se archivan en `_legacy/`» | Se conserva el principio y se precisa: una sola versión vigente **es un solo archivo por nombre lógico en la carpeta de trabajo**. Deja de ser una regla que hay que cumplir y pasa a ser una propiedad estructural |

`README.md:122` declara que modificar una invariante es el cambio de mayor impacto del framework. Acá se modifican dos. Tres consideraciones que acotan el costo:

1. **Es un cambio permisivo en la forma, restrictivo en el fondo.** D5 se vuelve más fácil de cumplir, no más difícil: con un solo archivo por nombre lógico, violarla requiere esfuerzo.
2. **No hay documentación previa que migrar.** El responsable declaró que no se busca preservar el historial ni la compatibilidad. Los árboles de cliente ya generados quedan como están, bajo la normativa con la que se hicieron, que es exactamente lo que la procedencia de §3.5 permite declarar.
3. **Requiere decisión explícita del responsable y nota de coherencia**, según `README.md:122-123`. Este documento es el insumo de esa decisión.

---

## §5 Alcance medido

Los tres costos, verificados sobre el árbol actual.

| # | Trabajo | Volumen | Naturaleza |
|---|---|---|---|
| 1 | Renombrar los 11 archivos del framework que llevan sufijo, y actualizar sus referencias | **163 referencias**, una sola vez | Mecánico, verificable con un barrido |
| 2 | Quitar el patrón `-v<X.Y>.md` de los patrones de nombre que las reglas declaran para los artefactos del cliente | **274 ocurrencias en 16 archivos**: los 15 archivos de reglas que lo declaran más el master-prompt | Mecánico en su mayoría, con revisión de contexto |
| 3 | Reescribir la política de §5 y §5.1 del master-prompt, D4 y D5 en el README, y la guía de desarrollo | 4 archivos | Redacción normativa |

**No requiere tocar** las diez reglas en su cláusula de archivado: al desaparecer la distinción entre artefactos con y sin sufijo, `_legacy/` pasa a ser uniforme y lo que hoy escriben sigue siendo correcto.

---

## §6 Etapas

Ordenadas por dependencia. Cada una cierra con su verificación.

| # | Etapa | Archivos | Qué deja listo |
|---|---|---|---|
| **E1** | Reformular D4 y D5 en `README.md`, y el criterio de intervención de sus líneas 118-123 | `README.md` | La base normativa que todo lo demás cita |
| **E2** | Renombrar los 11 archivos con sufijo a nombre lógico estable; actualizar las 163 referencias; reiniciar las cabeceras de versión | 11 renombrados + los que los citan | El framework con una sola lógica adentro |
| **E3** | Declarar la mecánica de `_legacy/<version-del-framework>/` en `SDD-Development-Guide.md` y en la anatomía del `README.md`; crear el primer snapshot | `SDD-Development-Guide.md`, `README.md`, `_legacy/` | Reconstrucción de versiones sin git |
| **E4** | Reescribir `Master-Prompt.md` §5 y §5.1 con la regla única; ajustar §3.5 del layout | `Master-Prompt.md` | La política que el orquestador inyecta a los subagentes |
| **E5** | Quitar el sufijo de los patrones de nombre de las 16 reglas | 16 archivos, 274 ocurrencias | El cliente con una sola lógica adentro |
| **E6** | Bloque de procedencia en `SOLUTION-MANIFEST-template.md` y su derivación en `Master-Prompt.md` §3 | 2 archivos | Poder decir con qué framework se generó un cliente |
| **E7** | Verificación, nota de coherencia y entrada `[4.0]` del changelog | `CHANGELOG.md` + nota | Cierre trazable |

### §6.1 Reinicio de versiones

El responsable declaró que se puede reiniciar como si el framework recién empezara. La propuesta es asimétrica y conviene justificarla:

| Qué | Propuesta | Motivo |
|---|---|---|
| Versión de cada archivo | **Reinicio a 1.0** | Empieza la línea nueva. Las versiones anteriores describen un esquema que deja de existir |
| Versión del conjunto en el `CHANGELOG.md` | **Continúa: `[4.0]`** | Es la identidad del framework y lo que permite decir «este cliente se generó con SDD 4.0». Reiniciarla a 1.0 haría ambiguas las referencias a las versiones ya publicadas. El salto a major declara la discontinuidad |

---

## §7 Qué pasa con la reparación aplicada hoy

La intervención del `[3.2]` no se pierde, pero cambia de forma.

| Reparación | Destino |
|---|---|
| R-1 · sufijo al archivar artefactos sin versión | **Desaparece como caso especial.** Al no haber artefactos con sufijo en el nombre, la excepción que R-1 resolvía deja de existir. **H-1 deja de ser una clase de problema posible** |
| R-2 · ruta única de archivado | **Se conserva**, con el eje de fecha del lado del cliente |
| R-3 · snapshot a cargo del orquestador | **Se conserva sin cambios.** Sigue siendo necesario |
| R-4 · estado `Superado` y nota | **Se conserva sin cambios** |
| R-5 · versionado por estado de cabecera | **Se conserva.** Es la regla de estado de §3.4 |
| R-6 · eje de ronda en el informe de audit | **Se conserva sin cambios** |
| R-7 · layout declara `_legacy/` y `Audit/` | **Se conserva y se amplía** con `_legacy/<version>/` del framework |
| R-8 · exenciones y Fases I/J | **Se conserva**, con la tabla de exenciones reducida: `AGENTS.md`, maqueta, ADR y `VER-XX` siguen exentos; `CHANGELOG.md` deja de necesitar mención especial |

---

## §8 Lo que este plan NO resuelve, y hace falta

**El flujo de re-evaluación no existe.** `Master-Prompt.md:30` solo ofrece dos caminos cuando `SDD/Docs/` del destino tiene contenido previo: archivar todo y empezar de cero, o abortar. No hay un flujo que tome un árbol generado con SDD 4.0, lo contraste contra el framework vigente e informe qué reglas subieron major y qué documentos quedaron invalidados.

Sin ese flujo, `_legacy/<version>/` y la procedencia del manifiesto quedan sin consumidor: tendrías la normativa vieja guardada, sabrías con cuál se generó cada árbol, y ningún mecanismo que use ese dato.

Es una fase nueva del orquestador, no una normalización, así que se propone como intervención siguiente y no como etapa de este plan. Pero es la que le da sentido a las etapas E3 y E6.

---

## §9 Verificaciones de cierre

| # | Verificación | Resultado esperado |
|---|---|---|
| N-1 | Buscar `-v<X.Y>` en todo el árbol del framework | Solo aparece describiendo el nombre de copias archivadas |
| N-2 | Listar los nombres de archivo del framework | Cero archivos con sufijo de versión en el nombre |
| N-3 | Contrastar el campo `Versión` de cada archivo contra su nombre | Ninguna contradicción posible: el nombre ya no afirma una versión |
| N-4 | Resolver todos los enlaces internos del framework | Cero enlaces rotos tras los 163 reemplazos |
| N-5 | Leer `_legacy/4.0/` como si fuera un agente sin contexto | Se obtiene el conjunto normativo completo y coherente, sin usar git |
| N-6 | Simular tres subidas de versión de un documento de cliente | La carpeta de trabajo mantiene un solo archivo por nombre lógico; ninguna referencia entrante cambió |
| N-7 | Leer un `SOLUTION-MANIFEST` derivado | Declara la versión del framework y de cada regla usada |

---

## Control de cambios

| Versión | Fecha | Cambios | Autor |
|---|---|---|---|
| 1.0 | 2026-07-28 | Plan inicial de normalización. Regla única de versionado y archivado para los dos planos, con el eje de agrupación de `_legacy/` diferenciado y justificado. Git queda fuera de la mecánica de versionado: el `CHANGELOG.md` versiona y `_legacy/<version>/` hace reconstruible cada versión desde el árbol. Criterio de versionado para ambos planos. Bloque de procedencia en el manifiesto. Impacto sobre D4 y D5 declarado. Alcance medido: 163 referencias de renombre, 274 ocurrencias del patrón de nombre en 16 archivos, 4 archivos de redacción normativa. Siete etapas, reinicio asimétrico de versiones y siete verificaciones de cierre. Se declara fuera de alcance el flujo de re-evaluación, que es el consumidor natural de las etapas E3 y E6. | Revisión SDD (Claude Code) |
