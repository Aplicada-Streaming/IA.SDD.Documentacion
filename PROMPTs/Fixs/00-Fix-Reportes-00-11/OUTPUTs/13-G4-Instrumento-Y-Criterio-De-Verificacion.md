# G4 — El instrumento y el criterio de verificación

**Documento:** 13-G4-Instrumento-Y-Criterio-De-Verificacion.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Reportes de la familia:** 09, 10, y el hueco C del 00
**Estado:** Vigente

---

## 1. Dónde se produce el fallo

Esta familia no habla de lo que el framework produce sino de cómo lo verifica. Sus tres piezas son
distintas y encajan:

| Pieza | Reporte | Qué falla |
|---|---|---|
| El **instrumento** | 09 | Un solo instrumento —un lector independiente— para propiedades de dos naturalezas. El caro se gasta en lo barato |
| El **criterio** | 10 | Los criterios preguntan si una declaración está, no si es verdadera. Y el único que pregunta por la relación cuenta lo que falta, de modo que una declaración falsa lo ayuda a cumplirse |
| La **taxonomía** | 00, hueco C | Los cuatro niveles de hallazgo se definen por impacto sobre la fase. Un defecto originado aguas arriba que la fase reprodujo fielmente no puede ser P0 ni P1, y termina en P3 por descarte |

El reporte 09 §6 y el reporte 10 §6 se complementan: uno habla de quién mira y el otro de qué se le
pide que confirme. Un criterio que se cumple mejor con una declaración falsa que con una honesta no
lo arregla ningún instrumento.

## 2. Por qué la normativa vigente no lo atrapa

### 2.1 Lo que ya funciona y no hay que reescribir

**El audit como compuerta es correcto y el destino lo comprueba.** Los once hallazgos interpretativos
de la Fase E son defectos graves que ningún otro mecanismo habría encontrado, y dos de ellos habrían
llegado a las pruebas de la categoría 08 como criterios que verifican lo contrario de lo que el
diseño exige. La independencia del auditor también funciona: los tres despachos encontraron conjuntos
distintos. `Master-Prompt.md` §10 declara esa independencia con precisión —«un subagente auditor que
se invoca desde cero, sin contexto previo»— y §10 argumenta por qué el informe es por ronda.

**Las preguntas guía de §5 están bien formuladas.** En `Rules-Examples.md` §5.5, «¿Cada sample declara
qué verifica, y no sólo qué ilustra?» es exactamente la pregunta que hubiera atajado los tres
incidentes. El problema es dónde vive, no cómo está escrita.

**La clasificación P3 del hueco C fue correcta** bajo la normativa vigente, y el auditor declaró
explícitamente el límite de su alcance en lugar de excederlo. El defecto está en que la normativa no
le ofrecía otra salida.

### 2.2 Los tres huecos, con su cita

**Nada corre antes del audit.** `Master-Prompt.md` §6 cierra cada fila del plan de generación con una
columna `Audit post-fase`, y §10 desarrolla la mecánica. No hay compuerta mecánica, no hay lista de
propiedades enumerables, y la comprobación es reproducible:

```bash
grep -n "compuerta\|verificaci[óo]n mec[áa]nica\|antes de despachar" Orchestrator/Master-Prompt.md
# ninguna de las coincidencias es una precondición del audit
```

**Los criterios de §6 no distinguen su naturaleza.** Están redactados para que los evalúe un lector
—«existe X con las cinco secciones», «cada TC referencia al menos un CU»— sin declarar cuáles son
decidibles por una máquina. Varios lo son: existencia de archivos, presencia de tablas, ausencia de
sufijos en los nombres, tabla de contenido.

**El criterio de trazabilidad pregunta por la presencia, y el que pregunta por la relación apunta al
revés.** Los dos están en `Rules-Examples.md` §6 y son verificables:

```bash
grep -n "al menos una fila\|que lo ejercita" Rules/Rules-Examples.md
# 382: «Cada sample declara trazabilidad a CU, ADR o NFR en §8 con al menos una fila»
# 398: «Todo CU declarado crítico en 02 tiene al menos una sonda VER-XX que lo ejercita,
#       o la ausencia está justificada»
```

El verbo del segundo es el correcto —ejercitar, no nombrar— y la dirección es la contraria a la que
hace falta: pregunta si todo caso de uso crítico está cubierto, no si toda cobertura declarada es
cierta. De ahí sale lo peor del caso:

> Una trazabilidad falsa no evade el único criterio que pregunta por la relación: **lo ayuda a
> cumplirse**. Lo que cuenta es cuántos casos de uso quedan sin sonda, y una sonda mentirosa reduce
> esa cuenta igual que una verdadera.

**Los niveles de hallazgo no tienen eje de origen.** `Master-Prompt.md` §10 los define por impacto
sobre la fase: P0 rompe trazabilidad o viola invariantes, P1 incumple criterios de aceptación, P2 son
ítems recomendados ausentes, P3 son mejoras de estilo. Un defecto upstream que la fase reproduce
fielmente no puede ser P0 ni P1 —la fase no incumplió nada: hizo bien su trabajo copiando algo
incorrecto— y termina en P3 por descarte. El nivel no mide su gravedad: mide que el auditor no tenía
dónde ponerlo.

## 3. La medición que ordena la corrección

Es lo que hace a esta familia distinta de una opinión sobre el proceso.

| Ronda | Hallazgos | Detectables por guion | Solo por lectura |
|---|---|---|---|
| 1 | 6 P0 + 9 P1 | 9 | 6 |
| 2 | 3 P0 + 6 P1 | 6 | 3 |
| 3 | 2 P0 + 7 P1 | 7 | 2 |
| **Total** | **33** | **22** | **11** |

Tres lecturas de esa tabla, y las tres importan:

1. **El rendimiento baja y no llega a cero.** 15, 9, 9. Cada corrección introduce material nuevo que
   la ronda siguiente tiene que volver a muestrear, y en las tres rondas hubo hallazgos que eran la
   corrección anterior aplicada a media oración.
2. **Dos tercios de los hallazgos los encuentra un guion.** Y el costo no es que queden sin
   verificar: es que **las interpretativas quedan peor verificadas de lo que el método supone**. Los
   dos P0 de la ronda 3 fueron accidentes de escapado, encontrados por el mismo auditor que tenía que
   verificar 309 citas contra su fuente.
3. **No hay criterio de corte.** La Fase E cerró con tres rondas por decisión del Product Owner, no
   por un criterio del método.

Y la evidencia del reporte 10 sobre cuánto cuesta arreglar el criterio y no el artefacto: cuando el
destino exigió que la trazabilidad declarara qué pasos del flujo recorre la salida prometida, cuatro
casos de uso declarados no sobrevivieron a la pregunta. **Ninguno de los cuatro lo había detectado un
auditor: los detectó tener que escribir la frase.** Y la ronda siguiente encontró el mismo defecto
desplazado un artefacto más adelante: la trazabilidad ya era verdadera y ninguna aserción tocaba los
pasos declarados, de modo que en nueve de catorce contratos la sonda pasaba en verde con el caso de
uso enteramente roto. La lección queda enunciada en el reporte 10 §7 y es la que gobierna G4-E:

> Una declaración verdadera y una declaración verificable no son lo mismo, y el framework pide la
> primera.

## 4. Correcciones propuestas

### G4-A · Compuerta mecánica antes del audit, con su alcance declarado

**Archivos:** `Master-Prompt.md` §6 y §10.
**Severidad:** minor.

Antes de despachar el audit de una fase corre una comprobación mecánica cuyo resultado es insumo del
despacho. El prompt del auditor recibe qué quedó verificado y lo excluye explícitamente, para gastar
su presupuesto en lo interpretativo.

Tres comprobaciones mínimas, cada una salida de un defecto real de la corrida:

1. **Enlaces y anclas** sobre el árbol de la fase.
2. **Recuentos anclados** (la familia G2): contrastar contra su fuente los números que la prosa
   declara sobre colecciones contables.
3. **Idempotencia de los generadores**: correr dos veces produce los mismos bytes.

Y una obligación sobre su salida, que es la parte que no se puede omitir: **la compuerta declara qué
no mira**. Una compuerta que se lee como aprobación es peor que ninguna, porque el audit siguiente
llega con la guardia baja. La salida en verde del destino decía literalmente «lo mecánico está
limpio; lo que afirma cada frase, no lo mira nadie todavía», y eso es lo que se vuelve norma.

### G4-B · Criterios de aceptación clasificados por naturaleza

**Archivos:** §6 de las reglas de categoría.
**Severidad:** minor.

Cada criterio de §6 se marca como **enumerable** —decidible contando o comparando— o
**interpretativo** —decidible solo leyendo los dos lados—. La suma de los enumerables es lo que la
compuerta de G4-A tiene que cubrir.

Con la restricción que el reporte 10 §8.5 hace explícita y que es la razón de la aplicación parcial
de §6: **declarar mecanizable un criterio que no lo es produce falsa confianza, que es peor que la
ausencia**. Marcar mal un criterio interpretativo lo saca del alcance del lector sin ponerlo bajo el
de ninguna máquina.

### G4-C · Criterio de corte de las rondas de audit

**Archivo:** `Master-Prompt.md` §10.
**Severidad:** minor.

Hoy una fase cierra cuando el audit aprueba, y si nunca aprueba no hay regla. El criterio que se
adopta, tomado del reporte 09 §8.3: **una fase cierra cuando dos rondas seguidas no encuentran
ningún hallazgo de la clase interpretativa, con los enumerables en cero por la compuerta**. Si el
criterio no se alcanza en un número declarado de rondas, la decisión sube al humano de forma
explícita, y el informe declara que cerró por decisión y no por criterio.

### G4-D · El audit clasifica sus propios hallazgos

**Archivo:** `Master-Prompt.md` §10, estructura del informe.
**Severidad:** minor.

Cada hallazgo declara si un guion podría haberlo encontrado. Es barato, y produce la métrica que el
reporte 09 tuvo que reconstruir a mano: si la proporción no baja ronda a ronda, la compuerta no está
cubriendo lo que debería.

### G4-E · Los criterios de relación se escriben nombrando los dos lados

**Archivos:** `Rules-Examples.md` §6; `SDD-Development-Guide.md` como regla de redacción de criterios.
**Severidad:** minor en la regla; minor en la guía.

1. **Trazabilidad recíproca.** Si un sample declara que ejercita `CU-XX`, el criterio exige que la
   declaración diga **qué pasos del flujo principal recorre la salida prometida y cuáles no**. No
   verifica la verdad —eso sigue exigiendo leer los dos documentos— pero convierte una afirmación
   vaga en falsable: «ilustra CU-25» no se puede refutar sin discutir qué significa ilustrar;
   «recorre los pasos 3 a 7 y no el 2» se refuta leyendo siete renglones.
2. **Discriminación.** Cada aserción declara qué caso de uso deja de cumplirse si ella falla, y no se
   admite un caso de uso declarado que ninguna aserción discrimine. Es la segunda reparación que el
   destino necesitó, y sin ella la declaración es verdadera y sigue sin ser verificable.
3. **Regla de redacción de criterios, para el resto del framework.** Al escribir un criterio de
   aceptación: si la propiedad que importa es una relación entre dos artefactos, el criterio nombra
   los dos lados; y por cada criterio que cuenta algo, hay que preguntarse si una declaración falsa
   sube o baja la cuenta. Es la comprobación barata del reporte 10 §9 y la que detecta los criterios
   con el signo cambiado.
4. **Promover preguntas de §5 a §6.** Las preguntas guía que expresan una relación verificable son
   cantera de criterios: están escritas, están bien formuladas y no bloquean nada. El reporte 10 §6
   declara que en `Rules-Examples.md` hay al menos cuatro candidatas, y que moverlas es más barato
   que inventarlas.

### G4-F · Hallazgo aguas arriba

**Archivo:** `Master-Prompt.md` §10.
**Severidad:** minor.

Una **clase de hallazgo ortogonal al nivel**: el hallazgo *aguas arriba*, que se marca además de su
nivel y declara el artefacto de origen. Con dos consecuencias operativas:

1. El informe lo lista aparte, con el artefacto upstream que hay que corregir.
2. El orquestador presenta los hallazgos aguas arriba al humano **antes de despachar la fase
   siguiente**, porque son los únicos que la corrección de la fase no puede resolver: corregir el
   entregable sin corregir el intake reintroduce el defecto en la próxima regeneración.

En el caso real el defecto viajaba hacia un criterio de aceptación de la categoría 08, y lo detuvo
una lectura humana del informe, no la maquinaria.

## 5. Cómo se verifica

| Caso | Entrada | Resultado esperado | Origen |
|---|---|---|---|
| V1 | Los criterios de §6 de las reglas alcanzadas | Están clasificados, y la suma de los enumerables coincide con lo que la compuerta comprueba | 09.1 |
| V2 | Un audit despachado | Recibe el resultado de la compuerta y su prompt excluye explícitamente lo ya verificado | 09.2 |
| V3 | La primera ronda de una fase | La proporción de hallazgos detectables por guion, reportada por el propio audit, baja de un umbral declarado | 09.3 |
| V4 | Una fase cerrada | Cita el criterio de corte, en vez de cerrar por decisión; o declara que cerró por decisión | 09.4 |
| V5 | Una salida en verde de la compuerta | Declara qué no miró | 09, §7 del reporte |
| V6 | Una categoría emitida con una tabla de trazabilidad deliberadamente falsa | **Falla** su audit por un criterio de §6, y no por la atención de quien lo leyó | 10 |
| V7 | Cada criterio de §6 que cuenta algo | Una declaración falsa **no** acerca el criterio a cumplirse | 10, la comprobación barata |
| V8 | Las preguntas de §5 de cada regla alcanzada | Ninguna expresa una relación verificable que §6 no exija | 10 |
| V9 | Un audit que detecta un defecto reproducido fielmente del intake | Lo lista como hallazgo aguas arriba, con su artefacto de origen, y el orquestador lo presenta antes de despachar la fase siguiente | 00 V5 |

## 6. Qué queda fuera de esta corrección, y por qué

**La clasificación completa de los criterios de aceptación de las diecisiete reglas.** Es la
propuesta 1 del reporte 09 y la 1 del reporte 10, y las dos la declaran como «una pasada mecánica
sobre texto que ya existe». No lo es del todo: clasificar un criterio exige decidir si la propiedad
que verifica es enumerable, y equivocarse tiene consecuencia asimétrica —el reporte 10 §8.5 dice que
declarar mecanizable lo que no lo es produce falsa confianza, «que es peor que la ausencia»—. Sobre
las reglas donde los reportes aportan evidencia criterio por criterio, la clasificación se aplica.
Sobre el resto se aplica el **mecanismo** —la convención de marcado y la obligación de clasificar— y
la pasada queda declarada como trabajo siguiente, con su alcance escrito. Es una aplicación parcial y
así se declara; lo que no se hace es marcar 340 criterios sin haber leído qué verifica cada uno.

**Construir la compuerta mecánica.** G4-A la exige y declara sus tres comprobaciones mínimas; el
guion que las implementa es del repositorio destino, no del framework. El destino ya construyó el
suyo en `SDD/Herramientas/Verificacion/`, y el framework no distribuye código: distribuye la
obligación y el contrato de su salida.

**La alternativa económica del hueco C.** El reporte 00 §5 ofrece, en lugar de la clase ortogonal de
G4-F, declarar que un hallazgo cuyo origen es el intake «eleva su nivel al que tendría el defecto en
su artefacto de origen». Se descarta: mueve el número sin decir de dónde viene el defecto, y lo que
la corrida necesitó fue precisamente saber qué artefacto upstream había que corregir. Queda
registrada.
