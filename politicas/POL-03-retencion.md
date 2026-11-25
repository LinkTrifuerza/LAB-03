
# POL-03 Política de retención

* **Objetivo:** Reducir el riesgo de conservar datos personales más tiempo del necesario, perder trazabilidad de auditoría antes de tiempo o depurar información sin criterio ni respaldo documental.
* **Alcance:** Aplica a los conjuntos de datos y tipos de bitácora de `comercial` en PostgreSQL 16 (`public.clientes`, `auditoria.cambios_clientes`, bitácoras del colector) y MongoDB 7 (`clientes`, `system.profile`, bitácora de `mongod`, `auditoria.instantaneas_privilegios`).
* **Regla:** Cada conjunto de datos y bitácora se conserva durante el plazo definido en `evidencias/05-conservacion.md` (o bajo *"Plazo a confirmar con el área jurídica"* cuando dependa de obligación normativa), manteniendo la trazabilidad de auditoría al menos durante el periodo necesario para revisiones y soporte de `POL-05`. Al vencer el plazo, se aplica archivo protegido, anonimización o depuración controlada según corresponda, sin borrar registros de auditoría de forma arbitraria ni anticipada.
* **Responsable:** Decide/autoriza la disposición el propietario del dato (y el área jurídica cuando aplique plazo normativo); ejecuta el DBA bajo control documentado.
* **Excepciones:** Pueden suspenderse temporalmente las purgas o depuraciones cuando exista una investigación de seguridad, auditoría interna o requerimiento legal/administrativo en curso, con autorización formal de seguridad o auditoría interna.
* **Evidencia:** Matriz y análisis de conservación en `evidencias/05-conservacion.md`, bitácora exportada (`evidencias/E5-bitacora-postgresql.log`) con su hash (`evidencias/E5-bitacora-postgresql.sha256`), muestra de auditoría (`evidencias/E4-muestra-auditoria.txt`) y evidencias de MongoDB (`evidencias/E6-mongodb/`). Hoy no existe un registro histórico de ejecuciones de purga o archivo automático.
* **Revisión:** Se revisa anualmente o cuando se confirme un plazo normativo con el área jurídica.
