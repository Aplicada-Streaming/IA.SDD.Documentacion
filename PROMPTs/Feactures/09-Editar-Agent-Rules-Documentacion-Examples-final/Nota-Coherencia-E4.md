# Nota de coherencia — E4 Documentación viva en el orquestador

**Proyecto:** Framework SDD
**Documento:** Nota-Coherencia-E4.md
**Versión:** 1.0
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD

---

> **Nota posterior (E8, 2026-07-26).** Todas las decisiones que esta nota menciona quedaron cerradas. Ver `Nota-Coherencia-E8.md` y §7 del informe de intervención.

## 1. Alcance

Verificación de implantación de la etapa E4, que cablea en el orquestador el modelo de documentación viva definido en E3, y ancla su cadencia a la Definition of Done del sprint.

El reparto acordado en **DEC-03** se respetó: la mecánica de los tres momentos, la cadencia, el ensayo de entrega y la bitácora viven en `Rules-Documentacion.md`, escritas en E3. E4 no las redefine; las invoca por referencia y declara cuándo se ejecutan, con qué precondición y bajo qué criterios de auditoría. Es el principio de delegación de la especialidad que el propio master-prompt declara en §1.

## 2. Inventario de archivos

| Archivo | Versión | Cambio |
| --- | --- | --- |
| `SDD/Devs/Orchestrator/Master-Prompt.md` | 3.5 → 3.6 | Fases I y J, precondición dura, criterio de re-ejecución, criterios de audit, glosario |
| `SDD/Devs/Rules/Rules-Plan-Sprint.md` | 1.2 → 1.3 | La categoría 11 dentro de la Definition of Done |

Dos archivos. Ninguna creación, ningún renombre, ninguna eliminación.

### 2.1 Detalle de `Master-Prompt.md`

| Sección | Cambio |
| --- | --- |
| §0 Salida | Suma `/samples/` y `AGENTS.md` en la raíz del repositorio destino a la salida declarada del orquestador |
| §3.5 Layout | Declara `Solucion/11-Documentacion/`; la categoría 11 pasa de «según `project_type` y flags» a «siempre; qué cuerpos se materializan depende del `project_type`»; declara `AGENTS.md` como única salida fuera de `SDD/`, con su razón funcional |
| §6 Plan de generación | Reordena las filas: **F** queda solo con 09-Devops; **G** produce la pasada de diseño de 10-Examples con sus contratos de verificación; **H** suma el plan documental de 11 (Momento 1); se agregan **tres filas nuevas** para I (10 pasada de ejecución, 11 actualización incremental) y J (11 consolidación). El párrafo introductorio aclara que la categoría 11 no sigue el patrón de las demás |
| §7 Ejecución por fases | Reescribe el tramo F a J. Introduce el corte explícito del handoff y la distinción entre los dos tramos: antes del handoff se especifica, después se documenta contra hechos verificables |
| §7.1 (nueva) | Precondición dura de la Fase I, con sus tres condiciones y su método de verificación |
| §7.2 (nueva) | Criterio de re-ejecución de la Fase I: qué se regenera, qué se preserva y cómo se tratan las correcciones manuales del usuario |
| §10 Auditoría | Criterios propios de las Fases G, I y J. Diez hallazgos P0 enumerados. Path de informe diferenciado por incremento. Declara que el ensayo de entrega se registra en el informe de audit de su fase |
| §12 Handoff | Suma el plan documental y los contratos de verificación pendientes al resumen ejecutivo. **Declara que el handoff cierra el tramo de especificación, no el alcance de SDD** |
| §15 Glosario | Nueve términos nuevos: Fase I, Fase J, documentación viva, momento, contrato de verificación, ensayo de entrega, eventualidad, rol de intervención, `AGENTS.md` |

### 2.2 Detalle de `Rules-Plan-Sprint.md`

Nueva §4.5, «La categoría 11 dentro de la Definition of Done», con la condición de cierre, las tres exigencias prácticas y el racional de por qué vive en la DoD y no en una lista de buenas prácticas. La renumeración corrió §4.5 a §4.7 previas hacia §4.6 a §4.8.

Además, §4.2 punto 5 enuncia la condición dentro de la estructura del plan de sprint y el punto 7 la incorpora a los criterios de hecho; §4.8 suma el anti-patrón de cerrar un sprint con documentos de 11 sin revisar; §5 suma una pregunta guía y §6 un criterio de aceptación.

Es la única modificación que esta intervención introduce en la regla, tal como la restricción del alcance exige. No se agregan artefactos ni carga narrativa.

## 3. El orden de fases resultante

```
Fase validación de intake        → sin cambios
Fase A  (solución)               → sin cambios
Fase B / B2 / C / D / E          → sin cambios
Fase F  (por proyecto)           → 09-Devops + audit F
Fase G  (por proyecto)           → 10-Examples pasada de diseño + alta de sondas VER-XX + audit G
Fase H  (consolidación)          → vista de solución + pipeline + README raíz
                                   + plan documental de 11 (Momento 1) + audit final
Paso 6  (humano)                 → handoff a codificación
─────────────────── a partir de acá el sistema se construye ───────────────────
Fase I  (por incremento, re-ejecutable) → precondición dura §7.1
                                          + 10 pasada de ejecución
                                          + 11 actualización incremental (Momento 2)
                                          + triaje de la bitácora
                                          + AGENTS.md emitido o refrescado
                                          + ensayo automatizado
                                          + actualización de la matriz de sensado
                                          + audit I acotado al incremento
Fase J  (una vez, al cierre)            → 11 consolidación (Momento 3)
                                          + AGENTS.md definitivo
                                          + ensayo humano (gate) + audit final de entrega
```

Coincide con lo que S4.4 pide. Las diferencias respecto del esquema del prompt son dos, ambas de detalle operativo y ninguna de fondo: la Fase G incorpora el alta de las sondas `VER-XX` en la matriz de sensado, que es consecuencia directa de lo que E2 definió; y la Fase I incorpora el triaje de la bitácora y la actualización de la matriz, que son mecanismos que E3 declaró y que necesitaban un momento de ejecución declarado.

**El estado en que E1 había dejado el orden queda superado.** E1 dejó F produciendo 09 y 10, y G produciendo 11, que era el estado consistente mínimo tras el intercambio. E4 lo lleva al estado definitivo, tal como la observación 1 de la nota de E1 anticipaba.

## 4. Lista de comprobación

| # | Comprobación | Resultado | Evidencia |
| --- | --- | --- | --- |
| 1 | Invariantes D1–D9 intactas | Cumple | D1 español rioplatense técnico. D2 UTF-8 sin BOM, LF, sin saltos triples de línea. D3 y D4 sin cambios de nombres de archivo. D5: ambos archivos suben versión in situ. D6: control de cambios actualizado en los dos. D7: ningún literal de dominio introducido. D8: ocho valores en `Master-Prompt.md`, sin alterar la tabla de adaptabilidad. **D9**: la precondición dura de §7.1 es una aplicación directa de D9 —sin sistema no hay evidencia del estado del sistema—, y tres de los diez hallazgos P0 de las Fases I y J la operacionalizan |
| 2 | Autosuficiencia: cero referencias fuera de `/IA/IA.SDD/` | Cumple | Las once cadenas dan cero. Cero URLs nuevas fuera de placeholders de localhost |
| 3 | Referencias internas: todo lo citado existe | Cumple | Las veintidós referencias cruzadas de `Master-Prompt.md` a secciones de archivos de reglas resuelven contra una sección real, incluidas las nueve que introdujo esta etapa. La única de `Rules-Plan-Sprint.md` también. Las referencias `§7.1` y `§7.2` internas resuelven |
| 4 | Vocabulario normalizado | Cumple | Las adiciones usan «integrador», «mantenedor», «operador» y «rol de intervención». «Agente» calificado |
| 5 | Sin contradicción con etapas anteriores | Cumple | La pasada de diseño y la pasada de ejecución se nombran igual que en `Rules-Examples.md` §0.2. Los tres momentos, la cadencia, el ensayo y la bitácora se invocan por referencia a `Rules-Documentacion.md`, sin redefinirse. El gating «la categoría 11 existe siempre» de §3.5 coincide con `Rules-Documentacion.md` §2.5 |
| 6 | Control de cambios actualizado | Cumple | Una fila por archivo, fechada 2026-07-26, sin citar el prompt de origen ni su repositorio |
| 7 | El caso degenerado sigue produciendo el layout aplanado | Cumple | §3.5 conserva intacto su párrafo de caso degenerado. Los artefactos de nivel solución de la categoría 11 colapsan a `SDD/Docs/11-Documentacion/`, según lo que `Rules-Documentacion.md` §2.1 ya declaraba |
| 8 | Nada fuera del alcance de la etapa fue modificado | Cumple con una corrección declarada | Dos archivos. La corrección es la referencia genérica a la sección de anti-patrones; ver observación 1 |

Verificación adicional de formato: marcas de bloque de código balanceadas en ambos archivos, cero saltos triples de línea, fases A a E sin una sola línea modificada.

## 5. Observaciones

1. **Corregí una referencia preexistente incorrecta, y con eso queda resuelta una decisión que E3 había dejado abierta.** `Master-Prompt.md` §8 y §10 citaban la sección de anti-patrones de los archivos de reglas como «§4.5». Verifiqué la numeración real en los trece archivos: va de §4.4 a §4.10, y solo siete coincidían con §4.5. La discrepancia es **preexistente** —`Rules-Calidad-Y-Pruebas.md` estaba en §4.10 y `Rules-UX-UI-DX.md` en §4.4 desde antes de esta intervención—, así que no la introdujo E3 al ubicar los anti-patrones de la categoría 11 en §4.9. La corrección aplicada es ubicar la sección **por su título y no por su numeración**, que es lo robusto. Con eso, la decisión abierta que la nota de E3 elevaba —si convenía forzar §4.5 en todos los archivos— deja de ser necesaria: el orquestador ya no depende del número.
2. **La Fase I no tiene cantidad de corridas declarada, a propósito.** Es una por incremento, y el framework no fija cuántos incrementos tiene un proyecto. La consecuencia práctica es que el path de los informes de audit necesita distinguirlas, y por eso §10 declara el patrón `I-<incremento>-...`. Queda a cargo del equipo definir qué numera `<incremento>`: sprint, release o hito.
3. **`Rules-Plan-Sprint.md` sufrió una renumeración de tres subsecciones.** La condición de DoD entró como §4.5 y corrió §4.5 a §4.7 hacia §4.6 a §4.8. Verifiqué que ningún archivo del framework referenciaba esas secciones por número: las únicas referencias eran internas al propio archivo, y quedaron actualizadas. Lo registro porque una renumeración silenciosa es exactamente la clase de cambio que rompe referencias meses después.
4. **El ensayo de entrega no produce artefacto propio.** Se registra en el informe de audit de su fase, en `SDD/Docs/Audit/`, reutilizando la maquinaria existente. Fue una decisión de diseño: un ensayo es una verificación, y las verificaciones ya tienen dónde vivir. La alternativa —un artefacto de ensayo por corte— habría agregado un tipo de documento nuevo sin necesidad.
5. **`AGENTS.md` es la primera salida del orquestador fuera de `SDD/`.** Hasta ahora todo lo que el orquestador escribía en el repositorio destino vivía bajo `SDD/Docs/` o `SDD/Maquetas/`. Quedó declarado explícitamente en §0 y en §3.5 para que no se lea como un descuido. Es coherente con el precedente que E3 ya había registrado sobre D3 y D4.

## 6. Veredicto

**CONFORME.**

El orquestador refleja el modelo de documentación viva completo: las Fases I y J con su contenido, su precondición dura, su criterio de re-ejecución y sus diez hallazgos P0; el plan documental incorporado a la Fase H; el reordenamiento de F y G que separa DevOps de la pasada de diseño de los ejemplos; y el handoff redefinido como cierre del tramo de especificación en lugar de cierre del alcance. La cadencia quedó anclada a la Definition of Done del sprint con una sola modificación en `Rules-Plan-Sprint.md`, sin agregarle artefactos ni carga narrativa.

Las cinco observaciones son una corrección declarada que además cierra una decisión abierta de E3, un parámetro que queda a criterio del equipo y tres decisiones de diseño que conviene dejar registradas. Ninguna bloquea.

Corresponde detenerse y esperar confirmación humana antes de arrancar E5.

## 7. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.0 | 2026-07-26 | Nota de coherencia inicial de la etapa E4: inventario de los dos archivos afectados con detalle por sección, orden de fases resultante contrastado contra S4.4, lista de comprobación de ocho puntos con verificación D1–D9, cinco observaciones y veredicto CONFORME. | Reformulación SDD |
