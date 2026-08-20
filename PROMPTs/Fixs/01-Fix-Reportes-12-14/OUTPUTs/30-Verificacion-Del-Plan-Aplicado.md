# 30 — Verificación del plan aplicado contra el plan propuesto

**Pasos 7, 8 y 9 del prompt de intervención.** Se corre **después** de la publicación, sobre los
archivos vivos del árbol, y no sobre el `CHANGELOG` ni sobre la nota de coherencia de quien aplicó.

**Vigente al correr esta verificación:** SDD **10.1**, árbol limpio, `main` al día
(`fb7a3ca`). La intervención de este prompt se publicó como **10.0** (`a447b45`), y **10.1** es
posterior y de otro origen (`Reportes/15`).

---

## 1. Lo primero, porque cambia la lectura de todo lo demás: se aplicó **B**, no **A**

`20-Plan-De-Aplicacion.md` §3 recomendaba la **opción A** —sólo detección, conjunto **minor**, cero
migración— y dejaba **B** para cuando hubiera un segundo caso medido. **Lo publicado es `B`**: el
conjunto subió **major** a 10.0, `Root-Rules.md` §12.2 declara la forma obligatoria de cuatro campos
y `Rules-Devops.md` §4.3 partió su punto 3.

**No es un defecto de la aplicación: es la bifurcación resuelta por quien tenía que resolverla.** El
plan la declaraba explícitamente como decisión del Product Owner y la entrada 10.0 del `CHANGELOG`
emite el bloque «Impacto sobre destinos existentes» que `B` obligaba y `A` no. **Lo que falta es el
registro de la decisión**, no la decisión: el plan sigue diciendo «recomendación: A» y ningún archivo
declara que se eligió otra cosa. Se anota acá y con eso queda cerrado.

**Consecuencia para el paso 9:** lo aplicado **es** una migración, y el conjunto la declaró como tal.

---

## 2. Las cinco propuestas del reporte 14, contra los archivos vivos

| # | Propuesta | Estado | Evidencia sobre el archivo vivo |
|---|---|---|---|
| 6.1 | El ítem diferido es figura declarada, no prosa | **Aplicada** | `Root-Rules.md` §12.2 (línea 622), cuatro campos numerados. La regla está en **7.0** |
| 6.2 | El evento de cierre nombra un artefacto, no un momento | **Aplicada** | §12.2 punto 4: *«En qué evento se cierra, **nombrando un artefacto y su sección** — no un momento»*, con el contraste literal «punto de control de la etapa `a`» contra «`Plan-Etapa-A.md` §7» |
| 6.3 | La compuerta mecánica los cuenta | **Aplicada** | `Master-Prompt.md` §10.0 comprobación **6** (línea 1128): *«se cuentan los que el árbol de la fase declara, y **es hallazgo el que nombre un evento de cierre que ya ocurrió**»*. La compuerta pasó de cinco comprobaciones a seis |
| 6.4 | Los ítems que empaquetan dos decisiones se separan | **Aplicada en el caso medido, no en la clase** | `Rules-Devops.md` §4.3 punto **3.b** (línea 174) existe. La auditoría sobre las quince reglas que el propio reporte pedía **no se corrió**: ver `40-Auditoria-Empaquetado-Y-Diferimiento-Ilegitimo.md` |
| 6.5 | El orquestador de reanudación lo mira | **Aplicada** | `Master-Prompt-Reanudacion.md` R0 paso 4 (línea 141) y bloque `ÍTEMS DIFERIDOS` en R1 con sus tres renglones —declarados, vencidos, sin forma— |

**Registro:** `Catalogo-De-Criterios.md` incorpora cuatro criterios de §12.2 y recalifica la fila de
§12 a §12.1, que es lo que la comprobación 12 de `SDD-Development-Guide.md` §VI.3 exige.

---

## 3. Los cinco criterios de «cómo verificar que la corrección funcionó» (reporte 14 §7)

| # | Criterio | Veredicto | Sobre qué se apoya |
|---|---|---|---|
| 1 | Existe forma marcada y **se puede contar** | **Cumple** | §12.2 fija cuatro campos numerados; la forma es buscable porque el campo 4 obliga a nombrar artefacto y sección |
| 2 | Los cuatro campos, y el evento **nombra artefacto y sección** | **Cumple** | §12.2 punto 4, literal |
| 3 | **Alguna compuerta levanta un ítem cuyo evento ya ocurrió** | **Cumple** | Dos, no una: `Master-Prompt.md` §10.0 comprobación 6 y `Master-Prompt-Reanudacion.md` R0 paso 4. **Es el decisivo y es el que cierra el lazo** |
| 4 | Ningún ítem obligatorio de una §4.x empaqueta dos decisiones. **Se audita una vez sobre las quince reglas** | **NO cumple** | Se corrigió el caso medido (`Rules-Devops.md` §4.3) y **no se corrió la auditoría**. Ni la entrada 10.0 del `CHANGELOG` ni `Coherencia-Item-Diferido.md` §2 la mencionan, y su inventario tiene doce archivos, ninguno por este concepto |
| 5 | *(Interpretativo)* Sobre el destino que originó el reporte, la corrección **habría detectado** el prefijo al cerrar la etapa `a` | **Cumple por construcción, no medido acá** | El prefijo pasa a ser ítem propio (§4.3 punto 3.b) y, diferido en forma, su evento sería un artefacto abierto; la comprobación 6 lo habría levantado en la primera compuerta posterior. **No se corrió sobre el destino real**, que está fuera de este repositorio |

**Cuatro de cinco.** El que falta es el 4, y es el que este prompt pedía por partida doble: el reporte
lo lista como criterio y la solicitud 6 del prompt encarga además la lista de ítems donde diferir sea
ilegítimo, *«mirando las quince reglas»*. Los dos se resuelven en el documento `40`.

---

## 4. Dos hallazgos que la verificación levanta, y los dos son trabajo propio

**Ninguno de los dos es una decisión: los dos se contestan con una cita literal del árbol**, que es
la prueba que `Master-Prompt.md` §8.1 pide para no detener.

### 4.1 · **P2** — El barrido de 10.0 declaró un residuo que no es el que hay

`Coherencia-Item-Diferido.md` §3 declara para el patrón `` `Root-Rules.md` §12 `` sin subsección:
**14 ocurrencias en 9 archivos, 13 reemplazadas, residuo 1**, excluida con motivo.

Contado sobre el árbol vivo, quedan **nueve** ocurrencias sin subsección fuera de `_legacy/`:

| Dónde | Cuántas | Clase |
|---|---|---|
| Filas de control de cambios de siete reglas de categoría | 7 | **Legítimamente excluible**: es la clase «Filas de control de cambios» que `SDD-Development-Guide.md` §VI.3.2 ya enumera, y que la 10.1 obliga a **citar** en lugar de reescribir |
| `Master-Prompt.md` línea **527** | 1 | **No excluible.** Es prosa normativa: *«El artefacto la declara con la forma de **referencia pendiente** de `Root-Rules.md` §12»* |
| `Master-Prompt.md` línea **1600** | 1 | **No excluible.** Es el glosario operativo §15, entrada «Referencia pendiente»: *«Definida en `Root-Rules.md` §12»* |

**Las dos últimas son exactamente lo que la propia entrada 10.0 manda migrar:** *«Toda cita a
`Root-Rules.md` §12 que hable de referencias pasa a §12.1»*. **El framework no se lo aplicó a sí
mismo**, y las dos viven en el archivo que la misma intervención estaba editando.

**Es la misma clase de defecto que `Reportes/15` mide y que la 10.1 corrige** —una corrida que afirma
un recuento y el recuento es literalmente falso—, con una diferencia que conviene declarar: **la 10.1
no la habría evitado**. Su corrección alcanza a las exclusiones que no se enumeran; acá la exclusión
estaba bien vista y **lo que falló fue el recuento**, y dos ocurrencias que ninguna exclusión cubre
quedaron adentro de él.

**Corrección:** las dos citas de `Master-Prompt.md` pasan a **§12.1**. No cambia ningún
procedimiento; ningún documento generado deja de cumplir.

### 4.2 · **P1** — `Rules-Prompts-AI.md` §4.2 punto 9 contradice a `Root-Rules.md` §12.2

Texto vivo (línea 165), literal:

> *«9. Costos esperados. […] hasta entonces el costo se expresa en la moneda que el intake declare, o
> **sin moneda con la magnitud declarada como pendiente**. No es una referencia que se pueda dejar
> colgada —es un dato que falta— y **por eso no se resuelve con la forma de `Root-Rules.md` §12.1**
> sino declarando de dónde sale el dato.»*

**El diagnóstico de la regla es correcto y su conclusión dejó de serlo el 2026-08-19.** «Es un dato
que falta, no una referencia» es la distinción exacta que §12.2 vino a cubrir: *«un dato de contenido
que no se puede escribir hoy»*. La regla descarta §12.**1** —bien— y no conoce §12.**2**, que se
publicó después de que esa frase se escribiera; el barrido de 10.0 le corrigió el número de sección y
**no miró que la frase quedaba mandando lo contrario de la figura nueva**.

**El efecto no es teórico y es enumerable.** «La magnitud declarada como pendiente» es una promesa en
prosa; la tabla de escalamiento de §12.2 la califica **P1**, y la comprobación 6 de §10.0 la va a
levantar como hallazgo **en todo destino con `usa_llm` en true**. Hoy una regla de categoría autoriza
lo que la compuerta transversal sanciona.

**Corrección, y no agrega obligación ninguna:** el punto 9 remite a **§12.2**, cuyo evento de cierre
se nombra solo —el artefacto de la categoría 09 que declara la moneda—. §12.2 es transversal y rige
sobre ese ítem **desde la 10.0**: lo que se quita es una excepción que ya no tiene efecto, no un
permiso vigente. Ningún destino que hoy cumpla deja de cumplir.

---

## 5. Impacto sobre destinos existentes (paso 9)

| Qué | Migración o regla | Estado |
|---|---|---|
| Lo publicado en **10.0** | **Migración**, y así se declaró: el bloque «Impacto sobre destinos existentes» consolida los renombres, las secciones partidas y los campos bloqueantes nuevos | **Emitido y verificado** en el `CHANGELOG` |
| Hallazgo **4.1** | **Ninguna de las dos.** Corrige dos citas del framework a sí mismo | Pendiente de aplicar |
| Hallazgo **4.2** | **Regla, y sin migración**: alinea un ítem con una obligación transversal ya vigente | Pendiente de aplicar |
| Auditoría del criterio 4 (documento `40`) | **Sería migración**, y por eso no se aplica sola | Elevada |

---

## 6. Veredicto

**El plan aplicado se corresponde con el propuesto en el mecanismo y difiere en la opción**, que se
resolvió por la vía prevista. De los cinco criterios del reporte 14, **cumplen cuatro**, incluido el
decisivo. **El criterio 4 queda abierto** y se resuelve en el documento `40`. La verificación levanta
**dos hallazgos, ambos con cita**, y ninguno de los dos es una decisión de producto.

---

## 7. Cierre, después de aplicar

**Los dos hallazgos y la auditoría se aplicaron el 2026-08-20 como SDD 11.0.**

| Qué | Dónde quedó |
|---|---|
| Hallazgo **4.1**, las dos citas de `Master-Prompt.md` | Corregidas a §12.1. La regla sube a **8.9** (patch) |
| Hallazgo **4.2**, `Rules-Prompts-AI.md` §4.2 punto 9 | Remite a §12.2. La regla sube a **4.5** (minor) |
| Criterio 4, los cinco ítems empaquetados | Partidos. `Rules-Devops.md` **6.0** y `Rules-Backlog-Tecnico.md` **5.0**, las dos major |

**Los cinco criterios de `Reportes/14` §7 cumplen.** El único que quedaba abierto era el 4, y la
auditoría que lo cerraba está en `40-Auditoria-Empaquetado-Y-Diferimiento-Ilegitimo.md`.
