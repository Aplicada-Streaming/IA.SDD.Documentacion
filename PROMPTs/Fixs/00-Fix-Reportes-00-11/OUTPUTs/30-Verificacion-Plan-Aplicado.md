# Verificación: lo aplicado contra lo planificado

**Documento:** 30-Verificacion-Plan-Aplicado.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Verifica:** `20-Plan-De-Aplicacion-Unificado.md` contra el árbol de `IA/SDD/IA.SDD/`
**Estado:** Vigente

Los tres resultados posibles son **aplicado**, **aplicado con desviación declarada** y **no aplicado
con motivo**. Un ítem del plan sin fila acá sería un defecto de esta verificación, no un ítem menor:
el plan tiene 36 ítems y esta tabla tiene 36 filas.

---

## 1. Resultado por ítem

| Id | Qué pedía el plan | Resultado | Comprobación |
|---|---|---|---|
| P-01 | `Root-Rules.md` §10 nueva: sistema de identificadores | **Aplicado con desviación declarada** | Quedó como **§9**, no §10. Ver §2.1 |
| P-02 | `Root-Rules.md` §11: datos derivados | **Aplicado con desviación declarada** | Quedó como §10. Ver §2.1 |
| P-03 | `Root-Rules.md` §12: apartamiento declarado | **Aplicado con desviación declarada** | Quedó como §11. Ver §2.1 |
| P-04 | `Root-Rules.md` §13: referencia pendiente | **Aplicado con desviación declarada** | Quedó como §12. Ver §2.1 |
| P-05 | `README.md` D3: ancho y remisión | **Aplicado** | D3 declara cinco dígitos, ámbito producto y remite a `Root-Rules.md` §9 |
| P-06 | `Master-Prompt.md` §8: las transversales en todo despacho | **Aplicado** | Bloque de insumos y regla de construcción, junto a la de `Vocabulario-Rules.md` |
| P-07 | `Master-Prompt.md` §3.4: mapa de rangos | **Aplicado** | Bloque con ámbito, ancho, rangos por proyecto de código y rango de nivel producto |
| P-08 | `Master-Prompt.md` §10: compuerta, corte, marcas, cuatro criterios | **Aplicado** | §10.0 con cuatro comprobaciones y la obligación de declarar qué no mira; §10.1 con el corte; marcas de origen y de detectabilidad; criterios de conjuntos cerrados (P0), recuentos anclados, referencias pendientes y apartamientos |
| P-09 | `Master-Prompt.md` §7 y §12: decisiones pendientes | **Aplicado** | §7.0 crea el registro y la detención por arbitraje; §12 lo lee en lugar de reconstruirlo |
| P-10 | `Master-Prompt.md` §6: reapertura con insumo | **Aplicado** | Grafo de obligaciones, referencia pendiente, reapertura con insumo y prohibición de emitir por otra categoría |
| P-11 | `Master-Prompt.md` §15: cuatro términos | **Aplicado** | `Sonda`, `Pasada de diseño`, `Pasada de ejecución`, `Arnés` |
| P-12 | `PRODUCT-INTAKE-template.md` §20: transcripción fiel | **Aplicado** | Tres obligaciones, bloque de conteo y dos anti-patrones |
| P-13 | `Intake-Rules.md` §5 y §7: coherencia intra-escenario | **Aplicado** | Con su alcance acotado a conteos del propio payload y sus dos niveles de bloqueo |
| P-14 | `Maqueta-Rules.md` §3.5 y §3.6 | **Aplicado** | Propagación por iteración, regla de escape, fila nueva, manifiesto en la regla de corte y distinción propagar/contradecir |
| P-15 | `Rules-Especificacion-Funcional.md` §3.2: códigos de error | **Aplicado** | Forma `E-<DOMINIO>-NNNNN`, ámbito producto, asignador declarado, más `FA-NN` |
| P-16 | `Rules-Especificacion-Funcional.md` §4.2: conjuntos cerrados | **Aplicado** | Marcado obligatorio y remisión a la detención de §7.0 |
| P-17 | Criterio de glosario en las dos reglas que no lo tenían | **Aplicado** | `Rules-Especificacion-Funcional.md` y `Rules-UX-UI-DX.md` |
| P-18 | `Rules-Arquitectura-Tecnica.md`: condicionar sobre el proyecto | **Aplicado** | §2.1, §2.2, §6 y §7 leen `tiene_persistencia`; la columna por tipo remite al flag |
| P-19 | `Rules-Examples.md`: gating por `redistribuible` | **Aplicado** | §0 con tabla de condiciones, §2.1 y §2.2 con la cláusula ya no en prosa, y válvula al piso de samples |
| P-20 | `Rules-Examples.md` §4.6: trazabilidad recíproca | **Aplicado** | Declaración falsable, bloque `discrimina` y los tres ingredientes del defecto |
| P-21 | `Rules-Calidad-Y-Pruebas.md` §6: fuente única | **Aplicado** | Sube del prompt-snippet §8 a criterio, con alcance ampliado a cualquier artefacto |
| P-22 | `Rules-Calidad-Y-Pruebas.md` §6: retirar el noveno destino | **Aplicado** | `sonda` y `umbral` se citan del glosario operativo; `nivel` y `fixture` quedan en la categoría |
| P-23 | `Rules-Contexto.md` §4.2 y §6 | **Aplicado** | El acuerdo de equipo referencia con la forma de §12 y no enumera |
| P-24 | `Rules-Plan-Sprint.md`: artefactos del equipo al producto | **Aplicado** | Columna de nivel, cuatro artefactos a `SDD/Docs/Producto/07-Plan-Sprint/`, numeración del roadmap y criterios reformulados |
| P-25 | `Vocabulario-Rules.md` §4 R3: nivel por artefacto | **Aplicado** | Con la señal de detección para el resto del framework |
| P-26 | Columna de nivel en las doce tablas maestras | **Aplicado con desviación declarada** | Ver §2.2 |
| P-27 | `Vocabulario-Rules.md` §8: alcance declarado | **Aplicado** | Gobierna los términos que colisionan con el dominio; el resto va al glosario operativo |
| P-28 | Primera cláusula del glosario en las once reglas | **Aplicado y ampliado** | Está en **catorce** archivos: las trece reglas de categoría más `Vocabulario-Rules.md` |
| P-29 | Clasificación de criterios en las diecisiete reglas | **Aplicado** | 17 de 17. 85 enumerables y 235 interpretativos, más 30 marcados a mano al escribirlos |
| P-30 | `Deriva-Rules.md` §2.1 y §2.3 | **Aplicado** | Ancho remitido a §9, estabilidad y capacidad juntas, matriz declarada colección derivada |
| P-31 | Barrido de notación a cinco dígitos | **Aplicado** | 336 patrones y 177 identificadores de ejemplo, cero residuos. Exclusiones conservadas |
| P-32 | `Migracion-Rules.md`: renumeración | **Aplicado** | §4.3.1 con las dos pasadas, el árbol de migración y las tres comprobaciones bloqueantes |
| P-33 | Guías de cara al usuario | **Aplicado** | `SDD-User-Guide.md` 1.10: cinco entradas de glosario y las preguntas F-30 y F-31 |
| P-34 | Regla de redacción de criterios | **Aplicado** | `SDD-Development-Guide.md` 1.7, Parte IV |
| P-35 | Nota de coherencia | **Aplicado** | `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con el patrón de `Coherencia-Auditoria-Marco.md` |
| P-36 | `CHANGELOG.md` y `_legacy/` | **Aplicado** | Entrada `[7.0]` con bloque de impacto sobre destinos existentes; `_legacy/6.0/` con 63 archivos |

**Resumen:** 31 aplicados, 5 aplicados con desviación declarada, 0 no aplicados.

## 2. Las desviaciones, con su motivo

### 2.1 P-01 a P-04 · Numeración de las secciones transversales

El plan las ubicaba como §10 a §13 de `Root-Rules.md`. Quedaron como **§9 a §12**, y el control de
cambios pasó de §9 a §13.

**Motivo.** `Root-Rules.md` terminaba en §9 Control de cambios. Ubicar las secciones nuevas después
habría dejado material normativo detrás del control de cambios, que es el cierre convencional de todo
archivo de reglas del framework. Se verificó antes de mover que **ninguna referencia externa cita
secciones de `Root-Rules.md`**, de modo que el corrimiento no rompe ninguna cita.

**Alcance de la desviación:** cambia el número, no el contenido ni el orden relativo. Todas las
referencias emitidas en esta intervención apuntan a la numeración final.

### 2.2 P-26 · Columna de nivel en las tablas maestras

El plan pedía agregar la columna **Nivel** a la tabla maestra §2.1 de las doce reglas de categoría.
Se aplicó de otra forma: `Vocabulario-Rules.md` §4 R3 exige la columna **solo cuando una categoría
contiene artefactos de más de un nivel**, y en las demás la declaración de la cabecera gobierna a
todos sus artefactos. Hoy la única que la lleva es `Rules-Plan-Sprint.md`.

**Motivo.** En once de las doce categorías la columna repetiría el mismo valor en todas sus filas.
Una columna de valores idénticos no agrega información y sí agrega superficie de desincronización:
es exactamente el dato derivado que `Root-Rules.md` §10 R1 manda **no escribir**. Aplicar el plan a
la letra habría hecho que la intervención cometiera el defecto que corrige.

**Qué se conserva del objetivo original.** La obligación de declarar el nivel por artefacto, que es
lo que el reporte `08` pedía y lo que habilita la intervención estructural posterior. Lo que cambia
es cuándo hace falta materializarla en una columna.

## 3. Comprobaciones mecánicas

Cada una es reproducible desde la raíz de `IA/SDD/IA.SDD/`.

| Qué se comprueba | Comando | Resultado |
|---|---|---|
| Las cuatro secciones transversales existen | `grep -c "^## 9\. Sistema de identificadores\|^## 10\. Datos derivados\|^## 11\. Apartamiento\|^## 12\. Referencia pendiente" SDD/Devs/Rules/Root-Rules.md` | 4 |
| Ningún residuo de la notación vieja | `grep -roE "\b(NB\|CU\|RN\|ADR\|US\|BT\|...)-([0-9]{2}\|XX)\b" SDD/Devs SDD/Guides README.md` | 0, fuera de las exclusiones |
| Las exclusiones se conservaron | `grep -c "AG-XX"` y el ordinal de iteración | 17 y 20 |
| Ninguna mención vigente a «dos dígitos» | `grep -rn "dos dígitos"` excluyendo `_legacy/`, `Bootstrap/` y las filas históricas de control de cambios | 0 |
| Las diecisiete reglas clasifican sus criterios | `grep -l "\[enumerable\]" SDD/Devs/Rules/*.md \| wc -l` | 17 |
| La política del vocabulario del método está en todas | `grep -l "glosario operativo de \`Master-Prompt.md\` §15" SDD/Devs/Rules/*.md \| wc -l` | 14 |
| Las tablas de control de cambios son consistentes | Comparación de columnas entre la cabecera y la fila `2026-08-15` de cada regla | Sin discrepancias |
| El conjunto superado está archivado | `find _legacy/6.0 -type f \| wc -l` | 63 |

## 4. Dos defectos introducidos durante la aplicación

Se registran porque los encontró una comprobación mecánica y no una lectura, y porque son la clase de
defecto que el reporte `04` documenta: una corrección aplicada a medias.

| Defecto | Cómo se detectó | Reparación |
|---|---|---|
| Trece filas de control de cambios quedaron con una columna de más: el script supuso cuatro columnas y esas tablas tienen tres | Comparación automática de columnas entre la cabecera de cada tabla y su fila nueva | Se retiró la celda sobrante en los trece archivos |
| Una fila de `Root-Rules.md` recibió su texto en la columna de fecha | La misma comparación | Se recompuso la fila con el texto en la columna de cambios |

Ninguno de los dos llegó al estado final. Los dos habrían pasado desapercibidos a una lectura, que es
el argumento de la compuerta mecánica que esta misma intervención incorpora.

## 5. Lo que el plan declaró que no haría, y no se hizo

Verificado por ausencia, para que no se lea como omisión:

**Actualización del 2026-08-15.** Tres de estos pendientes se cerraron después de emitir esta
verificación, dentro de la misma sesión: el grafo de obligaciones se corrió y su tratamiento se
incorporó a las filas de control de cambios de siete reglas y a la entrada `[7.0]` del `CHANGELOG.md`;
el inventario de vocabulario se corrió y sus nueve términos entraron al glosario operativo; y el
glosario de categoría quedó definido. El detalle está en `42-Verificacion-8.0.md` §4.

| Pendiente declarado | Estado |
|---|---|
| Correr la comprobación del grafo de obligaciones sobre las diecisiete reglas | No hecho. El **mecanismo** está aplicado en `Master-Prompt.md` §6; la corrida completa es trabajo siguiente |
| Inventario completo del vocabulario propio del framework | No hecho. Los cuatro términos que la corrida encontró están incorporados |
| Adelantar la condición de terminado a la Fase A | No hecho. Se administra con la referencia pendiente |
| Decidir si el «glosario de categoría» es un artefacto real | No hecho |
| Declarar que un recuento en prosa es afirmación bajo D9 | No hecho. `Root-Rules.md` §10 consigue el efecto sin ampliar la invariante |
| Reprocesar los recuentos de la documentación ya emitida | No hecho, por la misma razón por la que D9 no es retroactiva |

## 6. Fuera del alcance de esta verificación

`15-Modelo-De-Dos-Ejes-Y-Unidad-De-Entrega.md` es material de análisis emitido durante esta corrida y
**no forma parte de este plan**: es el insumo de la intervención estructural siguiente.

**Actualización del 2026-08-15**: esa intervención **se ejecutó** en la misma sesión. Su plan es
`40-Plan-8.0-Unidad-De-Entrega.md` y su verificación `42-Verificacion-8.0.md`. El framework quedó en
la versión 8.0, no en la 7.0 que verifica este documento.
