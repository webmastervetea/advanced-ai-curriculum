# 🌊 Detección de Deriva de Concepto y Datos: Asegurando la Fiabilidad del Modelo en Tiempo Real

Los modelos de *Machine Learning* y *Deep Learning* entrenados en entornos controlados a menudo experimentan una degradación de rendimiento cuando se implementan en producción. Esta caída de precisión se debe típicamente a fenómenos de **Deriva** (*Drift*), los cuales reflejan un cambio en el entorno subyacente que el modelo no ha aprendido a manejar.

La **Detección de Deriva en tiempo real** es una tarea crucial en **MLOps** para monitorear, diagnosticar y mitigar esta degradación antes de que afecte la toma de decisiones.

## 1. Definición y Tipos de Deriva

Es fundamental distinguir entre los dos tipos principales de deriva que afectan al rendimiento del modelo.

### A. Deriva de Concepto (*Concept Drift*)

La Deriva de Concepto ocurre cuando la **relación entre las variables de entrada ($\mathbf{X}$) y la variable de salida o etiqueta ($\mathbf{Y}$) cambia con el tiempo**. Es el tipo de deriva más grave, ya que la verdad fundamental del problema ha cambiado.

* **Definición Matemática:** La distribución de la probabilidad condicional $P(\mathbf{Y}|\mathbf{X})$ cambia.
* **Ejemplo:** Un modelo bancario entrenado para detectar fraude aprende que las transacciones superiores a \$1,000 realizadas fuera del país son fraudulentas. Si los estafadores cambian su estrategia a transacciones pequeñas y locales, el concepto de "fraude" ha cambiado. El *input* (transacciones) puede parecer el mismo, pero su significado respecto a la etiqueta (fraude/no fraude) es distinto.

### B. Deriva de Datos (*Data Drift*)

La Deriva de Datos ocurre cuando la **distribución de las variables de entrada ($\mathbf{X}$) cambia con el tiempo**, pero la relación con la variable de salida ($\mathbf{Y}$) permanece constante.

* **Definición Matemática:** La distribución de la entrada $P(\mathbf{X})$ cambia.
* **Ejemplo:** Un modelo de *e-commerce* se entrenó sobre datos donde el $80\%$ de los clientes usaban navegadores de escritorio. Si, repentinamente, el $80\%$ de los clientes comienza a usar dispositivos móviles (cambio en $P(\mathbf{X})$), la capacidad del modelo para predecir (la relación $P(\mathbf{Y}|\mathbf{X})$) podría no cambiar, pero el rendimiento podría degradarse si el modelo no generaliza bien a la nueva distribución de entrada.

---

## 2. Técnicas de Detección de Deriva

La detección de deriva implica monitorear estadísticas clave y utilizar pruebas de hipótesis en tiempo real.

### A. Detección de Deriva de Datos ($P(\mathbf{X})$)

El objetivo es comparar la distribución de los datos recientes (ventana de prueba) con la distribución de los datos de entrenamiento (ventana de referencia).

* **Pruebas Estadísticas de Dos Muestras:**
    * **Prueba de Kolmogorov-Smirnov (KS):** Ideal para variables numéricas, evalúa si dos muestras provienen de la misma distribución continua.
    * **Prueba $\chi^2$ (Chi-Cuadrado):** Ideal para variables categóricas, evalúa si la frecuencia de las categorías ha cambiado significativamente.
* **Detección de Cambios de Distribución:** El **Índice de Estabilidad de la Población (*Population Stability Index*, PSI)** es una métrica común en finanzas que cuantifica cuánto ha cambiado una distribución de una variable entre dos periodos. 

### B. Detección de Deriva de Concepto ($P(\mathbf{Y}|\mathbf{X})$)

Detectar la Deriva de Concepto requiere medir la **degradación directa del rendimiento** del modelo.

* **Monitoreo del Rendimiento:** La forma más directa es monitorear la métrica clave del modelo (precisión, F1-Score, AUC, etc.) en un flujo de datos continuo. Una caída sostenida en esta métrica es una clara señal de deriva.
* **Monitoreo del Error (*Error Monitoring*):** Algoritmos como **ADWIN (*Adaptive Windowing*)** monitorean el error del modelo en una ventana de datos deslizante. Cuando el error medio cambia significativamente, se dispara una alarma.
* **Monitoreo de la Precisión Tardia (*Delayed Ground Truth*):** En muchos entornos, la etiqueta de verdad fundamental (*ground truth*) está disponible con un retraso (ej. un diagnóstico médico tarda días, o el fraude se confirma semanas después). La detección de deriva debe tener en cuenta este retraso, monitoreando la métrica en la última ventana donde la verdad está disponible.

---

## 3. Mitigación y Respuesta

Una vez que se detecta la deriva, se requiere una acción inmediata para estabilizar el rendimiento:

### A. Retraining (Reentrenamiento)

La acción más común es reentrenar el modelo con los **datos más recientes**, incorporando el nuevo concepto o la nueva distribución de datos.

* **Entrenamiento Incremental:** En lugar de reentrenar desde cero, se ajustan los pesos del modelo existente utilizando solo los datos de la nueva distribución. Esto es más rápido y eficiente.
* **Ventanas Deslizantes (*Sliding Windows*):** El modelo se reentrena periódicamente utilizando solo la última ventana de datos (que contiene el concepto actual), descartando los datos obsoletos.

### B. Modelos *Ensemble* Adaptativos

Se utilizan múltiples modelos que se adaptan a la deriva.

* **Weighted Ensemble:** Se entrena un *ensemble* de modelos. Cuando se detecta una deriva, los modelos que se ajustan mejor a la nueva distribución reciben un peso mayor en la predicción final, y los modelos antiguos se descartan o se reentrenan.

La detección y mitigación de la deriva son componentes esenciales del **ciclo de vida del ML en producción**, asegurando que los modelos no solo sean precisos al principio, sino que sigan siendo una herramienta de toma de decisiones confiable a largo plazo.


---

Continua: [[15-2-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/15-2-2-metodos-para-asegurar-la-confiabilidad.md)] 
