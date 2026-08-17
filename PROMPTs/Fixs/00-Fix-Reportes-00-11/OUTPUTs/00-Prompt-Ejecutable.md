# Prompt de intervención sobre el Framework SDD a partir de los reportes 00 a 11

**Documento:** 00-Prompt-Ejecutable.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Origen:** reedición de `Fix-Analizar-Reportes-00-11.md` pedida por su §Reglas, punto 3
**Estado:** ejecutado en esta corrida

Este archivo es el prompt que la §Reglas del tool-prompt manda redactar: las mismas solicitudes,
ordenadas como procedimiento ejecutable, con rol, insumos, restricciones, formato de salida y
criterios de aceptación explícitos. No agrega alcance: reordena el que ya está dado.

---

## 1. Rol

Actuás como responsable de intervención normativa sobre el `Framework SDD`. No sos el autor de los
reportes ni el ejecutor de una corrida: tu trabajo es decidir, con la evidencia de los reportes y
con el texto vigente del framework, qué se corrige, con qué severidad y en qué orden, y después
aplicarlo.

## 2. Insumos, en orden de lectura obligatorio

1. `/IA/SDD/IA.SDD/README.md` — modelo de tres repositorios, invariantes D1 a D9 y reglas de
   intervención sobre el framework. Fija qué severidad exige cada clase de cambio.
2. `/IA/SDD/IA.SDD/SDD/Guides/SDD-User-Guide.md` — qué ve el usuario. Toda corrección que cambie
   lo que el usuario hace o ve obliga a tocar esta guía en la misma intervención.
3. Los doce reportes de `/IA/SDD/IA.SDD.Documentacion/Reportes/`, más su `README.md`, que ya
   propone un agrupamiento y hay que evaluar, no adoptar.
4. El texto vigente de los artefactos que cada reporte declara alcanzados.

## 3. Restricciones duras

- **No inventar información.** Ningún enunciado del análisis ni del plan puede introducir un hecho
  que no esté en los insumos.
- **Toda afirmación debe estar respaldada por evidencia verificable.** Cada afirmación sobre lo que
  el framework dice o no dice se verifica contra el archivo vigente antes de escribirla, citando
  archivo y sección. Los reportes son evidencia de los incidentes, no autoridad sobre el estado
  actual del framework: entre el reporte y el archivo, manda el archivo.
- Un reporte propone; no decide. Las tablas de «propuestas de intervención» son punto de partida.
- Ninguna corrección puede contradecir a otra. La coherencia entre correcciones es criterio de
  aceptación del plan, no una aspiración.
- Lo que cada reporte declara **que el framework ya resuelve bien** no se reescribe. Todos los
  reportes tienen esa sección a propósito, para acotar la sobrecorrección.
- Una intervención que toque una invariante D1 a D9 **no se aplica sin decisión explícita del
  responsable**, por `README.md` §Reglas de intervención. Se propone, se deja lista y se pregunta.
- La carpeta `PROMPTs/` es del usuario: la única escritura admitida es dentro de `OUTPUTs/`.

## 4. Procedimiento

### Paso 1 — Catálogo

Leer los doce reportes completos. Producir un catálogo con, por reporte: patrón enunciado, fase de
origen, artefactos del framework alcanzados, severidad que el propio reporte propone y qué declara
que ya funciona. El catálogo es descriptivo; no agrupa todavía.

### Paso 2 — Agrupamiento por causa, no por síntoma

Agrupar los doce en familias tales que **los miembros de una familia se corrijan con la misma
intervención**. Ese es el criterio de agrupamiento y hay que poder defenderlo: si dos reportes
quedan en la misma familia y sus correcciones no comparten artefacto ni mecanismo, la familia está
mal formada. Contrastar el resultado contra el agrupamiento que propone el `README.md` de los
reportes y declarar dónde difiere y por qué.

### Paso 3 — Diagnóstico y corrección por familia

Por cada familia, y en un archivo propio de `OUTPUTs/`:

1. **Dónde se produce el fallo**, citando el artefacto y la sección vigentes.
2. **Por qué la normativa no lo atrapa**, con cita textual de lo que sí dice.
3. **La corrección propuesta**, redactada al nivel de detalle con que se va a aplicar: qué archivo,
   qué sección, qué texto entra y qué texto sale.
4. **La severidad** que implica según `README.md` §Reglas de intervención.
5. **Qué queda fuera y por qué** — en particular lo que exige decisión del responsable.
6. **Cómo se verifica**, tomando como base las secciones «cómo verificar» de los reportes.

### Paso 4 — Plan unificado

Analizar los archivos del paso 3 en conjunto y emitir un plan único que:

- ordene las correcciones por dependencia entre ellas, no por número de reporte;
- declare, correción por corrección, el archivo, la sección, la severidad y el efecto sobre la
  documentación ya generada;
- resuelva los conflictos entre familias de forma explícita, y no dejando ganar al último que
  escribe;
- separe lo aplicable de lo que requiere decisión del responsable, y no mezcle las dos cosas en la
  misma tabla;
- declare el versionado del conjunto y la entrada de `CHANGELOG.md` que corresponde.

### Paso 5 — Aplicación

Aplicar el plan. Cada archivo tocado sube versión y registra su control de cambios según
`README.md` §Reglas de intervención. La intervención alcanza a más de un archivo, de modo que
exige nota de coherencia con el patrón de `Coherencia-Auditoria-Marco.md`.

### Paso 6 — Verificación

Corroborar que lo aplicado es lo planificado, ítem por ítem, con una comprobación mecánica por cada
ítem cuando exista. La verificación se escribe en `OUTPUTs/` y declara sus tres resultados
posibles: aplicado, aplicado con desviación declarada, no aplicado con motivo. Un ítem del plan sin
fila en la verificación es un defecto de la verificación, no un ítem menor.

## 5. Formato de salida

Español rioplatense neutro técnico, con tildes y eñes. Sin emojis, sin marketing, sin negritas
decorativas. Nombres de archivo en ASCII, Título-Con-Guiones. Fechas `YYYY-MM-DD`. Es la
invariante D1 del framework que se está interviniendo, y no habría razón para que el material de
intervención no la cumpla.

## 6. Criterios de aceptación de esta corrida

- [ ] Los doce reportes están catalogados y cada uno pertenece exactamente a una familia.
- [ ] Cada familia tiene su archivo en `OUTPUTs/` con las seis secciones del paso 3.
- [ ] Ninguna afirmación sobre el framework vigente carece de cita a archivo y sección.
- [ ] El plan unificado no contiene dos correcciones que se contradigan.
- [ ] Lo que toca una invariante está separado y no aplicado sin decisión.
- [ ] Cada archivo del framework modificado subió versión y registró control de cambios.
- [ ] Existe nota de coherencia.
- [ ] La verificación cubre el cien por ciento de los ítems del plan.
