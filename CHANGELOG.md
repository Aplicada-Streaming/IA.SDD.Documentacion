# Changelog

Todos los cambios relevantes de este repositorio (`IA.SDD.Documentacion`) se documentan acá.
Formato basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/).

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
