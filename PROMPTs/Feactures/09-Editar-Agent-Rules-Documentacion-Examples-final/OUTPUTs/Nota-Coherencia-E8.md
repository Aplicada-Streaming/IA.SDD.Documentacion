# Nota de coherencia — E8 Cierre de decisiones abiertas

**Proyecto:** Framework SDD
**Documento:** Nota-Coherencia-E8.md
**Versión:** 1.0
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD

---

## 1. Alcance

Etapa no prevista en la segmentación original. Se abre para cerrar las siete decisiones que el informe de intervención dejó abiertas y las inconsistencias accionables que registró, por indicación del responsable del framework de no dejar nada pendiente.

Al medir el alcance de la primera decisión apareció un hallazgo que ninguna de las siete describía correctamente: **E2 introdujo una contradicción entre dos archivos de reglas y ni la nota de E2 ni el informe la detectaron**. La decisión A-2 estaba mal enunciada —hablaba de una mención faltante— cuando lo real era que las dos reglas se contradecían. Ese hallazgo justifica esta etapa por sí solo.

## 2. Inventario de archivos

| Archivo | Versión | Cambio |
| --- | --- | --- |
| `SDD/Devs/Rules/Rules-Calidad-Y-Pruebas.md` | 1.6 → 1.7 | Corrección de la contradicción con `Deriva-Rules.md` |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 3.6 → 3.6 | Nomenclatura D1–D9 y enumeraciones completadas |
| `SDD/Guides/SDD-User-Guide.md` | 1.5 → 1.5 | Nomenclatura D1–D9 y enumeraciones completadas |
| `SDD/Guides/SDD-Getting-Started-Guide.md` | 1.1 → 1.2 | Nomenclatura y despersonalización del ejemplo aplicado |
| `SDD/Guides/SDD-Development-Guide.md` | 1.0 → 1.1 | Dos ejes de extensión nuevos |
| `SDD/Devs/Guides/Marco-Teorico-SDD-v1.0.md` | 1.7 → 1.8 | Nomenclatura y corrección de referencia muerta |
| `SDD/Devs/Rules/Root-Rules.md` | 1.4 → 1.4 | Nomenclatura |
| `SDD/Devs/Rules/Rules-Necesidades-Negocio.md` | 1.4 → 1.4 | Nomenclatura |
| `SDD/Devs/References/Design/Design-Rules-Blazor-Mudblazor-v1.0.md` | sin cambio de versión | Nomenclatura |
| `SDD/Devs/Intake/SOLUTION-INTAKE-template.md` | 1.2 → 1.3 | Versión de plantilla en cabecera |
| `README.md` | Sin versión (índice) | Nota sobre el origen de D9 reformulada |
| `CHANGELOG.md` | 3.0 | La entrada absorbe las correcciones de esta etapa |

Los cambios de sola nomenclatura sobre archivos que ya habían subido versión en esta misma intervención y en la misma fecha no vuelven a subirla: la fila de control de cambios de esa versión ya cubre el trabajo del día. Los que sí suben son los que reciben un cambio semántico propio.

## 3. Resolución de las siete decisiones abiertas

| # | Decisión | Resolución | Resultado |
| --- | --- | --- | --- |
| **A-1** | Normalizar «D1-D8» a «D1-D9» | **Hecha** | 18 ocurrencias normativas en 7 archivos. Las 21 de las notas de coherencia ya emitidas se conservan |
| **A-2** | Mencionar `VER-XX` en `Rules-Calidad-Y-Pruebas.md` | **Hecha, y reencuadrada** | No era una mención faltante: era una contradicción. Ver §4 |
| **A-3** | Expresar los pisos de samples en cobertura | **Cerrada sin acción** | El criterio de aceptación de E2 ya exige que todo CU crítico tenga sonda o justificación. El piso de cantidad cubre otra cosa: la progresión didáctica. Mezclarlos degradaría ambos |
| **A-4** | Control de cambios propio en el `README.md` | **Cerrada sin acción** | El framework trata a los `README.md` como índices sin versión. Su historial vive en el `CHANGELOG.md` |
| **A-5** | Reponer la tabla consolidada de tiempo a primer éxito | **Cerrada sin acción** | La tabla no se perdió: `Rules-UX-UI-DX.md` la tiene en §4.2, §4.9 y §7, y es la categoría dueña de las métricas DX. Reponerla en 11 sería duplicar contenido entre categorías, el anti-patrón número uno de la guía de desarrollo |
| **A-6** | Despersonalizar el ejemplo aplicado de la guía de arranque | **Hecha** | 18 ocurrencias al placeholder `<Nombre-Solucion>`, más la descripción del dominio y los flujos de usuario en términos genéricos |
| **A-7** | Dos ejes más en la guía de desarrollo | **Hecha** | §III.8 agregar una regla transversal y §III.9 agregar un flag de gating. El fundamento de D8 se renumera a §III.10 |

Tres resoluciones son «cerrada sin acción». No es evasión: en los tres casos la verificación mostró que el problema que la decisión enunciaba no existía o ya estaba cubierto. A-5 es el caso más claro y el más instructivo, porque el informe la había registrado como pérdida y no lo era.

## 4. El hallazgo que motivó esta etapa

`Deriva-Rules.md` 1.1, escrita en E2, declara que un proyecto con categoría 10 emite matriz de sensado **aunque no ejecute la Fase B2**, y lo justifica: las sondas `VER-XX` no dependen de la maqueta, así que un proyecto sin interfaz visual también tiene contratos que pueden derivar.

`Rules-Calidad-Y-Pruebas.md` seguía diciendo lo contrario en dos lugares:

| Sección | Qué decía |
| --- | --- |
| §2.1 | `Matriz-Sensado-Deriva` obligatoria para «proyectos con `requiere_maqueta == true`»; **omitir para** «proyectos que no ejecutaron la Fase B2» |
| §6 | «**Si el proyecto ejecutó la Fase B2**, existe `Matriz-Sensado-Deriva-v1.0.md`…» |

**Por qué importa y no es una discrepancia cosmética.** La categoría 08 es la **dueña operativa** de la matriz: AG-08 es quien la incorpora a la estrategia de testing en la Fase E. Y AG-08 recibe un solo archivo de reglas, el suyo. Nunca lee `Deriva-Rules.md`. Con la contradicción en pie, un `worker-service` o una `library` seguían quedando sin matriz, que es exactamente lo que la extensión de E2 vino a corregir. La extensión existía en el papel y no se ejecutaba.

**Es el mismo error que E3 cometió y DEC-05 corrigió**: declarar algo de un solo lado, cuando cada subagente lee un solo archivo. La diferencia es que en E3 lo detecté al cerrar la etapa, y acá no. La corrección aplicada:

- §0 declara las **dos clases de sonda** con su origen distinto: las visuales, que emite AG-03M al cerrar la Fase B2 y miden parecido con lo aprobado; y las de contrato y comportamiento, que aporta la categoría 10 y miden si el sistema sigue haciendo lo especificado.
- §2.1 hace la matriz obligatoria también para proyectos con categoría 10, con la columna «Omitir para» reducida a proyectos sin Fase B2 **y** sin categoría 10.
- §6 separa el criterio en dos, uno por clase de sonda, y agrega un tercero: **no existe matriz sin filas**. Una matriz vacía es un proyecto sin instrumento de sensado, no un proyecto conforme.

**Registro de la falla de proceso.** La nota de coherencia de E2 verificó la comprobación 5, «sin contradicción con etapas anteriores», y la declaró cumplida. Era correcto en su literalidad —E2 no contradecía a E1— pero incompleta: no verificó que el archivo transversal no contradijera a la categoría que lo opera. El anti-patrón «declarar la frontera de un solo lado» que la guía de desarrollo documenta cubre este caso, y su ejemplo trabajado de §III.8 ahora lo usa.

## 5. Lista de comprobación

| # | Comprobación | Resultado | Evidencia |
| --- | --- | --- | --- |
| 1 | Invariantes D1–D9 intactas | Cumple | D1 español rioplatense técnico. D2 UTF-8 sin BOM, LF. D3 y D4 sin cambios de nombres. D5: los archivos con cambio semántico suben versión in situ. D6: control de cambios actualizado en cada uno. **D7 reforzada**: la despersonalización del ejemplo aplicado elimina el último literal de dominio de un artefacto vivo. D8: ocho valores intactos. D9: la corrección de §6 de la categoría 08 exige evidencia por fila de matriz |
| 2 | Autosuficiencia | Cumple | Las once cadenas en cero. Veintisiete URLs, todas preexistentes |
| 3 | Referencias internas | Cumple | Las anclas y los enlaces relativos de la guía de arranque y de la guía de desarrollo resuelven tras la renumeración y el cambio de título de §6 |
| 4 | Vocabulario normalizado | Cumple | Las adiciones respetan la terminología fijada |
| 5 | Sin contradicción con etapas anteriores | Cumple | Esta etapa existe precisamente para eliminar la que quedaba. Se verificó que `Rules-Calidad-Y-Pruebas.md` y `Deriva-Rules.md` ahora coinciden en las tres afirmaciones que hacen sobre la matriz |
| 6 | Control de cambios actualizado | Cumple | Fila nueva en cada archivo con cambio semántico. La entrada `[3.0]` del `CHANGELOG.md` absorbe las correcciones |
| 7 | El caso degenerado sigue produciendo el layout aplanado | Cumple | E8 no toca layout ni rutas |
| 8 | Nada fuera del alcance de la etapa fue modificado | Cumple con dos ampliaciones autorizadas | El cambio en `Rules-Calidad-Y-Pruebas.md` excede los cuatro casos que la restricción admite sobre 00-09, y la despersonalización de la guía de arranque excede lo que S7 pedía. Las dos fueron decididas explícitamente por el responsable |

## 6. Observaciones

1. **Queda una ocurrencia del nombre de la solución concreta**, en la fila 1.0 del control de cambios de `SDD-Getting-Started-Guide.md`. Es registro histórico: describe qué contenía la versión 1.0 de esa guía, y contenía ese ejemplo. Reescribirla haría que el changelog mienta. Es el mismo criterio aplicado a lo largo de toda la intervención y se declara acá para que un barrido futuro no lo lea como olvido.
2. **Veintiuna ocurrencias de «D1-D8» sobreviven en las cuatro notas de coherencia ya emitidas.** Verificaron contra el conjunto de ocho invariantes que estaba vigente cuando se emitieron. El `README.md` lo explica en su sección de invariantes, de modo que un lector que encuentre las dos formas entienda por qué coexisten y cuál rige.
3. **Dos archivos recibieron cambios de nomenclatura sin subir versión**: `Root-Rules.md` y `Rules-Necesidades-Negocio.md` ya habían subido versión en esta misma intervención y en la misma fecha, y la fila correspondiente cubre el trabajo del día. Subirlas de nuevo produciría dos filas consecutivas con la misma fecha describiendo el mismo esfuerzo. `Design-Rules-Blazor-Mudblazor-v1.0.md` recibió una sola sustitución de nomenclatura y no tiene cambio semántico.
4. **La guía de desarrollo pasó de siete a nueve ejes de extensión.** El ejemplo trabajado del eje nuevo §III.8 es, deliberadamente, la contradicción que esta etapa corrigió: es el mejor caso disponible de un mecanismo transversal cuyo cambio no se propagó a la categoría que lo opera.
5. **La segmentación original tenía ocho etapas, E0 a E7.** Esta es la novena. No estaba prevista y su existencia es consecuencia de que el informe de cierre hiciera su trabajo: registrar lo que quedaba abierto en lugar de darlo por cerrado.

## 7. Veredicto

**CONFORME.**

Las siete decisiones abiertas quedaron resueltas: cuatro con acción y tres cerradas sin acción tras verificar que el problema enunciado no existía o ya estaba cubierto. La contradicción entre la categoría 08 y la regla de sensado —el hallazgo real detrás de A-2, que ni la nota de E2 ni el informe habían identificado como tal— quedó corregida en las tres afirmaciones que la producían.

Las cinco observaciones son cuatro decisiones de criterio declaradas y una constatación sobre el proceso. Ninguna deja trabajo pendiente.

**No quedan decisiones abiertas.**

## 8. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.0 | 2026-07-26 | Nota de coherencia de la etapa E8, no prevista en la segmentación original: resolución de las siete decisiones abiertas del informe, corrección de la contradicción entre `Rules-Calidad-Y-Pruebas.md` y `Deriva-Rules.md` con el registro de la falla de proceso que la dejó pasar, normalización de nomenclatura de invariantes, despersonalización del ejemplo aplicado, dos ejes de extensión nuevos y dos referencias muertas corregidas. Lista de comprobación de ocho puntos, cinco observaciones y veredicto CONFORME. | Reformulación SDD |
