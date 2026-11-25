
# POL-06 Política de cambios en producción

* **Objetivo:** Reducir el riesgo de cambios no autorizados, desactivación indebida de controles de auditoría o interrupciones en el ambiente de `produccion` de la base `comercial`.
* **Alcance:** Aplica a cambios de estructura (`DDL`), privilegios (`GRANT`/`REVOKE`), disparadores (`TRIGGER`), configuraciones y objetos en el ambiente de `produccion` de `comercial`, distinguiéndolo de los ambientes `desarrollo` y `pruebas` definidos en la Unidad I.
* **Regla:** Antes de aplicar un cambio en `produccion`, debe existir una solicitud registrada y aprobada con ventana de ejecución definida; no deben realizarse cambios de estructura o privilegios en `produccion` sin autorización previa ni desactivar disparadores de auditoría sin aprobación formal. Durante la ventana aprobada el DBA ejecuta el cambio con registro en bitácora, y después se concilia la bitácora contra el registro de cambios autorizado para detectar discrepancias.
* **Responsable:** Solicita el área requirente/propietario; aprueba el comité o responsable de cambios autorizado; ejecuta y concilia el DBA (idealmente con revisión independiente de auditoría/seguridad).
* **Excepciones:** Ante una falla crítica en `produccion` que comprometa la disponibilidad o integridad, puede ejecutarse una intervención de emergencia con aprobación del responsable designado, debiendo registrarse formalmente y conciliarse en `gobierno.solicitudes_cambio` a la brevedad.
* **Evidencia:** Esquema y tabla `gobierno.solicitudes_cambio` (`sql/04-registro-cambios.sql`), conciliación de bitácora vs. solicitudes (`evidencias/06-conciliacion.md`), disparadores habilitados (`evidencias/E3-disparadores.txt`) y bitácora de PostgreSQL (`evidencias/E5-bitacora-postgresql.log`).
* **Revisión:** Se revisa cada 3 a 6 meses o tras cada conciliación con hallazgos o cambios no autorizados.
