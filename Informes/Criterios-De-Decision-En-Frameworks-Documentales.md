---
doc_id: INF-2026-08-17-criterios-de-decision
doc_type: informe
title: Captura y aplicación de criterios de decisión en frameworks documentales operados por agentes
status: vigente
origin: agent
confidence: alta para lo medido sobre el árbol del framework; media para la caracterización del estado del arte, que se apoya en las fuentes citadas y no en una revisión sistemática
owner: AG-ROOT (Arquitecto de Soluciones)
fecha: 2026-08-17
---

# Captura y aplicación de criterios de decisión en frameworks documentales operados por agentes

## Resumen ejecutivo

**Qué es.** Un informe sobre cómo un framework normativo captura los criterios con los que se decide
ante una situación, y qué hace falta para que un agente pueda **identificar la situación, encontrar el
criterio y aplicarlo** en lugar de recordarlo.

**Para qué sirve.** Para decidir si un cuerpo normativo necesita un índice de criterios, si conviene
adoptar un estándar de modelado de decisiones, y qué se gana y qué se pierde con cada opción.

**A quién le sirve.** A quien interviene un framework de documentación —agregando reglas, criterios o
comprobaciones— y a quien opera uno con agentes.

**Hallazgo central, medido:** el Framework SDD contenía **202 situaciones catalogadas** con su
criterio, repartidas en **16 archivos de reglas** y escritas en **cuatro formas distintas**, **sin
ningún punto de entrada**. Los criterios existían y no se podían encontrar. La evidencia de que eso
tiene consecuencia está en §4.3: durante una migración real se aplicó un criterio **por memoria y no
por búsqueda**.

---

## 1. Definiciones

Los términos se fijan acá y se usan con el mismo sentido en todo el informe.

| Término | Definición | Fuente |
|---|---|---|
| **Criterio de decisión** | Regla que, dada una situación, determina qué salida corresponde | Definición operativa de este informe |
| **Situación** | Estado observable del sistema que activa un criterio. En DMN son las **condiciones de entrada** de una tabla de decisión | [OMG DMN](https://www.omg.org/intro/DMN.pdf) |
| **Tabla de decisión** | Artefacto con columnas de entrada y al menos una de salida, que expresa la lógica de negocio de una decisión | [Camunda, DMN Tutorial](https://camunda.com/dmn/) |
| **DRD** (Decision Requirements Diagram) | Representación gráfica de la estructura y dependencias de un modelo de decisión | [Wikipedia, DMN](https://en.wikipedia.org/wiki/Decision_Model_and_Notation) |
| **ADR** (Architecture Decision Record) | Registro de **una** decisión arquitectónica y su fundamento | [adr.github.io](https://adr.github.io/) |
| **ADL** (Architecture Decision Log) | Colección de los ADR de un proyecto u organización | [adr.github.io](https://adr.github.io/) |
| **Gobernanza en tiempo de ejecución** | Control de las acciones de un agente mediante políticas evaluadas en el momento de actuar, en lugar de supervisión externa posterior | [IBM, Agentic AI Governance Playbook](https://www.ibm.com/think/insights/agentic-ai-governance-playbook) |

**Distinción que ordena todo lo demás.** Un **ADR registra una decisión ya tomada**; una **tabla de
decisión declara el criterio con el que se decidirá**. Son artefactos complementarios y no
intercambiables: el primero mira hacia atrás y explica, el segundo mira hacia adelante y resuelve.

---

## 2. Situación y contexto

### 2.1 El contexto: un framework normativo operado por agentes

El caso estudiado es el **Framework SDD**, un cuerpo normativo en Markdown que gobierna la generación,
migración y reanudación de documentación de productos de software. Lo ejecutan agentes: un orquestador
despacha subagentes especializados por categoría, y cada uno lee las reglas que lo gobiernan.

Su escala al momento del informe, medida sobre el árbol en la versión **9.11**:

| Magnitud | Valor |
|---|---|
| Reglas con tabla de anti-patrones | 16 |
| Situaciones catalogadas con su solución | 202 |
| Umbrales numéricos declarados | 76 |
| Criterios de aceptación clasificados `[enumerable]` / `[interpretativo]` | 645 |
| Detenciones obligatorias declaradas | 8 |

*Evidencia: recuentos obtenidos con `grep` sobre `SDD/Devs/` en el commit de la versión 9.11.*

### 2.2 La situación: los criterios existían y no se podían encontrar

El framework escribía sus criterios en **cuatro formas distintas**:

1. **Tablas de anti-patrones** — *situación → problema → solución*, la forma más usada.
2. **Umbrales numéricos** dentro de la prosa de una regla.
3. **Secciones «cuándo corresponde y cuándo no»**, para salidas alternativas.
4. **Reglas de resolución** enunciadas en prosa, del tipo «cuando A y B no coinciden, gana A».

Ninguna de las dieciocho reglas se llamaba Criterios, Decisiones ni Situaciones. **No existía un
índice.** Un agente que enfrentaba una situación sólo podía aplicar el criterio correcto si ya lo había
leído.

### 2.3 Por qué importa: el modo de falla

Un criterio que no se encuentra **no falla ruidosamente**. El agente resuelve igual, con su propio
juicio, y produce algo defendible. La divergencia con el criterio del framework aparece más tarde —en
un audit, en una migración, o nunca—. Esa es su característica más incómoda: **no deja rastro en el
momento en que ocurre**.

---

## 3. Estado del arte

### 3.1 DMN: el estándar para expresar criterios

**Decision Model and Notation** es un estándar de la OMG introducido en 2015 para modelar, comunicar y
ejecutar decisiones operativas. Un modelo DMN tiene dos componentes: un **DRD**, que muestra la
estructura y las dependencias entre entradas, decisiones y fuentes de conocimiento, y una o más
**tablas de decisión**, cada una con columnas de entrada y al menos una de salida, que contienen la
lógica detallada. Está diseñado para que los modelos sean **intercambiables entre organizaciones**, y
se combina con BPMN y CMMN, con los que comparte familia de estándares visuales de la OMG.

**Lo que aporta al problema de este informe:** DMN nombra explícitamente lo que a las tablas de
anti-patrones les faltaba —**las condiciones de entrada**—. Una tabla *situación → solución* sin
declarar cómo se evalúa la situación no es una tabla de decisión: es una lista de recomendaciones.

### 3.2 ADR: el estándar para registrar la decisión tomada

Un ADR captura **una** decisión y su fundamento; el conjunto de ADR de un proyecto constituye su
**decision log**. El formato típico incluye título, contexto, decisión y consecuencias.

Dos criterios de esa práctica son directamente aplicables:

**Qué merece un ADR.** Se documentan las decisiones que **afectan la estructura, los atributos de
calidad clave, o que son difíciles de revertir**. Es un criterio de umbral: sin él, el registro se
llena de decisiones triviales y deja de leerse.

**Por qué el fundamento es obligatorio.** *«Un registro sin justificación pierde valor con el tiempo,
porque los interesados no pueden evaluar si la decisión sigue aplicando cuando las circunstancias
cambian.»* La consecuencia práctica: **un registro sin fundamento no se puede revisar**, sólo obedecer
o ignorar.

### 3.3 Gobernanza de agentes: la dirección de 2026

La gobernanza de sistemas agénticos se está moviendo **de la supervisión externa hacia la gobernanza en
tiempo de ejecución**: decisiones conscientes de política sobre prompts, salidas, llamadas a
herramientas, identidad, permisos, sensibilidad de datos y escalamiento.

Lo que se pide registrar por cada acción de un agente incluye su identificador y versión, los permisos
delegados, la herramienta invocada, **la decisión de política —permitir o denegar— y el paso de
razonamiento que el agente generó antes de actuar**.

Y el motivo por el que esto dejó de ser opcional: una encuesta de KPMG a líderes de grandes empresas
reporta que **el 75 % cita seguridad, cumplimiento y auditabilidad como los requisitos más críticos**
para desplegar agentes.

**Nota de alcance.** Esta sección caracteriza una dirección a partir de las fuentes citadas. **No es
una revisión sistemática de literatura** y no debe leerse como tal.

---

## 4. Ejemplos concretos

Los tres casos son del Framework SDD, verificables en su repositorio.

### 4.1 Una tabla de anti-patrones antes y después

**Antes**, en `Rules-Devops.md`:

```markdown
| Anti-patrón | Problema | Solución |
| Falta de SBOM | Inventario opaco, imposibilidad de responder ante CVE de dependencias | SBOM CycloneDX o SPDX adjunto a cada release |
```

**Después**, con la columna que declara cómo se evalúa la entrada:

```markdown
| Anti-patrón | Problema | Solución | Detección |
| Falta de SBOM | Inventario opaco, imposibilidad de responder ante CVE de dependencias | SBOM CycloneDX o SPDX adjunto a cada release | [enumerable] |
```

**Qué demuestra.** La marca no agrega criterio: **declara quién puede aplicarlo**. Un `[enumerable]` lo
verifica una compuerta mecánica antes de que ningún agente interprete; un `[interpretativo]` requiere
juicio y va al audit.

**Precondición del ejemplo:** el vocabulario `[enumerable]` / `[interpretativo]` **ya existía** en el
framework, con 645 usos en criterios de aceptación. No se introdujo una clasificación nueva.

### 4.2 Un criterio aplicado por memoria y no por búsqueda

Durante la consolidación de una categoría documental, cuatro documentos con el mismo nombre —uno por
componente— esperaban decisión: fundirlos en uno o conservarlos con identidad propia.

Se eligió conservarlos. **El fundamento aplicado fue el correcto** y estaba escrito en
`Migracion-Rules.md` §4.3.2, con su caso medido: *fundir artefactos con contratos de verificación
distintos no produce uno con un contrato, produce uno que no verifica ninguno*.

**Pero se aplicó por recuerdo de una migración anterior, no por búsqueda.** El criterio existía, tenía
fundamento y estaba en el archivo correcto. Sin ese recuerdo, la decisión habría dependido del juicio
del momento.

**Qué demuestra.** Un criterio correcto, bien escrito y bien fundamentado **es inútil si no hay forma
de llegar a él desde la situación**.

### 4.3 Una convergencia con ADR, alcanzada sin consultarlo

El framework exige que todo **apartamiento** —la decisión documentada de no emitir un artefacto
obligatorio— declare cuatro cosas, la cuarta de las cuales es **los disparadores concretos que
superarían la decisión**. Una intervención posterior le sumó **estado** y **contador de saltos de
versión sobrevividos**.

Ese diseño coincide con el criterio de la práctica ADR citado en §3.2: sin fundamento, nadie puede
evaluar si la decisión sigue aplicando cuando las circunstancias cambian.

**Qué demuestra, y en las dos direcciones.** Que el razonamiento del framework era sólido —llegó al
mismo lugar que el estándar—, y que **no se consultó el estándar a tiempo**, lo cual habría ahorrado el
recorrido.

---

## 5. Preguntas guía

Para evaluar si un cuerpo normativo permite aplicar sus criterios, y no sólo contenerlos:

1. **¿Cuántas formas distintas** hay de escribir un criterio en el cuerpo? Más de una obliga a conocer
   todas para buscar en todas.
2. **¿Existe un punto de entrada por situación?** Si la única forma de encontrar un criterio es
   recordar dónde está, el cuerpo no es aplicable por alguien que no lo escribió.
3. Por cada criterio: **¿está declarada la condición de entrada?** Es la diferencia entre una tabla de
   decisión y una lista de recomendaciones.
4. **¿Está declarado quién puede aplicarlo?** Un criterio verificable por guion y uno que exige juicio
   no se administran igual.
5. **¿Qué pasa cuando la situación no está catalogada?** Si el cuerpo no lo responde, cada agente
   improvisa y la improvisación no deja rastro.
6. **¿El criterio lleva su fundamento?** Sin él sólo se puede obedecer o ignorar, nunca revisar.
7. **¿Cómo se entera el índice de un criterio nuevo?** Un índice sin comprobación de cobertura se
   desactualiza y vuelve al punto de partida.

---

## 6. Criterios de calidad

Cómo se distingue una captura de criterios sólida de una pobre.

| Dimensión | Versión pobre | Versión sólida |
|---|---|---|
| **Localización** | El criterio vive donde se escribió | Hay un índice por situación que lleva a él |
| **Condición de entrada** | Implícita en el nombre del criterio | Declarada y evaluable |
| **Detectabilidad** | Sin declarar | Marcada, y con quién la verifica |
| **Fundamento** | «Porque sí» o ausente | Con su caso, y con lo que lo superaría |
| **Cobertura de la ausencia** | No se contempla | Hay una salida declarada para la situación no catalogada |
| **Mantenimiento del índice** | Manual y voluntario | Con comprobación de cobertura |

**El criterio de aceptación que resume el conjunto:** *alguien que no escribió el cuerpo normativo debe
poder, partiendo de una situación concreta, llegar al criterio que la resuelve y entender por qué es
ése*. Si hace falta haber leído el cuerpo entero, el cuerpo contiene los criterios pero no los aplica.

---

## 7. Observaciones

Se distinguen hechos de interpretaciones, según `Rule-Evidences.md`.

**Hechos verificados sobre el árbol del framework:**

- 16 reglas con tabla de anti-patrones; 202 situaciones; 76 umbrales; 645 marcas de detectabilidad en
  criterios de aceptación; 8 detenciones obligatorias. *Verificable con `grep` sobre `SDD/Devs/`.*
- Ningún archivo de reglas se llamaba Criterios, Decisiones ni Situaciones antes de la versión 9.11.
  *Verificable listando `SDD/Devs/Rules/`.*

**Interpretaciones del autor, declaradas como tales:**

- Que la ausencia de índice **causa** decisiones divergentes. El caso de §4.2 es **una** observación,
  no una medición de frecuencia. **No se midió** cuántas veces ocurrió.
- Que adoptar DMN completo no corresponde a un framework en Markdown. Es un juicio de costo-beneficio,
  no un resultado.

**Limitaciones:**

- La clasificación de las 202 situaciones en `[enumerable]` / `[interpretativo]` se hizo en **una
  primera pasada con criterio conservador** —ante la duda, `[interpretativo]`— y **no está verificada
  fila por fila**. El sesgo esperable es subestimar lo automatizable.
- §3.3 caracteriza una dirección a partir de las fuentes citadas y **no es una revisión sistemática**.
- Este informe describe **un** framework. No se compara con otros cuerpos normativos, y por lo tanto no
  se puede afirmar que el patrón sea general.

---

## 8. Conclusiones

**Un cuerpo normativo puede contener sus criterios y no permitir aplicarlos.** Es un estado estable:
nada falla, los criterios están bien escritos y bien fundamentados, y aun así el agente decide por su
cuenta porque no tiene cómo llegar a ellos.

**La diferencia entre contener y aplicar tiene un nombre en la industria y es anterior a los agentes.**
DMN separa la lógica de decisión del proceso que la invoca, y exige declarar las condiciones de
entrada; ADR separa la decisión de su fundamento y exige el segundo para que la primera sea revisable.
Ninguna de las dos ideas es nueva.

**Lo que sí cambió con los agentes es el costo del olvido.** Una persona que no encuentra un criterio
pregunta. Un agente que no lo encuentra **decide igual**, con un resultado plausible y sin dejar
rastro. Es la razón por la que la gobernanza de 2026 pide registrar el paso de razonamiento previo a la
acción, y no sólo el resultado.

**La intervención mínima no es adoptar un estándar: es hacer encontrables los criterios que ya
existen.** En el caso estudiado, un índice por situación y una marca de detectabilidad por criterio
—ambas apoyadas en vocabulario que el framework ya tenía— cubren el problema sin motor de decisión ni
formato de intercambio.

---

## 9. Referencias

| # | Fuente | Consultada |
|---|---|---|
| 1 | [OMG — Standard for Decision Model and Notation (DMN)](https://www.omg.org/intro/DMN.pdf) | 2026-08-17 |
| 2 | [Decision Model and Notation — Wikipedia](https://en.wikipedia.org/wiki/Decision_Model_and_Notation) | 2026-08-17 |
| 3 | [DMN Tutorial — Camunda](https://camunda.com/dmn/) | 2026-08-17 |
| 4 | [Decision Model and Notation — Drools Documentation](https://docs.drools.org/latest/drools-docs/drools/DMN/index.html) | 2026-08-17 |
| 5 | [Architectural Decision Records (ADRs)](https://adr.github.io/) | 2026-08-17 |
| 6 | [Maintain an architecture decision record — Microsoft Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/architect-role/architecture-decision-record) | 2026-08-17 |
| 7 | [Agentic AI Governance Playbook — IBM](https://www.ibm.com/think/insights/agentic-ai-governance-playbook) | 2026-08-17 |
| 8 | [Runtime Governance for AI Agents: Policies on Paths — arXiv](https://arxiv.org/html/2603.16586v1) | 2026-08-17 |
| 9 | [AI Agent Data Governance: The Enterprise Playbook for 2026 — Promethium](https://promethium.ai/guides/ai-agent-data-governance-enterprise-playbook-2026/) | 2026-08-17 |

**Evidencia interna del framework**, reproducible sobre el repositorio `IA.SDD` en la versión 9.11:

| Afirmación | Cómo verificarla |
|---|---|
| 16 reglas con tabla de anti-patrones | `grep -lc '^| Anti-patrón |' SDD/Devs/Rules/*.md` |
| 202 situaciones catalogadas | `grep -hc '\[enumerable\]\|\[interpretativo\]' SDD/Devs/Rules/*.md` sobre las filas de esas tablas |
| 645 criterios de aceptación clasificados | `grep -rn '\[enumerable\]\|\[interpretativo\]' SDD/Devs/Rules` |
| Ningún índice de criterios antes de 9.11 | `ls SDD/Devs/Rules/` en `_legacy/9.10/` |

---

## Control de cambios

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 2026-08-17 | Emisión inicial. Definiciones, situación y contexto medidos sobre el Framework SDD 9.11, estado del arte con DMN, ADR y gobernanza de agentes, tres ejemplos verificables, preguntas guía, criterios de calidad, observaciones con hechos e interpretaciones separados, y nueve referencias externas con su evidencia interna reproducible. |
