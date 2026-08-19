# Reportes de evidencia sobre el Framework SDD

**Documento:** README.md
**Versión:** 1.11
**Fecha:** 2026-08-18
**Estado:** Vigente — los doce primeros **RESUELTOS**; el `12`, el `13` y el `14` **para evaluación**
**Cierre:** aplicados sobre el framework en **SDD 7.0** (2026-08-15), como una sola intervención sobre los doce

Índice de los reportes de hallazgos contra el `Framework SDD`, escritos para ser **insumo de prompts de intervención sobre el framework**. Ninguno modifica el framework: cada uno documenta un hueco con evidencia reproducible, para que la corrección se decida con datos y no con impresiones.

## Cómo se usan

Un prompt que vaya a intervenir el framework cita el reporte por su número y usa sus secciones así:

| Sección del reporte | Para qué sirve en el prompt |
|---|---|
| Los incidentes | Evidencia de que el hueco produce defectos reales, no hipotéticos |
| Lo que la normativa dice y lo que no dice | Delimita qué no hay que reescribir. Todos los reportes son explícitos sobre lo que el framework ya resuelve bien |
| La causa raíz | Impide que la corrección ataque el síntoma |
| **El patrón, enunciado** | Es lo que hay que corregir. Está escrito en forma general a propósito, para que la intervención no se limite al caso que lo reveló |
| Propuestas de intervención | Punto de partida, no decisión tomada |
| Cómo verificar que la corrección funcionó | Criterio de aceptación de la intervención |

## Los reportes

| # | Título | Patrón que captura | Origen |
|---|---|---|---|
| [00](00-Regla-Transcripción.md) | Regla de transcripción | El framework exige transcribir de fuentes y no declara qué obliga la transcripción; una corrección en la fuente no llega al artefacto que la copió | Fase A |
| [01](01-Ambito-De-Unicidad-De-Identificadores.md) | Ámbito de unicidad de los identificadores | El framework declara la forma de sus identificadores y no su ámbito de unicidad; en un producto multiproyecto alguien decide sin saber que hay una decisión que tomar | Fase B |
| [02](02-Propagacion-De-La-Fase-B2.md) | Propagación de la Fase B2 | Una fase iterativa con propagación puntual acumula distancia entre lo aprobado y lo documentado; y una matriz de propagación cerrada convierte cada caso no previsto en omisión silenciosa | Fase B2 |
| [03](03-Conjuntos-Cerrados-Entre-Categorias.md) | Conjuntos cerrados entre categorías | Dos categorías pueden afirmar cosas incompatibles sobre el mismo conjunto; el framework tiene trazabilidad pero no arbitraje, y ningún audit cruza categorías buscando contradicciones | Fase B2 |
| [04](04-Recuentos-Declarados-En-Prosa.md) | Recuentos declarados en prosa | El framework escribe datos derivados con la misma forma que los datos declarados, no exige que nombren su fuente y no los verifica; el defecto es silencioso y se detecta por casualidad | Fases A, B y B2 |
| [05](05-Ancho-De-Los-Identificadores.md) | Ancho de los identificadores | El framework fija el ancho como convención de forma, sin declarar de qué colección es función ni qué hacer al agotarse el rango; la tabla que define como derivada de todas las otras es la que con más seguridad desborda | Fase B2 |
| [06](06-Obligatoriedad-Por-Tipo-Sin-Condicion.md) | Obligatoriedad por tipo sin condición | El framework decide qué artefactos son obligatorios por el tipo del proyecto, que describe su forma, cuando lo que la determina es una responsabilidad que en un producto multiproyecto se reparte; las dos salidas que deja para cumplir la letra producen peor documentación que el incumplimiento | Fase C |
| [07](07-Obligacion-Hacia-Una-Fase-Posterior.md) | Obligación hacia una fase posterior | Una regla de categoría obliga a referenciar un artefacto que otra categoría emite en una fase posterior, hasta cinco más adelante; el framework ordena las fases por dependencia de contenido y no comprueba ese orden contra las obligaciones que sus propias reglas declaran | Fases D y E |
| [08](08-El-Sprint-Es-Del-Equipo-Y-La-Categoria-Es-Del-Proyecto.md) | La iteración es del equipo y la categoría es del proyecto de código | El framework declara el nivel de aplicación por categoría; cuando una categoría contiene artefactos de dos niveles, los del nivel equivocado se replican por proyecto de código y dejan de medir lo que su nombre dice | Fase D |
| [09](09-El-Audit-Como-Unica-Compuerta.md) | El audit como única compuerta | El framework verifica cada fase con un solo instrumento —un lector independiente— y lo aplica igual a propiedades enumerables y a propiedades interpretativas; las primeras consumen la atención que sólo las segundas necesitan, y el método no declara criterio de corte para las rondas | Fase E |
| [10](10-Criterios-Que-Se-Satisfacen-Trivialmente.md) | Criterios que se satisfacen trivialmente | Los criterios de aceptación preguntan si una declaración está presente y no si es verdadera; el único que pregunta por la relación cuenta lo que falta, de modo que una declaración falsa lo ayuda a cumplirse | Fase G |
| [11](11-El-Vocabulario-Del-Metodo-No-Tiene-Glosario.md) | El vocabulario del método no tiene glosario | El framework acuña vocabulario propio fuera de los seis términos que su regla gobierna y no lo declara en ningún glosario suyo; el criterio que exige declararlo está replicado en once reglas que mandan a nueve destinos distintos, y una de ellas ya enuncia la política correcta sobre términos que no la necesitaban | Fase G |
| [12](12-La-Compuerta-Declarada-Y-La-Compuerta-Ejecutada.md) | La compuerta declarada y la compuerta ejecutada | El método declara una compuerta y no comprueba que se haya corrido; una compuerta que nadie ejecuta se lee igual que una que pasó | Migración de un destino real |
| [13](13-El-Estrato-Del-Hallazgo-Y-La-Legitimidad-De-La-Detencion.md) | El estrato del hallazgo y la legitimidad de la detención | El framework clasifica **cómo** se verifica un hallazgo y no **quién** puede cerrarlo; medido en una corrida real, tres de cinco detenciones llevadas al humano no eran del humano | Fase M6 de una migración real |
| [14](14-El-Item-Obligatorio-Contestado-Con-Un-Diferimiento.md) | El ítem obligatorio contestado con un diferimiento | Un ítem de contenido obligatorio puede contestarse con la promesa de contestarlo; la promesa se lee igual que el dato, se ata a un evento del producto y **nada comprueba ese evento cuando ocurre** | Tercera reanudación de un destino real |

**Los tres últimos están fuera del cierre de SDD 7.0** y de la lectura común de más abajo, que se
escribió sobre los doce primeros. El `12`, el `13` y el `14` se incorporaron al índice el 2026-08-18,
**después de haber sido escritos**: los dos primeros llevaban tiempo en la carpeta sin fila acá, que
es la misma clase de defecto que varios de ellos describen.

## Lo que los doce tienen en común

No es casualidad que los doce sean del mismo tipo. **Ninguno es un error de un agente**: en los doce, los agentes cumplieron la regla que tenían, o la única que había no se podía cumplir sin empeorar el resultado. Son huecos del método, y comparten una forma:

> El framework declara **qué** hay que producir y con **qué forma**, y con menos frecuencia declara **qué propiedad tiene que conservarse** cuando eso que produjo cambia, se copia, se cuenta o entra en conflicto con otra cosa que también produjo.

Dentro de esa forma común hay cuatro familias, y distinguirlas importa porque cada una se corrige de otra manera.

**Cinco aparecen al modificar y no al escribir** —los reportes `00` a `04`—. Son los que más deberían pesar en cualquier intervención, porque su defecto es invisible en la primera pasada: el artefacto se emite bien y se rompe después, cuando su fuente cambia, cuando alguien lo copia, cuando el recuento que declara deja de coincidir o cuando otra categoría afirma lo contrario.

**Tres aparecen al escribir, y son el mismo error tres veces**: un atributo fijado sin declarar de qué depende, derivado de la forma cuando lo determinaba otra cosa. El `05` fijó el ancho de un identificador sin declarar de qué colección es función. El `06` fijó la obligatoriedad de un artefacto sin declarar de qué responsabilidad es función. El `08` fijó el nivel de aplicación sin declarar de qué habla el artefacto, y lo derivó de en qué categoría está. Los tres se corrigen igual: declarando la dependencia.

**Uno es un conflicto entre reglas.** El `07` no es un atributo mal derivado sino una regla local escrita sin mirar el conjunto: cada regla de categoría es correcta leída sola, y el conflicto está entre ellas y el plan que las ordena. Su corrección no vive dentro de ninguna de las reglas involucradas.

**Y los dos últimos son del instrumento con que el método se mira a sí mismo.** El `09` no habla de lo que el framework produce sino de cómo lo verifica: una sola compuerta, un solo instrumento, y ninguna distinción entre lo que se decide contando y lo que se decide leyendo. Es el único de los doce que se descubrió no al escribir ni al modificar, sino **al auditar tres veces la misma fase y ver que el rendimiento no llegaba a cero**.

El `10` es el otro lado de esa misma cuestión: el `09` habla del **instrumento** —quién mira— y el `10` del **criterio** —qué se le pide que confirme—. Un criterio que se cumple mejor con una declaración falsa que con una honesta no lo arregla ningún instrumento.

Cuatro de los doce salieron de la **Fase B2**, y tampoco es casualidad: es la primera fase que produce información que sólo puede nacer aguas abajo, y por lo tanto la primera en que el método se encuentra con lo que no previó. Dos salieron de la **Fase D**, que es la primera en que el método tiene que hablar del equipo y del calendario y no sólo de los artefactos. El `07` siguió creciendo en la **Fase E**, que le aportó un cuarto incidente: cada fase nueva encuentra otra obligación apuntando hacia adelante, y no hay motivo para suponer que la lista esté cerrada.

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 2026-08-10 | Índice inicial, con los cinco reportes emitidos hasta la fecha, la guía de uso para un prompt de intervención y la propiedad común a los cinco. |
| 1.1 | 2026-08-11 | Se incorpora el reporte `05`, emitido al cerrar la Fase B2: el ancho de los identificadores. Se ajusta la sección de lo común: el `05` es el único de los seis que aparece al escribir y no al modificar, y cuatro de los seis salieron de la Fase B2. |
| 1.2 | 2026-08-11 | Se incorpora el reporte `06`, emitido al cerrar la Fase C: la obligatoriedad de un artefacto decidida por el tipo del proyecto. Se ajusta la sección de lo común: son dos, y no uno, los reportes que aparecen al escribir, y los dos comparten la misma forma —un atributo fijado sin declarar de qué depende, derivado de la forma cuando lo determinaba otra cosa—. |
| 1.3 | 2026-08-11 | Se incorporan los reportes `07` y `08`, emitidos al cerrar la Fase D. Se reescribe la sección de lo común: las nueve entradas se agrupan ahora en tres familias —las cinco que aparecen al modificar, las tres que fijan un atributo sin declarar de qué depende y la única que es un conflicto entre reglas y el plan que las ordena—. Se corrigen dos recuentos que habían quedado desactualizados en la versión anterior, que es el defecto que el propio reporte `04` documenta. |
| 1.4 | 2026-08-11 | Corrección de la segunda ronda del audit: la fila del reporte `07` conservaba la medida «dos fases más tarde» que ese mismo reporte ya había corregido, de modo que el índice contradecía al documento que indexa. |
| 1.5 | 2026-08-11 | Correcciones del audit de la Fase E: la cabecera declaraba 1.3 con una fila 1.4 posterior, las dos últimas filas estaban en orden invertido, y la fila del reporte `07` seguía declarando la Fase D como único origen y «hasta cuatro fases» como distancia máxima. Se ajusta también la sección de lo común, que daba por cerrada una lista que la Fase E amplió. |
| 1.6 | 2026-08-12 | Se incorpora el reporte `09`, emitido al cerrar la Fase E tras tres rondas de audit: el audit como única compuerta de fase, sin ninguna comprobación mecánica delante. Se agrega una cuarta familia a la sección de lo común, para el hallazgo que no es sobre lo que el método produce sino sobre cómo lo verifica. |
| 1.7 | 2026-08-12 | Actualización tras la Fase G, sin reportes nuevos. El reporte `06` suma una segunda instancia de su patrón, en `Rules-Examples.md`: la categoría es obligatoria para `library` porque «el integrador la necesita», y los cuatro `library` de este producto tienen `redistribuible` en `false` en el manifiesto, campo que la regla no lee. El reporte `07` registra el desenlace de su cuarto incidente: las matrices de sensado faltantes se emitieron, pero las emitió el generador de otra categoría, de modo que la reapertura que ese reporte propone tiene que devolver el insumo y no sólo el turno. Se decidió no abrir reportes nuevos: los dos hallazgos son instancias de patrones ya enunciados, y contarlos aparte inflaría el recuento sin agregar un patrón. |
| 1.8 | 2026-08-12 | Se incorpora el reporte `10`, emitido al cerrar la primera ronda del audit de la Fase G: tres contratos de verificación nombraban seis casos de uso que su comando no ejercita, y cumplían todos los criterios de aceptación de su regla. El patrón que documenta es que los criterios preguntan por la presencia de una declaración y no por su verdad, con el agravante de que el único que pregunta por la relación cuenta casos de uso sin cubrir, de modo que una sonda mentirosa lo acerca a cumplirse en vez de alejarlo. El propio framework nombró este patrón en el control de cambios de esa regla, a propósito del glosario, y lo reparó sin generalizarlo. |
| 1.9 | 2026-08-12 | Se incorpora el reporte `11`, emitido durante la Fase G a partir de una pregunta de revisión humana: de dónde salían cinco términos que la categoría 10 usaba sin declarar. Cuatro son del framework y sólo uno está definido en un glosario suyo; `sonda`, que nombra la unidad del sensado de deriva y las 376 filas de una matriz, no está definida en ninguno. El framework además da tres respuestas distintas sobre dónde vive el vocabulario de una categoría. |
| 1.10 | 2026-08-12 | El reporte `11` pasa a v1.1 tras una revisión humana que pidió evidencia suficiente para el análisis de corrección. Su §4 pasa de una tabla de tres filas a un inventario de las once reglas que llevan el criterio, con cita textual, los nueve destinos a los que mandan, la evidencia de que la inconsistencia se replica en el destino, y lo que cada respuesta posible exige y rompe. Se actualiza la fila del índice. |
| 1.11 | 2026-08-17 | Suma el **reporte 12**, primero emitido después del cierre de los doce originales. La cabecera distingue los RESUELTOS del que está para evaluación. |


---

## Reporte 12, abierto

**El circuito volvió a usarse.** El `12` se emitió el 2026-08-17 sobre la compuerta mecánica: está
declarada en prosa y **su ejecución depende del agente que la lee**, así que no es reproducible, no
deja evidencia estructurada y su cobertura no se puede medir.

Propone evaluar un **verificador**, y declara que la decisión **no es de implementación sino de alcance
del método**: hoy el framework no contiene código ejecutable —verificable con `find SDD -type f -not
-name '*.md'`, que devuelve cero—, y agregarlo cambia qué significa versionar el conjunto, cómo se
audita, quién lo mantiene y dónde vive.

**Se emite sin conclusión a propósito**, con las cuatro mediciones que faltan para decidir.

| # | Reporte | Estado |
|---|---|---|
| 12 | [La compuerta declarada y la compuerta ejecutada](12-La-Compuerta-Declarada-Y-La-Compuerta-Ejecutada.md) | **Para evaluación** |
| 13 | [El estrato del hallazgo y la legitimidad de la detención](13-El-Estrato-Del-Hallazgo-Y-La-Legitimidad-De-La-Detencion.md) | **Para evaluación** |

---

## Cierre de los doce reportes

**Los doce se aplicaron sobre el framework en una sola intervención, la SDD 7.0**, y cada uno lleva al
final su sección «Cómo se resolvió» con dónde quedó escrito y qué pasó después. La nota de coherencia
de esa intervención es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Por qué se trataron juntos y no de a uno.** Los doce salieron de la misma corrida sobre el mismo
destino, y **alcanzaban artefactos compartidos**: `Root-Rules.md` aparece en cinco de ellos y
`Master-Prompt.md` en seis. Aplicarlos por separado habría producido seis versiones sucesivas del
mismo archivo, cada una sin ver lo que la siguiente iba a cambiar.

**La propiedad que tenían en común, y que los volvió insumo del método.** Ninguno era un error de un
agente: en los doce, **el agente cumplió la regla que tenía**, o la única que había no se podía cumplir
sin inventar. Un reporte donde el agente se equivocó se corrige en el destino; uno donde el agente
acertó y el resultado igual está mal **se corrige en el framework**, y por eso estos doce llegaron acá.

**Cinco de los doce siguieron dando trabajo después de la 7.0**, y está anotado en cada uno: el `01` en
la 8.2, el `05` en la 8.1, el `06` disuelto por la 8.0, el `07` mejorado por la 8.0, y el `09` llevado
más lejos por la 8.4. Los reportes `04` y `10` tienen la constancia más incómoda: **su patrón se volvió
a cometer**, y uno de los dos lo cometió la intervención que lo tenía presente.
