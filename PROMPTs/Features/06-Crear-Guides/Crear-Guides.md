# Tool-Prompt — Actualizar y generar guías de usuario del SDD

> **Invocación**: `Lee y ejecuta /IA/SDD/IA.SDD.Documentacion/PROMPTs/Features/06-Crear-Guides/Crear-Guides.md`
>
> **Overview**: Actualizar y agregar la guía getting started del SDD

---

## Contexto

Leer [SDD User Guide], la guía que describe la metodología de uso del `Framework SDD` (SDD, Spec-Driven Development) para especificar y codear por asistencia de IA.

Se requiere contar con una guía de consulta para quien recién comienza a utilizar el framework SDD para especificar y codear una solución mediante la asistencia por IA.

Por otro lado, se requiere documentar el último proceso experimentado, este consistió en:

1. **Paso 1**. Se crean dos repositorios necesarios para el desarrollo. Un repositorio será el repositorio destino en el que se generá la documentación de especificación de diseño y el código propiamente dicho de la solución. El otro repositorio se utiliza para documentar la solución, indexar el repositorio destino, diseñar prompts que se aplicaran sobre el repositorio destino. Por ejemplo:
- `SAI.Service.Core`: es el [Repositorio destino] , normalmente lleva el nombre de la solución.
- `SAI.Service.Core.Documentacion`: es el [Repositorio de Documentación] en el que se van a editar prompt para ejecutar sobre el  [Repositorio destino] , indexación de código fuente para uso de agentes IA, documentación sobre analisis , etc.

2. **Paso 2**. Se crea una jerarquía de carpetas:
1.a. Se crea una carpeta workspace. Será la carpeta que reuna todos los repositorios y que muchos de estos estarán vinculados a algunos repositorios durante el proceso de desarrollo en cuanto a la ejecución de prompt y consultas de los agentes AI.
2.b. Se clonan dos los dos repositorios necesarios para el desarrollo de la nueva solución. 
2.c. Se clona el repositorio del `Framework SDD`. Desde aquí se lanzará el prompt orquestador dirigido al repositorio destino

Por ejemplo el workspace podría quedar de la siguiente manera:
```
workspace
  |- DEV       /*Carpeta para agrupa los repositorios que se estan desarrollando , el nombre puede variar */
  |    |- SAI.Service.Core       /*repositorio destino*/
  |    |   |- Otros archivos de la solución              
  |    |     
  |    |- SAI.Service.Core.Documentacion /*Repositorio para documetación, indexación de fuentes del repositorio destino, y prompts aplicados*/
  |    |    |
  |    |    |- PROMPTs
  |    |    |    |- 01-Ejecutar-Prompt-Integrador-Documento-Intake
  |    |    |    |- 02-Ejecutar-Prompt-Orquestador
  |    |    |    |- **-Otros
  |    |    |   
  |    |    |- Otros...
  |    |
  |    |- Otro.Repositorio
  |    |
  |    |- Otro.Repositorio.Documentacion
  |
  |- IA  /*Carpeta para agrupar frameworks IA, el nombre puede variar*/
  |   |- IA.SDD    /*Respoistorio del Framework SDD*/ 
  |   |     |
  |   |
  |   |- Otro.Repositorio    
```

3. **Paso 3**. El desarrollador investiga, recopila datos y requerimientos funcionales en diferentes documentos de la solución que se propone como objetivo a desarrollar. Estos documentos se utilizarán luego en el paso siguiente. Muchos documentos los irá dejando en `/DEV/SAI.Service.Core.Documentacion/PROMPTs/01-Ejecutar-Prompt-Integrador-Documento-Intake/INPUTs`

4. **Paso 4**. Se crea e invoca un **prompt integrador** ([Ejemplo Prompt Integrador]), el cual toma como entrada toda la documentación recopilada en la etapa de investigación previa por parte del desarrollador y la reúne en un único documento de entrada (*intake*).

   Este documento intake formaliza y reune todas las evidencias, ejemplos, datos y requerimientos aportados por el desarrollador sobre la solución a desarrollar. Un ejemplo es: [Solution Intake SAI Service]. El agente IA crea este documento intake en la carpeta donde corresponda en el repositorio destino de la solución a desarrollar. En el repositorio destino se crean las carpetas necesarias y se crea el documento intake donde corresponde.

5. **Paso 5**. Se invoca al **prompt orquestador**, por ejemplo: [Ejecutar Prompt Orquestador].

   Cuando se invoca el **prompt orquestador**, el agente orquestador comienza a crear la documentación de especificación (SDD) tomando como entrada el documento intake generado en el paso anterior. Terminada la etapa de
   creación de documentación de especificación, sigue con la codificación de la solución propuesta. En conclusión, una vez que comienza el agente orquestador, el flujo queda bajo el proceso que está predeterminado en el **prompt orquestador**.

---

## Objetivo

Crear un documento que permita a un desarrollador comenzar por primera vez a diseñar y construir una nueva solución usando este `Framework SDD`.

Retroalimentar con la experiencia expuesta en [Contexto](#contexto) la documentación de usuario del `Framework SDD`.

---

## Solicitudes

1. Leer y evaluar [SDD User Guide].
2. A partir de los objetivos y el contexto planteado, generar el documento markdown **"SDD Getting Started Guide"** en `/IA/SDD/IA.SDD/SDD/Guides/SDD-Getting-Started-Guide.md`.

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