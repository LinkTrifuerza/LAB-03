## 1. Verificación de ventana de cambio (`CAM-2026-014`)

Se registró la solicitud `CAM-2026-014` (`Agregar columna canal_alta a clientes`, ambiente `produccion`, aprobada por `Comité de cambios`), se ejecutó `ALTER TABLE clientes ADD COLUMN canal_alta VARCHAR(20);` (línea 94 del log `postgresql-2026-11-25_084411.log`) y su comentario correspondiente dentro de la ventana vigente (`dentro_de_ventana = t`).

---

## 2. Conciliación entre bitácora (`log/postgresql-2026-11-25_084411.log`) y solicitudes de cambio (`gobierno.solicitudes_cambio`)

| Momento (bitácora)     | Usuario                  | Sentencia / Evento (línea)                                             |            Folio que lo autoriza            |          ¿Autorizado?          | Hallazgo / Observación                                                                                                                              |
| :---------------------- | :----------------------- | :---------------------------------------------------------------------- | :------------------------------------------: | :------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2026-11-25 09:34:51` | No registrado (`NULL`) | `ALTER SYSTEM SET log_statement = 'mod';` (línea 20)                 | Sin folio en `gobierno.solicitudes_cambio` |   No / Preparación de entorno   | Ajuste de configuración del servidor durante la preparación del laboratorio; `usuario` no queda identificado por el formato actual de bitácora. |
| `2026-11-25 09:35:00` | No registrado (`NULL`) | `ALTER SYSTEM SET log_statement = 'mod';` (línea 33)                 | Sin folio en `gobierno.solicitudes_cambio` |   No / Preparación de entorno   | Segunda ejecución del ajuste de `log_statement` durante la preparación inicial.                                                                  |
| `2026-11-25 09:35:41` | No registrado (`NULL`) | `CREATE OR REPLACE VIEW dba.bitacora AS ...` (línea 35)              | Sin folio en `gobierno.solicitudes_cambio` |   No / Preparación de entorno   | Creación/actualización de la vista `dba.bitacora` en el entorno de laboratorio sin folio previo en `gobierno.solicitudes_cambio`.              |
| `2026-11-25 10:19:44` | No registrado (`NULL`) | `CREATE SCHEMA gobierno;` (línea 77)                                 | Sin folio en `gobierno.solicitudes_cambio` | No / Preparación de Actividad 7 | Creación del esquema `gobierno` para alojar el registro de solicitudes de cambio (requerimiento de la Actividad 7).                               |
| `2026-11-25 10:19:53` | No registrado (`NULL`) | `REVOKE ALL ON SCHEMA gobierno FROM PUBLIC;` (línea 78)              | Sin folio en `gobierno.solicitudes_cambio` | No / Preparación de Actividad 7 | Restricción de acceso público al esquema `gobierno` al inicializar la estructura de control.                                                     |
| `2026-11-25 10:19:59` | No registrado (`NULL`) | `CREATE TABLE gobierno.solicitudes_cambio (...)` (línea 79)          | Sin folio en `gobierno.solicitudes_cambio` | No / Preparación de Actividad 7 | Creación de la tabla de control `gobierno.solicitudes_cambio` previa al registro del folio `CAM-2026-014`.                                      |
| `2026-11-25 10:20:21` | No registrado (`NULL`) | `ALTER TABLE clientes ADD COLUMN canal_alta VARCHAR(20);` (línea 94) |               `CAM-2026-014`               |               Sí               | Cambio autorizado por `Comité de cambios` y ejecutado dentro de su ventana vigente.                                                               |

> **Limitaciones y huecos observados:** (1) El campo `usuario` resulta `NULL` al extraer `user=` porque el prefijo de bitácora actual no registra el nombre de usuario en ese formato; (2) cambios ejecutados antes de crear `gobierno.solicitudes_cambio` carecen de folio; (3) cualquier periodo con `log_statement` distinto de `'mod'`/`'all'` o `'none'` deja sentencias fuera de la bitácora.

---

## 3. Preguntas de análisis

### 1. ¿Qué cambios de la bitácora no tienen folio? Clasifícalos: ¿son de preparación del laboratorio o representan hallazgos?

Los cambios anteriores al registro del folio `CAM-2026-014` (ajustes de `ALTER SYSTEM SET log_statement`, creación de `dba.bitacora` y creación/aseguramiento del esquema y tabla `gobierno.solicitudes_cambio`) no tienen folio en esa tabla. En este contexto corresponden a la preparación y montaje de las actividades del laboratorio; no obstante, en un entorno productivo real, cualquier cambio de estructura, configuración o privilegios sin folio ni ventana aprobada se clasificaría como hallazgo de control.

### 2. La desactivación del disparador del LAB-01 (Actividad 14), ¿qué tan grave es como hallazgo? ¿Por qué? ¿Qué distingue la línea 163 de la 169?

Desactivar el disparador de auditoría sobre `clientes` tiene una gravedad **alta** en producción porque suspende la captura automática de cambios sobre datos sensibles en `auditoria.cambios_clientes`, generando puntos ciegos de trazabilidad. En el escenario de referencia de la guía, la distinción entre la línea 163 (`app_ventas`) y la línea 169 (`postgres`) radica en el **usuario/rol que ejecuta la sentencia** (un rol de aplicación frente al rol administrativo), lo que permite diferenciar si una cuenta de aplicación o un administrador realizó la operación.

### 3. ¿Qué limitación tiene esta conciliación si `log_statement` estuvo en `none` durante algún periodo?

Si `log_statement` estuvo en `none`, el motor no registró en la bitácora las sentencias ejecutadas durante ese tramo, por lo que no puede comprobarse mediante el log si ocurrieron modificaciones de estructura, privilegios o objetos sin autorización durante dicho periodo.

### 4. ¿Quién debería realizar la conciliación: el DBA que hizo los cambios u otra persona?

Debe realizarla o validarla de forma independiente **otra persona o área distinta a quien ejecutó los cambios** (por ejemplo, auditoría interna o seguridad de la información), evitando que quien ejecuta y opera el cambio sea el mismo que certifica su propia conformidad; el custodio/DBA provee los datos técnicos y la evidencia.
