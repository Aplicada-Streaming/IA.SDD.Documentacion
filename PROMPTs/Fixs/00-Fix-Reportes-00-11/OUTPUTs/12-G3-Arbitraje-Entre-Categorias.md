# G3 — Falta arbitraje entre categorías

**Documento:** 12-G3-Arbitraje-Entre-Categorias.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Reportes de la familia:** 02, 03, 07
**Estado:** Vigente

---

## 1. Dónde se produce el fallo

La frase que agrupa a los tres está en el reporte 03 §5 y la repite el 07 §5:

> el framework tiene **trazabilidad** entre categorías —D6— pero no tiene **arbitraje**. Sabe
> conectar, no sabe resolver.

Los tres son la misma carencia vista desde tres ángulos:

| Reporte | Qué conflicto aparece | Qué le falta al método |
|---|---|---|
| 02 | Lo aprobado en una iteración y lo documentado divergen mientras la fase sigue abierta; y lo que la matriz de propagación no enumera no se propaga | Un disparador de propagación por iteración, y una regla de escape para lo no previsto |
| 03 | Una categoría necesita extender un conjunto cerrado que otra declaró | Quién decide, dónde se registra la decisión pendiente, y qué audit la detecta |
| 07 | Una regla obliga a referenciar un artefacto que otra categoría emite hasta cinco fases después | Un estado de referencia pendiente, una reapertura que traiga el insumo, y una comprobación del grafo de obligaciones contra el orden de fases |

## 2. Por qué la normativa vigente no lo atrapa

### 2.1 Lo que el framework ya resuelve bien y no hay que reescribir

Es la acotación que los tres reportes traen a propósito, verificada sobre el árbol vigente:

| Qué funciona | Dónde |
|---|---|
| La propagación de la Fase B2 es obligatoria y su omisión es hallazgo P0 | `Maqueta-Rules.md` §3.6, y el criterio en `Master-Prompt.md` §10, que declara P0 «la aprobación de la maqueta sin retroalimentación de la documentación» |
| Hay matriz explícita, con ocho filas y dirección declarada | `Maqueta-Rules.md` §3.6 |
| Hay orden de propagación: primero 03, después atrás, después adelante | `Maqueta-Rules.md` §3.6 |
| Hay regla de corte para el intake y para 00 y 01 | `Maqueta-Rules.md` §3.6, última línea |
| El maquetador no corrige la especificación por su cuenta: emite el hallazgo | `Maqueta-Rules.md` §3 |
| La titularidad por categoría está clara: 02 es dueña de los casos de uso, 03 de la experiencia | Cabeceras y §1 de las dos reglas |
| El orden de fases por dependencia de contenido es correcto | `Master-Prompt.md` §6. Adelantar la 08 para resolver el reporte 07 sería cambiar un problema por otro peor |

### 2.2 Los tres huecos, con su cita

**El disparador de la propagación es un evento y la fase es un bucle.** `Maqueta-Rules.md` §3.6 abre
con «**Con la maqueta aprobada**, el orquestador propaga lo aprendido», y §3.5 declara el paso
anterior como «Ciclo de corrección y validación (detención, **iterativo**)». Los dos son correctos por
separado. Juntos producen un intervalo, tan largo como dure la validación, en el que la documentación
describe un producto y la maqueta aprobada describe otro. En la corrida real el intervalo duró dos
días y la documentación quedó catorce estados, dos representaciones y un proyecto de código entero
por detrás de lo aprobado. Y la regla se cumplió: por la letra de §3.6 la propagación no estaba
vencida, porque la maqueta como tal todavía no estaba aprobada.

**La matriz es cerrada y no tiene escape.** Las ocho filas comparten una propiedad no declarada:
todas propagan a categorías de proyectos de código **que ya existen**. Ninguna contempla que la
validación cree un proyecto de código, cuyos destinos no son categorías sino el `PRODUCT-INTAKE` §13,
§16, §17 y §18, el `PRODUCT-MANIFEST` §2, §3 y §4, y un árbol completo que exige volver a correr la
Fase B. Y el `PRODUCT-MANIFEST` no aparece en ninguna de las ocho filas ni en la regla de corte, que
nombra el intake y nombra 00 y 01: es documento derivado del intake y queda desincronizado en
silencio.

**No hay arbitraje sobre un conjunto cerrado.** El framework no dice qué pasa cuando la categoría
dueña de la experiencia necesita, para que la experiencia funcione, algo que la categoría dueña del
modelo declara imposible; no exige que los conjuntos cerrados estén marcados como tales; y ningún
audit cruza dos categorías buscando contradicciones sobre un mismo referente. Los criterios de
`Master-Prompt.md` §10 verifican «coherencia cross-doc **dentro de la fase**», que es otra cosa.

**No hay estado para una referencia que todavía no puede resolver.** `Rules-Contexto.md` §4.2 línea
182 exige «§5 Definition of Done (referencia a 08)» en un documento de la Fase A;
`Rules-Backlog-Tecnico.md` §3.4 línea 115 y `Rules-Plan-Sprint.md` §4.2 línea 149 exigen referenciar
la condición de terminado de la 08 desde la Fase D. La 08 se emite en la Fase E. No hay estado
`Pendiente de emisión` para una referencia, no hay reapertura obligatoria y no hay criterio de audit
que detecte una referencia colgada.

**Y la declaración de fuente única no es normativa.** Es el agravante que el reporte 07 §2 precisa:

```bash
grep -n "única fuente" Rules/Rules-Calidad-Y-Pruebas.md
# 453: dentro de «## 8. Prompt-snippet sugerido», no en §6
```

`Rules-Calidad-Y-Pruebas.md` §6 sí cubre una parte —«la DoD no se redefine en sprint plans»— y por
eso alcanza a una copia hecha en un plan de sprint. **No alcanza a la copia hecha en el acuerdo de
equipo de la Fase A**, que es el incidente A: no es un plan de sprint, y `Rules-Contexto.md` no tiene
criterio equivalente. El incidente cae exactamente en el hueco entre dos reglas.

## 3. Las dos salidas disponibles rompen otra regla del framework

Es lo que hace que este grupo no se resuelva pidiéndole más cuidado al agente:

| Salida | Qué rompe | Dónde ocurrió |
|---|---|---|
| Copiar el contenido | Crea una segunda fuente de algo que otra regla declara fuente única. El costo se paga cuando las dos divergen, y no hay regla normativa que diga cuál gana | El `Acuerdo-Equipo.md` de la Fase A enumeró los ocho criterios de la condición de terminado, cinco fases antes de que la 08 emitiera la suya |
| Dejar la referencia colgada | Cumple la letra —hay una referencia— y nada más. Un audit que compruebe «existe referencia explícita a la DoD canónica» la da por satisfecha, con lo cual **el hueco queda sellado por el mecanismo que debería detectarlo** | Las condiciones de listo de la Fase D |
| Una nota en prosa dentro del artefacto | Tiene el mismo peso visual que el resto del texto y no interrumpe a nadie. «Una decisión pendiente que no interrumpe no es una decisión pendiente: es una nota» | Cuatro documentos de la 03 declararon el conflicto de los valores de respuesta; el Product Owner se enteró dos días después y por otra vía |

## 4. Correcciones propuestas

### G3-A · Propagación por iteración

**Archivo:** `Maqueta-Rules.md` §3.5 y §3.6.
**Severidad:** minor.

Al cerrar cada iteración del ciclo de corrección, el orquestador propaga lo aprobado en ella **o
registra explícitamente qué queda diferido y por qué**, en la bitácora de validación que §3.5 ya
exige. La propagación al cierre de la fase deja de ser el único evento y pasa a ser el cierre de lo
diferido.

Efecto: cada iteración deja la documentación consistente, o deja escrito que no lo está. Y un audit
que corra en el intervalo puede distinguir deriva legítima de omisión, que hoy es imposible porque la
documentación no lleva ninguna marca de que hay una fase abierta sobre ella.

### G3-B · Regla de escape de la matriz, fila nueva y el manifiesto en la regla de corte

**Archivo:** `Maqueta-Rules.md` §3.6.
**Severidad:** minor.

1. **Regla de escape.** Todo hallazgo de la validación que no encaje en ninguna fila de la matriz se
   declara explícitamente, con su destino propuesto, en lugar de no propagarse. Una matriz cerrada
   sin escape convierte cada caso no previsto en una omisión silenciosa; con escape, deja de
   comportarse como la lista de lo único que existe.
2. **Fila nueva: «la validación creó un proyecto de código»**, con sus destinos declarados —intake
   §13, §16, §17, §18; manifiesto §2, §3 y §4; y una corrida de Fase B para el árbol nuevo—.
3. **La regla de corte nombra el `PRODUCT-MANIFEST`**: tocar §13 del intake obliga a rederivarlo en
   la misma corrida. Hoy la regla nombra el intake y no su derivado.
4. **Las afirmaciones derivadas del manifiesto se revisan al rederivarlo.** El caso real: §3 del
   manifiesto declaraba «la cadena es lineal, de modo que no hay proyectos de código paralelizables:
   cada nivel tiene exactamente uno», y con el proyecto nuevo el nivel 0 pasó a tener dos. Es una
   afirmación derivada que nadie revisó porque nadie la marcó como derivada, y engancha con G2-C.

### G3-C · Conjuntos cerrados marcados, y detención cuando hay que extenderlos

**Archivos:** `Rules-Especificacion-Funcional.md` §3.2 y §4.2; `Maqueta-Rules.md` §3.6;
`Master-Prompt.md` §7 y §10.
**Severidad:** minor.

1. Toda categoría **marca explícitamente sus conjuntos cerrados** —valores de un campo, estados de
   una entidad, códigos de resultado, clasificaciones— en lugar de enumerarlos en prosa. Un conjunto
   cerrado marcado es identificable, y por lo tanto verificable entre categorías.
2. Cuando una categoría necesita extender un conjunto cerrado de otra, el orquestador **se detiene**
   y lleva la pregunta al humano, en lugar de admitir una nota dentro del documento. La decisión
   llega a quien la puede tomar en el momento en que aparece.
3. Los audits incorporan una **verificación cruzada entre categorías** sobre los conjuntos cerrados
   marcados, y la divergencia es hallazgo **P0**. Es lo que hace que la contradicción deje de
   sobrevivir a los audits, que hoy verifican cada categoría contra su regla y su upstream y ninguno
   cruza dos buscando contradicciones sobre un mismo referente.
4. La matriz de propagación distingue **propagar** de **contradecir**: el segundo caso tiene
   procedimiento propio y no se trata como si fuera agregar un flujo alternativo.

### G3-D · Registro único de decisiones pendientes del producto

**Archivos:** `Master-Prompt.md` §7 y §12; `Root-Rules.md` para el artefacto.
**Severidad:** minor.

Un artefacto de nivel producto, `SDD/Docs/Producto/Decisiones-Pendientes.md` —en el caso degenerado,
`SDD/Docs/Decisiones-Pendientes.md`—, fuera de los documentos que originan las decisiones, que el
orquestador exhibe **al cerrar cada fase** y no solo en el handoff.

**Por qué esta y no otra.** El reporte 03 §8 declara que de sus cinco propuestas la P-4 es la que más
lejos llega: «el problema que este reporte describe es, en el fondo, que el framework no tiene dónde
poner una pregunta abierta». Y esa corrida produjo siete preguntas más, todas viviendo dentro de un
archivo de datos de la maqueta.

**Lo que ya existe y hay que reusar, no duplicar.** `Master-Prompt.md` §12 ya declara un bloque
«Decisiones pendientes» en el resumen ejecutivo del handoff: «ambigüedades no resueltas, ADRs sin
cerrar, secciones `Por confirmar` y bloqueos a despejar antes de codear». Lo que falta no es el
concepto: es que exista como artefacto durante toda la corrida y no como bloque de un resumen final.
El bloque de §12 pasa a **leerse** del artefacto.

### G3-E · Referencia pendiente y reapertura que trae el insumo

**Archivos:** `Master-Prompt.md` §6 y §10; `Root-Rules.md` para la forma de la referencia.
**Severidad:** minor.

1. **Estado de referencia pendiente.** Un artefacto puede referenciar algo que todavía no existe si
   declara que no existe, cuál es su origen provisorio y cuándo se resuelve. Es lo que el destino
   improvisó, y su ventaja es que entonces hay algo que el audit puede verificar. La forma tiene
   evidencia a favor: la única matriz que ya existía declaraba literalmente «No hay sondas `VER-XX` …
   esa categoría todavía no se generó», y al generarse pasó a declarar cuántas hay. El documento
   avisó de su propia carencia y la carencia se cerró sin fricción.
2. **Reapertura obligatoria, con el insumo.** Cuando la categoría N se emite, las categorías que la
   referenciaban en estado pendiente vuelven a la cola para cerrar la referencia. Y la reapertura
   **trae consigo el insumo que faltaba**, no solo el turno: reabrir la 08 sin darle acceso a los
   contratos de la 10 la deja igual de imposibilitada. En la corrida real las cuatro matrices
   faltantes las emitió el generador de la categoría 10 —resultado correcto, vía incorrecta—, y el
   propio reporte 07 §9 declara por qué importa: «que el resultado sea correcto no vuelve correcta la
   vía por la que se produjo, porque la próxima vez que ese artefacto haya que regenerarlo no va a
   estar claro quién lo hace».
3. **Comprobación mecánica del grafo de obligaciones.** Recorrer las reglas de categoría buscando
   toda frase de la forma «referencia a la categoría N» o «según lo que declare N», y contrastar la
   fase de la categoría que la contiene con la fase de N. Es de una sola pasada y produce la lista
   completa en lugar de los cuatro casos que una corrida encontró. El propio reporte 07 §6 advierte
   que no hay razón para suponer que la lista esté cerrada.

### G3-F · La fuente única, donde el audit la pueda exigir

**Archivos:** `Rules-Calidad-Y-Pruebas.md` §6; `Rules-Contexto.md` §4.2 y §6.
**Severidad:** minor.

1. La declaración «`Definition-Of-Done.md` es la única fuente; los sprint plans referencian, no
   redefinen» sube del prompt-snippet §8 a un **criterio de aceptación de §6**, reformulada para
   alcanzar a cualquier artefacto y no solo a los planes de sprint.
2. `Rules-Contexto.md` gana el criterio que le falta: el `Acuerdo-Equipo.md` **referencia** la
   condición de terminado, no la enumera; y mientras la 08 no exista, lo hace con la forma de
   referencia pendiente de G3-E.

Esto cierra el hueco entre las dos reglas donde cayó el incidente A. La propuesta 4 del reporte 07
—adelantar la condición de terminado a la Fase A— se descarta: es la más profunda de las cinco y
reordena el contenido de dos categorías, y el propio reporte declara que el orden de fases por
dependencia de contenido es correcto. Queda registrada en §6.

## 5. Cómo se verifica

| Caso | Entrada | Resultado esperado | Origen |
|---|---|---|---|
| V1 | Una Fase B2 con al menos dos iteraciones | Al cerrar la primera hay propagación aplicada o diferimiento declarado | 02.1 |
| V2 | Estados demostrados por la maqueta contra declarados en los wireframes | Coinciden al cerrar **cada iteración**, no solo la fase | 02.2 |
| V3 | Un hallazgo que no encaja en ninguna fila de la matriz | Se declara con su destino propuesto, en lugar de perderse | 02.3 |
| V4 | Tocar §13 del intake | Produce versión nueva del manifiesto en la misma corrida | 02.4 |
| V5 | Una maqueta que necesita un valor que la 02 no admite | El orquestador **se detiene**, en lugar de escribir una nota | 03.1 |
| V6 | Un árbol con dos categorías que declaran conjuntos incompatibles sobre el mismo referente | El audit lo reporta como **P0** | 03.2 |
| V7 | El cierre de una fase | Existe lista de decisiones pendientes fuera de los documentos, y su recuento coincide con lo que los documentos declaran como propuesta | 03.3 |
| V8 | La comprobación mecánica del grafo de obligaciones | Devuelve cero obligaciones hacia una fase posterior, o devuelve la lista y cada una tiene su tratamiento declarado | 07.1 |
| V9 | Búsqueda de los criterios de la condición de terminado en las categorías 00, 06 y 07 | Devuelve referencias y ninguna enumeración | 07.2 |
| V10 | Un artefacto con referencia pendiente que no lo declara con esa forma | El audit de su fase **falla** | 07.3 |
| V11 | El cierre del producto con una referencia pendiente todavía abierta | Una comprobación falla | 07.4 |
| V12 | La reapertura de la categoría N que necesita un insumo de la categoría M | La reapertura lo trae, y ningún artefacto de una categoría queda emitido por la herramienta de otra | 07.5 |

El caso V5 es el que decide la familia: el desenlace real de esa corrida —el Product Owner resolvió a
favor de los tres valores dos días después y por una pregunta suya— es exactamente lo que una
corrección del framework tendría que haber producido «dos días antes y sin que el humano tuviera que
preguntar».

## 6. Qué queda fuera de esta corrección, y por qué

**Adelantar la condición de terminado a la Fase A** (propuesta 4 del reporte 07). Resolvería el
incidente A de raíz en vez de administrarlo, y es la intervención más profunda de las cinco: mueve
contenido entre dos categorías y toca el orden que los tres reportes declaran correcto. G3-E y G3-F
lo administran bien y a costo minor. Queda registrada como candidata para una intervención propia.

**La marca de fase en curso sobre los documentos alcanzados** (propuesta P-5 del reporte 02). G3-A la
vuelve innecesaria en su caso principal: si cada iteración propaga o declara su diferimiento, la
distinción entre deriva legítima y omisión ya está escrita. Se descarta por redundante, no por
incorrecta.

**El alcance real del grafo de obligaciones.** G3-E punto 3 declara la comprobación; correrla sobre
las diecisiete reglas y tratar cada caso encontrado excede esta intervención, porque cada obligación
detectada exige decidir si se administra con referencia pendiente, si se reordena o si la regla
estaba mal. Se aplica el mecanismo y se declara la corrida como trabajo siguiente.
