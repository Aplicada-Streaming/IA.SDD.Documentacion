

**Aplicado** el 2026-07-29. El framework quedó publicado en la **6.0** (`IA.SDD`, entrada `[6.0]` del changelog), con la capacidad de migración normativa completa: `Migracion-Rules.md` 1.0 como decimoctava regla y sexta transversal, `Master-Prompt-Migracion.md` 1.1 con sus fases M0 a M6, y `PROMPT-Agente-Migracion-SDD.md` 1.0 como prompt de entrada. La nota de coherencia es `SDD/Devs/Guides/Coherencia-Migracion.md`.

Las cuatro decisiones abiertas se resolvieron antes de tocar ningún archivo, todas por la recomendación que este documento proponía: **D-2** minor (5.1 a 5.2), **D-3** invocación nueva, **D-5** sí con clasificación degradada, **D-6** sí con la procedencia intacta y el estado parcial declarado en el informe, sin marca adicional en el manifiesto. El plan de `INPUTs/` quedó en 1.2 con las resoluciones y con las tres correcciones que la ejecución detectó en él.

Lo que quedó afuera, declarado: la emisión del plan sobre un destino real (la prueba de humo se corrió en lectura, porque el tool-prompt prohíbe escribir en un destino), H4 por decisión D-4, y una observación abierta no bloqueante sobre celdas de verificación de dos notas de coherencia anteriores.

Lo que sigue abajo es el material de decisión tal como se presentó, conservado como registro.

---

# Estado verificado antes de decidir

  - Framework vigente: conjunto 5.1 (CHANGELOG.md), Master-Prompt.md 5.1 (línea 4).
  - _legacy/ contiene una sola subcarpeta: 4.1/. Confirma el fundamento 4 de §3.1 del plan.
  - Observación (no es un cambio, se reporta): el plan §6 y §7.1 declaran que F4 sube SDD-Development-Guide.md de 1.3 a 1.4, pero la guía ya está en 1.4 (línea 6, fila del 2026-07-29). El bump real de F4 será 1.4 → 1.5. Lo corrijo en el plan al ejecutar E1, salvo indicación contraria.

  ---

  # D-2 — ¿F3 sube minor o major?
Recomendación: minor (5.1 → 5.2).

Master-Prompt.md §16 (líneas 961-963) reserva major para exactamente cinco casos: principio de delegación, flujo plan-then-confirm, conjunto D8, insumos obligatorios, cardinalidad de generación. F3 no toca ninguno: tolera nombres legados en §2 paso 1, nombra el instrumento en §2.1 y agrega un caso de escritura a §13. La misma sección clasifica «cambio en el flujo de §7 o en la mecánica de §8» como minor.

El criterio sustantivo de §VI.1 apunta igual: un destino generado con 5.1 sigue cumpliendo 5.2 — nada de lo que F3 agrega invalida documentación emitida.

El contra-argumento honesto: §13 se titula «Reglas de no-modificación del intake» y su línea 856 declara que toda escritura fuera del flujo «es un error de orquestación y dispara abort». Agregar un tercer caso relaja una prohibición dura. Quien considere que relajar una prohibición es incompatible con minor tiene base para pedir major.

  # D-3 — ¿La salida A encadena automáticamente con la migración?

  Recomendación: invocación nueva, sin encadenamiento.

  §2.1 línea 147 ya declara, sobre la salida A: «el orquestador vuelve a detenerse: ejecutar el plan es una decisión aparte». Y sus prohibiciones (líneas 155-157) incluyen «no elegir salida por cuenta propia». Encadenar debilitaría una detención existente y obligaría a una cuarta salida; con invocación nueva, §2.1 solo necesita nombrar el instrumento de la salida que ya tiene. Es el cambio más chico que resuelve el problema, y es lo que mantiene F3 en el terreno de minor (ver D-2).

  # D-5 — ¿La migración alcanza destinos sin procedencia declarada?
  
  Recomendación: sí, con clasificación degradada a «revisar todo».

  Hoy la fila «Sin procedencia» de §2.1 (línea 103) solo ofrece regenerar o abortar. Bajo A2 el estado objetivo está declarado en las reglas vigentes, así que migrar no requiere conocer el origen: lo único que se pierde es la capacidad de discriminar entre «regenerar» y «sin cambio», y todo pasa a «revisar». Es degradación de precisión, no pérdida de capacidad (§3.1 fundamento 4). Excluirlos dejaría fuera precisamente a los destinos más viejos, que son los que más necesitan la capacidad.

  Costo a declarar: el plan de migración de un destino sin procedencia enumera todos los documentos, sin priorización. El volumen se absorbe con el orden D6 y las detenciones por fase de M4, no se elimina.

  # D-6 — ¿Se admite migración parcial como estado final?
Recomendación: sí, con la procedencia intacta y el estado parcial declarado en el informe.

Es lo que D9 exige: una procedencia que declara la versión vigente sobre un árbol migrado a medias es una afirmación falsa sobre el estado del sistema. Por eso M5 es una fase condicional y no un efecto colateral. Prohibir la parcialidad sería incoherente con un orquestador que se detiene a pedir confirmación en cada fase: cualquier «no confirmo» dejaría el destino en un estado que la propia regla declara inválido, sin salida legítima.

Hay una variante más fuerte que conviene considerar: además del informe, marcar el estado parcial en el manifiesto —donde vive la procedencia—, para que la corrida siguiente del orquestador de generación lo vea sin tener que leer Audit/. Cuesta una fila más en F1 y cierra el agujero de que el informe sea el único registro.