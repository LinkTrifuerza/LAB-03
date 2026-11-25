
# Actividad 1 — Calidad de datos desde la operación

## 1. Resultados de verificación en `public.clientes`

* **Posibles duplicados por correo:** `0` filas encontradas (al existir únicamente dos registros iniciales en la tabla, no se presentaron correos repetidos).
* **RFC con formato no válido o ausente:** `2` filas (`id = 1` con RFC incompleto `LOAA850315AB` y `id = 2` con RFC `NULL`).
* **Correo con formato no válido:** `2` filas (`id = 1` con `'ANA. LOPEZ@example.test'` y `id = 2` con `'sin-correo'`).
* **Edad fuera de rango:** `1` fila (`id = 2` con edad `-3`).
* **Completitud general:** `total = 2`, `sin_telefono = 0` (aunque `id = 2` tiene valor incompleto `'667'`) y `sin_rfc = 1`.

---

## 2. Registro de hallazgos de calidad

| #            | Hallazgo                                                                   |           Registros (`id`)           | Regla de calidad                               | ¿Quién decide la corrección?                 | Acción propuesta                                                                              |
| :----------- | :------------------------------------------------------------------------- | :------------------------------------: | :--------------------------------------------- | :---------------------------------------------- | :--------------------------------------------------------------------------------------------- |
| **C1** | RFC incompleto (`LOAA850315AB`, 12 caracteres)                           |           `1` (Ana Lopez)           | Formato de RFC                                 | Gerencia Comercial (propietario del dato)       | Solicitar el RFC completo y validado al cliente o área comercial antes de emitir facturación |
| **C2** | RFC ausente (`NULL`)                                                     |          `2` (Jorge Díaz)          | Completitud y formato de RFC                   | Gerencia Comercial (propietario del dato)       | Confirmar con el área comercial si el cliente requiere facturación y recabar su RFC          |
| **C3** | Correo con formato inválido (`ANA. LOPEZ@example.test`, `sin-correo`) | `1` (Ana Lopez), `2` (Jorge Díaz) | Formato de correo electrónico                 | Gerencia Comercial / Área operativa de captura | Validar los correos reales de contacto con los clientes y corregir la captura                  |
| **C4** | Edad fuera de rango (`-3`)                                               |          `2` (Jorge Díaz)          | Rango válido de edad (`18` a `120` años) | Gerencia Comercial (propietario del dato)       | Verificar la fecha de nacimiento o edad real del cliente en su expediente de alta y corregir   |

---

## 3. Preguntas de análisis

### 1. ¿Por qué el DBA no debe corregir por su cuenta el RFC del registro 7 (en esta instancia, registro `id = 1`)?

Porque el DBA es **custodio** del dato, no su **propietario**. El propietario del dato (Gerencia Comercial) es quien tiene la autoridad y el contexto de negocio para decidir qué valor es correcto y autorizar su corrección. Si el DBA modifica un RFC por iniciativa propia sin respaldo documental del propietario, puede inventar un identificador fiscal erróneo, afectar la facturación y asumir una responsabilidad operativa que no le corresponde.

### 2. ¿Qué restricción (`CHECK`, `UNIQUE`) habría evitado cada hallazgo? ¿Quién debería aprobar agregarla?

* **Hallazgos C1 y C2 (RFC inválido o ausente):** Una restricción `CHECK (rfc ~ '^[A-ZÑ&]{3,4}[0-9]{6}[A-Z0-9]{3}$')` (junto con `NOT NULL` si el flujo exige RFC obligatorio) habría bloqueado el registro con formato incompleto o nulo.
* **Hallazgo C3 (correo inválido):** Una restricción `CHECK (correo ~ '^[^@\s]+@[^@\s]+\.[^@\s]+$')` habría rechazado cadenas con espacios o sin dominio, y una restricción `UNIQUE` sobre `lower(correo)` evitaría duplicados.
* **Hallazgo C4 (edad fuera de rango):** Una restricción `CHECK (edad IS NULL OR (edad BETWEEN 18 AND 120))` habría impedido insertar edades negativas o irreales.
* **¿Quién aprueba agregarla?** El **propietario del dato** (Gerencia Comercial) junto con el responsable funcional de la aplicación, coordinándose con el DBA, ya que introducir restricciones en una base en operación puede rechazar capturas si los sistemas que escriben en ella aún no validan los datos.

### 3. Si el DBA corrige un dato, ¿qué mecanismo del LAB-01 (Parte B) dejará evidencia del cambio?

El disparador de auditoría (`trg_auditoria_clientes`) y la tabla `auditoria.cambios_clientes`, los cuales registran automáticamente cada operación `UPDATE` sobre `public.clientes` guardando el momento exacto, el usuario de sesión, el identificador del cliente afectado y el estado completo del registro antes (`valores_antes`) y después (`valores_despues`) del cambio, complementado por el registro de sentencias de modificación en la bitácora de PostgreSQL (`log_statement = mod`).
