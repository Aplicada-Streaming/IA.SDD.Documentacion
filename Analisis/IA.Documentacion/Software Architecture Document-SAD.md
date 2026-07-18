
 **Software Architecture Document (SAD)** 

Perfecto. Yo no haría un documento "común". Haría un documento que pueda acompañar al proyecto durante años y sirva tanto para desarrollar como para incorporar nuevos módulos.

Lo estructuraría como un **Software Architecture Document (SAD)** mezclado con un **Project Vision**, de forma similar a la documentación de Kubernetes, Home Assistant o Frigate.

## Documento propuesto

```markdown
HomeHub/
│
├── README.md
│
├── 01-Introduccion/
│   ├── 01-Vision.md
│   ├── 02-Objetivos.md
│   ├── 03-Alcance.md
│   ├── 04-Glosario.md
│   └── 05-Tecnologias.md
│
├── 02-Arquitectura/
│   ├── 01-Arquitectura-General.md
│   ├── 02-Componentes.md
│   ├── 03-Patrones.md
│   ├── 04-Diagramas.md
│   └── 05-Decisiones-Arquitectura.md
│
├── 03-Modulos/
│   ├── DeviceManager.md
│   ├── VideoManagement.md
│   ├── StorageManager.md
│   ├── AutomationEngine.md
│   ├── Scheduler.md
│   ├── Security.md
│   ├── Notifications.md
│   └── API.md
│
├── 04-Video/
│   ├── Camaras.md
│   ├── Streaming.md
│   ├── Segmentacion.md
│   ├── Timeline.md
│   ├── Eventos.md
│   ├── Exportacion.md
│   └── Playback.md
│
├── 05-Storage/
│   ├── StorageProvider.md
│   ├── StorageNode.md
│   ├── Replicacion.md
│   ├── Retencion.md
│   ├── Backup.md
│   ├── DisasterRecovery.md
│   └── Sincronizacion.md
│
├── 06-Seguridad/
│   ├── Usuarios.md
│   ├── Roles.md
│   ├── Auditoria.md
│   ├── PermisosTemporales.md
│   ├── OAuth.md
│   └── MFA.md
│
├── 07-CasosUso/
│   ├── CU01-VerCamaras.md
│   ├── CU02-ReproducirVideo.md
│   ├── CU03-ExportarVideo.md
│   ├── CU04-Automatizaciones.md
│   ├── CU05-UsuarioTemporal.md
│   └── ...
│
├── 08-Modelo/
│   ├── Dominio.md
│   ├── Entidades.md
│   ├── Eventos.md
│   ├── BaseDatos.md
│   └── Metadata.md
│
├── 09-API/
│   ├── REST.md
│   ├── GraphQL.md
│   ├── WebSocket.md
│   └── StorageAPI.md
│
├── 10-Roadmap/
│   ├── MVP.md
│   ├── Version1.md
│   ├── Version2.md
│   └── IdeasFuturas.md
│
└── anexos/
```

---

# Mi propuesta de arquitectura

Yo dividiría completamente el sistema.

```text
HomeHub

NO almacena videos

NO habla con Google Drive

NO habla con NAS

NO conoce SMB

NO conoce S3

Sólo coordina.
```

En cambio existirían varios servicios.

```
                HomeHub

        Device Manager
        Automation
        VMS
        Scheduler
        Security

               │

               ▼

         Storage Manager

               │

    ┌──────────┼──────────────┐

    ▼          ▼              ▼

 Local      GoogleDrive      NAS

               ▼

        Storage Node API
```

---

# Otra idea

Yo incluso separaría el NAS.

No expondría un SMB.

Crearía una aplicación.

```
HomeHub Storage Node
```

que corra dentro del NAS.

Ese servicio tendría responsabilidades.

```
Recibir segmentos

Guardar

Calcular Hash

Eliminar

Compactar

Servir Video

Verificar Integridad

Informar Espacio

Replicar

Versionar
```

Así HomeHub jamás accede al filesystem.

Siempre habla con una API.

---

# Lo que me gustó de la conversación

La idea de los permisos temporales.

Eso normalmente no existe en Home Assistant.

Pero sí existe en VMS empresariales.

Ejemplo.

```
Crear usuario

Policia_Parana
```

Permisos

```
Solo Cámara Frente
```

Periodo

```
14/07/2026

08:00

↓

15/07/2026

18:00
```

Puede

```
✔ Descargar

✔ Ver Timeline

✔ Buscar Eventos

✘ Configurar Cámara

✘ Eliminar Videos

✘ Ver otras cámaras
```

Eso es extremadamente útil.

---

# Otra idea que me parece excelente

No guardar "videos".

Guardar segmentos.

```
14-00-00.mp4

14-00-30.mp4

14-01-00.mp4

14-01-30.mp4
```

Y en la base de datos.

```
Segmento

Id

CameraId

Inicio

Fin

Duración

Hash

Storage

Estado

Checksum

MotionDetected

PeopleDetected

VehicleDetected

Exportado
```

Eso hace que todo sea muchísimo más escalable.

---

# Una evolución interesante

Durante la charla me di cuenta de que el sistema podría crecer mucho más de lo inicialmente planteado.

Yo ya no lo llamaría solamente un sistema de domótica.

Lo veo como una plataforma distribuida.

```
                HomeHub

                     │

     ┌───────────────┼────────────────┐

     ▼               ▼                ▼

 Device Hub     Automation      Video Hub

                                        │

                                 Storage Manager

                                        │

                     ┌──────────────────┼────────────────────┐

                     ▼                  ▼                    ▼

               Storage Node      Storage Node         Cloud Storage

                 Casa A             Casa B               S3
```

Y esos Storage Nodes podrían estar en cualquier parte del mundo.

---

## Hay una característica que agregaría desde el primer día

Durante la conversación apareció una idea que, en mi opinión, cambia bastante el alcance del proyecto: **separar completamente el plano de control del plano de datos**.

* **Plano de control (HomeHub):** administra cámaras, dispositivos, automatizaciones, usuarios, permisos, auditoría y el catálogo de videos.
* **Plano de datos (Storage Nodes):** almacena físicamente los segmentos de video y responde a solicitudes de carga, descarga o reproducción.

Con esa separación, HomeHub nunca necesita saber si un segmento está en un SSD local, en un NAS remoto conectado por Tailscale, en Google Drive o en un bucket S3. Solo consulta el catálogo, identifica el proveedor responsable y delega la operación al Storage Node correspondiente.

Esta decisión arquitectónica facilita agregar nuevos proveedores de almacenamiento, distribuir réplicas, balancear la carga y escalar el sistema sin modificar la lógica del VMS. Es una arquitectura muy similar a la utilizada por plataformas distribuidas de almacenamiento y videovigilancia empresariales.

Creo que ese debería ser uno de los principios arquitectónicos fundamentales del proyecto.
