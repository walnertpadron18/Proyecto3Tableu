# Vanguard · Análisis de un experimento A/B

Proyecto de **Salvador Cruz de Bustos y Walnert Padrón** para el bootcamp de Data Analytics de Ironhack.

## Objetivo

Evaluar si un rediseño del proceso digital mejora la finalización y la experiencia de los clientes de Vanguard.

Comparamos **Control** (diseño original) con **Test** (nuevo diseño), analizando finalización, tiempos entre pasos, retrocesos y perfiles de clientes.

## Datos y metodología

- **50.500 participantes:** 23.532 en Control y 26.968 en Test.
- **Periodo:** del 15 de marzo al 20 de junio de 2017.
- **Fuentes:** perfiles de clientes, asignación experimental y eventos de navegación.

Eliminamos 10.764 registros web exactamente duplicados y revisamos eventos de orden ambiguo e inconsistencias entre edad y antigüedad.

Utilizamos contrastes de Welch y chi-cuadrado con un nivel de significación de 0,05. La finalización cuenta clientes; el embudo cuenta visitas que avanzan por las etapas en orden.

## Resultados principales

| Indicador | Control | Test |
|---|---:|---:|
| Finalización por cliente | 65,59 % | 69,29 % |
| Finalización en primera visita | 53,39 % | 57,15 % |
| Retrocesos por transición | 8,66 % | 11,50 % |

Test mejora la finalización en **3,70 puntos porcentuales**, una mejora relativa observada del **5,65 %**.

Existe evidencia estadística de mejora, pero no suficiente para afirmar que la mejora real supera el **5 % relativo**. Además, Test presenta más retrocesos y un mayor tiempo en el paso 1.

## Recomendación y limitaciones

Recomendamos investigar las dificultades del recorrido, validar con datos actuales y evaluar la estabilidad y rentabilidad antes de extender el rediseño.

Desconocemos el protocolo de asignación. Los tiempos pueden incluir pausas y los retrocesos no equivalen a errores técnicos. Los datos son históricos y no explican los motivos del comportamiento.

## Cómo ejecutar

```bash
git clone https://github.com/walnertpadron18/Proyecto3Tableu.git
cd Proyecto3Tableu
python -m pip install -r requirements.txt
```

Abre `analisis_vanguard.ipynb`, selecciona el entorno de Python y ejecuta todas las celdas desde la raíz del proyecto.

Para consultar la presentación, abre el archivo `.twbx` de `data/tableau/` en Tableau. Regenerar los CSV no actualiza automáticamente las extracciones ni el embudo estático del libro.

## Herramientas

Python · pandas · SciPy · Matplotlib · seaborn · Jupyter · Tableau · Git y GitHub
