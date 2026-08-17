# Reporte 00 — Regla de transcripción y coherencia interna de los anexos de datos

| Campo | Valor |
|---|---|
| Reporte | 00 |
| Fecha | 2026-08-09 |
| Origen | Corrida real del orquestador de generación sobre el destino `Repos-RPIs/RPI.VidelControl` |
| Versión del framework evaluada | SDD 6.0 (`Master-Prompt` 5.2, `Intake-Rules` 3.2, `PRODUCT-INTAKE-template` 2.1) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Intake-Rules.md`, `SDD/Devs/Intake/PRODUCT-INTAKE-template.md`, `SDD/Devs/Orchestrator/Master-Prompt.md` |
| Naturaleza | Tres huecos normativos verificados con un caso real, con su propuesta de corrección |
| Estado | **RESUELTO** — aplicado sobre el framework en **SDD 7.0**. Ver «Cómo se resolvió», al final |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**. Cada hueco declara: el síntoma, la evidencia reproducible, por qué la normativa vigente no lo atrapa citando la sección exacta, el impacto, la corrección propuesta y la severidad de versión que implica. La última sección propone cómo verificar que la corrección funcionó.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. El caso que lo originó](#2-el-caso-que-lo-originó)
- [3. Hueco A — la coherencia interna de un escenario no se valida](#3-hueco-a--la-coherencia-interna-de-un-escenario-no-se-valida)
- [4. Hueco B — no hay regla de transcripción fiel](#4-hueco-b--no-hay-regla-de-transcripción-fiel)
  - [4.1 La regla, tal como se aplicó](#41-la-regla-tal-como-se-aplicó)
- [5. Hueco C — nada vuelve a mirar el intake después de aprobado](#5-hueco-c--nada-vuelve-a-mirar-el-intake-después-de-aprobado)
- [6. Qué no es un fallo del framework](#6-qué-no-es-un-fallo-del-framework)
- [7. Propuestas de intervención](#7-propuestas-de-intervención)
- [8. Cómo verificar que la corrección funcionó](#8-cómo-verificar-que-la-corrección-funcionó)
- [9. Anexo — evidencia reproducible](#9-anexo--evidencia-reproducible)

---

## 1. Resumen

Un escenario de la Parte D de un `PRODUCT-INTAKE` declaró **once** elementos en su bloque de prosa y enumeró **nueve** en el bloque contiguo del mismo escenario, mientras la fuente que transcribía enunciaba **nueve** y contenía en realidad **diez**. Ninguno de los tres números era el correcto, y el intake se contradecía a sí mismo dentro de un mismo escenario, separado por veinte líneas.

El intake pasó la validación de completitud de `Intake-Rules.md` §5 sin observaciones, el orquestador derivó el manifiesto, generó la Fase A y el número incorrecto se propagó a un criterio de transición de fase del roadmap y a una métrica de éxito de una necesidad de negocio. El audit de fase lo detectó, lo clasificó **P3** y lo declaró «aguas arriba, no es defecto de la fase» —clasificación correcta según el alcance que la normativa vigente le da al auditor— con lo cual el defecto quedó registrado como menor y camino a convertirse en caso de prueba de la categoría 08.

Los tres huecos que el caso expone:

| Id | Hueco | Artefacto a intervenir | Severidad propuesta |
|---|---|---|---|
| A | La validación del intake no compara lo que la prosa de un escenario afirma contra lo que su propio payload contiene | `Intake-Rules.md` §5 | minor |
| B | No existe regla de transcripción fiel: qué hacer cuando la transcripción de una fuente arroja un conteo distinto del que la fuente enuncia | `PRODUCT-INTAKE-template.md` §20 e `Intake-Rules.md` §5 | minor en la regla; minor en la plantilla |
| C | Un defecto originado en el intake y detectado por un audit de fase no tiene vía de escalamiento: se clasifica por su impacto en la fase, no por su origen | `Master-Prompt.md` §10 | minor |

Ninguno de los tres exige tocar una invariante D1 a D9 ni el conjunto cerrado D8.

---

## 2. El caso que lo originó

### 2.1 La fuente

`Repos-RPIs/RPI.VidelControl.Documentacion/Analisis/01-Analisis-Relevamiento-Contexto-Existente/07-Actuadores-Locales.md` §4, líneas 539 a 561, contiene una tabla de nueve filas que describe las ventanas horarias que un planificador del sistema operativo ejecuta sobre una salida digital. Dos de sus filas llevan **dos valores en una misma celda**:

```text
| Ventana        | Encendido     | Apagado       |
| Mañana         | 11:00         | 11:05         |
| Tarde (prueba) | 15:30 · 16:00 | 15:35 · 16:30 |   <- una fila, dos ventanas
| Noche          | 20:00 · 20:01 | 20:37         |   <- una fila, una ventana con dos encendidos
| ...            |               |               |
```

La misma fuente enuncia en prosa «**nueve** ventanas horarias» en tres lugares: `07-Actuadores-Locales.md` línea 576, `09-Despliegue-y-Operacion.md` línea 210 y `11-Observaciones-y-Evidencias.md` línea 352. Ese número cuenta **filas**, no ventanas.

### 2.2 Lo que el intake transcribió

En `Repos-RPIs/RPI.VidelControl/SDD/Intake/PRODUCT-INTAKE-Videocontrol-De-Camaras-Y-Actuadores.md`, versión 1.8, escenario `§20.E-6`:

- El bloque **«Qué ejercita»** decía «nueve ventanas con duraciones de 5, 17, 25, 26, 30 y 37 minutos», copiando la prosa de la fuente.
- El bloque **JSON** enumeraba **once** entradas, porque desdobló las dos celdas agrupadas por igual: trató `20:00 · 20:01` como dos ventanas cuando es una ventana con un encendido redundante.
- El bloque **«Qué verificar»** decía «Las once ventanas se pueden cargar sin pérdida de información».

El conteo correcto es **diez**: nueve filas, de las cuales la de «Tarde (prueba)» agrupa dos ventanas reales.

### 2.3 Un hallazgo colateral que la transcripción fiel habría expuesto antes

Al contar las duraciones de las diez ventanas aparece una de **40 minutos** (21:20 a 22:00) que **no figura** entre las que la propia fuente enumera (5, 17, 25, 26, 30, 37). Y la fuente advierte, en el mismo apartado, que «una entrada rotulada a las 21:20 programa las 17:13» porque los comentarios del archivo de horarios se copiaron sin actualizar, y que «vale el campo de horario, no el comentario».

Es decir: la fila dudosa era detectable **desde la propia fuente**, cruzando su tabla con su lista de duraciones. Una regla de transcripción que obligue a reproducir el conteo de la fuente y declarar la diferencia habría hecho aflorar esa fila en el momento de escribir el intake, no dos fases después.

### 2.4 Cómo se propagó

| Documento generado | Qué afirmaba | Consecuencia |
|---|---|---|
| `SDD/Docs/00-Contexto/Roadmap-Producto.md` §5.4 | «Las once ventanas horarias se pueden expresar como reglas sin pérdida de información» | Criterio de transición entre fases con un número incorrecto |
| `SDD/Docs/01-Necesidades-Negocio/Necesidades-De-Negocio/NB-06-…md` §1 y §5 | «once ventanas»; criterio de éxito «11 de 11» | Métrica de éxito de una necesidad de negocio, insumo directo de la categoría 08 |

### 2.5 Qué detectó el audit, y con qué alcance

El informe `SDD/Docs/Audit/Audit-Fase-A.md` registra el hallazgo **H-18**, nivel **P3**, con el texto «no es defecto de la Fase A». Y en su línea 95 el propio auditor declara el límite de su alcance:

> El contenido del intake y del manifiesto no se audita: se usan como referencia para verificar derivación. Las incoherencias internas del intake detectadas al contrastarlas se registran como observación.

La clasificación es **correcta** según la normativa vigente. El problema no es el auditor: es que la normativa no le da ninguna vía distinta de P3 para un defecto cuyo origen está aguas arriba y cuya propagación aguas abajo ya ocurrió.

---

## 3. Hueco A — la coherencia interna de un escenario no se valida

**Síntoma.** Un escenario de la Parte D puede afirmar en prosa un hecho que su propio payload contradice, y pasar íntegra la validación de intake.

**Qué valida hoy la normativa.** `Intake-Rules.md` §5, viñeta «Anexos de datos (Parte D)», exige que cada escenario declare procedencia, un `Estado` del enum cerrado y sus cuatro bloques —contexto, qué ejercita, JSON completo y qué verificar—. La misma sección agrega la **regla de resolución** (todo identificador citado existe y todo anexo está citado) y la **regla de autocontención** (ningún dato se respalda sólo en una referencia externa).

**Qué no valida.** Nada compara los bloques entre sí. La validación verifica que los cuatro bloques **existan**; no que **digan lo mismo**. Un escenario con «nueve» en un bloque y once entradas en el siguiente cumple los cuatro requisitos.

**Por qué importa más de lo que parece.** Los cuatro bloques de un escenario tienen consumidores declarados distintos: `02-Especificacion-Funcional` toma el modelo, `08-Calidad-Y-Pruebas` toma el bloque «qué verificar» como criterio de aceptación y `10-Examples` lo convierte en contrato de verificación. Si los bloques se contradicen, **cada consumidor aguas abajo cree una cosa distinta**, y ninguno tiene forma de saber que el otro leyó otra.

**Corrección propuesta.** Agregar a `Intake-Rules.md` §5, dentro de la viñeta de la Parte D, una validación de **coherencia intra-escenario**: toda magnitud, conteo o enumeración que la prosa de un escenario enuncie tiene que coincidir con lo que su payload contiene, o el escenario tiene que declarar explícitamente por qué difieren. La discrepancia no declarada es bloqueante; la declarada es un dato más del escenario.

**Severidad.** Minor sobre `Intake-Rules.md`. Agrega una validación sin cambiar la estructura de los artefactos que gobierna; un intake ya emitido no deja de cumplir por su forma, aunque pueda fallar la validación nueva, que es exactamente el efecto buscado.

---

## 4. Hueco B — no hay regla de transcripción fiel

**Síntoma.** La Parte D existe para transcribir datos de una fuente, y la normativa no dice qué hacer cuando la transcripción produce un conteo distinto del que la fuente enuncia. Quien transcribe elige uno de los dos números en silencio, o inventa un tercero sin advertirlo.

**Qué dice hoy la normativa.** `PRODUCT-INTAKE-template.md` §20 exige transcribir «con su JSON completo y sin recortar», prohíbe inventar datos —«si un escenario citado no existe en las fuentes, no se crea»—, fija el enum de `Estado` y marca como anti-patrón presentar datos sintéticos como medidos.

**Qué falta.** Todo eso gobierna la relación entre el dato y su existencia. Nada gobierna la relación entre el dato y **la afirmación que la fuente hace sobre ese dato**. Y el caso es frecuentísimo: cualquier tabla con celdas agrupadas, cualquier lista cuyo encabezado declare una cantidad, cualquier fuente que enuncie un total en prosa y lo detalle en una tabla.

**Por qué no lo cubre la regla de no invención.** Porque nada se inventó: la tabla se copió fielmente. Lo que se derivó mal fue **el conteo sobre la tabla**, que es una operación intermedia que la normativa no nombra. El enum de `Estado` tampoco lo cubre: el escenario era legítimamente `medido` en cuanto a sus filas, y su conteo era `derivado`. Hoy un escenario declara un solo `Estado` para todo su contenido.

**Corrección propuesta.** Incorporar a `PRODUCT-INTAKE-template.md` §20 una **regla de transcripción fiel** con tres obligaciones:

1. Si la fuente enuncia un conteo, un total o una cardinalidad, el escenario lo reproduce tal como la fuente lo enuncia.
2. Si la transcripción arroja un valor distinto, el escenario declara **los dos valores y la razón de la diferencia**, en un bloque propio, en lugar de elegir uno.
3. Toda magnitud que el escenario derive de la fuente en lugar de copiarla se marca como derivada, con su regla de cálculo, aunque el escenario sea `medido` en su conjunto.

Y agregar en `Intake-Rules.md` §5 la validación correspondiente, que es la de Hueco A aplicada a este caso.

### 4.1 La regla, tal como se aplicó

Esta subsección es la evidencia principal del hueco B: la regla que faltaba **existe y funciona**, porque hubo que inventarla a mano para poder cerrar el caso. Se enuncia así:

> **Regla de transcripción fiel.** Si la fuente enuncia un número y tu transcripción arroja otro, **declarás los dos y por qué difieren**, en lugar de elegir uno.

Es una regla barata de cumplir y de verificar, y es la única que resuelve el caso sin obligar a nadie a decidir cuál de las dos fuentes miente. La diferencia entre nueve y diez no era un error de ninguna de las dos: eran dos preguntas distintas —cuántas filas tiene la tabla, cuántas ventanas describe— que producían dos números legítimos. Elegir uno destruye información; declarar los dos la conserva y además **expone la pregunta que nadie había hecho**.

**Antes**, en el escenario `§20.E-6` del intake versión 1.8. Los dos bloques del mismo escenario, separados por veinte líneas:

```text
Bloque «Qué ejercita»:
  «... nueve ventanas con duraciones de 5, 17, 25, 26, 30 y 37 minutos ...»

Bloque JSON:
  "ventanas": [ ...once entradas... ]

Bloque «Qué verificar»:
  «2. Las once ventanas se pueden cargar sin pérdida de información,
      incluidas las dos que se solapan a las 20:00 y 20:01 sobre el mismo recurso.»
```

Tres afirmaciones sobre lo mismo, dos números distintos, y un tercero —diez, el correcto— que no aparecía en ninguna.

**Después**, en la versión 1.9, aplicando la regla. El escenario dejó de afirmar un número y pasó a declarar los tres con su razón:

```json
"conteo": {
  "filas_de_la_tabla_fuente": 9,
  "ventanas_derivadas": 10,
  "por_que_no_coinciden": "la fila «Tarde (prueba)» agrupa dos ventanas en una sola fila; las tres menciones en prosa de las fuentes («nueve ventanas») cuentan filas, no ventanas",
  "encendidos_totales": 11,
  "por_que_hay_un_encendido_mas": "la ventana de las 20:00 tiene dos encendidos, 20:00 y 20:01, y un solo apagado; es un encendido redundante sobre la misma ventana y no una ventana adicional"
}
```

**Qué produjo aplicarla, más allá de corregir el número.** Tres cosas que la elección silenciosa de un número no habría producido:

1. **Quedó explicado por qué la fuente dice nueve.** No es un error de la fuente: cuenta filas. Un lector futuro que abra el relevamiento y lea «nueve» ya no va a creer que el intake se equivocó.
2. **Apareció la fila dudosa.** Al enumerar las diez ventanas con su duración, una de 40 minutos no figuraba entre las que la fuente enumera, y la propia fuente advertía que ese rótulo está desfasado del horario real. Quedó marcada en el escenario con su motivo, para resolverse releyendo el equipo. Esa fila era invisible mientras el conteo fuera un número suelto.
3. **El criterio de verificación dejó de ser frágil.** El bloque «qué verificar» pasó de «las once ventanas se cargan sin pérdida» —que es falso— a «las diez ventanas se cargan sin pérdida, incluido el encendido redundante de las 20:01, que no constituye una ventana aparte». Lo que la categoría 08 va a convertir en caso de prueba ahora describe el sistema real.

El tercero es el que justifica que la regla sea normativa y no una buena práctica: **el conteo de un escenario no es un dato ornamental, es un criterio de aceptación en camino**.

**Forma sugerida del bloque.** Es la que la corrección real de este caso terminó adoptando, y sirve de referencia:

```json
"conteo": {
  "filas_de_la_tabla_fuente": 9,
  "ventanas_derivadas": 10,
  "por_que_no_coinciden": "la fila «Tarde» agrupa dos ventanas; las menciones en prosa de la fuente cuentan filas, no ventanas",
  "encendidos_totales": 11,
  "por_que_hay_un_encendido_mas": "una ventana tiene dos encendidos y un solo apagado: es redundante, no una ventana aparte"
}
```

**Severidad.** Minor sobre la plantilla y minor sobre la regla. Incorpora una exigencia nueva sin reestructurar el artefacto. Conviene evaluar si un intake ya emitido con un conteo transcripto sin declarar deja de cumplir: si la respuesta es sí, es major sobre la plantilla y arrastra al conjunto según `SDD-Development-Guide.md` §VI.1.

---

## 5. Hueco C — nada vuelve a mirar el intake después de aprobado

**Síntoma.** Un defecto que se origina en el intake, pasa su validación, se propaga a documentación generada y es detectado por un audit de fase, se clasifica como hallazgo de la fase auditada. Como la fase reprodujo fielmente lo que el intake decía, el hallazgo cae en el nivel más bajo y se archiva.

**Qué establece hoy la normativa.**

- La validación de intake de `Intake-Rules.md` corre **una sola vez**, antes de la Fase A. `Master-Prompt.md` §8 lo declara: «la Fase de validación de intake del master-prompt invoca estas reglas una sola vez».
- El audit de `Master-Prompt.md` §10 audita **los entregables de la fase**, sus insumos upstream «que cita» y los archivos de reglas. El intake entra como referencia de derivación, no como objeto auditado.
- Los cuatro niveles de hallazgo —P0 a P3— están definidos por **impacto sobre la fase**: P0 rompe trazabilidad o viola invariantes, P1 incumple criterios de aceptación, P2 son ítems recomendados ausentes, P3 son mejoras de estilo. Ninguno contempla el eje «dónde se originó».

**La consecuencia exacta.** Un defecto upstream que la fase reproduce fielmente **no puede ser P0 ni P1**, porque la fase no incumplió nada: hizo bien su trabajo copiando algo incorrecto. Y como no es un ítem recomendado ausente ni una mejora de estilo, termina en P3 por descarte. El nivel no mide su gravedad: mide que el auditor no tenía dónde ponerlo.

En este caso el defecto viajaba hacia un criterio de aceptación de la categoría 08. Lo detuvo una lectura humana del informe, no la maquinaria.

**Corrección propuesta.** Agregar a `Master-Prompt.md` §10 una **clase de hallazgo ortogonal al nivel**: el hallazgo *aguas arriba*, que se marca además de su nivel y que declara el artefacto de origen. Con dos consecuencias:

1. El informe de audit lo lista aparte, con el artefacto upstream que hay que corregir.
2. El orquestador, al cerrar la fase, **presenta los hallazgos aguas arriba al humano antes de despachar la fase siguiente**, porque son los únicos que la corrección de la fase no puede resolver: corregir el entregable sin corregir el intake reintroduce el defecto en la próxima regeneración.

Alternativa más económica, si se prefiere no tocar la taxonomía: declarar en §10 que un hallazgo cuyo origen es el intake **eleva su nivel al que tendría el defecto en su artefacto de origen**, y no al que le corresponde por su impacto en la fase auditada.

**Severidad.** Minor sobre `Master-Prompt.md`. Agrega una clasificación y una detención acotada sin alterar el orden de fases ni la mecánica de confirmación.

---

## 6. Qué no es un fallo del framework

Conviene delimitarlo para que la intervención no sobrecorrija.

**La regla de evidencia funcionó, y fue lo que permitió resolverlo.** `PRODUCT-INTAKE-template.md` §20 obliga a que cada escenario declare procedencia con archivo y secciones. Gracias a eso se pudo volver a la fuente exacta, contar las filas y determinar cuál de los tres números era el correcto. Sin esa obligación, la discusión se habría resuelto por memoria o por autoridad.

**El audit entre fases detectó la incoherencia**, que es exactamente su función, y la documentó con los dos documentos afectados y sus secciones.

**La clasificación P3 fue correcta** bajo la normativa vigente, y el auditor declaró explícitamente el límite de su alcance en lugar de excederlo. Eso es comportamiento deseable: el defecto está en que la normativa no le ofrecía otra salida.

**La estructura de cuatro bloques por escenario es la que hizo visible la contradicción.** Si el escenario hubiera sido sólo un payload, los dos números no habrían convivido en el mismo artefacto y nadie habría notado nada. El hueco A existe porque la estructura es buena: hay dos afirmaciones sobre lo mismo y nada las compara.

---

## 7. Propuestas de intervención

Resumen operativo para el prompt de intervención. Cada fila declara el archivo, la sección, qué agregar y la severidad.

| # | Archivo | Sección | Qué agregar | Severidad |
|---|---|---|---|---|
| 1 | `SDD/Devs/Rules/Intake-Rules.md` | §5, viñeta de la Parte D | Validación de coherencia intra-escenario: toda magnitud enunciada en prosa coincide con el payload o la diferencia está declarada. Bloqueante si no está declarada | minor |
| 2 | `SDD/Devs/Intake/PRODUCT-INTAKE-template.md` | §20, formato por escenario | Regla de transcripción fiel con sus tres obligaciones, y el bloque de conteo como forma sugerida. Sumar el anti-patrón «conteo derivado presentado como transcripto» | minor, evaluar major |
| 3 | `SDD/Devs/Rules/Intake-Rules.md` | §5 y §7 | La discrepancia de conteo no declarada entra como bloqueante en §7; la declarada, como recomendado | minor, en la misma intervención que 1 |
| 4 | `SDD/Devs/Orchestrator/Master-Prompt.md` | §10 | Clase de hallazgo *aguas arriba*, con su listado aparte en el informe y la presentación al humano antes de despachar la fase siguiente | minor |
| 5 | `CHANGELOG.md` del framework | Entrada nueva | Registrar la intervención. Si 2 resulta major, el conjunto sube major y la entrada lleva el bloque «Impacto sobre destinos existentes» según `SDD-Development-Guide.md` §VI.4 | según 2 |

**Nota de coherencia.** La intervención alcanza a más de un archivo, de modo que `README.md` del framework exige emitir una nota de coherencia siguiendo el patrón de `Coherencia-Auditoria-Marco.md`: alcance, inventario, verificación de invariantes, trazabilidad, observaciones y veredicto.

**Qué documentación ya emitida deja de cumplir.** Si la propuesta 2 se resuelve como minor, ninguna. Si se resuelve como major, todo `PRODUCT-INTAKE` con Parte D que transcriba una fuente con conteos y no los declare; en la práctica, todos los emitidos hasta la 6.0.

---

## 8. Cómo verificar que la corrección funcionó

Casos de prueba para la validación nueva. Los tres primeros son el caso real y sus variantes; el cuarto es el criterio negativo, que evita que la corrección produzca falsos positivos.

| Caso | Entrada | Resultado esperado |
|---|---|---|
| V1 | Escenario cuya prosa dice «nueve» y cuyo payload enumera once, sin declaración | **Bloqueante**. La batería de `Intake-Rules.md` §6 reporta el escenario, los dos valores y los dos bloques donde aparecen |
| V2 | Escenario cuya prosa dice «nueve», su payload enumera diez y declara por qué difieren | **Pasa**. La diferencia declarada es un dato del escenario |
| V3 | Escenario cuya fuente enuncia un total que el escenario no reproduce en ningún lado | **Recomendado**, no bloqueante: la regla de transcripción fiel pide reproducirlo, pero su ausencia no contradice nada |
| V4 | Escenario cuya prosa menciona un número que **no es un conteo del payload** —una fecha, una versión, una medición— | **Pasa sin observación**. La validación compara conteos y enumeraciones, no cualquier número que aparezca en el texto |
| V5 | Audit de fase que detecta un defecto reproducido fielmente del intake | El informe lo lista como hallazgo aguas arriba, con el artefacto de origen, y el orquestador lo presenta antes de despachar la fase siguiente |

El caso V4 es el que decide si la corrección es aplicable: una validación que compare todos los números de la prosa contra todos los números del payload produce ruido y se desactiva sola. La comparación tiene que acotarse a **conteos y enumeraciones del propio payload**.

---

## 9. Anexo — evidencia reproducible

Los comandos se ejecutan desde la raíz del workspace que contiene `IA/IA.SDD`, `Repos-RPIs/RPI.VidelControl` y `Repos-RPIs/RPI.VidelControl.Documentacion`.

```bash
# La tabla de la fuente: nueve filas, dos de ellas con celdas agrupadas
sed -n '/^## 4. La operación desatendida/,/^Junto a esas entradas/p' \
  Repos-RPIs/RPI.VidelControl.Documentacion/Analisis/01-Analisis-Relevamiento-Contexto-Existente/07-Actuadores-Locales.md

# Las tres menciones en prosa que cuentan filas y no ventanas
grep -rn "nueve ventanas" \
  Repos-RPIs/RPI.VidelControl.Documentacion/Analisis/01-Analisis-Relevamiento-Contexto-Existente/*.md

# El hallazgo del audit, su nivel y su clasificación
grep -n "H-18" Repos-RPIs/RPI.VidelControl/SDD/Docs/Audit/Audit-Fase-A.md

# El límite de alcance que el propio auditor declara
sed -n '95p' Repos-RPIs/RPI.VidelControl/SDD/Docs/Audit/Audit-Fase-A.md

# Lo que la normativa vigente valida sobre la Parte D
sed -n '/Anexos de datos (Parte D)/,/Regla de choque/p' IA/IA.SDD/SDD/Devs/Rules/Intake-Rules.md

# Los cuatro niveles de hallazgo, definidos por impacto sobre la fase
sed -n '/^Niveles de hallazgo/,/^Path del informe/p' IA/IA.SDD/SDD/Devs/Orchestrator/Master-Prompt.md
```

El estado corregido del escenario, con el bloque de conteo que sirve de referencia de forma, está en `Repos-RPIs/RPI.VidelControl/SDD/Intake/PRODUCT-INTAKE-Videocontrol-De-Camaras-Y-Actuadores.md` §20.E-6, versión 1.9, y su corrección está registrada en el control de cambios de ese documento.

## Control de cambios

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 2026-08-09 | Reporte inicial. Tres huecos normativos verificados sobre la corrida de la Fase A del destino `RPI.VidelControl` bajo el framework 6.0, con su evidencia reproducible y su propuesta de intervención. |
| 1.1 | 2026-08-17 | Se marca **RESUELTO**: el reporte se aplicó en la **SDD 7.0** y se suma la sección «Cómo se resolvió», con dónde quedó escrito cada hueco y qué pasó después. |


---

## Cómo se resolvió

**Estado: RESUELTO.** Se aplicó sobre el framework en la intervención **SDD 7.0**, que trató los
**doce reportes `00` a `11` juntos** por ser de la misma corrida y alcanzar artefactos compartidos. Su
nota de coherencia es `SDD/Devs/Guides/Coherencia-Reportes-00-11.md`, con la trazabilidad reporte por
reporte en su §4.

**Qué resolvió, en una línea:** Transcripción fiel y coherencia intra-escenario de los anexos de datos, y la marca de hallazgo *aguas arriba*.

| Dónde se aplicó | Qué quedó escrito |
|---|---|
| `PRODUCT-INTAKE-template.md` §20 | La Parte D exige que todo escenario **transcriba** el dato completo y no lo referencie a un archivo externo |
| `Intake-Rules.md` §5 y §7 | La validación suma la **regla de coherencia intra-escenario**: los cuatro bloques de un escenario dicen lo mismo, y toda magnitud que la prosa enuncie coincide con el JSON |
| `Master-Prompt.md` §10 | El audit distingue el hallazgo **propio** del **aguas arriba**, que es el que no es del agente que lo encuentra |

**Lo que este reporte tenía en común con los otros once**, y que el `CHANGELOG.md` del framework dejó
registrado en su entrada `[7.0]`: **ninguno era un error de un agente**. En los doce, el agente cumplió
la regla que tenía, o la única que había no se podía cumplir sin inventar. Es la propiedad que los
volvió insumo de una intervención sobre el método, en lugar de una corrección sobre el destino.
