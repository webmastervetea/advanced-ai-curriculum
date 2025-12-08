# 🛡️ Mitigación de Sesgos en Datos y Modelos: Hacia una IA Equitativa

El **sesgo** en el contexto de la IA se refiere a errores sistemáticos y a menudo injustos en el proceso de toma de decisiones de un modelo, generalmente influenciados por desequilibrios o prejuicios presentes en los datos de entrenamiento. Estos sesgos pueden llevar a un rendimiento desigual para diferentes **grupos demográficos protegidos** (como raza, género, edad, o ingresos).

## 1. Tipos Comunes de Sesgos en Machine Learning

Es esencial distinguir entre los sesgos que afectan a la recopilación de datos y los que afectan al modelado:

### A. Sesgos en los Datos (Data Bias)

* **Sesgo de Muestreo (*Sampling Bias*):** Ocurre cuando el conjunto de datos utilizado para entrenar el modelo no es representativo de la población real.
    * *Ejemplo:* Un sistema de reconocimiento facial entrenado principalmente con imágenes de personas caucásicas tendrá un rendimiento significativamente peor en individuos de otras etnias.
* **Sesgo de Etiquetado (*Label Bias*):** Ocurre cuando las etiquetas utilizadas para el aprendizaje supervisado reflejan prejuicios humanos o creencias históricamente injustas.
    * *Ejemplo:* Etiquetar ciertas interacciones como "agresivas" en un conjunto de datos, donde la definición de "agresivo" puede estar sesgada culturalmente o basada en estereotipos raciales/de género.
* **Sesgo Histórico (*Historical Bias*):** El sesgo es inherente a la sociedad y está codificado en los datos históricos. Incluso si los datos son precisos, reflejan injusticias pasadas.
    * *Ejemplo:* Entrenar un modelo de contratación con datos históricos donde los hombres ocupaban la mayoría de los puestos de liderazgo sesgará el modelo para favorecer a los candidatos masculinos, sin importar las cualificaciones.

### B. Sesgos en el Modelado (Algorithm/Model Bias)

* **Sesgo de Agregación (*Aggregation Bias*):** Ocurre cuando se aplica un modelo único a un grupo muy heterogéneo, ignorando las diferencias importantes entre subgrupos.
    * *Ejemplo:* Usar un único modelo predictivo de riesgo de enfermedad para toda la población, cuando la enfermedad se manifiesta de forma diferente en hombres y mujeres, o en distintos grupos de edad.
* **Sesgo de Medida (*Measurement Bias*):** El *proxy* (variable sustituta) utilizado para medir un concepto resulta ser un predictor sesgado.
    * *Ejemplo:* Usar el historial de arrestos como *proxy* de "criminalidad" futura, cuando las tasas de arresto pueden reflejar el sesgo policial en lugar de la actividad delictiva real.

---

## 2. Estrategias de Mitigación en el Ciclo de Vida del ML

La mitigación debe abordarse en tres fases clave para ser efectiva:

### Fase I: Pre-Procesamiento (Mitigación en los Datos)

El objetivo es eliminar o reducir el sesgo de los datos antes de que lleguen al modelo.

1.  **Detección de Sesgo y Exploración (EDA):** Analizar la distribución de los datos para identificar desequilibrios en grupos protegidos. 
    * *Técnica:* Calcular las **Tasas de Paridad Demográfica** (la proporción de la métrica de interés entre grupos).
2.  **Re-muestreo (*Resampling*):** Ajustar el número de instancias en los subgrupos minoritarios para asegurar la paridad en el conjunto de entrenamiento.
3.  **Supresión de Sesgo (*Suppressing Bias*):** Eliminar o modificar explícitamente atributos sesgados o altamente correlacionados con el sesgo (por ejemplo, remover código postal si se demuestra que induce sesgo socioeconómico). *Advertencia:* Esto puede ser insuficiente, ya que el sesgo puede estar codificado en otros atributos.
4.  **Aprendizaje de Representaciones Invariantes (Adversarial Debiasing):** Entrenar un modelo auxiliar para asegurar que la representación de los datos sea indistinguible entre diferentes grupos protegidos, forzando al modelo principal a ignorar esa información.

---

### Fase II: Procesamiento (Mitigación en el Modelo)

El objetivo es modificar el algoritmo de entrenamiento para incorporar la justicia como un factor de optimización.

1.  **Restricciones de Equidad (*Fairness Constraints*):** Modificar la función de pérdida del modelo. En lugar de solo minimizar el error predictivo, la nueva función de pérdida incluye un término de penalización si se violan ciertas métricas de equidad.
    $$\mathcal{L}_{\text{final}} = \mathcal{L}_{\text{predicción}} + \lambda \cdot \mathcal{L}_{\text{equidad}}$$
    Donde $\lambda$ es un hiperparámetro que equilibra la precisión y la equidad.
2.  **Aprendizaje Adversario (*Adversarial Learning*):** Utilizar una arquitectura similar a las GANs. Se entrena un **predictor de justicia** (un discriminador) para predecir a qué grupo protegido pertenece un individuo basándose en las representaciones latentes del modelo principal. El modelo principal se entrena para **engañar** a este predictor, creando representaciones que no contengan información de sesgo, sin sacrificar la precisión predictiva.

---

### Fase III: Post-Procesamiento (Mitigación en las Predicciones)

El objetivo es ajustar el resultado final del modelo para garantizar la equidad antes de que se implemente la decisión.

1.  **Clasificación Equitativa (*Equalized Odds*):** Ajustar los umbrales de decisión del modelo (el punto de corte de probabilidad para clasificar como positivo) de manera diferente para cada grupo protegido.
    * *Objetivo:* Lograr la paridad en las **tasas de falsos positivos** y **tasas de falsos negativos** entre los grupos. Por ejemplo, si el modelo tiene una tasa de falsos positivos del 5% en el Grupo A y del 15% en el Grupo B, se ajustan los umbrales para igualar estas tasas.
2.  **Paridad de Oportunidad (*Equal Opportunity*):** Una métrica menos estricta que solo requiere igualar la **tasa de verdaderos positivos** (recall) entre los grupos para una clasificación específica. Esto se aplica a escenarios donde el resultado positivo es un beneficio (ej. préstamos, admisión universitaria).
3.  **Calibración:** Asegurar que las probabilidades de predicción del modelo reflejen con precisión las verdaderas probabilidades para todos los subgrupos.

---

## 3. Desafíos y Consideraciones Éticas

* **Trade-off entre Equidad y Precisión:** A menudo, los métodos de mitigación de sesgos requieren sacrificar una pequeña cantidad de precisión general del modelo para lograr una mayor equidad. La decisión sobre dónde trazar la línea es ética y de política, no solo técnica.
* **Imposibilidad de la Equidad Total:** No es matemáticamente posible satisfacer todas las definiciones de equidad simultáneamente (por ejemplo, es imposible garantizar la paridad demográfica y las tasas de falsos positivos iguales al mismo tiempo). Se debe elegir la métrica de equidad más relevante para el dominio de la aplicación (ej. en sistemas penales, la tasa de falsos positivos es crítica).
* **Interseccionalidad:** Los métodos de mitigación a menudo se centran en un único atributo (género o raza), ignorando los sesgos experimentados por subgrupos minoritarios múltiples (mujeres negras de bajos ingresos).

La mitigación de sesgos es un proceso continuo que requiere un esfuerzo multidisciplinario y una supervisión constante para garantizar que los sistemas de IA sigan siendo justos y éticos después de la implementación.


---

Continua: [[6-2]()] 
