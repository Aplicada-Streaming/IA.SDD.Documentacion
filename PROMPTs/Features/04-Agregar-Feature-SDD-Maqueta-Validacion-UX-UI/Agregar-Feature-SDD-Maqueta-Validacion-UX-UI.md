# Tool-Prompt — Fase SDD - Validacion-UX-UI

> **Invocación**:
> - `Lee y ejecuta /IA/SDD/IA.SDD.Documentacion/PROMPTs/Features/04-Agregar-Feature-SDD-Maqueta-Validacion-UX-UI/Agregar-Feature-SDD-Maqueta-Validacion-UX-UI.md`
> Overview: agregar fase de validación y maquetado como fase de validación. Primera prueba de sensor de deriva.

---

# Contexto

Leer `/IA/SDD/IA.SDD/README.md` y `/IA/IA.SDD/SDD/Guides/Guia-Usuario-SDD-v1.0.md`, trata sobre un `Framework SDD` para diseñar software asistido por IA.

La etapa de documentación automática por parte del agente orquestador es una carga muy pesada para seguir, controlar y validar por parte del desarrollador humano. Y por otro lado, no se puede apreciar claramente como resultará las especificaciones sobre la UX y UI que se va a generar por parte del agente orquestador y las documentaciones establecidas en las diferentes etapas.

Tambien, se necesita introducir un etapa de validación visual de propuesta, de la cual podría ser una maquetización por parte de la IA en html y css de aquellos proyectos con diseño de interfaces visuales al usuario.

Y por último, se necesita introducir mecanismos y evidencias constrastables para que los agentes no deriven.


---

# Objetivo

1. Construir o enriquecer la etapa de especificación automatica de UI y UX opcional, durante la fase de creación de la documentación por parte del orquestador, partiendo o tomando modelos y estructuras de datos y/o descripción provistas previamente por la documentación intake o autogenerada, y que son pevias al desarrollo de la documentación de especificación  UX y UI.

2. La maqueta resultante debe permitir la validación de la navegación, rediseño de la misma, tanto de la experiencia de usuario, como el flujo de usuario y apariencia (UX y UI) interviendo manualmente en los htmls , javascript html jpg de la maqueta como mediante prompt por parte del agente humano hacia el agente orquestador.

3. Enriqueser el `Framework SDD` con la experiencia acumulada en diseños UX-UI. A partir de la maquetización validada y aprobada evaluar y sintentizar las reglas constructivas del mismo, capturar todos los aspectos observados en un documento markdown de reglas para su uso por parte de agentes en la etapa de maquetado y construccion del SDD en la fase de UX-UI, depositarlos en `/IA/IA.SDD/SDD/Devs/Modelos-UX-UI/<solicitar-nombre>.md` . De esta manera el agente orquestador puede ofrecer alterntivas antes de presentar la maqueta inicial, o si se basa en lo que tiene por defecto en las reglas constructivas en prefijado en `/IA/IA.SDD/SDD/Devs/References/Design`. Estas alternavitas serían alguna de las experiencias previas documentadas como reglas en `/IA/IA.SDD/SDD/Devs/Modelos-UX-UI`. Aprobada la maqueta, la captura de las especificaciones de la maqueta debe contemplar diseños especificos de diseño, forma de presentar los datos, recursos y aspectos visuales utilizados, efectos, elementos de UX, modos de navegación, formas constructivas de los css y html. Se debe evaluar lo necesario a incluir como regla de modelo UX-UI para que un agente presente en un diseño posterior algo similar o igual a lo que se capturo en un diseño anterior en materia de diseño UX-UI.

4. La maqueta generada junto con las correcciones de modelos y estuctura de datos debe servir para introducir  elementos para el sensado de deriva de los agentes. Estos elementos podrían aportar un guía tutora para comparar (tanto para el agente humano como el agente IA) si el resultado logrado durante las etapas de codificación se ajustan tanto a los modelos de datos como los diseños visuales aprobados en la etapa final de maquetización. Así se introduciría un mecanismo de control de deriva de agentes (`sensado de deriva`). Con elementos verificables permitiría introducir reglas como ` Toda afirmación deberá estar respaldada por evidencias verificables.`. Las evidencias verificables se ajustarian en esta etapa de prototipado inicial.

---

# Solicitud

1. Ponerse en contexto al agente IA buscando toda la información acorde al contexto dado y objetivos planteados.

2. En base a los objetivos planteados, introducir una etapa opcional visual de maquetización respetando la estructura planteada por el framework y escalandola las nuevas funcioanlidades. 

3. Determinar las nuevas funcionalidades, planificar su incorporacion teniendo en cuenta lo siguiente:

3.a Debe alcanzar a todos los proyectos visuales que contemple la solución a desarrollar por el `Framework SDD`, proyecto por proyecto de forma independiente.

3.b. La maquetización debe ser html, css, bootstrap 5.0, tomando los estilos web y diseños de pantallas, definidos en `/IA/IA.SDD/SDD/Devs/References` de forma inicial, o bien segun lo que se elegio previamente a tomar del catalogo de `/IA/IA.SDD/SDD/Devs/Modelos-UX-UI`.

3.c El orquestador, luego que ha definido su documentación o etapa de especificación respecto a UX y UI, debe diseñar y mostrar, lanzando en el navegador web, una maqueta html con css y esperar las valoraciones del agente humano para su validación y/o espectativas resultantes.

3.d Confirmados la maqueta final, se retroalimenta en la documentación de especificación pertinente que se genera para el proyecto particular. Debe sugerir el agente orquestador si el agente humano desarrollador desea incorporar como modelo de diseño UX-UI en  `/IA/IA.SDD/SDD/Devs/Modelos-UX-UI` bajo un nombre para cita en referencias futuras o propuestas por el agente orquestador previo a la etapa antes de presentar la maquetizacion

3.e La maqueta debe presentar los modelos de datos provistos por las etapas previas de sdd, con ejemplos propuestos en la documentación, harcodeados en los htmls-css-js, a fin de que sirva tambien para validar visualmente modelos de datos y/o situaciones que puedan resultar, navegación , consistencia de la idea, etc.

3.f Todas las correcciones finales se deben retroalimentar en los documentos de especificación que el `Framework SDD` va generando por parte del agente orquestador y propagar en las demás fases documentales hacia atrás y hacia adelante.

3.g Evaluar esta regla de `sensado de deriva` y estudiar, proponer como incorporarla al `Framework SDD`: 

3.h Aprobado el modelo de la uix, generar una representación tipo ejemplo de la regla registrada en 
`/IA.SDD/Templates/<Nombre-Modelo-Solicitado-Al-Usuario>`, esto es para cada proyecto visual que se maquetice. Aquí se debe construir a partir de la maqueta algo similar copiando css, jss, html, pero ofuscando datos para no comprometer la seguridad el diseño ya que `IA.SDD` es un repositorio publico.

3.i Evaluar el metodo de relanzado el navegador web con server light de visualcode. El modelo de maquetado construido por el agente orquestador debe ser dejado dentro de la carpeta `<Repositorio-Destino>/SDD` en el repositorio destino. El esquema final va a quedar como:

```
Workspace
  |- IA.SDD     respositorio del `Framework SDD` (`https://github.com/Aplicada-Streaming/IA.SDD.git`)
  |    |-SDD
  |    |    |- PROMPTs
  |    |    |   |- Devs
  |    |    |    
  |    |    |- Devs
  |    |    |   |- Otros-Ficheros       
  |    |    |   |- Intake 
  |    |    |   |    |- SOLUTION-INTAKE-template.md    
  |    |    |   |    |- SOLUTION-MANIFEST-template.md    
  |    |    |   |    |- Otros-Ficheros   
  |    |    |   |
  |    |    |   |- Modelos-UX-UI         
  |    |    |        |- Rules-Disign-<Nombre-Modelo-Solicitado-Al-Usuario>.md
  |    |    |   
  |    |    |- Guides
  |    |        |- Otros-Ficheros              
  |    |   
  |    |- Templates         
  |    |    |- `<Nombre-Modelo-Solicitado-Al-Usuario>` - generar un ejemplo generico 
  |    |    |        |- assets, 
  |    |    |        |    |- js, 
  |    |    |        |    |    |- ficheros js generados 
  |    |    |        |    | 
  |    |    |        |    |- css, 
  |    |    |        |    |    |- ficheros css generados 
  |    |    |        |    |   
  |    |    |        |    |- img, 
  |    |    |        |
  |    |    |        |- ficheros htmls     
  |    |    |        |- Index.html
  |    |    |
  |    |    |- otros templates de otros modelos ...
  |    |              |- ...
  |    |
  |    |- Otros-Ficheros       
  |    
  |- `<Repositorio-Destino>`
  |    |-SDD
  |    |    |- Intake 
  |    |    |   |- `SOLUTION-INTAKE-<nombre-solución>_v1.0.md`    este es el markdown por la ia
  |    |    |   |- `SOLUTION-MANIFEST-<nombre-solución>_v1.0.md`    
  |    |    |   |- otros ficheros
  |    |    |   
  |    |    |- Docs
  |    |    |   |- otros ficheros (las especificaciones que generá el prompt maestro)
  |    |    |
  |    |    |- Maquetas
  |    |    |   |- `<nombre-proyecto-web-front>`  > maqueta visual de aplicación web front
  |    |    |   |        |- assets, 
  |    |    |   |        |    |- js, 
  |    |    |   |        |    |    |- ficheros js generados 
  |    |    |   |        |    | 
  |    |    |   |        |    |- css, 
  |    |    |   |        |    |    |- ficheros css generados 
  |    |    |   |        |    |   
  |    |    |   |        |    |- img, 
  |    |    |   |        |
  |    |    |   |        |- ficheros htmls 
  |    |    |   |        |- index.html
  |    |    |   |        
  |    |    |   |- `<nombre-proyecto-app>`  > maqueta visual de aplicación app como ejemplo adicional aquí, pero puede haber otros web-library, etc.
  |    |    |   |        |- assets, 
  |    |    |   |        |    |- js, 
  |    |    |   |        |    |    |- ficheros js generados 
  |    |    |   |        |    | 
  |    |    |   |        |    |- css, 
  |    |    |   |        |    |    |- ficheros css generados 
  |    |    |   |        |    |   
  |    |    |   |        |    |- img, 
  |    |    |   |        |
  |    |    |   |        |- ficheros htmls 
  |    |    |   |        |- index.html
  |    |    |   |        
  |    |    |- README.md      
  |    |    
  |    |- Otros-Ficheros       
  | 
  |- Otros-Ficheros       
```

3.j El agente humano debe poder modificar dicha maqueta manualmente e indicarle al agente orquestador mediante el chat, en la fase de validación de maqueta que la reevalue y tome las correcciones realizadas.

4. Actualizar la documentación de desarrollo del framwork `Framework SDD`: `/IA/IA.SDD/SDD/Guides/Guia-Usuario-SDD-v1.0.md` acorde al propósito del documento. Agrega una tabla de contenido.

5. Revisar coherencia y consistencia de todos los documentos.


Todo esto Aplicaría a:
- Proyectos de aplicaciones visuales como proyectos móviles y proyectos web finales o front y libreria de componentes web dentro de la solución.


---

# Restricciones

- Ser fiel a la estructura del `Framework SDD`, siguiendo en principio lo dictado por `/IA/IA.SDD/SDD/Guides/Guia-Usuario-SDD-v1.0.md`.
- No inventar información; toda afirmación debe estar respaldada por evidencia verificable.
- Toda afirmación deberá estar respaldada por evidencias verificables.