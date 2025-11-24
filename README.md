# **Remote Photoplethysmography (rPPG)**

El proyecto consiste en la obtención de datos haciendo uso de la visión por computador, creando un modelo de **rPPG (Fotoplestimografía Remota)** en donde se extraerán los siguientes datos a partir de un video o en tiempo real de la cara de una persona:

1. **Frecuencia Cardíaca (Heart Rate - HR):** Es el dato base a obtener y el objetivo principal del proyecto.
    - **¿Qué es?:** El número de contracciones del corazón por minuto (*BPM*).

    - **¿Cómo obtenerlo?:** Calculando la frecuencia dominante del espectro de Fourier (**FFT**) de la señal rPPG.

    - **Precisión esperada:** Muy alta. Con una buena iluminación ha de tener un error menor a ±3 *BPM* comparado con un reloj inteligente o un pulsioxímetro de dedo.

    - **Algoritmo:** Transformada rápida de Fourier (**FFT**) haciendo uso de las librerías `spicy.fftpack.fft`o `numpy.fft`.

    - **Proceso:** Se convierte la señal temporal al espectro de frecuencias. Se identifica el pico de máxima potencia (*Peack Frequency*) dentro del rango 0.7-0.4 Hz:

$$
\text{BPM} = f_{\text{max}} \times 60
$$


2. **Variabilidad de la Frecuencia Cardíaca (HRV):** Este es el parámetro que saca la proporción de cambio de la frecuencia cardíaca.

    - **¿Qué es?:** En el corazón el tiempo de latido va variando con el tiempo (ej. 0.8s, 0.85s, 0.79s, etc). La HRV mide dicha variación.

    - **¿Qué indica?:** Es un indicador directo de **estrés y salud del sistema nervioso autónomo**.
        - HRV Alta = Relajado / Recuperado.
        - HRV Baja = Estrés / Fatiga / Ansiedad.

    - **¿Cómo obtenerlo?:** Se han de detectar los "picos" de la onda con mucha precisión (*Peak Detection*) en el dominio del tiempo, no solo la frecuencia promedio. En caso de cámaras con pocos FPS (ej. 30) se tendrá que usar interpolación para mejorar la resolución temporal.

    - **Algoritmo:** Detección de picos usando las librerías `spicy.signal.find_peaks`.

    - **Proceso:** Se localizan los máximos locales de la señal filtrada que corresponden a los latidos sistólicos. Se calculan los intervalos entre latidos (*NN intervals*) y se deriva la métrica **SDNN** (Desviación estándar de los intervalos NN).

3. **Frecuencia Respiratoria (Resporatory Rate - RR):**

    - **¿Qué es?:** Son las respiraciones por minuto (*RPM*).

    - **¿Cómo se obtiene?:** Hay dos fenómenos que permiten medir el dato a partir de la cara:
        - **RSA (Arritmia Sinusal Respiratoria):** El corazón se acelera ligeramente cuando se inspira y se frena cuando se espira. Analizando las fluctuaciones lentas de la señal del pulso, se puede obtener la respiración.
        - **Movimiento:** Usando visión artificial se puede detectar el sutil movimiento cíclico de los hombros o el pecho si entran en el encuadre.
    
    - **Algoritmo:** Uso de **_Downsamplig_ (Re-muestreo)** y un filtrado de banda específica que a diferencia del pulso, si interesan las frecuencias muy bajas. Librería `spicy.signal.resample`.

    - **Proceso:** Se usa un filtro pasa-banda (*Butterworh* de orden 2) con un corte inferior de 0.1 Hz (6 respiraciones/min) y un corte superior de 0.5 Hz (30 respiraciones/min), se extrae la señal respiratoria aislando la "envolvente" de la señal PPG filtrada (o analizar la variación de picos) y se estima el **RPM** sobre la nueva señal de baja frecuencia:

$$
\text{RPM} = \text{Frecuencia Dominante Baja} \times 60
$$

4. **Saturación de Oxígeno ($SpO_2$):** Es con diferencia el parámetro más complicado de obtener y puede ser el que se termine descartando.

    - **¿Qué es?:** Es el porcentaje de hemoglobina que transporta oxígeno.

    - **Problema:** Los oxímetros reales usan luz infraroja o luz roja y la webcam y cámaras comúnes solo tienen roja, azul y verde.

    - **Proceso:** Se aplica una regresión lineal empírica para estimar el porcentaje. Y luego, usando la fórmula empírica para estimar porcentaje ($SpO_2 = A - B \times Ratio$).

    - **Importante:** Sin calibración este valor no será más que una estimación relativa.

    - **Solución aproximada:** Utilizando el "ratio de ratios" se puede comparar la absorción del canal rojo vs la del canal azul o verde:
$$
\text{Ratio} = \frac{AC_{\text{red}} / DC_{\text{red}}}{AC_{\text{blue}} / DC_{\text{blue}}}
$$

