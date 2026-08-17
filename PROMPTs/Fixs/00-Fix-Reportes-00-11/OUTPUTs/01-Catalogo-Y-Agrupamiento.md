# Catálogo de los doce reportes y agrupamiento por causa

**Documento:** 01-Catalogo-Y-Agrupamiento.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Estado:** Vigente
**Insumo de:** los cinco análisis por familia (`10-` a `14-`) y el plan unificado (`20-`)

---

## 1. Catálogo

Una fila por reporte. La columna «Ya funciona» recoge lo que el propio reporte declara que el
framework resuelve bien, que es lo que acota la sobrecorrección.

| # | Patrón que enuncia | Fase de origen | Artefactos alcanzados | Severidad propuesta por el reporte | Ya funciona, y no se toca |
|---|---|---|---|---|---|
| 00 | Se exige transcribir de una fuente y no se declara qué obliga la transcripción | A | `Intake-Rules` §5 y §7, `PRODUCT-INTAKE-template` §20, `Master-Prompt` §10 | minor ×3, con una evaluación de major | La obligación de declarar procedencia; la detección del audit; la clasificación P3 bajo la normativa vigente; la estructura de cuatro bloques por escenario |
| 01 | Se declara la forma del identificador y no su ámbito de unicidad | B | `README.md` D3, `Rules-Especificacion-Funcional`, `Rules-Necesidades-Negocio`, `Master-Prompt` | minor, salvo D3 que es invariante | El orquestador delgado que lee la especialidad y no la asigna; el comportamiento de los subagentes, que escalaron en vez de invadir |
| 02 | Fase iterativa con propagación puntual, y matriz de propagación cerrada sin regla de escape | B2 | `Maqueta-Rules` §3.5 y §3.6, `Master-Prompt` §13 | no declarada | La propagación obligatoria y su anti-patrón P0; la matriz explícita con dirección; el orden de propagación; la regla de corte sobre el intake |
| 03 | Dos categorías afirman cosas incompatibles sobre el mismo conjunto y no hay arbitraje | B2 | `Maqueta-Rules` §3.6, reglas de categoría, criterios de audit | no declarada | La titularidad por categoría; la prohibición al maquetador de corregir la especificación; la fila de la matriz que cubre el estado no previsto |
| 04 | El dato derivado se escribe con la misma forma que el declarado, no nombra su fuente y no se verifica | A, B, B2 | `Root-Rules`, reglas de categoría, criterios de audit | no declarada | D9 y D6 como invariantes; la estructura que permitió detectar por contradicción interna |
| 05 | El ancho del identificador se fija como convención de forma, sin declarar de qué colección es función | B2 | `Deriva-Rules` §2.1 y §2.3, D3 y D4 | no declarada; toca invariante | Las tres propiedades que el ancho uniforme da: orden lexicográfico, alineación y reconocibilidad; la regla de estabilidad |
| 06 | La obligatoriedad de un artefacto se decide por el tipo, que describe la forma, no por la responsabilidad | C, y segunda instancia en G | `Rules-Arquitectura-Tecnica` §2.1, §2.2, §6 y §7; `Rules-Examples` §0, §2.1 y §2.2 | no declarada | El tipado del proyecto; el mecanismo `requiere_maqueta`, que ya pregunta por capacidad del proyecto y no por tipo |
| 07 | Una regla de categoría obliga hacia un artefacto que otra categoría emite en una fase posterior | D, ampliado en E y G | `Master-Prompt` §6, `Rules-Contexto`, `Rules-Backlog-Tecnico`, `Rules-Plan-Sprint`, `Rules-Calidad-Y-Pruebas` | no declarada | El orden de fases por dependencia de contenido; la intención de fuente única |
| 08 | El nivel de aplicación se declara por categoría cuando la categoría contiene artefactos de dos niveles | D | `Rules-Plan-Sprint`, `Vocabulario-Rules` §4 R3 | no declarada | Planificar el trabajo por proyecto de código; el roadmap y el acuerdo de equipo en nivel producto; la propia regla R3, que es la que hace visible el problema |
| 09 | Un solo instrumento de verificación para propiedades enumerables e interpretativas, sin criterio de corte | E | `Master-Prompt` §6 y §10; §6 de las reglas de categoría | no declarada | El audit como compuerta y la independencia del auditor, que encontraron los once hallazgos interpretativos |
| 10 | Los criterios de aceptación preguntan si una declaración está, no si es verdadera | G | `Rules-Examples` §4.2, §6; por extensión los §6 de todas las reglas | no declarada | Las preguntas guía de §5, que están bien formuladas; el criterio de cobertura, que apunta bien aunque en la dirección contraria |
| 11 | El framework acuña vocabulario propio, no lo declara en ningún glosario suyo, y el criterio que lo gobierna manda a nueve destinos | G | Once reglas, `Vocabulario-Rules`, `Master-Prompt` §15, `Deriva-Rules` | no declarada | `Vocabulario-Rules` resuelve bien los seis términos que gobierna; `Rules-Plan-Sprint` §6 ya enuncia la política correcta |

## 2. Comprobación del estado vigente

Los reportes fueron escritos entre el 2026-08-09 y el 2026-08-12 contra SDD 6.0. Antes de agrupar
se verificó, sobre el árbol vigente, que los huecos siguen abiertos. Las comprobaciones y su
resultado:

| Afirmación del reporte | Comprobación | Resultado |
|---|---|---|
| 01: el ámbito de unicidad no está declarado en ninguna regla ni orquestador | `grep -rniE "identificador(es)? (único\|únicos)\|unicidad" Rules/ Orchestrator/` | Cero coincidencias. Abierto |
| 01: el framework sí valida colisión, de otra cosa | `grep -rn "colisión" Rules/Intake-Rules.md Orchestrator/Master-Prompt.md` | `Intake-Rules.md:90` y `Master-Prompt.md:179`: colisión de `Nombre-Proyecto-Codigo` e `Identidad-Codigo`. Confirmado |
| 01: los códigos de error se exigen y no se regulan | `Rules-Especificacion-Funcional.md:160` | «Cada error con código, causa y respuesta del sistema». Sin forma ni ámbito. Abierto |
| 05: el ancho de dos dígitos es convención sin capacidad declarada | `Deriva-Rules.md:82`, `Rules-Documentacion.md:392`, `Rules-Backlog-Tecnico.md` §3.2 | Cuatro reglas fijan dos dígitos; ninguna declara qué hacer al agotarse. Abierto |
| 06: las tres menciones del modelo lógico están desalineadas | `grep -n "Modelo-Datos-Logico\|Modelo lógico" Rules-Arquitectura-Tecnica.md` | §2.1 línea 70 condiciona «con almacenamiento» pegado al último tipo; §2.2 línea 86 incondicional; §6 línea 340 y §7 línea 465 «si el tipo D8 exige persistencia». Abierto |
| 07: la fuente única de la DoD vive en el prompt-snippet | `grep -n "única fuente" Rules-Calidad-Y-Pruebas.md` | Línea 453, dentro de §8. Abierto |
| 07: las tres reglas que obligan hacia 08 | `Rules-Contexto.md:182`, `Rules-Backlog-Tecnico.md:115`, `Rules-Plan-Sprint.md:149` | Las tres vigentes. Abierto |
| 10: el criterio de trazabilidad pregunta por presencia | `Rules-Examples.md:382` y `:398` | «con al menos una fila» y «que lo ejercita», en la dirección de la cobertura. Abierto |
| 11: `sonda` no está en el glosario operativo | `grep -n "sonda" Orchestrator/Master-Prompt.md` | Dos apariciones, las dos en uso (§ despacho de Fase G y § handoff), ninguna definición. Abierto |
| 08: el nivel se declara por regla y no por artefacto | `grep -n "Nivel de aplicación" Rules/*.md` | Una sola declaración por archivo de reglas. Abierto |

Los doce huecos siguen abiertos sobre el árbol vigente. Ninguno fue corregido entre la emisión de
los reportes y esta intervención.

## 3. Agrupamiento

El criterio es el del prompt: **dos reportes van juntos si se corrigen con la misma
intervención**, no si se parecen.

| Familia | Reportes | Qué comparten | Dónde vive su corrección |
|---|---|---|---|
| **G1 — Un atributo fijado sin declarar de qué depende** | 01, 05, 06, 08 | El framework fija lo visible de una propiedad y omite lo que la gobierna: la forma del identificador sin su ámbito, el ancho sin su capacidad, la obligatoriedad sin la responsabilidad, el nivel sin el referente | `Root-Rules.md` como regla transversal, más las reglas de categoría que declaran cada atributo |
| **G2 — El dato que se copia o se deriva no tiene regla** | 00, 04 | Los dos son el mismo dato en dos lugares: transcripto de una fuente el uno, derivado de una colección el otro. Ninguno de los dos está marcado como tal, y por eso ninguno se puede recalcular | `Intake-Rules.md` y `PRODUCT-INTAKE-template.md` para la transcripción; `Root-Rules.md` y los criterios de audit para la derivación |
| **G3 — Falta arbitraje entre categorías** | 02, 03, 07 | El framework tiene trazabilidad entre categorías y no tiene resolución de conflictos: no declara quién decide cuando dos categorías se contradicen, ni qué hacer con una obligación que apunta a una fase posterior, ni qué pasa con lo que la matriz de propagación no enumera | `Maqueta-Rules.md` §3.6, `Master-Prompt.md` §6 y §10, y un registro de decisiones pendientes que hoy no existe |
| **G4 — El instrumento y el criterio de verificación** | 09, 10, y el hueco C del 00 | No es sobre lo que el framework produce sino sobre cómo lo verifica: un solo instrumento para dos naturalezas de propiedad, criterios que preguntan por presencia y no por verdad, y una taxonomía de hallazgos sin eje de origen | `Master-Prompt.md` §10 y las secciones §6 de las reglas de categoría |
| **G5 — El vocabulario del método no tiene glosario** | 11 | Único en su familia. El framework acuña vocabulario y lo manda a declarar a glosarios del producto | `Master-Prompt.md` §15, `Vocabulario-Rules.md` y el criterio de gobierno de glosario replicado en once reglas |

### 3.1 Dónde difiere del agrupamiento del `README.md` de los reportes

El índice de los reportes propone cuatro familias. Las diferencias, con su motivo:

| Diferencia | Qué dice el índice | Qué dice este agrupamiento | Por qué |
|---|---|---|---|
| Ubicación del `00` y del `04` | Los mete en la familia de «los cinco que aparecen al modificar», junto con `01`, `02` y `03` | Los separa en G2, y manda `01` a G1 y `02` y `03` a G3 | «Aparecer al modificar» es un momento de detección, no una causa. El `01` no se corrige con nada de lo que corrige al `00`: uno se arregla declarando un ámbito y el otro declarando una regla de transcripción. El propio `04` §5 dice que su parentesco real es con el `00` —«la transcripción y la derivación son dos formas de tener el mismo dato en dos lugares»— y no con el `01` |
| Ubicación del `08` | Familia de los tres que fijan un atributo sin declarar de qué depende, junto con `05` y `06` | Igual, y suma el `01` a esa familia | El índice deja el `01` afuera por haber salido de otra fase, pero su §4.1 enuncia exactamente el mismo defecto: forma declarada, ámbito no. El `05` §5 lo dice explícitamente: «allá el framework declaraba la forma y no su ámbito; acá declara la forma y no su capacidad» |
| Ubicación del `07` | Familia propia, «un conflicto entre reglas» | Dentro de G3, con el `02` y el `03` | Su §5 declara que su causa raíz es la misma del `03`: «en los dos casos el framework tiene trazabilidad entre categorías y no tiene arbitraje». Y su corrección comparte artefacto con la del `02`: las dos son mecanismos que el orquestador ejecuta entre categorías |
| El hueco C del `00` | Dentro del reporte `00` | Se trata en G4 | El hueco C no es sobre la transcripción: es sobre que la taxonomía de hallazgos del audit no tiene eje de origen. Se corrige en `Master-Prompt.md` §10, junto con lo demás de G4, y no en `Intake-Rules.md` |

Los tres primeros son reagrupamientos, no desacuerdos: el índice agrupa por cuándo se detecta el
defecto, y este documento agrupa por dónde vive la corrección, que es lo que el prompt pide. El
cuarto sí parte un reporte en dos familias, y por eso queda registrado: el `00` aporta a G2 sus
huecos A y B, y a G4 su hueco C.

## 4. Orden de tratamiento

G1 y G2 primero, porque son los que agregan declaraciones nuevas a reglas transversales, y G3, G4 y
G5 se apoyan en ellas. En particular:

- G4 necesita saber qué es un dato derivado —lo declara G2— para poder pedir que el audit lo
  verifique.
- G3 necesita el mecanismo de referencia pendiente, que también consume G5 para el
  `Glosario-Tecnico.md` de la 11 que seis categorías referencian desde fases anteriores.
- G5 necesita el registro de decisiones pendientes que introduce G3, para lo que quede sin resolver.
