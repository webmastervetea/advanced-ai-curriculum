# 💾 Compresión y Cuantización de Modelos para Edge AI

El objetivo de la **Inteligencia Artificial en el Borde (*Edge AI*)** es ejecutar modelos de *Deep Learning* directamente en dispositivos locales (móviles, IoT, *wearables*) en lugar de en la nube. Esto garantiza **baja latencia**, **privacidad** y **fiabilidad** (independencia de la red). Para lograrlo, los modelos deben ser drásticamente reducidos en tamaño y complejidad.

## 1. Cuantización (*Quantization*): Reducción de Precisión Numérica

La **Cuantización** es el proceso de reducir la precisión numérica de los pesos y activaciones del modelo. Esto tiene el impacto más significativo e inmediato en la reducción del tamaño del modelo y en el consumo de energía.

### A. Mecanismo Fundamental

Los modelos de *Deep Learning* se entrenan típicamente utilizando números de punto flotante de 32 bits (**FP32**). La cuantización mapea estos valores a representaciones de menor precisión, generalmente números enteros de 8 bits (**INT8**).

* **Reducción de Memoria:** Pasar de FP32 a INT8 reduce el tamaño de almacenamiento del modelo y la memoria en tiempo de ejecución en **cuatro veces**.
* **Aceleración:** Los procesadores especializados de *Edge AI* (como los NPUs o los aceleradores móviles) están optimizados para realizar cálculos con INT8 mucho más rápido y con menos energía que con FP32.

### B. Tipos de Cuantización

1.  **Post-Training Quantization (PTQ):** La cuantización se realiza **después** de que el modelo ha sido completamente entrenado. Es el método más rápido y simple. Solo requiere un pequeño conjunto de datos de calibración para determinar los rangos de mapeo de los valores FP32 a INT8.
    * **Limitación:** Puede causar una ligera pérdida de precisión si el rango de valores de los pesos es muy amplio o desigual.
2.  **Quantization-Aware Training (QAT):** El modelo se entrena o se somete a un ajuste fino (*fine-tuning*) utilizando operaciones simuladas de baja precisión.
    * **Ventaja:** El modelo se vuelve **consciente** de los errores de cuantización y ajusta sus pesos para compensarlos. Esto resulta en una precisión mucho mayor, a menudo igualando o superando la precisión del modelo base FP32.
    * **Desventaja:** Requiere el proceso de entrenamiento o ajuste fino y el conjunto de datos de entrenamiento completo.

---

## 2. Poda de Modelos (*Model Pruning*): Reducción de la Redundancia

La **Poda** es el proceso de eliminar las conexiones o neuronas del modelo que tienen poco impacto en la precisión, basándose en la idea de que la mayoría de los modelos de *Deep Learning* están **sobredimensionados** y contienen redundancia.

### A. Mecanismo de Poda

1.  **Identificación:** Se evalúa la **importancia** de los pesos de la red (típicamente midiendo su magnitud absoluta). Los pesos cercanos a cero se consideran poco importantes.
2.  **Eliminación (*Clipping*):** Los pesos no importantes se establecen a cero.
3.  **Ajuste Fino:** Se realiza un ajuste fino del modelo (con los pesos restantes) para recuperar cualquier pequeña pérdida de precisión causada por la poda.

### B. Tipos de Poda

* **Poda No Estructurada (Sparcity):** Se eliminan individualmente los pesos menos importantes en cualquier lugar de las matrices.
    * **Ventaja:** Logra la mayor tasa de compresión (ej. hasta el 90-95% de los pesos).
    * **Desventaja:** El patrón disperso resultante es difícil de acelerar con el *hardware* de propósito general (GPUs/CPUs), aunque es útil en *hardware* especializado para matrices dispersas.
* **Poda Estructurada:** Se eliminan bloques enteros de la red (ej. filas o columnas de una matriz de pesos, o canales completos en una red convolucional).
    * **Ventaja:** La estructura regular resultante es **amigable con el *hardware*** estándar (GPUs) y facilita el uso de bibliotecas optimizadas para lograr una aceleración real en el tiempo de ejecución.

---

## 3. Destilación del Conocimiento (*Knowledge Distillation*): El Estudiante y el Maestro

La **Destilación del Conocimiento** es una técnica de compresión basada en el entrenamiento donde se utiliza un modelo grande y de alto rendimiento (**Modelo Maestro**) para entrenar un modelo mucho más pequeño y rápido (**Modelo Estudiante**).

### A. Mecanismo

1.  **Modelo Maestro (*Teacher*):** El modelo grande y lento (ej. un LLM completo) se entrena primero para lograr la máxima precisión. Sus salidas (típicamente los ***logits* o las probabilidades suaves**) contienen el conocimiento "destilado" del entrenamiento.
2.  **Modelo Estudiante (*Student*):** El modelo pequeño y eficiente (ej. un Transformer con menos capas o menos cabezas de atención) se entrena para imitar las salidas del Maestro.
3.  **Función de Pérdida Doble:** El Estudiante se entrena con una función de pérdida que incluye dos componentes:
    * **Pérdida Hard Target:** La pérdida estándar calculada a partir de las etiquetas reales.
    * **Pérdida Soft Target:** La pérdida calculada a partir de la diferencia entre la salida del Estudiante y la salida "suave" (con alta temperatura) del Maestro.



### B. Ventajas

* **Rendimiento Superior:** El Estudiante, aunque tiene menos parámetros, a menudo logra una precisión significativamente **mayor** que la de un modelo entrenado de forma independiente con la misma arquitectura pequeña. Esto se debe a que el Maestro actúa como un fuerte regularizador, proporcionando etiquetas más informativas y menos ruidosas.
* **Flexibilidad:** El Estudiante puede tener una arquitectura completamente diferente al Maestro (ej. Destilar un Transformer grande en un modelo RNN o CNN pequeño).

---

## 4. Combinación de Técnicas para el Despliegue en Edge

En la práctica, estas técnicas se utilizan a menudo en secuencia para lograr la máxima compresión y aceleración:

1.  **Destilación:** Se reduce la complejidad arquitectónica (de Maestro a Estudiante pequeño).
2.  **Poda:** Se elimina la redundancia de los pesos del Modelo Estudiante.
3.  **Cuantización:** Se convierten los pesos y activaciones resultantes a INT8.

Al pasar de un modelo FP32 sobredimensionado a un modelo INT8 podado y destilado, se puede lograr una reducción de la memoria de **$20\times$ a $50\times$** y una aceleración de **$5\times$ a $10\times$** en la inferencia, lo que hace posible el *Edge AI* de alto rendimiento.


---

Continua: [[7-3-1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/7-3-1-deteccion-deriva-de-datos.md)] 
