# 1. Introducción

MIDAS es el sistema central de operación de HIR Seguros. Su arquitectura responde a dos necesidades complementarias: administrar con precisión los datos estructurados del negocio asegurador —pólizas, siniestros, contabilidad— y sostener un motor de flujos de trabajo configurable que permita evolucionar los procesos operativos sin modificar el código base.

El sistema tiene dos generaciones activas en producción:

- **MIDAS 1.0** — sistema legado en SQL Server con el historial operativo de HIR.
- **MIDAS 2.0** — plataforma actual: Django REST Framework en el backend, Next.js en el frontend.

El componente central de MIDAS 2.0 es **Héron**: un motor de workflow cuya configuración vive íntegramente como documentos JSON en MongoDB. Héron es lo que permite definir, modificar y evolucionar cualquier trámite —cotización, emisión, siniestro, endoso— sin tocar el código del backend ni del frontend.

## El modelo 4+1 aplicado a Héron

Este documento aplica el modelo 4+1 de Philippe Kruchten. Las cuatro vistas —lógica, de desarrollo, de procesos y física— describen el sistema desde perspectivas complementarias; la vista de escenarios las integra mediante casos de uso representativos.

El contenido está organizado para acompañar la forma real en que se comprende el motor. Primero se explica cómo se comunican y se mueven los trámites entre áreas y usuarios (Vista Lógica); después, cómo se construye y ejecuta el JSON que define cada flujo, con el detalle técnico que necesita un desarrollador (Vista de Desarrollo); y por último, cómo opera el motor cuando el trabajo ocurre fuera del ciclo HTTP tradicional, en robots y procesos paralelos (Vista de Procesos). La Vista Física describe dónde se despliega todo esto.

| Vista | Pregunta que responde |
|---|---|
| Lógica | ¿Qué es un trámite y cómo se mueve dentro del motor? |
| Desarrollo | ¿Cómo se construye, se renderiza y se ejecuta el JSON que define un flujo? |
| Procesos | ¿Cómo opera el sistema en ejecución, incluyendo el trabajo asíncrono? |
| Física | ¿Dónde y sobre qué infraestructura corre? |
| Escenarios | ¿Cómo se combinan las capacidades anteriores en casos reales? |

## A quién está dirigido

La documentación está pensada para desarrolladores y arquitectos que van a mantener o evolucionar la plataforma. El objetivo es dejar una base técnica clara para operar Héron con menor dependencia del conocimiento informal, reduciendo la curva de aprendizaje de quienes se incorporan al equipo.

La guía para configurar un trámite específico —el [Playbook: Nuevo Trámite](08-playbook-nuevo-tramite.md)— acompaña a este documento (capítulo 8): es un manual operativo paso a paso. Este texto se concentra en la arquitectura: qué hace cada componente, cómo se relacionan y por qué están construidos de esa forma.

---

<!-- nav -->
<table width="100%">
<tr>
<td align="left"><a href="00-guia-rapida.md">← Anterior: Guía rápida</a></td>
<td align="center"><a href="../../../README.md">Principal</a></td>
<td align="right"><a href="02-vista-logica.md">Siguiente: Vista Lógica →</a></td>
</tr>
</table>
