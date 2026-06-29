3. Acciones
3.1 Mapeo del recorrido y diagnóstico
Se reconstruyó el recorrido de Andrés en tres fases —Búsqueda, Reserva y Confirmación— comparando la experiencia actual contra el estado ideal. Esto permitió aislar en qué paso exacto del embudo ocurría cada caída, en lugar de asumir que “la app es lenta” como diagnóstico general.
3.2 Soluciones propuestas
•	Disponibilidad en tiempo real: integración directa con el sistema de gestión de mesas del restaurante mediante webhooks, eliminando el desfase entre lo mostrado en la app y la disponibilidad real.
•	Reserva en un solo paso: rediseño del formulario para reducir los campos obligatorios a tres (nombre, teléfono, número de personas), dejando el resto como opcional.
•	Confirmación automática con SLA visible: se reemplazó la validación manual por una confirmación automática para restaurantes con disponibilidad verificada, y un mensaje de “tiempo estimado de respuesta” para los demás casos.
3.3 De la fricción al backlog: historias de usuario
Los hallazgos se tradujeron en historias de usuario priorizadas, listas para ingresar a un sprint:
•	US-01 (Crítica): Como Andrés, quiero ver solo horarios realmente disponibles para no recibir un rechazo después de reservar.
•	US-02 (Crítica): Como Andrés, quiero completar mi reserva en una sola pantalla para hacerlo en menos de 60 segundos.
•	US-03 (Alta): Como Andrés, quiero recibir confirmación inmediata o un tiempo de espera claro para decidir si busco otra opción.
3.4 Post-mortem del proceso analítico
Siguiendo la cultura de post-mortem de Google SRE, este análisis se centró en el diseño del sistema y del flujo, evitando cualquier señalamiento a personas o equipos específicos.
Línea de tiempo del diagnóstico: 
•	Día 1 — Extracción de datos del embudo: se detecta una caída marcada entre el inicio del formulario y la confirmación.
•	Día 1 — Auditoría del flujo de reserva: se identifica la desincronización de horarios como posible causa.
•	Día 2 — Revisión del formulario: se cuantifican los campos solicitados y se comparan con apps equivalentes del sector.
•	Día 2 — Análisis de soporte: se mide un tiempo de respuesta promedio superior a 20 minutos en confirmaciones manuales.
•	Día 3 — Consolidación: se confirma que las tres fricciones actúan de forma acumulativa sobre el mismo grupo de usuarios.
Causas raíz: 
1.	Falta de integración en tiempo real entre la app y el sistema de gestión del restaurante.
2.	Formulario diseñado sin priorizar velocidad de conversión, heredado de una versión inicial del producto.
3.	Dependencia total de validación humana para confirmar reservas, sin una capa de automatización básica.
