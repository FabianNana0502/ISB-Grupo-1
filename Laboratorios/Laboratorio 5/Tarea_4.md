# LABORATORIO 5: – Uso de ECG mediante BITalino:
## Integrantes
- Fabian Alcides Ñaña Alfaro
- Christian Huarancca Quispe
- Ryoshin Cavero Mosquera
- Flavio Andreas Avendanho Cáceres
- Joao Marco Torres Rivera

<p style="text-align: justify;">
     Este es el desarrollo de la actividad de Laboratorio - Sesión 5 realizada en el laboratorio de prototipado.    
     </p>
     <p style="text-align: justify;">
    Una breve introducción sobre el ECG o EKG...                   
    El electrocardiograma (ECG o EKG) es una prueba rápida utilizada para medir y registrar la actividad eléctrica generada por el corazón, lo cual ayuda a diagnosticar enfermedades cardiovasculares como las arritmias, entre otras[1]. La historia del ECG o EKG remonta desde el siglo XIX cuando el fisiólogo británico Augustus Desiré Waller realiza la primera publicación conocida del uso de un Electrocardiograma. Su idea fue presentada en el Congreso Internacional de Fisiología en Londres en 1887 y fue entonces que inspira al médico fisiólogo Willem Einthoven (1860-1927), quien es reconocido por el triángulo de Einthoven para la obtención de derivaciones cardiacas.En 1901 Einthoven presenta un electrocardiógrafo hecho por un galvanómetro de cuerda extremadamente sensible e introdujo las bases de la telemedicina en 1905 cuando conectó su laboratorio con el Hospital Académico de Leiden a través de una línea telefónica. Otro pionero importante fue Norman Jefferis Holter por el uso de rayos catódicos y cubos de agua salina como electrodos envés de un galvanómetro para su electrocardiograma de 38kg. A pesar del avance acelerado a inicios del siglo XX, en 1954 se realizó la estandarización de las 12 derivaciones empezando así la era moderna en el uso de ECG y diseñando prototipos cada vez más portátiles [2].     
</p>


## Contenido de la sesión
1. [Objetivos](#id1)
2. [Materiales y equipos](#id2)
3. [Metodologia](#id3)
4. [Resultados](#id4)  
   4.1 [Implementación](#id5)  
   4.2 [ECG Estado Basal](#id6)  
   4.3 [ECG Respiracion](#id7)  
   4.4 [ECG Post-respiración](#id8)  
   4.5 [ECG Post-ejercicio](#id9)
   4.6 [Discusión](#id0)  
   4.7 [Conclusión](#id11)

## Objetivos <a name="id1"></a>
* Entendimiento del funcionamiento del BITalino y distribución de electrodos.
* Obtención de señal ECx|G de distintos músculos mediante BITalino.
* Procesamiento y exhibición de la señal mediante OpenSignals.
* Adquirir señales biomédicas de ECG.
* Hacer una correcta configuración de BITalino.
* Extraer la información de las señales ECG del software OpenSignals (R)evolution.

## Materiales y equipos <a name="id2"></a>
* Kit BITalino (r)evolution  
    Kit biomédico que contiene una placa donde vienen conectados los bloques de sensores de manera que permitan trabajar directamente, sin tener que realizar ninguna conexión. Tiene comunicación Bluetooth y conectores UC-E6.  
    ![Bitalino](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSnWHodKjcLUpp2YCgDVI5sHgXSXkNj-763tQ&s)
    
    3 electrodos 

  **Figura 1. BITalino. Fuente: BITalino (r)evolution Home Guide: EXPERIMENTAL GUIDES TO MEET & LEARN YOUR BIOSIGNALS.**
* Laptop o dispositivo móvil para recepción de datos y el software OpenSignals para obtener la señal medida por el sensor ECG. 

## Metodología <a name="id3"></a>
<div align="justify">
Las señales ECG se adquirieron utilizando el sistema BITalino junto con un sensor ECG de tres electrodos, siguiendo el protocolo de la BITalino (r)evolution Lab-Home Guide [3]. Cada miembro del equipo participó en la toma de señales. Los electrodos se colocaron siguiendo la metodología estándar para registros electrcardiográficos, manteniendo una separación mínima de 2 cm entre los electrodos activos y ubicando el electrodo de referencia en un área ósea apropiada para minimizar el ruido. 

El corazón fue analizado en 3 situaciones:
<ul>
  <li>Reposo</li>
  <li>Manteniendo respiración</li>
  <li>Post Respiración</li>
  <li>Post Ejercicio</li>
</ul>

### Proceso de Filtrado
El procesamiento de las señales ECG incluyó un proceso de filtrado en dos etapas: 

El primer paso del filtrado fue la aplicación de un filtro notch centrado en 60 Hz. Este filtro se utilizó para eliminar la interferencia de la red eléctrica, una fuente común de ruido en los registros ECG, EEG y EMG. Al eliminar estas frecuencias no deseadas, se obtuvo una señal más limpia, lo que es crucial para un análisis fiable de la actividad cardiaca.

El segundo paso fue aplicar un filtro pasa banda con un rango de 0.5 a 100 Hz. Este rango de frecuencia fue elegido para aislar las frecuencias relevantes de la señal ECG, eliminando los ajenos a la señal ECG . Este procedimiento es esencial para mejorar la calidad de la señal y su representatividad de ella. Por ejemplo, Lorenzo (2015) [4] emplearon un filtrado similar (0.5-100 Hz) en su estudio para obtener señales ECG claras y útiles.


</div>

### Código usado en Python
<a name="id11"></a>
<div align="justify">

El código realiza un completo proceso de adquisición, preprocesamiento y visualización de señales electrocardiográficas (ECG) capturadas utilizando el sistema BITalino y OpenSignals. A continuación, se detallan los procesos de filtrado que se llevaron a cabo en el código:

1. **Conversión de la Señal ADC a Milivoltios (mV):**
   - La función `ADCtomV` se utiliza para convertir la señal digital (ADC) a analógico (voltaje en milivoltios (mV)). Dado que la señal se obtiene en formato ADC, es necesario convertirla a un formato más comprensible (milivoltios) para analizar la amplitud real de la señal ECG. Esta conversión es crucial para asegurar que los datos se interpreten correctamente y se puedan comparar con otros estudios electromiográficos.

2. **Remoción del Componente DC:**
   - Antes de aplicar cualquier filtrado, se realiza una remoción del componente DC de la señal. Este paso implica restar el valor promedio de la señal para eliminar cualquier desplazamiento en el nivel base. La presencia de un componente DC podría distorsionar los resultados del filtrado posterior y la representación de la señal. La remoción del componente DC garantiza que las etapas de filtrado subsiguientes no se vean afectadas por un nivel base elevado.

3. **Filtrado Pasa Banda (Bandpass Filter):**
   - Se aplica un filtro pasa banda utilizando la función `butter_bandpass_filter`, que emplea el diseño de filtros Butterworth. Este filtro se diseña con una frecuencia de corte inferior de 0.5 Hz y una frecuencia de corte superior de 100 Hz. El objetivo de este filtrado es eliminar las componentes de baja frecuencia (como el ruido de movimiento y la línea base) y las componentes de alta frecuencia que suelen estar asociadas con el ruido eléctrico o artefactos no deseados. El uso del filtro pasa banda permite aislar las frecuencias relevantes de la señal ECG que se encuentran dentro de este rango, proporcionando una representación más precisa de la actividad cardiaca.

4. **Filtrado Notch para Eliminar Ruido de Red Eléctrica:**
   - Después del filtrado pasa banda, se aplica un filtro notch (rechaza bandas específicas de frecuencias) centrado en 60 Hz mediante la función `iirnotch`. La frecuencia de 60 Hz corresponde al ruido de la red eléctrica que es común en los registros electromiográficos. Este ruido puede interferir con la señal ECG, haciendo que sea difícil distinguir las características de la señal. El filtro notch elimina este ruido específico, resultando en una señal más limpia y más representativa de la actividad cardiaca verdadera.

5. **Visualización de la Señal:**
   - Una vez que la señal ha sido filtrada, se procede a su visualización en tres subgráficos:
     - **Señal Original:** Muestra la señal convertida a mV antes del filtrado.
     - **Señal Filtrada:** Muestra la señal después de los procesos de filtrado (pasa banda y notch), destacando cómo el filtrado mejora la claridad de la señal ECG.


## Resultados <a name="id4"></a>

La toma de señales comenzó con la captura de señales de cada miembro del equipo, siguiendo el protocolo BITalino (r)evolution Lab-Home Guide [2].  

## Implementación <a name="id5"></a>
***
### Click on images to visualize the videos
***
|  **Biceps en reposo**  | **Biceps en movimiento** | **Biceps con contrafuerza** |
|:------------:|:---------------:|:------------:|
|[![Watch the video](http://img.youtube.com/vi/BjMBCKBP3xE/0.jpg)](https://youtu.be/BjMBCKBP3xE)|[![Watch the video](http://img.youtube.com/vi/j_i9HvOEN8w/0.jpg)](https://youtu.be/j_i9HvOEN8w)|![Watch the video](http://img.youtube.com/vi/Mb9VrSXVKTM/0.jpg)(https://youtu.be/Mb9VrSXVKTM)||
***
|  **Triceps en reposo**  | **Triceps en movimiento** | **Triceps con contrafuerza** |
|:------------:|:---------------:|:------------:|
|[![Watch the video](http://img.youtube.com/vi/fyPeHq-zefs/0.jpg)](https://youtu.be/fyPeHq-zefs)|[![Watch the video](http://img.youtube.com/vi/374YDF8yrnQ/0.jpg)](https://youtu.be/374YDF8yrnQ)|![Watch the video](http://img.youtube.com/vi/mZ9duE8vDmA/0.jpg)(https://youtu.be/mZ9duE8vDmA)||
***
|  **Gastrocnemio en reposo**  | **Gastrocnemio en movimiento** | **Gastrocnemio con contrafuerza** |
|:------------:|:---------------:|:------------:|
|[![Watch the video](http://img.youtube.com/vi/_bWFJmUXq3E/0.jpg)](https://youtu.be/_bWFJmUXq3E)|[![Watch the video](http://img.youtube.com/vi/d3edzegH4jY/0.jpg)](https://youtu.be/d3edzegH4jY)|![Watch the video](http://img.youtube.com/vi/E-TUcp1oyTM/0.jpg)(https://youtu.be/E-TUcp1oyTM)||

*** 
|![BITalino con electrodos](https://github.com/angiet04/Intro_se-ales06/blob/648e8eb0ea78ae11fa8690847565b76faea9742d/Im%C3%A1genes/Laboratorio_3/BITalino.jpeg)|  
**Figura 2. BITalino con electrodos.**

Se siguió el protocolo de conexión y posicionamiento de los electrodos para el sensor ECG:
* Conexión del sensor de ECG
* Colocación de los electrodos
* Posicionamiento del sensor ECG 
* Inicio de la grabación en el software OpenSignals (R)evolution
* Fin de la grabación

## ECG Estado Basal <a name="id6"></a>

| **ECG Estado Basal 1** | 
|![Bíceps en reposo](./Ploteos/RBICEPS.png)| 
| **ECG Estado Basal 2** |
|![Bíceps en movimiento](./Ploteos/MBICEPS.png)| 
| **ECG Estado Basal 3** |
|![Bíceps con contrafuerza](./Ploteos/CFBICEPS.png)| 

## ECG Respiración <a name="id7"></a>

| **ECG Respiración 1** | 
|![Tríceps en reposo](./Ploteos/RTRICEPS.png)| 
| **ECG Respiración 2** |
|![Tríceps en movimiento](./Ploteos/MTRICEPS.png)| 
| **ECG Respiración 3** |
|![Tríceps con contrafuerza](./Ploteos/CFTRICEPS.png)| 

## ECG Post-respiración <a name="id8"></a>

| **ECG Post-respiración 1** | 
|![Gastrocnemios en reposo](./Ploteos/RGASTROCNEMIO.png)| 
| **ECG Post-respiración 2** |
|![Gastrocnemios en movimiento](./Ploteos/MGASTROCNEMIO.png)| 
| **ECG Post-respiración 3** |
|![Gastrocnemios con contrafuerza](./Ploteos/CFGASTROCNEMIO.png)| 

## ECG Post-ejercicio <a name="id9"></a>

| **ECG Post-ejercicio 1** | 

| **ECG Post-ejercicio 2** | 

| **ECG Post-ejercicio 3** | 



## Discusión <a name="id10"></a>

### Discusión de Estado Basal 
<div align="justify">

ESCRIBA SU DISCUSION

### Discusión de aguantar respiración

ESCRIBA SU DISCUSION

### Discusión post-respiración

ESCRIBA SU DISCUSION

### Discusión post-ejercicio

ESCRIBA SU DISCUSION

## Conclusión <a name="id11"></a>
<div align="justify">

...

</div>



## Bibliografía

[1] https://www.mayoclinic.org/es/tests-procedures/ekg/about/pac-20384983 
[2] https://scielo.isciii.es/scielo.php?script=sci_arttext&pid=S2695-50752022000400011
[3]BITalino (r)evolution Home Guide: EXPERIMENTAL GUIDES TO MEET & LEARN YOUR BIOSIGNALS. Disponible en: [link](https://support.pluxbiosignals.com/wp-content/uploads/2022/04/HomeGuide0_GettingStarted.pdf)\
[4] https://openaccess.uoc.edu/bitstream/10609/40186/6/jlorenzoroTFC0115memoria.pdf

[3]I. Zunaidi, W. Zulkarnain, T. I., & A. B. Shahriman, "Pattern Behavior of Electromyography Signal During Arm Movements," TATI University College, Malaysia, 2008.\
[4] Zhang, Q., Hosoda, R., & Venture, G. "Human joint motion estimation for electromyography (s)-based dynamic motion control." Proceedings of the Annual International Conference of the IEEE Engineering in Medicine and Biology Society, Osaka, Japan, pp. 21-24, 2013, doi: 10.1109/EMBC.2013.6609427​.\
[5]Varela-Benítez, J.L., Rivera-Delgado, J.O., Espina-Hernández, J.H., & de la Rosa-Vázquez, J.M. "Electrodo Capacitivo de Alta Sensibilidad para la Detección de Biopotenciales Eléctricos." Revista Mexicana de Ingeniería Biomédica, vol. 36, no. 2, pp. 131-142, May 2015. DOI: 10.17488/RMIB.36.2.1​.\
[6] Shankhwar, V., Singh, D., & Deepak, K. K. "Effect of Novel Designed Bodygear on Gastrocnemius and Soleus Muscles during Stepping in Human Body." Microgravity Science and Technology, vol. 33, pp. 29, 2021, doi: 10.1007/s12217-021-09855-y .\
[7] Martinez-Valdes, E., Farina, D., Negro, F., & Falla, D. "Early motor unit conduction velocity changes to high-intensity interval training versus continuous training." Medicine & Science in Sports & Exercise, vol. 49, no. 6, pp. 1126-1136, 2017, doi: 10.1249/MSS.0000000000001210.

