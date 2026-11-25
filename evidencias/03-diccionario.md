
# Actividad 3 — Diccionario de datos operativo dentro del motor

## 1. Metadatos adicionales (fuera del motor)

| Columna           | Clasificación (Unidad II)            | Finalidad                                | Control aplicado                                         | ¿Cambios auditados?                                 | Regla de calidad                                               |
| :---------------- | :------------------------------------ | :--------------------------------------- | :------------------------------------------------------- | :--------------------------------------------------- | :------------------------------------------------------------- |
| `id`            | Identificador interno                 | Identificación unívoca del registro    | Clave primaria / secuencia (`SERIAL`)                  | Sí (`auditoria.cambios_clientes`)                 | No nulo y único                                               |
| `nombre`        | Dato personal (identificador directo) | Identificación del cliente              | Acceso restringido por rol                               | Sí (`auditoria.cambios_clientes`)                 | No nulo y con texto válido                                    |
| `correo`        | Dato personal                         | Notificaciones y contacto con el cliente | Restricción de acceso / validación operativa           | Sí (`auditoria.cambios_clientes`)                 | Formato válido de correo y sin duplicados (`lower(correo)`) |
| `telefono`      | Dato personal                         | Contacto y seguimiento comercial         | Acceso restringido por rol                               | Sí (`auditoria.cambios_clientes`)                 | Formato telefónico válido cuando aplique                     |
| `rfc`           | Dato personal (identificador fiscal)  | Facturación                             | Vista enmascarada para soporte (U2) / control de cambios | Sí, LAB-01 Parte B (`auditoria.cambios_clientes`) | Formato válido de RFC y no nulo cuando aplique                |
| `edad`          | Dato personal                         | Analítica agregada                      | Acceso restringido por rol                               | Sí (`auditoria.cambios_clientes`)                 | Rango entre `18` y `120` años cuando no sea nulo          |
| `ciudad`        | Dato de ubicación                    | Segmentación geográfica y operación   | Acceso restringido por rol                               | Sí (`auditoria.cambios_clientes`)                 | Nombre de ciudad válido                                       |
| `codigo_postal` | Dato de ubicación                    | Cobertura y logística                   | Acceso restringido por rol                               | Sí (`auditoria.cambios_clientes`)                 | Código postal válido (p. ej. 5 dígitos)                     |
| `fecha_alta`    | Metadato operativo                    | Trazabilidad del alta del registro       | Valor por defecto del sistema (`now()`)                | Sí (`auditoria.cambios_clientes`)                 | Marca de tiempo válida                                        |
| `segmento`      | Dato de negocio                       | Clasificación comercial                 | Control de actualización autorizado                     | Sí (`auditoria.cambios_clientes`)                 | Categoría o segmento válido                                  |

---

## 2. Preguntas de análisis

### 1. ¿Qué ventaja tiene guardar el significado en `COMMENT` en lugar de solo en un documento aparte?

Porque el significado queda **versionado y pegado al esquema dentro del propio SGBD**, accesible directamente desde consultas SQL (`col_description`, `pg_description` o herramientas de administración como pgAdmin). Esto reduce el riesgo de que la documentación quede desactualizada o separada de la estructura real de la tabla cuando cambian columnas o se inspecciona la base directamente.

### 2. ¿Quedaron registrados los `COMMENT` en la bitácora? ¿Con qué nivel de `log_statement`?

Sí quedan registrados en la bitácora de PostgreSQL cuando `log_statement` está configurado en `'mod'` (o `'ddl'` / `'all'`), ya que `COMMENT ON` es una sentencia de definición/metadatos que modifica objetos de la base de datos. Con `log_statement = 'none'` no se registrarían sentencias.

### 3. ¿Qué información del diccionario debe mantenerse fuera del motor? ¿Por qué?

La información de **gobierno, contexto organizacional y criterios de negocio/normativos**—como el propietario del dato (área de negocio), la clasificación de privacidad/sensibilidad, los acuerdos de nivel o reglas de escalamiento y las justificaciones de uso—conviene mantenerla o consolidarla en documentación de gobierno y catálogos externos, porque el motor solo guarda metadatos técnicos y descripciones breves, no la autoridad organizacional ni el contexto de cumplimiento o auditoría más amplio.
