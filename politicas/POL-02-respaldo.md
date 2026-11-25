
# POL-02 Política de respaldo

* **Objetivo:** Reducir el riesgo de pérdida definitiva de información operativa, de auditoría o de configuración ante fallos de hardware, errores humanos o incidentes en los sistemas de datos.
* **Alcance:** Aplica a los conjuntos de datos, esquemas, colecciones y registros relevantes de `comercial` en PostgreSQL 16 y MongoDB 7 across los ambientes `desarrollo`, `pruebas` y `produccion`, con énfasis en `produccion`.
* **Regla:** Se respaldan periódicamente los datos operativos de `comercial`, la estructura/metadatos y los registros de auditoría conforme a la criticidad definida en el catálogo (`evidencias/04-catalogo.md`) y los criterios de `evidencias/05-conservacion.md`. El DBA verifica periódicamente la integridad y disponibilidad de los respaldos generados y reporta cualquier fallo de respaldo. El procedimiento técnico se definirá en la Unidad V.
* **Responsable:** Decide los requisitos y prioridades el propietario del dato junto con el DBA y seguridad; ejecuta y verifica el DBA como custodio.
* **Excepciones:** Conjuntos temporales o de perfilado volátil como `system.profile` pueden excluirse de respaldos regulares siempre que no contengan hallazgos requeridos para investigación; cualquier exclusión en `produccion` requiere aprobación del propietario y del DBA.
* **Evidencia:** Catálogo mínimo de datos (`evidencias/04-catalogo.md`), criterios de conservación (`evidencias/05-conservacion.md`) y salidas de verificación de entorno/objetos (`evidencias/E1-configuracion-registro.txt` a `evidencias/E6-mongodb/`). Hoy no existe un registro formal de pruebas periódicas de restauración de respaldos ni bitácora automatizada de trabajos de respaldo (se incorporarán en la Unidad V).
* **Revisión:** Se revisa cada 6 meses y al incorporarse los mecanismos técnicos de la Unidad V.
