---
doc_id: INF-2026-08-18-memoria-de-antecedentes
doc_type: informe
title: Memoria de antecedentes — qué hacer con los casos resueltos que no se deducen de los criterios
status: vigente
origin: agent
confidence: alta para lo medido sobre los árboles del framework y del destino; media para la caracterización del estado del arte, que se apoya en fuentes secundarias y resúmenes, no en los originales completos
owner: AG-ROOT (Arquitecto de Soluciones)
fecha: 2026-08-18
---

# Memoria de antecedentes — qué hacer con los casos resueltos que no se deducen de los criterios

## Resumen ejecutivo

**Qué es.** Un informe sobre qué hacer con el conocimiento que deja una corrida real: no el criterio
—que ya tiene su lugar— sino **el caso**, con la forma que tomó su solución y el motivo por el que
tomó esa forma y no la obvia.

**Para qué sirve.** Para decidir si el framework necesita un repositorio de antecedentes, con qué
forma, y —sobre todo— **qué mecanismo lo mantiene vivo**, que es donde la literatura reporta que
fallan casi todos.

**A quién le sirve.** A quien opera el método con agentes y quiere que la sesión número doce no
vuelva a razonar lo que se razonó en la tercera.

**Hallazgo central, medido.** El método **no tiene un problema de captura**: un solo destino tiene
**15.763 líneas** en **58 documentos** de `Audit/`, más **51 ADR** y **43 entradas** de registro de
cambios. Lo que no tiene es **recuperación por episodio**. La consecuencia está medida en §2.2: para
saber qué quedaba abierto hubo que abrir **cinco informes de migración** y cruzarlos a mano, y el
resultado de ese cruce fue que **tres de los diez hallazgos no eran lo que declaraban ser**.

**Y el segundo hallazgo, que decide el diseño:** el conocimiento del despliegue de ese producto está
repartido en **tres repositorios y una carpeta no versionada**, y **ningún artefacto lo ensambla**. Un
agente que lee el repositorio del fuente —el único que un lector supondría autoritativo— llega a una
conclusión equivocada. Está documentado en §2.3, con el caso ocurrido.

---

## 1. Definiciones

| Término | Qué designa en este informe |
| --- | --- |
| **Criterio** | Regla general que resuelve una **situación**: «cuando pasa X, hacé Y, por Z». Se recupera **por situación**. Vive en las reglas y lo indexa `Catalogo-De-Criterios.md` |
| **Antecedente** | **Caso particular ya resuelto** cuya solución **no se deduce de los criterios**. Se recupera **por episodio** —«lo del despliegue»—, que es otra clave de búsqueda |
| **Registro de lecciones** | Artefacto **del proyecto**, que se llena durante él. En PMBOK, *lessons learned register* |
| **Repositorio de lecciones** | Artefacto **de la organización**, alimentado por transferencia al cerrar. En PMBOK, *lessons learned repository*, un activo de proceso organizacional |
| **Vaporización del conocimiento** | Pérdida del razonamiento detrás de una decisión, que sobrevive como resultado sin motivo. Término de Jansen y Bosch, adoptado por la literatura de ADR |

**La distinción que ordena el informe es la clave de búsqueda.** Un criterio se busca por situación;
un antecedente, por episodio. Son dos índices porque son dos preguntas, y por eso un catálogo de
criterios —por bueno que sea— no responde «¿qué hicimos con aquel despliegue?».

---

## 2. Situación y contexto

### 2.1 El contexto: el método captura mucho y recupera poco

Medido el 2026-08-18, sobre el conjunto **9.19** del framework y sobre el destino `Lab-Geometria`:

| Dónde | Cuánto |
| --- | --- |
| `SDD/Docs/Audit/` del destino | **58 documentos, 15.763 líneas** |
| ADR del producto | **51** |
| Entradas del registro de cambios del producto | **43** |
| Notas de coherencia del framework | **29** |
| `Catalogo-De-Criterios.md` | **173 líneas**, **26 situaciones** indexadas, versión 1.5 |

**El catálogo resolvió la recuperación de criterios y lo hizo bien**: 173 líneas que dicen dónde vive
cada uno, de modo que un agente lee el índice y abre **una** sección en lugar de dieciocho archivos.
Su propia introducción lo declara: «es un índice, no una regla».

**Lo que ningún artefacto resuelve es la otra clave.** No hay ningún punto de entrada por episodio.

### 2.2 La situación: tres hallazgos que no eran lo que declaraban

Durante la corrida del 2026-08-17 se cerraron los **diez** hallazgos abiertos del destino. Estaban
repartidos en **cinco informes de migración**, cada uno con su numeración y su fecha, y para saber qué
quedaba pendiente hubo que abrir los cinco y cruzarlos a mano.

**Al abrirlos uno por uno, tres no eran lo que decían:**

| Hallazgo | Qué declaraba | Qué se encontró al abrirlo |
| --- | --- | --- |
| `N-03` y `M-06` | «Cuatro enlaces rotos en `Audit/`» | **Ninguno era un enlace roto que hubiera que arreglar.** Dos eran tramos de código —markdown nunca los renderizó— y dos estaban dentro de una transcripción textual |
| `N-05` | «Cuatro citas a documentos que **no existen bajo ningún nombre** en el árbol vigente» | **Los cuatro existían**, renumerados y reubicados, con su título literal intacto |
| `M-07` | Los casos de uso no habían absorbido un cierre del intake | **La propagación ya había ocurrido**, y el propio texto declaraba el punto cerrado tres veces |

**Los cuatro «enlaces rotos» viajaron tres informes** antes de que alguien los abriera. Cada informe
los heredó del anterior y los volvió a listar.

**Lo que esto muestra no es descuido:** muestra que **un hallazgo copiado de un informe al siguiente
no es conocimiento recuperado, es conocimiento reenviado**. La diferencia es que el segundo nunca se
verifica.

### 2.3 El caso que define el problema: un conocimiento sin dueño

El producto tiene un **despliegue no convencional**, forzado por una restricción de red ajena al
producto: las redes de la facultad **bloquean por política** el acceso a direcciones residenciales
(`RN-B1` del intake). De ahí la inversión: la pieza pública se publica en un hosting alcanzable desde
cualquier red, y **es ella la que alcanza al servidor propio**, que es privado y de IP dinámica.

**Ese conocimiento está escrito, y está repartido:**

| Pieza | Dónde vive |
| --- | --- |
| La restricción que lo fuerza (`RN-B1`) | Intake del destino, repositorio del fuente |
| La decisión sobre la IP dinámica (`ADR-14003`, 118 líneas) | `SDD/Docs/Producto/Adrs/` del fuente |
| El canal del front (flujo de FTP, 101 líneas) | `.github/workflows/` del fuente |
| Los tres niveles y la regla de propagación (171 líneas) | README de **otro repositorio**, el del contenedor |
| La constancia de qué corre hoy (35 líneas) | Una **carpeta no versionada** del host |

**Tres repositorios distintos y una carpeta que no está en ninguno. Ninguno ensambla el conjunto.**

**Y tiene consecuencia medida.** El 2026-08-18, un agente leyó `deploy/compose.yaml` del repositorio
del fuente —el archivo que el nombre, la carpeta y su primera línea presentaban como la composición de
despliegue— y **le informó al Product Owner que la composición del host estaba incompleta porque no
declaraba la clave de firma**. Era el archivo equivocado: la composición real vive en el proyecto de
contenedor y **sí la declara**, con una guarda que aborta antes de construir. El agente sólo se enteró
de la existencia de ese repositorio **porque el Product Owner se lo nombró**.

**Nada en el árbol del fuente llevaba hasta ahí.** No es un defecto de lectura: es un conocimiento que
no tiene dueño.

### 2.4 Por qué importa

**Porque lo que se pierde no es el resultado sino el motivo.** El resultado —el despliegue funciona—
es visible. Lo que se vaporiza es *por qué tiene esa forma rara*, y sin eso la próxima intervención
propone lo obvio. Ocurrió dos veces en la misma sesión: se propuso **DDNS** después de que el Product
Owner ya lo hubiera evaluado y diferido, y se propuso **renombrar** un archivo sin saber que el intake
lo declaraba, lo que habría obligado a escritura controlada sobre documento humano.

**Las dos veces el costo lo pagó el humano**, explicando algo que ya estaba resuelto.

---

## 3. Estado del arte

### 3.1 Los estándares de gestión de proyectos ya nombran los dos artefactos

**PMBOK 6 (2017)** incorporó el proceso **«Manage Project Knowledge»** (4.4, Integración, grupo de
Ejecución), con dos propósitos declarados: **reusar conocimiento existente y crear conocimiento
nuevo**. Y distingue dos artefactos que suelen confundirse:

- **Lessons Learned Register** — **del proyecto**, se llena durante él, con categoría, descripción de
  la situación, impacto, recomendaciones y acciones propuestas.
- **Lessons Learned Repository** — **de la organización**, un *organizational process asset*, al que
  el registro **se transfiere**.

**Esa separación responde una pregunta de diseño que suele quedar abierta:** el conocimiento es
primero del producto y después de la empresa, y **el pasaje es un acto explícito**, no una acumulación
espontánea.

**PRINCE2** eleva «Learn from experience» a **principio**, y lo instrumenta con un **Lessons Log**
corriente y un **Lessons Report** al cierre. Y tiene una pieza que los repositorios documentales
suelen no tener: en *Starting Up* hay una actividad llamada **«Capture Previous Lessons»** —
**consultar lo aprendido es un paso obligatorio del arranque**.

**ISO 21502** integra las lecciones al cierre y a las actividades posteriores al proyecto, con el
propósito declarado de prevenir errores recurrentes.

### 3.2 La investigación reporta que el problema es el reuso, no la captura

**Schindler y Eppler (2003)**, sobre investigación-acción en **nueve empresas multinacionales**,
distinguen métodos **basados en proceso** —el acto de debriefing— de métodos **basados en
documentación** —el formato en que se representa lo aprendido—, y encuentran que los que funcionan
**combinan los dos**. Nombran las barreras: presión de tiempo, falta de motivación y documentación
pobre, que producen lo que llaman **«amnesia de proyecto»**.

**Y la literatura posterior reporta el modo de falla del lado del reuso:** los repositorios de
lecciones **rara vez se revisan y a veces se abandonan**, por dos motivos concurrentes — que quien
podría usarlas **no sabe que existen**, y la **dificultad de buscar** la lección relevante. Un tercer
factor recurrente es que **cantidad no es calidad**: el resultado típico se describe como un conjunto
de lecciones desarticuladas que no se concentra en lo importante.

El problema está lo bastante reconocido como para tener trabajo académico aplicándole **clasificadores
de recuperación de información**.

### 3.3 El razonamiento basado en casos da el ciclo, y corrige el orden

**Aamodt y Plaza (1994)** describen el ciclo canónico del razonamiento basado en casos, conocido como
**4R**:

| Paso | Qué hace |
| --- | --- |
| **Retrieve** | Identificar descriptores, buscar casos pasados, emparejar y **seleccionar el más similar** |
| **Reuse** | Aplicar la solución del caso recuperado, con o sin adaptación |
| **Revise** | **Evaluar** la solución propuesta contra el resultado real |
| **Retain** | Incorporar el caso nuevo a la base |

**El orden es el aporte.** `Retain` es **el último paso**, no el primero: un caso entra a la base
**después de haber sido reusado y revisado**. Un caso guardado sin haber sido nunca reusado no es
conocimiento validado — es una hipótesis con formato de conclusión.

### 3.4 En lo técnico, los ADR ya resolvieron la forma

Los **Architecture Decision Records** (Nygard, 2011) nacieron contra la **vaporización del
conocimiento** —término de Jansen y Bosch— con un argumento fundacional que vale para este informe:
**nadie lee documentos grandes**, y a la vez perder el razonamiento detrás de una decisión lleva a que
alguien después la deshaga sin saber que existía.

Su forma —contexto, decisión, consecuencias— es hoy estándar de facto de gestión de conocimiento
arquitectónico, y existe trabajo empírico comparando plantillas por comprensión, usabilidad y
facilidad de adopción.

**El framework SDD ya usa esa forma, y en un punto la excede:** sus ADR de apartamiento declaran
además el **campo 4, los disparadores que superarían la decisión**, y desde `Root-Rules` 6.1 el
**estado** y el **contador de saltos sobrevividos**. Un ADR estándar dice por qué se decidió; uno de
este framework dice además **cuándo deja de valer** y **cuánto lleva vigente**.

---

## 4. El criterio propuesto

### 4.1 Qué merece ser antecedente

**Un caso merece guardarse cuando su resolución no se deduce de los criterios**: cuando un agente
competente que siguiera el método **no habría llegado a esa solución**.

Es un filtro y no una preferencia. Si el despliegue del destino fuera convencional —registro de
imágenes, servidor alcanzable, nombre estable— los criterios alcanzarían y no habría antecedente que
escribir. Lo que hay que guardar es exactamente lo que **la restricción ajena forzó**.

**Tres pruebas para aplicarlo, y hay que pasar las tres:**

1. **¿La solución se deduce de los criterios vigentes?** Si sí, no es antecedente: es aplicación.
2. **¿Sobrevive el motivo sin el caso?** Si el porqué ya está escrito en un ADR y es completo, no hace
   falta duplicarlo: hace falta **apuntarlo**.
3. **¿Costaría reconstruirlo?** Si la próxima sesión lo re-deriva en cinco minutos, no es antecedente.

### 4.2 Qué lleva un antecedente

Cinco campos, cuatro tomados del apartamiento —que en este framework ya probó funcionar— y uno nuevo:

| Campo | Para qué | Origen |
| --- | --- | --- |
| **Restricción que lo forzó** | Es lo único que hace inteligible una solución rara. Sin esto se lee como capricho | Análogo al contexto del ADR |
| **Forma de la solución** | Qué se hizo, breve, **con punteros a dónde está cada pieza** | Análogo a la decisión del ADR |
| **Qué se descartó y por qué** | **Para que nadie lo vuelva a proponer** | Alternativas descartadas del ADR |
| **Alcance de validez** | Bajo qué condiciones vale. Un antecedente aplicado a un caso distinto es peor que ninguno | **Nuevo** |
| **Disparador** | Cuándo deja de aplicar | Campo 4 de `Root-Rules` §11 |

**El tercero es el que este informe defiende con más evidencia.** Las dos propuestas redundantes de
§2.4 —DDNS y el renombre— las habría evitado un campo que dijera qué ya se evaluó y se descartó.

**Y el cuarto es el que evita el daño mayor.** Un antecedente sin alcance declarado se aplica donde no
corresponde, y eso es peor que no tenerlo: da confianza de haber decidido con historia cuando se
decidió con una analogía falsa.

### 4.3 Documento nuevo o puntero

**La regla propuesta: puntero por omisión, documento sólo cuando el conocimiento cruza fronteras.**

- **Puntero**, cuando lo que hay que recordar vive entero en un repositorio y ya está escrito. Volver
  a redactarlo crea dos versiones de lo mismo, que es el defecto que este framework combate en todas
  sus reglas.
- **Documento**, cuando **está repartido y ningún repositorio es el dueño** — el caso de §2.3. Y ese
  documento **no re-narra: ensambla y apunta.** Su contenido propio es la restricción, lo descartado y
  el alcance; lo demás son referencias.

### 4.4 Los dos pasos que lo mantienen vivo

**Esta es la parte que la literatura señala como decisiva, y la que un repositorio solo no resuelve.**

**Paso de entrada — transferir al cerrar.** Siguiendo PMBOK, el registro del proyecto —`Audit/`— es lo
que ya existe; lo que falta es el acto de **transferir** al repositorio de la organización. Y
siguiendo el ciclo 4R, ese acto no es el primero sino el que sigue a haber **usado** el caso.

**Paso de salida — consultar al arrancar.** Es la pieza de PRINCE2 y la que ningún índice reemplaza:
si consultar depende de que alguien se acuerde, no se consulta. El framework SDD tiene **dónde
colgarlo sin inventar nada**: sus orquestadores ya se detienen a **recomendar con fundamento y
alternativa** —R2 de la reanudación, M1 de la migración—, y esa recomendación es exactamente el lugar
donde un antecedente es insumo.

**Con eso el antecedente deja de ser archivo y pasa a ser fundamento disponible**, que es la única
forma conocida de que se lo consulte.

---

## 5. Aplicación al framework SDD

### 5.1 Dónde estaría parado, contra los estándares

| Artefacto del estándar | Qué existe hoy | Qué falta |
| --- | --- | --- |
| Registro de lecciones, del proyecto | `Audit/`, ADR, registro de cambios — **abundante** | Nada |
| **Repositorio de la organización** | — | **Todo** |
| Informe de lecciones al cierre | Los informes de migración se le aproximan | Un cierre de corrida deliberado |
| **Consultar lo previo al arrancar** | — | **El paso** |
| **Retrieve** del ciclo 4R | — | **El índice por episodio** |
| ADR con contexto y consecuencias | 51, con disparador y contador | Nada |

**El método es fuerte donde la literatura reporta que casi todos son fuertes —capturar— y está vacío
donde reporta que casi todos fallan —recuperar y reusar—.**

### 5.2 Qué NO conviene hacer

**No crear una carpeta de «conocimiento» aparte.** Sería un cuarto lugar donde buscar, y el catálogo
de criterios existe justamente para que haya uno. La evidencia de §3.2 sobre repositorios abandonados
se aplica con más fuerza a un repositorio nuevo sin paso que lo consulte.

**No mudar el material existente.** Las 15.763 líneas de `Audit/` están bien donde están: son el
registro del proyecto, fechado, y su valor es exactamente ese. El antecedente **apunta**, no absorbe.

**No escribir antecedentes de forma continua.** El Product Owner ya lo dijo con precisión: esto se
actualiza en sesiones como la que originó este informe. Restringirlo a esos momentos **es una
propiedad y no una limitación**: es lo que evita el «lío de lecciones desarticuladas» que §3.2
reporta.

### 5.3 El primer antecedente candidato

El caso de §2.3 pasa las tres pruebas de §4.1: la solución **no se deduce** de ningún criterio, su
motivo **no sobrevive** en ningún documento único, y reconstruirlo **ya costó** una conclusión
equivocada y una explicación del humano.

Sería el primero, y —siguiendo §3.3— **se escribiría ahora y se revisaría la primera vez que se use
de verdad**: cuando la IP rote y alguien siga el procedimiento. Recién ahí se sabe si servía.

---

## 6. Observaciones

Se distinguen hechos de interpretaciones, según `Rule-Evidences.md`.

**Hechos verificados sobre los árboles, el 2026-08-18:**

- Conjunto del framework: **9.19**. `Catalogo-De-Criterios.md` en **1.5**, **173 líneas**, **26**
  situaciones indexadas. *Verificable con `grep` y `wc` sobre el archivo.*
- Destino `Lab-Geometria`: **58 documentos y 15.763 líneas** en `SDD/Docs/Audit/`, **51 ADR** vivos,
  **43 entradas** de `changelog.md`, **5 informes de migración**. *Verificable con `ls`, `cat | wc -l`
  y `find`.*
- **29 notas de coherencia** en el framework. *Verificable con `ls`.*
- El conocimiento del despliegue del destino está repartido en **tres repositorios** —el del fuente, el
  del contenedor y ninguno para la carpeta de despliegue— más una **carpeta no versionada** con 35
  líneas de constancia. *Verificable listando las cinco piezas de §2.3.*
- **Tres de los diez hallazgos** cerrados el 2026-08-17 no correspondían a lo que declaraban.
  *Verificable en `Cierre-De-Hallazgos-Abiertos-2026-08-17.md` §2 y §3.*
- El framework SDD declara el campo de **disparador** y, desde `Root-Rules` 6.1, el **estado** y el
  **contador de saltos** en sus apartamientos. *Verificable en `Root-Rules.md` §11.*

**Hechos verificados sobre fuentes externas** —resúmenes y páginas de referencia, no los originales
completos—:

- PMBOK 6 incorporó «Manage Project Knowledge» y distingue *register* (del proyecto) de *repository*
  (activo organizacional).
- PRINCE2 tiene «Learn from experience» como principio, con *Lessons Log*, *Lessons Report* y la
  actividad *Capture Previous Lessons* en el arranque.
- Aamodt y Plaza (1994) describen el ciclo **Retrieve–Reuse–Revise–Retain**, con `Retain` al final.
- Schindler y Eppler (2003) distinguen métodos de proceso y de documentación sobre nueve
  multinacionales, y nombran la «amnesia de proyecto».
- La literatura reporta repositorios de lecciones **rara vez revisados o abandonados**, por
  desconocimiento de su contenido y dificultad de búsqueda.
- Los ADR (Nygard, 2011) atacan la «vaporización del conocimiento» de Jansen y Bosch.

**Interpretaciones del autor, no verificadas:**

- Que la clave de búsqueda —situación contra episodio— sea la distinción que ordena el diseño es una
  lectura propia. Ninguna fuente consultada la formula así.
- Que «lo no convencional» sea el filtro correcto de admisión es una propuesta derivada del caso de
  §2.3 y de un solo producto. **No está probada sobre más casos.**
- Que el paso de consulta deba colgar de la detención de recomendación de los orquestadores es una
  ubicación propuesta, coherente con lo que esos orquestadores ya hacen, pero no ensayada.
- El campo **alcance de validez** no proviene de ninguna fuente: se propone por el riesgo de aplicar un
  antecedente a un caso distinto, riesgo que este informe **no midió**.

**Límites declarados de este informe:**

- Las fuentes externas se consultaron por resumen. Las tesis centrales están sostenidas, pero fundar
  una decisión de método sobre alguna en particular exigiría el original — el de Aamodt y Plaza está
  disponible en abierto.
- Toda la evidencia interna proviene de **un solo destino**. Un antecedente que sobreviva a un segundo
  producto sería lo que demuestre que el mecanismo generaliza, con el mismo argumento con el que el
  contador de apartamientos distingue lo particular de lo general.

---

## 7. Conclusiones

**1. El método no tiene un problema de captura, tiene uno de recuperación**, y es el que la
literatura reporta como el que hace fracasar a casi todos los repositorios de lecciones.

**2. Los estándares ya nombran los dos artefactos y su pasaje**: registro del proyecto y repositorio
de la organización, con una transferencia explícita al cerrar. No hace falta inventar estructura: hace
falta adoptar la que ya tiene nombre.

**3. El repositorio no es lo que resuelve el problema: los dos pasos sí.** Uno que transfiere al
cerrar y otro que consulta al arrancar. Un repositorio sin el segundo es exactamente el artefacto
abandonado que §3.2 describe.

**4. `Retain` va al final.** Un antecedente se escribe y **se revisa cuando se usa**. Antes de eso es
una hipótesis, y presentarla como conclusión es el modo en que un caso guardado envejece sin avisar.

**5. El filtro de admisión es lo no convencional**, y mantiene el inventario chico solo, sin que nadie
tenga que podarlo.

**6. La decisión de si esto es del producto o de la empresa no hay que tomarla hoy.** Se escribe como
antecedente del producto; **que un segundo producto enfrente lo mismo es lo que probaría que es de la
empresa**, y recién ahí se sube. Antes de eso sería adivinar.

---

## 8. Referencias

**Externas**

- Aamodt, A. y Plaza, E. (1994). *Case-Based Reasoning: Foundational Issues, Methodological
  Variations, and System Approaches*. AI Communications. https://www.iiia.csic.es/~enric/papers/AICom.pdf
- *Retrieval, reuse, revision and retention in case-based reasoning*. https://www.iiia.csic.es/~mantaras/RRRR.pdf
- Schindler, M. y Eppler, M. J. (2003). *Harvesting project knowledge: a review of project learning
  methods and success factors*. International Journal of Project Management, 21, 219-228.
  https://www.sciencedirect.com/science/article/abs/pii/S0263786302000960
- PMBOK 6 — proceso *Manage Project Knowledge*. https://www.brainbok.com/guide/pm-processes/project-integration-management/manage-project-knowledge
- Distinción entre *lessons learned register* y *repository*. https://www.aileenellis.com/blog/lessonslearnedregister
- PMI — *Lessons (Really) Learned? How To Retain Project Knowledge*. https://www.pmi.org/learning/library/knowledge-management-lessons-learned-10161
- *Searching for Relevant Lessons Learned Using Hybrid Information Retrieval Classifiers*. arXiv.
  https://arxiv.org/pdf/1812.05168
- PRINCE2 — principio *Learn from experience*. https://prince2.wiki/principles/learn-from-experience/
- PRINCE2 — *Lessons Log*. https://prince2.wiki/management-products/lessons-log/
- ISO 21502 — *Project management — Guidance on project management*. https://iso-library.com/standard/21502/
- *Architectural Decision Records*. https://adr.github.io/
- *One Size Fits All? An Empirical Comparison of ADR Templates*. arXiv. https://arxiv.org/html/2604.27333

**Internas, reproducibles**

- `SDD/Devs/Rules/Catalogo-De-Criterios.md` 1.5 — el índice por situación, y su §1 declarando que es
  índice y no regla.
- `SDD/Devs/Rules/Root-Rules.md` 6.1 §11 — apartamiento declarado, con estado y contador.
- `SDD/Devs/Rules/Migracion-Rules.md` §4.7 — la revisión de apartamientos y el umbral de dos saltos.
- `SDD/Devs/Orchestrator/Master-Prompt.md` §8.1 — la forma de toda detención, con propuesta y
  alternativa: el lugar propuesto para el paso de consulta.
- Destino `Lab-Geometria`: `SDD/Docs/Audit/Cierre-De-Hallazgos-Abiertos-2026-08-17.md` §2, §3 y §6 —
  los tres hallazgos que no eran lo que declaraban.
- Destino `Lab-Geometria`: `SDD/Docs/Producto/Adrs/ADR-14003-Direccion-Del-Backend-Por-IP-Dinamica.md`
  — el ADR con disparador que sirve de modelo al antecedente.

---

## Control de cambios

| Versión | Fecha | Cambios |
| --- | --- | --- |
| 1.0 | 2026-08-18 | Emisión inicial. Estudia qué hacer con los casos resueltos cuya solución no se deduce de los criterios. **Hallazgo medido:** el método captura mucho —15.763 líneas en un solo destino— y **no recupera por episodio**, con la consecuencia de que tres de diez hallazgos viajaron entre informes sin verificarse y de que el conocimiento de un despliegue no convencional, repartido en tres repositorios, llevó a un agente a una conclusión equivocada. **Contra el estado del arte:** PMBOK distingue registro del proyecto de repositorio de la organización con transferencia explícita; PRINCE2 hace de la consulta previa **un paso del arranque**; Aamodt y Plaza ponen `Retain` **al final** del ciclo; Schindler y Eppler reportan que funcionan los métodos que combinan proceso y documentación. **Propone** un filtro de admisión —lo no convencional—, cinco campos con el alcance de validez como aporte propio, la regla de puntero por omisión y documento sólo cuando el conocimiento cruza fronteras, y **los dos pasos** —transferir al cerrar, consultar al arrancar— como lo que realmente mantiene vivo el repositorio. Declara sus límites: fuentes consultadas por resumen y evidencia interna de un solo destino. |
