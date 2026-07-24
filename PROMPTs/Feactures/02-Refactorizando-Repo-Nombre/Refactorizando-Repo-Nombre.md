# Tool-Prompt — Refactorizando SDD

> **Invocación**:
> - `Lee y ejecuta /IA/IA.SDD.Documentacion/PROMPTs/Feactures/02-Refactorizando-Repo-Nombre/Refactorizando-Repo-Nombre.md`

---

# Contexto

He tomado el repositorio `https://github.com/Aplicada-Streaming/Template_IA_SDD2.0R.git` y a partir de este cree uno nuevo con el contenido de este. El nuevo repositorio es: `https://github.com/Aplicada-Streaming/IA.SDD.git` y lo clone en `/IA/IA.SDD`.

Lee `/IA/IA.SDD/SDD/Guides/Guia-Usuario-SDD-v1.0.md` y `/IA/IA.SDD/SDD/Devs/Guides/Marco-Teorico-SDD-v1.0.md`

Ahora todo lo que se llamaba `sdd2.2` pasa a llamarse `SDD`. Renombre la carpeta `SDD2.2D/` a `SDD/` y tambien renombre archivos y carpetas.

El repositorio contiene un framework para crear código por especificación (SDD, Spec-Driven Development). Su utilización consiste en una serie de pasos:
1. Primero se crea el contexto en un chat IA con el tipo de sistema a desarrollar, requerimientos del usuario, arquitectura a utilizar, entre otras. 
2. Luego, se le solicita a la IA que genere un documento intake basado en la plantilla `/IA/IA.SDD/SDD/Devs/Intake/SOLUTION-INTAKE-template.md` en base al contexto creado. Se genera un nuevo documento intake con toda la información dada en el contexto: `/IA/IA.SDD/SDD/Devs/Intake/SOLUTION-INTAKE-<Nombre-Solucion>-v1.0.md` 
3. Se prepara el repositorio destino de la aplicación a codear localmente, se clona en una carpeta local, y se copia la carpeta `/IA/IA.SDD/SDD` en el repositorio destino ()`<Repositorio-Destino>`) quedano el repositorio destino en forma resumida como:

```
 workspace
  |- `<Repositorio-Destino>`
       |-SDD
       |    |- Devs
       |    |   |- otros archivos
       |    |   |- Intake 
       |    |        |- SOLUTION-INTAKE-<Nombre-Solucion>-v1.0.md    este es el markdown por la ia
       |    |        |- Otros-Ficheros
       |    |- Docs
       |    |   |- Otros-Ficheros
       |    |   
       |    |- Guides
       |        |- Otros-Ficheros
       |       
       |- Otros-Ficheros
```

4. Se reemplaza el documento intake template por el generado en el punto 2 . quedará finalmente `/IA/IA.SDD/SDD/devs/intake/SOLUTION-INTAKE-template.md`  por este `/IA/IA.SDD/SDD/Devs/Intake/SOLUTION-INTAKE-<Nombre-Solucion>-v1.0.md`

5. Se ejecuta el prompt maestro: `/IA/IA.SDD/PROMPTS/PROMPT-Agente-Bootstrap-SDD.md`. En  en la documentación vas a encontrar que se hace referencia al markdown:
`/IA/IA.SDD/PROMPTS/PROMPT-claude-code-bootstrap-sdd.md` este lo renombre a `/IA/IA.SDD/PROMPTS/PROMPT-Agente-Bootstrap-SDD.md`.

Este prompt orquestador va a generar documentación de especificación primero en el
`/<repositorio-Destino>/SDD/docs` y finalizada la documentación de especificación este continua con la codificación de la solución propuesta en la misma documentación. 

---

# Objetivos

1. Revisar toda la documentación y arreglar las referencias internas de este repositorio. Tener en cuenta la nueva nomenclatura que he establecido al renombrar los archivos y carpetas

2. Revisar y cambiar aspectos metodologicos siguiendo una nueva metodología:

Hasta ahora se copiaban los documentos SDD iniciales al repo destino y desde ahí se ejecutaba el prompt orquestador. Ahora se pretende trabajar con dos repositorios ubicados en un workspace común.

Ahora, el sdd establecido se puede resumir en esta estructura:

```
 workspace
  |- <Repositorio-Destino>`
       |-SDD
       |    |- devs
       |    |   |- otros archivos
       |    |   |- intake 
       |    |        |- `SOLUTION-INTAKE-<nombre-solución>_v1.0.md`  este es el markdown por la ia
       |    |        |- otros
       |    |- docs
       |    |   |- otros
       |    |   
       |    |- guides
       |        |- otros          
       |- otros
```

El objetivo es pasar a esta otra estructura:

```
Workspace
  |- IA.SDD     respositorio refactorizado (`https://github.com/Aplicada-Streaming/IA.SDD.git`)
  |    |-SDD
  |    |    |- PROMPTs
  |    |    |   |- Devs
  |    |    |    
  |    |    |- Devs
  |    |    |   |- Otros-Ficheros       
  |    |    |   |- Intake 
  |    |    |        |- SOLUTION-INTAKE-template.md    
  |    |    |        |- SOLUTION-MANIFEST-template.md    
  |    |    |        |- Otros-Ficheros       
  |    |    |   
  |    |    |- Guides
  |    |        |- Otros-Ficheros              
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
  |    |    |   |- .gitkeep
  |    |    |
  |    |    |- README.md      
  |    |    
  |    |- Otros-Ficheros       
  | 
  |- Otros-Ficheros       
```
Esto va a permite que las reglas de los agentes, templates y prompts maestros queden fuera del repositorio destino, y si hay mejoras se puedan propagar a nuevas soluciones que utilicen este SDD. Y por otro lado los ficheros generados del SDD queden del lado del repositorio Destino.

Con la nueva estructura, en concreto la metología sería:

1. Se crea el contexto, igual que se venia haciendo, puede ser local o en un chat IA web.
2. Se le pide al chat IA o agente local que a partir de ese contexto se genere un nuevo documento intake partiendo de la plantilla `/IA/IA.SDD/SDD/Devs/Intake/SOLUTION-INTAKE-template.md`. Para el caso de un agente local se le puede pedir que lo ubique en la carpeta del repositorio destino quedando algo como: `/<Repositorio-Destino>/Intake/SOLUTION-INTAKE-<nombre-solución>_v1.0.md`
3. Se invoca el agente orquestador

```
Leer y Ejecutar `/IA/IA.SDD/PROMPTS/PROMPT-Agente-Bootstrap-SDD.md` en el repositorio: `/<Repositorio-Destino>`
```

El agente crearía la esctructura carpeta junto con documentación y el manifest en el respositorio destino. 


---

# Solicitudes

- Planifica y arma el plan acorde a los objetivos propuestos. 
- Muestra los hallazgos, el plan de modificaciones resultantes y esperá a que te confirme.
- No inventar información.
- No inventar información; toda afirmación debe estar respaldada por evidencia verificable.


