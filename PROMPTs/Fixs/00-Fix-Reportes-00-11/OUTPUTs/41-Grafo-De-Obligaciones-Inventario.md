# Inventario del grafo de obligaciones contra el orden de fases

**Documento:** 41-Grafo-De-Obligaciones-Inventario.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Origen:** pendiente declarado de la intervención 7.0, propuesta 1 del reporte `07`
**Estado:** Vigente

---

## 1. Qué se corrió

La comprobación que el reporte `07` §8.1 pide: recorrer las reglas de categoría buscando toda frase
que cree una obligación hacia otra categoría, y contrastar la fase de la categoría que la contiene
con la fase de la categoría destino. El reporte declara por qué importa correrla:

> los cuatro incidentes de este reporte son los que aparecieron hasta la Fase E de una corrida, y **no
> hay ninguna razón para suponer que son todos**.

**Resultado bruto:** 48 coincidencias en 22 pares distintos de categoría origen y destino. El bruto
no es el hallazgo: hace falta triaje, porque el patrón captura tres clases distintas y solo una es un
defecto.

## 2. Las tres clases

| Clase | Qué es | ¿Es defecto? |
|---|---|---|
| **Obligación hacia adelante** | El artefacto tiene que referenciar, o satisfacer una condición sobre, algo que una categoría posterior emite | **Sí**. Es el patrón del reporte `07` |
| **Declaración de downstream** | La regla declara a qué alimenta su categoría: «los quality gates de 09 ejecutan los tests categorizados en 08» | **No.** D6 la exige explícitamente |
| **Declaración de frontera** | La regla declara qué **no** le corresponde: «sin tipos físicos, eso vive en 05» | **No.** Es lo que evita el solapamiento entre categorías |

## 3. Las obligaciones reales

### 3.1 Ya documentadas por el reporte `07` y tratadas en la 7.0

| Origen | Fase | Destino | Fase | Distancia | Qué obliga | Tratamiento |
|---|---|---|---|---|---|---|
| 00 | A | 08 | E | 5 fases | `Acuerdo-Equipo.md` §5 referencia la condición de terminado | Referencia pendiente (`Rules-Contexto.md` §4.2) |
| 06 | D | 08 | E | 1 fase | La condición de listo referencia la de terminado sin solaparse | Referencia pendiente |
| 07 | D | 08 | E | 1 fase | El plan referencia la condición de terminado canónica | Referencia pendiente |
| 07 | D | 11 | H | 4 fases | El corte no cierra con documentos de la 11 sin revisar | Referencia pendiente |
| 08 | E | 10 | G | 2 fases | La matriz exige una fila `VER-XXXXX` por contrato de la 10 | Reapertura con insumo (`Master-Prompt.md` §6) |

### 3.2 Ya documentadas por el reporte `11`, sin tratar

| Origen | Fase | Destino | Fase | Qué obliga |
|---|---|---|---|---|
| 05, 06, 08, 09, 10 | C a G | 11 | H | El criterio de gobierno de glosario manda declarar vocabulario técnico en `Glosario-Tecnico.md`, que la 11 emite en la Fase H |

El reporte `11` §4.3 lo enuncia: «seis categorías lo referencian desde las Fases C a G: es el patrón
del reporte `07`, acá con seis instancias más». La 7.0 resolvió a dónde va el vocabulario **del
método**; el vocabulario técnico **del producto** sigue apuntando a una categoría posterior.

### 3.3 Hallazgos nuevos: ninguna corrida los había encontrado

| # | Origen | Fase | Destino | Fase | Distancia | Cita textual |
|---|---|---|---|---|---|---|
| N-1 | 00 | A | 06 | D | 3 fases | `Rules-Contexto.md` §4.2: «§6 Definition of Ready (**referencia a 06**)» |
| N-2 | 04 | B | 08 | E | 3 fases | `Rules-Prompts-AI.md`, tabla de trazabilidad del contrato de prompt: «Tests de comportamiento previstos \| **<referencia a 08>**» |
| N-3 | 04 | B | 09 | F | 4 fases | `Rules-Prompts-AI.md` §4.2 punto 9: «costo por request en la moneda **declarada en 09**» |

**N-1 es de la misma familia exacta que el incidente A del reporte `07`, y estaba al lado.** El
mismo documento —`Acuerdo-Equipo.md`, Fase A— referencia la condición de terminado de la 08 en su §5
y la condición de listo de la 06 en su §6. El reporte encontró la primera y no la segunda, porque la
corrida se chocó con una y no con la otra.

**N-3 tiene una particularidad**: no es una referencia, es una **dependencia de dato**. Un contrato
de prompt de la Fase B tiene que expresar un costo en una moneda que la categoría 09 declara cuatro
fases después. No se resuelve con una referencia pendiente sino declarando la moneda antes, o
declarando el costo sin moneda hasta que exista.

## 4. Qué prueba este inventario

Que la lista **no estaba cerrada**, tal como el reporte `07` advertía. Una corrida encuentra las
obligaciones con las que se choca; la comprobación mecánica encuentra las que existen. La diferencia,
medida: cinco obligaciones conocidas contra ocho reales, más la familia del glosario técnico.

Y que la comprobación **no puede correrse sin triaje**: de 48 coincidencias brutas, la mayoría son
declaraciones de downstream que D6 exige. Un instrumento que reportara las 48 como defectos produciría
el ruido que el reporte `04` documenta, y se desactivaría solo.

## 5. Tratamiento aplicado

| Hallazgo | Tratamiento | Dónde |
|---|---|---|
| N-1 | Referencia pendiente de `Root-Rules.md` §12 | `Rules-Contexto.md` §4.2 y §6 |
| N-2 | Referencia pendiente | `Rules-Prompts-AI.md` §4.2 |
| N-3 | Se declara como dependencia de dato: el costo se expresa sin moneda hasta que la 09 la declare, o la moneda se toma del intake | `Rules-Prompts-AI.md` §4.2 |
| Familia del glosario técnico | Referencia pendiente en el criterio de gobierno de glosario de las cinco reglas que apuntan a la 11 | §6 de `Rules-Arquitectura-Tecnica`, `Rules-Backlog-Tecnico`, `Rules-Calidad-Y-Pruebas`, `Rules-Devops`, `Rules-Examples` |

## 6. Cómo reproducir la comprobación

El guion recorre las doce reglas de categoría, mapea cada una a su fase según el plan de generación de
`Master-Prompt.md` §6, y busca referencias a categorías cuya fase es posterior. Su salida es una lista
de candidatas que **exige triaje humano** en las tres clases de §2. Correrla después de cada
intervención que toque reglas de categoría es barato y cierra el patrón en lugar de administrarlo caso
por caso.
