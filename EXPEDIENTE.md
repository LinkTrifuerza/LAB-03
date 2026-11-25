
# Expediente de auditoría interna

Base comercial

## 1. Alcance

* **Sistemas:** Base **comercial** en **PostgreSQL 16** (esquemas `public`, `auditoria`, `dba` y `gobierno`) y **MongoDB 7** como referencia del LAB-02, en el entorno de trabajo de la práctica[cite: 1, 5, 19].
* **Periodo revisado:** Actividades y registros generados entre LAB-01, LAB-02 y LAB-03 (incluyendo las revisiones y conciliación del 25 de noviembre de 2026)[cite: 6, 24, 25].
* **Responsable:** DBA / custodio técnico de la base `comercial` para la revisión de auditoría interna de Comercializadora Norte S.A. de C.V.[cite: 3, 5, 25]

## 2. Configuración de auditoría (A3)

* **PostgreSQL 16:** El estado del registro y los disparadores se respalda en `evidencias/E1-configuracion-registro.txt`, `evidencias/E3-disparadores.txt` y `evidencias/E5-bitacora-postgresql.log` (con su hash en `evidencias/E5-bitacora-postgresql.sha256`), además de los registros del LAB-01 (`LAB-01/evidencias/bitacora-lab01.log`)[cite: 6, 22, 23, 24, 25].
* **MongoDB 7:** Se incluyen en `evidencias/E6-mongodb/` los archivos del LAB-02 (`system-profile.json`, `bitacora-mongod.log`, sus hashes y `09-comparacion.md`)[cite: 24, 25].

## 3. Bitácora interpretada (A3, A5)

1. **Incidente del RFC de María García (LAB-01):**

   * **Quién:** Usuario, sesión y aplicación registrados en `auditoria.cambios_clientes` o en `LAB-01/evidencias/bitacora-lab01.log` *(anota aquí los datos exactos de tu registro)*[cite: 6, 25].
   * **Cuándo:** Fecha y hora del evento según tu bitácora o tabla de auditoría del LAB-01[cite: 6, 25].
   * **Desde dónde:** Origen o sesión registrada en el evento[cite: 25].
   * **Qué ocurrió:** Hubo un cambio sobre el RFC de María García[cite: 25].
   * **Tipo de evento:** Cambio/auditoría de un dato sensible (`UPDATE` sobre `rfc`)[cite: 10, 25].
   * **Qué prueba:** Muestra que la auditoría de datos y/o bitácora del LAB-01 sí deja rastro cuando se modifica un dato sensible como el RFC[cite: 1, 10, 25].
2. **Cambio autorizado `CAM-2026-014` (`ALTER TABLE clientes ADD COLUMN canal_alta VARCHAR(20);`):**

   * **Quién:** Sesión en PostgreSQL (en el log activo el campo `usuario` sale vacío por el formato del prefijo), solicitado por `Gerencia Comercial` y aprobado por `Comité de cambios`[cite: 19, 20, 25].
   * **Cuándo:** `2026-11-25 10:20:21` (línea 94 del log actual), dentro de su ventana aprobada[cite: 19, 25].
   * **Desde dónde:** Conexión administrativa sobre `comercial` en `produccion`[cite: 6, 19, 25].
   * **Qué ocurrió:** Se agregó la columna `canal_alta` a `clientes` y se le puso comentario tras registrar la solicitud `CAM-2026-014`[cite: 19, 25].
   * **Tipo de evento:** Cambio de estructura (`ALTER TABLE`) autorizado y dentro de ventana[cite: 19, 20, 25].
   * **Qué prueba:** Queda respaldado en `sql/04-registro-cambios.sql`, `evidencias/06-conciliacion.md` y `evidencias/E5-bitacora-postgresql.log` (línea 94)[cite: 22, 24, 25].
3. **Ajustes de configuración y vista `dba.bitacora` sin folio previo:**

   * **Quién:** Sesión administrativa en PostgreSQL (`usuario` sin capturar en el extracto actual)[cite: 20, 25].
   * **Cuándo:** `2026-11-25 09:34:51` (línea 20), `2026-11-25 09:35:00` (línea 33) y `2026-11-25 09:35:41` (línea 35) en el log actual[cite: 25].
   * **Desde dónde:** Sesión administrativa en el servidor PostgreSQL 16[cite: 6, 25].
   * **Qué ocurrió:** Se ajustó `log_statement = 'mod'` y se creó/actualizó `dba.bitacora` antes de tener folio en `gobierno.solicitudes_cambio`[cite: 19, 21, 25].
   * **Tipo de evento:** Cambio de configuración de bitácora y creación de vista durante la preparación del laboratorio[cite: 21, 25].
   * **Qué prueba:** Se observa en `evidencias/06-conciliacion.md` y en las líneas 20, 33 y 35 de `evidencias/E5-bitacora-postgresql.log`[cite: 22, 24, 25].

## 4. Elementos de gobierno (A1, A2)

* **Responsables:** En `evidencias/02-responsables.md` se detalla quién es propietario, quién custodia (el DBA, sin tomar decisiones de negocio por sí solo) y quiénes usan o aprueban accesos en cada conjunto[cite: 3, 9, 10, 25].
* **Diccionario de datos:** Los comentarios en la base están en `sql/02-comentarios-diccionario.sql`, el CSV en `evidencias/diccionario-clientes.csv` y el detalle completo con datos externos en `evidencias/03-diccionario.md`[cite: 10, 11, 12, 25].
* **Catálogo mínimo de datos:** En `evidencias/04-catalogo.md` y `sql/03-inventario-tablas.sql` se listan los conjuntos de PostgreSQL 16 y MongoDB 7 (`CAT-01` a `CAT-05`), diferenciando dueño del dato y dueño técnico[cite: 13, 14, 25].

## 5. Conservación (A3)

* En `evidencias/05-conservacion.md` se define qué se conserva, por cuánto tiempo (o pendiente de confirmar con el área jurídica si es tema legal), qué riesgos tiene cada registro, cómo cuidar los hashes y cómo manejar el choque entre borrar datos personales y conservar auditoría[cite: 15, 16, 25].

## 6. Políticas (A4)

* **Acceso (`politicas/POL-01-acceso.md`):** Pide aprobación previa, mínimo privilegio y revisión mensual de permisos; se apoya en `evidencias/E2-privilegios.txt`, `evidencias/E5-bitacora-postgresql.log` y `evidencias/E6-mongodb/`[cite: 17, 18, 23, 24, 25].
* **Respaldo (`politicas/POL-02-respaldo.md`):** Define respaldar según importancia y conservación, verificando que sirvan, y deja el procedimiento técnico para la Unidad V; se apoya en `evidencias/04-catalogo.md`, `evidencias/05-conservacion.md` y `E1`–`E6`[cite: 18, 23, 24, 25, 30].
* **Retención (`politicas/POL-03-retencion.md`):** Marca plazos según `evidencias/05-conservacion.md` (con apoyo jurídico cuando aplique) sin borrar auditoría a la ligera; se apoya en `evidencias/05-conservacion.md`, `E4` y `E5` (con su `.sha256`)[cite: 15, 18, 23, 24, 25].
* **Restauración (`politicas/POL-04-restauracion.md`):** Exige autorización antes de restaurar en `produccion` o datos sensibles y revisar después, dejando el procedimiento técnico para la Unidad V; se relaciona con `sql/04-registro-cambios.sql` y `evidencias/06-conciliacion.md`[cite: 18, 19, 22, 25].
* **Datos sensibles (`politicas/POL-05-datos-sensibles.md`):** Limita el acceso a motivos justificados, mantiene la auditoría de cambios en `clientes` y pide limpiar secretos al exportar; se apoya en `02-responsables.md`, `03-diccionario.md`, `diccionario-clientes.csv`, `E3`, `E4` y `E5`[cite: 18, 23, 24, 25, 27].
* **Cambios en producción (`politicas/POL-06-cambios-produccion.md`):** Exige solicitud aprobada y ventana en `produccion` (a diferencia de `desarrollo` y `pruebas`) y conciliación posterior; se apoya en `sql/04-registro-cambios.sql`, `evidencias/06-conciliacion.md`, `E3` y `E5`[cite: 18, 19, 22, 23, 24, 25].

## 7. Conciliación de cambios (A5)

* Está en `evidencias/06-conciliacion.md` junto con `sql/04-registro-cambios.sql` y `evidencias/E5-bitacora-postgresql.log`[cite: 22, 24, 25]. Ahí se revisa que `CAM-2026-014` estuvo dentro de su ventana y se señalan los cambios previos sin folio (preparación del laboratorio), la falta de usuario en el extracto por el formato del log y los huecos cuando no se registran sentencias[cite: 19, 21, 22, 25].

## 8. Hallazgos

*(Se dejan tal como se encontraron, sin "arreglarlos" por debajo del agua en el expediente[cite: 25, 26, 30]).*

| ID     | Hallazgo                                                                                                                                                                                                                                      | Fuente                                                                                                                                         | Severidad | Impacto                                                                                             | Recomendación                                                                                                                 | Decisión requerida de                                                                  |
| :----- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- | :-------: | :-------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------- |
| HAL-01 | Datos con errores en `public.clientes` (posible correo repetido, RFC incompleto/nulo, correo inválido, edad fuera de rango y campos faltantes)                                                                                             | `evidencias/01-calidad.md`, `sql/01-verificaciones-calidad.sql`                                                                            |   Media   | Puede provocar fallos en ventas, cobros o avisos si se usan registros mal capturados                | Escalar la corrección al dueño del dato y revisar si conviene poner `CHECK` o `UNIQUE`                                   | Propietario del dato (`Gerencia Comercial`) y DBA[cite: 3, 8, 9, 25]                  |
| HAL-02 | Cambios de configuración, vista y tabla de cambios sin folio previo (y riesgo de apagar disparadores o hacer cambios sin folio en producción)                                                                                               | `evidencias/06-conciliacion.md`, `sql/04-registro-cambios.sql`, `evidencias/E5-bitacora-postgresql.log`                                  |   Alta   | Deja huecos en la trazabilidad o permite cambios productivos sin autorización formal               | Pedir folio aprobado y ventana antes de tocar producción, cuidar los disparadores y separar quién ejecuta de quién concilia | Comité de cambios, propietario del dato y auditoría interna[cite: 19, 22, 25]         |
| HAL-03 | Límites técnicos de registro: `usuario` vacío en la extracción por el formato del log, problemas con `0xfa`/codificación en Windows, `system.profile` de 1 MB que se sobrescribe y falta de auditoría nativa en MongoDB Community | `evidencias/05-conservacion.md`, `evidencias/06-conciliacion.md`, `evidencias/E1-configuracion-registro.txt`, `evidencias/E6-mongodb/` |   Media   | Dificulta saber quién ejecutó ciertos comandos o perder registros si no se extraen/rotan a tiempo | Ajustar el formato del log y la codificación al leer, extraer a tiempo y documentar controles alternativos                    | Responsable de seguridad, arquitectura y propietario del dato[cite: 15, 21, 23, 24, 25] |

## 9. Limitaciones conocidas y riesgos aceptados (A6)

* **Lo que sí se probó o vio en los laboratorios:**
  * En MongoDB Community (LAB-02 / `evidencias/E6-mongodb/`) no hay auditoría nativa como en versiones comerciales; se trabajó con perfilado (`system.profile`), log de `mongod` e instantáneas de privilegios[cite: 1, 24, 25].
  * Las lecturas (`SELECT`) no quedan registradas en `log_statement = 'mod'`, y quien tiene privilegios administrativos podría cambiar ajustes o desactivar un disparador si no hay control y revisión[cite: 21, 25].
  * En Windows hubo detalle de codificación (`0xfa` en UTF-8) y el extractor de `user=` devolvió vacío por cómo está armado el prefijo del log[cite: 20, 21, 25].
  * `system.profile` tiene tamaño limitado (1 MB) y se sobrescribe solo[cite: 15, 25].
* **Lo que solo se leyó o quedó pendiente, sin probarse aquí:**
  * Funciones de MongoDB Enterprise (como auditoría nativa empresarial) o herramientas externas no probadas en la práctica, además de los procedimientos técnicos de respaldo/restauración (Unidad V) y plazos legales por confirmar con el área jurídica[cite: 15, 18, 25, 26, 30].
* **Quién acepta el riesgo:** Cada riesgo debe aceptarlo formalmente el **propietario del dato** junto con **seguridad de la información / auditoría interna** (y el **área jurídica** en plazos legales)[cite: 3, 15, 25].

## 10. Índice de evidencias

| Archivo / Ruta                                                          | Qué contiene                                                                                                                   | Integridad / Hash                                              |
| :---------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------- |
| `sql/01-verificaciones-calidad.sql` a `sql/04-registro-cambios.sql` | Scripts de calidad, diccionario, inventario de tablas y registro de cambios[cite: 8, 12, 14, 22, 27]                            | En repositorio                                                 |
| `politicas/POL-01-acceso.md` a `POL-06-cambios-produccion.md`       | Las seis políticas prácticas de `comercial`[cite: 16, 17, 18, 27]                                                           | En repositorio                                                 |
| `evidencias/01-calidad.md` a `evidencias/06-conciliacion.md`        | Análisis y respuestas de las actividades 1 a 5 y 7[cite: 8, 10, 12, 14, 16, 22, 27]                                            | En repositorio                                                 |
| `evidencias/diccionario-clientes.csv`                                 | Exportación CSV del diccionario de `clientes`[cite: 11, 27]                                                                  | En repositorio                                                 |
| `evidencias/E1-configuracion-registro.txt`                            | Ajustes vigentes de registro en PostgreSQL[cite: 23, 27]                                                                        | Archivo técnico E1[cite: 23, 27]                              |
| `evidencias/E2-privilegios.txt`                                       | Permisos vigentes por rol en tablas[cite: 23, 27]                                                                               | Archivo técnico E2[cite: 23, 27]                              |
| `evidencias/E3-disparadores.txt`                                      | Disparadores activos (no internos)[cite: 23, 27]                                                                                | Archivo técnico E3[cite: 23, 27]                              |
| `evidencias/E4-muestra-auditoria.txt`                                 | Muestra reciente de `auditoria.cambios_clientes` sin datos completos[cite: 23, 27]                                            | Archivo técnico E4[cite: 23, 27]                              |
| `evidencias/E5-bitacora-postgresql.log`                               | Bitácora exportada de PostgreSQL con secretos saneados[cite: 24, 27]                                                           | Ver `evidencias/E5-bitacora-postgresql.sha256`[cite: 24, 27] |
| `evidencias/E5-bitacora-postgresql.sha256`                            | Hash SHA-256 del archivo `E5-bitacora-postgresql.log`[cite: 24, 27]                                                           | Hash SHA-256[cite: 24, 27]                                     |
| `evidencias/E6-mongodb/`                                              | Archivos de MongoDB del LAB-02 (`system-profile.json`, `bitacora-mongod.log`, hashes y `09-comparacion.md`)[cite: 24, 27] | Hashes dentro de `E6-mongodb/`[cite: 24, 27]                 |

# Expediente de auditoría interna

Base comercial

## 1. Alcance

* **Sistemas:** Base **comercial** en **PostgreSQL 16** (esquemas `public`, `auditoria`, `dba` y `gobierno`) y **MongoDB 7** como referencia del LAB-02, en el entorno de trabajo de la práctica[cite: 1, 5, 19].
* **Periodo revisado:** Actividades y registros generados entre LAB-01, LAB-02 y LAB-03 (incluyendo las revisiones y conciliación del 25 de noviembre de 2026)[cite: 6, 24, 25].
* **Responsable:** DBA / custodio técnico de la base `comercial` para la revisión de auditoría interna de Comercializadora Norte S.A. de C.V.[cite: 3, 5, 25]

## 2. Configuración de auditoría (A3)

* **PostgreSQL 16:** El estado del registro y los disparadores se respalda en `evidencias/E1-configuracion-registro.txt`, `evidencias/E3-disparadores.txt` y `evidencias/E5-bitacora-postgresql.log` (con su hash en `evidencias/E5-bitacora-postgresql.sha256`), además de los registros del LAB-01 (`LAB-01/evidencias/bitacora-lab01.log`)[cite: 6, 22, 23, 24, 25].
* **MongoDB 7:** Se incluyen en `evidencias/E6-mongodb/` los archivos del LAB-02 (`system-profile.json`, `bitacora-mongod.log`, sus hashes y `09-comparacion.md`)[cite: 24, 25].

## 3. Bitácora interpretada (A3, A5)

1. **Incidente del RFC de María García (LAB-01):**

   * **Quién:** Usuario, sesión y aplicación registrados en `auditoria.cambios_clientes` o en `LAB-01/evidencias/bitacora-lab01.log` *(anota aquí los datos exactos de tu registro)*[cite: 6, 25].
   * **Cuándo:** Fecha y hora del evento según tu bitácora o tabla de auditoría del LAB-01[cite: 6, 25].
   * **Desde dónde:** Origen o sesión registrada en el evento[cite: 25].
   * **Qué ocurrió:** Hubo un cambio sobre el RFC de María García[cite: 25].
   * **Tipo de evento:** Cambio/auditoría de un dato sensible (`UPDATE` sobre `rfc`)[cite: 10, 25].
   * **Qué prueba:** Muestra que la auditoría de datos y/o bitácora del LAB-01 sí deja rastro cuando se modifica un dato sensible como el RFC[cite: 1, 10, 25].
2. **Cambio autorizado `CAM-2026-014` (`ALTER TABLE clientes ADD COLUMN canal_alta VARCHAR(20);`):**

   * **Quién:** Sesión en PostgreSQL (en el log activo el campo `usuario` sale vacío por el formato del prefijo), solicitado por `Gerencia Comercial` y aprobado por `Comité de cambios`[cite: 19, 20, 25].
   * **Cuándo:** `2026-11-25 10:20:21` (línea 94 del log actual), dentro de su ventana aprobada[cite: 19, 25].
   * **Desde dónde:** Conexión administrativa sobre `comercial` en `produccion`[cite: 6, 19, 25].
   * **Qué ocurrió:** Se agregó la columna `canal_alta` a `clientes` y se le puso comentario tras registrar la solicitud `CAM-2026-014`[cite: 19, 25].
   * **Tipo de evento:** Cambio de estructura (`ALTER TABLE`) autorizado y dentro de ventana[cite: 19, 20, 25].
   * **Qué prueba:** Queda respaldado en `sql/04-registro-cambios.sql`, `evidencias/06-conciliacion.md` y `evidencias/E5-bitacora-postgresql.log` (línea 94)[cite: 22, 24, 25].
3. **Ajustes de configuración y vista `dba.bitacora` sin folio previo:**

   * **Quién:** Sesión administrativa en PostgreSQL (`usuario` sin capturar en el extracto actual)[cite: 20, 25].
   * **Cuándo:** `2026-11-25 09:34:51` (línea 20), `2026-11-25 09:35:00` (línea 33) y `2026-11-25 09:35:41` (línea 35) en el log actual[cite: 25].
   * **Desde dónde:** Sesión administrativa en el servidor PostgreSQL 16[cite: 6, 25].
   * **Qué ocurrió:** Se ajustó `log_statement = 'mod'` y se creó/actualizó `dba.bitacora` antes de tener folio en `gobierno.solicitudes_cambio`[cite: 19, 21, 25].
   * **Tipo de evento:** Cambio de configuración de bitácora y creación de vista durante la preparación del laboratorio[cite: 21, 25].
   * **Qué prueba:** Se observa en `evidencias/06-conciliacion.md` y en las líneas 20, 33 y 35 de `evidencias/E5-bitacora-postgresql.log`[cite: 22, 24, 25].

## 4. Elementos de gobierno (A1, A2)

* **Responsables:** En `evidencias/02-responsables.md` se detalla quién es propietario, quién custodia (el DBA, sin tomar decisiones de negocio por sí solo) y quiénes usan o aprueban accesos en cada conjunto[cite: 3, 9, 10, 25].
* **Diccionario de datos:** Los comentarios en la base están en `sql/02-comentarios-diccionario.sql`, el CSV en `evidencias/diccionario-clientes.csv` y el detalle completo con datos externos en `evidencias/03-diccionario.md`[cite: 10, 11, 12, 25].
* **Catálogo mínimo de datos:** En `evidencias/04-catalogo.md` y `sql/03-inventario-tablas.sql` se listan los conjuntos de PostgreSQL 16 y MongoDB 7 (`CAT-01` a `CAT-05`), diferenciando dueño del dato y dueño técnico[cite: 13, 14, 25].

## 5. Conservación (A3)

* En `evidencias/05-conservacion.md` se define qué se conserva, por cuánto tiempo (o pendiente de confirmar con el área jurídica si es tema legal), qué riesgos tiene cada registro, cómo cuidar los hashes y cómo manejar el choque entre borrar datos personales y conservar auditoría[cite: 15, 16, 25].

## 6. Políticas (A4)

* **Acceso (`politicas/POL-01-acceso.md`):** Pide aprobación previa, mínimo privilegio y revisión mensual de permisos; se apoya en `evidencias/E2-privilegios.txt`, `evidencias/E5-bitacora-postgresql.log` y `evidencias/E6-mongodb/`[cite: 17, 18, 23, 24, 25].
* **Respaldo (`politicas/POL-02-respaldo.md`):** Define respaldar según importancia y conservación, verificando que sirvan, y deja el procedimiento técnico para la Unidad V; se apoya en `evidencias/04-catalogo.md`, `evidencias/05-conservacion.md` y `E1`–`E6`[cite: 18, 23, 24, 25, 30].
* **Retención (`politicas/POL-03-retencion.md`):** Marca plazos según `evidencias/05-conservacion.md` (con apoyo jurídico cuando aplique) sin borrar auditoría a la ligera; se apoya en `evidencias/05-conservacion.md`, `E4` y `E5` (con su `.sha256`)[cite: 15, 18, 23, 24, 25].
* **Restauración (`politicas/POL-04-restauracion.md`):** Exige autorización antes de restaurar en `produccion` o datos sensibles y revisar después, dejando el procedimiento técnico para la Unidad V; se relaciona con `sql/04-registro-cambios.sql` y `evidencias/06-conciliacion.md`[cite: 18, 19, 22, 25].
* **Datos sensibles (`politicas/POL-05-datos-sensibles.md`):** Limita el acceso a motivos justificados, mantiene la auditoría de cambios en `clientes` y pide limpiar secretos al exportar; se apoya en `02-responsables.md`, `03-diccionario.md`, `diccionario-clientes.csv`, `E3`, `E4` y `E5`[cite: 18, 23, 24, 25, 27].
* **Cambios en producción (`politicas/POL-06-cambios-produccion.md`):** Exige solicitud aprobada y ventana en `produccion` (a diferencia de `desarrollo` y `pruebas`) y conciliación posterior; se apoya en `sql/04-registro-cambios.sql`, `evidencias/06-conciliacion.md`, `E3` y `E5`[cite: 18, 19, 22, 23, 24, 25].

## 7. Conciliación de cambios (A5)

* Está en `evidencias/06-conciliacion.md` junto con `sql/04-registro-cambios.sql` y `evidencias/E5-bitacora-postgresql.log`[cite: 22, 24, 25]. Ahí se revisa que `CAM-2026-014` estuvo dentro de su ventana y se señalan los cambios previos sin folio (preparación del laboratorio), la falta de usuario en el extracto por el formato del log y los huecos cuando no se registran sentencias[cite: 19, 21, 22, 25].

## 8. Hallazgos

*(Se dejan tal como se encontraron, sin "arreglarlos" por debajo del agua en el expediente[cite: 25, 26, 30]).*

| ID     | Hallazgo                                                                                                                                                                                                                                      | Fuente                                                                                                                                         | Severidad | Impacto                                                                                             | Recomendación                                                                                                                 | Decisión requerida de                                                                  |
| :----- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- | :-------: | :-------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------- |
| HAL-01 | Datos con errores en `public.clientes` (posible correo repetido, RFC incompleto/nulo, correo inválido, edad fuera de rango y campos faltantes)                                                                                             | `evidencias/01-calidad.md`, `sql/01-verificaciones-calidad.sql`                                                                            |   Media   | Puede provocar fallos en ventas, cobros o avisos si se usan registros mal capturados                | Escalar la corrección al dueño del dato y revisar si conviene poner `CHECK` o `UNIQUE`                                   | Propietario del dato (`Gerencia Comercial`) y DBA[cite: 3, 8, 9, 25]                  |
| HAL-02 | Cambios de configuración, vista y tabla de cambios sin folio previo (y riesgo de apagar disparadores o hacer cambios sin folio en producción)                                                                                               | `evidencias/06-conciliacion.md`, `sql/04-registro-cambios.sql`, `evidencias/E5-bitacora-postgresql.log`                                  |   Alta   | Deja huecos en la trazabilidad o permite cambios productivos sin autorización formal               | Pedir folio aprobado y ventana antes de tocar producción, cuidar los disparadores y separar quién ejecuta de quién concilia | Comité de cambios, propietario del dato y auditoría interna[cite: 19, 22, 25]         |
| HAL-03 | Límites técnicos de registro: `usuario` vacío en la extracción por el formato del log, problemas con `0xfa`/codificación en Windows, `system.profile` de 1 MB que se sobrescribe y falta de auditoría nativa en MongoDB Community | `evidencias/05-conservacion.md`, `evidencias/06-conciliacion.md`, `evidencias/E1-configuracion-registro.txt`, `evidencias/E6-mongodb/` |   Media   | Dificulta saber quién ejecutó ciertos comandos o perder registros si no se extraen/rotan a tiempo | Ajustar el formato del log y la codificación al leer, extraer a tiempo y documentar controles alternativos                    | Responsable de seguridad, arquitectura y propietario del dato[cite: 15, 21, 23, 24, 25] |

## 9. Limitaciones conocidas y riesgos aceptados (A6)

* **Lo que sí se probó o vio en los laboratorios:**
  * En MongoDB Community (LAB-02 / `evidencias/E6-mongodb/`) no hay auditoría nativa como en versiones comerciales; se trabajó con perfilado (`system.profile`), log de `mongod` e instantáneas de privilegios[cite: 1, 24, 25].
  * Las lecturas (`SELECT`) no quedan registradas en `log_statement = 'mod'`, y quien tiene privilegios administrativos podría cambiar ajustes o desactivar un disparador si no hay control y revisión[cite: 21, 25].
  * En Windows hubo detalle de codificación (`0xfa` en UTF-8) y el extractor de `user=` devolvió vacío por cómo está armado el prefijo del log[cite: 20, 21, 25].
  * `system.profile` tiene tamaño limitado (1 MB) y se sobrescribe solo[cite: 15, 25].
* **Lo que solo se leyó o quedó pendiente, sin probarse aquí:**
  * Funciones de MongoDB Enterprise (como auditoría nativa empresarial) o herramientas externas no probadas en la práctica, además de los procedimientos técnicos de respaldo/restauración (Unidad V) y plazos legales por confirmar con el área jurídica[cite: 15, 18, 25, 26, 30].
* **Quién acepta el riesgo:** Cada riesgo debe aceptarlo formalmente el **propietario del dato** junto con **seguridad de la información / auditoría interna** (y el **área jurídica** en plazos legales)[cite: 3, 15, 25].

## 10. Índice de evidencias

| Archivo / Ruta                                                          | Qué contiene                                                                                                                   | Integridad / Hash                                              |
| :---------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------- |
| `sql/01-verificaciones-calidad.sql` a `sql/04-registro-cambios.sql` | Scripts de calidad, diccionario, inventario de tablas y registro de cambios[cite: 8, 12, 14, 22, 27]                            | En repositorio                                                 |
| `politicas/POL-01-acceso.md` a `POL-06-cambios-produccion.md`       | Las seis políticas prácticas de `comercial`[cite: 16, 17, 18, 27]                                                           | En repositorio                                                 |
| `evidencias/01-calidad.md` a `evidencias/06-conciliacion.md`        | Análisis y respuestas de las actividades 1 a 5 y 7[cite: 8, 10, 12, 14, 16, 22, 27]                                            | En repositorio                                                 |
| `evidencias/diccionario-clientes.csv`                                 | Exportación CSV del diccionario de `clientes`[cite: 11, 27]                                                                  | En repositorio                                                 |
| `evidencias/E1-configuracion-registro.txt`                            | Ajustes vigentes de registro en PostgreSQL[cite: 23, 27]                                                                        | Archivo técnico E1[cite: 23, 27]                              |
| `evidencias/E2-privilegios.txt`                                       | Permisos vigentes por rol en tablas[cite: 23, 27]                                                                               | Archivo técnico E2[cite: 23, 27]                              |
| `evidencias/E3-disparadores.txt`                                      | Disparadores activos (no internos)[cite: 23, 27]                                                                                | Archivo técnico E3[cite: 23, 27]                              |
| `evidencias/E4-muestra-auditoria.txt`                                 | Muestra reciente de `auditoria.cambios_clientes` sin datos completos[cite: 23, 27]                                            | Archivo técnico E4[cite: 23, 27]                              |
| `evidencias/E5-bitacora-postgresql.log`                               | Bitácora exportada de PostgreSQL con secretos saneados[cite: 24, 27]                                                           | Ver `evidencias/E5-bitacora-postgresql.sha256`[cite: 24, 27] |
| `evidencias/E5-bitacora-postgresql.sha256`                            | Hash SHA-256 del archivo `E5-bitacora-postgresql.log`[cite: 24, 27]                                                           | Hash SHA-256[cite: 24, 27]                                     |
| `evidencias/E6-mongodb/`                                              | Archivos de MongoDB del LAB-02 (`system-profile.json`, `bitacora-mongod.log`, hashes y `09-comparacion.md`)[cite: 24, 27] | Hashes dentro de `E6-mongodb/`[cite: 24, 27]                 |
