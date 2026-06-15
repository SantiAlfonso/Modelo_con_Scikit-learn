# 🌲 Análisis de Factores Climáticos y Modelado de Incendios Forestales

Este proyecto desarrolla un modelo de regresión lineal múltiple utilizando mínimos cuadrados ordinarios (OLS) para estimar y analizar el impacto de variables climáticas (precipitaciones en distintas ventanas temporales) y factores ecológicos sobre el tamaño del área afectada (`area_quemada`) por incendios forestales. 

El núcleo del proyecto radica en el proceso de **Análisis Exploratorio de Datos (EDA)** y la posterior **Ingeniería de Características**, demostrando cómo identificar y resolver problemas críticos de álgebra lineal (multicolinealidad y sesgo) que suelen invalidar los modelos predictivos en el mundo real.

---

## 📊 Reporte Interactivo de Datos

Para explorar a fondo la distribución, las alertas de calidad de datos, el sesgo y las correlaciones analizadas en la fase inicial del proyecto, puedes acceder al reporte interactivo completo generado con Pandas Profiling:

🚀 **[Ver el Análisis Exploratorio de Datos (EDA) Interactivo](https://htmlpreview.github.io/?https://github.com/SantiAlfonso/Modelo_con_Scikit-learn/blob/main/EDA.html)**

---

## 🛠️ Retos Técnicos y Soluciones Implementadas

### 1. Eliminación de Multicolinealidad Estricta
* **El Problema:** Al analizar la matriz de correlación, se detectó una redundancia extrema entre las variables de lluvia previa (`Prec_pre_7`, `Prec_pre_15` y `Prec_pre_30` con correlaciones superiores a $0.80$). Esto provocaba inestabilidad matemática en la inversión de la matriz $X^TX$, disparando el Número de Condición del modelo original a **2,080**.
* **La Solución:** Se aplicó una estrategia de descarte selectivo, conservando únicamente la ventana de 30 días (`Prec_pre_30`) por ofrecer mayor representatividad histórica y mejor variabilidad. El Número de Condición final se redujo a un saludable **37.7**, garantizando coeficientes totalmente estables y confiables.

### 2. Tratamiento de Sesgo Extremo (Highly Skewed)
* **El Problema:** Las variables de precipitación presentaban un sesgo positivo severo ($\gamma_1 > 23$), con una alta concentración de valores en cero (períodos de sequía) y colas excesivamente largas debido a lluvias torrenciales atípicas.
* **La Solución:** Se aplicó una transformación logarítmica complementaria $np.log1p()$ para estabilizar la distribución de los datos antes de alimentar el algoritmo de regresión.

### 3. Evitando la Trampa de la Variable Dummy
* **La Solución:** Para las variables cualitativas (`clase_incendio`, `mes_incendio`, `vegetacion`), se configuró la dummificación forzando la eliminación de la primera categoría (`drop_first=True`) y asegurando su codificación numérica estricta, evitando así la colinealidad perfecta con el intercepto del modelo.

---

## 📊 Principales Hallazgos Estadísticos

Tras limpiar el dataset y ajustar el modelo final con `statsmodels`, obtuvimos un $R^2$ de **0.23** con consistencia perfecta entre los conjuntos de entrenamiento y test (cero sobreajuste). Las variables con mayor relevancia e impacto significativo ($p < 0.05$) fueron:

* **Dinámica Temporal (Efecto Junio):** Los incendios iniciados en el mes de **junio** muestran un incremento esperado drástico en el área afectada ($+2,066$ Ha), marcando la estacionalidad más crítica del dataset.
* **Vulnerabilidad de Ecosistemas:** El **Bosque tropical perennifolio** resultó ser el ecosistema más propenso a la propagación a gran escala ($+1,598$ Ha debido a la alta disponibilidad de combustible denso). Por el contrario, el **Desierto** demostró un impacto negativo significativo ($-1,861$ Ha), actuando como una barrera natural por falta de vegetación continua.
* **Tendencia de la Lluvia:** Se validó el coeficiente negativo de la precipitación acumulada, confirmando estadísticamente la hipótesis física de que a mayor humedad previa en el suelo, menor es la tasa esperada de propagación del fuego.

---

## 💻 Tecnologías Utilizadas

* **Python 3.12**
* **Pandas & NumPy** (Limpieza y transformación matricial)
* **YData Profiling / Pandas Profiling** (Auditoría automática de calidad de datos)
* **Statsmodels** (Ajuste OLS e inferencia estadística)
* **Scikit-Learn** (Partición de datos y evaluación de métricas RMSE/MAE)
