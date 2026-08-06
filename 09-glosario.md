# 9. Glosario

## Términos del dominio

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

## Términos de configuración (Playbook)

Vocabulario que aparece al implementar un flujo en el JSON del proceso.

| Término | Definición |
|---|---|
| **accionId** | Identificador del botón; llave bajo la que vive la lógica de servidor (`backend[accionId][fase][etapa]`). Se resuelve como `ref ?? name` del botón |
| **Fase** | Cada paso del pipeline de servidor (`depuracion`, `prevalidacion`, `validacion`, `consulta`, `actualizacion`, `accion`, `obtencion`, `filtro`, `configuracion`, `sucesion`…) |
| **Etapa actual / posterior** | La etapa sobre la que trabaja una fase: la que el usuario acaba de enviar (actual) o la que se va a renderizar a continuación (posterior) |
| **refForm** | Llave bajo la que un formulario agrupa sus datos en `datosTarea`; debe coincidir entre frontend y backend y ser única por etapa |
| **typeForm** | Tipo de componente React de un formulario (PascalCase), resuelto en el `switch` de `Tramite.js` |
| **propsForm** | Props adicionales que se pasan al componente del formulario; dependen del `typeForm` |
| **fields** | Definición de los campos (inputs) de un `Formulario`, organizada por filas |
| **blockForm** | Bloquea la UI **y** hace que el backend ignore el valor del formulario (distinto de `readOnly`, que sí envía el valor) |
| **camposObligatorios** | Campos requeridos de un formulario en una etapa de validación |
| **camposValidaciones** | Árbol condicional (if / elif / switch) de una fase de validación o de la sucesión |
| **funciones{Fase} / campos{Fase}** | Par de subllaves de las fases de escritura: `funciones…` se ejecuta siempre; `campos…` es el árbol condicional |
| **operador~alias** | Formato de la llave de una regla; el motor usa la parte izquierda (`split("~")[0]`) como operador y el sufijo da unicidad a la llave |
| **opciones / `*`** | Ramas de una regla: una llave literal es un caso (hace `break` al coincidir); `"*"` ejecuta el operador y recursa (guardia / default) |
| **validacionSucesion** | Árbol de ramificación de la clave `sucesion` que decide el sucesor |
| **typeSucesor** | De qué lista de `TareasHeron` se toma el destino: `"Autoriza"` o `"Rechaza"` |
| **indexSucesor** | Posición (base 0) dentro de la lista `autoriza`/`rechaza` |
| **vigencia** | Flag de `ProcesosHeron` que controla si el proceso está activo (`"0"` = inactivo, no aparece) |
| **ssla** | SLA en minutos, de un proceso o de una tarea |
| **tareaFinal** | Marca de nodo terminal; el estatus de cierre lo fija la llave `estatus` de la `sucesion`, no `tareaFinal` por sí solo |
| **repetir / proceso / subproceso** | Acciones especiales de enlace; cada una se configura en un bloque dedicado de `backend[accionId]` (`repetidor`, `nuevoProceso`, `subProceso`), no como entrada de `autoriza`/`rechaza`. `repetir` reasigna el folio dentro de la misma tarea; `proceso` lanza un folio hijo independiente; `subproceso` embebe un desvío en el mismo folio |
| **ConfigHeron "Accesos"** | Documento que concede a un tipo de usuario/área la visibilidad para **iniciar** un proceso en "Nuevo Trámite" |
| **VAR_DEL** | Valor centinela que, asignado a una llave, hace que el core la **borre** de `datosTarea` |
| **código 201 / 400** | Únicos códigos que interpreta la validación: `201` éxito (avanza, no muestra mensaje); `400` error (detiene y muestra el mensaje) |

---

<!-- nav -->
<table width="100%">
<tr>
<td align="left"><a href="08-playbook-nuevo-tramite.md">← Anterior: Playbook: Nuevo Trámite</a></td>
<td align="center"><a href="README.md">Principal</a></td>
<td align="right">&nbsp;</td>
</tr>
</table>
