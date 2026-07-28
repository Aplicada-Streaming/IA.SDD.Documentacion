# Changelog

Todos los cambios relevantes de este repositorio (`IA.SDD.Documentacion`) se documentan acá.
Formato basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/).

## [1.10] - 2026-07-28

Prompt nuevo de revisión del framework SDD sobre los hallazgos de una corrida real, con el insumo que los reportó y los seis documentos que produjo la revisión, más una nota de trabajo y el reordenamiento de las salidas del prompt 09 en su carpeta `OUTPUTs/`.

### Añadido

- **Prompt «Crear revisión sobre hallazgos»** en `PROMPTs/Feactures/10-Revision-SDD/Crear-Revision-Sobre-Hallazgos.md`: tool-prompt que encarga contrastar contra el framework SDD los conflictos y observaciones detectados durante las primeras categorías de especificación, evaluar soluciones e impacto, y proponer las reparaciones. Toma como referencia el prompt aplicado inmediatamente antes (`09-Editar-Agent-Rules-Documentacion-Examples-final`) y sus salidas, y manda volcar el análisis en `OUTPUTs/`.
- **Insumo de la revisión** en `PROMPTs/Feactures/10-Revision-SDD/INPUTs/Evaluacion-SDD.md`: evaluación escrita por el orquestador durante una corrida real del bootstrap SDD sobre `DEV/SelfHosted.Service.Core` —cuatro proyectos, validación de intake y Fase A completa, con `Master-Prompt.md` en v3.6—, en la que se perdieron cinco estados archivados. Reporta cuatro defectos y un vacío de fondo alrededor de la política de deprecación (D5) y del archivado en `_legacy/`, con evidencia citada y sin aplicar ninguna corrección.
- **Registro de la revisión** en `PROMPTs/Feactures/10-Revision-SDD/OUTPUTs/`, con los seis documentos que produjo la corrida sobre `/IA/IA.SDD/`:
  - `Revision-Hallazgos-SDD-v1.0.md` (versión 2.0): documento padre. Confirma los cinco hallazgos del insumo —dos de ellos subdimensionados: nueve clases de artefacto sin versión en lugar de dos, y cuatro convenciones de ruta de archivado en lugar de dos—, suma cuatro hallazgos propios y registra las ocho reparaciones R-1 a R-8 ya aplicadas sobre ocho archivos, con `Master-Prompt.md` de v3.6 a **v3.7** y el changelog del framework en `[3.2]`.
  - `Anexo-Barrido-Artefactos-Sin-Version-v1.0.md` (versión 1.1): ejecuta la verificación V-1 que el insumo dejó pendiente. Barre los dieciséis archivos de `SDD/Devs/Rules/` más el master-prompt y la superficie de entrada, encuentra **nueve clases** de artefacto que el framework manda emitir sin sufijo de versión, y re-ejecuta el barrido contra el framework ya reparado. Corrige de paso el «trece reglas» del insumo, número heredado de una fila de changelog anterior a `Maqueta-Rules.md` y `Deriva-Rules.md`.
  - `Alternativa-Eliminar-Legacy-v1.0.md` (versión 1.1): evalúa la vía de fondo —eliminar el concepto de `_legacy/` en lugar de repararlo, con lo que seis de los nueve hallazgos dejarían de existir— y la recomienda, condicionada a declarar el control de versiones del repositorio destino como prerrequisito duro (DEC-04). **El responsable del framework eligió la Opción A**, reparar y conservar; la evaluación queda registrada como disponible.
  - `Nota-Coherencia-Reparacion-Legacy-v1.0.md`: verificación de implantación de las ocho reparaciones, siguiendo el patrón de `Coherencia-Auditoria-Marco-v1.0.md`. Deja fuera de alcance las observaciones O-1 y O-3, con su motivo.
  - `Plan-Normalizacion-Versionado-v1.0.md`: plan para que el framework tenga **una sola lógica de versionado y archivado** dentro de cada plano, según el criterio que fijó el responsable del framework. Mide el incumplimiento actual en los dos planos —treinta y cuatro archivos con versión en la cabecera contra once con versión en el nombre, de los cuales cuatro mienten— y fija la regla única: un solo archivo por nombre lógico en la carpeta de trabajo, versión en la cabecera, sufijo solo en la copia archivada en `_legacy/`.
  - `Nota-Coherencia-Normalizacion-Versionado-v1.0.md` (versión 1.2): verificación de implantación del plan anterior, que llevó el changelog del framework de `[3.1]` a **`[4.0]`** absorbiendo `[3.2]`. Cubre las dos partes de la intervención —las siete etapas de normalización más una E5b abierta al descubrir un defecto preexistente, y las seis etapas F1 a F6 de reconciliación normativa que permiten al orquestador leer con qué versión del framework se generó un destino—, sobre cuarenta y un archivos, 1086 líneas agregadas y 829 eliminadas, con once renombres de historial preservado.
- **Nota de trabajo** en `Notas/2022607271949.md`, con su captura en `Notas/2022607271949/Captura-Panel-De-Verificaciones.png`: pendientes surgidos del último proyecto (`DEV/SelfHosted.Service.Core.Documentos`) —formar los ciclos de vida de los modelos y relacionarlos con el modelo de datos en el documento integrador, incluir ejemplos y modelos de datos en el anexo de maqueta web y tests, ofuscación de datos, tabla de contenido y revisión final del intake, caracterización de asistentes o wizards, y arneses en agentes para mejorar deriva y validación—. Primera carpeta `Notas/` del repositorio.

### Cambiado

- **Salidas del prompt 09 movidas a `PROMPTs/Feactures/09-Editar-Agent-Rules-Documentacion-Examples-final/OUTPUTs/`**: `Estado-Intervencion.md`, `Informe-Intervencion-v1.0.md` y las ocho notas de coherencia `Nota-Coherencia-E1.md` a `E8.md` pasan de la raíz de la carpeta del prompt a una subcarpeta `OUTPUTs/`, sin cambios de contenido, para unificar el criterio de `INPUTs/` y `OUTPUTs/` que usa el prompt 10.

## [1.9] - 2026-07-26

Actualización del registro de la intervención sobre `/IA/IA.SDD/`: el estado de entrega deja de ser «working tree sin commitear» y pasa a publicado sobre `main`.

### Cambiado

- **`PROMPTs/Feactures/09-Editar-Agent-Rules-Documentacion-Examples-final/Estado-Intervencion.md`**: el estado global sigue siendo **CERRADA** con las nueve etapas CONFORME, pero ya no anota los cambios como pendientes en el working tree. Registra que el responsable del framework levantó la restricción de no commitear y que la intervención se publicó sobre `main`.
- **`PROMPTs/Feactures/09-Editar-Agent-Rules-Documentacion-Examples-final/Informe-Intervencion-v1.0.md`**, §9 «Estado de entrega»: reescrito en la misma línea. Deja constancia de que la restricción del prompt se levantó al cierre tras la revisión del responsable, y reemplaza la nota sobre `SDD/Guides/SDD-Development-Guide.md` como untracked por el alcance real de la publicación, que incluye la eliminación de material histórico absorbido de la entrada `[3.1]` del changelog del framework: las carpetas `SDD/Devs/Reformulacion/` y `SDD/Devs/Intake/_legacy/`, seis archivos sin referencias entrantes preservados en el historial de git.

## [1.8] - 2026-07-26

Prompt nuevo de reordenamiento de las categorías 10 y 11 del framework SDD con todo el registro de su corrida, una carpeta `PROMPTs/Analisis/` nueva y la renumeración del prompt de log de agentes.

### Añadido

- **Prompt «Editar agent rules documentación examples final»** en `PROMPTs/Feactures/09-Editar-Agent-Rules-Documentacion-Examples-final/`: tool-prompt autocontenido (790 líneas) que interviene las reglas constructivas del framework SDD para intercambiar las categorías 10 y 11, redefinir `10-Examples` con doble arista (referencia de integración y arnés de autovalidación), redefinir `11-Documentacion` como cuerpo documental de entrega organizado por rol de intervención (integrador, mantenedor, operador) y destinado a un lector humano en primer contacto, incorporar el modelo de documentación viva en tres momentos con cadencia anclada al cierre de sprint, agregar tabla de contenido a las categorías 00 a 09, y dotar al framework de un README raíz como router y de una guía de desarrollo y extensibilidad. Diagnostica tres déficits —un solo rol de intervención modelado, ausencia de documento de primer contacto y criterios de calidad inaplicables en una fase pre-código— y segmenta la ejecución en ocho etapas con control de coherencia entre cada una.
- **Registro de la intervención** en la misma carpeta, con el material que produjo la corrida sobre `/IA/IA.SDD/`:
  - `Estado-Intervencion.md`: tablero de las nueve etapas ejecutadas (E0 a E8), todas con veredicto CONFORME, más las siete decisiones DEC-01 a DEC-07 tomadas por el responsable del framework. Fue el mecanismo de reanudación entre sesiones; queda **cerrada**, sin etapas ni decisiones pendientes.
  - `Informe-Intervencion-v1.0.md` (versión 1.1): informe de cierre. Veintiséis archivos tocados —veinticuatro modificados, uno renombrado (`Rules-Developer-Guide.md` a `Rules-Documentacion.md`, con `git mv`) y `SDD-Development-Guide.md` que pasa de 0 bytes a contenido—, framework en changelog 3.0. Registra la corrección de los tres déficits y el defecto que E8 destapó: E2 había dejado a `Rules-Calidad-Y-Pruebas.md` contradiciendo a `Deriva-Rules.md` sobre cuándo se emite la matriz de sensado.
  - `Nota-Coherencia-E1.md` a `Nota-Coherencia-E8.md`: verificación de implantación de cada etapa —renombrado estructural, categoría 10, categoría 11, documentación viva en el orquestador, navegabilidad de 00 a 09, superficie de entrada, cierre e E8 de cierre de decisiones abiertas—, siguiendo el patrón de `Coherencia-Auditoria-Marco-v1.0.md`. Las notas de las etapas cuyas decisiones se resolvieron después llevan la nota posterior de E8 que remite al cierre.
- **Prompt «Generación ayuda de usuario documentación»** en `PROMPTs/Analisis/01-Generacion-Ayuda-Usuario-Documentacion/`: primera carpeta bajo `PROMPTs/Analisis/`. Plantea determinar el mecanismo para generar la ayuda de usuario del framework a partir de la necesidad de registrar el historial de avance de los agentes, con la alternativa de un `Log.md` en `/<Repositorio-Destino>/SDD/Logs/` y la consideración de subagentes en paralelo escribiendo sobre el mismo fichero.

### Cambiado

- **`PROMPTs/Feactures/Crear-Log-File/` renombrada a `08-Crear-Log-File/`**, para entrar en la numeración del resto de los prompts de *features*. El registro de ejecución `OUTPUTs/00.md` se mueve sin cambios.
- **Prompt «Crear log file» (`08-Crear-Log-File/Crear-Log-File.md`) reescrito**: el objetivo deja de ser la guía *getting started* y pasa a ser el log de contexto y registro del flujo de trabajo, con los dos usos separados —reanudar en una sesión nueva desde el estado de interrupción, y darle al desarrollador un historial consultable de la actividad orquestada—. Las solicitudes incorporan la evaluación de alternativas y la propuesta concreta del `Log.md` en el repositorio destino, con la salvedad de la escritura concurrente de subagentes.

## [1.7] - 2026-07-25

Dos prompts nuevos en `PROMPTs/Feactures/`: la normalización de nombres de los archivos de reglas del framework SDD y el registro de historial de trabajo de los agentes.

### Añadido

- **Prompt «Crear refactorización rules file name»** en `PROMPTs/Feactures/07-Crear-Refactorizacion-Rules-File-Name/`: tool-prompt que encarga quitar el prefijo numérico a las doce reglas por categoría de `/IA/IA.SDD/SDD/Devs/Rules` (`00-Rules-Contexto.md` a `Rules-Contexto.md`, …, `11-Rules-Examples.md` a `Rules-Examples.md`), dejando intactas las cuatro meta-reglas (`Root-`, `Intake-`, `Maqueta-` y `Deriva-Rules.md`), y actualizar todas las referencias a esos nombres en los markdown de `/IA/IA.SDD` verificando la coherencia del resultado.
- **Prompt «Crear log file»** en `PROMPTs/Feactures/Crear-Log-File/`: plantea registrar el historial de avance de los agentes para poder retomar el flujo del prompt orquestador recuperando contexto y tareas de los subagentes, y para que el desarrollador del framework SDD pueda revisar decisiones y resultados de cada etapa. Incluye la tabla de referencias del caso `SAI.Service.Core` (intake, prompt integrador, prompt orquestador y ambos repositorios).
- **Registro de ejecución** en `PROMPTs/Feactures/Crear-Log-File/OUTPUTs/00.md`: documenta la corrida del renombrado de reglas en dos tandas — primero `/IA/IA.SDD` (20 markdown, ~200 ocurrencias, más tres arreglos de coherencia que el reemplazo mecánico no cubría: el placeholder `XX-Rules-<Categoria>.md`, el rango colapsado de `SDD-User-Guide.md:358` y la numeración que sí es semántica en títulos, carpetas y tablas de auditoría) y después `DEV/` (18 markdown de `SAI.Service.Core`, incluida la forma abreviada `05-Rules`, `08-Rules`, `10-Rules` expandida al nombre real de cada regla). Deja constancia del único archivo no tocado por estar bajo `PROMPTs/`.

## [1.6] - 2026-07-24

Consolidación de los prompts de guías de estudio en una única carpeta `PROMPTs/Guia-De-Estudio/` y reescritura del prompt «Crear guides» como tool-prompt reejecutable que genera la guía *getting started* del framework SDD.

### Añadido

- **Registro de ejecución** del prompt «Crear guides» en `PROMPTs/Feactures/06-Crear-Guides/Historial/00.md`: documenta la corrida que produjo la «SDD Getting Started Guide» (fuentes leídas, verificación de rutas del framework y contenido de la guía generada).

### Cambiado

- **Consolidación de `PROMPTs/Guias-Estudio/` en `PROMPTs/Guia-De-Estudio/`**: los prompts de generación de guías de estudio se unifican bajo una sola carpeta con nombre consistente (guiones y singular), y se quita la numeración `01-`. Se mueven sin cambios de contenido `Generacion-Documentacion-Informe-Despliegue/`, `Generacion-Documentacion-Tecnica/` (con su `Inputs/Tipos-De-Documentacion-Tecnica.md`), `Organizacion-Estilo-Patrones-Codigo/`, `Organizacion-Estilo-Rest-API/` y `Crear-Guia-Organizacion-Estilo-Patrones-Codigo/`. La carpeta `PROMPTs/Guias-Estudio/` desaparece.
- **Prompt «Crear guides» (`PROMPTs/Feactures/06-Crear-Guides/Crear-Guides.md`) reescrito** de una consulta suelta a un tool-prompt completo: describe el modelo de tres repositorios (framework, destino y documentación) y el flujo de seis pasos del proceso SDD, y encarga generar la **«SDD Getting Started Guide»** en `/IA/IA.SDD/SDD/Guides/SDD-Getting-Started-Guide.md`, con reglas, tabla de referencias y la regla de audiencia dual.
- **`PROMPTs/Feactures/02-Refactorizando-Repo-Nombre/Refactorizando-Repo-Nombre.md`**: la ruta de invocación apunta a la carpeta actual del prompt.
- **`PROMPTs/Feactures/05-Consultas-Sobre-Tabla-Contenido-Y-Valores/Consultas-Sobre-Tabla-Contenido-Y-Valores.md`**: limpieza menor de formato.

## [1.5] - 2026-07-23

Reorganización de `PROMPTs/` y `Analisis/` a un esquema de una carpeta por unidad de trabajo, más un análisis nuevo sobre el consumo de contexto del orquestador Generar-SDD y dos prompts nuevos.

### Añadido

- **Análisis «Optimizaciones del orquestador Generar-SDD»** en `Analisis/Analisis-Optimizaciones-Generar-SDD/`: evalúa la corrida real de generación de SDD sobre `DEV/SAI.Service.Core` contra el Master-Prompt v3.4 del framework, con foco en qué pasos conviene delegar a subagentes de contexto aislado para no inflar el contexto del orquestador. Distingue memoria de pesos y memoria de contexto, inventaria los mecanismos que el framework ya resuelve bien, señala dónde se infla el contexto en la práctica, prioriza los pasos a delegar, propone cómo medir la mejora y registra el doble rol del `Historial/` como artefacto de mejora. Es de solo lectura: las propuestas sobre el framework quedan como recomendación, no aplicadas.
- **Prompt «Extraer e incorporar perfil UX/UI al SDD»** en `PROMPTs/Feactures/01-Extraer-Incorporar-Perfil-UX-UI-A-SDD/`, con su registro de ejecución en `Hitory/01.Log.md`: reglas de diseño web genéricas, su especialización a Blazor *interactive server* con MudBlazor y la decisión de ubicarlas en `devs/references/design/` del template SDD.
- **Prompt «Crear guides»** en `PROMPTs/Feactures/06-Crear-Guides/`: guía rápida y de consulta para aplicar la metodología SDD sobre un proyecto nuevo, derivada de `SDD-User-Guide.md`.
- **Captura del panel de verificaciones** en `Analisis/Estudio-UX-UI-Historias/Captura-Panel-De-Verificaciones.png`.
- **`README.md` placeholder** en `Guias-de-Estudios/`, `PROMPTs/` y `PROMPTs/Guia-De-Estudio/`, como índices a completar.

### Cambiado

- **Una carpeta por prompt en `PROMPTs/Feactures/`**: cada prompt pasa de archivo suelto a carpeta numerada con el `.md` adentro y su `History/` cuando corresponde. La numeración se corre para dar el lugar 01 al prompt de perfil UX/UI: `Refactorizando-Repo-Nombre` a `02-`, `Extraer-Caracteristicas-De-Proyecto-Web` a `03-`, `Agregar-Feature-SDD-Fase-Validacion-UX-UI` a `04-Agregar-Feature-SDD-Maqueta-Validacion-UX-UI` y `Consultas-Sobre-Tabla-Contenido-Y-Valores` a `05-`.
- **`PROMPTs/Consultas/01.md`** movido a `PROMPTs/Guia-De-Estudio/01-Crear-Guia-Organizacion-Estilo-Patrones-Codigo/Crear-Guia-Organizacion-Estilo-Patrones-Codigo.md`: la consulta sobre organización de código se encuadra como prompt de generación de guía de estudio, no como consulta suelta. La carpeta `PROMPTs/Consultas/` desaparece.
- **`PROMPTs/Promptings/Crear-Prompt-adecuado.md`** renombrado a `Crear-Prompt-Adecuado.md`, con bloque *Overview* en la invocación.
- **`Analisis/IA.Documentacion/`** renombrada a `Analisis/IA-Documentacion/` y **`Analisis/git-workflow-agente.md`** movido a `Analisis/Git-Workflow-Agente/Git-Workflow-Agente.md`, para unificar el criterio de nombres con guiones y una carpeta por tema.
- **Restricciones uniformes en los prompts**: los prompts de *features* incorporan «No inventar información; toda afirmación debe estar respaldada por evidencia verificable» y separadores de sección consistentes.

## [1.4] - 2026-07-21

Guía de estudio nueva sobre cómo redactar un informe de solución (arquitectura, despliegue y requisitos), más el prompt que la origina.

### Añadido

- **Guía de estudio «Informe de solución: arquitectura, despliegue y requisitos»** en `Guias-de-Estudios/Documentacion-Informe-Despliegue/`: 30 documentos, ~5.350 líneas y 23 diagramas Mermaid. Enseña a escribir un informe que describe una solución de software en términos de arquitectura, despliegue y resolución de requisitos funcionales y no funcionales, con marco de referencia (escenarios, contextos y actores), mapa conceptual, cinco familias temáticas (naturaleza del informe, arquitectura, despliegue, requisitos y redacción) y anexos (plantilla, glosario, lista de verificación, referencias y pendientes). Todos los ejemplos usan un dominio de gestión de audiencias distribuido en el borde, sobre .NET 10, ASP.NET Core, Blazor, Worker Services y PostgreSQL.
- **Prompt de generación** en `PROMPTs/Guias-Estudio/Generacion-Documentacion-Informe-Despliegue/Generacion-Documentacion-Informe-Despliegue.md` que origina la guía.

## [1.3] - 2026-07-20

Material nuevo sobre el flujo de trabajo Git del agente y un prompt de consultas sobre el documento intake, más ajustes menores en prompts existentes.

### Añadido

- **Flujo de trabajo Git para el agente** en `Analisis/git-workflow-agente.md`: define dos modalidades (feature branch + PR con merge manual, y trabajo directo sobre `main`), convenciones fijas de rama base, changelog, nombres de rama y mensajes *Conventional Commits*.
- **Prompt de consultas sobre el documento intake** en `PROMPTs/Feactures/04-Consultas-Sobre-Tabla-Contenido-Y-Valores.md`: consultas sobre valores de medición/ejemplos y tabla de contenido en el documento intake generado por el framework SDD.

### Cambiado

- **`PROMPTs/Feactures/01-Refactorizando-Repo-Nombre.md`**: la carpeta `Docs` de la estructura de ejemplo ahora usa `.gitkeep` en lugar de un comentario sobre especificaciones generadas.
- **`PROMPTs/Feactures/03-Agregar-Feature-SDD-Fase-Validacion-UX-UI.md`**: separadores de sección para mejorar la legibilidad.

## [1.2] - 2026-07-20

Documento de análisis integral que responde una consulta sobre organización de código y la reorganización de la consulta que lo origina.

### Añadido

- **Análisis integral «Del modelo por tipo técnico a las capas actuales»** en `Guias-de-Estudios/Organizacion-Estilo-Patrones-Codigo/61-Analisis-Integral/Analisis-Integral.md`: ~590 líneas y 2 diagramas Mermaid. Responde, sobre el dominio de turnos, qué ocurrió con los patrones DAO, MVC, Repository, DTO y Entity y cómo se re-encuadran en el modelo de capas actual.
- **Prompt de consulta** en `PROMPTs/Consultas/01.md` que origina el análisis integral.

### Eliminado

- `CONSULTAS/01.md` y la carpeta `CONSULTAS/`: la consulta se reformuló como prompt en `PROMPTs/Consultas/`.

## [1.1] - 2026-07-20

Dos guías de estudio nuevas sobre organización de código y APIs REST, más la reorganización de los prompts y material de análisis nuevo.

### Añadido

- **Guía de estudio «Organización, estilo y patrones de código»** en `Guias-de-Estudios/Organizacion-Estilo-Patrones-Codigo/`: 37 documentos, ~10.700 líneas y 23 diagramas Mermaid. Cubre arquitectura de servicios (monolito, monolito modular y microservicios), organización de soluciones .NET, estilo de codificación y patrones de acceso a datos y de endpoint, con marco de referencia, mapa conceptual y anexos.
- **Guía de estudio «Organización, estilo y REST API»** en `Guias-de-Estudios/Organizacion-Estilo-Rest-API/`: 56 documentos, ~21.300 líneas y 61 diagramas Mermaid. Cubre fundamentos REST, diseño de recursos, semántica HTTP, contratos y representaciones, evolución y versionado, especificación con OpenAPI, seguridad y robustez, implementación en .NET y guías de la industria (Google AIP, Microsoft/Azure, Zalando).
- **Análisis de características UI/IX** en `Analisis/Caracteristicas-UI-IX/Caracteristicas.md`.
- **Consultas** en `CONSULTAS/`.
- **Prompts nuevos** en `PROMPTs/Feactures/`: extracción de características de proyecto web y feature SDD de validación UX/UI; y los prompts que originan las dos guías de estudio nuevas en `PROMPTs/Guias-Estudio/`.

### Cambiado

- **Reorganización de `PROMPTs/`** en subcarpetas temáticas:
  - `Crear-Prompt-adecuado.md` movido a `PROMPTs/Promptings/`.
  - `Refactorizando-Repo-Nombre.md` movido a `PROMPTs/Feactures/01-Refactorizando-Repo-Nombre.md`.
  - `Crear-Guia-de-Estudio-Documentacion-Tecnica.md` y su input `Tipos-De-Documentacion-Tecnica.md` movidos a `PROMPTs/Guias-Estudio/Generacion-Documentacion-Tecnica/`.

### Eliminado

- `Analisis/IA.Documentacion/Contexto.md`.

## [1.0] - 2026-07-18

Primera versión del contenido del repositorio: la guía de estudio sobre documentación técnica, más el material de análisis y los prompts que la originaron.

### Añadido

- **Guía de estudio «Documentación técnica»** en `Guias-de-Estudios/Documentacion-Tecnica/`: 59 documentos, ~20.700 líneas y 87 diagramas Mermaid. Generada con el Profile `Study-Guide-Documentation` del Prompt Framework a partir del prompt `PROMPTs/Crear-Guia-de-Estudio-Documentacion-Tecnica.md`.
  - **Marco de referencia** (`00-Marco-de-Referencia/`): cuatro escenarios (`ESC-1` desarrollo nuevo, `ESC-2` migración, `ESC-3` evaluación con acceso al código, `ESC-4` evaluación solo desde afuera), tres contextos (`CTX-1` web, `CTX-2` backend, `CTX-3` fullstack), diez actores (`ACT-01`..`ACT-10`, incluido el agente de IA) y las convenciones de identificadores, frontmatter y estilo.
  - **Mapa conceptual** (`01-Mapa-Conceptual/`): tablas de entrada por escenario, por contexto y por artefacto, más los cruces escenario × familia y actor × familia.
  - **Siete familias documentales** (`10-Vision/` a `70-Usuarios/`): un documento por cada una de las 28 filas de la tabla del catálogo de tipos, de Vision Document a RFC. Los nueve artefactos que el catálogo menciona solo dentro de su agrupación práctica —casos de uso, reglas de negocio, coding standards, git workflow, CI/CD, disaster recovery, tutoriales, FAQ y guías rápidas— se tratan como secciones dentro del documento de su familia, no como documentos propios.
  - **Métodos ágiles** (`80-Metodos-Agiles/`): manifiesto y documentación, Scrum, Kanban, Canvas y comparativa, tratados desde el ángulo de qué documentación produce, exige y elimina cada método.
  - **Modelos de arquitectura** (`90-Modelos-de-Arquitectura/`): cliente-servidor, capas, monolítico, hexagonal, microservicios y comparativa, cada uno vinculado con la documentación que exige y con el mismo sistema de ejemplo modelado bajo los cinco.
  - **Transversales** (`95-Transversales/`): UX, UI y flujo de usuario; Spec-Driven Development aplicado a la generación de código asistida por IA.
  - **Anexos** (`99-Anexos/`): glosario con términos canónicos y alias, informe de revisión de consistencia y registro de pendientes.
- **Material de análisis** en `Analisis/IA.Documentacion/`: SAD, ADR de diseño de software, manual técnico y contexto.
- **Prompts** en `PROMPTs/`, incluido el catálogo de tipos de documentación técnica que sirve de fuente a la guía.

### Decisiones de criterio propio

Registradas en `Guias-de-Estudios/Documentacion-Tecnica/99-Anexos/Revision-de-Consistencia.md`. Las tres que más condicionan el resultado:

- La tabla del catálogo de tipos tiene **28 filas**, no 27 como indicaba el enunciado del prompt. Se produjo un documento por fila.
- **Test Plan, Test Cases, Release Notes y Change Log** se ubican en la familia de desarrollo, que el catálogo no asigna a ninguna de sus siete agrupaciones.
- Se incorporó **`ACT-10`, el agente de IA**, al marco de actores, porque la guía incluye un documento sobre generación asistida y la práctica no se puede tratar sin ubicar a quien la ejecuta.

El dominio recurrente de los ejemplos es un sistema de reserva de salas, con .NET y C#, Blazor *interactive server*, ASP.NET MVC y .NET MAUI con MVVM como tecnologías de referencia. Se excluyó el caso HomeHub del catálogo por indicación del prompt.

### Corregido

Revisión de la guía completa contra el mapa conceptual, antes de esta versión:

- **Once identificadores divergentes** en las fronteras entre familias, unificados contra la lista canónica de `Convenciones.md`: `FAM-DES`, `FAM-QA` y `FAM-PRUEBAS` a `FAM-DEV`; `FAM-OPS` y `FAM-OPERATIVA` a `FAM-OPE`; `FAM-SEC` a `FAM-ARQ`; `FAM-METODOS` a `MET-INDICE`; `DOC-TEST-PLAN` a `DOC-TESTPLAN`; `DOC-TEST-CASES` a `DOC-TESTCASES`; `DOC-MODELO-DATOS` a `DOC-DATOS`; `DOC-DEV-GUIDE` a `DOC-DEVGUIDE`.
- **Contradicción sobre el número mínimo de estados de pantalla**: el marco fijaba cuatro y el documento de UX exigía seis sin declarar la relación. Resuelta por jerarquía, no por unificación.
- **Tres enlaces rotos o mal dirigidos**: uno apuntaba a `Casos-de-Uso.md`, que por diseño no existe; otro remitía el razonamiento de `ADR-012` al HLD en lugar del documento de decisiones.
- **Cuatro documentos temáticos sin ningún diagrama**: Release Notes, Change Log, API Specification y Operations Guide.
- **Ambigüedad del glosario canónico**: seis documentos contienen una sección de glosario. El anexo ahora distingue el glosario de la guía, que define términos del oficio, de los glosarios de plantilla, cuyo alcance es el vocabulario del sistema documentado.

### Pendiente

Registrado en `Guias-de-Estudios/Documentacion-Tecnica/99-Anexos/Pendientes.md`: siete temas no cubiertos (`HUE-01`..`HUE-07`), dos verificaciones contra fuente (`VER-01`, `VER-02`) y dos convenciones menores sin fijar (`CNV-01`, `CNV-02`). Ninguno bloquea el uso de la guía.
