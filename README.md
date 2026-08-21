# Arquitectura 4+1 — MIDAS / Héron

Documentación de arquitectura del sistema MIDAS de HIR Seguros, centrada en el motor de flujos **Héron**. Está escrita en Markdown y organizada en varios archivos para poder consultarse directamente desde el repositorio.

El documento sigue el modelo **4+1** de Philippe Kruchten. Cada vista describe el sistema desde una perspectiva distinta y puede leerse de forma independiente; la lectura en orden ofrece el recorrido completo, desde la comunicación entre componentes hasta la ejecución asíncrona en segundo plano.

## Estructura del repositorio

| Carpeta | Contenido |
|---|---|
| `backend/Documentacion/Archivos/` | Los documentos de la arquitectura y el playbook (`*.md`) |
| `backend/Documentacion/Diagramas/` | Diagramas: fuente Mermaid (`.mmd`), imagen renderizada (`.svg`) e índice (`README.json`) |
| `backend/Documentacion/Imagenes/` | Capturas de pantalla referenciadas por los documentos |
| `backend/Documentacion/Inventarios/` | Inventarios de funciones y componentes (`.xlsx`) |
| `backend/Documentacion/EjemplosFlujo/` | Ejemplos de configuración JSON de un flujo |
| `backend/Documentacion/EjemplosBase/` | Ejemplos base: por fase del pipeline (`PipelineBackend/`) y por tipo de formulario (`FormulariosFrontend/`) |

## Contenido

| # | Documento | Contenido |
|---|---|---|
| — | [README](README.md) | Portada, índice y control de versiones |
| 0 | [Guía rápida](backend/Documentacion/Archivos/00-guia-rapida.md) | Referencia condensada: nombrado, numeración, flujos, JSON, condiciones, sucesión, formularios y consultas |
| 1 | [Introducción](backend/Documentacion/Archivos/01-introduccion.md) | Contexto del sistema, generaciones y alcance |
| 2 | [Vista Lógica](backend/Documentacion/Archivos/02-vista-logica.md) | Motor Héron, procesos/tareas/etapas, tracking, sucesor y cadenas Autoriza/Rechaza, modelo de datos |
| 3 | [Vista de Desarrollo](backend/Documentacion/Archivos/03-vista-desarrollo.md) | Organización del código, estructura del JSON, variantes de frontend, pipeline de fases, formularios, parámetros y APIs |
| 4 | [Vista de Procesos](backend/Documentacion/Archivos/04-vista-procesos.md) | Runtime, autenticación, ciclo de vida del trámite, SLA, robots y ejecución paralela |
| 5 | [Vista Física](backend/Documentacion/Archivos/05-vista-fisica.md) | Topología de despliegue, recursos, entornos, documentos e integraciones |
| 6 | [Escenarios (+1)](backend/Documentacion/Archivos/06-escenarios.md) | Casos de uso que integran las cuatro vistas |
| 7 | [Decisiones arquitectónicas](backend/Documentacion/Archivos/07-decisiones.md) | Decisiones relevantes y su justificación |
| 8 | [Playbook: Nuevo Trámite](backend/Documentacion/Archivos/08-playbook-nuevo-tramite.md) | Guía operativa paso a paso para implementar un flujo (proceso, tareas, permisos, visibilidad) |
| 9 | [Glosario](backend/Documentacion/Archivos/09-glosario.md) | Términos del dominio y de configuración de Héron |

Los diagramas se referencian como imágenes **SVG** desde cada documento y viven en [`backend/Documentacion/Diagramas/`](backend/Documentacion/Diagramas/), junto con su fuente **Mermaid** (`.mmd`) editable. El SVG es lo que se ve al leer la documentación; el `.mmd` es la fuente de verdad y se regenera a SVG con `mmdc`. El índice completo está en [`Diagramas/README.json`](backend/Documentacion/Diagramas/README.json).

## Alcance

El foco de esta documentación es el motor Héron: cómo orquesta los trámites, cómo se configura mediante JSON y cómo se ejecuta. La guía operativa para configurar un trámite concreto —el [Playbook: Nuevo Trámite](backend/Documentacion/Archivos/08-playbook-nuevo-tramite.md)— se incluye como documento 8: es un manual paso a paso que parte de esta arquitectura sin repetirla, y remite a las vistas 4+1 para el detalle conceptual (por ejemplo, el pipeline de fases y el árbol de condiciones if/elif/switch viven en la [Vista de Desarrollo](backend/Documentacion/Archivos/03-vista-desarrollo.md)).

## Control de versiones

| Versión | Fecha | Descripción |
|---|---|---|
| v0.1 | mayo 2026 | Borrador inicial |
| v0.2 | junio 2026 | Documento 4+1 completo |
| v0.3 | junio 2026 | Detalle de la relación JSON ↔ ejecutor de Python |
| v0.4 | julio 2026 | Reestructuración a formato Markdown multi-archivo; reagrupación del contenido en torno al flujo del motor y ampliación del detalle en la Vista de Desarrollo |
| v0.5 | julio 2026 | Integración del Playbook de Nuevo Trámite; sección de árbol de condiciones (if/elif/switch con comparación en Python) en la Vista de Desarrollo; diagramas como SVG con fuente `.mmd` |
| v0.6 | julio 2026 | Playbook con capturas del panel de administración; reordenamiento (Glosario como documento final) y glosario ampliado con términos de configuración |
| v0.7 | agosto 2026 | Reorganización de la carpeta: documentos en `backend/Documentacion/Archivos` y recursos separados en `Diagramas`, `Imagenes`, `Inventarios` y `EjemplosFlujo` |
| v0.8 | agosto 2026 | Guía rápida (referencia condensada de sintaxis y reglas) como documento inicial |
| v0.9 | agosto 2026 | Ejemplos base de configuración JSON por fase del pipeline (`EjemplosBase/PipelineBackend`); aclaración de las acciones especiales `repetir`/`proceso`/`subproceso` como bloques dedicados en la Vista de Desarrollo |
| v0.10 | agosto 2026 | Ejemplos base de configuración JSON por tipo de formulario del frontend, con tipos de input (`EjemplosBase/FormulariosFrontend`, 26 componentes `typeForm`) |

---

<!-- nav -->
<table width="100%">
<tr>
<td align="left">&nbsp;</td>
<td align="center">&nbsp;</td>
<td align="right"><a href="backend/Documentacion/Archivos/00-guia-rapida.md">Siguiente: Guía rápida →</a></td>
</tr>
</table>
