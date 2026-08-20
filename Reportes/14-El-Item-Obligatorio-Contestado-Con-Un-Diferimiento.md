# Reporte 14 — Un ítem obligatorio se puede contestar difiriéndolo, y el diferimiento no tiene dueño ni vencimiento

| Campo | Valor |
|---|---|
| Reporte | 14 |
| Fecha | 2026-08-18 |
| Origen | La tercera reanudación de un destino real y la reparación de su divergencia `D-03`: la estrategia de versionado exigía una etiqueta por etapa cerrada y el repositorio tenía **cero**, con ocho etapas construidas |
| Versión del framework evaluada | SDD 9.19 (`Rules-Devops.md` §4.3; `Root-Rules.md` §9.5 y §12; `Master-Prompt.md` §8.1) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Root-Rules.md` §12 · `SDD/Devs/Rules/Rules-Devops.md` §4.3 · las §4.x de toda regla que enumere ítems obligatorios |
| Naturaleza | Un hueco de método con daño medido. **No es un defecto de ninguna regla escrita**: el framework pidió el dato y aceptó, sin decirlo, que se contestara con una promesa |
| Estado | **RESUELTO** — aplicado en **SDD 10.0** y completado en **SDD 11.0**, donde se cerró el criterio de aceptación que la primera intervención dejó sin auditar. Ver «Cómo se resolvió», al final |
| Reportes relacionados | `07-Obligacion-Hacia-Una-Fase-Posterior.md`, que trata obligaciones que apuntan hacia adelante entre categorías; acá la obligación apunta hacia adelante **en el tiempo del producto**, no entre categorías |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**, y
sigue la forma de los reportes `00` a `13`: evidencia primero, propuesta después, y lo que no se sabe
declarado como tal.

---

## 1. Resumen

**El framework enumera ítems obligatorios y verifica que estén contestados, no que estén
resueltos.** `Rules-Devops.md` §4.3 punto 3 exige que `Estrategia-Versionado.md` declare la
herramienta de versionado y **su prefijo de tag**. El destino contestó ese ítem así:

> *«El que se fije al anclarla, registrado en el punto de control de la etapa `a`.»*

**Eso no es el prefijo: es una promesa de fijarlo.** El artefacto pasó su audit, la fase cerró, y la
frase se copió a **cuatro filas** del mismo documento. **El punto de control de la etapa `a` cerró el
2026-08-13 y no lo registró.** Ocho etapas después, el repositorio tenía **cero etiquetas** contra un
documento que declara la etiqueta el instrumento de reversión del producto, y **nada chirrió en
ninguna de las ocho**.

**El daño no es la etiqueta que falta: es que la ausencia era invisible.** Un ítem obligatorio
contestado con un diferimiento **se lee igual que uno resuelto** en toda verificación del método: la
sección existe, la fila existe, el criterio de aceptación que pregunta si el ítem está declarado se
cumple.

---

## 2. Lo que el framework ya resuelve bien, y no hay que reescribir

**Esta sección delimita qué no toca la intervención.**

- **El prefijo estaba contemplado.** No es un olvido: `Rules-Devops.md` §4.3 punto 3 lo pide con esas
  palabras, y la tabla de canales de §4.5 hasta escribe su forma —«Sólo en tag `v<X.Y.Z>` sin
  sufijo»—. **El framework pidió el dato y dio el ejemplo.**
- **La titularidad de los identificadores está resuelta, y bien.** `Root-Rules.md` §9.5 obliga a que
  toda categoría que acuñe un identificador declare su prefijo, su forma y su ámbito **en §3.2 de su
  propia regla**, con el fundamento escrito: *«Un identificador cuya forma no está declarada lo
  inventa quien lo necesita primero.»* Ese mecanismo funciona y **no es el que falla acá**.
- **La figura del punto abierto existe.** El destino la usó: `PA-06`, `PA-07`, `PD-01` a `PD-05`, con
  columnas *«quién lo cierra»* y *«cuándo»*. **El framework sabe representar una decisión pendiente.**
- **`Root-Rules.md` §12 ya resuelve el caso hermano.** La *referencia pendiente* tiene cierre
  obligatorio y exige que la reapertura traiga el insumo. **Lo que falta es el simétrico para un ítem
  de contenido**, no para una referencia.

**Y una lección que el framework ya aprendió de este mismo destino, y que acá se repite en otra
forma.** La 9.x incorporó, a partir de la `D-01` de este producto, que **toda fuente declarativa
nombre a su responsable**, porque *«una obligación sin sujeto no la incumple nadie en particular»*.
Este reporte es la otra mitad: **una obligación con sujeto y con evento, donde nadie comprueba que
el evento haya ocurrido.**

---

## 3. La evidencia

### 3.1 El incidente principal, medido

| Qué | Dato |
|---|---|
| Ítem obligatorio | `Rules-Devops.md` §4.3 punto 3 — «Configuración base y **prefijo de tag**» |
| Cómo se contestó | «El que se fije al anclarla, registrado en el punto de control de la etapa `a`» |
| Cuántas veces se copió la frase | **4 filas** del mismo documento (§3.1 a §3.4) |
| Evento al que se difirió | El punto de control de la etapa `a` |
| Cuándo ocurrió ese evento | **2026-08-13** |
| ¿Registró la decisión? | **No.** Las decisiones de ese punto de control son `R-02`, `R-03` y `R-04`; el prefijo no está |
| Cuánto duró el diferimiento sin señal | **8 etapas**, del 2026-08-13 al 2026-08-18 |
| Daño observable | **`git tag` devolvía cero**, contra un documento que declara la etiqueta el instrumento de reversión: «la reversión es volver a la etiqueta anterior y reconstruir» |
| Cómo se detectó | **No lo detectó ningún audit.** Lo encontró un orquestador de reanudación contrastando una fuente declarativa contra el árbol |
| Costo del retraso | **Tres de las ocho etapas ya no se pueden etiquetar** sin inventar el punto: `c` y `d` tienen ramas de la etapa después de su fusión nominal, y `f` no tiene fusión propia. **El daño se volvió irreversible mientras nadie miraba** |

**La última fila es la que da la medida.** No es que faltaran cinco minutos de trabajo: es que
**esperar ocho etapas destruyó la posibilidad de hacerlo bien** para tres de ellas.

### 3.2 Y el diferimiento apuntaba a un punto abierto que no lo cubría

**El destino intentó registrarlo, y el registro no alcanzó.** La fila de §3.2 decía:

> *«Queda abierto como `PD-01` de `Pipeline-CI-CD.md` §10.»*

Y el `PD-01` de §10.2 de ese documento es *«la herramienta concreta de cada stage —ejecutor de
pruebas, recolector de cobertura y reglas de análisis estático— y su anclaje de versión»*. **La
herramienta de versionado no figura en esa lista, y el prefijo tampoco.**

**Nadie lo hizo mal a propósito, y ése es el punto.** El autor difirió, quiso dejar rastro, y apuntó
a la fila más parecida. **El framework no tiene ninguna comprobación que cruce un diferimiento con el
punto abierto que dice registrarlo**, de modo que un puntero que no alcanza se lee igual que uno que
sí.

### 3.3 Un agravante que la intervención debería mirar: el empaquetado

**El ítem pide dos cosas en una línea** —«Configuración base **y** prefijo de tag»— y **sólo una
estaba genuinamente bloqueada**. La herramienta sí dependía de una decisión futura, declarada aguas
arriba (`PA-06`, `ADR-02003` §6, que acepta explícitamente depender de una herramienta no elegida).
**El prefijo no dependía de nada**: elegir `v` no exige haber elegido MinVer.

**El diferimiento legítimo de una mitad arrastró a la otra**, y nada lo notó porque el ítem es uno
solo. Al repararlo, fijar el prefijo tomó **una cita literal del propio framework** y la elección de
la herramienta **sigue abierta**, que es exactamente como tendría que haber estado desde el principio.

---

## 4. La causa raíz

**El método verifica presencia, y un diferimiento está presente.**

Los criterios de aceptación de las reglas preguntan si el artefacto declara el ítem. Una frase que
promete declararlo **satisface esa pregunta con la misma forma** que el dato real: hay sección, hay
fila, hay texto. Es el patrón que el reporte `10` ya enunció para otro caso —criterios que se
satisfacen trivialmente— y acá aparece en su versión temporal: **no es una declaración falsa, es una
declaración verdadera sobre el futuro**, y por eso ni siquiera incomoda a quien la lee.

**Y el diferimiento carece de las tres propiedades que lo harían gobernable:**

| Propiedad | ¿La tiene? | Consecuencia |
|---|---|---|
| **Marca reconocible** | No. Se escribe en prosa, con la forma de una respuesta | Ninguna verificación lo puede contar ni distinguir de un dato |
| **Dueño** | A veces, y en prosa | Cuando no lo tiene, es la `D-01` otra vez |
| **Vencimiento verificable** | **No, y es lo decisivo.** «El punto de control de la etapa `a`» es un evento, y **nadie comprueba el evento cuando ocurre** | El diferimiento sobrevive al evento que lo tenía que cerrar, en silencio y sin límite |

**El evento pasó y la promesa siguió viva.** Esa es la raíz: el framework sabe atar una decisión a un
evento futuro y **no sabe cerrar el lazo cuando ese evento llega**.

---

## 5. El patrón, enunciado

> **Un ítem de contenido obligatorio puede contestarse con la promesa de contestarlo, y el método no
> distingue la promesa del dato.** Toda verificación de presencia se satisface igual con las dos. El
> diferimiento no lleva marca, de modo que no se puede contar; se ata a un evento del producto, de
> modo que su vencimiento no es una fecha ni un artefacto; y **nada comprueba ese evento cuando
> ocurre**, de modo que la promesa sobrevive indefinidamente al momento en que debía cumplirse. El
> daño crece con el silencio y puede volverse irreversible antes de que alguien lo mire.

**Enunciado en general a propósito.** El caso que lo reveló es un prefijo de etiqueta en la categoría
09, pero **nada del patrón es de esa categoría**: cualquier §4.x que enumere ítems obligatorios
admite la misma respuesta.

---

## 6. Propuestas de intervención

**Punto de partida, no decisión tomada.**

### 6.1 El ítem diferido es una figura declarada, no prosa

Simétrica de la *referencia pendiente* de `Root-Rules.md` §12, y en la misma sección o al lado. Un
ítem obligatorio que no se puede contestar hoy **se marca**, con forma fija y cuatro campos:
**qué** falta, **por qué** no se puede hoy, **quién** lo cierra y **en qué evento verificable**.

**Sin marca no hay nada que contar**, y contar es la única defensa contra un defecto silencioso.

### 6.2 El evento de cierre nombra un artefacto, no un momento

«El punto de control de la etapa `a`» no es verificable: no deja rastro que se pueda mirar. **«La
fila de decisiones de `Plan-Etapa-A.md` §7» sí.** La regla pediría que el evento se exprese como
**un artefacto y una sección**, para que la comprobación sea abrir un archivo.

### 6.3 La compuerta mecánica los cuenta

`Master-Prompt.md` §10.0 ya evalúa propiedades enumerables antes de que el audit interprete nada, y
desde la 9.13 consume los anti-patrones `[enumerable]` de la regla en curso. **Un ítem diferido con
marca es enumerable por construcción**: la compuerta puede listar los abiertos, y **levantar los que
nombran un evento ya ocurrido**. Ésa es la comprobación que hoy no existe en ninguna parte.

### 6.4 Los ítems que empaquetan dos decisiones se separan

`Rules-Devops.md` §4.3 punto 3 pide herramienta **y** prefijo en una línea. **Cuando un ítem admite
que una mitad se difiera y la otra no, se parte en dos ítems.** Es la corrección más barata de las
cuatro y la que habría evitado este incidente entero.

### 6.5 El orquestador de reanudación lo mira

`Master-Prompt-Reanudacion.md` §2 resuelve seis dimensiones y ninguna pregunta *«¿qué se difirió y ya
venció?»*. **Es el prompt que más barato lo detecta**, porque ya lee el árbol entero sin memoria — de
hecho es el que lo detectó acá, pero por el síntoma y no por el diferimiento.

---

## 7. Cómo verificar que la corrección funcionó

- [ ] **Enumerable.** Existe una forma marcada de ítem diferido, y **se puede contar** cuántos hay
      abiertos en un destino con una búsqueda mecánica.
- [ ] **Enumerable.** Todo ítem diferido declara sus cuatro campos, y **el evento de cierre nombra un
      artefacto y una sección**, no un momento.
- [ ] **Enumerable.** Alguna compuerta levanta un ítem diferido cuyo evento de cierre **ya ocurrió**.
      Es la comprobación que faltaba y sin ella las otras tres no cierran el lazo.
- [ ] **Enumerable.** Ningún ítem obligatorio de una §4.x empaqueta dos decisiones cuando una sola
      puede estar bloqueada. Se audita una vez sobre las quince reglas.
- [ ] **Interpretativo.** Sobre el destino que originó el reporte, la corrección **habría detectado**
      el prefijo al cerrar la etapa `a`, con siete etapas de anticipación.

---

## 8. Lo que este reporte no sabe

- **Cuántos destinos tienen ítems diferidos hoy.** La medición es de **uno**. El patrón se sostiene
  por su mecánica —una verificación de presencia no distingue una promesa de un dato— y no por su
  frecuencia, que nadie midió.
- **Si conviene prohibir el diferimiento en algunos ítems.** Hay ítems donde diferir es legítimo —la
  herramienta de versionado lo era— y otros donde quizá no. **Este reporte no propone esa lista**:
  hacerla exige mirar las quince reglas, y es trabajo de la intervención.
- **Si `Root-Rules.md` §12 debería absorberlo en vez de tener figura propia.** Las dos son
  «algo que falta y tiene que cerrarse», y unificarlas puede ser mejor que agregar un concepto. **El
  método declara que un procedimiento que crece deja de leerse**, y ese criterio manda sobre la
  prolijidad conceptual.
- **Qué pasa con los diferimientos ya vencidos en destinos existentes.** Si la corrección los vuelve
  visibles, un destino con años de historia puede iluminar decenas de golpe. **Eso es una migración,
  no una regla nueva**, y este reporte no la dimensiona.

---

## 9. Control de cambios

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 2026-08-18 | Emisión inicial. Nace de la reparación de la divergencia `D-03` de un destino real —cero etiquetas contra ocho etapas cerradas— cuya causa no era el olvido de crear la etiqueta sino **un ítem obligatorio contestado con un diferimiento que sobrevivió ocho etapas a su propio evento de cierre**. Aporta la medición del incidente con su costo irreversible —tres de las ocho etapas ya no se pueden etiquetar sin inventar el punto—, el agravante del **puntero que no alcanza** —el diferimiento se registró contra un punto abierto que no lo cubría— y el del **ítem que empaqueta dos decisiones** cuando sólo una estaba bloqueada. Cinco propuestas, y la tercera —**que alguna compuerta levante un diferimiento cuyo evento ya ocurrió**— es la que cierra el lazo que hoy no cierra nadie. |

---

## Cómo se resolvió

**Estado: RESUELTO**, en dos intervenciones y no en una. La **10.0** aplicó las cinco propuestas y la
**11.0** cerró el criterio de aceptación que aquélla había dejado sin contestar. Notas de coherencia:
`SDD/Devs/Guides/Coherencia-Item-Diferido.md` y `SDD/Devs/Guides/Coherencia-Items-Empaquetados.md`.

**Qué resolvió, en una línea:** el método sabía atar una decisión a un evento futuro y no sabía cerrar
el lazo cuando ese evento llegaba.

| Propuesta | Dónde quedó escrito | Versión |
|---|---|---|
| §6.1 El ítem diferido es figura declarada | `Root-Rules.md` **§12.2**, cuatro campos obligatorios | 10.0 |
| §6.2 El evento nombra un artefacto, no un momento | §12.2 punto 4: *«nombrando un artefacto y su sección — no un momento»* | 10.0 |
| §6.3 La compuerta mecánica los cuenta | `Master-Prompt.md` §10.0, **comprobación transversal 6** | 10.0 |
| §6.4 Los ítems que empaquetan dos decisiones se separan | `Rules-Devops.md` §4.3 punto 3.b, y **la auditoría sobre las quince reglas**: cinco ítems partidos | 10.0 y **11.0** |
| §6.5 El orquestador de reanudación lo mira | `Master-Prompt-Reanudacion.md` R0 paso 4 y el bloque `ÍTEMS DIFERIDOS` de R1 | 10.0 |

**Los cinco criterios de aceptación de §7, contestados uno por uno**, que es lo que este reporte
terminó produciendo como método —la comprobación 13 de `SDD-Development-Guide.md` §VI.3:

| # | Criterio | Veredicto |
|---|---|---|
| 1 | Forma marcada y contable | **Cumple** (10.0) |
| 2 | Cuatro campos, evento como artefacto y sección | **Cumple** (10.0) |
| 3 | **Alguna compuerta levanta un ítem cuyo evento ya ocurrió** | **Cumple** (10.0), en dos lugares |
| 4 | Ningún ítem de una §4.x empaqueta dos decisiones. **Auditado sobre las quince reglas** | **Incumplido en 10.0**, **cumplido en 11.0** |
| 5 | *(Interpretativo)* Habría detectado el caso con siete etapas de anticipación | **Cumple por construcción**; no se midió sobre el destino |

**Lo que este reporte dejó de herencia, y no estaba entre sus propuestas.** Su criterio 4 se declaró
resuelto sin haberse auditado, y las doce comprobaciones de la intervención pasaron igual **porque
todas miran el árbol que quedó y el trabajo que faltaba no estaba en ningún archivo tocado**. De ahí
salió la **comprobación 13, devolución al origen** (`SDD 11.1`): la nota enumera los criterios del
origen y los contesta uno por uno. **Este reporte es el caso medido que la produjo.**
