# Nota de coherencia — E7 Cierre de la intervención

**Proyecto:** Framework SDD
**Documento:** Nota-Coherencia-E7.md
**Versión:** 1.0
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD

---

> **Nota posterior (E8, 2026-07-26).** Las siete decisiones abiertas que el informe registraba se resolvieron en **E8**, etapa abierta a tal efecto: cuatro con acción y tres cerradas sin acción. Ver `Nota-Coherencia-E8.md` y §7 del informe, que quedó en versión 1.1. **No quedan decisiones pendientes.**

## 1. Alcance

Verificación de implantación de la etapa E7, la última. Ejecuta dos solicitudes:

- **S7** — actualizar la guía de usuario para reflejar el estado nuevo: renumeración de categorías, Fases I y J, el paso de usuario del ciclo incremental, la reformulación de §4.7, las referencias de los casos aplicados, seis entradas de FAQ nuevas y el mapa de carpetas.
- **S11** — emitir el informe de la intervención con el inventario de archivos, los tres reportes de verificación, las inconsistencias detectadas y las decisiones abiertas.

Se agrega la entrada del `CHANGELOG.md`, que la titularidad de E0 asignó a esta etapa y que ninguna solicitud nombra explícitamente pero el framework exige por su propia práctica.

## 2. Inventario de archivos

| Archivo | Versión | Cambio |
| --- | --- | --- |
| `SDD/Guides/SDD-User-Guide.md` | 1.4 → 1.5 | S7 completo |
| `CHANGELOG.md` | 2.5 → **3.0** | Entrada de la intervención |
| `Informe-Intervencion-v1.0.md` | Nuevo, 1.0 | S11 |

Los dos primeros viven en `/IA/IA.SDD/`; el informe, en la carpeta de este prompt, según el alcance de escritura declarado.

### 2.1 Detalle de `SDD-User-Guide.md`

| Sección | Cambio |
| --- | --- |
| §4 listado de fases | La Fase F queda solo con 09; la G produce la pasada de diseño de 10; se agregan H con el plan documental, el Paso 6 de handoff, y las Fases I y J. Se retiró la fila duplicada de la Fase H previa |
| §4.7 | Deja de afirmar que tras el handoff se sale del alcance de SDD. Ahora declara que el handoff cierra el **tramo de especificación** y remite al paso 7 |
| §4.8 (nueva) | El paso 7 del usuario: cuándo corre la Fase I, su precondición dura, los seis pasos de cada corrida, qué le toca al usuario, qué pasa en la Fase J y por qué sus correcciones manuales no se pisan |
| §5 casos aplicados | Corregida la referencia del caso móvil, que declaraba la categoría 10 opcional cuando ahora la 11 existe siempre y lo opcional es su cuerpo integrador |
| §6 FAQ | Seis entradas nuevas, F-24 a F-29 |
| §10 mapa de carpetas | `Solucion/11-Documentacion/` con sus seis artefactos, la nota sobre contratos de verificación en `10-Examples/`, `AGENTS.md` en la raíz del repositorio con su explicación, y `samples/` materializado desde 10 |
| Tabla de contenido | Entrada del §4.8 |

Las seis entradas de FAQ cubren exactamente lo que S7 pide como mínimo: por qué se intercambiaron las categorías (F-24), cuándo corre la Fase I y con qué cadencia (F-25), qué pasa si se quiere saltear la documentación incremental (F-26), cómo se relaciona `AGENTS.md` con el resto del cuerpo (F-27), cómo se corre un ensayo de entrega y qué hacer cuando no se completa (F-28), y cómo se registra y se triaja una eventualidad (F-29).

**F-29 desarrolla el caso del dispositivo USB de punta a punta**, como S7 exige: síntoma observable, diagnóstico, intentos descartados, resolución y triaje hacia sus dos destinos permanentes, `Guia-Contenedor` sección de dispositivos del host y `Runbook-Operacion` entrada `OPS-07`.

### 2.2 Detalle del `CHANGELOG.md`

Entrada `[3.0] - 2026-07-26`, con secciones Añadido, Cambiado, Corregido e **Impacto sobre documentación ya emitida**. Sube major porque cambia el alcance y el gating de dos categorías y porque la documentación generada con la numeración anterior deja de cumplir.

La sección de impacto declara explícitamente que la documentación existente **no se regenera automáticamente**: una solución previa conserva sus carpetas `10-Developer-Guide/` y `11-Examples/` hasta que se ejecute una regeneración parcial. Es la exigencia que la propia guía de desarrollo escrita en E6 impone para todo bump major.

## 3. Lista de comprobación

| # | Comprobación | Resultado | Evidencia |
| --- | --- | --- | --- |
| 1 | Invariantes D1–D9 intactas | Cumple | D1 español rioplatense técnico. D2 UTF-8 sin BOM, LF. D3 y D4 sin cambios de nombres. D5: los dos archivos del framework suben versión in situ. D6: control de cambios actualizado en ambos. D7: el caso del dispositivo USB se narra sin literal de dominio; el dispositivo se nombra genéricamente. D8: ocho valores intactos. D9: el informe distingue explícitamente hechos verificados de interpretaciones, y cada conteo se midió y no se estimó |
| 2 | Autosuficiencia: cero referencias fuera de `/IA/IA.SDD/` | Cumple | Las once cadenas dan **cero**. Veintisiete URLs distintas, todas preexistentes; cero introducidas |
| 3 | Referencias internas: todo lo citado existe | Cumple | Las referencias nuevas de la guía de usuario apuntan a artefactos que `Rules-Documentacion.md` define. La entrada de changelog nombra archivos y versiones que existen |
| 4 | Vocabulario normalizado | Cumple | Las adiciones usan «integrador», «mantenedor», «operador» y «rol de intervención» |
| 5 | Sin contradicción con etapas anteriores | Cumple | La cadencia de F-25 coincide con `Rules-Documentacion.md` §0.4; la precondición de §4.8 con `Master-Prompt.md` §7.1; el triaje de F-29 con §0.6; el tratamiento de `AGENTS.md` de F-27 con §2.1 y con `Master-Prompt.md` §3.5 |
| 6 | Control de cambios actualizado | Cumple | Fila 1.5 en la guía de usuario, entrada 3.0 en el changelog, control de cambios propio en el informe |
| 7 | El caso degenerado sigue produciendo el layout aplanado | Cumple | La nota del mapa de carpetas de §10 lo conserva intacta |
| 8 | Nada fuera del alcance de la etapa fue modificado | Cumple | Dos archivos del framework más el informe |

## 4. Verificación de cierre de la intervención completa

Los tres reportes que S11 exige están en el informe, medidos y no estimados.

| Reporte | Resultado |
| --- | --- |
| Barrido de propagación (S1.5) | Cero ocurrencias de las tres cadenas obligatorias en archivos vivos. Lo restante está en material congelado por DEC-01 o en filas de control de cambios, con su motivo declarado |
| Normalización de vocabulario (S1.7) | 60 sustituciones de «consumidor», 19 de «audiencia», 1 de «constructor», 0 de «implementador» por DEC-04. Las conservadas tienen su motivo por término |
| Autosuficiencia | Las once cadenas en **cero**, por primera vez en la historia del repositorio. `PROMPTs/` pasó de 12 a 0. Cero URLs externas introducidas |

Verificación adicional sobre el árbol completo: los ocho valores D8 intactos, veintisiete URLs distintas sin variación respecto de la línea de base, y **cero commits nuevos** — los cambios quedan en el working tree como la restricción exige.

## 5. Observaciones

1. **La entrada del `CHANGELOG.md` no la pide ninguna solicitud.** S11 enumera lo que el informe debe contener y no menciona el changelog del repositorio. La agregué porque el framework mantiene ese archivo por práctica propia desde su versión 1.0, y una intervención que sube dos archivos de reglas a major sin dejar rastro ahí sería la única del historial en no hacerlo. Además, la guía de desarrollo escrita en E6 declara que todo bump major se anota ahí con su impacto. Se reporta por si el responsable considera que excede el alcance.
2. **El listado de fases de §4 tenía una fila duplicada de la Fase H.** Al insertar las fases nuevas quedó la descripción vieja además de la nueva. Se retiró la vieja. Es una corrección menor de la propia etapa, no un hallazgo preexistente.
3. **F-26 responde algo distinto de lo que la pregunta sugiere.** «¿Qué pasa si quiero saltearme la documentación incremental?» invita a una respuesta administrativa —vas a tener que escribirla después—, y esa no es la razón de fondo. La entrada desarrolla la razón real: la actualización incremental funciona como instrumento de control del diseño, y saltearla no posterga trabajo, posterga señales. Se declara porque es una decisión editorial, no una transcripción de la solicitud.
4. **El informe registra doce inconsistencias y siete decisiones abiertas.** Ninguna bloquea el uso del framework. Cuatro de las siete decisiones tienen recomendación de «sí, hacerlo», y la más clara es normalizar «D1-D8» a «D1-D9» en todo el árbol, que es un barrido mecánico de bajo riesgo.
5. **`SDD-Development-Guide.md` sigue untracked.** Existía vacío y nunca commiteado; la intervención le dio contenido pero no commitea nada. Quien revise no debe confundirlo con un archivo generado por error.

## 6. Veredicto

**CONFORME.**

La guía de usuario refleja el estado nuevo en las cinco secciones que S7 nombra, con el paso 7 del usuario documentado, el handoff reformulado como cierre del tramo de especificación y seis entradas de FAQ que cubren las seis preguntas mínimas, incluida la eventualidad del dispositivo USB desarrollada de punta a punta. El informe de intervención contiene los tres reportes de verificación medidos, el inventario de veinticinco archivos con su cambio de versión, doce inconsistencias separadas en hechos e interpretaciones, y siete decisiones abiertas con recomendación.

**La intervención queda cerrada.** Las ocho etapas cumplieron su criterio de terminación y ninguna quedó con veredicto NO CONFORME.

## 7. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.0 | 2026-07-26 | Nota de coherencia inicial y final de la etapa E7: inventario de los tres archivos, detalle de las siete secciones tocadas en la guía de usuario y de la entrada de changelog, lista de comprobación de ocho puntos con verificación D1–D9, verificación de cierre de la intervención completa con los tres reportes, cinco observaciones y veredicto CONFORME. | Reformulación SDD |
