# Reporte 10 — Un criterio de aceptación que verifica la presencia de una declaración, y no su verdad, lo cumple un artefacto falso

| Campo | Valor |
|---|---|
| Reporte | 10 |
| Fecha | 2026-08-12 |
| Origen | Corrida real del orquestador sobre el destino `Repos-RPIs/RPI.VidelControl`: Fase G, categoría 10 de los cinco proyectos de código, 2026-08-12 |
| Versión del framework evaluada | SDD 6.0 (`Rules-Examples.md` §4.2, §6 y su propia fila 4.1 de control de cambios; por extensión, toda regla de categoría cuyos criterios de aceptación pregunten por la existencia de un artefacto) |
| Artefactos del framework alcanzados | `SDD/Devs/Rules/Rules-Examples.md`; el patrón alcanza a los criterios de aceptación de las diecisiete reglas de categoría |
| Naturaleza | Criterios de aceptación que preguntan si una declaración **está**, cuando la propiedad que importa es si lo que declara **es cierto**. Un artefacto vacío, o uno que declara algo falso, los cumple igual |
| Estado | Para evaluación. Ninguna modificación aplicada sobre el framework |
| Reportes relacionados | `09-El-Audit-Como-Unica-Compuerta.md`, que documenta el instrumento; éste documenta el criterio. `04-Recuentos-Declarados-En-Prosa.md`, que es el caso particular de este patrón aplicado a los números |

Este documento está escrito para ser **insumo de un prompt de intervención sobre el framework**.

## Tabla de contenido

- [1. Resumen](#1-resumen)
- [2. El incidente](#2-el-incidente)
- [2.1 Cómo se produce la afirmación falsa](#21-cómo-se-produce-la-afirmación-falsa)
- [3. El framework ya se chocó con esto una vez y lo arregló sin generalizarlo](#3-el-framework-ya-se-chocó-con-esto-una-vez-y-lo-arregló-sin-generalizarlo)
- [4. Lo que la normativa dice, con precisión](#4-lo-que-la-normativa-dice-con-precisión)
- [5. La causa raíz](#5-la-causa-raíz)
- [6. El patrón, enunciado](#6-el-patrón-enunciado)
- [7. Qué hizo el destino](#7-qué-hizo-el-destino)
- [8. Propuestas de intervención](#8-propuestas-de-intervención)
- [9. Cómo verificar que la corrección funcionó](#9-cómo-verificar-que-la-corrección-funcionó)
- [Control de cambios](#control-de-cambios)

---

## 1. Resumen

`Rules-Examples.md` §4.2.8 obliga a cada sample a declarar una tabla de trazabilidad con los casos de uso que ilustra, y §6 lo convierte en criterio de aceptación: «Cada sample declara trazabilidad a CU, ADR o NFR en §8 **con al menos una fila**».

El criterio pregunta si la tabla existe. No pregunta si es cierta.

En la primera emisión de esta categoría, **tres de los catorce contratos de verificación nombraban seis casos de uso que su comando no ejercita**. Los tres cumplían el criterio de §6 sin excepción: tenían su §8, con más de una fila, con identificadores que resuelven contra artefactos vigentes. Lo que no tenían era relación entre lo que la tabla afirma y lo que el sample hace. Y el único criterio de §6 que sí pregunta por esa relación —§4 lo detalla— **se cumple mejor cuanto más falsa es la declaración**.

El daño no es documental. La categoría 10 tiene una segunda arista: el contrato de verificación de §4.6 alimenta la matriz de sensado de deriva como sonda `VER-XX`, y esa sonda declara que verifica los casos de uso de la tabla. Una trazabilidad falsa se convierte en **cobertura declarada que no existe**: la matriz afirma que un caso de uso está sensado por una sonda que no lo toca.

## 2. El incidente

| Sonda | Qué declaró verificar | Qué hace su comando |
|---|---|---|
| `VER-02` de `VideoControl-Web` | `CU-25`, activar la publicación de la fuente de un set | Un `GET` al endpoint de video, que es `CU-01`. La activación es **precondición** del sample, no flujo ejercitado, y ninguna de sus aserciones la toca |
| `VER-73` de `VideoControl-Infrastructure` | `CU-76`, distribuir la imagen de una fuente, y `CU-81`, publicar la fuente de imagen de un set | Barrido I²C, movimiento de un eje, conmutación del relé y verificación del contrato de la cámara de red. Ninguno de los cuatro caminos es el distribuidor ni el servidor de publicación |
| `VER-71` de `VideoControl-Infrastructure` | `CU-71`, `CU-72` y `CU-73` | Los tres contrastan contra el sistema en ejecución y persisten el sellado; el sample declaraba, en la misma página, «sólo lee y escribe archivos» y «sin base previa» |

Los tres pasaron la emisión. Los tres los encontró un lector independiente leyendo el flujo principal de cada caso de uso y preguntándose si la salida prometida lo recorre. **Ningún criterio de §6 pregunta eso.**

### 2.1 Cómo se produce la afirmación falsa

El reporte documentaba el defecto y no el mecanismo, y el mecanismo importa porque es regular y se puede nombrar.

**No se inventa un caso de uso: se confunde precondición con flujo ejercitado.** Los tres incidentes tienen la misma forma. El sample **necesita** que ese caso de uso haya ocurrido para poder correr, y de «lo necesita» se pasa a «lo ejercita» sin que medie ninguna comprobación.

Se ve entero en `VER-02`. El sample es un cliente de una etiqueta contra el endpoint de video. Para que ande, alguien tiene que haber activado antes la publicación de la fuente del set, que es `CU-25`. La activación aparece dos veces en el sample —en sus prerequisitos y como paso de preparación— y de ahí saltó a la tabla de trazabilidad. El salto es la falla, y es cómoda de dar porque las dos cosas son ciertas por separado: el sample **sí** depende de `CU-25`, y `CU-25` **sí** está relacionado con lo que el sample hace.

Ahí actúa el segundo ingrediente: **la afinidad de tema**. El sample es de video, `CU-25` es de publicación de video, los dos hablan de fuentes y de sets. Al elegir por vecindad temática en lugar de por flujo, la elección se siente evidente y no se verifica. La reincidencia lo prueba mejor que el incidente original: al retirar `CU-76` y `CU-81` de `VER-73` se incorporó `CU-74` en su lugar, y el motivo escrito fue que el sample «liga proveedores reales» —cierto— cuando lo que hace es **enumerar** dispositivos, que es una comprobación previa a la ligadura y no la ligadura.

Y hay un tercer ingrediente, propio de la pasada de diseño: **no hay código que desmienta**. La salida prometida la escribe la misma persona que escribe la trazabilidad, en el mismo rato, y no hay una corrida que devuelva otra cosa. Un sample implementado se choca contra su propia salida; uno en pasada de diseño, no.

**Qué significa «la salida no recorre el flujo».** La salida prometida de §6 es la única evidencia observable sobre la que el contrato puede asertar. Un caso de uso está recorrido cuando alguna línea de esa salida **cambia como consecuencia** de un paso de su flujo principal, de modo que si ese paso se rompe, la aserción falla. Si ninguna línea depende de él, correr el sample y verificar su criterio no dice nada sobre ese caso de uso: el sample pasa igual con el caso de uso roto.

En `VER-02` es literal. El flujo de `CU-25` declara la publicación **sin iniciar conversión** (paso 3), la inicia recién cuando aparece un suscriptor (paso 4) y registra el consumo contra el umbral de alarma (paso 5). Las cuatro líneas de la salida prometida son dos códigos de respuesta, una afirmación sobre el circuito de la interfaz y un piso de fotogramas por segundo. Ninguna distingue una fuente recién convertida de una que ya venía convirtiendo, y ninguna observa consumo ni umbral. Con `CU-25` enteramente roto —con la conversión arrancando siempre, o sin registrar nada— el sample da exactamente la misma salida y su criterio se cumple igual.

## 3. El framework ya se chocó con esto una vez y lo arregló sin generalizarlo

Esto es lo que hace al reporte algo más que un hallazgo. La fila 4.1 del control de cambios de `Rules-Examples.md` dice, textualmente, sobre el criterio de glosario:

> **Origen**: el audit verificaba «glosario sin contradicciones», que un glosario incompleto cumple trivialmente, y esta regla no mencionaba la palabra «glosario» ni una vez.

Es exactamente este patrón, reconocido y nombrado por el propio framework: un criterio que un artefacto deficiente satisface sin esfuerzo. La intervención de la versión 4.1 fue **agregar tres criterios de glosario a esa regla**. Correcta y local.

Lo que no ocurrió fue la pregunta siguiente: *¿qué otros criterios de esta regla, y de las otras dieciséis, se satisfacen trivialmente?* Si se hubiera hecho, el criterio de trazabilidad de §6 habría aparecido en la misma pasada, porque tiene la misma forma: pregunta por la presencia de una tabla, no por la verdad de sus filas.

## 4. Lo que la normativa dice, con precisión

`Rules-Examples.md` §6 tiene **veintiséis** criterios de aceptación: catorce de la categoría y doce propios de la arista B. Dos de ellos hablan de la trazabilidad, y la relación entre los dos es lo que hace al caso.

**El primero pregunta por la presencia.** «Cada sample declara trazabilidad a CU, ADR o NFR en §8 **con al menos una fila**». Los tres incidentes lo cumplían: tenían su §8, con más de una fila, con identificadores que resuelven contra artefactos vigentes.

**El segundo sí pregunta por una relación, y apunta para el otro lado.** «Todo CU declarado crítico en 02 tiene al menos una sonda `VER-XX` **que lo ejercita**, o la ausencia está justificada». El verbo es el correcto —ejercitar, no nombrar— pero la dirección es la contraria a la que hace falta: pregunta si todo caso de uso crítico está cubierto, no si toda cobertura declarada es cierta.

Y ahí está lo peor del caso, que conviene decirlo sin rodeos:

> **Una trazabilidad falsa no evade el único criterio que pregunta por la relación: lo ayuda a cumplirse.** Un sample que declara ejercitar `CU-25` sin ejercitarlo hace que `CU-25` figure como cubierto por una sonda. El criterio se satisface **mejor** cuanto más falsa es la declaración, porque lo que cuenta es cuántos casos de uso quedan sin sonda, y una sonda mentirosa reduce esa cuenta igual que una verdadera.

Un criterio que se cumple mejor con un artefacto falso que con uno honesto no es un criterio débil: es un criterio con el signo cambiado.

Nótese además dónde está escrita la pregunta que sí hubiera atajado el defecto. §5.5 —las preguntas guía del subagente— dice: «¿Cada sample declara qué verifica, y no sólo qué ilustra?» y «¿Hay dos sondas que verifican lo mismo?». Son las preguntas correctas, están bien formuladas, y viven en la sección que **no** es criterio de aceptación. §5 orienta a quien redacta; §6 es lo que el audit evalúa. Lo que hubiera detenido esto está del lado que no bloquea.

## 5. La causa raíz

Un criterio de aceptación se escribe, casi siempre, mirando el artefacto que se quiere obtener: «que exista la tabla», «que tenga al menos una fila», «que declare su nivel». Es la forma natural de escribirlo y produce criterios verificables de un vistazo.

La propiedad que importa, en cambio, casi nunca es del artefacto solo: es de la **relación** entre el artefacto y otra cosa. Que la tabla de trazabilidad sea cierta es una relación entre el sample y el caso de uso. Que un glosario esté completo es una relación entre el glosario y los términos usados. Que un recuento sea correcto es una relación entre el número y la colección que cuenta —y ése es el reporte 04, que resulta ser el caso particular de este patrón aplicado a los números.

Verificar una relación cuesta más que verificar una presencia: hay que leer los dos lados. Por eso los criterios derivan hacia la presencia, y por eso el derive es sistemático y no un descuido de una regla en particular.

## 6. El patrón, enunciado

> **Los criterios de aceptación del framework preguntan mayoritariamente si una declaración está presente, cuando la propiedad que hace útil a la declaración es que sea verdadera. Un artefacto que declara algo falso los cumple exactamente igual que uno que declara algo cierto, de modo que el criterio no discrimina entre los dos casos que existe para distinguir. El framework ya reconoció una instancia de esto —en su propia fila de control de cambios— y la reparó localmente, sin preguntarse cuántas más había.**

Un corolario, porque es el que decide dónde intervenir:

> **La pregunta correcta suele estar escrita en la regla, del lado que no bloquea.** En `Rules-Examples.md` la pregunta que hubiera atajado los tres incidentes está en §5.5, que orienta al que redacta, y no en §6, que es lo que el audit evalúa. Mover preguntas de §5 a §6 es más barato que inventarlas, y en esta regla hay al menos cuatro candidatas.

## 7. Qué hizo el destino

Corrigió los tres contratos: `VER-02` dejó de nombrar `CU-25` y lo declaró como precondición con la frase «la activación es estado previo y no algo que este sample ejercite»; `VER-73` retiró `CU-76` y `CU-81`, e incorporó `CU-74` con un argumento verificable —es el único sample que liga proveedores reales, y el sample vecino afirma explícitamente no instanciar ninguno—; y `VER-71` reescribió sus precondiciones para declarar la base temporal que efectivamente necesita.

Ninguna de las tres correcciones la produjo un criterio del framework. Las produjo un lector independiente, y la primera tanda de correcciones **volvió a cometer el defecto**: al retirar `CU-76` y `CU-81` de un contrato se incorporó `CU-74` en su lugar, por afinidad de tema, y la ronda siguiente encontró que tampoco lo recorría. Ocho de los doce hallazgos interpretativos de esa segunda ronda fueron el mismo defecto.

Ahí el destino dejó de corregir de a uno y cambió la forma de la afirmación. Ahora ningún caso de uso se puede declarar sin decir, en la misma tabla, **qué pasos de su flujo principal recorre la salida prometida y cuáles no**, y el generador se niega a emitir un sample que declare un caso de uso sin esa frase. El cambio no verifica la verdad —eso sigue exigiendo leer los dos documentos— pero convierte una afirmación vaga en una falsable: «ilustra CU-25» no se puede refutar sin discutir qué significa ilustrar, mientras que «recorre los pasos 3 a 7 y no el 2» se refuta leyendo siete renglones.

El resultado se vio en el acto. Al exigirlo, cuatro casos de uso declarados no sobrevivieron a la pregunta y se retiraron —`CU-53` de un sample de aplicación, `CU-11` de uno de Web, `CU-74` del sample de hardware—, y uno se declaró con su cobertura parcial escrita: `CU-53` en el planificador recorre el paso 6, el de solapamientos, y ningún otro. **Ninguno de esos cuatro lo detectó un auditor: los detectó tener que escribir la frase.**

Esto es lo que la propuesta 4 de §8 pide, implementado en el destino. Y no alcanzó.

**La ronda siguiente encontró el mismo defecto desplazado un artefacto más adelante.** La trazabilidad de §8 ahora decía la verdad —qué pasos recorre la salida prometida— y el criterio de aceptación de §9 no tocaba ninguno de esos pasos. En nueve de catorce contratos había al menos un caso de uso declarado en `verifica:` que **ninguna aserción alcanzaba**: la sonda pasaba en verde con ese caso de uso enteramente roto. La afirmación se había vuelto verdadera y seguía sin ser verificable.

La reparación de esa ronda cierra el circuito: cada aserción declara, en un bloque `discrimina` del propio contrato, **qué caso de uso deja de cumplirse si ella falla**, y el generador se niega a emitir un sample con un caso de uso que ninguna aserción discrimine. Al exigirlo, once de los catorce samples tuvieron que ganar líneas en su salida prometida para que sus casos de uso pudieran romperse —el barrido del bus pasó a declarar que rechaza un comando fuera de la lista cerrada, la ligadura pasó a contar los canales cableados y las credenciales resueltas fuera del mapa, el planificador pasó a imprimir la agenda que antes sólo vivía en una variación—.

**La lección, que es lo que este reporte tiene para dar:** una declaración verdadera y una declaración verificable no son lo mismo, y el framework pide la primera. Hicieron falta dos reparaciones estructurales sobre dos artefactos distintos, y la segunda sólo apareció porque un auditor aplicó a mano el criterio que ninguna regla exige: *si el paso se rompe, ¿falla la aserción?*

Lo que sigue sin resolverse es el caso general —el destino no puede reparar una regla del framework— y la dirección del criterio que §4 describe: una cobertura parcial declarada honestamente sigue contando como cobertura en el recuento de casos de uso críticos con sonda.

## 8. Propuestas de intervención

Ninguna está decidida; son punto de partida.

1. **Clasificar los criterios de aceptación de las diecisiete reglas por lo que preguntan.** Existencia, forma, o relación entre dos artefactos; y en los de relación, en qué dirección. Es una pasada mecánica sobre las reglas y produce el inventario completo del problema, en lugar de los tres casos que una corrida encontró. La hipótesis a falsear es que la mayoría pregunta por existencia.
2. **Para cada criterio de existencia, decidir si la propiedad que importa es una relación.** No todos lo son: que exista un `README.md` es genuinamente una propiedad de existencia. Los que sí lo son se reescriben nombrando los dos lados.
3. **Revisar las preguntas guía de §5 de cada regla como cantera de criterios.** Están escritas, están bien formuladas y no bloquean nada. Promover las que expresan una relación verificable es la intervención más barata de las cinco.
4. **Pedir que la trazabilidad sea recíproca.** Si un sample declara que ejercita `CU-XX`, que el criterio exija que la salida prometida contenga algo del flujo principal de ese caso de uso. No resuelve el caso general, pero convierte el defecto más caro de esta categoría en algo que un lector encuentra en un minuto en lugar de en veinte.
5. **Declarar explícitamente qué criterios no son mecanizables.** Es la propuesta 1 del reporte 09 vista desde el otro lado: si un criterio pregunta por una relación semántica, decirlo, para que nadie construya un guion que crea cubrirlo y produzca la falsa confianza que es peor que la ausencia.

## 9. Cómo verificar que la corrección funcionó

- La clasificación de la propuesta 1 existe y cubre las diecisiete reglas.
- Ningún criterio de aceptación de ninguna regla se satisface con un artefacto cuyo contenido sea falso, o el criterio declara que no lo cubre y por qué.
- Ningún criterio de aceptación se satisface **mejor** con un artefacto falso que con uno honesto. Ésa es la comprobación barata: por cada criterio que cuenta algo, preguntarse si una declaración falsa sube o baja la cuenta.
- La emisión de una categoría con una tabla de trazabilidad deliberadamente falsa **falla** su audit por un criterio de §6, y no por la atención de quien lo leyó.
- Ninguna pregunta de §5 de ninguna regla expresa una relación verificable que §6 no exija.

## Control de cambios

| Versión | Fecha | Descripción |
| --- | --- | --- |
| 1.0 | 2026-08-12 | Reporte inicial, emitido al cerrar la primera ronda del audit de la Fase G. Documenta que los criterios de aceptación del framework preguntan mayoritariamente por la presencia de una declaración y no por su verdad, con tres incidentes de trazabilidad falsa que cumplían §6 de `Rules-Examples.md` sin excepción. Registra que el propio framework nombró este patrón en la fila 4.1 del control de cambios de esa regla, a propósito del glosario, y lo reparó localmente sin generalizarlo. Muestra que de los veintiséis criterios de §6 el que pregunta por la trazabilidad lo hace por su presencia, y que el único que pregunta por la relación apunta en la dirección contraria: cuenta casos de uso sin sonda, de modo que **una sonda mentirosa lo ayuda a cumplirse**. Señala que las preguntas que hubieran atajado los tres incidentes están escritas en §5.5, del lado que no bloquea. |
| 1.1 | 2026-08-12 | Se registra el desenlace de la segunda ronda de audit. La primera tanda de correcciones reincidió en el mismo defecto —al retirar dos casos de uso de un contrato se incorporó un tercero por afinidad de tema, que tampoco se ejercitaba— y ocho de los doce hallazgos interpretativos de esa ronda fueron el mismo patrón. §7 documenta la reparación estructural que el destino aplicó: la trazabilidad pasa a declarar qué pasos del flujo recorre la salida prometida y cuáles no, y el generador se niega a emitir sin esa frase. Cuatro casos de uso declarados no sobrevivieron a la exigencia, y ninguno de los cuatro lo había encontrado un auditor. Es la propuesta 4 de §8 implementada en un destino, y sirve como evidencia de que la intervención rinde. |
| 1.2 | 2026-08-12 | Nueva §2.1, a pedido de una revisión humana que preguntó cómo se llega a declarar un caso de uso que no se ejercita. El reporte documentaba el defecto y no el mecanismo. Se nombran sus tres ingredientes —confundir precondición con flujo ejercitado, elegir por afinidad de tema, y no tener código que desmienta en la pasada de diseño— y se define qué significa que la salida no recorra el flujo: que ninguna línea de la salida prometida cambie como consecuencia de un paso, de modo que el sample pasa igual con ese caso de uso roto. |
| 1.3 | 2026-08-12 | §7 registra que la reparación de la ronda anterior **no alcanzó**: la trazabilidad pasó a ser verdadera y el criterio de aceptación seguía sin tocar los pasos declarados, de modo que en nueve de catorce contratos había un caso de uso que ninguna aserción alcanzaba. La segunda reparación estructural hace que cada aserción declare qué caso de uso discrimina, y once samples tuvieron que ganar líneas en su salida para que sus casos de uso pudieran romperse. Queda enunciada la lección: una declaración verdadera y una verificable no son lo mismo, y el framework pide la primera. |
