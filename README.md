# Arquitectura 4+1 — MIDAS / Héron

Documentación de arquitectura del sistema MIDAS de HIR Seguros, centrada en el motor de flujos **Héron**. Está escrita en Markdown y organizada en varios archivos para poder consultarse directamente desde el repositorio.

El documento sigue el modelo **4+1** de Philippe Kruchten. Cada vista describe el sistema desde una perspectiva distinta y puede leerse de forma independiente; la lectura en orden ofrece el recorrido completo, desde la comunicación entre componentes hasta la ejecución asíncrona en segundo plano.

## Contenido

| # | Documento | Contenido |
|---|---|---|
| — | [README](README.md) | Portada, índice y control de versiones |
| 1 | [Introducción](01-introduccion.md) | Contexto del sistema, generaciones y alcance |
| 2 | [Vista Lógica](02-vista-logica.md) | Motor Héron, procesos/tareas/etapas, tracking, sucesor y cadenas Autoriza/Rechaza, modelo de datos |
| 3 | [Vista de Desarrollo](03-vista-desarrollo.md) | Organización del código, estructura del JSON, variantes de frontend, pipeline de fases, formularios, parámetros y APIs |
| 4 | [Vista de Procesos](04-vista-procesos.md) | Runtime, autenticación, ciclo de vida del trámite, SLA, robots y ejecución paralela |
| 5 | [Vista Física](05-vista-fisica.md) | Topología de despliegue, recursos, entornos, documentos e integraciones |
| 6 | [Escenarios (+1)](06-escenarios.md) | Casos de uso que integran las cuatro vistas |
| 7 | [Decisiones arquitectónicas](07-decisiones.md) | Decisiones relevantes y su justificación |
| 8 | [Playbook: Nuevo Trámite](08-playbook-nuevo-tramite.md) | Guía operativa paso a paso para implementar un flujo (proceso, tareas, permisos, visibilidad) |
| 9 | [Glosario](09-glosario.md) | Términos del dominio y de configuración de Héron |

Los diagramas se referencian como imágenes **SVG** desde cada documento y viven en [`diagramas/`](diagramas/), junto con su fuente **Mermaid** (`.mmd`) editable. El SVG es lo que se ve al leer la documentación; el `.mmd` es la fuente de verdad y se regenera a SVG con `mmdc`. El detalle y el índice completo están en [`diagramas/README.md`](diagramas/README.md).

## Alcance

El foco de esta documentación es el motor Héron: cómo orquesta los trámites, cómo se configura mediante JSON y cómo se ejecuta. La guía operativa para configurar un trámite concreto —el [Playbook: Nuevo Trámite](08-playbook-nuevo-tramite.md)— se incluye como documento 8: es un manual paso a paso que parte de esta arquitectura sin repetirla, y remite a las vistas 4+1 para el detalle conceptual (por ejemplo, el pipeline de fases y el árbol de condiciones if/elif/switch viven en la [Vista de Desarrollo](03-vista-desarrollo.md)).

## Control de versiones

| Versión | Fecha | Descripción |
|---|---|---|
| v0.1 | mayo 2026 | Borrador inicial |
| v0.2 | junio 2026 | Documento 4+1 completo |
| v0.3 | junio 2026 | Detalle de la relación JSON ↔ ejecutor de Python |
| v0.4 | julio 2026 | Reestructuración a formato Markdown multi-archivo; reagrupación del contenido en torno al flujo del motor y ampliación del detalle en la Vista de Desarrollo |
| v0.5 | julio 2026 | Integración del Playbook de Nuevo Trámite; sección de árbol de condiciones (if/elif/switch con comparación en Python) en la Vista de Desarrollo; diagramas como SVG con fuente `.mmd` |
| v0.6 | julio 2026 | Playbook con capturas del panel de administración; reordenamiento (Glosario como documento final) y glosario ampliado con términos de configuración |

---

<!-- nav -->
<table width="100%">
<tr>
<td align="left">&nbsp;</td>
<td align="center">&nbsp;</td>
<td align="right"><a href="01-introduccion.md">Siguiente: Introducción →</a></td>
</tr>
</table>
