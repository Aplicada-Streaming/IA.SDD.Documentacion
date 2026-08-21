# Mediciones M2 y M3 — con patrón, alcance, exclusiones y salida cruda

**Documento:** Mediciones-M2-M3.md
**Versión:** 1.0
**Fecha:** 2026-08-20
**Corridas sobre:** `IA.SDD` en `cc1ff49` (SDD 11.1 + T0)
**Por qué existen:** el `Plan-12-14.md` §7 exige que *«una medición vale si es recalculable»*, después de
que las cifras de su propia versión 0.5 no se reprodujeran. Las dos bloquean el tramo **T2**.

---

## M2 — Rutas `../IA.SDD/`

**Patrón:** literal `../IA.SDD/`
**Alcance:** `SDD/ PROMPTS/ Templates/ README.md CHANGELOG.md`
**Exclusiones:** `_legacy/` (§VI.3.2, snapshots congelados) · `SDD/Devs/Bootstrap/` (§I.2, no editable)
**Unidad:** **ocurrencias**, no líneas

```
for f in $(grep -rl --include=*.md "\.\./IA\.SDD/" SDD PROMPTS Templates README.md CHANGELOG.md \
           | grep -v "^SDD/Devs/Bootstrap/"); do
  echo "$(grep -o "\.\./IA\.SDD/" $f | wc -l)  $f"; done | sort -rn
```

**Salida cruda:**

```
17  SDD/Devs/Orchestrator/Master-Prompt.md
16  SDD/Guides/SDD-User-Guide.md
 6  SDD/Devs/Rules/Maqueta-Rules.md
 6  SDD/Devs/Guides/Marco-Teorico-SDD.md
 5  PROMPTS/PROMPT-Agente-Bootstrap-SDD.md
 3  PROMPTS/PROMPT-Agente-Migracion-SDD.md
 2  CHANGELOG.md
TOTAL = 55
```

**Corrección de una medición previa.** El `Plan-12-14.md` 0.5 declaraba **41**. Era un recuento de
**líneas** con el alcance sin declarar. Y el primer intento de esta misma medición excluyó `Bootstrap`
**por palabra** en lugar de por ruta, y se comió `PROMPT-Agente-Bootstrap-SDD.md`: **el error se detectó
porque la salida cruda estaba a la vista**, que es exactamente para lo que sirve publicarla.

### Clasificación

| Clase | Qué es | Dónde | Ocurrencias |
|---|---|---|---|
| **A · Insumo de despacho cableado** | Ruta dentro de un bloque cercado que **se le entrega a un subagente**. Si no resuelve, el subagente trabaja sin ese insumo y **nada lo comprueba** | `Master-Prompt.md` 777, 778, 1279, 1280, 1281 · `Maqueta-Rules.md` 526, 528 | **7** |
| **B · Operación del orquestador** | Ruta que el orquestador **lee o escribe** en prosa normativa | `Master-Prompt.md` 23, 115, 118 · `Maqueta-Rules.md` 110, 370, 405 | **6** |
| **C · Declaración de la indirección** | Las líneas que declaran que `../IA.SDD/` **es un fallback y no un requisito** | `PROMPT-Agente-Bootstrap-SDD.md` 52, 77 · `PROMPT-Agente-Migracion-SDD.md` 66, 93 · `Master-Prompt.md` 18 | **5** |
| **D · Prosa explicativa y registro** | Guía de usuario, marco teórico, changelog | `SDD-User-Guide.md` (16) · `Marco-Teorico-SDD.md` (6) · `CHANGELOG.md` (2) · resto | **37** |

**Consecuencia para T2:** lo que **rompe una corrida** son **7 ocurrencias en 2 archivos**, no 55. La
clase **C no se toca** —es la regla de resolución— y la **D** es texto que se actualiza sin riesgo.

**Y un defecto de coherencia interna que la clasificación destapa:** en el bloque de insumos de
`Master-Prompt.md` **776-778** conviven los dos regímenes — `{{PATH_REGLA}}` **sustituido** y dos rutas
transversales **cableadas al lado**. Es un caso de la comprobación 9, *«ninguna sección contradice a
otra del mismo archivo»*.

---

## M3 — Formas de identidad en las cabeceras

**Alcance:** `*.md` del árbol vivo · **Exclusiones:** `_legacy/`, `SDD/Devs/Bootstrap/`

```
LIVE=$(find SDD PROMPTS Templates -name "*.md" | grep -v "SDD/Devs/Bootstrap/"; echo README.md; echo CHANGELOG.md)
echo "$LIVE" | xargs grep -l "^doc_id:"            # frontmatter
echo "$LIVE" | xargs grep -l "^\*\*Documento:\*\*"  # bloque legible
```

**Salida cruda:**

```
archivos vivos:                     81
con frontmatter doc_id:              3   (2 reales + 1 dentro de un ejemplo)
con bloque legible **Documento:**:  59
con Versión en cabecera:            75
sin ninguna cabecera de identidad:  20
```

### Las tres formas que ya conviven

| Forma | Cuántos | Ejemplo |
|---|---|---|
| **Frontmatter `doc_id:`** | **2** | `GUIDE-SDD-DEVELOPMENT`, `GUIDE-SDD-GETTING-STARTED`. **Sin prefijo de familia y sin cinco dígitos**: no es la forma de §9.2 |
| **Bloque legible `**Documento:**`** | **59** | `**Documento:** Coherencia-Item-Diferido.md` — la identidad **es el nombre de archivo** |
| **Reglas, con `**Versión de las reglas:**` y sin `**Documento:**`** | — | `Root-Rules.md`, `Rules-Devops.md` |

*(La tercera aparición de `doc_id:` está dentro de un bloque de ejemplo de `Rules-Documentacion.md`:
prescribe `DOC-<PROYECTO>-<TIPO>-<NN>` para la documentación **del destino**, no del framework.)*

### Los 20 sin ninguna cabecera de identidad

```
PROMPTS/PROMPT-Agente-Bootstrap-SDD.md      SDD/Devs/Rules/Root-Rules.md
PROMPTS/PROMPT-Agente-Migracion-SDD.md      SDD/Devs/Rules/Intake-Rules.md
PROMPTS/PROMPT-Agente-Reanudacion-SDD.md    SDD/Devs/Rules/Deriva-Rules.md
PROMPTS/README.md                           SDD/Devs/Rules/Vocabulario-Rules.md
SDD/Guides/SDD-User-Guide.md                SDD/Devs/Rules/Maqueta-Rules.md
Templates/Modelo-Generico/README.md         SDD/Devs/Rules/Migracion-Rules.md
SDD/Devs/Intake/PRODUCT-INTAKE-template.md  SDD/Devs/Rules/Rules-Necesidades-Negocio.md
SDD/Devs/Intake/PRODUCT-MANIFEST-template.md
SDD/Devs/Orchestrator/Master-Prompt.md       SDD/Devs/Orchestrator/Master-Prompt-Migracion.md
SDD/Devs/Orchestrator/Master-Prompt-Reanudacion.md
README.md   CHANGELOG.md
```

**Están los más citados del corpus**: `Root-Rules.md`, `Master-Prompt.md` y los tres prompts de entrada.

---

## Qué deciden las dos mediciones

| Decisión | Con el dato |
|---|---|
| **Bump de la sustitución de rutas** | **Minor.** Son 7 ocurrencias en 2 archivos, dentro de esqueletos de despacho. No cambia ninguna estructura obligatoria de ningún documento generado |
| **Alcance de T2 · identidad** | **Mayor de lo que el plan suponía.** No es «agregar un identificador a una cabecera uniforme»: hay **tres formas conviviendo** y **20 artefactos sin ninguna**. Unificar la cabecera es **trabajo previo** a asignar identificadores |
| **Orden dentro de T2** | 1) unificar la forma de cabecera · 2) asignar identificadores `AG-XXXXX` y familia del framework · 3) catálogo · 4) sustituir las 7 rutas de clase A |
