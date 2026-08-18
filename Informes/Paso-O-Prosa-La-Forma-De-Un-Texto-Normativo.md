---
doc_id: INF-2026-08-18-paso-o-prosa
doc_type: informe
title: Paso o prosa — cuándo un texto normativo debe ser un procedimiento y cuándo una explicación
status: vigente
origin: agent
confidence: alta para lo medido sobre el árbol del framework; media para la caracterización del estado del arte, apoyada en las fuentes citadas y no en una revisión sistemática
owner: AG-ROOT (Arquitecto de Soluciones)
fecha: 2026-08-18
---

# Paso o prosa — cuándo un texto normativo debe ser un procedimiento y cuándo una explicación

## Resumen ejecutivo

**Qué es.** Un informe sobre el criterio que decide la **forma** de un texto normativo: si va como
**paso ejecutable** o como **prosa explicativa**. No trata sobre si el contenido es correcto: trata
sobre si, siendo correcto, se va a aplicar.

**Para qué sirve.** Para quien escribe o interviene un cuerpo normativo y tiene que decidir dónde poner
cada cosa. Y para diagnosticar por qué una regla correcta no se cumple.

**A quién le sirve.** A quien mantiene un framework de documentación, y a quien opera uno con agentes.

**Hallazgo central, con su caso.** Una advertencia **específica, con su caso medido, escrita en la
sección que gobierna la operación**, no se aplicó. No estaba mal escrita ni era ambigua: estaba como
**bullet en una lista temática de ocho, dentro de una sección de 196 líneas**. La conclusión que se
sigue —y que este informe desarrolla— es que **a una sección larga no se entra a leerla: se entra a
buscar una cosa**, y por lo tanto el formato decide la aplicación con independencia de la calidad.

---

## 1. Definiciones

| Término | Definición | Fuente |
|---|---|---|
| **How-to guide** | Documentación orientada a una tarea, para quien **ya sabe del tema** y está trabajando con una pregunta concreta. Lleva pasos | [Diátaxis](https://diataxis.fr/) |
| **Explanation** | Documentación que da contexto y trasfondo, para quien quiere **entender**. Responde preguntas de «por qué» | [Diátaxis](https://diataxis.fr/) |
| **Killer item** | En diseño de listas de verificación, el paso **cuyo olvido causa daño serio**, y que se omite bajo presión. Es el criterio de admisión a la lista | [Checklist Manifesto](https://tallyfy.com/checklist-manifesto/) |
| **Punto de parada** (*pause point*) | El momento exacto en que una lista se corre, declarado —por ejemplo, «antes de la incisión»— | [Creating a Killer Checklist](https://www.projectmanagement.com/blog-post/21259/creating-a-killer-checklist--lessons-from--the-checklist-manifesto-) |
| **DO-CONFIRM / READ-DO** | Los dos modos de una lista: hacer de memoria y después confirmar, o leer cada paso y hacerlo | [Checklist Manifesto](https://tallyfy.com/checklist-manifesto/) |
| **Paso con fundamento pegado** | Un paso que lleva junto el porqué que permite reconocer cuándo **no** aplica | Definición operativa de este informe |

**La distinción que ordena todo lo demás.** Diátaxis decide la forma **por la situación del lector**;
el diseño de listas decide **qué merece entrar** en la forma procedimental. Son criterios
complementarios y responden preguntas distintas: **qué forma** y **qué contenido en esa forma**.

---

## 2. Situación y contexto

### 2.1 El contexto

El caso es el **Framework SDD**, cuerpo normativo en Markdown operado por agentes. Al 2026-08-18, en su
versión 9.15, contiene cinco procedimientos numerados y una lista de verificación:

| Artefacto | Ítems |
|---|---|
| `Master-Prompt.md` §12.1, traspaso por *pull request* | 7 (T0 a T6) |
| `Master-Prompt.md` §8.1, forma de toda detención | 4 (F1 a F4) |
| `Migracion-Rules.md` §4.3.1, mover un documento | 5 |
| `Migracion-Rules.md` §4.3.2, comparar y verificar | 5 (C1 a C5) |
| `Migracion-Rules.md` §4.3.2, emitir el consolidado | 4 (E1 a E4) |
| `SDD-Development-Guide.md` §VI.3, comprobaciones de intervención | **12** |

*Evidencia: recuentos con `grep` sobre el árbol en la versión 9.15.*

### 2.2 La situación: una regla correcta que no se aplicó

`Migracion-Rules.md` §4.3.2 contenía, desde una migración anterior, esta advertencia:

> **La transposición lee el documento entero, no sólo sus secciones numeradas.** El texto entre la
> cabecera y la primera sección —una nota previa, una declaración de origen— **se pierde si el
> procedimiento recorre encabezados.** En una corrida real alcanzó a dos documentos.

**Era específica, tenía su caso medido y estaba en la sección correcta.** Y en una consolidación
posterior el defecto se produjo igual: la transposición recorrió encabezados y perdió la prosa que no
colgaba de ninguno.

**Por qué no se leyó, reconstruido:** se entró a §4.3.2 buscando dos cosas concretas —qué salida
aplicaba, y cómo comparar versiones—. La advertencia estaba entre ambas, como **uno de ocho bullets**
sobre temas sin relación entre sí, dentro de una sección de **196 líneas**. Nada señalaba que fuera
procedimental.

### 2.3 Por qué importa

**Este modo de falla no es ruidoso.** La regla está, es correcta y nadie la contradice; simplemente no
se ejecuta, y el defecto aparece aguas abajo sin conexión visible con la regla que lo habría evitado.

---

## 3. El criterio

### 3.1 Las salidas son tres

| Salida | Cuándo |
|---|---|
| **Prosa** | Se lee para **entender o decidir**, no ejecutando |
| **Paso** | Se lee **ejecutando**, su omisión hace daño **y** es olvidable |
| **Paso con su fundamento pegado** | Lo anterior, y además hace falta saber **cuándo no aplica** |

**Tratarlo como dicotomía es el primer error**, y produce dos consecuencias opuestas: fundamentos
convertidos en pasos que nadie ejecuta, y pasos sin fundamento que nadie sabe cuándo no aplicar.

### 3.2 Las tres condiciones del paso, que son necesarias juntas

| ¿Se lee ejecutando? | ¿Su omisión hace daño? | ¿Es olvidable? | Forma |
|---|---|---|---|
| No | — | — | Prosa |
| Sí | No | — | Prosa |
| Sí | Sí | No | Prosa — se recuerda solo |
| Sí | Sí | Sí | **Paso** |

La primera columna es Diátaxis: la forma la decide la situación del lector, **no la importancia del
contenido**. Las otras dos son el filtro de *killer items*: la práctica de listas de verificación
insiste en incluir **sólo los pasos críticos que se omiten bajo presión, no la lista completa de la
tarea**.

### 3.3 Cuatro reglas de sostén

**R1 · Un paso previene; una comprobación detecta, y no son sustitutos.** Si el costo de rehacer lo
detectado es alto, el ítem va como paso **aunque la comprobación exista**.

**R2 · El paso lleva su fundamento junto.** Sin él se obedece o se ignora, nunca se adapta.

**R3 · Presupuesto de 5 a 9 ítems por punto de parada.** Al llenarse: se parte en dos puntos de parada,
o el ítem de menor daño vuelve a prosa. **Agrandar la lista no es una opción** — la práctica fija ese
rango porque más allá deja de leerse.

**R4 · El disparador de revisión es la falla, no la previsión.** *«Una lista nunca está bien la primera
vez: probala, mirala fallar y mejorala.»*

### 3.4 Y el ítem obligatorio que se olvida

**Declarar cuándo se corre.** El diseño de listas exige definir el punto de parada con precisión. Un
procedimiento sin momento de uso se ejecuta cuando alguien se acuerda, que es la definición de lo que
no es un procedimiento.

---

## 4. Aplicación por escenario

### 4.1 Escribir una regla nueva

Se decide la forma **antes** de escribir, con la tabla de §3.2. El error frecuente es escribir primero
y ubicar después: el texto sale con forma de explicación —porque explicar es lo natural al escribir— y
se ubica en una lista donde ya hay ocho cosas.

### 4.2 Revisar una regla existente que no se cumple

La pregunta no es «¿está bien escrita?». Es **«¿en qué forma está, y quién la lee en ese momento?»**.
Si el incumplimiento se repite con la regla correcta a la vista, el problema es la forma.

### 4.3 Después de una falla

R4 fija el disparador. Y el ítem que entra **no es el que falló**, sino **el paso cuya omisión produjo
la falla** — que puede ser anterior.

### 4.4 Cuando el procedimiento llega a nueve

Se aplica R3. Partir en dos puntos de parada es preferible a devolver a prosa **cuando los ítems se
agrupan naturalmente por momento de uso**; devolver a prosa es preferible cuando el ítem sobrante es de
daño bajo.

---

## 5. Ejemplos concretos

Los tres son verificables sobre el repositorio del framework.

### 5.1 Un bullet que debía ser paso

El caso de §2.2. La advertencia sobre leer el documento entero cumplía **las tres condiciones**: se lee
ejecutando una transposición, su omisión pierde contenido, y es olvidable —lo fue—. **Correspondía
paso, estaba como bullet.** En la versión 9.14 pasó a ser el paso **E2** con su fundamento pegado, y el
bullet se retiró de la lista para que la advertencia y el paso no vivieran separados.

### 5.2 Una comprobación que no reemplazó al paso

La verificación de preservación de §4.3.2 detectó **tres defectos de emisión en cuatro
consolidaciones**: la transposición que recorría el documento vivo en vez de la unión, el cuerpo sin
salto de línea final, y la prosa perdida al regenerar el índice.

**La comprobación funcionó las tres veces.** Y aun así los tres pasaron a ser pasos —E1, E4 y E3—,
porque **cada detección costó rehacer la categoría entera**. Es el caso que fundamenta R1, y **no
proviene de las fuentes**: proviene de esta medición.

### 5.3 Una lista propia que excede el presupuesto

`SDD-Development-Guide.md` §VI.3 tiene **doce comprobaciones**. El rango recomendado es **5 a 9**.

**El criterio que este informe propone lo declara fuera de presupuesto**, y la salida de R3 es
aplicable: las doce se agrupan por momento de uso —algunas se corren **durante** la intervención, otras
**al cerrarla**—, así que corresponde partirla en dos puntos de parada antes que quitar comprobaciones.

**No se aplicó todavía.** Se declara acá como observación, no como hecho consumado.

---

## 6. Preguntas guía

1. ¿Este texto se lee **ejecutando una operación**, o **decidiendo cuál ejecutar**?
2. Si se omite, **¿qué se rompe?** Y si no se rompe nada, ¿por qué está?
3. ¿Es **olvidable bajo presión**, o se recuerda solo?
4. ¿Existe una **comprobación** que lo detecte después? ¿Cuánto cuesta rehacer lo que detecte?
5. ¿El paso lleva **su fundamento**, o sólo la orden?
6. ¿El procedimiento pasa de **nueve**? ¿Los ítems se agrupan por momento de uso?
7. ¿Está declarado **cuándo se corre**?

---

## 7. Criterios de calidad

| Dimensión | Versión pobre | Versión sólida |
|---|---|---|
| **Forma** | Elegida por importancia del contenido | Elegida por la situación del lector |
| **Admisión** | Todo lo relevante entra al procedimiento | Sólo lo que cumple las tres condiciones |
| **Fundamento** | En otra sección, o ausente | Junto al paso |
| **Tamaño** | Crece con cada lección | Acotado, con regla de qué hacer al llenarse |
| **Momento de uso** | Implícito | Declarado |
| **Revisión** | Por previsión | Por falla observada |

**El criterio de aceptación que resume el conjunto:** *quien está ejecutando la operación debe
encontrar lo que necesita sin leer la sección entera*. Si hace falta leerla toda, la forma está mal
elegida por más correcto que sea el texto.

---

## 8. Observaciones

Se distinguen hechos de interpretaciones, según `Rule-Evidences.md`.

**Hechos verificados sobre el árbol del framework, versión 9.15:**

- Los cinco procedimientos y sus tamaños: 7, 4, 5, 5 y 4 ítems. *Verificable con `grep` sobre los
  archivos citados en §2.1.*
- `SDD-Development-Guide.md` §VI.3 tiene **12** comprobaciones. *Verificable contando las filas de su
  tabla.*
- §4.3.2 tiene **196 líneas** y **8 bullets** en la lista donde estaba la advertencia. *Verificable con
  `awk` sobre el rango de la sección.*
- La advertencia existía antes del defecto que describía, y el defecto se produjo igual. *Verificable
  comparando el archivo en `_legacy/` con el registro de la consolidación.*

**Interpretaciones del autor, declaradas como tales:**

- Que **el formato causó** la no aplicación. Es la reconstrucción más plausible del caso, no una
  medición: no se instrumentó la lectura. Una explicación alternativa —descuido, sin relación con el
  formato— **no puede descartarse con la evidencia disponible**.
- Que el rango 5–9 aplica a un cuerpo normativo leído por agentes. Las fuentes lo establecen para
  listas de verificación operadas por personas bajo presión de tiempo. **La transferencia es
  razonable y no está validada.**

**Limitaciones:**

- **Un caso, un framework.** No se puede afirmar que el patrón sea general.
- El estado del arte se apoya en las fuentes citadas y **no es una revisión sistemática**. En
  particular, las fuentes consultadas sobre listas de verificación son secundarias —resúmenes y
  artículos sobre la obra—, no la obra original.
- El criterio **decide la forma, no la corrección del contenido**. Un paso bien ubicado y equivocado
  sigue siendo equivocado.

---

## 9. Conclusiones

**Un cuerpo normativo puede fallar sin que ninguna de sus reglas esté mal.** El caso que originó este
informe no tiene una regla incorrecta, ambigua ni desactualizada: tiene una regla correcta en una forma
que no se lee en el momento en que hace falta.

**La forma no es presentación: es parte de la norma.** Diátaxis lo formula para documentación de
producto —la forma la decide la situación del lector— y el diseño de listas de verificación lo formula
para la operación crítica —sólo entra lo que se olvida y hace daño—. Aplicados juntos, deciden dónde va
cada texto sin dejarlo al gusto de quien escribe.

**Y hay un límite que conviene tener presente antes que después:** el mismo criterio que exige promover
a paso lo que falla, exige **no promover todo**. Un procedimiento que crece con cada lección aprendida
termina siendo la sección larga que nadie lee — que es exactamente el problema del que se partió.

---

## 10. Referencias

| # | Fuente | Consultada |
|---|---|---|
| 1 | [Diátaxis](https://diataxis.fr/) | 2026-08-17 |
| 2 | [Start here — Diátaxis en cinco minutos](https://diataxis.fr/start-here/) | 2026-08-17 |
| 3 | [The Checklist Manifesto — resumen (Tallyfy)](https://tallyfy.com/checklist-manifesto/) | 2026-08-17 |
| 4 | [Creating a Killer Checklist (ProjectManagement.com)](https://www.projectmanagement.com/blog-post/21259/creating-a-killer-checklist--lessons-from--the-checklist-manifesto-) | 2026-08-17 |
| 5 | [Lessons We Can Learn From Aviation Checklists (SafetyCulture)](https://blog.safetyculture.com/tips-tricks/checklist-best-practices/lessons-we-can-learn-from-aviation-checklists) | 2026-08-17 |
| 6 | [OMG — Decision Model and Notation (DMN)](https://www.omg.org/intro/DMN.pdf), por la noción de condición de entrada declarada | 2026-08-17 |
| 7 | [Architectural Decision Records](https://adr.github.io/), por el criterio de qué merece registrarse | 2026-08-17 |

**Evidencia interna**, reproducible sobre `IA.SDD` en la versión 9.15:

| Afirmación | Cómo verificarla |
|---|---|
| Tamaño de los cinco procedimientos | `grep -c '^\*\*C[0-9] ·'`, `'^\*\*E[0-9] ·'`, `'^### T[0-6] ·'`, `'^\*\*F[0-9] ·'` sobre los archivos de §2.1 |
| 12 comprobaciones en §VI.3 | Contar las filas `\| N \|` de la tabla de esa sección |
| 196 líneas y 8 bullets en §4.3.2 | `awk '/^### 4\.3\.2/,/^### 4\.4/'` sobre `Migracion-Rules.md` |
| La advertencia precedía al defecto | El archivo en `_legacy/` de la versión anterior contra el registro de la consolidación |

---

## Control de cambios

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 2026-08-18 | Emisión inicial. Definiciones con fuente, el caso medido que originó el criterio, las tres salidas y las tres condiciones necesarias, cuatro reglas de sostén, aplicación por escenario, tres ejemplos verificables —incluido uno donde **el propio framework excede el presupuesto**—, preguntas guía, criterios de calidad, observaciones que separan hechos de interpretaciones con sus limitaciones, y siete referencias externas con su evidencia interna reproducible. |
