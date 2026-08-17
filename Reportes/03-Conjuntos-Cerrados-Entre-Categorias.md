# Reporte 03 — Conjuntos cerrados declarados en una categoría y extendidos en otra

| Campo | Valor |
|---|---|
| Reporte | 03 |
| Fecha | 2026-08-10 |
| Origen | Corrida real de la Fase B2 sobre el destino `Repos-RPIs/RPI.VidelControl`, proyecto de código `VideoControl-Web` |
| Versión del framework evaluada | SDD 6.0 (`Master-Prompt` 5.2, `Rules-Especificacion-Funcional` 4.0, `Rules-UX-UI-DX` 4.0, `Maqueta-Rules` 3.1) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Maqueta-Rules.md` §3.6; `SDD/Devs/Rules/Rules-Especificacion-Funcional.md`; `SDD/Devs/Rules/Rules-UX-UI-DX.md` |
| Naturaleza | Un hueco de arbitraje: dos categorías pueden afirmar cosas incompatibles sobre el mismo conjunto y el método no declara quién decide |
| Estado | Para evaluación. Ninguna modificación aplicada sobre el framework |
| Reportes relacionados | `02-Propagacion-De-La-Fase-B2.md`, que documenta por qué la contradicción tuvo tiempo de existir |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. El incidente](#2-el-incidente)
- [3. Por qué la contradicción es legítima de los dos lados](#3-por-qué-la-contradicción-es-legítima-de-los-dos-lados)
- [4. Lo que la normativa dice y lo que no dice](#4-lo-que-la-normativa-dice-y-lo-que-no-dice)
- [5. La causa raíz](#5-la-causa-raíz)
- [6. El patrón, enunciado](#6-el-patrón-enunciado)
- [7. Lo que hizo el agente y por qué no alcanza](#7-lo-que-hizo-el-agente-y-por-qué-no-alcanza)
- [8. Propuestas de intervención](#8-propuestas-de-intervención)
- [9. Cómo verificar que la corrección funcionó](#9-cómo-verificar-que-la-corrección-funcionó)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

La categoría 02 declaró que la verificación de efecto tiene **dos** valores de respuesta. La Fase B2 descubrió que hacían falta **tres**, y la categoría 03 especificó tres. Las dos categorías quedaron vigentes, aprobadas, y afirmando cosas incompatibles sobre el mismo conjunto.

El framework no tiene ninguna regla que impida eso, ninguna que lo detecte, y ninguna que declare quién arbitra. La única salida disponible fue que la categoría 03 escribiera dentro de sus documentos «esto es propuesta, consultar al Product Owner» — una nota en prosa, dentro de un artefacto, esperando que alguien la leyera.

Como lo señaló el humano al descubrirlo: **o se admiten dos valores o se admiten más; las dos cosas a la vez no**.

## 2. El incidente

| Momento | Qué decía la 02 | Qué decía la 03 |
|---|---|---|
| Fase B, cierre | CU-14 FA-02 y CU-15 FA-01: la verificación se registra como confirmada o como **no confirmada**. Dos valores | Los tres wireframes de configuración declaraban un «par de confirmación» de dos botones. Coherente |
| Fase B2, iteración 3 | Sin cambios | El análisis conceptual encontró que el par no tiene lugar para el observador que no pudo mirar, y la propuesta de rediseño especificó **tres** respuestas. La maqueta las implementó y el humano la aprobó |
| Entre medio | «no confirmada» significa que el equipo no produjo el efecto | «no observada» significa que la persona no pudo verlo. Colapsarla en «no confirmada» registra como hecho del equipo lo que fue una circunstancia de la persona |

El tercer valor no es un matiz de interfaz: **cambia qué se guarda**. Un canal registrado como «no confirmado» dice que se comandó y no se movió nada; el mismo canal registrado como «no observado» no dice nada sobre el canal. En un producto cuya razón de ser es no volver a afirmar cosas que no se saben, la diferencia es exactamente el punto.

## 3. Por qué la contradicción es legítima de los dos lados

Esto es lo que hace al hallazgo interesante y no un simple descuido:

- **La 02 no se equivocó.** Cuando se escribió, dos valores era lo que la necesidad de negocio pedía y lo que el flujo describía. No había forma de saber que faltaba un tercero sin haber visto a alguien frente a la pantalla.
- **La 03 tampoco se equivocó.** La Fase B2 existe precisamente para descubrir eso. `Maqueta-Rules.md:32` le dice al maquetador que cuando la maqueta revela que la especificación estaba incompleta, emita el hallazgo. Lo emitió.
- **La matriz de propagación tiene la fila.** «Un estado no previsto (vacío, error, sin permiso, parcial) → 03 (tabla de estados), 02 (flujo alternativo del CU), 08 (casos de prueba)» (`Maqueta-Rules.md:236`).

Y sin embargo la contradicción existió. Porque la fila de la matriz dice **a dónde** propagar, y no dice **quién decide** cuando lo que hay que propagar contradice una decisión ya aprobada de la categoría de destino.

## 4. Lo que la normativa dice y lo que no dice

**Dice** que 02 es dueña de los casos de uso y de las reglas de negocio, y que 03 es dueña de la experiencia. La titularidad está clara.

**Dice** que el maquetador no corrige la especificación por su cuenta (`Maqueta-Rules.md:32`).

**No dice** qué pasa cuando la categoría dueña de la experiencia necesita, para que la experiencia funcione, algo que la categoría dueña del modelo declara imposible.

**No dice** que los conjuntos cerrados que una categoría declara —valores de respuesta, estados de una entidad, códigos de resultado, clasificaciones— tengan que estar marcados como tales ni que su extensión tenga un procedimiento.

**No dice** que un audit tenga que verificar que dos categorías no afirmen cosas incompatibles sobre el mismo conjunto. Los audits de categoría verifican cada categoría contra su regla y contra su upstream; **ninguno cruza dos categorías buscando contradicciones sobre un mismo referente**.

## 5. La causa raíz

El framework trata la relación entre categorías como **jerárquica y unidireccional**: 02 es upstream de 03, y 03 traza contra 02. Ese modelo funciona mientras la información viaje en esa dirección.

La Fase B2 rompe el modelo, porque produce información que **sólo puede nacer aguas abajo**: nadie puede descubrir que un diálogo de dos respuestas está mal repartido leyendo un caso de uso. Cuando esa información contradice al upstream, el framework no tiene un canal declarado y lo que queda es una nota en prosa.

Dicho de otro modo: el framework tiene **trazabilidad** entre categorías —D6— pero no tiene **arbitraje**. Sabe conectar, no sabe resolver.

## 6. El patrón, enunciado

> **Cuando una categoría declara un conjunto cerrado y otra categoría necesita extenderlo, el framework no declara quién decide, ni exige que la contradicción se registre en un lugar donde alguien la vea. La única salida disponible es una nota en prosa dentro de un artefacto, que sobrevive a todos los audits porque ningún audit cruza dos categorías buscando contradicciones sobre el mismo referente.**

Dos propiedades del patrón que conviene retener:

1. **La contradicción no rompe nada al momento.** Cada categoría es internamente coherente, cada audit pasa, y el producto documentado es incoherente. Es exactamente la clase de defecto que el reporte `00` ya describía en otra forma: cada pantalla cumple y el conjunto no.
2. **La nota en prosa da falsa tranquilidad.** En esta corrida, cuatro documentos de la 03 decían «es propuesta: la 02 declara dos valores y el tercero es consulta al Product Owner». Estaba escrito, era honesto, y no le llegó a nadie: el Product Owner se enteró dos días después y por otra vía.

## 7. Lo que hizo el agente y por qué no alcanza

El agente hizo lo único que el framework le deja hacer: declarar la diferencia dentro de cada documento afectado y en la maqueta, con distintivo visible, y no cerrar la decisión por su cuenta. Fue correcto y fue insuficiente.

Insuficiente porque el mecanismo elegido —una fila en una tabla dentro de cuatro documentos— tiene el mismo alcance que el resto del texto de esos documentos, y por lo tanto no interrumpe a nadie. Una decisión pendiente que no interrumpe no es una decisión pendiente: es una nota.

El desenlace real lo confirma: la contradicción se resolvió cuando el humano leyó un informe de trabajo y preguntó «¿cómo puede haber dos valores?». No la resolvió el método.

## 8. Propuestas de intervención

No se aplican acá. Se enumeran para el prompt que intervenga el framework.

| # | Intervención | Artefacto | Efecto esperado |
|---|---|---|---|
| P-1 | Que toda categoría **marque explícitamente sus conjuntos cerrados** —valores de un campo, estados de una entidad, clasificaciones— como tales, en lugar de enumerarlos en prosa | `Rules-Especificacion-Funcional.md` y las reglas de cada categoría | Un conjunto cerrado se vuelve identificable, y por lo tanto verificable entre categorías |
| P-2 | Que exista una **detención declarada** cuando una categoría necesita extender un conjunto cerrado de otra: no una nota en el documento, una parada del orquestador con la pregunta al humano | `Maqueta-Rules.md` §3.6, `Master-Prompt.md` §7 | La decisión llega a quien la puede tomar, en el momento en que aparece |
| P-3 | Que los audits incorporen una **verificación cruzada entre categorías** sobre los conjuntos cerrados marcados, y que la divergencia sea hallazgo P0 | criterios de audit del `Master-Prompt` §10 | La contradicción deja de sobrevivir a los audits |
| P-4 | Que exista un **registro único de decisiones pendientes del producto**, fuera de los documentos que las originan, que el orquestador exhiba al cerrar cada fase | `Master-Prompt.md` §12, o artefacto nuevo | Una decisión pendiente deja de tener el mismo peso visual que el resto del texto |
| P-5 | Que la matriz de propagación de la Fase B2 distinga **propagar** de **contradecir**, y que el segundo caso tenga procedimiento propio | `Maqueta-Rules.md` §3.6 | El caso deja de tratarse como si fuera el mismo que agregar un flujo alternativo |

De las cinco, la P-4 es la que más lejos llega: el problema que este reporte describe es, en el fondo, que el framework no tiene dónde poner una pregunta abierta. Y esta corrida produjo siete más, todas viviendo hoy dentro de un archivo de datos de la maqueta.

## 9. Cómo verificar que la corrección funcionó

1. Correr una Fase B2 donde la maqueta necesite un valor que la 02 no admite, y comprobar que el orquestador se detiene en lugar de escribir una nota.
2. Correr un audit sobre un árbol con dos categorías que declaran conjuntos incompatibles sobre el mismo referente y comprobar que lo reporta como P0.
3. Comprobar que al cerrar una fase existe una lista de decisiones pendientes fuera de los documentos, y que su recuento coincide con lo que los documentos declaran como propuesta.

El desenlace de esta corrida sirve de caso de prueba: el Product Owner resolvió el 2026-08-10 a favor de los tres valores, CU-14 y CU-15 pasaron a versión 1.2 y los cuatro documentos de la 03 dejaron de declararlo como propuesta. Una corrección del framework tendría que haber producido ese mismo desenlace **dos días antes y sin que el humano tuviera que preguntar**.

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-10 | Reporte inicial: el framework no declara quién arbitra cuando una categoría necesita extender un conjunto cerrado que otra declaró, y ningún audit cruza dos categorías buscando contradicciones sobre el mismo referente. Verificado con el incidente de los valores de respuesta de la verificación de efecto, con cinco propuestas de intervención. |
