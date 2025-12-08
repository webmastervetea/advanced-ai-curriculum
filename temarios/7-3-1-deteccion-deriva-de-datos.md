# 📉 Detección de Deriva de Datos y Deriva de Modelos: Asegurando la Fiabilidad de la IA

Una de las principales diferencias entre el software tradicional y los sistemas de *Machine Learning* es que el rendimiento de un modelo de ML puede degradarse con el tiempo, incluso sin que se cambie una sola línea de código. Este fenómeno se debe a la **Deriva (*Drift*)**, que rompe la suposición fundamental de que el entorno de producción es estadísticamente similar al entorno de entrenamiento.

## 1. Deriva de Datos (*Data Drift*)

La **Deriva de Datos** ocurre cuando las **propiedades estadísticas** de los datos de entrada (las características $\mathbf{X}$) cambian en el tiempo entre el entorno de entrenamiento y el entorno de producción. Es la causa principal de la eventual degradación del modelo.

### A. Causas de la Deriva de Datos

La deriva puede manifestarse en cualquier característica de entrada y a menudo es un reflejo de cambios en el mundo real:

* **Cambios Naturales/Ambientales:** Estacionalidad (cambios en el patrón de compras en Navidad), ciclos económicos, o tendencias sociales (cambios en la jerga utilizada en las redes sociales).
* **Cambios en el Sensor/Recolección de Datos:** La actualización o fallo de un sensor puede alterar el rango, la distribución o la calidad de la característica que recopila.
* **Introducción de Nuevas Poblaciones:** Un modelo desplegado globalmente que de repente comienza a recibir tráfico de una región geográficamente distinta con diferentes patrones demográficos.

### B. Detección de la Deriva de Datos

La detección de *Data Drift* es una tarea de **comparación de distribución de probabilidad** y se realiza mediante pruebas estadísticas entre la distribución de la característica en el **conjunto de entrenamiento de referencia** y la distribución en el **lote de datos de producción** actual.

* **Pruebas Estadísticas para Características Categóricas:**
    * **Estadístico Chi-Cuadrado ($\chi^2$):** Mide la diferencia entre la frecuencia observada en producción y la frecuencia esperada (de entrenamiento).
* **Pruebas Estadísticas para Características Numéricas:**
    * **Prueba de Kolmogorov-Smirnov (K-S):** Mide la distancia máxima entre las Funciones de Distribución Acumulada (CDF) de los dos conjuntos de datos. Es sensible a cambios en la forma y la ubicación de la distribución.
    * **Divergencia de Kullback-Leibler (KL):** Mide la diferencia de información o "distancia" entre las dos distribuciones de probabilidad.

**Alerta:** Si la diferencia estadística supera un umbral predefinido (determinado por el nivel de significancia $\alpha$), se activa una alerta de deriva.

---

## 2. Deriva de Conceptos (*Concept Drift*)

La **Deriva de Conceptos** es una forma particular de deriva que ocurre cuando la **relación entre las características de entrada ($\mathbf{X}$) y la variable objetivo ($Y$) cambia**. El modelo se vuelve obsoleto porque las reglas de decisión que aprendió ya no son válidas en el nuevo entorno.

* **Ejemplo:** Un modelo entrenado para predecir si una cuenta es fraudulenta ($Y$) se basa en el patrón de transacciones ($X$). Si los criminales cambian sus tácticas (el patrón $X$ cambia), la relación $P(Y|\mathbf{X})$ se altera. Las entradas aún se ven normales (no hay *Data Drift*), pero la predicción del modelo ya no es correcta (hay *Concept Drift*).

El *Concept Drift* es más difícil de detectar directamente porque **requiere la etiqueta verdadera ($Y$)**, que a menudo solo está disponible con un retraso (ej. hasta que el cliente confirma el fraude o la venta se realiza).

---

## 3. Deriva de Modelos (*Model Drift*)

La **Deriva de Modelos** es el término general para la **degradación del rendimiento predictivo** del modelo en producción. Es el **efecto** que resulta de la Deriva de Datos y/o la Deriva de Conceptos.

### A. Detección de la Deriva de Modelos

La detección de *Model Drift* se realiza monitoreando las métricas de rendimiento clave del modelo, asumiendo que las etiquetas verdaderas están disponibles.

* **Métricas de Clasificación:** Precisión, *Recall*, $F_1$-Score, o Área Bajo la Curva (AUC).
* **Métricas de Regresión:** Error Cuadrático Medio (MSE) o Error Absoluto Medio (MAE).

**Alerta:** Se establece un umbral de caída de rendimiento (ej. si el AUC cae un 5% por debajo del nivel de validación).

### B. Detección de Degradación Indirecta (Cuando $Y$ es Desconocida)

Cuando la etiqueta verdadera ($Y$) tiene un retraso largo (por ejemplo, en la predicción de la pérdida de clientes, que solo se confirma meses después), el *Model Drift* se puede inferir indirectamente:

1.  **Monitoreo de la Estabilidad de la Salida:** Se monitorea la distribución de las **predicciones** del modelo. Un cambio repentino en la distribución de las probabilidades de salida (ej. un modelo de fraude que de repente predice que casi todo es "no fraudulento") a menudo indica que el modelo está fallando.
2.  **Análisis de la Importancia de las Características:** Si los métodos de interpretabilidad (como SHAP) muestran que la importancia de las características clave está cambiando drásticamente, puede ser una señal de que el modelo está adaptándose mal o que ha habido un *Data Drift* severo.

---

## 4. Estrategias de Mitigación

Una vez detectada la deriva (ya sea de datos o de modelo), la intervención es necesaria para recuperar el rendimiento.

| Estrategia | Descripción | Aplicación |
| :--- | :--- | :--- |
| **Reentrenamiento Programado** | Entrenar de nuevo el modelo periódicamente (ej. cada mes) con los datos más recientes, independientemente de la detección de deriva. | Deriva lenta o estacionalidad predecible. |
| **Reentrenamiento Reactivo** | Volver a entrenar el modelo inmediatamente después de que se detecta una alerta de *Data Drift* o *Model Drift* significativa. | Deriva rápida e impredecible (ej. evento noticioso, fallo de sensor). |
| **Aprendizaje Continuo (*Continual Learning*)** | El modelo se actualiza de forma incremental con cada nuevo lote de datos a medida que llega, sin borrar el conocimiento antiguo. | Entornos muy dinámicos (ej. mercados financieros, sistemas de recomendación). |
| **Mantenimiento del Modelo** | Analizar la causa raíz de la deriva (ej. si una característica está corrupta debido a un sensor defectuoso), y corregir el problema de ingeniería de datos antes de reentrenar. | Problemas de *Data Drift* relacionados con la infraestructura o el *pipeline* de datos. |

La gestión de la deriva requiere un sólido *pipeline* de **MLOps** que automatice la supervisión de la calidad de los datos de entrada y las métricas de rendimiento del modelo.
