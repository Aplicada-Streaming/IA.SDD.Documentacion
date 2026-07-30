
# Aplicado.

# Resumen

  > Esto va a generar un set de reglas especifico para todos los  agentes

# Update Todos

  Bloque 0.1 — restituir «resolución» en las 30 ocurrencias de «reproducto»

  Bloque 0.2 — restituir la cabecera «| Anti-patrón | Problema | Solución |» en 23 tablas

  Bloque 0.3 — restituir «solución» en la prosa de negocio que R2 autoriza

  Bloque 0.4 — fila 5.0 en Master-Prompt §16 y alineación cabecera/control-de-cambios

  Bloque 0.5 — registrar la 5.0 en los ~20 archivos que la sufrieron sin declararla

  Bloque 0.6 — quitar la línea «Nivel de aplicación» duplicada de Rules-Arquitectura-Tecnica

  Bloque 1 — criterio de desambiguación léxica en Vocabulario-Rules + cableado en Master-Prompt §8, §10, §15

  Bloque 2 — Glosario-Funcional como artefacto de 02 y criterio de glosario en las 13 reglas

  Bloque 3 — poner los tres guides al día contra 5.0

  Registro — entrada 5.1 en CHANGELOG, nota de coherencia y versiones

  Verificación final — las seis comprobaciones del plan

  ---

# Lo que se aplicó
Bloque 0 — la migración 5.0 estaba rota, y en más formas de las que había detectado antes de empezar. Encontré cuatro clases de daño, no dos:

Clase	Alcance	Estado
«resolución» → «reproducto»	30 ocurrencias, 12 archivos. Incluía el título de §6 de la guía de usuario, el nombre del campo resolucion del contrato de bitácora, y texto que el orquestador copia literal al despacho	0 en texto normativo

Columna «Solución» de anti-patrones → «Producto»	23 tablas en 17 archivos: 15 de las 17 reglas, más el marco teórico	23 restituidas
Etiqueta **Proyecto:** → **Proyecto de código:** sobre valores que no son unidades de compilación	14 en 13 archivos, más 27 marcadores {{nombre-proyecto}} que la 5.0 declaró renombrar y no renombró	0
60 filas históricas de control de cambios reescritas en 23 archivos, contra la prohibición textual de §VI.2	—	60 restituidas al texto original
Más el registro: Master-Prompt.md declaraba 5.0 sin fila en su §16 —que dice que todo cambio queda registrado—, la guía de arranque declaraba dos versiones contradictorias en el mismo archivo, y ~20 archivos se habían modificado sin fila. Todo registrado, con las filas retroactivas declarando que son retroactivas.

Bloque 1 — Vocabulario-Rules.md §9 (1.0 → 2.0), en cinco subsecciones: la regla de decisión, la declaración de que el contexto de lectura de un subagente es la sección y no el documento, las tres formas por costo creciente, la prohibición de declarar una invariante sin verificar la colisión, y §9.5, que prohíbe la sustitución global de cadena con el daño de la 5.0 como evidencia. La ubiqué en §9 y no después de §6 —su lugar natural— porque insertar antes habría renumerado §8, citada desde el CHANGELOG.md, que es registro intocable.

Y el hallazgo que no estaba en el informe: Vocabulario-Rules.md no estaba cableada. Declaraba como lector «todo subagente AG-XX», las 17 reglas la citan desde su cabecera, y no figuraba en los insumos del despacho de §8. Ningún subagente la recibía. Ahora se inyecta en todo despacho y en el del auditor.

Bloque 2 — Glosario-Funcional.md obligatorio para los ocho tipos D8 (era sección de un documento condicional: un library no tenía glosario). Menciones de «glosario» en esa regla: 1 → 20. Reglas sin ninguna mención: 9 de 17 → 0 de 17. Master-Prompt.md §10 pasa de un criterio a cuatro, incluido el criterio negativo: una polisemia con contextos disjuntos no es hallazgo.

Bloque 3 — la guía de usuario contradecía al orquestador justo en lo que la 5.0 vino a corregir: decía que Raiz-Codigo se deriva en PascalCase del nombre del producto, cuando §3.2 declara que se declara y admite segmentos. Un usuario que la siguiera disparaba la validación bloqueante. La guía de desarrollo contradecía al README raíz en tres conteos.

Dos correcciones al informe de origen
Rules-UX-UI-DX.md sí necesitaba cambio. El informe lo daba por «sin cambios porque ya cumple». Su glosario era «Recomendado» y su único criterio verificaba la no duplicación: se podía omitir y pasar el audit.
El criterio no va a Master-Prompt.md §5.2 como proponía el informe, porque en 4.1 Vocabulario-Rules.md no existía.
Lo que quedó sin hacer, declarado
La prueba de regresión con corrida real (generar la categoría 02 de un library y comprobar que emite glosario) requiere el orquestador sobre un destino. Está declarada como pendiente en la nota de coherencia y en el CHANGELOG, no como cumplida.
Las filas de control de cambios que la 4.1 escribió y la 5.0 reescribió el mismo día no son restituibles: no hubo commit intermedio, así que no existe fuente de verdad de su texto. Quedan con el vocabulario nuevo. Lo declaré en lugar de reconstruirlas.
También reexpresé dos celdas de la nota de coherencia de la 5.0 que afirmaban verificaciones no realizadas —el barrido de concordancias de género y la restitución del LF final—, sin tocar su alcance verificado, según el criterio de reexpresión del README.

Registro en CHANGELOG.md entrada [5.1] y en la nota nueva Coherencia-Sustitucion-Lexica-Y-Gobierno-Glosario.md, con las once comprobaciones reproducibles. Las diez ejecutables dan cero residuos; cero desajustes entre cabecera y control de cambios en los 46 archivos.