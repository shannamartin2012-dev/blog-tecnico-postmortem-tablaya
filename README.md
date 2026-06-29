# blog-tecnico-postmortem-tablaya
Post-Mortem Constructivo: Fricciones en el Embudo de Reservas de TablaYa
Autor: Sandy Katherine Martin Mendez | Fecha: junio de 2026 | Categoría: Producto Digital, UX/CX, Reservas Online
Nota para el lector: esta entrada está escrita para ser comprendida tanto por perfiles técnicos (desarrollo, producto) como por perfiles de negocio (operaciones, atención al cliente). Cada término específico de la industria (MTTR, KPI, SLA, conversión) se explica brevemente en su primera aparición.
1. Contexto
TablaYa es una aplicación de reservas para restaurantes que permite a los usuarios encontrar mesa disponible, reservar en pocos pasos y recibir confirmación inmediata. El producto se dirige principalmente a un perfil que llamamos Andrés: 34 años, profesional con agenda variable, que suele decidir dónde cenar con poca anticipación (entre 30 y 90 minutos antes) y que abandona cualquier app que le tome más de un minuto en confirmar una reserva.
El objetivo de este análisis fue auditar el flujo completo de reserva —desde la búsqueda del restaurante hasta la confirmación final— para identificar por qué, a pesar de tener buen tráfico de usuarios nuevos, la tasa de reservas completadas era notablemente inferior a la esperada para este tipo de producto.
Propósito del ejercicio
•	Aplicar un marco de diagnóstico de experiencia de usuario para separar síntomas visuales de causas raíz reales.
•	Traducir los hallazgos de negocio en requerimientos técnicos accionables (historias de usuario) para un backlog ágil.
•	Practicar la documentación técnica clara para audiencias mixtas, siguiendo la estructura problema–acción–impacto.
•	Aplicar y reflexionar sobre el uso de feedback radicalmente sincero durante el proceso de revisión del propio análisis.
2. Problema
Durante la auditoría del flujo de reserva se identificaron tres capas de fricción que, en conjunto, explicaban la caída de conversión:
Capa de fricción	Causa raíz diagnosticada	Impacto / consecuencia
1. Disponibilidad poco confiable	Los horarios mostrados como “disponibles” no se sincronizaban en tiempo real con el sistema del restaurante, generando reservas rechazadas tras la confirmación.	Pérdida de confianza inmediata y abandono tras el primer rechazo de reserva.
2. Formulario de reserva extenso	El flujo solicitaba datos redundantes (nombre, teléfono, correo, preferencias) en pantallas separadas antes de confirmar.	Abandono del proceso (drop-off) entre el paso 2 y el paso 4 del formulario.
3. Confirmación y soporte lentos	La confirmación final dependía de validación manual por parte del restaurante, sin automatización ni aviso claro de tiempos de espera.	Usuarios reservando en paralelo por otro canal (llamada telefónica) o cancelando por falta de respuesta.

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
4. KPIs de seguimiento
Para verificar el impacto real de estas acciones se definió un tablero mínimo de control:
•	Tasa de conversión del embudo (CR): porcentaje de usuarios que completan una reserva tras iniciar el flujo.
•	Tasa de abandono del formulario: medición específica del drop-off entre el paso 2 y el paso 4 actuales.
•	SLA de confirmación: tiempo objetivo de respuesta menor a 2 minutos para reservas automatizadas.
•	Reservas rechazadas post-confirmación: meta de reducción a 0% tras la integración en tiempo real.
5. Aprendizajes
•	Los síntomas visuales no son la causa: un formulario largo es un síntoma; la causa raíz era la falta de priorización de campos obligatorios frente a opcionales.
•	La confianza se rompe en milisegundos: un solo rechazo post-confirmación es suficiente para que un usuario no vuelva a intentarlo.
•	Automatizar libera capacidad humana: delegar la confirmación automática en los casos simples permite que el equipo de soporte se enfoque en los casos verdaderamente complejos.
•	Documentar con la estructura problema–acción–impacto permitió que este análisis fuera comprensible tanto para el equipo técnico como para operaciones, sin perder rigor.

6. Reflexión: aplicación de feedback radicalmente sincero
Este análisis no llegó a su versión final en un solo intento. Atravesó tres iteraciones, cada una impulsada por feedback directo y, a la vez, respetuoso con el trabajo ya realizado — el equilibrio que propone el modelo de Radical Candor entre cuidado personal y desafío directo.
•	Iteración 1 → 2 (foco en causa raíz): la primera versión describía únicamente síntomas (“el formulario es largo”, “la confirmación es lenta”). El feedback recibido señaló que estas observaciones eran superficiales y no explicaban el porqué. Esto llevó a profundizar hasta identificar las causas raíz reales: falta de integración en tiempo real y dependencia de validación manual.
•	Iteración 2 → 3 (de lo cualitativo a lo medible): se señaló que las soluciones propuestas no tenían forma de verificarse objetivamente. En respuesta, se incorporaron los KPIs y el SLA de confirmación, convirtiendo recomendaciones generales en metas verificables.
•	Iteración 3 → versión final (foco en el usuario real): se ajustaron las soluciones para responder específicamente a las restricciones de tiempo de Andrés, evitando soluciones genéricas que no consideraran su contexto real de uso.
Para estructurar uno de los momentos de retroalimentación más relevantes del proceso, utilicé el modelo SCI (Situación – Comportamiento – Impacto), que ayuda a separar el hecho observable de cualquier juicio sobre la persona:
“Durante la revisión intermedia del análisis (Situación), cuando nos enfocamos en describir fricciones visuales en lugar de causas estructurales del sistema (Comportamiento), perdimos la posibilidad de generar requerimientos técnicos precisos para el equipo de desarrollo (Impacto). Por eso, propongo reformular cada fricción como una historia de usuario con un criterio de aceptación claro (Acción correctiva).”
Recibir este tipo de feedback con apertura — en lugar de defensividad — fue posible gracias a separar el comentario sobre el trabajo de cualquier juicio sobre mi capacidad. Ese es, en mi experiencia, el núcleo real de una mentalidad de crecimiento aplicada a la comunicación técnica: tratar cada observación como información útil para iterar, no como una evaluación personal.
