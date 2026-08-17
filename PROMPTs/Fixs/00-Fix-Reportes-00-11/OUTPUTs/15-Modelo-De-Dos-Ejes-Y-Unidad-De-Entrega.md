# El nivel de unidad de entrega y el modelo de dos ejes

**Documento:** 15-Modelo-De-Dos-Ejes-Y-Unidad-De-Entrega.md
**Versión:** 1.0
**Fecha:** 2026-08-15
**Estado:** **Aplicado.** Este documento fue el análisis; su plan de aplicación es `40-Plan-8.0-Unidad-De-Entrega.md` y su ejecución quedó verificada en `42-Verificacion-8.0.md`. El framework está en la versión 8.0
**Naturaleza:** análisis que originó la intervención estructural, surgido durante la intervención sobre los reportes 00 a 11 y separado de ella a propósito

---

> **Nota de estado, 2026-08-15.** Cuando se escribió, este documento declaraba que ninguna de sus
> propuestas se había aplicado. Ya no es así: se aplicaron todas en la versión 8.0 del framework. Lo
> que sigue se conserva como el análisis que la fundamentó, sin reescribirlo, para que se pueda
> contrastar lo propuesto contra lo hecho.

## 1. Por qué este documento existe aparte

La intervención sobre los doce reportes corrige huecos dentro del modelo vigente: dos niveles,
producto y proyecto de código. Durante su aplicación apareció una pregunta que no es de esa clase:
**el modelo tiene dos niveles y los productos reales tienen tres**.

No se resuelve acá porque es una intervención estructural, y porque una de las correcciones de la
7.0 —declarar el nivel **por artefacto** y no por categoría, del reporte `08`— es su prerrequisito
literal: sin esa columna no hay dónde escribir qué artefacto corresponde a qué nivel.

## 2. El framework ya declaró este pendiente

No es un hallazgo nuevo. `Vocabulario-Rules.md` §8, titulada «Pendiente declarado»:

> La **unidad de entrega** está definida en §2 y tiene correspondencia de industria, pero **todavía
> no es un nivel del layout de salida**. Hoy el nivel intermedio de
> `SDD/Docs/Proyectos/<Nombre-Proyecto-Codigo>/` se puebla con proyectos de código, y las once
> categorías que cuelgan de él producen artefactos de nivel producto: casos de uso, experiencia de
> uso, *Product Backlog*, plan de sprint, pipeline, samples y cuerpo documental.
>
> La reubicación de esas categorías al nivel de unidad de entrega es una intervención estructural
> aparte, que esta regla no ejecuta.

Y declara la consecuencia sobre el conjunto cerrado:

> Por la misma razón, `tipo_proyecto_codigo` conserva el conjunto cerrado D8 aunque D8 sea, según el
> propio `SDD-Development-Guide.md`, un catálogo de **formas de entrega**.

Verificado en la fuente: `SDD-Development-Guide.md` línea 442 dice «los ocho tipos cubren el espacio
de **formas de entrega** de software». Dos notas de coherencia lo registran como abierto:
`Coherencia-Vocabulario-Producto-Y-Proyecto-De-Codigo.md` §4 y
`Coherencia-Sustitucion-Lexica-Y-Gobierno-Glosario.md`.

**El diagnóstico en una línea:** un atributo de entrega está colgado del nivel de arquitectura.

## 3. Evidencia medida sobre productos reales

Tres destinos del workspace, contados el 2026-08-15. Los conteos excluyen las carpetas `_legacy`.

### 3.1 Lab-Geometria

Siete proyectos de código declarados en §13 del intake. **Dos se despliegan**:
`GeometriaFactory-Api`, «desplegado en el servidor propio», y `GeometriaFactory-Web`, «desplegado en
el hosting público». Los cinco restantes son librerías con `redistribuible: false`.

| Proyecto de código | Casos de uso emitidos |
|---|---|
| GeometriaFactory-Api | 12 |
| GeometriaFactory-Application | 11 |
| GeometriaFactory-Contracts | 8 |
| GeometriaFactory-Domain | 13 |
| GeometriaFactory-Infrastructure | 10 |
| GeometriaFactory-Visor | 7 |
| GeometriaFactory-Web | 10 |
| **Total** | **71** |

Necesidades de negocio del producto: **9**.

**La misma capacidad aparece fragmentada por capa.** Dos archivos, de proyectos distintos:

```
Domain/…/Casos-De-Uso/CU-05-Crear-Y-Reeditar-Un-Trabajo.md
Contracts/…/Casos-De-Uso/CU-03-Contrato-De-Carga-Y-Edicion-Del-Trabajo.md
```

**Y hay artefactos de entrega emitidos sobre lo que no se entrega.**
`GeometriaFactory-Contracts` es un proyecto de DTOs, y tiene:

- `03-UX-UI-DX/` con `Guia-Onboarding-Developer.md` y `DX-Error-Messages.md`.
- `09-Devops/` con `Entornos-Deploy.md`, `Pipeline-CI-CD.md` y `Supply-Chain-Seguridad.md`.

Su documento de entornos tuvo que abrir con una sección `1.1 Apartamiento declarado del modelo de la
categoría`, y declarar que «este proyecto de código **no tiene ambientes ni canales propios**».

Es la **tercera vez** que un destino inventa por su cuenta la figura del apartamiento: el `ADR-07`
del reporte `06`, el `Glosario-Metodo.md` del reporte `11`, y éste. Tres destinos independientes
resolviendo la misma ausencia.

### 3.2 RPI.VidelControl

Cinco proyectos de código y **una sola unidad de entrega**: `VideoControl-Web`, «punto de entrada
único». Los otros cuatro viajan adentro de su publicación autocontenida, con `redistribuible: false`,
como ya registraba el reporte `06` §6.1.

| Proyecto de código | Casos de uso | Documentos en `03-UX-UI-DX` | Documentos en `09-Devops` |
|---|---|---|---|
| VideoControl-Web | 28 | 29 | 6 |
| VideoControl-Infrastructure | 11 | 5 | 5 |
| VideoControl-Application | 9 | 5 | 5 |
| VideoControl-Domain | 5 | 5 | 5 |
| VideoControl-PinMap | 5 | 5 | 5 |
| **Total** | **58** | | |

Necesidades de negocio del producto: **8**. `VideoControl-Domain` es una librería de modelo puro y
tiene cinco documentos de experiencia de uso y cinco de DevOps.

### 3.3 SelfHosted.Service.Core

Un solo proyecto de código y una sola unidad de entrega. El layout se aplana y no exhibe el defecto:
es el caso degenerado, y confirma que el problema aparece con la composición, no con el framework en
general.

### 3.4 El tipo D8 mal ajustado, en concreto

`GeometriaFactory-Visor` es un «proyecto Node.js/TypeScript que produce el bundle del visor 3D», y
está tipado `library`. `SDD-User-Guide.md` §3.3 define `library` como «librería reutilizable,
distribuida via package manager del ecosistema». No se distribuye —`redistribuible: false`— y no es
un paquete. El valor no le queda porque **D8 describe entregas y el Visor no es una entrega**.

## 4. Los dos ejes

El modelo correcto no es una jerarquía de tres niveles. Son **dos grafos distintos sobre el mismo
producto**, y el propio intake de Lab-Geometria lo demuestra:

> La arista `Web → Api` es de **runtime**, no de compilación: el front habla con la API por HTTP con
> `HttpClient` y tipos de `Contracts`. Por eso no aparece en la columna de dependencias y no
> introduce ciclo.

| Eje | Nodos | Qué los relaciona |
|---|---|---|
| **Entrega** | Producto → unidades de entrega (1..N) | Integración en runtime: quién le habla a quién estando desplegados |
| **Construcción** | Producto → soluciones de código (1..N) → proyectos de código (1..M) | Dependencia de compilación: quién referencia a quién al construir |

Los dos grafos **no coinciden**, y un modelo de tres niveles anidados obliga a que coincidan. La
relación entre los ejes es de **composición, de muchos a muchos**: una unidad de entrega se compone
de varios proyectos de código, y un proyecto de código puede componer varias unidades de entrega.

**El caso que lo prueba.** `GeometriaFactory-Contracts`, en §13 del intake: «DTOs de la API.
**Referenciado por Api y por Web**». Y más abajo: «Es el contrato compartido entre los **dos procesos
desplegables**». Un proyecto de código, dos unidades de entrega. Anidarlo obligaría a documentarlo
dos veces o a asignarlo arbitrariamente a una, dejando en la otra una referencia colgada, que es el
patrón del reporte `07`.

**El eje de construcción es de nivel producto, y el vocabulario ya lo decía.**
`Vocabulario-Rules.md` §3 declara `Raiz-Codigo` y `Artefacto-Agrupacion` como planos de identidad
**del producto**: en Lab-Geometria hay una sola raíz `GeometriaFactory` para los siete proyectos.
Meter los proyectos de código dentro de una unidad de entrega los sacaba del nivel donde su propio
agrupador vive.

**La cardinalidad de soluciones de código no es uno.** `Vocabulario-Rules.md` §5: «Producto y
solución de código… su cardinalidad **no es necesariamente uno a uno**». El modelo no debe asumir una
sola. Lab-Geometria tiene una, heterogénea: el `Visor`, que es Node y TypeScript, vive dentro de la
misma solución que los proyectos .NET.

### 4.1 La forma propuesta

```
PRODUCTO
├── Eje de entrega ─────── unidades de entrega (1..N)   → las once categorías y D8
└── Eje de construcción ── soluciones de código (1..N)
                           └── proyectos de código (1..M)  → stack, dependencias, capas

        puente: matriz de composición  (unidad de entrega × proyecto de código)
```

- **N1 · Producto.** Visión, alcance, roadmap, necesidades de negocio, el equipo y su capacidad; y el
  inventario **completo** de la solución de código con su grafo de compilación.
- **N2 · Unidad de entrega.** `SDD/Docs/Unidades-Entrega/<Nombre>/` con las once categorías. Acá vive
  D8, renombrado a `tipo_unidad_entrega`. No describe los proyectos de código: declara cuáles compone.
- **El proyecto de código no es un nivel de carpetas.** Se inventaría una sola vez, a nivel producto.
- **La matriz de composición** hace visible lo compartido de un vistazo: una columna con más de una
  marca es un proyecto cuyo cambio alcanza a varias entregas.

### 4.2 El artefacto de N1 ya existe y está poblado

No hay que inventarlo. `Master-Prompt.md` §11 ya despacha `Producto/Vista-Producto.md` con «mapa de
proyectos de código con su D8 y rol, contratos inter-proyecto coherentes con las dependencias del
manifiesto, y el grafo de dependencias como vista navegable». En Lab-Geometria está emitido, con
estas secciones: `2. Mapa de proyectos de código`, `3. Grafo de dependencias`, `4. Contratos
inter-proyecto`, `6. Cross-cutting compartido`.

El framework **ya documenta los proyectos de código a nivel producto** y además le da a cada uno un
árbol de once categorías. Lo primero es correcto; lo segundo es la duplicación medida en §3. La
corrección no agrega artefacto: le quita a los árboles por proyecto la competencia con la vista de
producto, y le suma la matriz de composición.

## 5. El test de tres preguntas

Decide si un conjunto de capacidades es un producto o varios. Materializa el criterio de
`Vocabulario-Rules.md` §2: «dos conjuntos de capacidades con clientes, roadmaps y ciclos de vida
**desacoplados** son dos productos, y llevan dos intakes».

1. **¿Hay alguna necesidad de negocio que solo se cumple si las dos piezas están?** Si sí, es un
   producto.
2. **¿Podrías publicar una sin coordinar con la otra?** Si sí, apunta a dos productos.
3. **¿Quién decide en qué se trabaja esta semana?** Si es la misma persona, es un producto.

**Lo que cuesta partir en dos productos**, para que la decisión se tome con el precio a la vista: la
trazabilidad se corta en la frontera. Una necesidad que atraviesa los dos —«que el supervisor audite
en 24 horas lo que el inspector relevó ayer»— no tiene dónde vivir, y cada intake declara su mitad
más un contrato con el otro. El framework lo admite; la cadena D6 no la cubre.

## 6. Dos casos trabajados

### 6.1 Relevamiento georreferencial

Inspectores de obra levantan observaciones de campo; un panel de control las revisa y monitorea.

| Test | Respuesta |
|---|---|
| Necesidad conjunta | Sí: la observación no vale hasta que alguien la revisa |
| Publicación independiente | No: si la app cambia qué captura, el panel se entera |
| Quién prioriza | Una sola persona |

**Un producto, dos unidades de entrega, tres proyectos de código, uno compartido, una solución de
código.**

```
Entrega        Producto «Relevamiento»
               ├── App de campo        (instrumento de captura)
               └── Panel de control    (base necesaria)

Construcción   Solución de código
               ├── App-Core     → App
               ├── Panel-Web    → Panel
               └── Contratos    → App y Panel     ← compartido
```

Es la misma forma que Lab-Geometria: dos procesos desplegables y un proyecto compartido entre ellos.

### 6.2 Sistema de ventas

Portal general y panel de control alcanzan para el producto; la app es un complemento.

| Test | Respuesta |
|---|---|
| Necesidad conjunta | Sí: vender por el portal y administrarlo desde el panel es una sola cadena de valor |
| Publicación independiente | Sí, cada entregable tiene su cadencia — no los separa: son unidades de entrega, no productos |
| Quién prioriza | Una sola persona |

**Un producto, tres unidades de entrega: portal, panel y app.**

**Lo que este caso agrega y no estaba en ninguno de los doce reportes:** una unidad de entrega puede
ser **opcional o diferida** sin dejar de pertenecer al producto. Hoy no se puede expresar: el gating
de `Master-Prompt.md` §4 tiene flags por proyecto de código, y hace falta un gating **por unidad de
entrega**, del estilo de `requiere_maqueta`, para declarar que una entrega está en el roadmap y no en
esta etapa.

**Precisión sobre canales.** Una app publicada en dos tiendas es **una** unidad de entrega con dos
canales, no dos entregas: la unidad se define por poder publicarse de forma independiente, no por
dónde aterriza. `Rules-Devops.md` ya trabaja con canales.

## 7. Qué habría que cambiar, y qué cuesta

| Cambio | Alcance | Severidad |
|---|---|---|
| Layout: el nivel intermedio pasa a unidad de entrega | `Master-Prompt.md` §3.5, `Root-Rules.md` | major |
| D8 pasa de `tipo_proyecto_codigo` a `tipo_unidad_entrega` | Las diecisiete reglas, el intake, el manifiesto, los dos orquestadores | major |
| Intake §13 se parte en dos tablas: unidades de entrega y proyectos de código | `PRODUCT-INTAKE-template.md`, `Intake-Rules.md` §4 | major |
| §14 distingue contratos entre unidades de entrega (integración) de contratos entre proyectos de código (acoplamiento interno) | `PRODUCT-INTAKE-template.md` | minor |
| Matriz de composición como artefacto de nivel producto | `Root-Rules.md`, `Rules-Arquitectura-Tecnica.md` | minor |
| Gating por unidad de entrega, incluida la entrega diferida | `Master-Prompt.md` §4 | minor |
| Nivel por artefacto con tres valores | `Vocabulario-Rules.md` §4 R3 y las tablas maestras | minor, **pero se aplica en la 7.0** |
| Migración de destinos ya generados | `Migracion-Rules.md` | major |

Reducción esperable sobre los destinos medidos: Lab-Geometria pasa de 7 árboles de once categorías a
2; RPI.VidelControl, de 5 a 1; SelfHosted.Service.Core no cambia.

**Por qué no tres niveles de carpetas.** Con dos niveles ya sobran 5 árboles en Lab-Geometria y 4 en
VidelControl. Agregar profundidad agrega réplicas, no precisión. Y el reporte `08` ya midió que con
dos niveles se emiten cinco plantillas de ceremonia byte a byte idénticas.

## 8. Correspondencia de industria

Las tres ya están registradas por el framework en `Vocabulario-Rules.md` §7, de modo que no se
incorpora ninguna fuente nueva:

| Concepto | Correspondencia | Fuente registrada |
|---|---|---|
| Unidad de entrega | *Container* | Modelo C4 |
| Módulo | *Component* | Modelo C4 |
| Producto, con un *Product Backlog* y un *Product Owner* | *Product* | Scrum Guide 2020 |
| Producto entregado al cliente | *Solution* | SAFe |

El propio §7 advierte que estas afirmaciones «se verifican contra los estándares publicados, que no
viven acá», y que «no se adopta ningún marco completo». Se leen como correspondencias declaradas, no
como evidencia comprobable desde el repositorio.

## 9. Qué queda por decidir, y qué habría que falsear

- **La decisión de cada producto**, no del framework: uno o varios, según el test de §5. El framework
  solo tiene que admitir las dos formas.
- **`equipo_n`** es hoy un solo número de nivel producto. Con varias unidades de entrega
  probablemente sea uno por unidad, y eso alcanza al reporte `08`, que ya encontró que la velocidad y
  la capacidad se replicaban donde no correspondía.
- **El ámbito de unicidad de los identificadores.** La 7.0 lo fija en producto. Con N2 conviene
  revisarlo por familia: las que atraviesan el sistema —`NB`— únicas en N1, y las de ejecución —`CU`,
  `RN`, `US`, `BT`— únicas en N2, que es donde se citan entre sí.
- **Falsear el modelo con un producto de tres o más unidades de entrega.** Los dos medidos tienen dos
  y una. El modelo se sostiene sobre ellos y sobre dos casos razonados; le falta un producto
  documentado con la forma completa.

## 10. Relación con la intervención 7.0

| Corrección de la 7.0 | Efecto sobre este modelo |
|---|---|
| Nivel **por artefacto** en lugar de por categoría | **Prerrequisito.** Sin esa columna no hay dónde declarar qué va a N1, qué a N2 y qué al eje de construcción |
| Obligatoriedad por flag del proyecto (`tiene_persistencia`, `redistribuible`) | Compatible, y en la dirección correcta: `redistribuible` es un proxy de «esto se publica», que es el criterio de unidad de entrega |
| Apartamiento declarado | Compatible, y necesario mientras N2 no exista: es lo que tres destinos inventaron por su cuenta |
| Referencia pendiente y reapertura | Compatible, independiente del número de niveles |
| Compuerta mecánica, criterios de audit, vocabulario del método | Compatibles, independientes |
| Ámbito de unicidad y mapa de rangos | A revisar cuando N2 exista: cambia a qué se asigna cada rango, no el mecanismo |
| Ancho de cinco dígitos | Independiente: es una decisión de capacidad |
