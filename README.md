# Vanguard: evaluación de un rediseño mediante un experimento A/B

Proyecto de análisis de datos desarrollado por **Salvador y Walnert** durante el bootcamp de Data Analytics de Ironhack.

## Objetivo y contexto del negocio

Vanguard es una empresa de servicios de inversión que busca evaluar un rediseño de su proceso digital antes de extenderlo a todos los usuarios.

Comparamos dos grupos:

- **Control:** clientes que utilizan el diseño original.
- **Test:** clientes que utilizan el nuevo diseño.

El objetivo es determinar si el rediseño mejora la finalización y cómo cambia el recorrido de los clientes. Analizamos también los tiempos entre pasos, los retrocesos y los perfiles de los participantes para fundamentar una recomendación.

## Preguntas del análisis

- ¿Finalizan el proceso más clientes con el nuevo diseño?
- ¿Existe evidencia de que la mejora relativa supera el 5 %?
- ¿En qué etapas dejan de avanzar las visitas?
- ¿Cambian los tiempos entre pasos y la frecuencia de retrocesos?
- ¿Qué relación existe entre la edad y la finalización?
- ¿Difieren las edades y antigüedades medias de Test y Control?

## Datos utilizados

El análisis parte de cuatro archivos:

| Archivo | Contenido |
|---|---|
| `df_final_demo.txt` | Perfil de los clientes: edad, antigüedad, saldo y otras características. |
| `df_final_experiment_clients.txt` | Asignación de los clientes a Test o Control. |
| `df_final_web_data_pt_1.txt` | Primera parte de los eventos del proceso digital. |
| `df_final_web_data_pt_2.txt` | Segunda parte de los eventos del proceso digital. |

Los eventos abarcan del **15 de marzo al 20 de junio de 2017**.

La muestra del experimento incluye **50.500 clientes**:

- **23.532** en Control.
- **26.968** en Test.

Campos principales:

| Campo | Significado |
|---|---|
| `client_id` | Identificador del cliente. |
| `visitor_id` | Identificador del visitante. |
| `visit_id` | Identificador de la visita. |
| `Variation` | Grupo experimental: Test o Control. |
| `process_step` | Etapa: `start`, `step_1`, `step_2`, `step_3` o `confirm`. |
| `date_time` | Fecha y hora del evento. |

## Preparación y calidad de los datos

1. Unimos las dos fuentes de eventos web.
2. Eliminamos **10.764 filas exactamente duplicadas**, considerando todos los pasos del proceso. Las repeticiones que no son duplicados exactos se conservan.
3. Incorporamos la asignación experimental y seleccionamos los participantes de Test y Control.
4. Identificamos las visitas mediante la combinación de `client_id` y `visit_id`.
5. Revisamos los eventos simultáneos que impiden establecer el orden de los pasos. Los intervalos afectados se excluyen de los cálculos de tiempos y retrocesos.
6. Detectamos **371 clientes** cuya antigüedad en meses supera su edad multiplicada por 12. Se conservan en el análisis principal y se repiten determinados contrastes sin ellos para comprobar su influencia.
7. Excluimos los valores ausentes en las variables necesarias para cada contraste, sin eliminar indiscriminadamente al cliente del resto del análisis.

## Métricas y metodología

Las métricas utilizan distintas unidades de análisis:

| Métrica | Definición |
|---|---|
| Finalización por cliente | Porcentaje de clientes que registra al menos una confirmación durante el periodo. |
| Finalización en primera visita | Porcentaje de clientes que confirma en su primera visita observada. |
| Tiempo entre pasos | Duración de los intervalos observables entre eventos consecutivos. |
| Tasa de retrocesos | Porcentaje de transiciones válidas que vuelve a una etapa anterior. |
| Embudo de visitas | Visitas que alcanzan las etapas en orden, respecto a las visitas con inicio de cada grupo. |

Los retrocesos son una señal del comportamiento de navegación, **no una medición directa de errores técnicos**.

### Construcción del embudo

El recorrido analizado es:

**Inicio → Paso 1 → Paso 2 → Paso 3 → Confirmación**

Para avanzar, cada etapa debe registrarse después de la anterior dentro de la misma visita. Se permiten repeticiones y retrocesos, pero los eventos con la misma fecha y hora no permiten establecer un orden.

El denominador es el número de visitas con un inicio registrado. Las visitas sin inicio quedan fuera del embudo.

Por tanto, sus porcentajes no deben confundirse con la finalización por cliente: un cliente puede realizar varias visitas o confirmar sin que se observe toda la secuencia.

### Contrastes estadísticos

Utilizamos un nivel de significación de **0,05**:

- Test t de Welch unilateral para evaluar la mejora de finalización y el umbral del 5 % relativo.
- Chi-cuadrado para estudiar la asociación entre edad y finalización dentro de cada grupo.
- Test t de Welch bilateral para comparar las edades y antigüedades medias de Test y Control.

**H₀, hipótesis nula**, es la afirmación de referencia que ponemos a prueba. **H₁, hipótesis alternativa**, expresa la diferencia o relación que queremos evaluar.

Cada contraste tiene sus propias hipótesis. En la comparación principal, H₀ plantea que Test no mejora la finalización y H₁ que sí la mejora.

No rechazar H₀ significa que no hay evidencia suficiente para rechazarla; no demuestra que sea verdadera.

## Principales resultados

### 1. Test mejora la finalización

| Indicador | Control | Test |
|---|---:|---:|
| Finalización por cliente | 65,59 % | 69,29 % |
| Finalización en primera visita | 53,39 % | 57,15 % |
| Retrocesos por transición | 8,66 % | 11,50 % |

La finalización por cliente aumenta **3,70 puntos porcentuales**, equivalentes a una mejora relativa observada del **5,65 %**.

Existe evidencia estadística de mejora, con un p-valor aproximado de **4,00 × 10⁻¹⁹**.

Sin embargo, el contraste del umbral obtiene un p-valor de **0,1604**: no hay evidencia suficiente para afirmar que la mejora real supere el **5 % relativo**.

### 2. El mayor avance convive con dificultades en el recorrido

Test presenta más retrocesos por transición y un mayor tiempo en el paso 1.

Estos resultados señalan aspectos que conviene investigar. Por sí solos no permiten determinar si se deben a confusión, revisión de información u otros comportamientos.

### 3. El comienzo concentra la mayor caída del embudo

Porcentaje de visitas que alcanza cada etapa en orden, respecto a las visitas con inicio:

| Etapa | Control | Test |
|---|---:|---:|
| Inicio | 100,00 % | 100,00 % |
| Paso 1 | 75,28 % | 84,46 % |
| Paso 2 | 63,92 % | 72,64 % |
| Paso 3 | 57,41 % | 65,04 % |
| Confirmación | 47,88 % | 53,87 % |

La diferencia al llegar a confirmación es de **5,99 puntos porcentuales** a favor de Test.

La mayor caída entre etapas se produce entre inicio y paso 1 en ambos grupos. No continuar dentro de esta secuencia no equivale necesariamente a abandonar definitivamente: el cliente puede regresar en otra visita.

### 4. La edad se relaciona con la finalización

Existe evidencia de asociación entre los tramos de edad y la finalización en ambos grupos, también al excluir los casos dudosos.

Los clientes de 70 años o más presentan las menores tasas descriptivas de finalización. Esto no demuestra causalidad ni que el rediseño tenga un efecto diferente según la edad.

### 5. La composición de los grupos requiere una interpretación cuidadosa

- **Edad:** Test tiene una edad media aproximadamente 0,33 años menor. La diferencia es estadísticamente significativa, pero pequeña.
- **Antigüedad:** la diferencia media es aproximadamente de −0,10 años. El contraste principal no es significativo, pero sí lo es al excluir los casos dudosos.

Estos contrastes no demuestran por sí solos que los grupos sean equivalentes ni que la asignación esté libre de sesgos.

## Recomendación de negocio

Los resultados apoyan una mejora de la finalización con Test, pero recomendamos completar la evaluación antes de un despliegue general:

1. Investigar el aumento de los retrocesos y el mayor tiempo del paso 1.
2. Revisar el inicio del recorrido, donde se concentra la mayor caída del embudo.
3. Investigar las dificultades de los segmentos con menor finalización mediante pruebas de usabilidad.
4. Comprobar la estabilidad del efecto a lo largo del tiempo.
5. Incorporar costes y beneficios para evaluar la rentabilidad del rediseño.

La mejora observada del 5,65 % no basta para demostrar que la mejora real supera el umbral del 5 %, ni permite concluir por sí sola si el rediseño es rentable.

## Limitaciones

- No disponemos del protocolo de asignación ni de la proporción prevista entre Test y Control.
- No hemos confirmado un seguimiento equivalente para todos los clientes ni analizado la estabilidad semanal del efecto.
- Los tiempos pueden incluir pausas y no representan actividad continua en pantalla.
- Los eventos simultáneos limitan la reconstrucción de algunos recorridos.
- Los retrocesos no identifican los motivos de navegación ni las incidencias técnicas.
- La conclusión sobre antigüedad es sensible a la exclusión de casos dudosos.
- No disponemos de información suficiente sobre dispositivos, motivos de interrupción, costes o beneficios posteriores a la confirmación.

## Dashboard en Tableau

El archivo `data/tableau/vanguard_ab_dashboard.twbx` presenta la historia del análisis mediante cuatro bloques:

1. **Resultados:** qué cambia con el nuevo diseño.
2. **Recorrido:** dónde avanzan las visitas y dónde aparecen dificultades.
3. **Perfiles:** cómo se distribuyen los resultados entre los clientes.
4. **Evidencia y decisión:** qué respaldan los contrastes y qué recomendamos.

El selector **Ambos / Control / Test** permite explorar las visualizaciones vinculadas a él. El embudo estático y los contrastes precalculados no se recalculan con este selector.

El libro está empaquetado con extracciones. El CSV del embudo se incluye para reproducir el análisis; no constituye una conexión en vivo del dashboard.

## Estructura del repositorio

```text
Proyecto3Tableu/
├── README.md
├── requirements.txt
├── .gitignore
├── analisis_vanguard.ipynb
├── df_final.csv
├── archivos/
│   ├── Prueba1Salva.ipynb
│   └── prueba2walnert.ipynb
└── data/
    ├── df_final_demo.txt
    ├── df_final_experiment_clients.txt
    ├── df_final_web_data_pt_1.txt
    ├── df_final_web_data_pt_2.txt
    └── tableau/
        ├── clientes_tableau.csv
        ├── visitas_tableau.csv
        ├── tiempos_tableau.csv
        ├── transiciones_tableau.csv
        ├── embudo_tableau.csv
        ├── resumen_contrastes.csv
        └── vanguard_ab_dashboard.twbx
```

- `analisis_vanguard.ipynb`: notebook principal con el análisis integrado.
- `archivos/`: notebooks de trabajo anteriores, conservados como referencia.
- `data/`: archivos originales utilizados por el notebook principal.
- `data/tableau/`: tablas exportadas y dashboard empaquetado.

## Cómo reproducir el análisis

### 1. Descargar el repositorio

```bash
git clone https://github.com/walnertpadron18/Proyecto3Tableu.git
cd Proyecto3Tableu
```

### 2. Instalar las dependencias

```bash
python -m pip install -r requirements.txt
```

### 3. Ejecutar el notebook

Abre la carpeta del proyecto en VS Code y selecciona `analisis_vanguard.ipynb`.

Selecciona el entorno de Python donde instalaste las dependencias, reinicia el kernel y ejecuta todas las celdas en orden.

Mantén el directorio de trabajo en la raíz del proyecto para que funcionen las rutas relativas `data/...`.

El notebook genera los seis CSV de `data/tableau/`, incluido el resumen con **diez resultados estadísticos** correspondientes a los cinco contrastes y sus comprobaciones.

### 4. Abrir el dashboard

Abre `data/tableau/vanguard_ab_dashboard.twbx` en Tableau.

Regenerar los CSV no actualiza automáticamente las extracciones ni el embudo estático del libro. Si cambian los datos, también deben actualizarse y verificarse los elementos correspondientes en Tableau.

## Herramientas

- Python.
- pandas.
- SciPy.
- Matplotlib y seaborn.
- Jupyter y VS Code.
- Tableau.
- Git y GitHub.

## Autores

**Salvador y Walnert** — Proyecto de Data Analytics, Ironhack.
