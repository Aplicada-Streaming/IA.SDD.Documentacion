# Nota de coherencia — E5 Navegabilidad de las categorías 00 a 09

**Proyecto:** Framework SDD
**Documento:** Nota-Coherencia-E5.md
**Versión:** 1.0
**Estado:** Vigente
**Fecha:** 2026-07-26
**Autor:** Reformulación SDD

---

## 1. Alcance

Verificación de implantación de la etapa E5, que ejecuta la solicitud S6: incorporar la exigencia de tabla de contenido a los diez archivos de reglas de las categorías conservadas, en §4 estructura de redacción y en §6 criterios de aceptación.

La exigencia recae sobre los **documentos que esas categorías generan** en el repositorio destino, no sobre los archivos de reglas en sí. Es la distinción que hay que tener presente al leer los cambios: lo que se modifica es la regla; lo que va a llevar tabla de contenido es su salida.

E5 es la etapa de menor superficie de la intervención y la única cuyo alcance el prompt acota de forma explícita y negativa: «No introducir ningún otro cambio en estas diez reglas. En particular, no aumentar su carga narrativa ni agregarles artefactos».

## 2. Inventario de archivos

| Archivo | Versión |
| --- | --- |
| `SDD/Devs/Rules/Rules-Contexto.md` | 1.4 → 1.5 |
| `SDD/Devs/Rules/Rules-Necesidades-Negocio.md` | 1.3 → 1.4 |
| `SDD/Devs/Rules/Rules-Especificacion-Funcional.md` | 1.2 → 1.3 |
| `SDD/Devs/Rules/Rules-UX-UI-DX.md` | 1.7 → 1.8 |
| `SDD/Devs/Rules/Rules-Prompts-AI.md` | 1.3 → 1.4 |
| `SDD/Devs/Rules/Rules-Arquitectura-Tecnica.md` | 1.3 → 1.4 |
| `SDD/Devs/Rules/Rules-Backlog-Tecnico.md` | 1.2 → 1.3 |
| `SDD/Devs/Rules/Rules-Plan-Sprint.md` | 1.3 → 1.4 |
| `SDD/Devs/Rules/Rules-Calidad-Y-Pruebas.md` | 1.5 → 1.6 |
| `SDD/Devs/Rules/Rules-Devops.md` | 1.5 → 1.6 |

Diez archivos, tres cambios idénticos en cada uno: el párrafo de exigencia al final de §4.1, el criterio de aceptación al final de §6, y la fila de control de cambios. Ninguna creación, ningún renombre, ninguna eliminación.

## 3. El cambio aplicado

En §4.1, al cierre de la cabecera obligatoria y antes de §4.2:

> **Tabla de contenido.** Todo documento generado que supere las tres secciones de primer nivel incluye una tabla de contenido inmediatamente después de la cabecera de metadatos, con enlaces ancla a cada sección de primer y de segundo nivel. La tabla de contenido no cuenta como sección de contenido ni altera la estructura obligatoria del documento: se ubica entre la cabecera y la primera sección, y las secciones obligatorias siguen siendo las que declara §4.2. Los documentos breves —fichas de una sola sección, entradas de índice— quedan exceptuados.
>
> El ajuste es de navegabilidad. Estos documentos los lee principalmente un agente de IA que recorre la cadena de especificación acumulando contexto, y para ese lector la tabla de contenido es indiferente. Existe para el agente humano que entra a consultar un punto concreto sin haber leído el documento entero.

En §6, como último criterio de la lista:

> - [ ] Todo documento con más de tres secciones de primer nivel incluye tabla de contenido inmediatamente después de la cabecera, con enlaces ancla a las secciones de primer y de segundo nivel. Los documentos breves quedan exceptuados.

**Sobre la ubicación en §4.1 y no en una subsección propia.** La tabla de contenido va físicamente entre la cabecera y la primera sección del documento generado, así que la regla que la exige pertenece a la sección que gobierna la cabecera. Abrirle una subsección propia habría obligado a renumerar de §4.2 en adelante en los diez archivos, con el riesgo de romper referencias por un cambio puramente cosmético. La renumeración de `Rules-Plan-Sprint.md` en E4 fue inevitable; esta no lo era.

**Sobre el segundo párrafo.** Podría discutirse si agrega carga narrativa, que es lo que la restricción prohíbe. Lo incluí porque sin él la exigencia parece contradecir la decisión 4 del Contexto, que dice que estas categorías conservan su carga documental por ser leídas principalmente por la IA. El párrafo declara por qué la tabla de contenido no es una excepción a esa decisión sino un ajuste ortogonal. Son dos oraciones y no agrega artefactos ni secciones. **Se reporta** por si el responsable prefiere quitarlo.

## 4. Lista de comprobación

| # | Comprobación | Resultado | Evidencia |
| --- | --- | --- | --- |
| 1 | Invariantes D1–D9 intactas | Cumple | D1 español rioplatense técnico. D2 UTF-8 sin BOM, LF, cero saltos triples de línea en los diez archivos. D3 y D4: sin cambios de nombres. D5: los diez suben versión in situ. D6: control de cambios actualizado en los diez. D7: ningún literal de dominio. D8: sin tocar las tablas de gating. D9: la exigencia no introduce afirmaciones sobre el estado del sistema |
| 2 | Autosuficiencia: cero referencias fuera de `/IA/IA.SDD/` | Cumple | Las once cadenas dan cero. Cero URLs nuevas |
| 3 | Referencias internas: todo lo citado existe | Cumple | La única referencia introducida es a §4.2 del propio archivo, que existe en los diez |
| 4 | Vocabulario normalizado | Cumple | El texto agregado usa «agente humano» y «agente de IA», calificados según la decisión 5 del Contexto |
| 5 | Sin contradicción con etapas anteriores | Cumple | **La frontera con la categoría 11 que E3 escribió en 05, 08 y 09 quedó intacta**, verificada una por una. E5 tocó §4.1 y §6 de esos tres archivos; la frontera vive en §0 y no se retocó, tal como DEC-05 dispone |
| 6 | Control de cambios actualizado | Cumple | Una fila por archivo, fechada 2026-07-26 |
| 7 | El caso degenerado sigue produciendo el layout aplanado | Cumple | E5 no toca layout ni rutas |
| 8 | Nada fuera del alcance de la etapa fue modificado | Cumple | Tres bloques por archivo, siempre los mismos. Sin artefactos nuevos: se verificó que ninguna tabla maestra de las diez reglas incorporó filas. Sin cambios de gating, de estructura obligatoria ni de anti-patrones |

## 5. Observaciones

1. **La exigencia recae sobre la salida, no sobre las reglas.** Los diez archivos de reglas siguen sin tabla de contenido propia, y eso es correcto: S6 pide que la lleven los documentos generados. `Rules-Documentacion.md` sí la tiene, pero por su tamaño y porque su propio §4.7 la admite cuando el tamaño lo justifica, no por S6.
2. **El umbral de tres secciones queda sin verificación automática.** Un auditor tiene que contar las secciones de primer nivel del documento generado para decidir si la exigencia aplica. Es la misma clase de verificación manual que el framework ya usa en otros criterios de §6, así que no introduce un problema nuevo, pero conviene saber que no es automatizable con una búsqueda de cadena.
3. **`Rules-Plan-Sprint.md` fue tocado por E4 y por E5.** E4 escribió §4.5 (Definition of Done) y E5 §4.1 y §6. Secciones disjuntas, sin conflicto. Es el mismo patrón que DEC-05 estableció para 05, 08 y 09.
4. **Quedan sin tabla de contenido los archivos de reglas transversales** —`Root-Rules.md`, `Intake-Rules.md`, `Maqueta-Rules.md`, `Deriva-Rules.md`— y las categorías 10 y 11, que S6 no menciona. Para 10 y 11 es deliberado: sus reglas propias ya gobiernan el formato de su salida. Para las cuatro transversales, S6 simplemente no las alcanza. No es un hueco de esta etapa, pero es un lugar donde el framework quedó asimétrico.

## 6. Veredicto

**CONFORME.**

Los diez archivos de reglas de las categorías conservadas exigen ahora tabla de contenido en los documentos que generan, con el umbral, el alcance de anclas y la excepción para documentos breves que S6 fija, declarada tanto en la estructura de redacción como en los criterios de aceptación. El alcance negativo que la solicitud impone se respetó: no se agregaron artefactos, no se alteró ninguna estructura obligatoria y no se tocó ningún gating.

Las cuatro observaciones son informativas. La única que admite decisión es el segundo párrafo del texto insertado, que se reporta por si se lo considera carga narrativa innecesaria.

Corresponde detenerse y esperar confirmación humana antes de arrancar E6.

## 7. Control de cambios

| Versión | Fecha | Cambios | Autor |
| --- | --- | --- | --- |
| 1.0 | 2026-07-26 | Nota de coherencia inicial de la etapa E5: inventario de los diez archivos con su cambio de versión, texto exacto aplicado en §4.1 y en §6, racional de la ubicación elegida, lista de comprobación de ocho puntos con verificación D1–D9 y verificación explícita de que la frontera escrita en E3 quedó intacta, cuatro observaciones y veredicto CONFORME. | Reformulación SDD |
