# Análisis de Optimizaciones — Orquestador Generar-SDD

- Fecha: 2026-07-22
- Estado: draft
- Alcance: la corrida real de generación de SDD sobre `DEV/SAI.Service.Core` (prompt de invocación `DEV/SAI.Service.Core.Documentacion/PROMPTs/Generar-SDD/Crear-SDD-Documento-Intake.md`), evaluada contra el framework `IA/IA.SDD/SDD/Devs/Orchestrator/Master-Prompt.md` (v3.4).
- Foco pedido: qué pasos conviene delegar a subagentes con contexto aislado para no inflar el contexto del orquestador.
- Restricción: análisis de solo lectura. No se modifica el framework ni ningún otro archivo. Las propuestas sobre el framework quedan como recomendación, no como cambio aplicado.

## 1. Veredicto en una frase

El diseño del orquestador ya resuelve bien el problema de contexto (fan-out a subagentes, retorno destilado, estado en archivos, audit sin contexto previo). El costo de contexto no está en el diseño sino en la corrida: hay pasos que en la práctica se están resolviendo dentro del contexto del orquestador y deberían salir a un subagente aislado o a una sesión aparte. Este análisis los identifica y prioriza.

## 2. Marco: dos memorias distintas

Conviene separar dos cosas que se confunden bajo "cargar solo lo necesario":

- Pesos del modelo (RAM/VRAM del modelo cargado): se optimiza con arquitectura (MoE, offloading). Es ortogonal a este análisis.
- Contexto (lo que entra al prompt: documentos fuente, historial del orquestador, devoluciones de subagentes): es memoria de trabajo finita; cada token cuesta latencia, plata y foco. Este análisis es sobre esta segunda memoria.

La técnica para acotar el contexto es exactamente la que el framework ya prescribe: delegar a subagentes con contexto propio que devuelven un resumen destilado, y persistir el estado en archivos para no arrastrarlo en el hilo.

## 3. Qué ya hace bien el orquestador (no romper)

El Master-Prompt v3.4 ya implementa buenas prácticas de presupuesto de contexto. Se listan para delimitar qué NO hace falta cambiar:

| Mecanismo | Dónde | Efecto sobre el contexto |
|---|---|---|
| Principio de delegación de la especialidad | Master-Prompt §1 y §6 | El orquestador lee las reglas on-demand por categoría; no carga las 17 reglas de una. |
| Despacho a subagentes por categoría | §8 | Cada categoría se genera en un contexto aislado. |
| Contrato de devolución destilada | §8, bloque "Devolución" | El subagente devuelve resumen de 5 líneas + paths + auto-chequeo, no el documento entero. |
| Audit sin contexto previo | §10 ("se invoca desde cero, sin contexto previo") | La auditoría no hereda ni ensucia el contexto de generación. |
| Estado externalizado | `SDD/Docs/`, `SDD/Audit/`, log del orquestador | Los entregables viven en disco; el hilo no los transporta. |
| Detención plan-then-confirm | §7 | Cada fase corta; permite reanudar sin recargar todo. |

Conclusión parcial: el patrón está bien. El problema es de disciplina de ejecución, no de arquitectura.

## 4. Dónde se infla el contexto en la corrida real

Evidencia tomada del `Historial/` de la corrida SAI (`DEV/SAI.Service.Core.Documentacion/PROMPTs/Generar-SDD/Historial/`).

1. Front-load del antecedente de 4017 líneas. El prompt de invocación (`Crear-SDD-Documento-Intake.md`, punto 3.1) manda leer `Inputs/Planteo-Analisis-Unificado-Antecedente-SAI-Service.md` (4017 líneas) para derivar el intake. `Historial/00-.md` lo nombra explícitamente y se pregunta "sobrecarga al agente orquestador?". Ese documento entra crudo al contexto que después tiene que seguir orquestando.

2. Side-quests de entorno dentro del hilo del orquestador. El caso más claro: `Historial/14-identificado-investigando-nut.md` (394 líneas), una sesión completa de diagnóstico y arreglo de NUT/SAI por USB (regla udev, permisos, `upsc`, alta de usuario RO) para poder validar el adaptador de BT-15. Es infraestructura, no generación de SDD, y consumió un contexto enorme dentro de la conversación de orquestación. Mismo patrón, menor tamaño, en `08-Primer-Build-Verificacion-Maqueta-DevContainer.md`, `10-autentificado.md`, `07-request.md`.

3. Q&A del orquestador sin captura estructurada. El `Historial/` (archivos 00 a 22) no es el hilo del orquestador ni una salida suya: lo mantiene el humano a mano, como registro de las preguntas que el orquestador le hace durante la corrida, para reevaluarlas y trabajar optimizaciones. Que haga falta llevarlo a mano es en sí la señal: el ciclo de preguntas del orquestador (Master-Prompt §9) no queda capturado de forma reutilizable y el humano compensa. Dos consecuencias: (a) el hilo real del orquestador —que sí acumula planes, confirmaciones, veredictos y Q&A de todas las fases— no está instrumentado, y el `Historial/` es una reconstrucción manual parcial; (b) side-quests como la investigación de NUT (`14-...nut.md`, 394 líneas) quedaron registrados ahí, evidencia de que ocurrieron dentro de las sesiones reales.

4. Síntesis final sobre muchos archivos. El handoff (§12) arma un resumen ejecutivo leyendo transversalmente todos los `SDD/Docs/`, los audits y el log. Si lo hace el propio orquestador al final, recarga en su contexto lo que ya había externalizado.

## 5. Pasos a delegar (priorizado)

Cada fila indica el paso, por qué inflama, a qué subagente aislado va y —lo más importante— qué debe devolver (contrato de retorno destilado). El orden es por relación impacto/esfuerzo.

| ID | Paso a delegar | Por qué infla hoy | Subagente aislado | Qué devuelve (y solo eso) |
|---|---|---|---|---|
| DEL-01 | Investigaciones de entorno / infra (NUT, build, DevContainer, auth) | 394 líneas de una en el hilo (`14-...nut.md`) | Agente "operador de entorno" en sesión aparte | Un bloque de datos: por ejemplo "NUT accesible en 127.0.0.1:3493, ups `sai`, RO saimon/saimon, status OL". Nada del proceso de debug. |
| DEL-02 | Lectura y extracción del antecedente de 4017 líneas para llenar el intake | Front-load crudo al contexto que después orquesta | Agente "lector de antecedente" | Extracción estructurada por sección del template intake (mapa sección→dato), más las anclas de origen (Anexo A/B, rango de líneas). No el texto de las 4017 líneas. |
| DEL-03 | Validación de completitud del intake y scan de placeholders (§2, §3.1) | Requiere releer el intake entero cada vez | Agente "validador de intake" | Lista enumerada de pendientes (archivo, sección, placeholder) o "sin pendientes". |
| DEL-04 | Síntesis del handoff / resumen ejecutivo (§12) | Recarga todos los Docs y audits al final | Agente "consolidador de handoff" | Las tablas del resumen ejecutivo de §12 ya armadas, leyendo los archivos por su cuenta. |
| DEL-05 | Generación de cada categoría 02–11 | Ya delegado por §8; el riesgo es re-leer el doc generado "para revisar" | AG-00…AG-11 (ya existen) | Mantener estricto el contrato de §8: 5 líneas + paths + auto-chequeo. Nunca releer el documento completo al hilo del orquestador; para eso está el audit. |
| DEL-06 | Audit por fase | Ya delegado y sin contexto previo por §10 | Auditor independiente (ya existe) | Solo el veredicto (APROBADO / CON OBSERVACIONES / RECHAZADO) y los P0/P1; el informe completo va al archivo `SDD/Audit/`, no al hilo. |
| DEL-07 | Fijar en el intake la procedencia de los escenarios E-1…E-8 (fix de cadena de custodia, no delegación) | Sin ruta declarada, quien genere 02 / la maqueta vuelve a barrer el antecedente de 4017 líneas para resolver "E-1" | — (edición del intake, no subagente) | Dos ediciones de forma ya identificadas en `Historial/00-.md`: una oración de procedencia con el rango de líneas del Anexo A/B, y una fila en la tabla de trazabilidad downstream. Evita que DEL-02 se repita cada fase. |
| DEL-08 | Cosechar el `Historial/` para pre-responder aguas arriba las preguntas recurrentes del orquestador (feedback loop, no delegación) | Cada ambigüedad §9 es un round-trip que cuesta tokens (pregunta + respuesta + relectura/actualización del intake) y queda en el contexto; muchas se repiten entre fases | — (práctica de mejora del intake / inputs) | Que las preguntas recurrentes del Historial pasen a ser bloqueantes precargados del intake (Intake-Rules §2/§5), de modo que §3 las junte en una sola batería inicial y no se re-pregunten mid-run. Ver §8bis. |

Notas:

- DEL-01 es el de mayor retorno inmediato: es puro ruido para la generación de SDD y ya hoy su salida útil cabe en tres líneas. Conviene resolver la infra en una sesión propia y traer al orquestador solo el dato verificado (es justo lo que quedó registrado como conocimiento reusable del entorno de desarrollo).
- DEL-05 y DEL-06 no son cambios: son disciplina. El framework ya los delega bien; el modo de romperlos es que el humano (o el orquestador) relea documentos generados o informes de audit completos dentro del hilo.
- DEL-07 es preventivo: no ahorra contexto por sí mismo, pero cierra la fuga que obliga a la re-lectura repetida del antecedente. Es un cambio de forma sobre el intake de la solución (permitido), no sobre el framework. Su detalle exacto (párrafo y fila) ya está redactado en `Historial/00-.md`.

## 6. Recomendación de ejecución (sin tocar el framework)

1. Una sesión por fase, no un hilo único. El `Historial/` numerado sugiere una sola conversación de 00 a 22. Conviene cortar por fase (o al menos por proyecto y por cada side-quest), usando los archivos de `SDD/Docs/`, `SDD/Audit/` y el intake como handoff entre sesiones. El framework lo habilita: es plan-then-confirm con estado en disco.
2. Side-quests fuera de banda (DEL-01). Cualquier "andá a arreglar/verificar X en el entorno" se abre como sesión o subagente aparte y vuelve como dato, no como transcripción.
3. Contrato de retorno inviolable (DEL-05/06). Lo que sube al contexto del orquestador es el resumen de §8 y el veredicto de §10; los documentos e informes viven en archivo y se releen solo bajo demanda puntual.
4. Antecedentes grandes se leen una vez, por un lector (DEL-02). El orquestador recibe la extracción, no la fuente.
5. Compactación por fase y archivo de estado de reentrada. Cuando una sesión igual se alarga, al cerrar cada fase se compacta lo hecho en un único archivo de estado breve (fase cerrada, veredicto, paths generados, decisiones y ambigüedades pendientes) y se descarta el detalle del hilo. Una sesión nueva lee primero ese archivo de estado como índice de reentrada y expande a los documentos solo ante necesidad. Es el mismo patrón de índices jerárquicos que ya se usa en `ia-db` (leer la entrada y 1–2 índices; ampliar a la fuente ante insuficiencia comprobada), aplicado a la corrida del orquestador. El log del orquestador que ya prescribe el framework es el candidato natural a cumplir ese rol, y el `Historial/` que el humano ya lleva a mano es una versión artesanal de este mismo artefacto (ver §8bis).

## 7. Cuándo no delegar (riesgos y límites)

Delegar no es gratis; el plan debe aplicarse con criterio, no como regla ciega.

- Costo de coordinación. Cada subagente agrega un ida y vuelta (armar el prompt, recibir, integrar). Para un paso chico, el overhead puede superar el ahorro de contexto.
- Pérdida de matiz. El subagente solo ve lo que se le pasa. Si el paso necesita contexto transversal (coherencia entre categorías, decisiones tomadas en fases previas), un retorno demasiado destilado puede omitir lo que importaba. El contrato de devolución tiene que llevar lo justo, no menos.
- Más piezas móviles. Más archivos de handoff y más sesiones significan más superficie para que algo quede desincronizado. Se mitiga con el archivo de estado único de §6.5 como fuente de verdad de la reentrada.
- Regla práctica: delegar cuando el paso es voluminoso y su salida útil es chica (DEL-01, DEL-02, DEL-04) o cuando pide mirada externa (DEL-06). No delegar decisiones de orquestación ni confirmaciones plan-then-confirm: esas son la responsabilidad indelegable del orquestador (Master-Prompt §1).

## 8. Cómo medir que la mejora funciona

Criterios de éxito verificables, para tratar esto como plan y no solo como diagnóstico:

- Ningún archivo de `Historial/` (o su equivalente por sesión) mezcla generación de SDD con investigación de entorno. Las side-quests viven en sesiones propias (verifica DEL-01).
- El antecedente voluminoso aparece leído una sola vez en toda la corrida; las fases posteriores citan la extracción o la procedencia declarada, no la fuente (verifica DEL-02 y DEL-07).
- Cada reentrada de sesión arranca leyendo el archivo de estado y a lo sumo 1–2 documentos, no el árbol completo de `SDD/Docs/` (verifica §6.5).
- Proxy simple mientras no se midan tokens: la extensión de la sesión del orquestador por fase deja de crecer con la cantidad de fases ya cerradas.
- La cantidad de preguntas §9 mid-run baja corrida a corrida a medida que el Historial se cosecha hacia el intake (verifica DEL-08).

## 8bis. El Historial como artefacto de mejora (doble rol)

El `Historial/` que el humano mantiene a mano no es un residuo de la corrida: es un instrumento con dos funciones que conviene reconocer explícitamente en el plan.

- Rol de reflexión (el que ya cumple): registra las preguntas que el orquestador hace mid-run para reevaluarlas con calma y detectar dónde el proceso pide datos que deberían haber estado.
- Rol de optimización de tokens (el que se suma): cada pregunta capturada es un candidato a precargarse en el intake. Empujar la respuesta aguas arriba (DEL-08) convierte una detención §9 mid-run —cara en tokens y en contexto— en un bloqueante resuelto en la batería inicial de §3, que además no se repite en fases posteriores.

Convergencia con §6.5: el Historial es una versión artesanal del archivo de estado de reentrada. Darle una estructura mínima y estable (pregunta, fase, respuesta, decisión, ¿va al intake?) lo vuelve mecánicamente cosechable para DEL-08 sin perder su valor de reflexión. No se propone automatizarlo ni quitarle el carácter manual: se propone estructurarlo para que rinda en las dos funciones.

Límite: mantenerlo a mano tiene un costo de trabajo humano real. La mejora es que ese trabajo, hoy usado solo para reflexionar, pase a alimentar también la reducción de preguntas y de tokens de la próxima corrida.

## 9. Recomendaciones opcionales sobre el framework (no aplicadas)

Fuera del alcance de esta corrida, pero anotadas para una futura versión del Master-Prompt:

- Explicitar en §2 que la lectura de fuentes voluminosas del intake se delega a un subagente lector que devuelve extracción estructurada (formaliza DEL-02).
- Agregar una nota operativa en §7 que declare que las tareas de verificación de entorno (evidencia D9) se ejecutan fuera del hilo del orquestador y retornan solo el dato citable (formaliza DEL-01 y protege el contexto sin debilitar D9).
- En §12, asignar la síntesis del handoff a un subagente consolidador (formaliza DEL-04).

## 10. Limitaciones de este análisis

- Es de solo lectura; no se ejecutó ni se modificó el framework ni la corrida.
- La evidencia proviene del `Historial/` y de los prompts de invocación disponibles al 2026-07-22; no cubre partes de la conversación que no hayan quedado registradas en esos archivos.
- Las estimaciones de "cuánto infla" son cualitativas (conteo de líneas de los archivos de historial), no una medición de tokens real.
