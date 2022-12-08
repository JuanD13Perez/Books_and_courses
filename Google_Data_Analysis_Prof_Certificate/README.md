# Google Data Analysis Professional Capstone Project
- [English Version](#English-Version) 
- [Spanish Version](#Spanish-Version)

## Spanish Version

### **Problema de negocio** 📝
Este proyecto hace parte del certificado profesional de Google en análisis de datos.
Usamos el dataset de la marca _FitBit_, disponible vía [**Kaggle**](https://www.kaggle.com/datasets/arashnic/fitbit), que corresponden a registros de un dispositvo (Smartwatch) con información de consumo de calorías, número de pasos, etc.
En este caso, asumimos que trabajamos para Bellabeat, una competidora de FitBit. Nuestra tarea consiste en **identificar patrones de consumo en los usuarios de los productos FitBit** que puedan apoyar la estrategia de Marketing de la compañía.

| ![Banner](FitBit_Banner.png) |
|:--:|
| <b>Image Credits - Banner - https://www.kaggle.com/datasets/arashnic/fitbit  </b>|


### **Highlights** 
Este proyecto se hizo utlizando python 3.10.5 en un entorno de Jupyter Notebooks. Consta de tres secciones principales:
- [**Apéndice**](#Apéndice)
- [**Data Cleaning**](#Data-Cleaning)
- [**Data Analysis**](#Data-Analysis)
### Apéndice
Aquí manipulamos desde el sistema el [dataset](https://www.kaggle.com/datasets/arashnic/fitbit) con el fin de obtener todos los archivos csv como pandas dataframes. Nos dimos cuenta que la estructura de datos es similar a la de una base relacional y que ciertos dataframes son meramente agregaciones de los datos minuto a minuto. Decidimos trabajar únicamente con éstos dataframes 'raíz'. 

Tenemos información por minuto de:
* Calorías
* Intensidad de ejercicio
* METs
* Número de pasos
* Ritmo cardíaco
Información diaria de:
* Sueño
* Peso

### Data Cleaning:

0. Restringimos a 19 usuarios de los 25 originalmente disponibles. Ya que 6 usuarios tenían la información muy incompleta (no alcanzaban a cubrir un mes de datos).
1. Restringimos el rango de fechas desde 2016-04-12 hasta 2016-05-11 minuto a minuto. Esto para tener información de tiempo comparable para los distintos usuarios. Aquí se usaron todos los datos de Calorías, Intensidades, METs y Steps.
2. Interpolamos usando statistical matching registros faltantes de sueño y añadimos ruido normal para completar registros inexistentes de algunos usuarios a fin de evitar sesgo y respetar la distribución original. Datos son diarios para el mismo período. Ver nota.
3. Interpolamos datos faltantes de ritmo cardíaco usando fórmula que relaciona METs., basándonos en un método lineal robusto expuesto en [ELSEVIER- Int J Cardiol Heart Vasc.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6003065/). Además añadimos registros inexistentes con la misma fórmula + resampleo aleatorio de los registros existentes para tener datos que respeten la distribución original de los Id's presentes. Ver nota.
4. Desestimamos el uso de los datos de Peso al ser la mayoría subjetivamente ingresados por los usuarios. Esto los hace vulnerables a sesgo o malas mediciones. Además contábamos con muy pocos registros completos (apenas 2 de 19).

Nota: Obviamente entendemos que estos datos son ficticios. En un caso real el problema de la falta de registros debería tratarse con los stakeholders. El único propósito aquí es demostrar habilidad en el uso de métodos estadísticos. El muestreo aleatorio puede llevar a un conjunto de datos de mayor calidad, reduciendo el sesgo que únicamente usando la pequeña proporción incompleta de datos disponibles. Sin embargo, no defendemos aquí que sea una buena práctica hacer este tipo de interpolaciones.

### Data Analysis: 



#### Conclusiones


### Requerimientos
 * Descargar el dataset desde [Kaggle Fit Bit](https://www.kaggle.com/datasets/arashnic/fitbit)
 * Se usó Python 3.10.5 y las siguientes librerías:
 ```
pandas
numpy 
from scipy.stats import trim_mean
matplotlib.pyplot
seaborn
 ````

## English Version