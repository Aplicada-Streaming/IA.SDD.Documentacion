# Changelog

Todos los cambios relevantes de este repositorio (`IA.SDD.Documentacion`) se documentan acá.
Formato basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/).

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
