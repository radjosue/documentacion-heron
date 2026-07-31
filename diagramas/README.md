# Diagramas — Arquitectura 4+1 MIDAS / Héron

Diagramas de la documentación en dos formatos: el **`.svg`** (imagen renderizada, que es la que referencian los documentos `.md`) y el **`.mmd`** (fuente Mermaid editable). Ambos se conservan: el `.mmd` es la fuente de verdad y el `.svg` su salida.

La nomenclatura es `XX-YY-descripcion`, donde `XX` es el número de la vista de origen y `YY` el orden del diagrama dentro de esa vista.

Para regenerar un SVG después de editar su fuente `.mmd`:

```bash
mmdc -i 03-03-pipeline-consolidado.mmd -o 03-03-pipeline-consolidado.svg -b white
```

| Diagrama | Descripción | SVG | Fuente | Vista |
|---|---|---|---|---|
| `02-01-modelo-proceso-tarea-etapa` | Modelo conceptual Proceso / Tarea / Etapa | [svg](02-01-modelo-proceso-tarea-etapa.svg) | [mmd](02-01-modelo-proceso-tarea-etapa.mmd) | [02-vista-logica.md](../02-vista-logica.md) |
| `02-02-tracking-folio` | Evolución de los registros de TrackingHeron de un folio | [svg](02-02-tracking-folio.svg) | [mmd](02-02-tracking-folio.mmd) | [02-vista-logica.md](../02-vista-logica.md) |
| `02-03-sucesor-autoriza-rechaza` | Decisión del sucesor: Autoriza / Rechaza | [svg](02-03-sucesor-autoriza-rechaza.svg) | [mmd](02-03-sucesor-autoriza-rechaza.mmd) | [02-vista-logica.md](../02-vista-logica.md) |
| `02-04-cambio-bandejas` | Recorrido de un folio entre bandejas | [svg](02-04-cambio-bandejas.svg) | [mmd](02-04-cambio-bandejas.mmd) | [02-vista-logica.md](../02-vista-logica.md) |
| `02-05-ciclo-vida-folio` | Ciclo de vida de un folio | [svg](02-05-ciclo-vida-folio.svg) | [mmd](02-05-ciclo-vida-folio.mmd) | [02-vista-logica.md](../02-vista-logica.md) |
| `02-06-modulos-sistema` | Módulos del sistema y Héron como orquestador | [svg](02-06-modulos-sistema.svg) | [mmd](02-06-modulos-sistema.mmd) | [02-vista-logica.md](../02-vista-logica.md) |
| `02-07-modelo-datos` | Modelo de datos: PostgreSQL, MongoDB y SQL Server | [svg](02-07-modelo-datos.svg) | [mmd](02-07-modelo-datos.mmd) | [02-vista-logica.md](../02-vista-logica.md) |
| `03-01-estructura-frontend` | Capas del frontend | [svg](03-01-estructura-frontend.svg) | [mmd](03-01-estructura-frontend.mmd) | [03-vista-desarrollo.md](../03-vista-desarrollo.md) |
| `03-02-flujo-dependencias-pantalla` | Flujo de dependencias de una pantalla de Héron | [svg](03-02-flujo-dependencias-pantalla.svg) | [mmd](03-02-flujo-dependencias-pantalla.mmd) | [03-vista-desarrollo.md](../03-vista-desarrollo.md) |
| `03-03-pipeline-consolidado` | Grafo consolidado del pipeline de fases | [svg](03-03-pipeline-consolidado.svg) | [mmd](03-03-pipeline-consolidado.mmd) | [03-vista-desarrollo.md](../03-vista-desarrollo.md) |
| `03-04-arbol-condiciones` | Evaluación del árbol de condiciones (if / elif / switch) | [svg](03-04-arbol-condiciones.svg) | [mmd](03-04-arbol-condiciones.mmd) | [03-vista-desarrollo.md](../03-vista-desarrollo.md) |
| `04-01-procesos-runtime` | Procesos en tiempo de ejecución | [svg](04-01-procesos-runtime.svg) | [mmd](04-01-procesos-runtime.mmd) | [04-vista-procesos.md](../04-vista-procesos.md) |
| `04-02-workers-gunicorn` | Workers de Gunicorn y proceso de Next.js | [svg](04-02-workers-gunicorn.svg) | [mmd](04-02-workers-gunicorn.mmd) | [04-vista-procesos.md](../04-vista-procesos.md) |
| `04-03-autenticacion-jwt` | Autenticación y refresco de JWT | [svg](04-03-autenticacion-jwt.svg) | [mmd](04-03-autenticacion-jwt.mmd) | [04-vista-procesos.md](../04-vista-procesos.md) |
| `04-04-ciclo-vida-tramite` | Ciclo de vida de un trámite | [svg](04-04-ciclo-vida-tramite.svg) | [mmd](04-04-ciclo-vida-tramite.mmd) | [04-vista-procesos.md](../04-vista-procesos.md) |
| `04-05-proceso-paralelo` | Proceso paralelo independiente | [svg](04-05-proceso-paralelo.svg) | [mmd](04-05-proceso-paralelo.mmd) | [04-vista-procesos.md](../04-vista-procesos.md) |
| `04-06-subproceso-embebido` | Subproceso embebido con folio en espera | [svg](04-06-subproceso-embebido.svg) | [mmd](04-06-subproceso-embebido.mmd) | [04-vista-procesos.md](../04-vista-procesos.md) |
| `05-01-topologia-despliegue` | Topología de despliegue en AWS EC2 | [svg](05-01-topologia-despliegue.svg) | [mmd](05-01-topologia-despliegue.mmd) | [05-vista-fisica.md](../05-vista-fisica.md) |
| `05-02-generacion-documentos` | Cadena de generación de documentos | [svg](05-02-generacion-documentos.svg) | [mmd](05-02-generacion-documentos.mmd) | [05-vista-fisica.md](../05-vista-fisica.md) |
| `05-03-integraciones-externas` | Integraciones externas del backend | [svg](05-03-integraciones-externas.svg) | [mmd](05-03-integraciones-externas.mmd) | [05-vista-fisica.md](../05-vista-fisica.md) |
| `06-01-avance-inter-tarea` | Escenario 1: avance inter-tarea y cambio de bandeja | [svg](06-01-avance-inter-tarea.svg) | [mmd](06-01-avance-inter-tarea.mmd) | [06-escenarios.md](../06-escenarios.md) |
| `06-02-proceso-paralelo` | Escenario 3: proceso paralelo independiente | [svg](06-02-proceso-paralelo.svg) | [mmd](06-02-proceso-paralelo.mmd) | [06-escenarios.md](../06-escenarios.md) |
| `06-03-subproceso-embebido` | Escenario 4: subproceso embebido | [svg](06-03-subproceso-embebido.svg) | [mmd](06-03-subproceso-embebido.mmd) | [06-escenarios.md](../06-escenarios.md) |
| `06-04-folio-automatico-robot` | Escenario 5: folio generado por robot | [svg](06-04-folio-automatico-robot.svg) | [mmd](06-04-folio-automatico-robot.mmd) | [06-escenarios.md](../06-escenarios.md) |
| `09-01-descomposicion-ejemplo` | Descomposición del requerimiento en proceso, tareas y áreas | [svg](09-01-descomposicion-ejemplo.svg) | [mmd](09-01-descomposicion-ejemplo.mmd) | [09-playbook-nuevo-tramite.md](../09-playbook-nuevo-tramite.md) |
| `09-02-cascada-especificidad` | Cascada de especificidad de permisos y configuración | [svg](09-02-cascada-especificidad.svg) | [mmd](09-02-cascada-especificidad.mmd) | [09-playbook-nuevo-tramite.md](../09-playbook-nuevo-tramite.md) |
