
# Actividad 4 — Catálogo mínimo de datos

## 1. Inventario técnico y de gobierno de datos

| Motor      | Servidor      | Base / Esquema                | Objeto / Colección  | Propietario de negocio                            | Custodio técnico            | Clasificación                  |  Filas / Docs estimados  |         Tamaño         | Descripción / Notas                                                                                                                          |
| :--------- | :------------ | :---------------------------- | :------------------- | :------------------------------------------------ | :--------------------------- | :------------------------------ | :----------------------: | :----------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------- |
| PostgreSQL | `localhost` | `comercial` / `public`    | `clientes`         | Gerencia Comercial                                | DBA (`postgres`)           | Confidencial (datos personales) |          `2`          |        `32 kB`        | Clientes de Comercializadora Norte. Propietario: Gerencia Comercial. Custodio: DBA. Clasificación: confidencial (contiene datos personales). |
| PostgreSQL | `localhost` | `comercial` / `auditoria` | `cambios_clientes` | Auditoría Interna / Seguridad de la Información | DBA (`postgres`)           | Confidencial / Auditoría       |          `2`          |        `32 kB`        | Bitácora de cambios (`INSERT`, `UPDATE`, `DELETE`) disparada sobre `public.clientes` (sin `COMMENT` registrado en `pg_class`).   |
| MongoDB    | `localhost` | `comercial`                 | `clientes`         | Gerencia Comercial                                | DBA / Administrador de datos | Confidencial (datos personales) | Según instancia MongoDB | Según instancia MongoDB | Colección de clientes en MongoDB (referencia del entorno de datos del laboratorio).                                                          |
| MongoDB    | `localhost` | `comercial`                 | `system.profile`   | Seguridad de la Información / DBA                | DBA                          | Restringida / Diagnóstico      | Según instancia MongoDB | Según instancia MongoDB | Perfilado de operaciones en MongoDB (`system.profile`, referencia del entorno de datos del laboratorio).                                    |

---

## 2. Preguntas de análisis

### 1. ¿Qué diferencias hay entre los metadatos que entrega el motor y la información de gobierno que debe agregarse a mano?

El motor entrega datos técnicos y operativos internos—como esquema, nombre del objeto, propietario técnico (`relowner`), estimación de filas (`reltuples`), tamaño en disco (`pg_total_relation_size`) y descripciones breves (`obj_description`)—, pero no conoce por sí solo el área de negocio propietaria, la clasificación de confidencialidad o privacidad, las reglas de retención ni la criticidad organizacional. Esa información debe completarse y mantenerse con criterios de gobierno de datos.

### 2. ¿Por qué `ANALYZE` es importante antes de consultar `reltuples`?

Porque `pg_class.reltuples` no se recalcula en tiempo real con cada cambio de fila, sino que refleja las estadísticas recolectadas por el planificador. Ejecutar `ANALYZE` actualiza esas estadísticas para que la estimación de filas refleje de forma cercana el estado actual de la tabla en lugar de valores desactualizados o iniciales (`-1` o `0`).

### 3. ¿Para qué sirve tener este catálogo mínimo antes de definir políticas de retención o respaldo?

Porque permite identificar qué tablas o colecciones contienen datos sensibles o críticos para el negocio, cuál es su volumen/tamaño aproximado y quién responde por cada conjunto de datos. Con esa visibilidad es posible priorizar respaldos, fijar tiempos de retención y borrado acordes con su valor y riesgo, y evitar tratar por igual una tabla crítica de clientes que datos temporales o de diagnóstico.
