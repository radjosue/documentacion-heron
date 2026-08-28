# Ejemplos base — Pipeline del backend

Un ejemplo de configuración JSON por cada fase del pipeline de `AccionesTramite`, en su orden de ejecución. Cada archivo muestra el bloque completo `backend[accionId][fase][etapa]` listo para copiar y adaptar.

Las **llaves y la estructura** de cada fase están verificadas contra el código del motor (`backend/apis/Heron/herramientas/*API.py`). Los **nombres de función y sus parámetros** son ilustrativos; el inventario completo de funciones invocables está en [`Inventarios/heron-inventario-funciones-apis.xlsx`](../../Inventarios/heron-inventario-funciones-apis.xlsx).

| # | Fase | Archivo | Llave y estructura |
|---|---|---|---|
| 1 | Depuración | [`01-depuracion.json`](01-depuracion.json) | `depuracion` → `funcionesDepuracion` + `camposDepuraciones` |
| 2 | Prevalidación | [`02-prevalidacion.json`](02-prevalidacion.json) | `preValidacion` → `estatus` + `camposObligatorios` + `camposValidaciones` |
| 3 | Validación | [`03-validacion.json`](03-validacion.json) | `validacion` → `estatus` + `camposObligatorios` + `camposValidaciones` (con árbol `opciones`) |
| 4 | Ensamblaje | [`04-ensamblaje.json`](04-ensamblaje.json) | `ensamblaje` → `funcionesEnsamble` + `camposEnsamble` |
| 5 | Postvalidación | [`05-postvalidacion.json`](05-postvalidacion.json) | `postValidacion` → `estatus` + `camposObligatorios` + `camposValidaciones` |
| 6 | Consulta | [`06-consulta.json`](06-consulta.json) | `consulta` → `funcionesConsulta` + `camposConsultas` (con `valores`) |
| 7 | Actualización | [`07-actualizacion.json`](07-actualizacion.json) | `actualizacion` → `funcionesActualizacion` + `camposActualizaciones` (con `valores`) |
| 8 | Acción | [`08-accion.json`](08-accion.json) | `accion` → `funcionesAccion` + `camposAcciones` (sin `valores`) |
| 9 | SubAcción | [`09-subaccion.json`](09-subaccion.json) | `subAccion` → `funcionesAccion` + `camposAcciones` (encolada) |
| 10 | Obtención | [`10-obtencion.json`](10-obtencion.json) | `obtencion` → `funcionesObtencion` + `camposObtenciones` (con `valores`) |
| 11 | Filtro | [`11-filtro.json`](11-filtro.json) | `filtro` → `funcionesFiltro` + `camposFiltros` (sin `valores`) |
| 12 | Sucesión | [`12-sucesion.json`](12-sucesion.json) | `sucesion` → `sucesor` + `validacionSucesion` (**no** se indexa por etapa) |

Convenciones que comparten:

- Las fases de **escritura/datos** (depuración, ensamblaje, consulta, actualización, obtención) usan el par `funciones{Fase}` (dict keyed por `nombreFuncion~alias`, se ejecuta siempre) + `campos{Fase}` (árbol condicional if/switch). Cada función lleva `parametros`, `parametrosDinamicos` y —salvo acción/filtro— `valores` (dónde se guarda el resultado).
- Cada ejemplo de **escritura/datos/acción** (`01`, `04`, `06`–`11`) incluye **una validación** en su `campos{Fase}`: una regla `operador~alias` con sus `opciones`, cuyo nodo válido aplica las funciones solo si la condición se cumple (mismo mecanismo que `camposValidaciones`; `funciones{Fase}` a nivel de la regla es el *fallback* si no coincide ninguna opción). No van anidadas.
- Las fases de **validación** (prevalidación, validación, postvalidación) usan `estatus` + `camposObligatorios` + `camposValidaciones`.
- Si una fase se usa en alguna etapa, debe existir (aunque sea `null`) en todas las etapas de esa fase, o el core lanza error 500.

Descripción completa → [Vista de Desarrollo §3.5](../../Archivos/03-vista-desarrollo.md#pipeline)
