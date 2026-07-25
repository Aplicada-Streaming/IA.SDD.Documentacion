# Tool-Prompt — Actualizar y generar guías de usuario del SDD

> **Invocación**: `Lee y ejecuta /IA/IA.SDD.Documentacion/PROMPTs/Feactures/Crear-Log-File/Crear-Log-File.md`
>
> **Overview**: Actualizar y agregar la guía getting started del SDD

---

## Contexto

Leer [SDD User Guide], la guía que describe la metodología de uso del `Framework SDD` (SDD, Spec-Driven Development) para especificar y codear por asistencia de IA.

Actualmente se requiere registrar los avances o historial de los avances de los agentes en el trabajo de sus tareas.
1. Este historial debería servir para poder retomar el flujo de trabajo lanzado por el prompt orquestador recuparando el contexto y las tareas en las que estaban los subagentes.
2. Asi mismo, ese historial tambien serviría que el desarrollador de  `Framework SDD`  puedea revisar y consultar sobre decisiones tomadas, resultados presentados en cada etapa, deciciones.

---

## Objetivo

Crear un documento que permita a un desarrollador comenzar por primera vez a diseñar y construir una nueva solución usando este `Framework SDD`.

Retroalimentar con la experiencia expuesta en [Contexto](#contexto) la documentación de usuario del `Framework SDD`.

---

## Solicitudes

1. Leer y evaluar [SDD User Guide].
2. A partir de los objetivos y el contexto planteado, generar el documento markdown **"SDD Getting Started Guide"** en `/IA/IA.SDD/SDD/Guides/SDD-Getting-Started-Guide.md`.

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