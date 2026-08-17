# Reporte 11 — El vocabulario del método no tiene glosario propio, y el criterio que lo gobierna lo manda al glosario del producto

| Campo | Valor |
|---|---|
| Reporte | 11 |
| Fecha | 2026-08-12 |
| Origen | Corrida real del orquestador sobre el destino `Repos-RPIs/RPI.VidelControl`: Fase G, categoría 10 de los cinco proyectos de código, 2026-08-12. Lo detectó una revisión humana al preguntar de dónde salían cinco términos que la emisión usaba sin declarar |
| Versión del framework evaluada | SDD 6.0. El criterio de gobierno de glosario, en las secciones de criterios de aceptación de las **once** reglas que lo llevan: `Root-Rules`, `Rules-Contexto`, `Rules-Necesidades-Negocio`, `Rules-Arquitectura-Tecnica`, `Rules-Backlog-Tecnico`, `Rules-Plan-Sprint`, `Rules-Calidad-Y-Pruebas`, `Rules-Devops`, `Rules-Examples`, `Rules-Documentacion` y `Rules-Prompts-AI`. Además `Vocabulario-Rules.md` §2, §6 y §8; `Master-Prompt.md` §15; `Deriva-Rules.md` completo |
| Artefactos del framework alcanzados | Las once reglas de la fila anterior, `Vocabulario-Rules.md`, `Master-Prompt.md` §15 y `Deriva-Rules.md`. También las dos reglas que **no** llevan el criterio, `Rules-Especificacion-Funcional` y `Rules-UX-UI-DX`, siendo que la primera emite el glosario al que nueve de las once apuntan |
| Naturaleza | El framework acuña vocabulario propio fuera de los seis términos que su regla de vocabulario gobierna; ese vocabulario no está declarado en ningún glosario suyo; y el criterio que exige declararlo está replicado en once reglas que apuntan a nueve destinos distintos, casi todos glosarios del producto. Una de las once ya enuncia la política correcta, sobre términos que no la necesitaban |
| Estado | Para evaluación. Ninguna modificación aplicada sobre el framework |
| Reportes relacionados | `03-Conjuntos-Cerrados-Entre-Categorias.md`, que documenta otro caso de dos categorías respondiendo distinto sobre lo mismo. `10-Criterios-Que-Se-Satisfacen-Trivialmente.md`, cuyo §3 registra que el criterio de glosario de `Rules-Examples.md` nació de un audit que se cumplía trivialmente |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. El incidente](#2-el-incidente)
- [3. «Sonda» es la unidad de un mecanismo entero y ningún glosario la define](#3-sonda-es-la-unidad-de-un-mecanismo-entero-y-ningún-glosario-la-define)
- [4. Nueve destinos distintos para la misma pregunta](#4-nueve-destinos-distintos-para-la-misma-pregunta)
- [4.1 Las dos filas que importan, enfrentadas](#41-las-dos-filas-que-importan-enfrentadas)
- [4.2 Qué le pasa a un destino con esto](#42-qué-le-pasa-a-un-destino-con-esto)
- [4.3 Lo que cada respuesta implica, para quien tenga que elegir una](#43-lo-que-cada-respuesta-implica-para-quien-tenga-que-elegir-una)
- [5. El criterio manda declarar en el glosario del producto lo que acuñó el método](#5-el-criterio-manda-declarar-en-el-glosario-del-producto-lo-que-acuñó-el-método)
- [6. La causa raíz](#6-la-causa-raíz)
- [7. El patrón, enunciado](#7-el-patrón-enunciado)
- [8. Qué hizo el destino](#8-qué-hizo-el-destino)
- [9. Propuestas de intervención](#9-propuestas-de-intervención)
- [10. Cómo verificar que la corrección funcionó](#10-cómo-verificar-que-la-corrección-funcionó)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

`Vocabulario-Rules.md` es la regla de vocabulario del framework, y gobierna **seis términos**: producto, proyecto de código, unidad de entrega, módulo, proyecto y solución de código. Los define, declara su correspondencia con el vocabulario de industria y resuelve qué pasa cuando el dominio de un cliente usa una de esas seis palabras con otro sentido.

Fuera de esos seis, el framework acuña bastante más vocabulario propio, y ese vocabulario **no lo gobierna ninguna regla**. `sonda`, `pasada de diseño`, `pasada de ejecución` y `arnés` atraviesan hasta cinco artefactos del framework y no están definidos en ningún glosario suyo.

El criterio que debería atrapar esto está replicado en **once de las diecisiete reglas**, y manda a **nueve destinos distintos**. Casi todos son glosarios del producto: `Rules-Examples.md` §6, por ejemplo, exige que «todo término que **esta categoría** acuña o precisa» esté declarado en `Glosario-Funcional.md` de la 02 y en `Glosario-Tecnico.md` de la 11. Pero la categoría no acuña estos términos: los acuña el framework. Cumplir ese criterio al pie de la letra significa que el glosario funcional de un producto de videovigilancia declare qué es una sonda, al lado de qué es un eje de motor.

**Y el framework ya escribió la política correcta, una sola vez y en el lugar donde no hacía falta.** `Rules-Plan-Sprint.md` §6 dice que el vocabulario de proceso «es del framework y vive en el glosario operativo de `Master-Prompt.md` §15, **no en un glosario de producto**». Es exactamente la respuesta. Está enunciada sobre términos que ya estaban resueltos —`sprint`, `Definition of Done`— y no sobre los que no lo están. La regla que sí enfrenta uno de esos, `Rules-Calidad-Y-Pruebas.md`, nombra `sonda` con todas las letras y la manda a definirse **en línea, en el cuerpo del plan de pruebas**, que es un noveno destino y el único que no es un glosario.

## 2. El incidente

La categoría 10 del destino emitió diecinueve documentos que usan cinco términos de forma transversal: `sonda`, `contrato de verificación`, `pasada de diseño`, `salida prometida` y `arnés`. Ninguno está declarado en ninguno de los cinco `Glosario-Funcional.md` del producto.

Al rastrear de dónde venían, aparece esto:

| Término | En cuántos artefactos del framework aparece | ¿Está definido en algún glosario del framework? |
|---|---|---|
| `contrato de verificación` | 5: `Rules-Calidad-Y-Pruebas`, `Rules-Examples`, `Rules-Documentacion`, `Deriva-Rules`, `Master-Prompt` | **Sí**, en `Master-Prompt.md` §15, con remisión a `Rules-Examples.md` §4.6 |
| `sonda` | 5: los mismos cinco | **No** |
| `pasada de diseño` y `pasada de ejecución` | 3: `Rules-Examples`, `Deriva-Rules`, `Master-Prompt` | **No**. Se definen en una tabla de dos filas de `Rules-Examples.md` §0.2 y se usan como término conocido en las otras dos |
| `arnés` | 1: `Rules-Examples.md` §0.1, «arista B de arnés de autovalidación» | **No.** Es una metáfora usada una vez que el destino adoptó como término |
| `salida prometida` | 0 | **No existe en el framework**: lo acuñó el destino para nombrar la salida documentada en §6 de un sample que todavía no tiene código |

Sólo el primero de los cinco está gobernado. El último es genuinamente del destino y su deuda es del destino. Los tres del medio son el hallazgo.

## 3. «Sonda» es la unidad de un mecanismo entero y ningún glosario la define

De los tres, el que más pesa es `sonda`, y conviene medir cuánto.

Es la **unidad elemental del sensado de deriva**. Cada fila de cada `Matriz-Sensado-Deriva.md` es una sonda. Las cinco poblaciones del mecanismo —`SUP-XX`, `CMP-XX`, `EST-XX`, `NAV-XX`, `DM-XX`— son tipos de sonda, y `Rules-Examples.md` §4.6 agrega la sexta, `VER-XX`, con la frase «los contratos de verificación de esta categoría son sondas de la matriz de sensado de deriva». En un solo proyecto de código de este destino hay **376**.

Y sin embargo, **ningún glosario del framework la define**:

- No está entre los seis términos de `Vocabulario-Rules.md` §2.
- No está en las cuarenta y una entradas del glosario operativo de `Master-Prompt.md` §15 —que sí define `Sensado de deriva`, cuya definición no usa la palabra, y `Contrato de verificación`, que **es** una sonda—.
- No está definida en `Deriva-Rules.md`, que es la regla dueña del mecanismo. Ahí la palabra aparece por primera vez ya en uso, y sus dos apariciones más explícitas son un encabezado de tabla y el rótulo de un anti-patrón.

**Un solo lugar del framework se hace cargo, y la manda fuera de todo glosario.** `Rules-Calidad-Y-Pruebas.md` §6 la nombra explícitamente —«los términos de prueba propios de la categoría: nivel, sonda, umbral, fixture»— y ordena que «se definen en el plan de pruebas la primera vez que aparecen». Es decir: el término más central del mecanismo de sensado se define **en línea, en un documento del producto, una vez por cada producto**, y no en ninguno de los glosarios que el framework tiene.

La consecuencia se ve en el destino y está en §4.2: `sonda` quedó definida en la categoría 08 y sin definir en la 10, y las dos categorías cumplieron su regla.

Un mecanismo cuya unidad se define por repetición en cada destino es un mecanismo que se transmite por imitación. Que hasta ahora todos hayan entendido lo mismo no es una propiedad del método: es suerte, sostenida por lo transparente que resulta la metáfora.

## 4. Nueve destinos distintos para la misma pregunta

El criterio de gobierno de glosario está replicado en **once de las diecisiete reglas**, casi siempre con la misma primera oración —«Todo término que esta categoría acuña o precisa, y que aparece en más de uno de sus artefactos, está declarado en…»— y con una segunda oración distinta en cada una que dice **dónde**. Esa segunda oración es el problema: no hay dos iguales, y en dos casos se contradicen sobre la misma clase de término.

Esta es la tabla completa, que es el insumo principal de este reporte. Las citas son textuales, todas de la sección de criterios de aceptación de cada regla.

| Regla | A dónde manda declarar | Cita textual de la segunda oración |
|---|---|---|
| `Rules-Contexto.md` | `Vision-Producto.md` §9, glosario del dominio del cliente | «Es el glosario raíz de la cadena: 02 y 03 referencian sus términos en lugar de redefinirlos» |
| `Rules-Necesidades-Negocio.md` | `Vision-Producto.md` §9 de 00 | «Esta categoría consume el vocabulario del negocio y no acuña uno propio» |
| `Rules-Arquitectura-Tecnica.md` | `Glosario-Tecnico.md` de 11 **y** `Glosario-Funcional.md` de 02, con reparto declarado | «…de 11 para el vocabulario técnico, y `Glosario-Funcional.md` de 02 para los términos de dominio que reusa» |
| `Rules-Backlog-Tecnico.md` | `Glosario-Funcional.md` de 02 **y** `Glosario-Tecnico.md` de 11 | «Las US y los BT no acuñan vocabulario propio: reusan el de 02 y el de 05» |
| `Rules-Plan-Sprint.md` | Los dos glosarios del producto… **y después se desdice** | «El vocabulario de proceso de esta categoría —sprint, incremento, velocidad, Definition of Done— **es del framework y vive en el glosario operativo de `Master-Prompt.md` §15, no en un glosario de producto**» |
| `Rules-Calidad-Y-Pruebas.md` | Los dos glosarios del producto… **y después manda a un tercer lado** | «Los términos de prueba propios de la categoría —nivel, **sonda**, umbral, fixture— **se definen en el plan de pruebas la primera vez que aparecen**» |
| `Rules-Devops.md` | Sólo `Glosario-Tecnico.md` de 11 | «…se declaran una sola vez en el glosario técnico, no en cada pipeline» |
| `Rules-Examples.md` | `Glosario-Funcional.md` de 02 **y** `Glosario-Tecnico.md` de 11 | «Un sample no acuña vocabulario: si necesita un término que no está declarado, el defecto está aguas arriba» |
| `Rules-Documentacion.md` | `Glosario-Tecnico.md` **que esta categoría emite** | «Es la fuente canónica del vocabulario técnico del producto» |
| `Rules-Prompts-AI.md` | `Glosario-Funcional.md` de 02 **y el propio contrato de prompt** | «…de 02 para los términos de dominio, y el propio contrato de prompt para los términos de la interacción con el modelo» |
| `Root-Rules.md` | El glosario rápido del README raíz **y los glosarios de categoría que referencia** | «…que esta regla ya exige con mínimo de diez términos, y los glosarios de categoría que referencia» |

Y dos reglas de categoría —`Rules-Especificacion-Funcional.md` y `Rules-UX-UI-DX.md`— **no tienen el criterio en absoluto**, pese a que la 02 es la que emite el `Glosario-Funcional.md` al que las otras nueve apuntan.

### 4.1 Las dos filas que importan, enfrentadas

De las once, dos se plantan sobre exactamente la misma pregunta —qué hacer con el vocabulario **del método**, no del negocio— y contestan cosas incompatibles.

**`Rules-Plan-Sprint.md` §6 contesta bien, y con todas las letras:**

> «El vocabulario de proceso de esta categoría —sprint, incremento, velocidad, Definition of Done— es del framework y vive en el glosario operativo de `Master-Prompt.md` §15, no en un glosario de producto.»

Esa oración es la política correcta, ya redactada, ya dentro del framework. Distingue vocabulario del método de vocabulario del producto, nombra el artefacto donde va el primero, y prohíbe explícitamente mandarlo al segundo.

**`Rules-Calidad-Y-Pruebas.md` §6, ante la misma clase de término, contesta otra cosa:**

> «Los términos de prueba propios de la categoría —nivel, sonda, umbral, fixture— se definen en el plan de pruebas la primera vez que aparecen.»

`sonda`, `umbral` y `fixture` son tan del método como `sprint` y `velocidad`. Pero acá no van al glosario operativo: van **en línea, en el cuerpo de un documento del producto, en su primera aparición**. Es un noveno destino, distinto de los ocho anteriores, y es el único que no es un glosario.

Nótese que la regla que contesta bien lo hace **sobre términos que además están en el glosario operativo** —`Definition of Done` y las demás sí figuran ahí—, mientras que la que contesta mal lo hace sobre términos que **no** están. Es decir: la política correcta se enunció donde ya estaba resuelta, y no donde hacía falta.

### 4.2 Qué le pasa a un destino con esto

No es teórico. Este destino chocó con la misma pregunta **dos veces**, en dos fases distintas, y la resolvió de dos maneras distintas, porque las dos reglas le dijeron cosas distintas.

**En la Fase E**, la categoría 08 de los cinco proyectos de código definió cinco términos —nivel, capa, sonda, montaje, umbral— en una tabla dentro de `Plan-Pruebas.md`, siguiendo a `Rules-Calidad-Y-Pruebas.md`, y dejó escrito el conflicto:

> «La regla de la categoría pide que todo término que ésta acuñe y use en más de un artefacto esté declarado en el glosario funcional de la 02 y en el glosario técnico de la 11. **El de la 11 no existe**: esa categoría se planifica en la Fase H. Y el de la 02 es de vocabulario del dominio del producto —recurso, set, mapa del equipo—, no de vocabulario de método.»

**En la Fase G**, la categoría 10 de los mismos cinco proyectos de código usó `sonda` otra vez —y `contrato de verificación`, `pasada de diseño` y `arnés`— y **no las definió en ningún lado**, porque `Rules-Examples.md` §6 manda a dos glosarios de los cuales uno no existe y el otro es del dominio del negocio, y a diferencia de la regla de calidad no ofrece la salida en línea.

El resultado es que **la misma palabra, en el mismo producto, está definida en la categoría 08 y sin definir en la categoría 10**, y las dos categorías cumplieron su regla. Un lector que llegue a un sample por su contrato de verificación no tiene forma de saber que la definición de `sonda` que necesita está en el plan de pruebas de otra categoría.

### 4.3 Lo que cada respuesta implica, para quien tenga que elegir una

La intervención va a tener que elegir, y las opciones no son equivalentes. Esto es lo que cada una arrastra:

| Si la respuesta es… | Qué exige | Qué rompe hoy |
|---|---|---|
| **El glosario operativo del `Master-Prompt.md` §15** | Que el framework complete su propio glosario. Hoy tiene 41 entradas y le faltan al menos `sonda`, `pasada de diseño` y `pasada de ejecución` | Nada del producto. Es la única opción que no toca la documentación generada, y la única que ya está enunciada en una regla |
| **`Glosario-Tecnico.md` de la 11** | Que exista antes que sus consumidores. Hoy la 11 se planifica en la Fase H y las categorías 05, 06, 07, 08, 09 y 10 la referencian desde las Fases C a G | Seis categorías apuntan a un artefacto de una fase posterior: es el patrón del reporte `07`, acá con seis instancias más |
| **`Glosario-Funcional.md` de la 02** | Que el glosario del dominio del cliente admita vocabulario del método | Mezcla `sonda` con `eje de motor` en el artefacto que lee el Product Owner. Ninguna regla dice que esto sea deseable, y `Rules-Contexto.md` lo llama «el glosario del dominio del cliente» |
| **Un glosario propio por categoría** | Crear el artefacto. Hoy `Master-Prompt.md` §15 lo **define** como concepto —«artefacto propio de una categoría»— y ninguna regla de categoría lo emite | El concepto está definido en el glosario operativo y no existe en ninguna tabla maestra de artefactos: es vocabulario del framework sin materialización |
| **En línea, en la primera aparición** | Nada: es lo que hace hoy la 08 | Duplica la definición en cada categoría que use el término, que es exactamente lo que la entrada «Glosario de categoría» del glosario operativo prohíbe: «la regla de no duplicación manda referenciar el término ya declarado por otra categoría en lugar de redefinirlo» |

**La recomendación de este reporte, para que el análisis no arranque de cero:** las dos primeras filas no compiten, se complementan. El vocabulario **del método** va al glosario operativo del master-prompt, que es lo que `Rules-Plan-Sprint.md` ya dice; el vocabulario **del producto** va al `Glosario-Tecnico.md` de la 11, que es lo que dice `Rules-Documentacion.md`. Lo que falta es la frase que separa las dos clases, y está escrita una sola vez, en la regla equivocada para que se generalice sola.

## 5. El criterio manda declarar en el glosario del producto lo que acuñó el método

Resuelta la inconsistencia de §4, quedaría todavía el problema de fondo, y es el que no tiene salida buena para un destino.

El criterio dice «todo término que **esta categoría** acuña o precisa». `sonda` y `pasada de diseño` no los acuña la categoría 10 de un producto: los acuña el framework, y la categoría los usa porque el método se los impone. Las dos lecturas posibles dan dos malos resultados:

| Lectura | Qué produce |
|---|---|
| **Sí aplica**: el producto los declara | El glosario funcional de un producto de videovigilancia —que lo lee el Product Owner, y que declara qué es un eje de motor, un set y una fuente de imagen— pasa a declarar qué es una sonda y qué es una pasada de diseño. Vocabulario del método mezclado con el del negocio, en el artefacto donde menos corresponde. Y contradice la regla de no duplicación que el propio framework enuncia en la entrada «Glosario de categoría» de su glosario operativo: se estaría redefiniendo un término que el método ya posee |
| **No aplica**: son del método, no del producto | Los términos quedan sin declarar en ningún lado que el lector del producto pueda alcanzar, que es exactamente el estado que el criterio existe para evitar. Y el criterio no dice esto en ninguna parte: hay que deducirlo de una oración que vive en otra regla |

El destino no puede elegir ninguna de las dos sin empeorar algo. Es la misma forma de los reportes `06` y `07`: las dos salidas disponibles son peores que declarar el apartamiento.

**Lo que cierra este apartado, y es la observación que más orienta la corrección:** la frase que resuelve esta ambigüedad **existe**, en `Rules-Plan-Sprint.md` §6, y dice que el vocabulario del método no va en un glosario de producto. Que esté en una sola de las once reglas no es un descuido de redacción: es que se escribió al resolver un caso concreto —el vocabulario de proceso, que ya estaba en el glosario operativo— y nadie preguntó si el criterio de al lado tenía el mismo problema.

## 6. La causa raíz

`Vocabulario-Rules.md` nació para resolver un problema concreto y acotado: el renombre de «solución» a **producto** y de «proyecto» a **proyecto de código**, que era donde el framework se pisaba consigo mismo. Resolvió ese problema y quedó con ese alcance —seis términos, su correspondencia de industria, y la precedencia frente al dominio del cliente—.

Lo que no ocurrió fue la generalización: nadie preguntó **cuánto vocabulario propio más tiene el framework**. El resultado es que el método gobierna con cuidado las seis palabras que se chocaban con el negocio, y deja sin gobernar todas las que inventó él mismo y con las que el negocio no se choca. Que no haya colisión no significa que no haga falta definirlas: significa que el defecto es silencioso.

`§8 Pendiente declarado` de esa misma regla lo muestra desde otro ángulo: ahí se registra que la **unidad de entrega** está definida pero todavía no es un nivel del layout. La regla sabe que su alcance está inconcluso; lo que no registra es que hay vocabulario suyo que ni siquiera entró.

## 7. El patrón, enunciado

> **El framework acuña vocabulario propio muy por fuera de los seis términos que su regla de vocabulario gobierna, no lo declara en ningún glosario suyo, y el criterio con el que exige declarar vocabulario está replicado en once reglas que mandan a nueve destinos distintos, casi todos glosarios del producto. La consecuencia es que el vocabulario del método viaja a la documentación del cliente sin definición, o se define en un artefacto de negocio donde no corresponde, o se redefine en línea una vez por categoría, y las tres salidas son malas.**

Tres corolarios, que son los que orientan la corrección:

> **La política correcta ya está escrita, una sola vez y en la regla equivocada.** `Rules-Plan-Sprint.md` §6 dice que el vocabulario de proceso «es del framework y vive en el glosario operativo de `Master-Prompt.md` §15, no en un glosario de producto». No hay que redactar una política: hay que generalizar una que existe. Está enunciada sobre términos que ya estaban en ese glosario, que es por qué nadie notó que faltaba aplicarla a los que no.

> **El caso más caro es el de la unidad de un mecanismo.** `sonda` no es un término de color: nombra aquello de lo que hay 376 en un solo proyecto de código, y de lo que el mecanismo de sensado entero se compone. La única regla que se hace cargo de ella la manda a definirse en línea, una vez por producto, fuera de todo glosario.

> **La inconsistencia se propaga al destino y produce documentación incoherente consigo misma.** No es una discrepancia entre reglas que un destino resuelve una vez: es una discrepancia que el destino **replica**, porque cada categoría obedece a su regla. En este destino la misma palabra quedó definida en la categoría 08 y sin definir en la 10, con las dos categorías en cumplimiento.

## 8. Qué hizo el destino

Emitió **`SDD/Docs/Producto/Glosario-Metodo.md`**, un glosario del vocabulario del método a nivel producto, y conectó a él las dos categorías que habían resuelto la misma pregunta de dos maneras distintas.

Es **un apartamiento declarado**, y su primera sección dice por qué: las tablas maestras de las reglas de categoría no prevén un glosario a nivel producto, y los dos destinos a los que mandan no sirven para este contenido —el `Glosario-Tecnico.md` de la 11 no existe hasta la Fase H, y el `Glosario-Funcional.md` de la 02 es del dominio del cliente—.

Qué contiene y qué resuelve:

- **Diecisiete términos**, con una tercera columna que declara, término por término, **si el framework lo define y dónde**. Cuatro los define el glosario operativo del master-prompt y se citan sin redefinir; los otros trece no los define ningún glosario del framework. Esa columna es, de paso, la evidencia reproducible de este reporte.
- **La polisemia de `montaje` y `umbral` resuelta.** Las dos palabras tienen un sentido de método y otro del dominio, y las dos acepciones conviven en los mismos documentos: `montaje` es el estado inicial de una prueba y también el armado físico del equipo; `umbral` es el mínimo que detiene la construcción y también el umbral de alarma de un dispositivo. El glosario declara la regla de desambiguación en lugar de reescribir el corpus, siguiendo el criterio de `Vocabulario-Rules.md` §9.
- **Una sección de qué no va acá**, con los tres destinos que sí tienen dueño: el vocabulario del negocio a la 02, el técnico del producto a la 11, el de interfaz a la 03. Sin eso, un glosario así se convierte en el cajón donde va todo.
- **La categoría 08 dejó de definir** `sonda`, `montaje` y `umbral` en su plan de pruebas y ahora los cita. Conserva `nivel` y `capa`, que **no** son del método: los fija cada proyecto de código en su estrategia de testing y no tienen valor único a nivel producto. Los cinco planes registraron el cambio en su control de cambios.
- **La categoría 10 dejó de tener la deuda abierta.** Su sección de lo que la pasada de diseño no puede cumplir tenía una fila para esto; ahora está tachada y resuelta.

**Y una comprobación mecánica que impide que se deshaga solo.** La compuerta del destino verifica que ningún documento fuera del glosario vuelva a definir uno de sus términos, buscando la forma en que una definición se escribe en este corpus —una fila de tabla que abre con el término en negrita—. Se probó reintroduciendo a mano la definición retirada: la comprobación la marca, y sin ella no produce ningún aviso. Sin eso, la próxima categoría que necesite explicar qué es una sonda la define de nuevo, y vuelve el problema con dos definiciones que divergen.

**Qué no resuelve.** El destino arregló su propio corpus; no arregló la regla. La próxima corrida sobre otro producto va a encontrar las mismas once reglas mandando a los mismos nueve destinos, y va a tener que inventar su propio remedio. Y el glosario mismo está escrito para desaparecer: si la intervención adopta la política que `Rules-Plan-Sprint.md` §6 ya enuncia, se reduce a una tabla de remisiones al glosario operativo.

## 9. Propuestas de intervención

Ninguna está decidida; son punto de partida. Están ordenadas de la más barata a la más profunda, y las dos primeras resuelven el incidente concreto.

1. **Generalizar la frase de `Rules-Plan-Sprint.md` §6 a las once reglas.** Es una oración que ya existe, ya está redactada y ya distingue las dos clases de vocabulario. Convertirla en la primera cláusula del criterio de gobierno de glosario —«el vocabulario del método va al glosario operativo; el del producto, al glosario que corresponda»— resuelve la ambigüedad sin inventar nada. Es la intervención de menor costo y mayor alcance.
2. **Completar el glosario operativo de `Master-Prompt.md` §15.** Empezando por `sonda`, `pasada de diseño` y `pasada de ejecución`, que son los tres que este destino necesitó y no encontró. Sin esto, la propuesta 1 manda a un glosario que no los tiene.
3. **Inventariar el vocabulario propio del framework.** Recorrer las diecisiete reglas y el master-prompt buscando términos usados como técnicos que no estén ni en los seis de `Vocabulario-Rules.md` §2 ni en las cuarenta y una entradas del glosario operativo. Es una pasada mecánica y produce la lista completa en lugar de los tres casos que una corrida encontró.
4. **Unificar los nueve destinos.** La tabla de §4 es el inventario de partida. Hay que decidir, en particular: si el `Glosario-Tecnico.md` de la 11 es el destino del vocabulario técnico del producto, seis categorías lo referencian desde fases anteriores a la suya, y eso hay que resolverlo con la reapertura que propone el reporte `07`; y si el «glosario de categoría» que define el glosario operativo es un artefacto real, alguna regla tiene que emitirlo, porque hoy no lo emite ninguna.
5. **Cerrar los dos huecos.** `Rules-Especificacion-Funcional.md` y `Rules-UX-UI-DX.md` no tienen el criterio, y la primera es la que emite el `Glosario-Funcional.md` al que apuntan nueve de las once restantes.
6. **Revisar el alcance de `Vocabulario-Rules.md`.** Hoy gobierna seis términos y se presenta como la regla de vocabulario. O amplía su alcance a todo el vocabulario del framework, o declara —como ya hace en su §8 con la unidad de entrega— que sólo gobierna los términos que colisionan con el dominio del cliente, y entonces hace falta decir quién gobierna el resto.

## 10. Cómo verificar que la corrección funcionó

- El inventario de la propuesta 3 existe y cubre las diecisiete reglas y el master-prompt.
- Ningún término que el framework usa como técnico en más de un artefacto queda sin definición en un glosario del framework.
- Una búsqueda de `sonda` en los artefactos del framework encuentra su definición antes que su primer uso.
- Las once reglas que llevan el criterio de gobierno de glosario dan **la misma** respuesta a la pregunta «¿dónde va un término que el método acuñó?», y esa respuesta no es un glosario del producto.
- Ninguna regla manda definir un término en línea, en el cuerpo de un documento, como alternativa a un glosario.
- Ninguna regla apunta a un glosario que emite una categoría de una fase posterior, o el apuntamiento declara su estado pendiente y su cierre.
- Dos categorías del mismo producto que usen el mismo término del método lo referencian al mismo lugar, y ninguna lo redefine.
- Ningún destino tiene que decidir, para cumplir un criterio de aceptación, si el vocabulario del método va o no en el glosario que lee el Product Owner.

## Control de cambios

| Versión | Fecha | Descripción |
| --- | --- | --- |
| 1.0 | 2026-08-12 | Reporte inicial, emitido durante la Fase G a partir de una pregunta de revisión humana sobre cinco términos que la categoría 10 usaba sin declarar. Documenta que `Vocabulario-Rules.md` gobierna seis términos y que el framework acuña bastante más; que `sonda`, unidad del sensado de deriva y nombre de las 376 filas de una matriz, no está definida en ninguno de sus glosarios; que el framework da tres respuestas distintas sobre dónde vive el vocabulario de una categoría; y que el criterio de `Rules-Examples.md` §6 manda al glosario del producto términos que acuñó el método, con dos lecturas posibles y las dos malas. |
| 1.1 | 2026-08-12 | Se amplía §4 a pedido de una revisión humana que pidió evidencia y consistencia suficientes para que el análisis de corrección no arranque de cero. El criterio de gobierno de glosario resultó estar replicado en once de las diecisiete reglas, mandando a nueve destinos distintos, con cita textual de cada uno. Aparecen dos hallazgos que la primera versión no tenía: `Rules-Plan-Sprint.md` §6 **ya enuncia la política correcta** —el vocabulario del método va al glosario operativo y no a uno de producto— sobre términos que ya estaban resueltos; y `Rules-Calidad-Y-Pruebas.md` §6 nombra `sonda` explícitamente y la manda a definirse en línea en el plan de pruebas, que es un noveno destino y el único que no es un glosario. Nueva §4.2 con la evidencia de que la inconsistencia se replica en el destino: la misma palabra quedó definida en la categoría 08 y sin definir en la 10, con las dos en cumplimiento. Nueva §4.3 con lo que cada respuesta posible exige y rompe, y una recomendación de partida. §1, §3, §7, §9 y §10 se reescriben para ser consistentes con la evidencia nueva. |
| 1.2 | 2026-08-12 | §8 pasa de «nada todavía» a documentar el remedio local que el destino aplicó, a pedido del Product Owner, para poder seguir: un `Glosario-Metodo.md` a nivel producto, declarado como apartamiento, con diecisiete términos y una columna que dice cuáles define el framework y cuáles no; la polisemia de `montaje` y `umbral` resuelta; las categorías 08 y 10 citándolo en lugar de definir; y una comprobación mecánica, probada contra el defecto, que impide que otro documento vuelva a redefinir uno de sus términos. |
