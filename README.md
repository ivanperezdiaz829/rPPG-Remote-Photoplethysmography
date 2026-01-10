# **Fotoplestimografía Remota (rPPG)**

## Extracción de signos vitales en tiempo real mediante Visión por Computador

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-green?logo=opencv&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Face%20Mesh-orange)
![Status](https://img.shields.io/badge/Status-Validado-success)

## Documentación Completa
Para una explicación matemática detallada, fundamentos físicos y análisis de resultados, consulta la memoria técnica del proyecto:

### [👉 **Leer Informe Técnico (PDF)**](Remote_Photoplethysmography.pdf)
*(Haz clic arriba para ver el documento LaTeX compilado)*

---

## ¿En qué consiste?
Este proyecto implementa un sistema de **rPPG (Remote Photoplethysmography)** capaz de medir la **Frecuencia Cardíaca (BPM)** y la **Frecuencia Respiratoria (RPM)** utilizando únicamente una webcam convencional, sin necesidad de sensores físicos en contacto con la piel.

El sistema detecta las micro-variaciones imperceptibles en el color de la piel causadas por la absorción de luz de la hemoglobina con cada latido del corazón.

### Características Principales
* **No intrusivo:** Medición 100% sin contacto.
* **Tiempo Real:** Procesamiento de video en vivo (30 FPS).
* **Privacidad:** Todo el procesamiento es local (*Edge Computing*), ninguna imagen se guarda ni se envía a la nube.
* **Robustez:** Implementa el algoritmo **POS (Plane-Orthogonal-to-Skin)** para filtrar cambios de iluminación y movimiento.

## Stack Tecnológico
El proyecto ha sido desarrollado en **Python 3.10** utilizando las siguientes librerías clave:

* **OpenCV:** Captura de video y manejo de imagen.
* **MediaPipe FaceMesh:** Detección de rostro y mallado facial (468 puntos) para extracción de ROI robusta.
* **NumPy:** Operaciones vectoriales y álgebra lineal para el algoritmo POS.
* **SciPy:** Procesamiento Digital de Señales (Filtros Butterworth, FFT, Detrending).

## Validación y Resultados
El sistema ha sido validado experimentalmente con 7 sujetos de prueba comparando los resultados contra un oxímetro de pulso clínico (*Ground Truth*).

| Métrica | Resultado |
| :--- | :--- |
| **BPM Promedio (Ref)** | 78.94 |
| **BPM Promedio (Sistema)** | 78.21 |
| **Error Absoluto Medio (MAE)** | **1.01 BPM** |

> El sistema demostró una alta precisión en condiciones de reposo e iluminación controlada.

## Instalación y Uso

1. **Clonar el repositorio:**

   ```bash
   git clone [https://github.com/ivanperezdiaz829/rPPG-Remote-Photoplethysmography.git](https://github.com/ivanperezdiaz829/rPPG-Remote-Photoplethysmography.git)

   cd rPPG-Remote-Photoplethysmography
   ```

## Vídeo del producto

[![Ver en YouTube](https://img.youtube.com/vi/e5z5noEjIEY/0.jpg)](https://www.youtube.com/watch?v=e5z5noEjIEY)

## Autoría

Este proyecto ha sido desarrollado como parte de un trabajo de investigación de Visión por Computador por:

* **Iván Pérez Díaz** - *Desarrollo de Software,  Algoritmo de la Frecuencia Cardíaca (BPM), Investigación, Pruebas y Validación* - [GitHub](https://github.com/ivanperezdiaz829)
* **Asia Gatta** - *Algoritmo de la Frecuencia Respiratoria (RPM) e Investigación.*

---
*Este software es un prototipo con fines académicos y no constituye un dispositivo médico certificado.*