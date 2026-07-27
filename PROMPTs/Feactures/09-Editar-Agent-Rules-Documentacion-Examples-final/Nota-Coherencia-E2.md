# Nota de coherencia — E2 Categoría 10 con doble arista

**Proyecto:** Framework SDD
**Documento:** Nota-Coherencia-E2.md
**Versión:** 1.0
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD

---

> **Nota posterior (E8, 2026-07-26).** Las decisiones abiertas que esta nota eleva —los pisos de samples expresados en cantidad (A-3) y la mención de `VER-XX` en `Rules-Calidad-Y-Pruebas.md` (A-2)— se resolvieron en **E8**. A-3 se cerró sin acción. A-2 resultó estar mal enunciada: no era una mención faltante sino una **contradicción** entre esta etapa y la categoría 08, que E8 corrigió. Ver `Nota-Coherencia-E8.md` §4.

## 1. Alcance

Verificación de implantación de la etapa E2, que ejecuta la solicitud S2: redefinir la categoría 10 con doble arista, incorporar el contrato de verificación con su identificador `VER-XX`, declarar las dos pasadas de generación, y extender la matriz de sensado de deriva para que cubra contratos y comportamiento además de superficies visuales.

E2 amplía; no reemplaza. Todo lo que la categoría ya definía —tabla maestra de markdowns explicativos, pisos de cantidad de samples por tipo D8, matriz D8 a carpetas en `/samples`, correspondencia 1:1 entre cada `ejemplo-XX-*.md` y su carpeta ejecutable— se conserva íntegro y verificado.

## 2. Inventario de archivos

| Archivo | Versión | Cambio |
| --- | --- | --- |
| `SDD/Devs/Rules/Rules-Examples.md` | 1.3 → **2.0** | Redefinición de la categoría con doble arista |
| `SDD/Devs/Rules/Deriva-Rules.md` | 1.0 → 1.1 | Extensión del sensado a contratos y comportamiento |

Dos archivos. Ninguna creación, ningún renombre, ninguna eliminación.

### 2.1 Detalle de `Rules-Examples.md`

| Sección | Estado | Contenido |
| --- | --- | --- |
| §0.1 Las dos aristas del sample | Nueva | Arista A de referencia de integración (rol vigente, se conserva) y arista B de arnés de autovalidación (rol nuevo), con la regla dura de no bifurcar el sample en una versión ilustrativa y otra verificable |
| §0.2 Momento de generación: dos pasadas | Nueva | Pasada de diseño pre-código y pasada de ejecución durante la codificación, con el racional de por qué el criterio de aceptación se declara antes de escribir el código |
| §4.2 Secciones obligatorias | Modificada | De nueve a **diez** secciones. La sección 9 es el contrato de verificación; control de cambios pasa a 10 |
| §4.4 Tablas tipo | Ampliada | Tabla de contratos de verificación del README de sección, con las seis columnas de la vista de conjunto |
| §4.5 Anti-patrones | Ampliada | Siete anti-patrones propios de la arista B, entre ellos el criterio redactado como prosa y la evidencia inventada |
| §4.6 Contrato de verificación | Nueva | Los cinco campos, el formato YAML completo en sus dos estados, la regla dura de aserción evaluable, el identificador `VER-XX` y la vinculación con el sensado de deriva |
| §5.5 Contrato de verificación | Nueva | Ocho preguntas guía; mantenimiento se renumera a §5.6 |
| §6 Criterios de aceptación | Ampliada | Nueve criterios propios de la arista B, separados en un bloque rotulado |
| §7.1 y §7.2 Ejemplos genéricos | Ampliados | Cada uno incorpora su sección 9 con contrato completo. El de la librería queda en estado `Verificado` con evidencia real; el de la API queda en `No verificado — sin código`, de modo que los dos estados de la pasada estén ejemplificados |
| §8 Prompt-snippet | Modificado | Declara la doble arista, los cinco campos, la regla de aserción evaluable y la pasada en curso. Corrige el placeholder de especialidad, que decía `{{ESPECIALIDAD-VARIANTE-11}}` |

Sube **major** porque la categoría incorpora un rol nuevo y un artefacto obligatorio dentro de cada markdown explicativo. Un proyecto documentado con la versión 1.x no cumple la 2.0 sin agregar contratos.

### 2.2 Detalle de `Deriva-Rules.md`

| Sección | Estado | Contenido |
| --- | --- | --- |
| §2 encabezado | Ampliada | Declara la cuarta fuente de sondas y la distinción de dimensión: las sondas de maqueta miden parecido con lo aprobado, las de verificación miden si el sistema sigue haciendo lo especificado |
| §2.4 Contratos de verificación (`VER-XX`) | Nueva | Las sondas que aporta la categoría 10, sus tres consecuencias operativas y el tratamiento de un contrato todavía sin código |
| §2.3 Matriz de sensado | Modificada | `VER-XX` admitido en la columna de elemento de línea de base y en la de método de verificación. Declara que un proyecto con categoría 10 emite matriz aunque no ejecute Fase B2 |
| §3 Umbrales | Ampliada | Dimensión nueva de contratos y comportamiento, con su umbral menor y su umbral mayor |
| §4 Puntos de sensado | Modificada | De cuatro a **cinco** momentos. Se agrega el alta de sondas al cerrar la fase que genera la categoría 10, y se explica la asimetría entre una sonda que exige mirar y una que se corre sola |
| §6 Criterios de aceptación | Ampliada | Cuatro criterios nuevos, incluido el que impide que un proyecto con categoría 10 quede sin matriz |
| §7 Anti-patrones | Ampliada | Cuatro anti-patrones nuevos, entre ellos dejar sin matriz a un proyecto sin interfaz visual y transcribir la evidencia del contrato dentro de la matriz |

## 3. El hueco que esta etapa cierra, además del declarado

S2 pedía que los contratos de verificación fueran sondas adicionales de la matriz. Al implementarlo apareció una consecuencia que el prompt no anticipaba y que conviene dejar registrada, porque amplía el alcance del instrumento más de lo que la solicitud enunciaba.

La matriz de sensado se emitía **solo** desde la Fase B2, y la Fase B2 solo corre en proyectos con `requiere_maqueta` en `true`. Un `worker-service`, una `library` o un `cli-tool` no tenían ningún instrumento de sensado de deriva: quedaban cubiertos por la trazabilidad D6 y por la auditoría entre fases, que son los dos mecanismos que `Deriva-Rules.md` §0 declara insuficientes por sí solos.

Con las sondas `VER-XX` eso deja de ser así, porque no dependen de la maqueta. La regla ahora declara que todo proyecto con categoría 10 emite matriz, la abra AG-03M al cerrar la Fase B2 o AG-08 en la Fase E. Es una extensión de cobertura, no un cambio de mecánica, y se apoya en artefactos que la propia S2 introduce.

## 4. Lista de comprobación

| # | Comprobación | Resultado | Evidencia |
| --- | --- | --- | --- |
| 1 | Invariantes D1–D9 intactas | Cumple | D1 español rioplatense técnico. D2 UTF-8 sin BOM, LF. D3 y D4: `VER-XX` sigue el patrón de dos dígitos de `SUP-XX`, `CU-XX` y `ADR-XX`; los nombres de archivo no cambian. D5: ambos archivos suben versión in situ. D6: control de cambios actualizado en los dos. D7: los ejemplos usan dominios genéricos (parsing CSV, API de pagos), sin literales del dominio fuente. D8: los ocho valores presentes y sin alterar en la matriz de `/samples`. **D9**: el contrato de verificación es una aplicación directa de D9 —evidencia localizable, reproducible, contemporánea e independiente—, y §4.5 prohíbe explícitamente la evidencia inventada, copiada o sin fecha |
| 2 | Autosuficiencia: cero referencias fuera de `/IA/IA.SDD/` | Cumple | Las once cadenas dan cero. Cero URLs nuevas en el diff. *Specification by Example* y *Executable Specification* se nombran sin enlazar, según la restricción |
| 3 | Referencias internas: todo lo citado existe | Cumple | Las referencias cruzadas nuevas son `Rules-Examples.md` §4.6 desde `Deriva-Rules.md` §2.4, y `Deriva-Rules.md` §2 y §4 desde `Rules-Examples.md` §4.6. Ambas resuelven. `§4.5 Anti-patrones` conserva su número en los dos archivos, que es lo que `Master-Prompt.md` §8 y §10 dan por sentado de forma genérica |
| 4 | Vocabulario normalizado | Cumple | Las adiciones usan «integrador» y «rol de intervención». No se introdujo ningún término del vocabulario viejo |
| 5 | Sin contradicción con etapas anteriores | Cumple | E1 dejó la dependencia como «10 demuestra con código ejecutable y verificable, 11 explica y referencia». §0.1 y §0.2 la desarrollan sin alterarla. La numeración y el subagente AG-10 fijados en E1 se respetan |
| 6 | Control de cambios actualizado | Cumple | Una fila por archivo, fechada 2026-07-26, sin citar el prompt de origen ni su repositorio |
| 7 | El caso degenerado sigue produciendo el layout aplanado | Cumple | E2 no toca el layout. `Master-Prompt.md` §3.5 sin cambios |
| 8 | Nada fuera del alcance de la etapa fue modificado | Cumple | Solo los dos archivos que S2 nombra. Ver observación 1 |

Verificación adicional de formato: los cuatro bloques YAML tienen indentación par y consistente, sin tabuladores; las marcas de bloque de código cierran de a pares en ambos archivos; no hay saltos de nivel de encabezado.

## 5. Observaciones

1. **`Rules-Calidad-Y-Pruebas.md` no menciona las sondas `VER-XX`, y no se lo tocó.** La matriz de sensado vive en la categoría 08 y AG-08 es quien la incorpora a la estrategia de testing, así que su archivo de reglas es el lugar natural para nombrar la clase de sonda nueva. Queda fuera del alcance por la restricción que limita los cambios en las categorías 00 a 09 a cuatro casos tasados, entre los cuales S2 solo habilita `Deriva-Rules.md`. La mecánica funciona igual, porque `Rules-Calidad-Y-Pruebas.md` delega el sensado en `Deriva-Rules.md` por referencia y no por copia. **Se reporta como decisión del responsable del framework**: si se quiere una mención explícita en 08, hay que habilitarla, y el lugar natural es E5.
2. **El identificador `VER-XX` queda declarado en dos archivos de reglas**, `Rules-Examples.md` §4.6 como definición y `Deriva-Rules.md` §2.4 como uso. S3.6 lo va a listar además entre los prefijos que fija la categoría 11. No es duplicación de contenido: es una definición y dos referencias. Conviene que E3 lo cite y no lo redefina.
3. **El prompt-snippet tenía un placeholder desactualizado.** Decía `{{ESPECIALIDAD-VARIANTE-11}}` cuando la categoría ya es la 10. Es un residuo que E1 no detectó porque no buscaba esa forma de la cadena. Corregido acá.
4. **Los pisos de cantidad de samples por tipo D8 no se revisaron a la luz de la arista B.** Un proyecto puede cumplir el piso de samples y aun así dejar CU críticos sin ninguna sonda que los ejercite. El criterio de aceptación nuevo cubre el caso exigiendo justificación en `Decisiones-Proyecto-v1.0.md`, pero la tabla de pisos sigue expresada en cantidad de samples y no en cobertura de casos de uso. **Decisión abierta**: si el responsable prefiere que el piso se exprese en cobertura, es un cambio de §2.2 que excede lo que S2 pide.

## 6. Veredicto

**CONFORME.**

La categoría 10 quedó redefinida con sus dos aristas sin perder nada de lo que ya definía, con el contrato de verificación como artefacto obligatorio de cada markdown explicativo, su formato máquina-legible en los dos estados de la pasada, y su regla dura de aserción evaluable respaldada por siete anti-patrones y nueve criterios de aceptación. El sensado de deriva pasó de cubrir superficies visuales a cubrir también contratos y comportamiento, con el efecto lateral favorable de alcanzar por primera vez a los proyectos sin interfaz visual.

Las cuatro observaciones son dos remisiones a etapas posteriores, una corrección menor ya aplicada y una decisión abierta que se eleva al responsable del framework. Ninguna bloquea.

Corresponde detenerse y esperar confirmación humana antes de arrancar E3.

## 7. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.0 | 2026-07-26 | Nota de coherencia inicial de la etapa E2: inventario de los dos archivos afectados con detalle por sección, registro de la ampliación de cobertura del sensado a proyectos sin maqueta, lista de comprobación de ocho puntos con verificación D1–D9, cuatro observaciones y veredicto CONFORME. | Reformulación SDD |
