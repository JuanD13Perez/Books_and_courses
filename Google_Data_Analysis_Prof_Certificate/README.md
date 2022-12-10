# Google Data Analysis Professional Capstone Project
- [English Version](#English-Version) 
- [Spanish Version](#Spanish-Version)

## English Version

### **Business problem** 📝
This project is part of Google's Professional Certificate in Data Analysis.
We use the dataset of the _FitBit_ brand, available via [**Kaggle**](https://www.kaggle.com/datasets/arashnic/fitbit), which correspond to records of a device (Smartwatch) with consumption information of calories, number of steps, etc.
In this case, we're assuming we work for Bellabeat, a competitor of FitBit. Our task is to **identify consumption patterns in users of FitBit products** that can support the company's Marketing strategy.

We decided to use Python instead of R to practice Python skills. For the analysis and cleaning we rely on strategies and concepts of the course. The markdown comments that accompany the notebook are in Spanish and the code cells in English.

| ![Banner](FitBit_Banner.png) |
|:--:|
| <b>Image Credits - Banner - https://www.kaggle.com/datasets/arashnic/fitbit  </b>|


### **Highlights** 
This project was made using python 3.10.5 in a Jupyter Notebooks environment. It consists of three main sections:

- [**Appendix**](#Appendix)
- [**Data Cleaning EN**](#Data-Cleaning-EN)
- [**Data Analysis EN**](#Data-Analysis-EN)

### Appendix
Here we manipulate from O. System the [dataset](https://www.kaggle.com/datasets/arashnic/fitbit) in order to get all the csv files as pandas dataframes. We realized that the data structure is similar to that of a relational database and that certain dataframes are merely minute-by-minute aggregations of the data. We decided to work only with these 'root' dataframes.

We have information per minute of:
* Calories
* Exercise intensity
* METs
* Number of steps
* Heart rate

Daily information from:
* Sleep
* Weight

### Data Cleaning EN:

0. We restrict 19 users out of the 25 originally available. Since 6 users had very incomplete information (they did not cover a month of data).
1. We restrict the date range from 2016-04-12 to 2016-05-11 minute by minute. This to have comparable time information for different users. Here all Calories, Intensities, METs and Steps data was used.
2. We interpolated using statistical matching missing sleep records and added normal noise to complete missing sleep records from some users to avoid bias and respect the original distribution. Daily data here!... for the same month. See note.
3. We interpolated missing heart rate data using a formula that relates METs, based on a robust linear method exposed in [ELSEVIER- Int J Cardiol Heart Vasc.](https://www.ncbi.nlm.nih.gov/pmc/ articles/PMC6003065/). In addition, we add non-existent records with the same formula + random resampling of the existing records to have data that respects the original distribution of the Id's present. See note.
4. We dismiss the use of Weight data as most of it is subjectively entered by users. This makes them vulnerable to bias or mismeasurement. In addition, we had very few complete records (just 2 of 19).

Note: Obviously we understand that these data are fictitious. In a real case the problem of the lack of records should be dealt with the stakeholders. The sole purpose here is to demonstrate proficiency in the use of statistical tools. Random sampling can lead to a higher quality data set, reducing bias than just using the small incomplete proportion of available data. However, we do not advocate here that it is good practice to do this type of interpolation.

### Data Analysis EN:
We try to identify patterns in device usage on 3 fronts:
* A) Exercise
* B) Sleep
* C) Time of use

#### A) Exercise
We were able to identify the hours of greatest caloric consumption, using aggregations with different statistics:

![Calories-Hour](Calories_hour.png)

This same pattern is verified with other variables:
![Diff_hours](Different_hour.png)

However, when looking at the trimmed mean we see that the values ​​are closer to each other. Indicating that the greater activity in the afternoon is due to high values ​​for some users at those hours.

Given the above, we decided to classify users according to their most active hours during the month.

![class_hours](Clas_horas.png)

- **21%** of users are **more active in the morning** (i.e. before 11:00 a.m.)
- **31%** are **mixed**. That is, they have at least one active hour before 11 but no more than 2.
- The majority, **42%** prefer the afternoon hours.

Other discoveries, 5:00 and 9:00 are the preferred times for morning users. From 5:00 p.m. to 7:00 p.m., afternoon users tend to be more active. Mixed users rather do not have a preferred time to be active and tend to be more sedentary.

Regarding the days of the week, we did not find particular patterns, only:
* Sunday is the least active day.
* Curiously, morning users prefer to be more active on Monday, Tuesday and Wednesday.

Now, that a person is more active, that is to say that they consume more calories, does not mean that they are doing an exercise routine.
That's why we calculated the streaks of consecutive minutes that people had an intensity of 2 or 3 (medium to highly active). We consider 30-minute bouts as one exercise session. These were our discoveries:

![ex_per_dow](ex_per_dow.png)

* Clearly, since there are more users in the afternoon, the hours of more exercise were in the afternoon. We confirm that the preferred hours to exercise in the afternoon are from 17:00 to 19:00.
* The days that users prefer to exercise are the first days of the work week. They exercise less on Fridays and Sundays.

Researching this aspect further, we decided to classify people according to the number of sessions of more than 30 minutes that they had during the month (30-day period).
* People with 15 or more sessions were classified as athletic.
* Between 8 and 14 as normal.
* Less than or equal to 7 as sedentary.

![ex_people](ex_people.png)
The middle graph line indicates the threshold of 15 sessions in a month that a person must pass to be considered athletic. There was someone with 60 sessions.

We discovered that:
* **31%** of users exercise constantly. They are athletic.
* **63% are sedentary**.
* We validate our hypothesis that the favorite hours to be active for morning users are 5:00 and 9:00, since these hours were also those with the highest number of exercise sessions. It also indicates that there are super early risers and mid-morning people within that **21%** group of users.

Investigating the exercise patterns further, we asked ourselves: **Do they prefer long sessions of exercise or short sessions**? this could be an indicator of whether someone prefers to do cardio, (marathon type training) or prefers to exercise with weights.

For example, it is clear that the following users have different patterns:
![example_long](ejemplo_largas.png)

The one on the left prefers long sessions and the one on the right medium sessions. So we decided to further classify athletic people:

* A percentage of at least 20% of sessions of more than 80 minutes as preference to long sessions.
* A percentage of at least 40% of sessions of more than 50 minutes in preference to medium sessions.
* A percentage greater than 60% of sessions of less than 50 minutes, preferably short sessions.

These were our discoveries:
![sessions_pref](pref_sesiones.png)

* Within athletic users **66%** prefer short sessions.

#### Conclusions A)

We have been able to identify users with all kinds of preferences: from more activity in the mornings or in the evenings, to the preferred durations for exercising. In general terms, 63% of users are sedentary and among those who exercise, the preference is for sessions in the evening of no more than 50 minutes and preferably on Monday, Tuesday and Wednesday. Sundays are the most sedentary days for all users.

#### B) Sleep

Here we group some variables of interest daily:

* The total daily sum of: METs, Steps, Heart Rate and Calories.
* Average daily intensity.
* The longest exercise minute string per day.


We did not find any interesting relationship between sleep time and any of the groups we have identified. Neither on the athletic or sedentary, nor on the most active people in the morning or on the most active people in the afternoon. In all cases, the Pearson correlation was less than 0.3 in absolute value with respect to each of the other variables of interest.

On the other hand, regarding the difference between Time in bed and sleep time, we did notice significant differences:

![time_needed_sleep](time_needed_sleep.png)

We found that **Both sedentary and active people take longer to go to sleep in the afternoon than athletic people and people who are most physically active in the morning**

We can say that for this particular sample, one of the benefits of the product is that it helps to improve sleep habits when combined with physical activity.

It is recommended to have 8 hours of sleep. In other words, 480 minutes of sleep a day. We are going to count every day that users exceed that mark.

![over8_count](over8_count.png)

We now see that the previous results may be biased by the fact that athletic people do not usually sleep more than 8 hours. Or it could also be because during the night people take off their smartwatch because it bothers them when they sleep. That serves as an introduction to the next question: How much do users use their devices?

#### C) Device use
In this section we will consider long chains where the heart rate marks a value of zero. This will mean that the person removed the device. We consider a person to have removed their device in a given hour if at any point in that hour their **heart rate was 0 for at least 15 consecutive minutes**.

![Not_use](Not_use.png)

We have discovered that:

* **20%** of the time in the morning and evening hours users simply did not use their device.
* **2 of 6 athletic users** had device usage **less than 60% of all possible hours**. These users may only use the device for their training sessions. In particular, the user who prefers long sessions, used his device only 60% of the time.
* We confirm our hypothesis that athletic people also tend to remove the device at night.
* It is evident that 5 of the 19 users, that is to say **26% do not use the device 60% of the time.**

### Final Recommendations - Act

Some recommendations for the marketing strategy.

- 1) **One in four competitive users does not get to use the device even 50% of the time**. This information can be used for advertising strategies. An example of a similar strategy occurred in [**Super Bowl XV 1981 at Schlitz vs Michelob**](https://www.youtube.com/watch?v=P9a3K2vkvrU), for an explanation see Ch. 5 of Naked Statistics by Charles Wheelan.
- 2) There is a clear separation between sedentary users and athletic users. **33% of users are athletic**. These users prefer the afternoon to exercise, particularly between 17:00 and 19:00. Although **20% also prefer to exercise in the morning**, around 5:00 or 9:00. This indicates that if the target audience is people who exercise, the campaigns can be done in the afternoon. For example, at 17:00 in a gym. We have also noticed that most users prefer to exercise in routines of no more than 50 minutes. Only **16% prefer long sessions of exercise**, for example marathon runners. Different devices can be adjusted with specialized metrics for short sessions (for example, weights) and other metrics for runners.
- 3) For sedentary users the activity is rather constant and low caloric consumption throughout the day. We have noticed that **20% of the nights** users tend to remove the device. This may indicate discomfort or that it is necessary to carry it at night. We have also noticed that sedentary users tend to spend more time in bed and take longer to go to sleep. The development of a complementary product, for example a ring or bracelet that is more comfortable and tracks sleep is a possibility for this target audience. Emphasis can be placed on the sleep benefits of using a BellaBeat device. Reports or brief reports with sleep recommendations and other quality of life metrics can be sent to all users, emphasizing the comfort of BellaBeat products versus those of the competition.
- 4) Clearly all of the above conclusions are subject to a very limited set of data. It is necessary to collect data from more users and over a longer period of time to identify temporary trends in usage. Likewise, sales and e-commerce data can complement this information to carry out stratified statistical tests, (for example, A/B testing), which allow the identification of the best product for each sector. A variable indicating whether the device is in use (in this case we use a heart rate of 0) or is charging minute by minute is very useful to further segment the analysis. Likewise, a variable that indicates the type of exercise that the person initiates makes it possible to identify more patterns. With geolocation data, weight, age we could do even more. In general it is recommended to improve the design of the experiment by collecting the above information.

#### Acknowledgments:
* Googgle and coursera :)


## Spanish Version

### **Problema de negocio** 📝
Este proyecto hace parte del certificado profesional de Google en análisis de datos.
Usamos el dataset de la marca _FitBit_, disponible vía [**Kaggle**](https://www.kaggle.com/datasets/arashnic/fitbit), que corresponden a registros de un dispositvo (Smartwatch) con información de consumo de calorías, número de pasos, etc.
En este caso, asumimos que trabajamos para Bellabeat, una competidora de FitBit. Nuestra tarea consiste en **identificar patrones de consumo en los usuarios de los productos FitBit** que puedan apoyar la estrategia de Marketing de la compañía.

Decidimos usar Python en vez de R para prácticar habilidades en este idioma. Para el análisis y limpieza nos basamos en estrategias y conceptos del curso. Los comentarios markdown que acompañan el notebook están en español y las celdas de código en inglés.

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

Nota: Obviamente entendemos que estos datos son ficticios. En un caso real el problema de la falta de registros debería tratarse con los stakeholders. El único propósito aquí es demostrar habilidad en el uso de herramientas estadísticas. El muestreo aleatorio puede llevar a un conjunto de datos de mayor calidad, reduciendo el sesgo que únicamente usando la pequeña proporción incompleta de datos disponibles. Sin embargo, no defendemos aquí que sea una buena práctica hacer este tipo de interpolaciones.

### Data Analysis: 
Intentamos identificar patrones en el uso del dispositivo en 3 frentes:
* A) Ejercicio
* B) Sueño
* C) Tiempo de uso

#### A) Ejercicio
Logramos identificar las horas de mayor consumo calórico, usando agregaciones con diferentes estadísticos:

![Calorias-Hora](Calories_hour.png)

Este mismo patrón se comprueba con otras variables:
![Dif_horas](Different_hour.png)

Sin embargo, al observar la media truncada vemos que los valores son más cercanos entre sí. Indicando que la mayor actividad en la tarde se debe a altos valores para algunos usarios a dichas horas.

Dado lo anterior, decidimos clasificar a los usuarios de acuerdo a sus horas más activas durante el mes.

![clas_horas](Clas_horas.png)

- El **21%** de los usarios son **más activos en la mañana** (i.e. antes de las 11:00 hrs.)
- **31%** son **mixtos**. Es decir tienen al menos una hora activa antes de las 11 pero no más de 2.
- La mayoría, el **42%** prefieren las horas de la tarde.

Otros descubrimeintos, las 5:00 y las 9:00 son las horas preferidas para los usarios de la mañana. Desde las 17:00 hasta las 19:00 los usarios de la tarde suelen ser más activos. Los usuarios mixtos más bien no tienen una hora preferida para estar activos y suelen ser más sedentarios.

En cuanto a los días de la semana no encontramos patrones particulares, solamente:
* El domingo es el día menos activo. 
* Curiosamente los usuarios de la mañana prefieren estar más activos lunes, martes y miércoles.

Ahora bien , que una persona esté más activa, es decir que consuma más calorías, no significa que esté haciendo una rutina de ejercicio.
Por eso calculamos las rachas de minutos consecutivos que las personasn tenían una intensidad de 2 o 3 (medianamente a altamente axctivos). Consideramos rachas de 30 minutos como una sesión de ejercicio. Estos fueron nuestros descubrimientos:

![ex_per_dow](ex_per_dow.png)

* Claramente, dado que hay más usuarios de la tarde, las horas de más ejercicio fueron las de la tarde. Confirmamos que las horas preferidas para ejercitarse en la tarde son desde las 17:00 hasta las 19:00.
* Los días que los usarios prefieren hacer ejericio son los primeros días de la semana de trabajo. Se ejercitan menos los viernes y domingos.

Invesitgano aún más este aspecto decidimos clasificar a las personas de acuerdo al número de sesiones de más de 30 minutos que tuvieron durante el mes (período de 30 días). 
* Personas con 15 o más sesiones fueron clasificadas como atléticas.
* Entre 8 y 14 como normal.
* Menor o igual a 7 como sedentarios.

![ex_people](ex_people.png)
La línea de la gráfica del medio indica el umbral de 15 sesiones en un mes que debe superar una persona para considerarse atlética. Hubo alguine con 60 sesiones.

Descubrimos que:
* El **31%** de los usarios hacen ejercicio constantemente. Son atléticos.
* El **63% son sedentarios**.
* Validamos nuestra hipótesis de que las horas favoritas para tener actividad para los ususarios de la mañana son las 5:00 y las 9:00, pues éstas horas fueron también las de mayor número de sesiones de ejercicio. También indica que hay súper madrugadores y personas de media mañana dentro de ese grupo del **21%** de usuarios.

Invesitgando aún más los patrones de ejercicio, nos preguntamos: **¿Prefieren sesiones largas de ejercicio o sesiones cortas**? esto podría ser un indicador de si alguien prefiere hacer cardio, (tipo entrenamiento para maratón) o prefiere hacer ejercicio de pesas.

Por ejemplo, es claro que los siguientes usarios tienen patrones diferentes:
![ejemplo_largas](ejemplo_largas.png)

El de la izquierda prefiere sesiones largas y el de la derecha sesiones medias. Así pues decidimos clasificar aún más a las personas atléticas:

* Un porcentaje de al menos 20% de sesiones de más de 80 minutos como preferencia a sesiones largas.
* Un porcentaje de al menos 40% de sesiones de más de 50 minutos como preferencia a sesiones medias.
* Un porcentaje superior al 60% de sesiones inferiores a 50 minutos como preferencia sesiones cortas.

Estos fueron nuestros descubrimientos:
![pref_sesiones](pref_sesiones.png)

* Dentro de los usarios atléticos el **66%** prefiere sesiones cortas.

#### Conclusiones A)

Hemos podido identificar usuarios con todo tipo de preferencias: desde mayor actividad en las mañanas o en las tardes, hasta las duraciones preferidas para hacer ejercicio. En términos generales el 63% de los usarios son sedentarios y entre los que se ejercitan la preferencia es hacia sesiones en la tarde de no más de 50 minutos y preferiblemente los días lunes, martes y miércoles. Los domingos son los días más sedentarios para todos los usarios.

#### B) Sueño

Aquí agrupamos diariamente algunas variables de interés:

* La suma total diaria de: METs, Pasos, Ritmo Cardíaco y Calorías.
* El promedio de la intensidad diario.
* La cadena de minutos de ejericio más larga por día.


No encontramos ninguna relación interesante entre el tiempo de sueño y ninguno de los grupos que hemos identificado. Ni sobre los atléticos o sendentarios, ni sobre las personas más activas en la mañana o sobre las personas más activas en la tarde. En todos las casos la correlación de Pearson fue inferior en valor absoluto a 0.3 con respecto a cada una de las otras variables de interés.

Por otra parte, en cuánto a la diferencia de Tiempo en cama y tiempo de sueño sí notamos diferencias significativas:

![time_needed_sleep](time_needed_sleep.png)

Descubrimos que **Tanto las personas sedentarias como las personas activas en la tarde les toma más tiempo irse a dormir que a las personas atléticas y las personas que realizan la mayor actividad física en las mañanas**

Podemos decir, que para esta muestra particular uno de los beneficios del producto es que ayuda a mejorar los hábitos del sueño cunado se combina con actividad física.

Lo recomendable es tener 8 horas de sueño. Es decir 480 minutos de sueño díarios. Vamos a realizar un conteo de todos los días que los usuarios superan dicha marca.

![over8_count](over8_count.png)

Ahora vemos que los resultados anteriores pueden estar sesgados por el hecho de que las personas atléticas no suelen dormir más de 8 horas. O también puede deberse a que durante la noche las personas se quitan su reloj porque les molesta al dormir. Eso nos sirve de introducción hacia la siguiente pregunta: ¿Qué tanto usan los usuarios sus dispostivos?

#### C) Uso dispostivo
En esta sección consideraremos las cadenas largas donde el ritmo cardiaco marca un valor de cero. Esto significará que la persona se quitó el dispositivo. Consideramos que una persona se quitó el dispositivo en una hora dada si en algún momento de dicha hora su **ritmo cardíaco fue de 0 por al menos 15 minutos consecutivos**.

![Not_use](Not_use.png)

Hemos descubierto que:

* **20%** del tiempo en horas de la noche y de la mañana los usuarios simplemente no usaron su dispositivo.
* **2 de 6 usuarios atléticos** tuvieron un uso del dispositvo **inferior al 60% de todas las horas posible**. Es posible que éstos usuarios solo usen el dispositivo para sus sesiones de entrenamiento. En particular el usuario de preferencia de sesiones largas, usó su dispositvo solamente el 60% del tiempo.
* Confirmamos nuestra hipótesis de que las personas atléticas también tienden a quitarse el dispositivo de noche.
* Es evidente que 5 de los 19 usuarios, es decir **un 26% no llegan a usar el dispositvo un 60% del tiempo.**

### Recomendaciones Finales - Act

Algunas recomendaciones para la estrategia de marketing.

- 1) **Uno de cada cuatro usuarios de la competencia no llega a usar el dispositivo ni el 50% del tiempo**. Esta información puede servir para estrategias publicitarias. Un ejemplo de una estrategia similar ocurrió en el [**Super Bowl XV 1981 en Schlitz vs Michelob**](https://www.youtube.com/watch?v=P9a3K2vkvrU), para una explicación ver Ch. 5 de Naked Statistics de Charles Wheelan.
- 2) Hay una clara separación entre usuarios sedentarios y usuarios atléticos. **Un 33% de los usuarios son atléticos**. Dichos usuarios prefieren la tarde para ejercitarse, en particular entre las 17:00 y las 19:00. Aunque también un **20% prefiere ejercitarse en la mañana**, alrededor de las 5:00 o las 9:00. Esto indica que si el público objetivo son personas que hacen ejercicio, las campañas se pueden hacer sobre horas de la tarde. Por ejemplo, a las 17:00 en un gimnasio. También hemos notado que la mayoría de usuarios prefieren ejercitarse en rutinas de no más de 50 minutos. Solo un **16% prefiere sesiones largas de ejercicio**, por ejemplo los maratonistas. Se pueden ajustar dispositvos diferentes con métricas especializadas en sesiones cortas (por ejemplo pesas) y otras métricas para corredores. 
- 3) Para los usuarios sedentarios la actividad es más bien constante y de bajo consumo calórico a lo largo del día. Hemos notado que en un **20% de las noches los usuarios** suelen removerse el dispositvo. Esto puede indicar incomodidad o que es necesario cargarlo en las noches. También hemos notado que los usuarios sedentarios suelen pasar más tiempo en cama y tardan más en irse a dormir. El desarrollo de un producto complementario, por ejemplo un anillo o manilla que sea más cómodo y rastree el sueño es una posibilidad para este público objetivo. Se puede hacer énfasis en los beneficios del sueño al usar un dispositivo de BellaBeat. Se pueden enviar informes o reportes breves con recomendaciones del sueño y otras métricas de calidad de vida para todos los usuarios, haciendo énfasis en la comodidad de los productos BellaBeat versus los de la competencia.
- 4) Claramente todas las conclusiones anteriores están sujetas a un conjunto muy limitado de datos. Es necesario recopilar datos de más usuarios y por un período más largo de tiempo para identificar tendencias temporales en el uso. Asimismo, datos de ventas y e-commerce pueden complementar ésta información para realizar test estadísticos estratificados, (por ejemplo A/B testing), que permitan identificar el mejor producto para cada sector. Una variable que indique si el dispositivo está en uso (en este caso usamos un ritmo cardíco de 0) o está cargandose minuto a minuto es muy útil para segmentar aún más el análisis. Asimismo, una variable que indique el tipo de ejercicio que inicia la persona permite identificar más patrones. Con datos de geolocalización, peso, edad podríamos hacer aún más. En general se recomienda mejorar el diseño del experimento recopilando la información anterior.

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

