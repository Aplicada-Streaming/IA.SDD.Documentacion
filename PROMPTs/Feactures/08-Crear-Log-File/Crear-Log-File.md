# Tool-Prompt — Actualizar y generar guías de usuario del SDD

> **Invocación**: `Lee y ejecuta /IA/IA.SDD.Documentacion/PROMPTs/Feactures/Crear-Log-File/Crear-Log-File.md`
>
> **Overview**: Actualizar y agregar la guía getting started del SDD

---

## Contexto

Leer [SDD User Guide], la guía que describe la metodología de uso del `Framework SDD` (SDD, Spec-Driven Development) para especificar y codear por asistencia de IA.

Actualmente se requiere registrar los avances o historial de los avances de los agentes en el trabajo de sus tareas.


---

## Objetivo

El flujo de trabajo sobrellevado por  `Framework SDD`  debe contemplar crear un log de contexto y de registro para el desarrollador.

1. Es poder reanudar el flujo de trabajo en un nueva sesión recuperando en el estado donde se interrumpio.

2. El desarrollador debe poder consultar y revisar el historial de actividad que ha sucedido durante todo el flujo de trabajo orquestado por el `Framework SDD` .



---

## Solicitudes

1. Leer y evaluar [SDD User Guide].

2. Evalua los objetivos y que alternativas hay al respecto.

3. Evaluar la siguiente alternativa, mediante un archivo historial ubicado en
`/<Repositorio-Destino>/SDD/Logs/Log.md`, donde `/<Repositorio-Destino>` es la ruta destino que se especifico en la invocación del prompt: `PROMPT-Agente-Bootstrap-SDD.md`. Enctonces este archivo `Log.md` registraría todo lo necesario para que el prompt  `PROMPT-Agente-Bootstrap-SDD.md` inicie el flujo de trabajo en contexto donde se quedo antes durante la interrupción. Y tambien serviría para que el desarrollador tuviera un medio consulta sobre cuestiones, preguntas, respuestas y resultados presentados por los subagentes. Lo que queda analizar es incluir al información valida para que los subagentes retomen el contexto de forma adecuada. y que información debe contener para ela gente humano. 

En todos la alternativas hay que considerar.
En este sentido hay que considerar que si hay subagentes en paralelo no corrompan el fichero al escribir en este. Otra consideración es que 



3.a  Generar un archivo historial que debería servir que al relanzar el promptorquestador pueda retomar el flujo de trabajo lanzado por el mismo prompt orquestador en otra sesión después de haber sido interrumpido o haber cambioado de host, reanudando el flujo de tarea sobrellevado en el momento de la interrupción recuperando las tareas en curso con sus subagentes asignado, y el contexto de cada uno de los subagentes, 

2.b Asi mismo, ese historial debe servir para que el desarrollador de  `Framework SDD`  tenga ese documento como herramienta para revisar y consultar sobre decisiones tomadas en cada final de sprint o saber que consultas respondio a lo largo de la fase del flujo de trabajo por parte de los diferentes subagentes.


---

## Reglas

- Los documentos markdown de documentación deben estar organizados en secciones jerárquicas. Deben incluir definiciones, explicaciones, ejemplos claros y en cuando sea necesario gráficos mermaid. 
- Ser fiel a la estructura del `Framework SDD`, siguiendo en principio lo dictado por [SDD User Guide], pero contemplando la experiencia última como parte de una mejora en la metodología aplicada.
- No inventar información. 
- Toda afirmación debe estar respaldada por evidencia verificable.
- `/IA/IA.Prompts/PromptFramework/Rules/Rule-Dual-Audience.md`


---

## Referencias

| Referencia | Ruta |
|---|---|
| [SDD User Guide] | `/IA/IA.SDD/SDD/Guides/SDD-User-Guide.md` |
| [Ejemplo Prompt Integrador] | `/DEV/SAI.Service.Core.Documentacion/PROMPTs/01-Ejecutar-Prompt-Integrador-Documento-Intake/Ejecutar-Prompt-Integrador-Documento-Intake.md` |
| [Solution Intake SAI Service] | `/DEV/SAI.Service.Core/SDD/Intake/SOLUTION-INTAKE-Sai-Service-Core-v1.0.md` |
| [Ejecutar Prompt Orquestador] | `/DEV/SAI.Service.Core.Documentacion/PROMPTs/02-Ejecutar-Prompt-Orquestador/Ejecutar-Prompt-Orquestador.md` |
| [Repositorio destino] | `/DEV/SAI.Service.Core` |
| [Repositorio de Documentación] | `/DEV/SAI.Service.Core.Documentacion` |

[SDD User Guide]: /IA/IA.SDD/SDD/Guides/SDD-User-Guide.md
[Ejemplo Prompt Integrador]: /DEV/SAI.Service.Core.Documentacion/PROMPTs/01-Ejecutar-Prompt-Integrador-Documento-Intake/Ejecutar-Prompt-Integrador-Documento-Intake.md
[Solution Intake SAI Service]: /DEV/SAI.Service.Core/SDD/Intake/SOLUTION-INTAKE-Sai-Service-Core-v1.0.md
[Ejecutar Prompt Orquestador]: /DEV/SAI.Service.Core.Documentacion/PROMPTs/02-Ejecutar-Prompt-Orquestador/Ejecutar-Prompt-Orquestador.md
[Repositorio destino]: /DEV/SAI.Service.Core
[Repositorio de Documentación]: /DEV/SAI.Service.Core.Documentacion