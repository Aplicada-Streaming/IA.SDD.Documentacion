# Nota de coherencia — E3 Categoría 11 como cuerpo documental de entrega

**Proyecto:** Framework SDD
**Documento:** Nota-Coherencia-E3.md
**Versión:** 1.1
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD

---

> **Nota posterior (E8, 2026-07-26).** La decisión abierta que esta nota eleva —reponer la tabla consolidada de tiempo a primer éxito (A-5)— se resolvió en **E8**, cerrada sin acción: la tabla no se había perdido, vive en `Rules-UX-UI-DX.md`, que es la categoría dueña de las métricas DX. La otra, sobre forzar §4.5 como sección de anti-patrones, quedó resuelta en E4 al ubicarla por título.

## 1. Alcance

Verificación de implantación de la etapa E3, la pieza más grande de la intervención. Ejecuta tres solicitudes, concentradas en la reescritura de `Rules-Documentacion.md`:

- **S3** — redefinir la categoría 11 como cuerpo documental de entrega organizado por rol de intervención, con sus dos ejes, sus artefactos de solución y de proyecto, su gating por cuerpo, sus fronteras y sus identificadores.
- **S5** — embeber las reglas de redacción de modo que el archivo quede autosuficiente.
- **S4** — el modelo de documentación viva en tres momentos, la cadencia, el ensayo de entrega y la bitácora de eventualidades, incorporados acá por **DEC-03** en lugar de en E4.

Al cerrar la etapa, el responsable del framework decidió incorporarle además las notas recíprocas de frontera en 05, 08 y 09, que la titularidad de E0 había asignado a E5. El motivo está en la observación 1.

E4 conserva la titularidad exclusiva de `Master-Prompt.md` y de la línea de Definition of Done en `Rules-Plan-Sprint.md`.

## 2. Inventario de archivos

| Archivo | Versión | Cambio |
| --- | --- | --- |
| `SDD/Devs/Rules/Rules-Documentacion.md` | 1.3 → **2.0** | Reescritura completa |
| `SDD/Devs/Rules/Rules-Arquitectura-Tecnica.md` | 1.2 → 1.3 | Nota recíproca de frontera con la categoría 11, en §0 |
| `SDD/Devs/Rules/Rules-Calidad-Y-Pruebas.md` | 1.4 → 1.5 | Nota recíproca de frontera con la categoría 11, en §0 |
| `SDD/Devs/Rules/Rules-Devops.md` | 1.4 → 1.5 | Nota recíproca de frontera con la categoría 11, en §0 |

Cuatro archivos. Ninguna creación, ningún renombre, ninguna eliminación. El archivo principal queda en 999 líneas y 86.852 bytes; las cuatro filas previas de su control de cambios se conservan verbatim y la fila 2.0 documenta la reescritura.

Las tres notas recíprocas se incorporaron a esta etapa por decisión del responsable del framework, tomada al cerrarla. Estaban asignadas a E5 por la titularidad acordada en E0; el detalle del cambio de alcance está en la observación 1.

Sube **major** porque cambian el alcance, el gating y el conjunto de artefactos de la categoría. Un proyecto documentado con la versión 1.x no cumple la 2.0 ni por asomo: le faltan dos cuerpos enteros.

### 2.1 Estructura resultante

La estructura canónica de nueve secciones se respeta. Se agrega tabla de contenido, admitida por §4.7 del propio archivo cuando el tamaño lo justifica, y necesaria con 999 líneas.

| Sección | Contenido | Origen |
| --- | --- | --- |
| §0 Posición en la cadena | Upstream de 02, 05, 06, 07, 08, 09 y 10; lector primario declarado | S3 |
| §0.1 Los dos ejes | Rol de intervención (organiza los cuerpos) y naturaleza del lector (se resuelve dentro de cada artefacto), con la regla dura de no bifurcar | S3.1 |
| §0.2 Fronteras | Las cinco fronteras con 09, 05, 08, 10 y 03 | S3.5 |
| §0.3 Tres momentos | Plan documental, actualización incremental y consolidación de cierre | S4 |
| §0.4 Cadencia | Tres disparadores en orden de precedencia, con el cierre de sprint como corte por defecto | S4.1 |
| §0.5 Ensayo de entrega | Dos niveles, gate humano, tres guiones por rol, regla de oro y registro de hallazgos | S4.2 |
| §0.6 Bitácora de eventualidades | Concepto, caso testigo del dispositivo USB, tabla de triaje y distinción con el sensado de deriva | S4.3 |
| §1.1–§1.3 Especialidad | Faceta Documentation Lead nueva, ocho variantes D8, seis colaboraciones multi-especialidad | S3 |
| §1.4 Estilo narrativo formativo | Doce reglas, transcriptas íntegras | S5 |
| §1.5 Doble audiencia | Cara humana y cara agente, transcriptas íntegras | S5 |
| §2.1–§2.4 Artefactos | Nivel solución (7) más tres cuerpos de proyecto (7 + 3 + 2) | S3.2, S3.3 |
| §2.5 Gating | Por cuerpo para los ocho tipos D8, más gating fino del cuerpo integrador | S3.4 |
| §3.1–§3.5 Nomenclatura | Patrón de nombres, identificadores estables, vinculación, versionado, README con matriz de ruteo | S3, S3.6 |
| §4.1 Cabecera | Frontmatter YAML de ocho campos más bloque legible y resumen ejecutivo obligatorio | S3.1 |
| §4.2–§4.5 Estructura por artefacto | Solución, cuerpo integrador, cuerpo mantenedor, cuerpo operador | S3.2, S3.3 |
| §4.6 Voz narrativa | Once reglas, transcriptas íntegras | S5 |
| §4.7 Formato markdown | Nueve reglas, transcriptas íntegras | S5 |
| §4.8 Tablas tipo | Matriz de ruteo, estado del cuerpo, mapa arquitectura a repositorio, bloque para agentes | S3 |
| §4.9 Anti-patrones | Veinte anti-patrones, once de ellos nuevos | S3, S4 |
| §5 Preguntas guía | Ocho bloques temáticos | S3 |
| §6 Criterios de aceptación | Siete grupos rotulados | S3, S4 |
| §7 Ejemplos genéricos | Tres fragmentos: recorrido de código, dispositivo del host, entrada de bitácora | S3 |
| §8 Prompt-snippet | Parametrizado por momento en curso | S3, S5 |
| §9 Control de cambios | Fila 2.0; las cuatro previas verbatim | — |

### 2.2 Qué se conservó de la versión anterior

Migró íntegro el cuerpo integrador: los siete artefactos Diátaxis con su estructura, el gating fino por tipo D8, la parametrización de `<sistema-objetivo>` con slug genérico, el sufijo `-v<X.Y>.md` obligatorio, los identificadores `ISSUE-XX`, los objetivos de tiempo a primer éxito y los anti-patrones que siguen siendo válidos. Las dos correcciones que la regla anterior imponía sobre el fuente SDD 1.0 se conservan explícitamente.

Se retiró el objetivo de TTFS como tabla propia y quedó embebido en la estructura del onboarding en §4.3, porque con tres cuerpos la tabla dedicada a un solo rol desbalanceaba la sección.

## 3. Las dos decisiones de diseño que tomé y conviene revisar

**Ubicación de S4 dentro de la estructura canónica.** DEC-03 fijó que S4 se escribe acá, pero no dónde. Los tres momentos, la cadencia, el ensayo y la bitácora son *cuándo y cómo se produce* la categoría, no *qué produce*, así que fueron a §0 «Posición en la cadena SDD» como subsecciones §0.3 a §0.6. La alternativa era abrirles una sección propia, lo que habría roto la estructura de nueve secciones que el framework impone a todo archivo de reglas. §0 queda pesado —seis subsecciones— pero la estructura canónica se respeta, que es la restricción dura.

**Ubicación de los anti-patrones.** `Master-Prompt.md` §8 y §10 citan «los anti-patrones a evitar (§4.5)» de forma genérica para cualquier archivo de reglas. La versión anterior de este archivo los tenía en §4.10, así que la correspondencia ya no se cumplía antes de esta etapa. Los dejé en **§4.9**, al final de §4, siguiendo el orden lógico (cabecera, estructura por artefacto, voz, formato, tablas, anti-patrones) y el precedente del propio archivo. **Se reporta**: si el responsable quiere que §4.5 sea anti-patrones en todos los archivos de reglas sin excepción, es un cambio transversal que excede esta etapa y afecta a varios archivos.

## 4. Lista de comprobación

| # | Comprobación | Resultado | Evidencia |
| --- | --- | --- | --- |
| 1 | Invariantes D1–D9 intactas | Cumple | D1 español rioplatense técnico. D2 UTF-8 sin BOM, LF; se eliminaron dos caracteres de ancho cero que habían quedado al anidar bloques de código. D3 y D4: los diez artefactos nuevos llevan Título-Con-Guiones y sufijo `-v<X.Y>.md`; la única excepción es `AGENTS.md`, declarada en §2.1 y §3.1 con su razón funcional. D5: sube versión in situ, sin copias paralelas. D6: control de cambios con las cuatro filas previas intactas. D7: los ejemplos usan dominios genéricos (pagos, adquisición) y el dispositivo del caso testigo se nombra `<identificador-del-dispositivo>`, sin literal concreto. D8: ocho valores, verificados en las dos tablas de gating. D9: el frontmatter exige `last_review`, los criterios se expresan como aserción y §4.9 prohíbe afirmar sin verificar |
| 2 | Autosuficiencia: cero referencias fuera de `/IA/IA.SDD/` | Cumple | Las once cadenas dan cero en todo el árbol. La única ocurrencia de `http://` es el placeholder `http://localhost:<puerto>`, forma que ya existía en `Maqueta-Rules.md` §4. Diátaxis, C4, arc42, AGENTS.md, Game Day, postmortem sin culpa, Living Documentation, Docs as Code, Continuous Documentation, Definition of Done y RFC 9457 se nombran sin enlazar |
| 3 | Referencias internas: todo lo citado existe | Cumple | Las 23 referencias `§X.Y` internas resuelven contra un encabezado real. Los 62 enlaces de la tabla de contenido resuelven contra su ancla. Los nombres de archivo citados que no existen en `IA.SDD` son los artefactos que la categoría genera en el repositorio destino, que es lo correcto |
| 4 | Vocabulario normalizado | Cumple | «Integrador», «mantenedor», «operador», «rol de intervención» y «naturaleza del lector» en todo el archivo. «Agente» siempre calificado como agente humano o agente de IA |
| 5 | Sin contradicción con etapas anteriores | Cumple | La dependencia con 10 se enuncia igual que en E1 y E2. `VER-XX` se **cita** desde §3.2 remitiendo a `Rules-Examples.md` §4.6, y no se redefine, tal como la observación 2 de la nota de E2 recomendaba. El gating por cuerpo no contradice el gating de la categoría 10, que es independiente |
| 6 | Control de cambios actualizado | Cumple | Fila 2.0 fechada 2026-07-26, sin citar el prompt de origen ni su repositorio. Autor «Reformulación SDD», siguiendo el precedente |
| 7 | El caso degenerado sigue produciendo el layout aplanado | Cumple | Declarado en la cabecera del archivo y en §2.1: en solución de un proyecto los artefactos de nivel solución van bajo `SDD/Docs/11-Documentacion/`, sin subnivel `Solucion/` |
| 8 | Nada fuera del alcance de la etapa fue modificado | Cumple con una ampliación autorizada | Cuatro archivos tocados. La ampliación son las tres notas recíprocas de frontera, autorizada por el responsable del framework al cerrar la etapa (observación 1). En los tres archivos el cambio se limita al párrafo de frontera en §0 y a la fila de control de cambios: no se tocó ningún artefacto, ningún gating ni ninguna otra sección |

Verificación adicional de formato: cero caracteres de ancho cero, marcas de bloque de código balanceadas contando el bloque de cuatro tildes invertidas de §7.2, cero saltos de nivel de encabezado fuera de bloques de código, cero enlaces de tabla de contenido sin ancla.

## 5. Observaciones

1. **Las notas recíprocas de frontera se escribieron acá, ampliando el alcance de la etapa.** S3.5 pide declarar las fronteras «en §0 de `Rules-Documentacion.md`», y eso quedó hecho en la primera pasada. La tabla de Referencias del prompt asigna además una frontera con 11 a `Rules-Arquitectura-Tecnica.md`, `Rules-Calidad-Y-Pruebas.md` y `Rules-Devops.md`, archivos que la titularidad de E0 había asignado a **E5**.

   El motivo por el que no bastaba con dejarlo diferido: el orquestador despacha a cada subagente con **un solo archivo de reglas**. AG-05 recibe `Rules-Arquitectura-Tecnica.md` y nunca lee `Rules-Documentacion.md`, así que una frontera declarada de un solo lado no frena al agente que puede cruzarla. Es exactamente el solapamiento que S3.5 viene a corregir.

   Consecuencia sobre la titularidad: `Rules-Arquitectura-Tecnica.md`, `Rules-Calidad-Y-Pruebas.md` y `Rules-Devops.md` quedan tocados por E3 (§0, frontera) y volverán a tocarse en E5 (§4 y §6, tabla de contenido). Son secciones disjuntas, de modo que la regla de titularidad por archivo y sección se mantiene. E5 no debe reescribir la frontera.
2. **§0 quedó con seis subsecciones y es la sección más pesada del archivo.** Es consecuencia de meter S4 completo bajo la estructura canónica. Un lector que busque solo «qué produce esta categoría» tiene que atravesarla. Lo mitiga la tabla de contenido, pero es una asimetría real respecto de los demás archivos de reglas.
3. **El `AGENTS.md` es la primera excepción declarada a D3 y D4 en el framework.** Se admite por razón funcional: un archivo que las herramientas no encuentran no cumple su función. El artefacto versionado que lo gobierna, `Contrato-Agentes-v<X.Y>.md`, sí sigue la convención. Queda registrado acá porque abre un precedente y conviene que sea deliberado y no accidental.
4. **La tabla de TTFS dejó de existir como tabla propia.** Sus objetivos —Hello world en menos de 5 minutos, primer caso real en menos de 30, integración en menos de 1 hora— quedaron dentro de la estructura del onboarding en §4.3, con el criterio de reescritura si se supera. Se perdió la vista consolidada por tarea. **Decisión abierta**: si el responsable la quiere de vuelta, va en §4.8 tablas tipo.
5. **`Rules-Documentacion.md` pasó de 33 KB a 87 KB.** Es el segundo archivo de reglas más grande del framework, detrás de `Rules-UX-UI-DX.md`. El tamaño es consecuencia de tres cuerpos en lugar de uno, más las reglas de redacción transcriptas que S5 exige embeber en lugar de referenciar. La transcripción sola aporta unos 8 KB.

## 6. Veredicto

**CONFORME.**

La categoría 11 quedó redefinida como cuerpo documental de entrega con los dos ejes correctamente separados, tres cuerpos organizados por rol de intervención, el cuerpo mantenedor obligatorio para los ocho tipos D8, las cinco fronteras declaradas, los cinco identificadores fijados, el modelo de documentación viva completo con sus cuatro mecanismos, y las cuatro secciones de reglas de redacción transcriptas íntegras de modo que el subagente no necesite ningún otro archivo para saber cómo redactar. La estructura canónica de nueve secciones se respeta.

Las fronteras quedan declaradas en las dos direcciones, de modo que ningún subagente pueda cruzarlas por desconocimiento: la categoría 11 sabe qué no le corresponde, y 05, 08 y 09 saben qué le corresponde a 11.

Las cinco observaciones son una ampliación de alcance autorizada, una asimetría estructural asumida, un precedente que conviene dejar explícito y dos decisiones abiertas que se elevan al responsable del framework. Ninguna bloquea.

Corresponde detenerse y esperar confirmación humana antes de arrancar E4.

## 7. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.0 | 2026-07-26 | Nota de coherencia inicial de la etapa E3: inventario y estructura resultante del archivo reescrito, registro de qué se conservó de la versión anterior, las dos decisiones de diseño sobre ubicación de S4 y de los anti-patrones, lista de comprobación de ocho puntos con verificación D1–D9, cinco observaciones y veredicto CONFORME. | Reformulación SDD |
| 1.1 | 2026-07-26 | Ampliación de alcance decidida por el responsable del framework al cerrar la etapa: se incorporan las notas recíprocas de frontera con la categoría 11 en `Rules-Arquitectura-Tecnica.md`, `Rules-Calidad-Y-Pruebas.md` y `Rules-Devops.md`, que estaban asignadas a E5. Se actualizan el inventario, la observación 1, la comprobación 8 y el veredicto. | Reformulación SDD |
