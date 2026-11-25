
# POL-04 Política de restauración

* **Objetivo:** Reducir el riesgo de restauraciones no autorizadas, sobrescritura accidental de datos vigentes o recuperación inconsistente tras un incidente o falla.
* **Alcance:** Aplica a las restauraciones de bases de datos, tablas, colecciones o respaldos de `comercial` en PostgreSQL 16 y MongoDB 7 en los ambientes `desarrollo`, `pruebas` y `produccion`.
* **Regla:** Toda restauración en `produccion` (o sobre datos sensibles/auditados) requiere autorización explícita del propietario del dato y del responsable de operación/seguridad, indicando motivo, punto de recuperación y alcance. El DBA ejecuta la restauración y verifica la coherencia de los datos y controles posteriores antes de devolver el servicio a operación. El procedimiento técnico se definirá en la Unidad V.
* **Responsable:** Autoriza el propietario del dato (y seguridad/liderazgo operativo en `produccion`); ejecuta y valida el DBA.
* **Excepciones:** En pruebas controladas en `desarrollo` o `pruebas` sobre datos no sensibles, el DBA puede restaurar entornos de trabajo bajo su custodia; en emergencias críticas de `produccion`, la autorización puede registrarse de forma expedida con validación posterior formal.
* **Evidencia:** Registro de solicitudes de cambio (`sql/04-registro-cambios.sql` / tabla `gobierno.solicitudes_cambio`), conciliación en `evidencias/06-conciliacion.md` y evidencias técnicas (`evidencias/E1-configuracion-registro.txt` a `evidencias/E6-mongodb/`). Hoy no existe un historial de eventos de restauración ejecutados en la base `comercial` (se complementará en la Unidad V).
* **Revisión:** Se revisa cada 6 meses o tras cualquier incidente de recuperación o actualización de procedimientos en la Unidad V.
