
# POL-01 Política de acceso

* **Objetivo:** Reducir el riesgo de accesos no autorizados, escalamiento indebido de privilegios o exposición de datos operativos y sensibles en la base `comercial` y sus entornos asociados.
* **Alcance:** Aplica a cuentas, roles y usuarios que soliciten o utilicen acceso sobre `comercial` (PostgreSQL 16 y MongoDB 7 de referencia) en los ambientes `desarrollo`, `pruebas` y `produccion`.
* **Regla:** Todo acceso a datos o privilegios sobre objetos de `comercial` requiere solicitud formal aprobada por el propietario del dato correspondiente y ejecución acotada al principio de mínimo privilegio por el custodio (DBA). El DBA revisa mensualmente los eventos de `GRANT`, `REVOKE` y cambios de rol o privilegios para conciliarlos con las solicitudes aprobadas.
* **Responsable:** Decide/aprueba el propietario del dato (y seguridad/administración cuando aplique); ejecuta el DBA como custodio.
* **Excepciones:** Atenciones urgentes ante incidentes operativos o de seguridad pueden habilitarse temporalmente con autorización del responsable de seguridad/operación y registro posterior obligatorio en la bitácora y solicitud de regularización.
* **Evidencia:** Líneas `statement: GRANT` / `statement: REVOKE` y cambios de rol en la bitácora de PostgreSQL (`LAB-01`, Actividad 6 / `evidencias/E5-bitacora-postgresql.log`), instantáneas de privilegios de MongoDB (`LAB-02` / `evidencias/E6-mongodb/`) y registros de privilegios vigentes (`evidencias/E2-privilegios.txt`). El acta formal de conciliación mensual aún no existe en el expediente actual y debe generarse en revisiones periódicas.
* **Revisión:** Se revisa cada 6 meses o ante cambios relevantes en roles, esquemas o políticas de seguridad.
