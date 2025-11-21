# Descripción General

Bienvenido a mi análisis del mercado laboral en el área de datos, con un enfoque en los roles de **Data Analyst**. Este proyecto surge del deseo de comprender y navegar el mercado laboral de forma más efectiva. Explora las habilidades mejor pagadas y más demandadas, con el objetivo de identificar oportunidades óptimas para analistas de datos.

Los datos provienen del **curso de Python de Luke Barousse**, que sirve como base para este análisis e incluye información detallada sobre títulos de trabajo, salarios, ubicaciones y habilidades esenciales. A través de varios scripts de Python, investigo preguntas clave como: cuáles son las habilidades más demandadas, cómo evolucionan las tendencias salariales y cuál es la intersección entre demanda y salario en el análisis de datos.

---

# Preguntas del Proyecto

A continuación, las preguntas que busco responder:

- ¿Cuáles son las habilidades más demandadas para los **3 roles de datos más populares**?
- ¿Cómo están evolucionando las **habilidades más solicitadas** para los Data Analysts?
- ¿Qué tan bien **pagan los trabajos y las habilidades** en roles de Data Analyst?
- ¿Cuáles son las **habilidades óptimas** para que un Data Analyst aprenda? (Alta demanda **y** alto salario)

---

# Herramientas Utilizadas

Para realizar este análisis del mercado laboral de Data Analysts, utilicé varias herramientas clave:

- **Python**: La base del análisis, permitiéndome manipular los datos y extraer insights. También utilicé las siguientes librerías:
  - **Pandas**: Para analizar y manipular los datos.
  - **Matplotlib**: Para visualizar la información.
  - **Seaborn**: Para generar visualizaciones más avanzadas.
- **Jupyter Notebooks**: Mi entorno principal para ejecutar scripts de Python y documentar el análisis.
- **Visual Studio Code**: El editor que utilicé para ejecutar scripts y organizar el proyecto.
- **Git & GitHub**: Herramientas esenciales para control de versiones y publicación del código, facilitando colaboración y transparencia.

---

# Preparación y Limpieza de Datos

Esta sección describe los pasos realizados para preparar los datos antes del análisis, garantizando calidad y consistencia.

## **Importación y Limpieza Inicial**

Primero importé las librerías necesarias y cargué el dataset. Luego realicé tareas iniciales de limpieza para asegurar la calidad de los datos.

```python
# Importación de librerías
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Carga de datos
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Limpieza de datos
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

# Analisis

## 1. Cuales son las habilidades tecnicas mas demandas en los puestos de trabajo mas populares de datos? 

Para encontrar las habilidades mas demandas en los puestos mas populares. Filtre estos puestos por los mas populares, y despues dentro de cada uno filtre las 5 habilidades mas demandadas. Este analisis resalta los puestos y que habilidades son las mas populares dentro de este rubro, mostrando asi en que deberia enfocarme dependiendo del puesto al que apunte.

### Visualización de datos
```python
fig, ax = plt.subplots(len(job_titles), 1)
sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x="skill_percent", y="job_skills", ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Resultados

![Visualizacion de las habilidades mas demandadas](images\skills_demand_barh.png)

### Conclusiones

- **SQL es la habilidad más transversal**, requerida en los tres roles analizados, mostrando su importancia en cualquier puesto relacionado con datos.

- **Python domina en roles técnicos** (Data Engineer y Data Scientist), reflejando su uso para automatización, modelado y manejo de grandes volúmenes de datos.

- **Data Analyst requiere más herramientas de BI** como Excel, Tableau y Power BI, evidenciando un enfoque en reportes, visualización y análisis descriptivo.

- **Data Engineer demanda competencias en infraestructura y nube**, especialmente AWS, Azure y Spark, lo que indica un rol orientado al diseño de pipelines y sistemas.

- **Data Scientist combina programación y estadística**, con Python, SQL y R como pilares para análisis avanzados y modelado predictivo.


## 2. Cuales son las habilidades mas demandadas para los Data Analyst?
Para identificar las habilidades más demandadas, primero filtré las publicaciones correspondientes exclusivamente al rol de Data Analyst. Luego realicé un agrupamiento por habilidades según cada mes y, a partir de ese análisis, obtuve las cinco habilidades más solicitadas por mes durante 2023.

Podes ver mi notebook aca:
[3_Skills_Trends](3_Project\3_Skills_Trends.ipynb)

### Visualización de datos
```python
from matplotlib.ticker import PercentFormatter
df_plot = df_DA_percent.iloc[:, :5]

sns.lineplot(df_plot, dashes=False)
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Resultados
![Visualizacion de las habilidades mas demandadas](images\trendig_skills_da.png)

### Conclusiones
- **SQL se mantiene como la habilidad más demandada** durante todo el año, con una presencia cercana al 45–50% en las publicaciones.

- **Excel y Python ocupan el segundo grupo más solicitado**, con una demanda estable alrededor del 30–36%.

- **Tableau y Power BI muestran menor frecuencia**, pero mantienen una presencia constante, indicando que las herramientas de visualización siguen siendo relevantes.


## 3. Qué tan bien pago estan los trabajos y habilidades de los Data Analyst?

Para identificar los puestos y habilidades mejor pagados, solo busqué empleos en Estados Unidos y analicé su salario medio. Pero antes, examiné la distribución salarial de puestos comunes en el sector de datos, como científico de datos, ingeniero de datos y analista de datos, para hacerme una idea de cuáles son los empleos mejor remunerados.

### Visualización de datos
```python
sns.boxplot(data=df_top_6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

### Resultados
![Visualizacion de la distrucion de los salarios](images\salary_distribution.png)

### Conclusiones

- Al comparar el crecimiento salarial por trayectoria, se observa que optar por roles más especializados, como Data Scientist o Data Engineer, ofrece medianas salariales superiores a las de un Senior Data Analyst. Esto sugiere que profundizar en un área técnica específica puede resultar más rentable que avanzar únicamente dentro del camino analítico tradicional.

- La comparación entre roles senior y especializados evidencia diferencias claras en la progresión económica dentro del ámbito de datos. Mientras que el ascenso a Senior Data Analyst implica una mejora salarial moderada, los boxplots muestran que los roles especializados —como Data Scientist y Data Engineer— no solo parten de medianas más altas, sino que además cuentan con un rango salarial considerablemente mayor. Esto indica que la profundización en habilidades técnicas avanzadas suele traducirse en un retorno económico superior.

- En los roles analíticos, como Data Analyst y Senior Data Analyst, la distribución salarial es mucho más estable y concentrada, con menor dispersión y menos outliers en comparación con puestos más técnicos. Esto sugiere que, aunque estos roles ofrecen una progresión salarial más predecible y consistente, también presentan un techo más bajo en términos de potencial de crecimiento económico, reflejando un mercado con menor variabilidad y menor oportunidad de alcanzar salarios extraordinariamente altos.

### Highest Paid & Most Demanded Skills for Data Analysts
A continuación, centré mi análisis exclusivamente en los puestos de analista de datos. Analicé las habilidades mejor pagadas y las más demandadas. Para ello, utilicé dos gráficos de barras.


### Visualización de datos
```python
fig, ax = plt.subplots(2, 1)  

sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

### Resultados
![Las Habilidades Mejores Pagas y las mas Demandadas](images\highest_paid_skills.png)

### Conclusiones
- La diferencia entre demanda y salario revela dos caminos distintos de desarrollo profesional: uno enfocado en habilidades fundamentales altamente demandadas (mayor estabilidad laboral) y otro en habilidades técnicas de nicho (mayor potencial salarial).
- Existe una desconexión marcada entre las habilidades mejor remuneradas y las más demandadas. Las habilidades que generan los salarios más altos (como SVN, Solidity, dplyr, Terraform, GitLab) no coinciden con las más solicitadas en el mercado, lo que indica nichos altamente especializados pero con baja demanda masiva.

## 4. Cuáles son las habilidades que deberian aprender los Data Analyst?

Para identificar las habilidades más óptimas para aprender (las mejor pagadas y con mayor demanda), calculé el porcentaje de demanda de cada habilidad y el salario medio correspondiente. Así se facilita la identificación de las habilidades más óptimas para aprender.

## Visualización de datos
```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()

```

### Resultados
![Las habilidades mas optimas para Data Analytics](images\most_optimal_skills.png)

### Conclusiones
- SQL y Python se posicionan como las habilidades más “óptimas”, ya que combinan alta demanda en el mercado con salarios competitivos, convirtiéndose en pilares centrales para el rol de Data Analyst.

- Herramientas de visualización como Tableau y Power BI también muestran una buena relación demanda–salario, ubicándose en un rango alto de empleabilidad con remuneraciones superiores al promedio.

- Las habilidades cloud (AWS, Azure) presentan salarios muy altos pero baja presencia en ofertas, lo que indica un nicho reducido pero altamente rentable para quienes buscan una especialización técnica con fuerte impacto salarial.

# Lo Que Aprendí

A lo largo de este proyecto, profundicé mi comprensión del mercado laboral para analistas de datos y mejoré mis habilidades técnicas en Python, especialmente en manipulación y visualización de datos. Aquí algunos puntos específicos que aprendí:

- **Uso avanzado de Python:** Utilizar librerías como Pandas para manipulación de datos, Seaborn y Matplotlib para visualización, junto con otras herramientas, me permitió realizar tareas complejas de análisis de datos de manera más eficiente.
- **Importancia de la limpieza de datos:** Aprendí que una limpieza y preparación exhaustiva de los datos es crucial antes de realizar cualquier análisis, ya que garantiza la precisión de las conclusiones obtenidas.
- **Análisis estratégico de habilidades:** El proyecto resaltó la importancia de alinear las propias habilidades con la demanda del mercado. Comprender la relación entre demanda de habilidades, salario y disponibilidad de empleos permite una planificación profesional más estratégica dentro de la industria tecnológica.

---

# Conclusiones

Este proyecto proporcionó varias conclusiones generales sobre el mercado laboral de datos para analistas:

- **Correlación entre demanda de habilidades y salarios:** Existe una clara relación entre la demanda de ciertas habilidades y los salarios asociados. Habilidades avanzadas y especializadas como Python u Oracle suelen llevar a remuneraciones más altas.
- **Tendencias del mercado:** Las tendencias en habilidades demandadas cambian constantemente, destacando la naturaleza dinámica del mercado laboral en datos. Mantenerse actualizado es esencial para crecer profesionalmente.
- **Valor económico de las habilidades:** Entender qué habilidades tienen alta demanda y buena remuneración puede guiar a los analistas de datos a priorizar su aprendizaje para maximizar su retorno económico.

---

# Desafíos que Enfrenté

Este proyecto no estuvo exento de desafíos, pero cada uno representó una oportunidad de aprendizaje:

- **Inconsistencias en los datos:** Manejar valores faltantes o inconsistentes requiere atención detallada y técnicas sólidas de limpieza para asegurar la integridad del análisis.
- **Visualización de datos complejos:** Diseñar representaciones visuales efectivas para conjuntos de datos complejos fue un desafío, pero esencial para comunicar insights de forma clara y convincente.
---

# Conclusión

Esta exploración del mercado laboral para Data Analysts ha sido sumamente informativa, destacando las habilidades y tendencias que moldean este campo en constante evolución. Las conclusiones obtenidas fortalecen mi entendimiento y ofrecen una guía aplicable para quienes buscan avanzar en su carrera dentro del análisis de datos.

A medida que el mercado continúa cambiando, será esencial realizar análisis continuos para mantenerse actualizado. Este proyecto constituye una base sólida para futuras investigaciones y subraya la importancia del aprendizaje continuo y la adaptación dentro del mundo de los datos.
