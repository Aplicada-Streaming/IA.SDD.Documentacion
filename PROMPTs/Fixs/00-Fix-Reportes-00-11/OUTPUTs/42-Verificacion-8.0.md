# Verificación de la 8.0: lo aplicado contra lo planificado

**Documento:** 42-Verificacion-8.0.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Verifica:** `40-Plan-8.0-Unidad-De-Entrega.md` contra el árbol de `IA/SDD/IA.SDD/`
**Estado:** Vigente

---

## 1. Resultado por ítem

Los veinte ítems del plan tienen fila.

| Id | Qué pedía | Resultado | Comprobación |
|---|---|---|---|
| Q-01 | `Vocabulario-Rules.md`: la unidad de entrega como nivel | **Aplicado** | 3.0. §2 nivel del layout y D8; §4 R3 con tabla de tres niveles; §5 tres confusiones nuevas; §8 cierra el pendiente |
| Q-02 | `Root-Rules.md`: rangos por unidad de entrega | **Aplicado** | 5.0. §9.1 y el mapa de rangos de `Master-Prompt.md` §3.4 |
| Q-03 | Layout nuevo | **Aplicado** | `Unidades-Entrega/<Nombre-Unidad-Entrega>/`, con los cuatro casos de aplanado en cascada |
| Q-04 | Derivación y bloque informativo | **Aplicado** | §3.2 y §3.4 |
| Q-05 | Gating por nivel | **Aplicado** | Tabla de niveles de flag, más `redistribuible` y `entrega_diferida` |
| Q-06 | Bucle por unidad de entrega | **Aplicado** | §6 y §7, en orden topológico del grafo de **integración** |
| Q-07 | Despacho, vista de producto, glosario | **Aplicado** | §8, §11 con la matriz de composición, §15 con nueve términos nuevos |
| Q-08 | Intake §13 en tres | **Aplicado** | §13.1, §13.2 y §13.3 |
| Q-09 | Intake §14: dos clases de contrato | **Aplicado** | Integración contra compilación, con la aclaración de que son dos grafos |
| Q-10 | Manifiesto deriva los dos ejes | **Aplicado** | §2.A, §2.B, §2.C y §3 con los dos grafos por separado |
| Q-11 | `Intake-Rules.md`: validaciones | **Aplicado** | Tres grupos y siete validaciones nuevas |
| Q-12 | Nivel por artefacto en las doce reglas | **Aplicado** | 8 en unidad de entrega, 5 en unidad + producto, 4 en producto, 1 en los tres |
| Q-13 | `Rules-Arquitectura-Tecnica.md` repartida | **Aplicado** | `Arquitectura-Unidad-Entrega.md`; `Vista-Producto.md` como artefacto de los dos ejes; ADR de producto para el proyecto compartido |
| Q-14 | `Rules-Devops.md` | **Aplicado** | Se construye por proyecto de código y se publica por unidad de entrega; matriz partida en dos |
| Q-15 | Las nueve reglas restantes | **Aplicado** | 244 líneas sustituidas con guarda, 46 preservadas y revisadas |
| Q-16 | `Maqueta-Rules.md` y `Deriva-Rules.md` | **Aplicado** | Maqueta y línea de base de una unidad de entrega |
| Q-17 | Migración estructural | **Aplicado** | `Migracion-Rules.md` 3.0, §4.3.2 con detención de clasificación |
| Q-18 | Las tres guías | **Aplicado** | User Guide 1.11 con los dos ejes y el test; Development Guide 1.8; Getting Started |
| Q-19 | Marco teórico | **Aplicado** | 3.0 |
| Q-20 | `CHANGELOG`, `_legacy`, nota de coherencia | **Aplicado con desviación declarada** | Ver §2 |

**Resumen:** 19 aplicados, 1 con desviación declarada, 0 sin aplicar.

## 2. La desviación

**Q-20 · No existe `_legacy/7.0/`.** El plan preveía archivar el conjunto superado. Las versiones 7.0
y 8.0 se publican en la misma intervención, de modo que la 7.0 **nunca fue un conjunto vigente** que
un destino pudiera consumir: no hubo commit entre una y otra. `_legacy/` conserva la **6.0**, que es
el último conjunto efectivamente superado.

Archivar un estado que nadie usó agregaría un snapshot que no permite reconstruir ninguna corrida
real, que es exactamente para lo que `_legacy/README.md` dice que existe la carpeta. La decisión queda
declarada en la entrada `[8.0]` del `CHANGELOG.md` y en la nota de coherencia.

## 3. Comprobaciones mecánicas

| Qué se comprueba | Resultado |
|---|---|
| Residuo de `tipo_proyecto_codigo`, excluidas las filas de control de cambios, las notas de coherencia históricas y `_legacy/` | **0** |
| Residuo de la ruta `Proyectos/<Nombre-Proyecto-Codigo>/`, ídem | **1**, y es la cita histórica de `Vocabulario-Rules.md` §8, que describe cómo era el layout antes de la 8.0 |
| Niveles declarados en las cabeceras de regla | 8 unidad de entrega, 5 unidad + producto, 4 producto, 1 los tres niveles, 1 con columna por artefacto |
| Tablas de control de cambios con columnas consistentes | Sin discrepancias |
| Entradas del glosario operativo | 58 |

## 4. Los pendientes de la 7.0, cerrados o replanificados

| Pendiente | Estado |
|---|---|
| Correr el grafo de obligaciones sobre las diecisiete reglas | **Cerrado.** Corrido y triado: 48 coincidencias brutas, 22 pares, y **tres obligaciones que ninguna corrida había encontrado**. Inventario en `41-Grafo-De-Obligaciones-Inventario.md`, tratamiento aplicado |
| Inventario del vocabulario propio del framework | **Cerrado.** Nueve términos sin definir, **cinco de ellos acuñados por las propias versiones 7.0 y 8.0**. Los nueve entraron al glosario operativo |
| Decidir si el «glosario de categoría» es artefacto real | **Cerrado por definición.** Queda como concepto del glosario operativo: la 8.0 declara que el vocabulario del método vive en el glosario operativo y el del producto en el glosario de la categoría que lo acuña, que sí existe —`Glosario-Funcional.md`, `Glosario-UX.md`, `Glosario-Tecnico.md`— |
| Adelantar la condición de terminado a la Fase A | **Sigue pendiente**, declarado. Es ortogonal al nivel y se administra con la referencia pendiente |
| Ampliar D9 a los recuentos en prosa | **Descartado con motivo.** `Root-Rules.md` §10 consigue el efecto sin ampliar la invariante |

## 5. Lo que quedó declarado y sin ejecutar

**La cardinalidad de soluciones de código.** `Vocabulario-Rules.md` §5 declara que un producto puede
tener más de una solución de código, y el inventario del eje de construcción no agrupa por solución
cuando hay más de una. Los tres destinos medidos tienen una sola —aunque una es heterogénea, con
proyectos .NET y Node en el mismo agrupador— así que el caso no está probado. Registrado en
`Vocabulario-Rules.md` §8, en el mismo lugar donde estaba el pendiente que esta intervención cierra.

**La verificación en corrida.** Igual que en la 7.0: lo verificado es que la corrección está en el
texto y que las comprobabilidades estáticas dan el resultado esperado. Que el defecto no reaparezca
exige **correr el orquestador sobre un destino**, o migrar uno existente. Para la 8.0 el candidato
natural es `Lab-Geometria`, que tiene dos unidades de entrega y un proyecto compartido, que es
justamente la forma que la versión anterior no podía documentar.
