# Ejemplos base — Formularios del frontend (`typeForm`)

Un ejemplo de configuración JSON por cada componente del inventario de `typeForm` (26). Cada archivo muestra el objeto de un formulario tal como se declara dentro de `frontend.etapas[].forms[]`, con sus `propsForm` y, cuando aplica, sus campos (`fields`) con distintos tipos de input.

La numeración `NN` coincide con la fila del inventario [`Inventarios/heron-inventario-componentes-frontend.xlsx`](../../Inventarios/heron-inventario-componentes-frontend.xlsx) (hoja *Componentes typeForm*).
Las **props y la estructura** están tomadas de ese inventario.
Los **nombres de campo, catálogos y URLs** son ilustrativos.

## Montables vía `typeForm` (20)

Son los 20 valores de `typeForm` que resuelve el `switch` de `Tramite.js`; se declaran directamente en `forms[]`. Forma del objeto: `{ typeForm, blockForm, validForm, refForm, propsForm }` (el `datosIniciales` lo inyecta el backend).

| # | typeForm | Requiere `url` | Ejemplo |
|---|---|---|---|
| 1 | `Cuestionario` | No | [01-cuestionario.json](01-cuestionario.json) |
| 2 | `DatosPersonales` | No | [02-datospersonales.json](02-datospersonales.json) |
| 3 | `Domicilio` | No | [03-domicilio.json](03-domicilio.json) |
| 4 | `Formulario` | No | [04-formulario.json](04-formulario.json) — *showcase de tipos de input* |
| 5 | `FormularioArchivos` | No | [05-formularioarchivos.json](05-formularioarchivos.json) |
| 6 | `FormularioCoberturas` | **Sí** | [06-formulariocoberturas.json](06-formulariocoberturas.json) |
| 7 | `FormularioCondicional` | **Sí** | [07-formulariocondicional.json](07-formulariocondicional.json) |
| 8 | `FormularioConsulta` | **Sí** | [08-formularioconsulta.json](08-formularioconsulta.json) |
| 9 | `FormularioConsultaTabla` | **Sí** | [09-formularioconsultatabla.json](09-formularioconsultatabla.json) |
| 10 | `FormularioDinamico` | No | [10-formulariodinamico.json](10-formulariodinamico.json) |
| 11 | `FormularioFiltro` | **Sí** | [11-formulariofiltro.json](11-formulariofiltro.json) |
| 12 | `FormularioLista` | No | [12-formulariolista.json](12-formulariolista.json) |
| 13 | `FormularioMultiple` | No | [13-formulariomultiple.json](13-formulariomultiple.json) |
| 14 | `FormularioSelect` | **Sí** | [14-formularioselect.json](14-formularioselect.json) |
| 15 | `FormularioTabla` | No | [15-formulariotabla.json](15-formulariotabla.json) |
| 16 | `FormularioTabs` | No | [16-formulariotabs.json](16-formulariotabs.json) |
| 17 | `FormularioVisualizador` | **Sí** | [17-formulariovisualizador.json](17-formulariovisualizador.json) |
| 18 | `RegistroBeneficiarios` | No | [18-registrobeneficiarios.json](18-registrobeneficiarios.json) |
| 19 | `Tabla` | No | [19-tabla.json](19-tabla.json) — *solo lectura, se excluye del submit* |
| 20 | `VisualizadorDocumentos` | **Sí** | [20-visualizadordocumentos.json](20-visualizadordocumentos.json) |

## No montables vía `typeForm` (6)

El inventario los lista como componentes, pero **no aparecen en el `switch` de `Tramite.js`**: se usan directamente en páginas o dentro de otros componentes, no como entrada de `forms[]`. Sus ejemplos muestran el objeto de props que reciben (marcados con `"montableViaTypeForm": false`).

| # | Componente | Uso | Ejemplo |
|---|---|---|---|
| 21 | `Documentos` | Lo usa `PaqueteConfigurable` | [21-documentos.json](21-documentos.json) |
| 22 | `Formulario` (variante Servidor) | Import directo en páginas | [22-formulario-servidor.json](22-formulario-servidor.json) |
| 23 | `FormularioContext` | Interno de `FormularioLista` | [23-formulariocontext.json](23-formulariocontext.json) |
| 24 | `FormularioDinamico` (variante Servidor) | Import directo en páginas | [24-formulariodinamico-servidor.json](24-formulariodinamico-servidor.json) |
| 25 | `PagoTarjeta` | Flujos de venta | [25-pagotarjeta.json](25-pagotarjeta.json) |
| 26 | `PaqueteConfigurable` | Venta de paquetes | [26-paqueteconfigurable.json](26-paqueteconfigurable.json) |

## Tipos de input (`fields`)

Los componentes con captura directa (`Formulario`, y los sub-formularios anidados) definen sus campos en `propsForm.fields`, un diccionario `{ "fila": { "Etiqueta visible": { ref, type, variant, modifier, config, … } } }`. El ejemplo [04-formulario.json](04-formulario.json) reúne varios `type` representativos: `text`, `textArea`, `select`, `selectObject`, `date`, `checkBox`, `switch`, `int`, `dinero`, `porcentaje`, `rfc`, `curp`, `email`, `telefono`, `codigoPostal`, `ocupacion`, `pais`, `file-pdf`. El catálogo completo (95 `type`) y las llaves de input están en el inventario.

Detalle de componentes y props → [Vista de Desarrollo §3.4](../../Archivos/03-vista-desarrollo.md)
