Título: Análisis del testing 

Objetivo: Comparar el grupo de test (nueva versión) con el de control para decidir si la nueva versión mejora la experiencia del usuario en el proceso.

Contexto del negocio: [tipo de empresa]. El reto es saber si el rediseño funciona antes de implantarlo para todos. La decisión es lanzar la nueva versión, ajustarla o descartarla.

Dataset: df_web, df_control y df_test.

client_id: cliente
visitor_id: visitante
visit_id: visita
process_step: paso del embudo (start, step_1, step_2, step_3, confirm)
date_time: fecha y hora del evento

Calidad del dato: Hay duplicados en los pasos start y confirm, que se eliminan. En step_1, step_2 y step_3 se conservan porque pueden ser repeticiones reales del usuario.

Preguntas clave:

¿Es menor la tasa de error en test que en control?
¿Llega más gente a confirm en test?
[¿Cambia el tiempo entre pasos?]

Proceso de análisis:

Limpieza de duplicados.
Cálculo de la tasa de error, definida como retroceder a un paso anterior dentro de una visita.
Cálculo de métricas del embudo [tasa de finalización, tiempo por paso].
Comparación entre control y test.

Resultados / Insights: [tasa de error control vs test, tasa de finalización, diferencia en %].

Recomendaciones de negocio: [implantar o no según los resultados, y en qué paso del embudo se pierden más usuarios].

Limitaciones: [tamaño de la muestra, duración del test, falta de test de significancia estadística]

Próximos pasos: Aplicar un test estadístico (chi-cuadrado o z-test de proporciones), segmentar por tipo de cliente y ampliar el periodo del test.

Cómo replicar: Enlace GITHUB.

