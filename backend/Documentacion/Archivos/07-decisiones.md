# 7. Decisiones arquitectónicas relevantes

Este apartado reúne las decisiones de diseño que más condicionan la operación y el mantenimiento de la plataforma, junto con su justificación y su implicación práctica.

| Decisión | Justificación | Implicación práctica |
|---|---|---|
| Configuración de Héron en MongoDB | Cambiar procesos sin desplegar código | Un cambio de proceso es una edición de documento JSON; sin migración ni redeploy |
| Hash MD5 en URLs | Ofusca la superficie de endpoints | No es seguridad criptográfica; la autenticación Django es la capa real |
| Router por `db_table` (Djongo) | Reutiliza el ORM de Django para MongoDB | Limitaciones en agregaciones complejas; se recurre a PyMongo directo cuando hace falta |
| JWT corto + refresco transparente | Reduce la ventana de exposición de tokens | Latencia mínima por la verificación de refresco en cada request |
| Git submodule para `components/` | Compartir UI entre proyectos HIR | `git submodule update` es obligatorio al clonar |
| Variantes de frontend por rol | El backend sirve una vista distinta según el perfil | La clave `backend`, compartida, actúa como segunda línea de validación |
| `proceso` / `subproceso` diferidos | Desacopla operaciones largas del ciclo HTTP | Ventana de hasta 5 minutos hasta la ejecución; verificable en `ConfigHeron` |
| Lista blanca de conexiones por ambiente | Acota la superficie de red permitida | Cada entorno declara explícitamente las conexiones que el sistema puede realizar |
| Bitácora derivada de `TrackingHeron` | Evita duplicar el historial en otra colección | La trazabilidad se reconstruye desde el tracking; no hay una colección de bitácoras que mantener |
| Único nodo AWS EC2 | Simplicidad operativa para el volumen actual | Sin alta disponibilidad nativa; el respaldo de 12 horas es la única red de seguridad |

---

<!-- nav -->
<table width="100%">
<tr>
<td align="left"><a href="06-escenarios.md">← Anterior: Escenarios</a></td>
<td align="center"><a href="../../../README.md">Principal</a></td>
<td align="right"><a href="08-playbook-nuevo-tramite.md">Siguiente: Playbook: Nuevo Trámite →</a></td>
</tr>
</table>
