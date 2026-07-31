# 8. Glosario

| Término | Definición en MIDAS |
|---|---|
| **Trámite** | Instancia de ejecución de un proceso Héron para un folio |
| **Folio** | Identificador único de un trámite activo |
| **Proceso** | Definición del flujo (grafo de tareas) almacenada en MongoDB + PostgreSQL |
| **Tarea** | Nodo del grafo asignado a un área/bandeja con SLA |
| **Etapa** | Pantalla dentro de una tarea; forma parte del mini-grafo de navegación de la tarea |
| **Bandeja** | Cola de trabajo de un área; su catálogo vive en `ConfigHeron` (registro `Bandejas`, llave `Catalogo`) |
| **Sucesión** | Regla de enrutamiento a la siguiente tarea, definida en la clave `sucesion` del JSON |
| **Autoriza / Rechaza** | Listas de tareas sucesoras en `TareasHeron`; determinan el camino hacia adelante o de retorno del grafo |
| **TrackingFinal** | Flag del registro activo de un folio (único `True` por folio en `TrackingHeron`) |
| **Robot** | Proceso diferido que ejecuta operaciones fuera del ciclo HTTP |
| **Ramo** | Línea de negocio de seguro (vida, GMM, autos…) |
| **aliasTramite** | Nombre visual alternativo de una tarea, escrito en `datosTarea` por la fase `actualizacion` |
| **grupoTarea** | Agrupador de registros de tracking para medir permanencia en una tarea |
| **frontendAgrupador** | Clave contenedora en MongoDB que puede anidar múltiples variantes de frontend |
| **parametrosDinamicos** | Búsqueda de parámetros que están en múltiples ramas de un árbol dentro de `datosTarea` |
| **valor fijo** | Parámetro cuyo `formulario` es `null`; el `valor` se toma literalmente en lugar de como ruta en `datosTarea` |
| **datosTarea** | Documento MongoDB con todos los datos de formulario capturados en un tracking |
| **Bitácora** | Historial de auditoría de un folio; no es una colección, se deriva de `TrackingHeron` |
| **ConfigHeron** | Colección MongoDB con las configuraciones del core del negocio, identificadas por `nombreId` |

---

<!-- nav -->
<table width="100%">
<tr>
<td align="left"><a href="07-decisiones.md">← Anterior: Decisiones</a></td>
<td align="center"><a href="README.md">Principal</a></td>
<td align="right"><a href="09-playbook-nuevo-tramite.md">Siguiente: Playbook: Nuevo Trámite →</a></td>
</tr>
</table>
