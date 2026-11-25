
# POL-05 Política de uso de datos sensibles

* **Objetivo:** Reducir el riesgo de filtración, consulta indebida, alteración no trazada o exposición innecesaria de datos personales y confidenciales de clientes (`nombre`, `correo`, `telefono`, `rfc`, `edad`, etc.).
* **Alcance:** Aplica a todas las columnas y registros clasificados como datos personales o confidenciales en `comercial` (`public.clientes`, `auditoria.cambios_clientes`, MongoDB `clientes`, `system.profile` y bitácoras que contengan datos o secretos).
* **Regla:** El acceso a datos sensibles se concede únicamente a usuarios o procesos con finalidad de negocio o revisión justificada y nunca mediante consultas masivas o extracciones sin propósito autorizado. Las modificaciones sobre `public.clientes` deben permanecer auditadas en `auditoria.cambios_clientes` y las exportaciones de bitácoras deben sanear secretos o datos sensibles innecesarios antes de compartirse.
* **Responsable:** Define finalidad y aprueba accesos el propietario del dato; aplica controles técnicos, resguarda bitácoras y sanea extracciones el DBA.
* **Excepciones:** Solo se permiten accesos puntuales adicionales para atención de incidentes o auditoría interna bajo justificación documentada y control de acceso temporal, conservando la pista de auditoría correspondiente.
* **Evidencia:** Diccionario de datos con clasificación y finalidad (`evidencias/03-diccionario.md` y `evidencias/diccionario-clientes.csv`), matriz de responsables (`evidencias/02-responsables.md`), disparadores activos (`evidencias/E3-disparadores.txt`), muestra de `auditoria.cambios_clientes` (`evidencias/E4-muestra-auditoria.txt`) y bitácora saneada (`evidencias/E5-bitacora-postgresql.log` con `replace(..., 'Temporal_Practica_2026', '<REDACTED>')`).
* **Revisión:** Se revisa cada 6 meses o ante cambios en la estructura de `clientes`, nuevas vistas/reportes o hallazgos de privacidad.
