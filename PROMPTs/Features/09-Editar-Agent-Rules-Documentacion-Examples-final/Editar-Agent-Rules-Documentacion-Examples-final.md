# Tool-Prompt — Reordenar las categorías 10 y 11 del Framework SDD y redefinir el cuerpo documental de entrega

> **Invocación**: `Lee y ejecuta /IA/SDD/IA.SDD.Documentacion/PROMPTs/Features/09-Editar-Agent-Rules-Documentacion-Examples-final/Editar-Agent-Rules-Documentacion-Examples-final.md`
>
> **Overview**: Intervenir las reglas constructivas del Framework SDD para (a) intercambiar las categorías 10 y 11, (b) redefinir la nueva categoría 10 (`10-Examples`) con doble arista —referencia de integración y arnés de autovalidación—, (c) redefinir la nueva categoría 11 (`11-Documentacion`) como cuerpo documental de entrega organizado por rol de intervención (integrador, mantenedor, operador) y destinado a que un lector humano sin contexto previo comprenda el desarrollo por primera vez, (d) incorporar el modelo de documentación viva en tres momentos, con cadencia anclada al cierre de sprint, ensayo de entrega como validación de utilidad y bitácora de eventualidades para capitalizar la experiencia de despliegue y operación, (e) agregar tabla de contenido a los documentos de las categorías 00 a 09, y (f) dotar al framework de una superficie de entrada —README raíz como router— y de una guía de desarrollo y extensibilidad. La intervención se ejecuta segmentada en ocho etapas con control de coherencia entre cada una.
>
> **Este prompt es autocontenido**: todas las reglas de redacción, tablas de gating, nomenclatura y criterios están embebidos abajo. No requiere leer ningún otro markdown para conocer *qué* hacer. Los archivos listados en «Referencias» son **objetivos de edición**, no fuentes de contenido.

---

## Contexto

### Qué es el Framework SDD

SDD (*Specification-Driven Development*) es un template de trabajo que genera la documentación viva de una solución de software **antes** de escribir código. Vive en el repositorio `IA.SDD` y opera sobre un modelo de **tres repositorios separados por responsabilidad**:

| Rol | Escritura | Contiene |
|---|---|---|
| **Framework SDD** (`IA.SDD`, fuente, solo lectura) | Nunca se toca durante una corrida normal | Reglas, plantillas de intake, master-prompt, guías, prompt de entrada |
| **Repositorio destino** | El orquestador escribe acá | El intake (`SDD/Intake/`), la documentación generada (`SDD/Docs/`) y, más adelante, el código |
| **Repositorio de documentación** | El usuario, a mano | Los tool-prompts reejecutables (`PROMPTs/`), el material de investigación (`INPUTs/`), indexación y análisis |

Esta intervención es la excepción: escribe sobre el framework mismo.

Su modelo de trabajo distingue dos niveles:

- Una **solución** agrupa N proyectos (N ≥ 1). La solución no tiene tipo propio: es el contenedor que enumera proyectos, roles, dependencias y nombres de código.
- Cada **proyecto** declara exactamente uno de los 8 tipos cerrados **D8**: `library`, `web-monolith`, `web-microservices`, `desktop-app`, `mobile-app-maui`, `rest-api`, `cli-tool`, `worker-service`.

El usuario completa un único documento de entrada, el `SOLUTION-INTAKE`, con tres partes: A negocio (§1–§12), B composición (§13–§16, donde §13 es la tabla de proyectos tipados con su grafo de dependencias acíclico) y C técnica por proyecto (§17, bloque P.1–P.12 repetido). El orquestador deriva de §13 el `SOLUTION-MANIFEST`, lo valida, lo presenta para confirmación y recién entonces genera documentación proyecto por proyecto en orden topológico.

La documentación se organiza en **12 categorías numeradas**, cada una gobernada por un archivo de reglas constructivas propio en `/IA/IA.SDD/SDD/Devs/Rules/`:

| Cat. | Carpeta | Archivo de reglas | Nivel |
|---|---|---|---|
| 00 | `00-Contexto/` | `Rules-Contexto.md` | Solución |
| 01 | `01-Necesidades-Negocio/` | `Rules-Necesidades-Negocio.md` | Solución |
| 02 | `02-Especificacion-Funcional/` | `Rules-Especificacion-Funcional.md` | Proyecto |
| 03 | `03-UX-UI-DX/` | `Rules-UX-UI-DX.md` | Proyecto |
| 04 | `04-Prompts-AI/` | `Rules-Prompts-AI.md` | Proyecto |
| 05 | `05-Arquitectura-Tecnica/` | `Rules-Arquitectura-Tecnica.md` | Proyecto + Solución |
| 06 | `06-Backlog-Tecnico/` | `Rules-Backlog-Tecnico.md` | Proyecto |
| 07 | `07-Plan-Sprint/` | `Rules-Plan-Sprint.md` | Proyecto |
| 08 | `08-Calidad-Y-Pruebas/` | `Rules-Calidad-Y-Pruebas.md` | Proyecto |
| 09 | `09-Devops/` | `Rules-Devops.md` | Proyecto + Solución |
| **10** | **`10-Developer-Guide/`** | **`Rules-Developer-Guide.md`** | Proyecto |
| **11** | **`11-Examples/`** | **`Rules-Examples.md`** | Proyecto |

Reglas transversales: `Root-Rules.md` (invariantes D1–D8 y layout canónico), `Intake-Rules.md`, `Maqueta-Rules.md`, `Deriva-Rules.md`. Son **dieciséis archivos** en `SDD/Devs/Rules/`.

### Qué ya existe en el framework y no hay que rehacer

Verificar este inventario antes de escribir nada. El repositorio tiene material sustancial que esta intervención debe **respetar, referenciar internamente y en algunos casos corregir**, pero nunca duplicar:

| Artefacto | Ruta | Tamaño y estado | Relación con esta intervención |
|---|---|---|---|
| **Marco Teórico SDD** | `SDD/Devs/Guides/Marco-Teorico-SDD-v1.0.md` | ~151 KB, 14 secciones §1–§14: fundamentos, metodología, 13 especialidades, ágil, descomposición, estilos arquitectónicos, UX/DX, calidad, DevOps, prompting, anti-patrones, glosario, bibliografía | **El marco teórico ya está escrito.** No se reescribe ni se duplica. Se lo actualiza solo donde el intercambio 10↔11 y las Fases I/J lo dejen desactualizado |
| **Nota de coherencia del marco** | `SDD/Devs/Guides/Coherencia-Auditoria-Marco-v1.0.md` | ~7 KB: alcance, inventario, verificación D1–D8, trazabilidad, observaciones, veredicto | **Es el patrón a reutilizar** para las notas de coherencia por etapa que exige la sección «Etapas de ejecución» |
| **Getting Started Guide** | `SDD/Guides/SDD-Getting-Started-Guide.md` | ~31 KB, §1–§9: modelo de tres repositorios, prerrequisitos, flujo de 6 pasos, tool-prompts, ejemplo aplicado, errores de arranque | Se actualiza solo si el intercambio lo desactualiza |
| **User Guide** | `SDD/Guides/SDD-User-Guide.md` | ~115 KB | Objetivo de S7 |
| **Development Guide** | `SDD/Guides/SDD-Development-Guide.md` | **Archivo vacío, 0 bytes** | Objetivo de S10. Hay que escribirlo desde cero |
| **README raíz** | `README.md` | **7 líneas**: título más tres enlaces (guía de usuario, marco teórico, y una URL externa a un resumen) | Objetivo de S9. La URL externa es preexistente y se conserva |
| Carpetas adicionales | `SDD/Devs/Bootstrap/`, `SDD/Devs/References/`, `SDD/Devs/Reformulacion/`, `SDD/Devs/Modelos-UX-UI/`, `SDD/Devs/Intake/`, `PROMPTS/`, `Templates/` | — | Entran en el barrido de propagación de S1.5. Inventariarlas en la Etapa 0 antes de tocar nada |

El orquestador (`/IA/IA.SDD/SDD/Devs/Orchestrator/Master-Prompt.md`) despacha subagentes especializados por fase, con auditoría independiente entre fases y confirmación humana en cada corte. El orden vigente es:

```
Fase validación de intake  (una vez)   → valida SOLUTION-INTAKE, deriva SOLUTION-MANIFEST
Fase A  (solución, una vez)            → 00 + 01 + audit A
Fase B  (por proyecto)                 → 02 + 03 + 04 (si aplica) + audit B
Fase B2 (por proyecto, opcional)       → validación visual de maqueta
Fase C  (por proyecto)                 → 05 + audit C
Fase D  (por proyecto)                 → 06 + 07 + audit D
Fase E  (por proyecto)                 → 08 + audit E
Fase F  (por proyecto)                 → 09 + 10 (si aplica) + audit F
Fase G  (por proyecto)                 → 11 (si aplica) + audit G
Fase H  (consolidación de solución)    → vista de solución + pipeline + README raíz + audit final
Paso 6  (humano)                       → handoff a codificación
```

### Estado actual de las categorías 10 y 11

**Categoría 10 (`10-Developer-Guide/`)** produce siete artefactos organizados por los cuatro cuadrantes del framework Diátaxis: `conceptos-fundamentales` (Explanation), `guia-onboarding-developer` (Tutorial), `guia-integracion-<sistema-objetivo>` (How-to), `referencia-api` y `referencia-cli` (Reference), `troubleshooting` (How-to de diagnóstico) y `glosario-tecnico` (Reference), más el README de sección. Es obligatoria para `library`, `rest-api` y `cli-tool`; recomendada para `web-microservices` con API pública; opcional para los cuatro tipos restantes.

**Categoría 11 (`11-Examples/`)** produce un markdown explicativo por sample (`ejemplo-01-basico`, `ejemplo-02-intermedio`, `ejemplo-03-avanzado`) con correspondencia 1:1 contra carpetas ejecutables en `/samples/` del repositorio destino, con pisos de cantidad y estructura de carpetas fijados por tipo D8.

La dependencia declarada hoy entre ambas es `10 → 11`: *«10 explica y referencia, 11 demuestra con código completo»*.

### Los tres déficits que este trabajo corrige

**Déficit 1 — El framework modela un solo rol de intervención externo.**

La categoría 10 declara explícitamente su alcance único: documentación *«orientada al consumidor de la solución, no a su constructor»*, y refuerza la exclusión como anti-patrón (*«Conceptos fundamentales que documentan la implementación interna → confunde audiencia y duplica 05»*). La consecuencia es que ningún artefacto del framework sirve a dos roles reales:

- El **mantenedor** que retoma el proyecto meses después para conocerlo, intervenirlo manualmente y agregarle funcionalidad propia. La categoría 05 registra *qué se decidió* (vistas, ADRs, contratos), pero no existe puente entre esa arquitectura conceptual y el árbol de archivos real del repositorio.
- El **operador** que necesita montar un servicio. La categoría 09 cubre la *política* de despliegue (`entornos-deploy`: ambientes, IaC, secretos, promoción) y la *publicación* del artefacto (`guia-publicacion-image-docker`: prerrequisitos, stage de publicación, verificación post-publish, rollback). Ninguno de los dos documenta cómo **levantar y correr** el servicio, ni cómo arrancar una solución multi-proyecto completa en orden.

Como la categoría 10 solo modela al integrador, queda declarada **opcional para 4 de los 8 tipos D8** (`web-monolith`, `desktop-app`, `mobile-app-maui`, `worker-service`). Pero todo proyecto, sin excepción, va a ser mantenido por alguien.

**Déficit 1 bis — No hay ningún documento escrito para que una persona conozca el desarrollo por primera vez.**

SDD es un framework operado por agentes de IA: los subagentes construyen la especificación a partir del intake, se realimentan con esa especificación fase tras fase, y la cadena continúa hasta la codificación y eventualmente hasta el despliegue y las pruebas. Las categorías 00 a 09 están escritas para sostener esa cadena. Su densidad, su formato y su granularidad son adecuados a un lector que ya trae el contexto acumulado de las fases anteriores.

Ese diseño deja fuera un caso que el framework nunca atiende: **la persona que llega al desarrollo sin haber participado de ninguna fase**. Un mantenedor nuevo, un implementador externo, un equipo que recibe el producto. Esa persona no puede reconstruir el sistema leyendo la cadena de especificación —tendría que recorrerla entera y en orden—, y tampoco puede pedirle contexto al equipo que la generó, porque en buena medida ese equipo fue una secuencia de agentes.

La categoría 11 existe para cubrir exactamente ese hueco. Es el único cuerpo del framework cuyo lector primario es un ser humano en primer contacto, y de ahí sale su carga narrativa: definiciones, modelo mental, ejemplos, diagramas y recorridos explicados. Las categorías 00 a 09 no la necesitan y no deben adoptarla.

**Déficit 2 — Los criterios de calidad de la categoría 10 son inaplicables en su fase actual.**

La categoría 10 se genera en Fase F, previa al handoff a codificación. Sus criterios de aceptación y sus preguntas guía exigen verificaciones que requieren código existente:

| Criterio vigente en `Rules-Developer-Guide.md` | Por qué no se puede cumplir en Fase F |
|---|---|
| «Los snippets se validan en CI contra la versión documentada (compilan o se ejecutan al copy-paste)» | No hay código ni pipeline todavía |
| «¿Los códigos de error documentados coinciden con los que el código realmente emite?» | No hay código que emita nada |
| «¿El TTFS objetivo se cumple en una corrida real con un developer que nunca vio el proyecto?» | No hay nada ejecutable |
| Anti-patrón «Pseudo-código en lugar de la API real» | Pre-código, todo snippet es contra una API inexistente |
| Anti-patrón «Docs desactualizados respecto a la API real» | No hay API real de la cual desviarse |

El archivo está redactado con la voz de una categoría post-implementación pero ubicado en una fase pre-implementación. La misma tensión afecta a la categoría 11, que promete samples «ejecutables».

**Déficit 3 — Los ejemplos tienen una sola arista.**

La categoría 11 vigente es íntegramente ilustrativa: demuestra al integrador cómo integrar. No cumple el segundo rol que un sample ejecutable puede cumplir: servir de **arnés de autovalidación** durante la codificación asistida por IA, verificando que cada incremento construido sigue satisfaciendo los casos de uso especificados.

### Decisiones ya tomadas por el responsable del framework

1. **Se intercambian las categorías 10 y 11.** La actual 11 (`Examples`) pasa a ser la **10**; la actual 10 (`Developer-Guide`) pasa a ser la **11** y se redefine como cuerpo documental de entrega. El fundamento de la inversión es la dependencia real: los ejemplos deben existir antes, porque son insumo de la documentación final y porque su arista de autovalidación opera durante la codificación; la documentación final los referencia y se escribe con el sistema ya construido.
2. **La categoría 11 se genera en tres momentos**, no una sola vez (ver Solicitud S4).
3. **El agente que redacta la categoría 11 adopta el estilo narrativo formativo** descripto en las Reglas de este prompt: definiciones, explicaciones, ejemplos, diagramas, secciones jerárquicas, doble audiencia humano + IA. Se descarta explícitamente todo lo relativo a indexado de conocimiento: no aplica a este cuerpo documental.
4. **Las categorías 00 a 09 conservan su carga documental actual** —son leídas principalmente por la IA en las etapas de especificación y codificación— pero incorporan tabla de contenido para mejorar el seguimiento y la consulta por parte de lectores humanos.

5. **Se normaliza el vocabulario de actores del framework.** El vocabulario vigente confunde dos cosas distintas: quién interviene y de qué naturaleza es el lector. Se fija esta correspondencia, de aplicación obligatoria en todo el árbol `/IA/IA.SDD/`:

   | Término vigente | Término normalizado | Motivo |
   |---|---|---|
   | consumidor | **integrador** | El framework ya usa «developer integrador» en `Rules-Developer-Guide.md` §1.2 y en `Rules-UX-UI-DX.md` §5; se adopta el término que ya está en uso |
   | constructor | **mantenedor** | «Constructor» designa a quien lo hizo; el rol que hay que documentar es el de quien lo retoma y lo evoluciona, que suele ser otra persona u otro agente |
   | implementador | **operador** | Se unifica bajo el término de la práctica SRE, que ya gobierna el runbook |
   | audiencia | **rol de intervención** (eje 1) / **naturaleza del lector** (eje 2) | Un único término estaba cubriendo dos ejes ortogonales |

   «Agente» se usa como término genérico que abarca a personas y a modelos, y se califica siempre: **agente humano** o **agente de IA**. Es coherente con el hecho de que SDD es un framework operado por agentes de IA a lo largo de toda su cadena, donde el rol —especificar, codificar, desplegar, verificar— es lo que distingue a un interviniente, no su naturaleza.

---

## Objetivo

Reescribir y ajustar las reglas constructivas del Framework SDD para que la última etapa documental produzca un cuerpo organizado por los tres roles de intervención sobre el producto terminado —integrar, mantener, operar—, legible tanto por agentes humanos como por agentes de IA en un mismo artefacto, con el agente humano en primer contacto como lector primario, sostenido incrementalmente a lo largo de la construcción y no redactado de una sola vez al cierre; y para que los ejemplos ejecutables cumplan simultáneamente su rol ilustrativo y su rol de arnés de verificación.

---

## Solicitudes

### S1 — Ejecutar el intercambio de categorías 10 ↔ 11

1. Renombrar el archivo de reglas `/IA/IA.SDD/SDD/Devs/Rules/Rules-Developer-Guide.md` a `/IA/IA.SDD/SDD/Devs/Rules/Rules-Documentacion.md`. Su contenido se reescribe según S3.
2. Mantener el archivo `/IA/IA.SDD/SDD/Devs/Rules/Rules-Examples.md` con su nombre, cambiando su carpeta target de `11-Examples/` a `10-Examples/`. Su contenido se amplía según S2.
3. Fijar las carpetas target nuevas:
   - `SDD/Docs/Proyectos/<Nombre-Proyecto>/10-Examples/`
   - `SDD/Docs/Proyectos/<Nombre-Proyecto>/11-Documentacion/`
   - En solución de un solo proyecto (caso degenerado, layout aplanado): `SDD/Docs/10-Examples/` y `SDD/Docs/11-Documentacion/`.
   - Los artefactos de nivel solución de la categoría 11 van bajo `SDD/Docs/Solucion/11-Documentacion/`, y en caso degenerado bajo `SDD/Docs/11-Documentacion/`.
4. Invertir la relación de dependencia declarada entre ambas categorías. La formulación nueva es: **la categoría 10 demuestra con código ejecutable y verificable; la categoría 11 explica, referencia y enlaza esos ejemplos**. El upstream de 11 incluye ahora a 10.
5. **Barrido de propagación obligatorio.** Antes de dar por cerrado el intercambio, ejecutar una búsqueda literal sobre todo el árbol `/IA/IA.SDD/` de las cadenas `10-Developer-Guide`, `11-Examples`, `Rules-Developer-Guide`, `Developer-Guide`, `categoría 10`, `categoria 10`, `categoría 11`, `categoria 11`, `AG-10`, `AG-11`, `Fase F` y `Fase G`. Revisar **cada** ocurrencia y decidir caso por caso: las cuatro primeras cadenas siempre se corrigen; las referencias a categorías y a subagentes se corrigen según el mapeo nuevo; las de `Fase F` y `Fase G` **no siempre cambian** —las fases conservan su letra, cambia qué categoría produce cada una—, así que se corrige el contenido asociado, no la etiqueta. Reportar el conteo de ocurrencias encontradas, corregidas y deliberadamente no tocadas, con el motivo de estas últimas.
6. Reasignar los identificadores de subagente: el subagente de ejemplos pasa a ser **AG-10 Developer Advocate**; el de documentación pasa a ser **AG-11 Technical Writer / Documentation Lead**.

7. **Normalizar el vocabulario de actores** en todo el árbol `/IA/IA.SDD/` según la tabla de correspondencia de la decisión 5 del Contexto. Buscar las cadenas `consumidor`, `constructor`, `implementador` y `audiencia` en los dieciséis archivos de reglas de `SDD/Devs/Rules/`, el master-prompt y la guía de usuario, y sustituirlas donde designen un rol de intervención. **No sustituir** cuando el término aparezca dentro de una cita textual del estado anterior, en una entrada de control de cambios ya escrita, o cuando «consumidor» designe un consumidor de mensajes o de colas en sentido técnico (`consumer group`, consumidor de un tópico), donde el término es correcto y no refiere a un actor. Reportar las sustituciones y las omisiones deliberadas.

### S2 — Redefinir la categoría 10 (`10-Examples`) con doble arista

Ampliar `Rules-Examples.md` conservando todo lo que ya define (tabla maestra de markdowns explicativos, pisos de cantidad de samples por tipo D8, matriz D8 → carpetas en `/samples`, correspondencia 1:1 entre cada `ejemplo-XX-*.md` y su carpeta ejecutable). Agregar:

**Arista A — Referencia de integración** (rol vigente, se conserva). El sample demuestra al integrador cómo incorporar el proyecto en una aplicación propia: caso realista, autocontenido, ejecutable siguiendo su README local.

**Arista B — Arnés de autovalidación** (rol nuevo). Cada sample declara, además de qué ilustra, **qué verifica**. Se incorpora a cada `ejemplo-XX-*.md` una sección obligatoria `Contrato de verificación` con estos campos:

| Campo | Contenido | Obligatorio |
|---|---|---|
| `verifica` | Lista de IDs de casos de uso (`CU-XX`) y user stories (`US-XX`) que el sample ejercita, tomados de 02 y 06 | Sí |
| `comando` | Comando exacto de ejecución, copy-paste, desde la raíz del repositorio | Sí |
| `precondiciones` | Estado mínimo requerido (servicios levantados, datos seed, variables de entorno) | Sí |
| `criterio_aceptacion` | Aserción evaluable, no prosa. Exit code esperado, respuesta HTTP con código y cuerpo, o snapshot de salida comparable | Sí |
| `evidencia` | Salida real obtenida en la última corrida, con fecha | Sí, una vez que existe código |

La aserción debe ser evaluable por una máquina sin interpretación. Es válido `curl -sf localhost:8080/health` → exit `0` y cuerpo `{"status":"healthy"}`. No es válido «verificar que el servicio responda correctamente».

**Momento de generación.** La categoría 10 se genera en dos pasadas:

- **Pasada de diseño** (pre-código, en la Fase G del orden nuevo): se redactan los markdowns explicativos y el contrato de verificación con `criterio_aceptacion` declarado, y el campo `evidencia` marcado como `No verificado — sin código`. Las carpetas de `/samples` quedan esqueletadas con su README local y su comando previsto.
- **Pasada de ejecución** (durante la codificación, ante cada incremento): el sample se implementa, se corre, y `evidencia` se completa con la salida real. Un sample cuyo `criterio_aceptacion` falla es un hallazgo, no un documento pendiente.

**Vinculación con el sensado de deriva.** El framework ya cuenta con un instrumento de control post-handoff, la matriz de sensado de deriva (`Matriz-Sensado-Deriva-v1.0.md`, categoría 08, gobernada por `Deriva-Rules.md`), que hoy cubre exclusivamente superficies de UX aprobadas en la maqueta (`SUP-XX`, `CMP-XX`, `EST-XX`, `NAV-XX`, `DM-XX`). Extender ese instrumento para que los contratos de verificación de la categoría 10 sean sondas adicionales de la matriz, cubriendo contratos y comportamiento además de superficies visuales. Asignarles prefijo de identificador propio: `VER-XX`.

**Anclaje de industria.** Esta arista corresponde a las prácticas de *Specification by Example* y *Executable Specification*, donde el ejemplo concreto opera simultáneamente como documentación y como criterio de aceptación automatizable.

### S3 — Redefinir la categoría 11 (`11-Documentacion`) como cuerpo documental de entrega

Reescribir `Rules-Documentacion.md` completo, respetando la estructura canónica que comparten todos los archivos de reglas del framework: §0 posición en la cadena · §1 especialidad asignada y variantes por D8 · §2 documentos que produce (tabla maestra + reglas de inclusión/exclusión por tipo) · §3 nomenclatura y vinculación · §4 estructura de redacción por artefacto · §5 preguntas guía para el subagente · §6 criterios de aceptación · §7 ejemplos genéricos · §8 prompt-snippet sugerido · §9 control de cambios.

#### S3.1 — Los dos ejes: rol de intervención y naturaleza del lector

La categoría 11 abandona el modelo de audiencia única, pero **no lo reemplaza por una lista de audiencias**. Lo reemplaza por dos ejes que se cruzan. Confundirlos es el error de diseño que hay que evitar: produce documentos duplicados, unos «para personas» y otros «para la IA», que inevitablemente divergen.

**Eje 1 — Rol de intervención.** Qué viene a hacer el lector con el sistema. Es independiente de la naturaleza de ese lector: un agente de IA que despliega necesita exactamente lo mismo que un operador humano que despliega. Este eje es el que organiza los artefactos en cuerpos.

| Cuerpo | Rol | Pregunta que responde |
|---|---|---|
| **Integrador** | Consume el proyecto desde otra aplicación o sistema, sin conocer su interior | «¿Cómo lo uso?» |
| **Mantenedor** | Retoma el desarrollo para conocerlo, intervenir el código y extenderlo | «¿Dónde está cada cosa y cómo agrego funcionalidad sin romper el diseño?» |
| **Operador** | Monta, despliega, verifica y sostiene el servicio en ejecución | «¿Cómo lo levanto, cómo sé que anda y qué hago cuando falla?» |

**Eje 2 — Naturaleza del lector.** Cómo lee, no qué busca. No genera cuerpos ni documentos propios: se resuelve dentro de cada artefacto, mediante el contrato de doble audiencia que estas reglas imponen.

| Naturaleza | Necesita | Cómo se lo sirve |
|---|---|---|
| **Agente humano** | Modelo mental, contexto, narrativa, orientación en primer contacto | Cara humana del contrato de doble audiencia: resumen ejecutivo, definiciones en primer uso, flujos narrados con caso concreto, diagramas, preguntas guía |
| **Agente de IA** | Datos extraíbles, ubicaciones exactas, aserciones evaluables | Cara agente del contrato: frontmatter YAML, identificadores estables con prefijo, anclas predecibles, rutas absolutas, comandos verbatim, criterios como aserción, bloques `entradas`/`salidas`/`validaciones` |

**Regla dura**: un mismo documento sirve a las dos naturalezas. Está prohibido producir versiones paralelas de un mismo contenido segmentadas por tipo de lector. Ante divergencia entre ambas caras, se corrige el documento; nunca se bifurca.

**Sobre por qué esta categoría carga con el peso narrativo.** El eje 2 explica la diferencia de tratamiento entre la categoría 11 y las categorías 00 a 09. La cadena de especificación ya está bien servida para el agente de IA, que la recorre acumulando contexto fase tras fase. Lo que ninguna categoría atiende es al agente humano que llega sin ese contexto acumulado y tiene que entender el desarrollo desde cero. La categoría 11 es la única del framework que asume ese lector como primario, y por eso es la única que adopta el estilo narrativo formativo. Aplicárselo a 00–09 sería inflarlas sin destinatario.

#### S3.2 — Artefactos de nivel solución

Se generan una sola vez para toda la solución.

| Archivo | Rol de intervención | Contenido |
|---|---|---|
| `Contrato-Agentes-v<X.Y>.md` | Todos | Artefacto versionado del cual se deriva el `AGENTS.md` de la raíz. Es el que sigue la convención de nomenclatura del framework y el que se audita; el `AGENTS.md` es su materialización en la ruta que las herramientas esperan |
| `README.md` de la categoría | Todos | Landing de la documentación. Su núcleo obligatorio es la **matriz de ruteo**: tabla `actor × intención → documento`, de modo que el lector no necesite conocer la estructura de carpetas para encontrar su camino. Es el único nombre de archivo que hace falta recordar |
| `Vision-General-Sistema-v<X.Y>.md` | Todos | Mapa del sistema legible en diez minutos: qué hace la solución, qué proyectos la componen y qué hace cada uno en una línea, cómo se comunican entre sí, dónde vive el código de cada uno. Incluye diagrama de contexto y diagrama de contenedores en Mermaid. Es el «plano» que permite formarse una idea del producto sin leer arquitectura |
| `Guia-Inicio-Rapido-v<X.Y>.md` | Mantenedor, Operador | Levantar la **solución completa** en una máquina limpia, con el orden de arranque derivado del grafo de dependencias del manifiesto. Objetivo duro: un solo comando, o la menor cantidad posible, con verificación al final que confirme que el sistema quedó operativo |
| `Guia-Despliegue-v<X.Y>.md` | Operador | Procedimiento de despliegue por topología: prerrequisitos, orden de arranque entre proyectos, cómo se resuelven entre sí, configuración por ambiente, verificación paso a paso y rollback |
| `Bitacora-Eventualidades-v<X.Y>.md` | Operador, Mantenedor | Registro de las situaciones no previstas que aparecieron durante la construcción, el despliegue y la operación, con síntoma, causa, resolución e intentos descartados. Cada entrada se identifica `EVE-XX` y se triaja hacia un documento permanente. Definido en detalle en S4 |
| `AGENTS.md` (emitido en la **raíz del repositorio destino**) | Agentes de IA | Contrato de contexto para agentes: cómo se construye el proyecto, cómo se corren los tests, convenciones de código, comandos de validación, límites de intervención, y punteros a los documentos de 11 por intención |

Sobre `Vision-General-Sistema`: **no duplica la categoría 05**. La distinción se declara explícitamente en §0 del archivo de reglas. La 05 documenta la arquitectura *como decisión de diseño*, con vistas formales, ADRs y NFR, y se dirige a quien participó del diseño o lo continúa dentro de la cadena de especificación. La 11 documenta el sistema *como hecho consumado*, dirigida a alguien que llega de afuera sin ese recorrido y necesita orientarse. Una responde «por qué se decidió así»; la otra, «qué es esto y por dónde entro».

Sobre `AGENTS.md`: es un formato abierto y establecido para instruir agentes de codificación, gobernado bajo la Agentic AI Foundation de la Linux Foundation, que los agentes cargan automáticamente al iniciar sesión en un repositorio. Se lo adopta tal cual, sin renombrarlo ni versionarlo con el sufijo del framework, porque su valor depende de que las herramientas lo encuentren en la ruta convencional. El artefacto versionado que lo gobierna, y del cual se deriva, sí sigue la convención: `Contrato-Agentes-v<X.Y>.md`, dentro de la carpeta de la categoría.

#### S3.3 — Artefactos de nivel proyecto

**Cuerpo integrador** (migra desde la categoría 10 actual, conservando su estructura Diátaxis y su parametrización de nombres):

| Archivo | Cuadrante Diátaxis |
|---|---|
| `Conceptos-Fundamentales-v<X.Y>.md` | Explanation |
| `Guia-Onboarding-Developer-v<X.Y>.md` | Tutorial |
| `guia-integracion-<sistema-objetivo>-v<X.Y>.md` | How-to |
| `Referencia-Api-v<X.Y>.md` | Reference |
| `Referencia-Cli-v<X.Y>.md` | Reference |
| `Troubleshooting-v<X.Y>.md` | How-to orientado a diagnóstico |
| `Glosario-Tecnico-v<X.Y>.md` | Reference |

Se conservan las dos correcciones que el archivo vigente ya impone y que siguen siendo válidas: sufijo `-v<X.Y>.md` uniforme y obligatorio en todos los artefactos, y prohibición de hardcodear un sistema comercial concreto en el nombre de la guía de integración, que se parametriza con un slug genérico.

**Cuerpo mantenedor** (nuevo):

| Archivo | Contenido |
|---|---|
| `Recorrido-Codigo-v<X.Y>.md` | El puente entre la arquitectura y el repositorio real. Mapea cada componente declarado en 05 contra su ubicación exacta en el árbol de archivos: «la capa Application del ADR-002 vive en `src/<Proyecto>/Application/`». Recorre el flujo principal del sistema nombrando los archivos que se atraviesan en orden. Sin este documento, retomar un proyecto obliga a reconstruir el mapa leyendo código |
| `Guia-Contribucion-v<X.Y>.md` | Setup del entorno de desarrollo desde cero, cómo correr los tests y qué debería devolver, convenciones de código y de commits, y —el núcleo del documento— cómo agregar una funcionalidad de punta a punta: qué archivos se tocan, en qué orden, qué se actualiza en la documentación y qué verifica que quedó bien |
| `Guia-Extension-v<X.Y>.md` | Puntos de extensión publicados, contrato de cada uno y ejemplo de registro. Solo cuando el proyecto declara extensibilidad en 05 |

**Cuerpo operador** (nuevo):

| Archivo | Contenido |
|---|---|
| `Guia-Contenedor-v<X.Y>.md` | Contrato de ejecución del servicio: tabla de variables de entorno con tipo, default, obligatoriedad y efecto; puertos expuestos; volúmenes y su propósito; healthcheck con su endpoint y respuesta esperada; dependencias de arranque; límites de recursos sugeridos. Responde la pregunta «quiero montar este servicio en un contenedor, ¿qué necesito saber?» |
| `Runbook-Operacion-v<X.Y>.md` | Procedimientos de operación: arrancar, parar, reiniciar, verificar salud, leer logs y qué patrón buscar, métricas relevantes y sus umbrales. Incluye los incidentes conocidos con identificador `OPS-XX`, cada uno con síntoma, diagnóstico paso a paso y resolución |

#### S3.4 — Reglas de inclusión y exclusión por tipo D8

Sustituir la tabla de gating vigente por esta, de granularidad por cuerpo:

| Tipo D8 | Cuerpo integrador | Cuerpo mantenedor | Cuerpo operador |
|---|---|---|---|
| `library` | Obligatorio | **Obligatorio** | No aplica (no se despliega como servicio) |
| `rest-api` | Obligatorio | **Obligatorio** | Obligatorio |
| `cli-tool` | Obligatorio | **Obligatorio** | Opcional (`Guia-Contenedor` solo si se distribuye containerizado) |
| `web-microservices` | Obligatorio si expone APIs públicas | **Obligatorio** | Obligatorio |
| `web-monolith` | Opcional (solo si expone API externa) | **Obligatorio** | Obligatorio |
| `worker-service` | Opcional | **Obligatorio** | Obligatorio |
| `desktop-app` | Opcional (solo si publica SDK de plugins) | **Obligatorio** | No aplica; se reemplaza por guía de instalación y actualización |
| `mobile-app-maui` | Opcional (solo si publica SDK) | **Obligatorio** | No aplica; la distribución por store ya vive en 09 |

El cambio de fondo respecto del gating vigente: **el cuerpo mantenedor es obligatorio para los ocho tipos, sin excepción**. Todo proyecto va a ser retomado por alguien, incluso aquellos sin integrador externo, y ese alguien puede no haber participado de ninguna fase de su especificación. La categoría 11 deja de ser opcional para cuatro de los ocho tipos y pasa a existir siempre; lo que varía es qué cuerpos se materializan dentro de ella.

Cuando un cuerpo se omite por gating, la decisión se registra en `Decisiones-Proyecto-v1.0.md`. Cuando el equipo omite un cuerpo que el gating declara obligatorio, se requiere ADR con justificación.

#### S3.5 — Fronteras con las categorías vecinas

Declarar estas fronteras en §0 de `Rules-Documentacion.md`. Sin ellas, los artefactos nuevos se solapan con reglas existentes.

| Frontera | Categoría vecina documenta | Categoría 11 documenta |
|---|---|---|
| **09 DevOps** | La *política*: qué ambientes existen, cómo se promociona entre ellos, cómo se firma y publica el artefacto. Lector: quien participa del diseño, dentro de la cadena de especificación | El *procedimiento verificado*: qué comando corro, en qué orden, qué tiene que responder. Lector: quien llega de afuera, con el sistema ya construido |
| **05 Arquitectura** | La arquitectura *como decisión*: vistas formales, ADRs, contratos, NFR, alternativas descartadas | El sistema *como hecho*: qué es, qué componentes tiene, dónde vive cada uno en el repositorio |
| **08 Calidad y Pruebas** | La estrategia de testing, los casos de prueba y la matriz de sensado de deriva | Cita esa estrategia para explicarle al mantenedor cómo correr los tests. **No la redefine** |
| **10 Examples** | El ejemplo ejecutable y su contrato de verificación | Explica, contextualiza y enlaza esos ejemplos. **No duplica su código** |
| **03 UX-UI-DX** | El diseño de la experiencia y las superficies | Nada. La categoría 11 no documenta al usuario final no técnico; ese hueco se declara explícitamente como fuera de alcance del framework |

#### S3.6 — Identificadores estables

Fijar los prefijos que la categoría 11 introduce, para que las trazas y los enlaces apunten al identificador y no a la ruta:

- `OPS-XX` — incidente operativo en el runbook.
- `EXT-XX` — punto de extensión en la guía de extensión.
- `ISSUE-XX` — entrada de troubleshooting del integrador (ya existente, se conserva).
- `VER-XX` — sonda de verificación aportada por la categoría 10 (definido en S2).
- `EVE-XX` — eventualidad capturada en la bitácora (definido en S4).

### S4 — Incorporar el modelo de documentación viva en tres momentos

La categoría 11 deja de generarse en una sola pasada al cierre. Se estructura en tres momentos, que hay que reflejar tanto en `Rules-Documentacion.md` como en el orden de fases del `Master-Prompt.md`.

**Momento 1 — Plan documental (pre-código).** Una vez derivado y confirmado el `SOLUTION-MANIFEST`, se conoce la composición de la solución: qué proyectos hay, de qué tipo es cada uno y qué rol cumple. Con eso alcanza para determinar **qué documentos va a tener cada proyecto**, sin redactar contenido. El entregable de este momento es el índice del cuerpo documental: la lista de artefactos por proyecto, su rol de intervención, y el estado inicial `Planificado` de cada uno. Se genera junto con el plan de generación que el orquestador presenta para aprobación, de modo que el usuario vea desde el principio qué documentación va a existir al final.

**Momento 2 — Actualización incremental (durante la codificación).** Cada vez que la construcción alcanza un incremento funcional demostrable, la categoría 11 se actualiza al estado real del sistema. Este es el momento con mayor valor de los tres, porque convierte a la documentación en un instrumento de control y no en una obligación de cierre: el desarrollador revisa el producto desde los tres ángulos —¿se entiende qué hace?, ¿se puede intervenir el código?, ¿el despliegue resulta razonable o quedó complejo?— cuando todavía hay margen para corregir el diseño. Un procedimiento de despliegue que al documentarse resulta enredado es una señal de arquitectura, no un problema de redacción.

Cada actualización incremental toca únicamente los documentos afectados por el incremento, registra la fecha de la revisión y actualiza el estado del artefacto. Un documento no revisado desde hace N incrementos se marca como potencialmente desactualizado en el README de la categoría.

**Momento 3 — Consolidación de cierre.** Pasada final que verifica el cuerpo completo: ejecuta todo comando documentado, confirma que las aserciones se cumplen, revisa huecos y contradicciones entre documentos, y emite la versión definitiva del `AGENTS.md`.

**Anclaje de industria del modelo.** Los tres momentos corresponden a las prácticas establecidas de *Living Documentation* —documentación que evoluciona junto al sistema que describe—, *Docs as Code* —la documentación versionada y entregada por el mismo flujo que el código, de modo que no pueda derivar de él— y *Continuous Documentation* —la verificación continua y automatizada de la documentación contra el estado real del código—, con la documentación incorporada a la *Definition of Done* del incremento: una funcionalidad no está terminada hasta que su documentación está actualizada.

---

Los tres momentos se apoyan en cuatro mecanismos transversales, que hay que definir en `Rules-Documentacion.md` y reflejar en el `Master-Prompt.md`.

#### S4.1 — Cadencia de actualización

«Incremento funcional demostrable» no alcanza como disparador: hay que anclarlo a cortes que el framework ya tiene. Definir tres disparadores, en orden de precedencia:

| Disparador | Alcance de la actualización |
|---|---|
| **Cierre de sprint** (corte por defecto; el framework ya modela sprints en la categoría 07) | Todos los documentos de 11 tocados por los ítems del sprint. Es el corte principal |
| **Cierre de incremento demostrable**, cuando el equipo no trabaja por sprints | Equivalente al anterior, con el incremento como unidad |
| **Cambio que altera un contrato público, un procedimiento de despliegue o una ruta de código citada** | Actualización inmediata, sin esperar el corte. Un documento que apunta a una ruta inexistente es peor que un documento ausente |

La actualización de la categoría 11 forma parte de la **Definition of Done** del sprint o del incremento: el corte no se declara cerrado con documentos afectados sin revisar. Registrar esta condición explícitamente en `Rules-Plan-Sprint.md`, dentro de su definición de Done, como única modificación adicional permitida sobre esa regla.

#### S4.2 — Ensayo de entrega (validación de utilidad)

Actualizar la documentación no prueba que sirva. Al cierre de cada Momento 2 —o al menos en los cortes que el usuario elija— se corre un **ensayo de entrega**: se toma la documentación en su estado actual y se ejecuta con ella una tarea real, sin ayuda externa.

**Quién lo corre.** El ensayo es un **corte de confirmación humana**, no una tarea que el orquestador pueda auto-adjudicarse. El agente que ejecuta la Fase I no puede declararlo aprobado por sí mismo, por la misma razón por la que no puede aprobar su propia maqueta: conoce el sistema porque acaba de documentarlo, y esa contaminación anula la prueba. Definir dos niveles y no confundirlos:

- **Ensayo automatizado** (lo corre el agente, en cada Fase I): ejecuta los comandos documentados en un entorno limpio y verifica las aserciones. Detecta comandos rotos, rutas inexistentes y prerrequisitos faltantes. Es condición necesaria, no suficiente.
- **Ensayo humano** (lo corre el usuario, en los cortes que elija y obligatoriamente en la Fase J): una persona ejecuta el guion completo. Es lo único que detecta lo que la documentación no dice pero hace falta saber. Su resultado es un gate: sin ensayo humano aprobado no se cierra la Fase J.

El framework ya tiene el patrón exacto para esto en la Fase B2: la maqueta existe porque leer una especificación de UX y decidir si es lo que se quería resulta caro y poco confiable, así que se materializa algo navegable y se lo recorre. **El ensayo de entrega es a la documentación lo que la validación de maqueta es al diseño.** Misma lógica, mismo corte de confirmación humana.

Definir en las reglas al menos un guion de ensayo por rol de intervención:

| Rol | Guion del ensayo | Qué falla revela |
|---|---|---|
| Operador | Desplegar un servicio concreto desde cero, en una máquina o entorno limpio, siguiendo únicamente `Guia-Despliegue` y `Guia-Contenedor` | Prerrequisitos no declarados, pasos implícitos, orden de arranque mal documentado |
| Mantenedor | Ubicar una porción de código concreta e introducir una mejora acotada, siguiendo únicamente `Recorrido-Codigo` y `Guia-Contribucion` | Puentes rotos entre arquitectura y árbol de archivos, convenciones tácitas, tests que no se sabe cómo correr |
| Integrador | Consumir una capacidad del sistema desde un cliente nuevo, siguiendo únicamente el cuerpo integrador | Referencia incompleta, ejemplos que no compilan, autenticación no explicada |

**Regla de oro del ensayo**: durante la corrida solo se puede leer la documentación. No se le pregunta al equipo, no se lee código fuera de lo que la documentación indica leer, no se usa conocimiento previo del proyecto. **El momento en que hay que salirse de la documentación es, exactamente, el hallazgo.**

Cada ensayo registra: si la tarea se completó, cuánto tardó, en qué paso se trabó y qué hubo que averiguar por fuera. Cada trabada se convierte en un hallazgo con destino asignado —qué documento y qué sección lo tiene que absorber—, y se resuelve antes de cerrar el corte. Un ensayo que no se completa es un hallazgo P0 de la Fase I.

El resultado del ensayo se registra en el informe de audit de la fase, en `SDD/Docs/Audit/`, reutilizando la maquinaria de auditoría que el framework ya tiene.

#### S4.3 — Bitácora de eventualidades (capitalización de experiencia)

Durante la construcción, el despliegue y las primeras corridas aparecen situaciones que ningún documento de diseño podía anticipar, porque solo se manifiestan al ejecutar el sistema en un entorno real. Hoy esas situaciones se resuelven una vez, quedan en la memoria de quien las resolvió y se pierden. El siguiente operador las vuelve a sufrir idénticas.

El caso testigo, que conviene usar como ejemplo en las reglas: un servicio que se comunica con un dispositivo físico conectado por USB. Al containerizarlo aparece que necesita *passthrough* del dispositivo del host, con su regla de permisos y su ruta. Ninguna vista de arquitectura lo predijo, ninguna decisión de diseño lo registra, y es la primera cosa con la que se choca quien lo despliega. Es exactamente el conocimiento que hay que capitalizar.

Incorporar a la categoría 11 un artefacto de captura, `Bitacora-Eventualidades-v<X.Y>.md`, de **nivel solución**, con una entrada por eventualidad identificada como `EVE-XX` y estos campos:

| Campo | Contenido |
|---|---|
| `id` | `EVE-XX` |
| `ambito` | `solución` o el `Nombre-Proyecto` afectado |
| `fecha` | Cuándo se detectó |
| `momento` | Construcción, despliegue, operación o ensayo de entrega |
| `sintoma` | Qué se observó, en términos verificables |
| `causa` | Qué la provocaba realmente, no la hipótesis inicial |
| `resolucion` | Qué se hizo, con el comando o la configuración exacta |
| `intentos_descartados` | Qué se probó y no funcionó. Es lo que el documento permanente nunca va a conservar y lo que más tiempo ahorra al siguiente |
| `destino` | Documento y sección que absorbe la eventualidad de forma permanente |

**Triaje obligatorio.** Cada eventualidad se clasifica y se propaga a un documento permanente. La bitácora es un buffer de captura, no el destino final:

| Naturaleza de la eventualidad | Destino permanente |
|---|---|
| Requisito del host o del entorno no declarado (acceso a un dispositivo, permiso, módulo del kernel, límite de recursos, variable no documentada) | `Guia-Contenedor` → prerrequisitos y dispositivos requeridos |
| Falla reproducible con síntoma observable en ejecución | `Runbook-Operacion` → nueva entrada `OPS-XX` |
| Falla que golpea a quien integra el proyecto desde afuera | `Troubleshooting` → nueva entrada `ISSUE-XX` |
| Paso del despliegue que resultó no evidente | `Guia-Despliegue` o `Guia-Inicio-Rapido` |
| Reveló un problema de diseño, no de documentación | ADR en la categoría 05, más escalamiento al usuario |
| No reproducible o caso único sin valor para terceros | Queda solo en la bitácora, marcada `No absorbida`, con el motivo |

**Regla dura**: ninguna eventualidad se cierra sin destino asignado. «Sin destino» no es un estado válido de cierre; si no aplica ninguna categoría, se marca explícitamente `No absorbida` con justificación. El triaje se ejecuta en cada corte del Momento 2, junto con la actualización incremental.

**Distinción con el sensado de deriva.** No confundir ambos instrumentos, y declararlo en las reglas. La deriva mide divergencia entre lo construido y una línea de base aprobada: algo se apartó de lo acordado. Una eventualidad es un hecho del entorno que nadie había previsto: no hay línea de base de la cual apartarse, hay conocimiento nuevo que capturar. Un mismo hallazgo puede alimentar los dos instrumentos, pero se registran por separado.

**Anclaje de industria.** Corresponde a la práctica de *postmortem sin culpa* de la disciplina SRE, donde cada incidente produce un registro estructurado de síntoma, causa raíz, resolución y acciones derivadas, con el foco puesto en que la organización aprenda y no en atribuir responsabilidad. La sección de *known issues* de un producto es el destino publicado de ese mismo material.

#### S4.4 — Impacto en el orden de fases

Reflejar en `Master-Prompt.md`:

```
Fase validación de intake        → sin cambios
Fase A  (solución)               → sin cambios
Fase B  / B2 / C / D / E         → sin cambios
Fase F  (por proyecto)           → 09-Devops + audit F        (se le quita la categoría 10 vieja)
Fase G  (por proyecto)           → 10-Examples, pasada de diseño + audit G
Fase H  (consolidación)          → vista de solución + pipeline + README raíz
                                   + plan documental de 11 (Momento 1) + audit final
Paso 6  (humano)                 → handoff a codificación
─────────────────── a partir de acá el sistema se construye ───────────────────
Fase I  (por incremento, re-ejecutable) → 10 pasada de ejecución
                                          + 11 actualización incremental (Momento 2)
                                          + AGENTS.md emitido/refrescado
                                          + ensayo automatizado + audit I acotado al incremento
Fase J  (una vez, al cierre)            → 11 consolidación (Momento 3)
                                          + AGENTS.md definitivo
                                          + ensayo humano (gate) + audit final de entrega
```

**Sobre el momento de emisión del `AGENTS.md`.** Se emite en la **primera** corrida de la Fase I y se refresca en cada una, no solo al cierre. La razón es operativa: la Fase I es exactamente el tramo donde los agentes de IA codifican, despliegan y verifican, así que es cuando más necesitan ese contrato de contexto. Reservarlo para la Fase J lo dejaría disponible recién cuando ya no hace falta.

Definir la **precondición dura de la Fase I**: no puede ejecutarse sobre un repositorio sin código. Requiere que exista código fuente, que `/samples` tenga al menos un sample implementado y que los tests corran. Si la precondición no se cumple, el orquestador se detiene y lo informa en lugar de generar documentación sobre un sistema inexistente.

Definir el **criterio de re-ejecución**: qué se regenera y qué se preserva en cada corrida de Fase I. Las correcciones manuales del usuario sobre documentos de 11 no se pisan; el orquestador relee, enumera las diferencias, informa cómo las interpretó y espera confirmación antes de propagarlas —mismo patrón que ya rige para las correcciones manuales de maqueta.

**Criterios de auditoría de las Fases I y J.** Los hallazgos P0 propios de estas fases, que hay que declarar:

- Un comando documentado no ejecuta, o falla.
- Un criterio de aceptación está redactado como prosa en lugar de aserción evaluable.
- Un documento afirma algo que contradice el estado real del código.
- Una ruta de archivo citada no existe en el repositorio.
- Un artefacto declarado obligatorio por el gating de S3.4 está ausente sin ADR que lo justifique.
- **Un ensayo de entrega no se completó**, o requirió salirse de la documentación para avanzar.
- **Una eventualidad quedó cerrada sin destino asignado**, o abierta desde hace más de un corte sin triaje.
- El corte de sprint o de incremento se declaró cerrado con documentos de 11 afectados y sin revisar.

### S5 — Embeber las reglas de redacción en el subagente AG-11

Incorporar a `Rules-Documentacion.md` —en §1 especialidad y en §4 estructura de redacción— el cuerpo de reglas de redacción que figura en la sección **Reglas** de este prompt, bajo los títulos «Estilo narrativo formativo», «Doble audiencia», «Voz narrativa» y «Formato markdown». El archivo de reglas debe quedar autosuficiente: el subagente que lo lea no debe necesitar ningún otro archivo para saber cómo redactar.

Se descarta expresamente todo lo relativo a **indexado de conocimiento**: no forma parte de este cuerpo documental y no debe incorporarse.

### S6 — Agregar tabla de contenido a las categorías 00 a 09

Las categorías 00 a 09 mantienen su carga documental actual. Son leídas principalmente por la IA durante las etapas de especificación y de codificación, y su densidad actual es adecuada a ese uso: no corresponde aplicarles el estilo narrativo formativo de la categoría 11.

El único ajuste es de navegabilidad, para mejorar el seguimiento y la consulta por parte de lectores humanos. En cada uno de los diez archivos de reglas `Rules-Contexto.md`, `Rules-Necesidades-Negocio.md`, `Rules-Especificacion-Funcional.md`, `Rules-UX-UI-DX.md`, `Rules-Prompts-AI.md`, `Rules-Arquitectura-Tecnica.md`, `Rules-Backlog-Tecnico.md`, `Rules-Plan-Sprint.md`, `Rules-Calidad-Y-Pruebas.md` y `Rules-Devops.md`, agregar en §4 (estructura de redacción) y en §6 (criterios de aceptación) la exigencia siguiente:

- Todo documento generado que supere las tres secciones de primer nivel incluye una **tabla de contenido** inmediatamente después de la cabecera de metadatos, con enlaces ancla a cada sección de primer y segundo nivel.
- La tabla de contenido no cuenta como sección de contenido ni altera la estructura obligatoria del documento.
- Los documentos breves —fichas de una sola sección, entradas de índice— quedan exceptuados.

No introducir ningún otro cambio en estas diez reglas. En particular, no aumentar su carga narrativa ni agregarles artefactos.

### S7 — Actualizar la guía de usuario

Ajustar `/IA/IA.SDD/SDD/Guides/SDD-User-Guide.md` para reflejar el estado nuevo:

- §4: renumerar las categorías en la descripción de fases, incorporar las Fases I y J y agregar el paso de usuario correspondiente al ciclo incremental posterior al handoff.
- §4.7: hoy el documento afirma que tras el handoff se sale del alcance de SDD. Reformular: el alcance se extiende al ciclo incremental de documentación viva.
- §5: corregir las referencias a categorías en los cuatro casos aplicados.
- §6: agregar entradas de FAQ nuevas, continuando la numeración vigente a partir de `F-24`, que cubran al menos: por qué se intercambiaron las categorías; cuándo se corre la Fase I y con qué cadencia; qué pasa si se quiere saltear la documentación incremental; cómo se relaciona `AGENTS.md` con el resto del cuerpo; **cómo se corre un ensayo de entrega y qué hacer cuando no se completa**; y **cómo se registra y se triaja una eventualidad**, con el caso del dispositivo USB como ejemplo desarrollado de punta a punta, desde el síntoma hasta su absorción en `Guia-Contenedor` y en el runbook.
- §10: actualizar el mapa de carpetas.

### S9 — Convertir el `README.md` raíz en el punto de entrada del framework

El `README.md` de `/IA/IA.SDD/` tiene hoy siete líneas: un título y tres enlaces. Cuando se lo cita desde otro prompt, un agente no puede ubicarse: no sabe qué es SDD, qué piezas lo componen, ni a qué archivo ir según lo que le están pidiendo. La consecuencia práctica es que hay que citar varios archivos a mano en cada prompt para darle contexto.

Reescribirlo como **superficie de entrada única**, optimizada para que un agente que lo lee como primer y a veces único archivo del repositorio pueda orientarse y decidir a dónde ir. Contenido mínimo:

1. **Qué es SDD en tres a cinco oraciones**: qué problema resuelve, cuál es su unidad de trabajo (solución con N proyectos tipados D8), y qué produce (documentación viva por categorías, generada por un orquestador con auditoría entre fases).
2. **Modelo de tres repositorios**, con la tabla de rol, escritura y contenido. Es lo que evita que un agente escriba en el lugar equivocado.
3. **Anatomía del repositorio**: qué hay en cada carpeta de primer y segundo nivel, una línea por carpeta, con su ruta exacta.
4. **Matriz de ruteo por intención**, que es el núcleo del documento. Tabla `«vengo a hacer X» → leé este archivo`, cubriendo al menos: entender qué es SDD · arrancar una solución nueva · consultar qué genera una categoría · modificar el comportamiento de una categoría · extender el framework con algo nuevo · entender por qué el framework es como es · saber qué reglas rigen la redacción de un documento generado · encontrar el orquestador · encontrar las plantillas de intake.
5. **Mapa de las doce categorías** con su carpeta, su archivo de reglas y su nivel (solución o proyecto). Es la tabla que hoy obliga a abrir la guía de usuario para consultarla.
6. **Invariantes D1–D8 enunciadas**, no solo nombradas, para que un agente sepa qué no puede romper sin abrir otro archivo.
7. **Reglas de intervención sobre el framework**: qué se puede modificar, qué requiere subir versión, qué exige control de cambios y dónde vive la guía de extensibilidad.

Criterios duros: sin URLs externas nuevas —la existente se conserva—, todos los enlaces relativos internos, y **la prueba de aceptación es que un agente con solo este archivo en contexto pueda responder correctamente a qué archivo ir para cinco intenciones distintas tomadas de la matriz de ruteo**. El documento se optimiza para esa prueba, no para la lectura lineal.

### S10 — Escribir la guía de desarrollo del framework

El archivo `/IA/IA.SDD/SDD/Guides/SDD-Development-Guide.md` existe pero está vacío. Escribirlo completo.

**Delimitación de alcance — leer antes de empezar.** El marco teórico del framework **ya está escrito** en `SDD/Devs/Guides/Marco-Teorico-SDD-v1.0.md`, con ~151 KB y catorce secciones que cubren fundamentos de SDD, metodología, catálogo de especialidades, marco ágil, técnicas de descomposición, estilos arquitectónicos por tipo D8, calidad, DevOps, prompting, anti-patrones, glosario y bibliografía. **Está prohibido reescribirlo, resumirlo extensamente o duplicarlo.** La guía de desarrollo lo referencia por ruta relativa interna y sigue.

La distinción de audiencia entre las cuatro guías, que hay que declarar en el §1 de la guía nueva:

| Guía | Lector | Responde |
|---|---|---|
| `SDD-Getting-Started-Guide.md` | Quien arranca por primera vez | «¿Cómo pongo esto a andar hoy?» |
| `SDD-User-Guide.md` | Quien **usa** el framework en una solución real | «¿Cómo lo aplico paso a paso?» |
| `Marco-Teorico-SDD-v1.0.md` | Quien quiere entender los fundamentos | «¿Por qué está diseñado así?» |
| `SDD-Development-Guide.md` (nueva) | Quien **desarrolla y extiende el framework mismo** | «¿Cómo está construido por dentro y cómo lo modifico sin romperlo?» |

Es una audiencia que hoy no tiene documento: el mantenedor del framework, no el de una solución.

Estructura a producir:

**Parte I — Anatomía del framework.** Despiece completo de la estructura: qué es cada carpeta y cada tipo de archivo, qué responsabilidad tiene, quién lo lee y quién lo escribe. Cubrir `SDD/Devs/Rules/`, `SDD/Devs/Orchestrator/`, `SDD/Devs/Intake/`, `SDD/Devs/Guides/`, `SDD/Devs/References/`, `SDD/Devs/Modelos-UX-UI/`, `SDD/Devs/Bootstrap/`, `SDD/Guides/`, `PROMPTS/` y `Templates/`. Con diagrama Mermaid de la estructura y de quién depende de quién.

**Parte II — Los contratos internos.** Las piezas del framework se comunican por contratos implícitos que hoy no están escritos en ningún lado, y que son la causa más probable de que una extensión rompa algo. Documentar al menos: la estructura canónica de nueve secciones que comparte todo archivo de reglas y por qué es rígida; cómo el orquestador decide qué categoría generar a partir del `project_type` del manifiesto; cómo se propaga el gating de doble granularidad —existe la categoría, y dentro qué artefactos—; cómo se encadena la trazabilidad entre categorías; qué espera el auditor de cada fase; y cómo se derivan los flags del intake.

**Parte III — Metodología de extensibilidad.** El núcleo de la guía y la razón de escribirla. Un procedimiento por eje de extensión, cada uno con la misma estructura interna: qué se está agregando, qué archivos hay que tocar y en qué orden, qué invariantes no se pueden romper, cómo se verifica que la extensión no rompió nada, y un ejemplo trabajado de punta a punta. Los ejes son, como mínimo:

- Agregar una categoría documental nueva.
- Agregar o modificar un artefacto dentro de una categoría existente.
- Agregar una variante de especialidad a una categoría.
- Agregar una fase al orquestador.
- Modificar el gating por tipo D8 de una categoría.
- Agregar un modelo UX-UI al catálogo.
- Modificar una invariante global D1–D8, con la advertencia de que es el cambio de mayor impacto y por qué.

Sobre el conjunto D8: es cerrado por diseño. La guía debe explicar **por qué** está cerrado y qué habría que rehacer si alguna vez se ampliara, sin habilitar la ampliación.

**Parte IV — Criterios y preguntas guía.** Para cada eje de extensión, las preguntas que el desarrollador tiene que poder responder antes de tocar nada: ¿esto es una categoría nueva o un artefacto dentro de una existente? ¿corresponde al framework o al proyecto que lo usa? ¿qué audiencia lo lee? ¿qué categoría vecina se solapa y dónde está la frontera? ¿qué se rompe si el gating es incorrecto? El objetivo declarado es que el lector forme criterio, no que siga una receta.

**Parte V — Anti-patrones de extensión.** Los errores conocidos al modificar el framework, con su síntoma, su consecuencia y su corrección. Como mínimo: duplicar contenido entre categorías en lugar de declarar la frontera; hardcodear un stack comercial concreto en un nombre de archivo; agregar un artefacto sin declarar su gating por D8; romper la estructura canónica de nueve secciones; introducir una referencia a un repositorio externo; y exigir en una fase pre-código una verificación que requiere código, que es precisamente el defecto que esta intervención corrige.

**Parte VI — Procedimiento de cambio.** Cómo se versiona un archivo de reglas, cuándo corresponde subir mayor o menor, qué se registra en el control de cambios de §9, cómo se verifica la coherencia después de un cambio siguiendo el patrón de `Coherencia-Auditoria-Marco-v1.0.md`, y qué hacer cuando un cambio obliga a regenerar documentación ya emitida en repositorios destino.

**Reglas de redacción de esta guía**: aplican las mismas que la sección «Reglas de redacción a embeber» de este prompt —estilo narrativo formativo, doble audiencia, voz narrativa, formato markdown—. Definiciones, explicaciones, ejemplos concretos, diagramas Mermaid, secciones jerárquicas, tabla de contenido, y preguntas guía al cierre de las secciones densas. Es el mismo estándar que se le exige a la categoría 11, aplicado al framework en lugar de a una solución.

### S11 — Entregable de cierre

Generar un informe de la intervención en `/IA/IA.SDD.Documentacion/PROMPTs/Feactures/09-Editar-Agent-Rules-Documentacion-Examples-final/Informe-Intervencion-v1.0.md` con:

- Tabla de archivos creados, renombrados, modificados y eliminados, con su cambio de versión en el control de cambios de cada uno.
- Resultado del barrido de propagación de S1.5 y de la normalización de vocabulario de S1.7: cadenas buscadas, ocurrencias encontradas, corregidas y deliberadamente no tocadas con su motivo.
- **Resultado de la verificación de autosuficiencia** exigida en Restricciones: las once cadenas buscadas con su conteo, y el listado de URLs presentes en `/IA/IA.SDD/` distinguiendo las preexistentes de las eventualmente introducidas.
- Inconsistencias detectadas durante el trabajo que excedan el alcance de este prompt, diferenciando hechos de interpretaciones.
- Decisiones que quedaron abiertas y requieren definición del responsable del framework.

---

## Etapas de ejecución y control de deriva

La intervención es grande: toca más de veinte archivos, reescribe uno por completo, crea dos documentos extensos desde cero y modifica el orquestador. **Ejecutarla de corrido es la forma más segura de que derive.** Se segmenta en ocho etapas, con dos mecanismos de control distintos que no hay que confundir.

### Principio de segmentación

Cada etapa cumple tres condiciones, y una etapa que no las cumpla está mal cortada:

1. **Deja el framework en estado consistente.** Si la intervención se abandona al terminar cualquier etapa, lo que quedó es coherente y utilizable, aunque esté incompleto.
2. **Es verificable por sí sola**, sin depender de que se haya hecho una etapa posterior.
3. **No define dos veces el mismo artefacto.** Cada archivo tiene una etapa dueña; las demás pueden leerlo, no reescribirlo.

### Las ocho etapas

| Etapa | Solicitudes | Qué produce | Por qué va acá |
|---|---|---|---|
| **E0 — Reconocimiento** | ninguna (no escribe) | Inventario real del repositorio, verificación de los supuestos del Contexto contra el estado de los archivos, y plan de intervención presentado para confirmación | El Contexto de este prompt es una descripción, no una verdad verificada. Si algo no coincide, se reporta antes de tocar nada |
| **E1 — Renombrado estructural** | S1 | Intercambio 10↔11, renombre del archivo de reglas, carpetas target, AG-10/AG-11, barrido de propagación y normalización de vocabulario | Es mecánico y reversible. Va primero para que todas las etapas siguientes escriban ya sobre la numeración definitiva |
| **E2 — Categoría 10** | S2 | `Rules-Examples.md` ampliado con la doble arista, contrato de verificación, `VER-XX`, y extensión del sensado de deriva | Es acotada y no depende de la 11 |
| **E3 — Categoría 11** | S3, S5 | `Rules-Documentacion.md` completo, con las reglas de redacción embebidas | La pieza más grande. S5 va junta porque las reglas de redacción son secciones de ese mismo archivo: separarlas obligaría a reabrirlo |
| **E4 — Documentación viva** | S4 | Tres momentos, cadencia, ensayo de entrega, bitácora de eventualidades, y Fases I/J en el `Master-Prompt.md` | Requiere que existan los artefactos de E2 y E3 para poder referenciarlos |
| **E5 — Navegabilidad de 00 a 09** | S6 | Tabla de contenido en las diez reglas de las categorías conservadas | Independiente de todo lo anterior. Se puede correr en cualquier momento posterior a E1 |
| **E6 — Superficie de entrada** | S9, S10 | `README.md` raíz reescrito y `SDD-Development-Guide.md` completo | **Van al final por una razón de fondo: describen el framework, y el framework cambia en E1 a E5.** Escribirlos antes sería documentar un estado que va a dejar de existir |
| **E7 — Cierre** | S7, S11 | Guía de usuario actualizada e informe de intervención | Consolidación y trazabilidad final |

### Control de coherencia A — de diseño de las etapas

Antes de arrancar E1, y como parte del plan que E0 presenta para confirmación, verificar que la segmentación misma es coherente. Comprobar y reportar:

- Ningún archivo es escrito por dos etapas distintas. Si aparece uno, se reasigna a una sola dueña antes de empezar.
- Ninguna etapa referencia un artefacto que solo va a existir en una etapa posterior. Si aparece, se reordenan las etapas o se parte el artefacto.
- Cada etapa tiene un criterio de terminación observable, no una descripción de esfuerzo.
- El corte propuesto en la tabla sigue siendo el correcto a la luz del inventario real que E0 levantó. **Si el inventario contradice la segmentación, prevalece el inventario**: se propone un corte nuevo y se espera confirmación.

### Control de coherencia B — de implantación, al cierre de cada etapa

Este control lo ejecuta el agente al terminar cada etapa, antes de pasar a la siguiente. No es la auditoría del framework sobre documentación generada: es la verificación de que **esta intervención** no rompió nada. Es obligatorio y su omisión invalida la etapa.

Al cerrar cada etapa, emitir una nota de coherencia siguiendo el patrón del archivo `SDD/Devs/Guides/Coherencia-Auditoria-Marco-v1.0.md`, que ya existe en el repositorio y define la forma: alcance, inventario de archivos tocados, verificación de invariantes, verificación de trazabilidad, observaciones y veredicto.

Lista de comprobación de la nota, idéntica en todas las etapas:

| # | Comprobación | Resultado esperado |
|---|---|---|
| 1 | Invariantes D1–D8 intactas en todo archivo tocado | Sin violaciones |
| 2 | Autosuficiencia: cero referencias fuera de `/IA/IA.SDD/` | Cero ocurrencias de las once cadenas de Restricciones |
| 3 | Referencias internas: todo archivo, carpeta y sección citada existe | Cero enlaces rotos |
| 4 | Vocabulario normalizado según la decisión 5 del Contexto | Sin términos del vocabulario viejo, salvo los exceptuados |
| 5 | Sin contradicción entre lo escrito en esta etapa y lo de las etapas anteriores | Sin contradicciones, o reportadas |
| 6 | Control de cambios actualizado en cada archivo modificado | Una fila por archivo |
| 7 | El caso degenerado —solución de un solo proyecto— sigue produciendo el layout aplanado | Verificado |
| 8 | Nada fuera del alcance declarado de la etapa fue modificado | Sin cambios colaterales |

**Veredicto y corte.** La nota cierra con `CONFORME` o `NO CONFORME`. Con `NO CONFORME` no se avanza: se corrige y se reemite. Con `CONFORME`, el agente **se detiene y espera confirmación humana** antes de arrancar la etapa siguiente. Es el mismo patrón de auditoría entre fases que el framework ya aplica en sus propias corridas.

### Reglas anti-deriva

- **Un descubrimiento no habilita un cambio.** Si durante una etapa aparece algo que exigiría modificar el resultado de una etapa ya cerrada, **no se modifica en silencio**: se registra en la nota de coherencia como observación, se reporta al usuario y se espera decisión. Retocar hacia atrás sin declararlo es exactamente cómo una intervención se descontrola.
- **El alcance de la etapa es techo, no piso.** Detectar un problema real fuera del alcance de la etapa en curso se reporta; no se arregla de paso.
- **Estado persistente para reanudar.** Mantener `/IA/IA.SDD.Documentacion/PROMPTs/Feactures/09-Editar-Agent-Rules-Documentacion-Examples-final/Estado-Intervencion.md` con una fila por etapa: identificador, estado (`Pendiente`, `En curso`, `Conforme`, `No conforme`), fecha, archivos tocados y ruta de su nota de coherencia. Se actualiza al cerrar cada etapa. **La intervención está diseñada para ejecutarse en varias sesiones**: una sesión nueva lee este archivo, identifica la primera etapa pendiente y arranca desde ahí sin rehacer nada.
- **Una etapa por sesión, si el volumen lo justifica.** E3 y E6 son las más grandes; conviene darles sesión propia. No hay premio por terminar todo de una vez, y sí un costo alto si la calidad se degrada sobre el final.
- **Ante ambigüedad genuina, preguntar.** El framework declara el patrón *plan-then-confirm* como garantía de calidad y prohíbe saltearlo. Aplica también a esta intervención: es preferible una pregunta a una decisión inventada que después hay que revertir en veinte archivos.

---

## Reglas

### Reglas de ejecución de este prompt

- **No inventar información.** Toda afirmación debe estar respaldada por evidencia verificable.
- **No presentar como hecho lo que no se verificó.** Marcar explícitamente `No verificado`, `Requiere verificación manual` o `Información no disponible` según corresponda.
- Las conclusiones surgen de las evidencias, nunca al revés. No omitir ni reinterpretar evidencia para sostener una conclusión.
- Ante dudas razonables: explicarlas, indicar limitaciones y proponer cómo verificarlas. No resolverlas por defecto.
- Respetar las invariantes globales D1–D8 del framework (idioma, encoding, Título-Con-Guiones, versionado, deprecación, trazabilidad, conjunto cerrado D8). El conjunto D8 sigue teniendo exactamente ocho valores: no se amplía ni se reduce.
- Todo archivo de reglas modificado incorpora su fila nueva en §9 control de cambios, con versión, fecha, descripción del cambio y autor.
- Mantener el patrón *plan-then-confirm* del framework: presentar el plan de intervención y esperar confirmación antes de modificar archivos.

### Reglas de redacción a embeber en el subagente AG-11 (categoría 11)

Estas reglas son el contenido que la Solicitud S5 pide incorporar a `Rules-Documentacion.md`. Se transcriben acá completas para que este prompt sea autocontenido.

#### Estilo narrativo formativo

El cuerpo documental de la categoría 11 no es una colección de fichas sueltas: es un recorrido. El lector debe terminar entendiendo el sistema y con criterio para intervenir en él según su rol, no solo reconociendo términos.

- **Diseñar el mapa antes de escribir las piezas.** Primero se fija el marco de referencia —el vocabulario común que reaparece en todos los documentos—, y recién entonces se desarrolla cada documento sobre ese marco. Ir de lo general a lo particular.
- **Definir cada término en su primer uso** y registrarlo en el glosario. Usar analogías cuando acerquen un concepto complejo.
- **Contextualizar todo ejemplo o snippet**: qué demuestra, precondiciones, resultado esperado. Un comando sin contexto no es documentación.
- **Cerrar las secciones densas con preguntas guía** que ayuden al lector a formar criterio.
- **Formativo, no enciclopédico**: cada sección deja al lector en condiciones de decidir.
- **Explicar, no solo describir**: qué es, para qué sirve, cómo funciona, cuándo se usa, cómo se relaciona con el resto.
- **Un procedimiento explica** objetivo, prerrequisitos, pasos, resultado esperado, validaciones y errores posibles. No es una lista de comandos.
- **Una arquitectura documenta** componentes, responsabilidades, relaciones, flujos y límites. No es una enumeración de archivos.
- **Interconectado**: cada documento enlaza con los que lo preceden y lo continúan. Ningún documento queda huérfano del mapa.
- **Única fuente de verdad**: un dato vive en un solo documento; el resto lo referencia. Documentos pequeños y especializados, con una única responsabilidad cada uno.
- **Adecuar la profundidad al tema**: ni superficial ni innecesariamente extensa.
- **No asumir** que el lector conoce el proyecto ni la documentación previa.

#### Doble audiencia

Todo documento sirve a la vez al agente humano que necesita **comprender** y al agente de IA que necesita **extraer datos y razonar**. Las dos caras describen el mismo hecho; ante divergencia, se corrige, nunca se mantienen versiones paralelas ni se bifurca el documento por tipo de lector.

El lector primario de esta categoría es el agente humano en primer contacto: alguien que no participó de ninguna fase de la especificación y no puede recuperar el contexto preguntándole al equipo que la produjo. La cara agente no compite con esa prioridad, la complementa: el mismo documento que le da a una persona el modelo mental le da a un agente de IA las rutas, los identificadores y las aserciones con las que operar.

**Cara humana:**

- Abrir cada documento con un resumen ejecutivo breve: qué es, para qué sirve, a quién le sirve.
- Narrar los flujos importantes de punta a punta con un caso concreto y datos de ejemplo realistas pero sintéticos. La narrativa complementa al diagrama, no lo repite.
- Progresar de lo general a lo específico.

**Cara agente:**

- **Frontmatter YAML** en todo documento, con al menos: `doc_id`, `doc_type`, `title`, `status`, `audience`, `owner`, `last_review`, `traces`.
- **Identificadores estables con prefijo** (`OPS-`, `EXT-`, `ISSUE-`, `VER-`, `CU-`, `US-`, `ADR-`): los enlaces y las trazas apuntan al identificador, no a la ruta.
- **Encabezados y anclas predecibles**: los documentos del mismo tipo comparten las mismas secciones, lo que habilita parseo y validación por estructura.
- **Diagramas y modelos como código** (Mermaid, OpenAPI, dbml): diffeables y regenerables. Imágenes binarias solo cuando no exista alternativa.
- **Bloques para agentes** (`entradas` / `salidas` / `validaciones`) en todo documento que defina un proceso que un agente deba ejecutar o verificar.
- **Rutas absolutas desde la raíz del repositorio**, nunca referencias vagas del tipo «el archivo de configuración del servicio».
- **Comandos verbatim, copy-paste**, con su salida esperada textual.
- **Criterios de éxito expresados como aserción, no como prosa.** Un humano lo lee; un agente lo ejecuta y lo evalúa.
- **Snippets con procedencia**: ruta, rango de líneas y versión de la fuente. Nunca copias sin origen.
- El vocabulario narrativo y el máquina-legible comparten el mismo glosario; los sinónimos se registran como alias del término canónico.

#### Voz narrativa

Aplica solo a las zonas de prosa —resúmenes, explicaciones, racional, narración de flujos—. No altera las zonas estructuradas —frontmatter, identificadores, tablas, anclas, diagramas como código—, que se mantienen rígidas a propósito. Ante conflicto, prevalece la regla estructural.

- Escribir desde el criterio de un profesional que entiende el sistema, no desde un molde rellenado. Cada sección responde a lo que ese caso concreto requiere.
- Formal y técnica, sin acartonamiento. Afirmar con seguridad cuando hay evidencia; señalar la incertidumbre cuando la hay, sin hedging defensivo.
- **Variar la longitud de frase.** Un texto donde todas las oraciones tienen el mismo largo y forma se percibe como automático.
- **Prosa donde corresponde prosa**: causa → efecto → impacto se narra en un párrafo conectado, no se fragmenta en viñetas. Reservar las listas para enumeraciones reales.
- **Sin paralelismo forzado**: no todas las viñetas tienen que empezar igual ni medir lo mismo.
- **Abrir con contenido, no con el título**: la primera frase de una sección aporta un hecho o contexto; no reformula el encabezado ni anuncia lo que la sección «va a» tratar.
- **Evitar las muletillas de relleno**: «Es importante destacar/señalar/mencionar que», «Cabe destacar», «En resumen», «En conclusión», «Como se puede observar», «Vale la pena mencionar».
- **Evitar los conectores decorativos encadenados** usados como pegamento entre frases que no lo necesitan.
- **Evitar los cierres genéricos** que resumen lo ya dicho sin agregar información o recomendación concreta.
- La voz es uniforme en todo el documento y entre documentos del mismo conjunto: se percibe una sola autoría.
- **Validación antes de cerrar**: releer las zonas de prosa y verificar que no hay muletillas, que la longitud de frase varía, que no hay listas que deberían ser párrafos, que ninguna sección abre reformulando su título y que los cierres aportan algo. Si un párrafo se puede borrar sin perder información, sobra.

#### Formato markdown

- Encabezados jerárquicos sin saltar niveles ni títulos vacíos, de lo general a lo específico.
- Secciones autocontenidas. Tabla de contenido cuando el tamaño lo justifique.
- No agregar secciones vacías ni decorativas.
- **Tablas** para resumir o comparar (parámetros, variables de entorno, puertos, servicios); sin celdas de texto largo.
- **Bloques de código** con lenguaje indicado, mínimos y con contexto.
- **Diagramas en Mermaid**, preferido sobre ASCII, para arquitectura, flujos, secuencias y dependencias. Sin diagramas redundantes.
- **Ejemplos reales**, obtenidos en la ejecución; los ilustrativos se marcan como tales.
- **Enlaces con rutas relativas** entre documentos relacionados; referenciar en lugar de duplicar.
- Validación antes de cerrar: estructura coherente, sin secciones vacías ni títulos duplicados, enlaces válidos, Mermaid sintácticamente correcto.

---

## Restricciones

### Autosuficiencia del repositorio `IA.SDD` (restricción bloqueante)

`IA.SDD` es hoy autosuficiente: **ningún archivo suyo referencia otro repositorio**. Esa propiedad es la que permite moverlo, clonarlo solo o distribuirlo sin arrastrar dependencias. Hay que preservarla sin excepción.

- **Prohibido escribir dentro de `/IA/IA.SDD/` cualquier ruta que apunte fuera de ese árbol.** Sin referencias a `IA.Prompts`, a `PromptFramework`, a `IA.SDD.Documentacion`, a `PROMPTs/`, ni a este prompt. Aplica a enlaces markdown, a rutas en prosa, a bloques de código y a las tablas de referencias.
- **Prohibido citar este prompt como fuente en el control de cambios.** La fila de §9 describe *qué cambió y por qué*, en términos del framework, no *quién lo pidió ni desde dónde*. Autor válido: el rol del subagente o «Reformulación SDD», siguiendo el precedente de las filas ya existentes.
- **Las reglas de redacción se transcriben, no se referencian.** El contenido de la sección «Reglas de redacción a embeber» se copia íntegro dentro de `Rules-Documentacion.md`. Está prohibido reemplazarlo por un puntero a un archivo externo, aunque ese archivo exista hoy en el workspace.
- **Los estándares de industria se nombran, no se linkean.** Escribir «Diátaxis», «modelo C4», «arc42», «AGENTS.md», «Game Day», «postmortem sin culpa» como nombres propios, sin URL. Es el criterio que el framework ya aplica con RFC 9457, SemVer, Conventional Commits, CycloneDX y SLSA, que aparecen nombrados y nunca enlazados. La tabla de URLs de este prompt es trazabilidad de esta intervención y **no se copia** al framework.
- **Verificación de cierre obligatoria.** Antes de terminar, ejecutar sobre `/IA/IA.SDD/` una búsqueda de las cadenas `IA.Prompts`, `PromptFramework`, `IA.SDD.Documentacion`, `PROMPTs/`, `Rule-Dual-Audience`, `Rule-Narrative-Voice`, `Rule-Markdown`, `Rule-Documentation`, `Rule-Evidences`, `Rule-Indexing`, `Study-Guide`, `http://` y `https://`. El resultado esperado de las primeras once es **cero ocurrencias**; las URLs solo se admiten si ya existían antes de esta intervención. Reportar el resultado en el informe de cierre. Si aparece alguna, es un hallazgo bloqueante: se corrige antes de dar el trabajo por terminado.

### Restricciones generales

- **Ejecutar por etapas.** Está prohibido correr la intervención completa de una sola pasada sin los cortes de coherencia definidos en «Etapas de ejecución y control de deriva». Cada etapa cierra con su nota de coherencia y espera confirmación humana.
- **No duplicar el marco teórico.** `Marco-Teorico-SDD-v1.0.md` ya existe con ~151 KB y catorce secciones. La guía de desarrollo lo referencia; no lo reescribe, no lo resume extensamente y no lo reemplaza.
- **Alcance de escritura**: solo el árbol `/IA/IA.SDD/`, más el informe de cierre y el archivo de estado en la carpeta de este prompt. No modificar ningún repositorio de solución destino.
- **No commitear ni pushear.** Dejar los cambios en el working tree para revisión.
- **No renumerar ninguna categoría fuera del par 10 ↔ 11.** Las categorías 00 a 09 conservan su número.
- **No ampliar ni reducir el conjunto cerrado D8.** Sigue teniendo exactamente ocho valores.
- **No generar documentación de usuario final no técnico.** Ese hueco existe en el framework y se declara como fuera de alcance; corregirlo no es parte de este trabajo.
- **No incorporar reglas de indexado de conocimiento** al cuerpo documental de la categoría 11.
- **No aumentar la carga documental de las categorías 00 a 09.** Los únicos cambios permitidos en ellas son: la tabla de contenido de S6; las fronteras declarativas con la categoría 11 en 05, 08 y 09 (S3.5); la extensión del sensado de deriva en `Deriva-Rules.md` (S2); y la condición de Definition of Done en `Rules-Plan-Sprint.md` (S4). Ninguno de esos cambios agrega artefactos ni prosa narrativa a esas categorías.
- **No romper el caso degenerado.** Una solución de un solo proyecto debe seguir produciendo el layout aplanado, sin subnivel `Proyectos/<Nombre>/` ni carpeta `Solucion/`.
- No eliminar contenido vigente de los archivos de reglas sin dejar constancia en el control de cambios del archivo afectado.

---

## Referencias

### Archivos objetivo de la intervención

| Referencia | Ruta | Acción |
|---|---|---|
| Rules Developer Guide | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Developer-Guide.md` | Renombrar a `Rules-Documentacion.md` y reescribir |
| Rules Examples | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Examples.md` | Ampliar con la doble arista |
| Root Rules | `/IA/IA.SDD/SDD/Devs/Rules/Root-Rules.md` | Layout canónico y numeración |
| Deriva Rules | `/IA/IA.SDD/SDD/Devs/Rules/Deriva-Rules.md` | Extender el sensado de deriva a contratos y comportamiento |
| Intake Rules | `/IA/IA.SDD/SDD/Devs/Rules/Intake-Rules.md` | Verificar referencias a materialización de `/samples` |
| Maqueta Rules | `/IA/IA.SDD/SDD/Devs/Rules/Maqueta-Rules.md` | Verificar referencias cruzadas |
| Rules Contexto | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Contexto.md` | Tabla de contenido (S6) |
| Rules Necesidades Negocio | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Necesidades-Negocio.md` | Tabla de contenido (S6) |
| Rules Especificacion Funcional | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Especificacion-Funcional.md` | Tabla de contenido (S6) |
| Rules UX UI DX | `/IA/IA.SDD/SDD/Devs/Rules/Rules-UX-UI-DX.md` | Tabla de contenido (S6) |
| Rules Prompts AI | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Prompts-AI.md` | Tabla de contenido (S6) |
| Rules Arquitectura Tecnica | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Arquitectura-Tecnica.md` | Tabla de contenido (S6) + frontera con 11 |
| Rules Backlog Tecnico | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Backlog-Tecnico.md` | Tabla de contenido (S6) |
| Rules Plan Sprint | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Plan-Sprint.md` | Tabla de contenido (S6) |
| Rules Calidad Y Pruebas | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Calidad-Y-Pruebas.md` | Tabla de contenido (S6) + frontera con 11 |
| Rules Devops | `/IA/IA.SDD/SDD/Devs/Rules/Rules-Devops.md` | Tabla de contenido (S6) + frontera con 11 |
| Master Prompt | `/IA/IA.SDD/SDD/Devs/Orchestrator/Master-Prompt.md` | Orden de fases, Fases I y J, precondiciones y auditoría |
| SDD User Guide | `/IA/IA.SDD/SDD/Guides/SDD-User-Guide.md` | Actualización de cara al usuario (S7) |
| README raíz | `/IA/IA.SDD/README.md` | Reescribir como punto de entrada (S9) |
| SDD Development Guide | `/IA/IA.SDD/SDD/Guides/SDD-Development-Guide.md` | Escribir desde cero; hoy está vacío (S10) |
| Marco Teórico | `/IA/IA.SDD/SDD/Devs/Guides/Marco-Teorico-SDD-v1.0.md` | **No reescribir.** Actualizar solo donde el intercambio 10↔11 y las Fases I/J lo desactualicen |
| Getting Started Guide | `/IA/IA.SDD/SDD/Guides/SDD-Getting-Started-Guide.md` | Actualizar solo si el intercambio lo desactualiza |
| Coherencia del marco | `/IA/IA.SDD/SDD/Devs/Guides/Coherencia-Auditoria-Marco-v1.0.md` | Patrón a reutilizar para las notas de coherencia por etapa. No modificar |
| Estado de la intervención | `/IA/IA.SDD.Documentacion/PROMPTs/Feactures/09-Editar-Agent-Rules-Documentacion-Examples-final/Estado-Intervencion.md` | Crear y mantener por etapa |
| Informe de cierre | `/IA/IA.SDD.Documentacion/PROMPTs/Feactures/09-Editar-Agent-Rules-Documentacion-Examples-final/Informe-Intervencion-v1.0.md` | Crear (S11) |

### Nomenclatura de industria adoptada

Las decisiones de nomenclatura de este prompt se apoyan en marcos y convenciones establecidos. Se registran para que las elecciones sean auditables y no queden como criterio propio no declarado.

| Concepto adoptado | Marco de industria | Dónde se aplica |
|---|---|---|
| Separación tutorial / how-to / referencia / explicación | **Diátaxis** | Cuerpo integrador de la categoría 11 (ya vigente en el framework) |
| Diagrama de contexto y diagrama de contenedores | **C4 model** (Simon Brown) | `Vision-General-Sistema-v<X.Y>.md` |
| Estructura de documentación de arquitectura | **arc42** | Frontera declarada con la categoría 05 |
| Registro de decisiones de arquitectura | **ADR** | Ya vigente en la categoría 05 |
| Guía de contribución al repositorio | Convención **CONTRIBUTING** | `Guia-Contribucion-v<X.Y>.md` |
| Procedimiento de operación e incidentes | **Runbook** (práctica SRE) | `Runbook-Operacion-v<X.Y>.md` |
| Contrato de contexto para agentes de codificación | **AGENTS.md**, formato abierto bajo la Agentic AI Foundation (Linux Foundation) | `AGENTS.md` en la raíz del repositorio destino |
| Documentación que evoluciona con el sistema | **Living Documentation** | Momento 2 del modelo de tres momentos |
| Documentación versionada y entregada por el flujo del código | **Docs as Code** | Modelo general de la categoría 11 |
| Verificación continua de la documentación contra el código | **Continuous Documentation** | Fases I y J |
| Documentación como requisito de cierre del incremento | **Definition of Done** | Cadencia de actualización del Momento 2 |
| Ejemplo concreto que opera como criterio de aceptación | **Specification by Example** / *Executable Specification* | Arista B de la categoría 10 |
| Ensayo de un procedimiento operativo en condiciones reales para descubrir sus huecos | **Game Day** (práctica SRE) | Guion de ensayo del rol operador |
| Prueba de usabilidad de la documentación con un lector sin contexto previo | *Documentation usability testing* / «test del developer nuevo», ya nombrado en `Rules-Developer-Guide.md` §5.4 pero nunca operacionalizado | Ensayo de entrega, guiones de mantenedor e integrador |
| Registro estructurado de incidente con síntoma, causa raíz, resolución y acciones derivadas, sin atribución de culpa | **Blameless postmortem** (práctica SRE) | `Bitacora-Eventualidades-v<X.Y>.md` |
| Publicación de limitaciones y problemas conocidos del producto | *Known issues* | Destino de triaje de las eventualidades |

Fuentes consultadas para la verificación de las convenciones más recientes:

- [AGENTS.md — formato abierto para agentes de codificación](https://agents.md/)
- [The Agent-Native Repo: Why AGENTS.MD is the New Standard](https://www.harness.io/blog/the-agent-native-repo-why-agents-md-is-the-new-standard)
- [Teach agents your codebase — AGENTS.md](https://ona.com/docs/ona/agents-md)
- [What is Continuous Documentation? The manifesto](https://swimm.io/blog/what-is-continuous-documentation-manifesto-part-1)
- [Continuous Documentation in a CI/CD World](https://thenewstack.io/continuous-documentation-in-a-ci-cd-world/)
- [Continuous documentation: publishing docs early and often](https://www.doctave.com/blog/continuous-documentation)
- [Documentation-as-Code Has Silently Won For Tech Content](https://dev.to/zenika/documentation-as-code-has-silently-won-for-tech-content-e5o)
- [Effective architecture documentation with arc42 and C4](https://www.linkedin.com/pulse/effective-architecture-documentation-arc42-c4-torsten-mosis)
- [Example Software Architecture Documentation with arc42 and the C4 model](https://github.com/bitsmuggler/arc42-c4-software-architecture-documentation-example)
