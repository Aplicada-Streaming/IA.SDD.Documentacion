# Tool-Prompt — Fix orquestador: migración normativa de intake, manifiesto y documentos generados

> **Invocación**: `Lee y ejecuta /IA/IA.SDD.Documentacion/PROMPTs/Features/13-Ejecutar-Fix-Orquestador-Sobre-Intake/02-Ejecutar-Fix-Orquestador-Sobre-Intake.md`
>
> **Overview**: Dotar al `Framework SDD` de la capacidad de migrar un destino existente a la versión vigente, incluyendo la estructura del intake y del manifiesto.
>
> **Repositorio a intervenir**: `/IA/IA.SDD`. Versión vigente del conjunto al momento de escribir este prompt: 5.1.
>
> **Resultado esperado**: `Framework SDD` publicado en la versión 6.0, con la capacidad de migración normativa completa, sus prerrequisitos aplicados, su nota de coherencia emitida y su entrada de `CHANGELOG.md`.

---

## Contexto

  Leer `/IA/IA.SDD/README.md`, es la superficie de entrada del `Framework SDD`: modelo de tres repositorios, anatomía, matriz de ruteo por intención, mapa de las doce categorías, invariantes globales D1 a D9 y reglas de intervención sobre el framework.

  Leer `/IA/IA.SDD.Documentacion/PROMPTs/Features/13-Ejecutar-Fix-Orquestador-Sobre-Intake/INPUTs/Fix-Orquestador-Sobre-Intake.md`, es **el plan de esta intervención**. Contiene la evaluación verificada del estado actual, las cinco decisiones de diseño con su fundamento, la arquitectura propuesta, los cinco prerrequisitos, el inventario del renombre léxico, la segmentación en etapas, los criterios de aceptación y las decisiones abiertas. Es el insumo normativo de este prompt: lo que este prompt pide es ejecutarlo.

  Leer `/IA/IA.SDD/SDD/Guides/SDD-Development-Guide.md`, es el procedimiento de intervención sobre el framework: cómo se versiona un archivo de reglas (§VI.1), qué se registra en el control de cambios (§VI.2), cómo se verifica la coherencia después de un cambio (§VI.3), qué hacer cuando un cambio obliga a regenerar documentación ya emitida (§VI.4) y cómo se versiona el framework como conjunto (§VI.5).

---

## Objetivo

  Ejecutar el plan: dotar al `Framework SDD` de la capacidad de **migración normativa** de un destino generado con una versión anterior, con alcance sobre los dos documentos de entrada —intake y manifiesto— y sobre todos los documentos de especificación ya generados, y con la última versión del framework como principio rector de la migración.

---

## Solicitudes

  1. Leer los tres archivos de la sección **Contexto**. El plan es la especificación de lo que hay que hacer; este prompt no la duplica.

  2. Resolver las decisiones abiertas del §10 del plan (D-2, D-3, D-5 y D-6) antes de tocar ningún archivo. Cada una tiene su recomendación: presentarlas juntas, con la recomendación y su fundamento, y **esperar respuesta**. No asumir la recomendación por defecto.

  3. Ejecutar la etapa **E0** del §8 del plan antes de la primera modificación: snapshot del conjunto normativo 5.1 completo en `/IA/IA.SDD/_legacy/5.1/`, verificado con `diff -r`. Es la condición que hace reversible todo lo demás y no se saltea.

  4. Ejecutar las etapas **E1 a E5** del §8 del plan, **una por vez**. Al cerrar cada etapa: presentar qué se hizo, emitir su nota de coherencia según el patrón de `/IA/IA.SDD/SDD/Devs/Guides/Coherencia-Auditoria-Marco.md` y **esperar confirmación** antes de arrancar la siguiente. Cada etapa tiene que dejar el framework consistente y utilizable aunque la intervención se abandone ahí.

  5. Al aplicar el renombre léxico de la etapa E4, usar el inventario ya clasificado del §6.1 del plan: dieciocho ocurrencias de «adecua\*» en cuatro grupos, de las cuales se sustituyen siete. Verificar el inventario contra el árbol antes de sustituir, por si cambió; corregirlo en el plan si difiere. Al cerrar, ejecutar la **verificación negativa** que exige `Vocabulario-Rules.md` §9.5 y declarar cuántas ocurrencias se revisaron y cuántas se cambiaron.

  6. Revisar los documentos de `/IA/IA.SDD/SDD/Guides` contra el estado del framework al cerrar la intervención, no solo contra los cambios de este plan: la guía de arranque, la de usuario y la de desarrollo declaran fases, conteos de reglas y árboles que esta intervención modifica. Aplicar las actualizaciones siguiendo las reglas documentales del `Framework SDD`.

  7. Verificar uno por uno los criterios de aceptación del §9 del plan y reportar el resultado de cada uno con su evidencia. Los que no se cumplan se declaran como tales; no se dan por cumplidos.

  8. Publicar la versión: entrada `[6.0]` en `/IA/IA.SDD/CHANGELOG.md` con su bloque «Impacto sobre destinos existentes», y nota de coherencia consolidada de la intervención en `/IA/IA.SDD/SDD/Devs/Guides/Coherencia-Migracion.md`.

  9. Registrar en el §10 y en el control de cambios del plan las decisiones que se hayan resuelto durante la ejecución, para que el documento quede reflejando lo que efectivamente se decidió.

---

## Reglas

- No inventar información.
- Toda afirmación debe estar respaldada por evidencia verificable.
- Ser fiel a la estructura constructiva del `Framework SDD`.
- **Prohibida la sustitución global de cadena** para el renombre léxico (`Vocabulario-Rules.md` §9.5). Enumerar, clasificar por sentido, sustituir solo lo que cambia de referente y verificar con un barrido negativo. La regla existe porque la intervención 5.0 del framework produjo cuatro clases de daño por ese método.
- **No reescribir filas ya escritas de ningún control de cambios ni entrada de `CHANGELOG.md`** (`SDD-Development-Guide.md` §VI.2), aunque nombren artefactos que esta intervención renombra. Las filas viejas siguen nombrando lo viejo; la fila nueva declara el renombre.
- **No modificar nada dentro de `/IA/IA.SDD/_legacy/`.** Lo archivado es intocable: es lo que permite reconstruir con qué reglas exactas se generó un destino.
- **No modificar ninguna invariante D1 a D9** sin decisión explícita del responsable y nota de coherencia propia. Es el cambio de mayor impacto del framework.
- **Preservar la autosuficiencia**: ningún archivo de `/IA/IA.SDD` referencia otro repositorio. Los estándares de industria se nombran, no se enlazan.
- **Un descubrimiento no habilita un cambio** (`SDD-Development-Guide.md` §VI.3). Lo que aparezca durante una etapa y exigiría modificar una etapa ya cerrada se registra como observación, se reporta y se espera decisión.
- No escribir en el repositorio de un destino real. Esta intervención toca el framework; los destinos se migran después, con el orquestador que esta intervención crea.
