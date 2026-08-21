# 0. Guía rápida (referencia)

Resumen de la sintaxis y las reglas de uso más frecuentes de Héron, pensado para tener a la mano sin recorrer toda la documentación. Cada bloque enlaza al detalle en la arquitectura o el playbook.

- **Cómo funciona** → [Vistas 4+1](01-introduccion.md)
- **Cómo se hace, paso a paso** → [Playbook: Nuevo Trámite](08-playbook-nuevo-tramite.md)
- **Definición de términos** → [Glosario](09-glosario.md)

---

## Reglas de nombrado

| Elemento | Formato / regla | Ejemplo |
|---|---|---|
| `procesoId` | `{area}-{numProceso}-{version}` (numérico) | `11-40-1.0` |
| `tareaId` | `{procesoId}-{numTarea}` | `11-40-1.0-1` |
| `folioProcesoTareaId` | `{folio}-{procesoId}-{numTarea}` | `EMS47211-5-8-1-7` |
| `trackingId` | `{folio}-{procesoId}-{numTracking}` | `EMS47211-5-8-1-3` |
| `datosTareaId` | `{folio}-{numTracking}` | `EMS47211-10` |
| Etapa | Inicia en `0` / `0.0`; enteros = flujo lineal; decimales = variantes de una misma pantalla | `0.0`, `0.01` |
| `refForm` | Descriptivo; **coincide exacto** entre frontend y backend; **único por etapa** | `datosConvenio` |
| `accionId` | PascalCase; coincide con el botón del frontend (`ref ?? name`) | `Siguiente`, `Autorizar` |
| Variante de frontend | camelCase con prefijo de rol | `frontendSupervisor`, `frontendAgente{id}` |
| Llaves de config | minúsculas con guion bajo; `*` = todos | `robot_tramites`, `*` |
| Operador repetido | `operador~alias` (el motor usa la parte izquierda; el alias da unicidad) | `==~estatusPoliza` |
| Campo dinámico | Debe coincidir **literal** con lo que espera el core | `fisica` / `moral` |

> Regla práctica: un `refForm` o un valor de campo dinámico mal escrito hace que la pantalla **no renderice y no muestre error**. → [Playbook §11](08-playbook-nuevo-tramite.md), [Vista de Desarrollo §3.4](03-vista-desarrollo.md)

## Numeración de áreas, roles y estatus

| Áreas (`bandeja`) | Roles (`idRol`) | Estatus de folio |
|---|---|---|
| 0 Sistemas · 1 Suscripción · 2 Siniestros · 3 Cobranza · 4 Contabilidad · 5 Legal · 8 Emisión · 11 Comercial · 15 Enlace · 26 Reaseguro · 27 Estrategia | 0 Admin · 1 Supervisor · 2 Empleado · 3 Agente · 4 Asegurado · 5 Externo · 7 Jefe de Área · 9 Promotor | 0 Nuevo · 1 Procesando · 2 Cancelado · 3 Completado · 4 Detenido · 8 En espera · 9 Rechazado |

Catálogos completos → [Vista Lógica §2.1](02-vista-logica.md) · [Playbook Anexo A](08-playbook-nuevo-tramite.md). Solo `Nuevo` (0) y `Procesando` (1) permiten editar/atender.

## Flujos principales (endpoints)

| Endpoint | Hash (prefijo) | Ejecuta `sucesion` | Uso |
|---|---|---|---|
| `NuevoTramite` | `8607…` | — | Crea un folio nuevo |
| `AtenderTramite` | `470a…` | — | Abre un folio de la bandeja |
| `AccionesEtapa` | `e6f8…` | **No** | Navega entre etapas de la misma tarea |
| `AccionesTramite` | `12ab…` | **Sí** | Avanza de tarea (cambia de nodo del grafo) |

Detalle y variantes (Herramienta, Consulta, Bitácora) → [Vista de Desarrollo §3.7](03-vista-desarrollo.md)

## Armado del JSON

Estructura de primer nivel del documento de proceso (colección `FormulariosHeron`):

```json
{ "title": "...", "frontend": { "etapas": {} }, "backend": {}, "consulta": {}, "bitacora": {} }
```

- El `backend` se organiza por acción: **`backend[accionId][fase][etapa]`** (la fase `sucesion` **no** se indexa por etapa).
- `frontend` cambia por rol; `backend` es **compartido** entre todos los roles.
- Detalle → [Vista de Desarrollo §3.2](03-vista-desarrollo.md) · esqueleto completo en [§3.10](03-vista-desarrollo.md)

### Pipeline de fases (orden y efecto)

`AccionesTramite`: `depuracion → prevalidacion → validacion → ensamblaje → postvalidacion → consulta → actualizacion → accion → subAccion → obtencion → filtro → sucesion`.

| Persisten en `datosTarea` | No persisten (solo al front) |
|---|---|
| `consulta`, `actualizacion`, `accion`, `subAccion`, `ensamblaje` (si validó) | `obtencion`, `filtro`, `configuracion` |

`depuracion`/`prevalidacion` reciben el JSON del frontend; de `validacion` en adelante, la unión front + `datosTarea`. Tabla completa (etapa actual/posterior, entrada/salida) → [Vista de Desarrollo §3.5](03-vista-desarrollo.md#pipeline)

**Ejemplo JSON por fase** (uno por cada fase, listo para copiar) → [`EjemplosBase/PipelineBackend/`](../EjemplosBase/PipelineBackend/)

## Condiciones: if / elif / switch

El árbol `opciones` (en `validacion` y `sucesion`) es la lógica condicional:

| Construcción | Cómo se arma |
|---|---|
| `if` (guardia) | Una regla con `opciones: { "*": … }`; `estatus.valido:false` = condición negada (corta si se cumple) |
| `switch` / `if-elif-else` | Varias llaves **literales** (cada una un caso, hace `break`) + `"*"` como default |
| `if` anidado | Una opción contiene otro `camposValidaciones`; el `if` más profundo decide |

Operadores comunes: `==` (soporta `A|B` = OR), `!=`, `>`, `<`, `>=`, `<=`, `between`, `regex`, `in`, `contieneDatos`. Códigos: **201** éxito, **400** error (muestra el `mensaje`). Ejemplos JSON ↔ Python → [Vista de Desarrollo §3.9](03-vista-desarrollo.md#condiciones)

## Sucesión (cambio de tarea)

- Cada tarea declara `autoriza` y `rechaza` (listas de `tareaId` separadas por coma) en `TareasHeron`.
- La clave `sucesion` resuelve el destino con `typeSucesor` (`"Autoriza"`/`"Rechaza"`) + `indexSucesor` (posición en la lista), y define `usuario`, `bandeja`, `estatus`, `etapaId`.
- **El estatus final no es automático**: al llegar a una `tareaFinal` hay que fijar `estatus: 3` (completado) / `2` (cancelado) en la opción.

Valores especiales de `usuario`: `?` (mismo que envió), `?TareaProcesada`, `?usuarioCarrusel` (round-robin de área), `?usuarioAutorizante` (jefe inmediato), `null` (bandeja del área).

Acciones especiales — **bloques dedicados** en `backend[accionId]`, no entradas de `autoriza`/`rechaza`:

| Acción | Bloque | Efecto |
|---|---|---|
| `repetir` | `repetidor` | Reasigna dentro de la **misma** tarea (matrices de autorización) |
| `proceso` | `nuevoProceso[etapa]` | Folio hijo independiente en paralelo |
| `subproceso` | `subProceso` | Desvío embebido; el padre queda en espera |

Detalle y ejemplos → [Vista de Desarrollo §3.8](03-vista-desarrollo.md#sucesor)

## Formularios (frontend)

Llaves obligatorias de cada `form`: `typeForm`, `blockForm`, `validForm`, `refForm`, `propsForm`.

- `typeForm` frecuentes: `Formulario`, `FormularioDinamico`, `FormularioFiltro`, `FormularioMultiple`, `FormularioLista`, `FormularioTabla`, `FormularioArchivos`, `VisualizadorDocumentos`.
- `blockForm=true` bloquea la UI **y** el backend ignora el valor; `readOnly` bloquea la UI pero el backend **sí** lo recibe.
- Los campos de un `Formulario` van en `propsForm.fields` (por filas); cada input necesita `ref` y `type`.

Catálogo de `typeForm` y `propsForm` → [Vista de Desarrollo §3.4](03-vista-desarrollo.md) · inventario en [`Inventarios/heron-inventario-componentes-frontend.xlsx`](../Inventarios/heron-inventario-componentes-frontend.xlsx)

**Ejemplo JSON por tipo de formulario** (con tipos de input) → [`EjemplosBase/FormulariosFrontend/`](../EjemplosBase/FormulariosFrontend/)

## Parámetros de las funciones

```json
{ "formulario": "datosGenerales", "valor": "rfcContratante" }   // ruta fija en datosTarea
{ "formulario": null, "valor": "VALOR_FIJO" }                    // valor fijo (literal)
```

`parametrosDinamicos` busca la llave en cualquier rama del árbol (`filtro`, `indexUnion`, `indexValor`). → [Vista de Desarrollo §3.6](03-vista-desarrollo.md)

## Consultas (fase `consulta`)

Trae datos y **sí** persiste en `datosTarea`. Cada función lleva `funcion`, `parametros` y `valores` (dónde se guarda el resultado):

```json
{ "funcion": "consultarVigenciaPoliza",
  "parametros": { "noPoliza": { "formulario": "datosPoliza", "valor": "noPoliza" } },
  "valores": { "datosPoliza.estatusPoliza": { "formulario": "consulta", "valor": "estatus" } } }
```

Los datos devueltos solo se pintan si sus llaves coinciden literal con los `ref` de los inputs. Inventario de funciones → [`Inventarios/heron-inventario-funciones-apis.xlsx`](../Inventarios/heron-inventario-funciones-apis.xlsx)

## Visibilidad y configuración (`ConfigHeron`)

- Para que un proceso aparezca en **"Nuevo Trámite"**: `ProcesosHeron.vigencia != "0"` **y** entrada en `ConfigHeron "Accesos"` para el tipo de usuario/área.
- La configuración por usuario resuelve en cascada, gana la más específica: **RFC → Rol → Área → `*`**.
- Documentos de `ConfigHeron`: `Accesos`, `Barra Navegación`, `Bandejas`, `Pantallas`, `Procesos`, `Sucesion`, `Regex`, `SLA`, `Robot Acciones`/`Procesos`/`SubProcesos`/`Trámites`…

Detalle → [Playbook §10](08-playbook-nuevo-tramite.md) · [Vista Lógica §2.3](02-vista-logica.md)

## Errores más frecuentes

| Síntoma | Causa típica |
|---|---|
| No aparece en "Nuevo Trámite" | `vigencia="0"` o falta acceso en `ConfigHeron "Accesos"` |
| La pantalla no renderiza (sin error) | `refForm` o valor de campo dinámico mal escrito |
| Error 500 al avanzar | Falta una llave de una fase en alguna etapa (debe existir en todas, aunque sea `null`) |
| Cambié de pantalla pero el grafo no avanzó | Se usó `AccionesEtapa` en vez de `AccionesTramite` |
| Folio en nodo final sigue "Procesando" | No se fijó `estatus: 3`/`2` en la opción de `sucesion` |

Tabla completa de diagnóstico → [Playbook §15](08-playbook-nuevo-tramite.md)

---

<!-- nav -->
<table width="100%">
<tr>
<td align="left">&nbsp;</td>
<td align="center"><a href="../../../README.md">Principal</a></td>
<td align="right"><a href="01-introduccion.md">Siguiente: Introducción →</a></td>
</tr>
</table>
