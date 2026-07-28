# Nota de coherencia — E6 Superficie de entrada y guía de desarrollo

**Proyecto:** Framework SDD
**Documento:** Nota-Coherencia-E6.md
**Versión:** 1.0
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD

---

> **Nota posterior (E8, 2026-07-26).** Las dos decisiones abiertas que esta nota eleva se resolvieron en **E8**: el control de cambios propio del `README.md` (A-4) se cerró sin acción, y la normalización de «D1-D8» a «D1-D9» (A-1) se ejecutó sobre dieciocho ocurrencias normativas en siete archivos.

## 1. Alcance

Verificación de implantación de la etapa E6, que ejecuta dos solicitudes independientes entre sí pero unidas por el mismo fundamento: ambas describen el framework, y el framework cambió en E1 a E5.

- **S9** — convertir el `README.md` raíz en la superficie de entrada del repositorio, optimizada para que un agente que lo lee como primer y a veces único archivo pueda orientarse y decidir a dónde ir.
- **S10** — escribir `SDD-Development-Guide.md`, que existía como archivo vacío de cero bytes y nunca había sido commiteado.

E6 va al final de la intervención por diseño. Escribir estos dos documentos antes de E5 habría significado documentar un estado que iba a dejar de existir.

## 2. Inventario de archivos

| Archivo | Estado anterior | Estado nuevo |
| --- | --- | --- |
| `README.md` | 7 líneas: un título y tres enlaces | 132 líneas, 13.501 bytes |
| `SDD/Guides/SDD-Development-Guide.md` | Archivo vacío, 0 bytes, untracked en git | 504 líneas, 42.832 bytes, versión 1.0 |

Dos archivos. Ninguna eliminación, ningún renombre. El segundo pasa de existir vacío a existir con contenido, así que sigue apareciendo como untracked hasta que se lo commitee.

### 2.1 `README.md` — superficie de entrada

Siete bloques, en el orden que S9 fija:

| Bloque | Contenido |
| --- | --- |
| Qué es SDD | Tres párrafos: el problema que resuelve (los agentes derivan), la unidad de trabajo (solución con N proyectos tipados) y qué produce (doce categorías con auditoría entre fases, más el ciclo posterior al handoff) |
| Modelo de tres repositorios | Tabla de rol, escritura y contenido, más la única excepción de escritura sobre este repositorio |
| Anatomía del repositorio | Doce filas, una por carpeta de primer y segundo nivel, con su ruta enlazada |
| **Matriz de ruteo por intención** | Dieciséis intenciones, cada una con el archivo al que ir. Es el núcleo del documento |
| Mapa de las doce categorías | Carpeta de salida, archivo de reglas enlazado y nivel, más las cuatro reglas transversales y la dependencia 10 → 11 |
| Invariantes globales | **D1 a D9 enunciadas**, no solo nombradas, cada una con qué significa en la práctica |
| Reglas de intervención | Seis filas de qué exige cada clase de cambio, más la regla de autosuficiencia |

La matriz de ruteo cubre las nueve intenciones que S9 exige como mínimo, verificadas una por una, y agrega siete más: poner el framework a andar, aplicarlo paso a paso, entender el orden de fases, saber qué invariantes rigen, entender el sensado de deriva, entender la validación de maqueta, agregar un modelo UX-UI y consultar el changelog.

**Sobre las invariantes.** S9 pide enunciar D1 a D8. Enuncié **D1 a D9**, porque D9 existe desde la incorporación del sensado de deriva y omitirla dejaría al agente sin conocer una regla vigente. La tabla cierra declarando que buena parte del framework se refiere al conjunto como «D1-D8» por razones históricas y que el conjunto vigente es D1 a D9, para que un agente que encuentre las dos formas no las lea como una contradicción.

### 2.2 `SDD-Development-Guide.md` — guía de desarrollo

| Parte | Contenido |
| --- | --- |
| §1 Para quién es | Tabla de las cuatro guías con su lector y su pregunta, más la diferencia práctica: dirección de escritura |
| **I — Anatomía** | Mapa de dependencias en Mermaid, despiece de once carpetas con cuándo se toca cada una, y matriz de quién lee y quién escribe cada pieza |
| **II — Contratos internos** | Los seis contratos: estructura canónica de nueve secciones, cómo el orquestador decide qué generar, gating de doble granularidad, encadenamiento de la trazabilidad, expectativas del auditor y derivación de flags |
| **III — Extensibilidad** | Siete ejes, cada uno con qué agregás, archivos a tocar en orden, invariantes, cómo verificar y ejemplo trabajado. Más §III.8 sobre por qué D8 es cerrado |
| **IV — Criterios** | Preguntas guía agrupadas en cuatro bloques de decisión: antes de agregar, gating, verificación e impacto |
| **V — Anti-patrones** | Once anti-patrones con síntoma, consecuencia y corrección, más el desarrollo del caso de la verificación imposible |
| **VI — Procedimiento de cambio** | Versionado con la pregunta que resuelve las dudas, control de cambios, verificación de coherencia con lista de siete comprobaciones, segmentación de intervenciones grandes y tratamiento de documentación ya emitida |

Tres diagramas Mermaid: el grafo de dependencias del framework, el encadenamiento de la decisión de generación y la cadena de trazabilidad D6.

**Sobre la delimitación con el marco teórico.** La guía lo referencia por ruta relativa en tres lugares y no lo reescribe, no lo resume extensamente ni lo reemplaza. La distinción de audiencia entre las cuatro guías está declarada en §1, como S10 exige. Los siete ejemplos trabajados de la Parte III salen de las etapas E1 a E5 de esta misma intervención, que es material propio y no del marco.

## 3. Los ejemplos trabajados vienen de esta intervención

Los siete ejes de extensión llevan ejemplo trabajado, y en los siete el ejemplo es algo que ocurrió durante E1 a E5. No es una elección estética: son los únicos casos de extensión del framework que están documentados con su racional completo, y usar casos inventados habría producido una guía que enseña con material que nadie puede verificar.

| Eje | Ejemplo | De dónde sale |
| --- | --- | --- |
| Categoría nueva | Los siete lugares que tocó la categoría 11, y por qué el cuarto se olvida | E3 y DEC-05 |
| Artefacto nuevo | El contrato de verificación y su décimo paso sobre instrumentos transversales | E2 |
| Variante de especialidad | Las cuatro variantes `(opcional)` que pasaron a ser ocho distintas | E3 |
| Fase nueva | Las Fases I y J, y las tres cosas que ninguna fase anterior necesitaba | E4 |
| Gating por D8 | El cuerpo mantenedor de opcional a obligatorio para los ocho tipos | E3 |
| Modelo UX-UI | La ofuscación bloqueante y por qué la vía manual pierde esa verificación | Preexistente |
| Invariante global | D9 como precedente de «rige hacia adelante» | Preexistente |

El anti-patrón de la verificación imposible, que la Parte V desarrolla en prosa, es el déficit 2 que motivó esta intervención completa.

## 4. Lista de comprobación

| # | Comprobación | Resultado | Evidencia |
| --- | --- | --- | --- |
| 1 | Invariantes D1–D9 intactas | Cumple | D1 español rioplatense técnico, sin emojis ni negritas decorativas. D2 UTF-8 sin BOM, LF. D3 y D4: los nombres de archivo no cambian; `README.md` y `SDD-Development-Guide.md` conservan los suyos. D5: la guía nace en 1.0 con su control de cambios; el README no se versiona por convención de índice. D6: la guía declara sus `traces` en el frontmatter. D7: sin literales de dominio; los ejemplos son del propio framework. D8: los ocho valores enunciados en el README y fundamentados en §III.8. D9: enunciada en el README y usada como precedente en §III.7 |
| 2 | Autosuficiencia: cero referencias fuera de `/IA/IA.SDD/` | Cumple | Las once cadenas dan cero en todo el árbol. La guía de desarrollo no tiene ninguna URL. El README conserva **solo** la URL externa preexistente, sin agregar ninguna. Diátaxis, C4, arc42, SemVer, Game Day y las demás convenciones se nombran sin enlazar |
| 3 | Referencias internas: todo lo citado existe | Cumple | Los enlaces relativos de ambos documentos resuelven contra archivos y carpetas reales. Los 25 enlaces de la tabla de contenido de la guía resuelven contra sus anclas, y los del README también |
| 4 | Vocabulario normalizado | Cumple | «Integrador», «mantenedor», «operador», «rol de intervención». «Agente» calificado como agente humano o agente de IA |
| 5 | Sin contradicción con etapas anteriores | Cumple | El mapa de categorías del README declara 10-Examples y 11-Documentacion con la dependencia invertida. Las invariantes coinciden con `Master-Prompt.md` §5 y `Deriva-Rules.md` §1. La estructura canónica descripta en §II.1 coincide con la que E3 respetó. El anti-patrón de referenciar por número recoge la corrección que E4 aplicó |
| 6 | Control de cambios actualizado | Cumple | La guía nace con su fila 1.0. El README no lleva control de cambios propio: es índice, y sus cambios se registran en el `CHANGELOG.md` de la raíz, que es de E7 |
| 7 | El caso degenerado sigue produciendo el layout aplanado | Cumple | E6 no toca layout ni rutas de salida. La guía lo menciona como pregunta de impacto en la Parte IV |
| 8 | Nada fuera del alcance de la etapa fue modificado | Cumple | Dos archivos, los dos que S9 y S10 nombran |

Verificación adicional: marcas de bloque de código balanceadas en ambos documentos, tres bloques Mermaid con sintaxis de grafo válida, cero saltos triples de línea.

## 5. Observaciones

1. **Los diagramas Mermaid usan texto sin tildes dentro de los nodos.** Es deliberado. Los identificadores y las etiquetas de nodo de Mermaid conviven mal con caracteres acentuados en algunos renderizadores, y un diagrama que no renderiza es peor que uno con menos ortografía. La prosa alrededor sí lleva tildes, como D1 exige. Se declara para que no se lea como un descuido.
2. **El `README.md` no lleva sección de control de cambios.** Es un índice, y el framework ya trata a los `README.md` como artefactos sin sufijo de versión por convención. Su historial vive en el `CHANGELOG.md` de la raíz, que E7 actualiza. Si el responsable prefiere que lleve control de cambios propio, es una decisión abierta.
3. **La guía enuncia el conjunto de invariantes como D1 a D9 y el README también.** El resto del framework usa mayoritariamente «D1-D8», que es la forma histórica. No unifiqué esa nomenclatura en los dieciséis archivos de reglas porque excede el alcance de esta intervención y tocaría archivos que no son de esta etapa. **Decisión abierta**: si conviene un barrido que normalice «D1-D8» a «D1-D9» en todo el árbol, es una intervención propia.
4. **`SDD-Development-Guide.md` sigue untracked en git.** Existía como archivo vacío nunca commiteado, y esta etapa le puso contenido pero no commitea nada, según la restricción de dejar los cambios en el working tree. Conviene que quien revise no lo confunda con un archivo generado por error.
5. **La Parte III documenta siete ejes y el prompt pedía «como mínimo» esos siete.** No agregué un octavo. Los candidatos que evalué —agregar una regla transversal nueva, y agregar un flag— quedan cubiertos parcialmente por §III.1 y por §II.6. Si aparece necesidad concreta, son los dos ejes naturales para una versión 1.1 de la guía.

## 6. Veredicto

**CONFORME.**

El `README.md` pasó de tres enlaces a una superficie de entrada con matriz de ruteo de dieciséis intenciones, anatomía del repositorio, mapa de las doce categorías, las nueve invariantes enunciadas y las reglas de intervención. Cumple el criterio duro de S9: un agente con solo ese archivo en contexto puede responder a qué archivo ir para cualquiera de las nueve intenciones que la solicitud exige, y para siete más.

`SDD-Development-Guide.md` dejó de ser un archivo vacío. Documenta la anatomía, los seis contratos internos que hasta ahora no estaban escritos en ningún lado, siete ejes de extensión con ejemplo trabajado, los criterios para formar juicio antes de tocar nada, once anti-patrones y el procedimiento de cambio completo. No duplica el marco teórico: lo referencia y sigue.

Las cinco observaciones son tres decisiones de diseño declaradas y dos decisiones abiertas menores. Ninguna bloquea.

Corresponde detenerse y esperar confirmación humana antes de arrancar E7, la última etapa.

## 7. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.0 | 2026-07-26 | Nota de coherencia inicial de la etapa E6: inventario de los dos archivos con su estado anterior y nuevo, detalle de los siete bloques del README y de las seis partes de la guía de desarrollo, origen de los siete ejemplos trabajados, lista de comprobación de ocho puntos con verificación D1–D9, cinco observaciones y veredicto CONFORME. | Reformulación SDD |
