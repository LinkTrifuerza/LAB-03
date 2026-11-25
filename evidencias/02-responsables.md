
# Actividad 2 — Propietarios, custodios y usuarios

## 1. Matriz de responsables y accesos

| Conjunto de datos                           | Propietario                                                | Custodio                                      | Usuarios autorizados                                                                          | Quién aprueba accesos                                                   | Quién controla cambios                                                                        |
| :------------------------------------------ | :--------------------------------------------------------- | :-------------------------------------------- | :-------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------- |
| `comercial.public.clientes` (PostgreSQL)  | Gerencia Comercial                                         | DBA                                           | Aplicación de ventas (operación autorizada) y roles con acceso restringido según política | Propietario del dato (Gerencia Comercial) y responsable de seguridad/DBA | Comité de cambios / Propietario del dato (ejecuta/coordina el DBA)                            |
| `auditoria.cambios_clientes` (PostgreSQL) | Área de Auditoría Interna / Seguridad de la Información | DBA (con restricción de alteración directa) | Auditoría interna y roles de revisión autorizados (solo lectura)                            | Propietario de la auditoría / Seguridad de la Información              | Comité de cambios / Seguridad de la Información (no debe alterarlo quien opera `clientes`) |
| Bitácora de PostgreSQL                     | Seguridad de la Información / Auditoría Interna          | DBA                                           | Administradores/operación técnica acotada y auditoría interna según rol                   | Seguridad de la Información / Responsable del sistema                   | Comité de cambios / DBA bajo control de configuración                                        |
| `comercial.clientes` (MongoDB)            | Gerencia Comercial                                         | DBA / Administrador de datos                  | Aplicación de tienda/ventas y procesos autorizados                                           | Propietario del dato (Gerencia Comercial) y administración del motor    | Comité de cambios / Propietario del dato                                                      |
| `comercial.system.profile` (MongoDB)      | Seguridad de la Información / DBA                         | DBA                                           | DBA y personal técnico de diagnóstico/auditoría autorizado                                 | Seguridad de la Información / Administración técnica                  | DBA / Seguridad de la Información                                                             |

*(Nota: si cuentas con la matriz de gobierno de datos de la Unidad II con nombres de áreas o roles específicos de tu curso, puedes ajustar las celdas correspondientes manteniendo la separación entre propietario y custodio).*

---

## 2. Preguntas de análisis

### 1. ¿Por qué el propietario de `auditoria.cambios_clientes` no debería ser el mismo que opera `clientes`?

Porque se rompería la **segregación de funciones** y la independencia de la evidencia. Si quien opera o modifica `clientes` (o tiene control sobre esos cambios) también controla la tabla donde se registra lo ocurrido, podría intentar ocultar, alterar o justificar modificaciones indebidas sin supervisión independiente. Separar la propiedad/control de la auditoría garantiza que la pista de cambios sea confiable para revisión y control.

### 2. ¿Quién debería poder leer las bitácoras y la vista `dba.bitacora`? Relaciónalo con lo observado en el LAB-01 (secretos en la bitácora).

La lectura de las bitácoras y de `dba.bitacora` debe limitarse a perfiles autorizados (como **Auditoría Interna**, **Seguridad de la Información** y el **DBA** en funciones de administración y diagnóstico), evitando el acceso general o sin control. Como se observó en el LAB-01, las bitácoras pueden llegar a contener **secretos o datos sensibles** (como credenciales o contraseñas en sentencias registradas en texto plano, o información de clientes). Si usuarios amplios o no autorizados leen esos registros, se exponen credenciales y datos confidenciales.

### 3. ¿Qué ocurre con la responsabilidad sobre un dato cuando nadie la tiene asignada? Relaciónalo con el problema inicial de la unidad.

Cuando un dato no tiene un propietario o responsable claro, se produce un **vacío de decisión y rendición de cuentas**: nadie asume quién puede corregir un valor erróneo (como un RFC incorrecto), qué regla se incumplió ante un cambio indebido ni cuánto tiempo debe conservarse la evidencia. Eso conecta directamente con el problema inicial de la unidad: los mecanismos técnicos pueden mostrar *qué* ocurrió, pero sin responsables y reglas definidas no queda claro *quién* decide ni *quién* responde por el dato ni por su evidencia.
