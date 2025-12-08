# ✍️ Generación de Texto y Resumen Abstractivo: Creación de Contenido con IA

La **Generación de Texto** es el campo del Procesamiento del Lenguaje Natural (NLP) que se ocupa de la producción de secuencias de lenguaje coherentes, gramaticalmente correctas y contextualmente relevantes. Dentro de esta área, el **Resumen Abstractivo** es una de las tareas más avanzadas, ya que requiere comprensión profunda y síntesis creativa, en lugar de simplemente copiar fragmentos del texto original.

---

## 1. El Proceso Fundamental de la Generación de Texto

La mayoría de los modelos de generación de texto operan de manera **auto-regresiva**; es decir, generan un *token* (palabra o subpalabra) a la vez, basándose en todos los *tokens* que lo preceden.

### A. La Probabilidad del Siguiente Token
En el corazón de la generación está el cálculo de la probabilidad condicional del siguiente *token* ($w_t$) dado el historial de *tokens* generados ($w_1, w_2, \dots, w_{t-1}$):

$$P(w_t | w_1, w_2, \dots, w_{t-1})$$

Los **Modelos de Lenguaje Grandes (LLMs)**, como GPT, utilizan la arquitectura de **Decodificador de Transformador** para calcular esta probabilidad de manera eficiente y contextualizada.

### B. Técnicas de Muestreo para la Generación

Una vez que el modelo calcula una distribución de probabilidad sobre todo el vocabulario para el siguiente *token*, se utiliza una estrategia de muestreo para elegir la palabra real:

| Estrategia | Descripción | Efecto |
| :--- | :--- | :--- |
| **Búsqueda Codiciosa (*Greedy Search*)** | Siempre elige el *token* con la probabilidad más alta. | Rápida, pero genera texto repetitivo y subóptimo (no explora buenas secuencias futuras). |
| **Búsqueda de Haz (*Beam Search*)** | Mantiene un conjunto de $k$ secuencias candidatas de alta probabilidad. En cada paso, evalúa las extensiones de esas $k$ secuencias y selecciona las $k$ mejores en general. | Genera salidas de alta calidad y longitud fija (ideal para traducción o resumen extractivo). |
| **Muestreo por Temperatura (*Temperature Sampling*)** | Introduce aleatoriedad en las probabilidades. Una temperatura alta hace que las probabilidades sean más uniformes (más aleatoriedad), y una temperatura baja las hace más "puntiagudas" (más determinista). | Ideal para tareas creativas donde se desea diversidad (ej. *storytelling*). |
| **Muestreo Top-K / Top-P (Núcleo)** | Restringe el muestreo solo a los *k* *tokens* más probables (Top-K) o a los *tokens* cuya probabilidad acumulada suma un umbral $p$ (Top-P o *Nucleus Sampling*). | Equilibra la creatividad con la coherencia, evitando *tokens* muy improbables. Es la estrategia más utilizada en los LLMs conversacionales. |

---

## 2. El Desafío del Resumen de Texto

El **Resumen de Texto** es el proceso de generar una versión más corta del texto de entrada que conserve sus puntos clave. Se clasifica en dos categorías:

### A. Resumen Extractivo (*Extractive Summarization*)
El resumen se crea seleccionando y concatenando frases u oraciones directamente del texto original, basándose en su importancia (determinada por algoritmos de *ranking* de frases).

* **Ventajas:** Alta fidelidad al contenido original; gramaticalmente correcto.
* **Desventajas:** No puede parafrasear ni reestructurar. Puede carecer de cohesión si las frases seleccionadas no encajan bien.

### B. Resumen Abstractivo (*Abstractive Summarization*)
El resumen se crea **generando frases y palabras nuevas** que condensan el significado del texto original. Requiere la comprensión profunda del significado y la reformulación conceptual.

* **Ventajas:** Genera resúmenes más fluidos, concisos y humanos; puede sintetizar información de múltiples frases en una nueva.
* **Desventajas:** Alto riesgo de introducir **alucinaciones** (información falsa o no respaldada por el texto de entrada). Más complejo de entrenar.

---

## 3. Arquitecturas para el Resumen Abstractivo

El resumen abstractivo moderno está dominado por la arquitectura **Codificador-Decodificador (*Encoder-Decoder*)** basada en Transformadores.

### A. Estructura del Modelo

1.  **Codificador (Encoder):** El modelo de Transformador lee y procesa el documento fuente completo. Utiliza la **Atención Bidireccional** para construir una representación contextual rica de cada palabra, capturando las relaciones semánticas en todo el texto.
2.  **Decodificador (Decoder):** El modelo de Transformador genera el resumen de manera auto-regresiva. Utiliza la **Atención Causal Enmascarada** en su propia salida y **Atención Cruzada (*Cross-Attention*)** para enfocarse en las partes relevantes de la representación codificada del documento fuente en cada paso de generación.



Modelos como **BART** (Bidirectional and Auto-Regressive Transformer) y **T5** (Text-to-Text Transfer Transformer) son ejemplos típicos de esta arquitectura, pre-entrenados específicamente para tareas de secuencia-a-secuencia, incluyendo el resumen.

### B. Control y Coherencia

Para mejorar la calidad y reducir las alucinaciones en el resumen abstractivo, se utilizan técnicas avanzadas:

* **Pérdida de Coherencia (*Coherence Loss*):** Pérdidas adicionales se añaden durante el entrenamiento para penalizar resúmenes que rompan la lógica o la coherencia entre frases.
* **Inclusión de Extractividad:** Algunos modelos híbridos se entrenan para ser abstractivos, pero se les introduce una pérdida que los alienta ligeramente a usar frases clave del texto original, mejorando la fidelidad.
* **Refinamiento con RL:** Se puede utilizar el Aprendizaje por Refuerzo (RL) para ajustar el modelo. El agente (el Decodificador) es recompensado por generar resúmenes que obtienen altas puntuaciones en métricas humanas o en métricas de fidelidad (como **ROUGE** o **BERTScore**).

---

## 4. Métricas de Evaluación (ROUGE)

Evaluar la calidad de un resumen (especialmente el abstractivo) es complejo, ya que la "verdad" puede tener múltiples formulaciones. La métrica más utilizada es **ROUGE** (*Recall-Oriented Understudy for Gisting Evaluation*):

* **ROUGE-N:** Mide el solapamiento de $N$-gramas (secuencias de $N$ palabras) entre el resumen generado y uno o más resúmenes de referencia escritos por humanos.
    * **ROUGE-1:** Mide el solapamiento de unigramas (palabras individuales).
    * **ROUGE-2:** Mide el solapamiento de bigramas (pares de palabras).
* **ROUGE-L:** Mide la longitud de la subsecuencia común más larga (LCS), lo que favorece el flujo y la estructura.

Aunque ROUGE es imperfecto (prefiere la superposición literal), sigue siendo la herramienta estándar para medir la *fidelidad* y la *calidad* de la generación en tareas de resumen.
