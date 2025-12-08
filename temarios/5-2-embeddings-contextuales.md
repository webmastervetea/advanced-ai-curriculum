# 🧠 Embeddings Contextuales: Más Allá de la Ambigüedad en el Lenguaje

## 1. El Problema de la Ambigüedad y los Embeddings Estáticos

Antes de la llegada de los *embeddings* contextuales, los modelos de lenguaje dependían de **Embeddings Estáticos** (como Word2Vec o GloVe). Estos modelos asignaban un único vector (un *embedding*) a cada palabra en el vocabulario.

* **Limitación:** Un solo vector no puede capturar la polisemia (múltiples significados) de una palabra. Por ejemplo, la palabra **"banco"** siempre tendría el mismo vector, independientemente de si se usaba en el contexto de "banco de inversión" o "banco del río". La ambigüedad es inherente al lenguaje humano, y los modelos estáticos no podían resolverla.

Los **Embeddings Contextuales** resuelven este problema al generar una representación vectorial para una palabra que es **dinámica**; es decir, la representación cambia en función del **contexto** circundante dentro de una frase.

## 2. El Mecanismo de los Embeddings Contextuales

El secreto detrás de los *embeddings* contextuales es el uso de la arquitectura **Transformador** y su mecanismo de **Auto-Atención (*Self-Attention*)**.

### Auto-Atención y Contexto

Cuando un modelo basado en Transformador (como BERT o GPT) procesa una frase:

1.  Cada *token* (palabra o subpalabra) se inicializa con un *embedding* estático base.
2.  A través de las múltiples capas del Transformador, el mecanismo de Auto-Atención permite que cada *token* **interactúe** con todos los demás *tokens* de la frase.
3.  La atención calcula la **relevancia** de cada palabra circundante para determinar la representación final de la palabra actual.

El resultado es que, después de pasar por las capas del Transformador, el vector de la palabra "banco" en la frase "Fui al **banco** a depositar" será significativamente diferente del vector de "Me senté en el **banco** del parque". El *embedding* final está saturado de información contextual.

## 3. Arquitecturas Clave

Los *embeddings* contextuales se popularizaron con modelos que emplean diferentes estrategias de procesamiento direccional: **Bidireccional** (BERT) y **Unidireccional** (GPT).

### A. BERT (Bidirectional Encoder Representations from Transformers)

BERT, lanzado por Google en 2018, fue un cambio de juego porque introdujo el procesamiento **bidireccional** del contexto.

* **Arquitectura:** Utiliza el **Codificador (*Encoder*)** del Transformador.
* **Contexto:** El *embedding* de cada palabra se calcula mirando el contexto completo, tanto **izquierdo** (precedente) como **derecho** (siguiente).
* **Tareas de Entrenamiento Clave:**
    * ***Masked Language Model (MLM):*** El modelo aprende a predecir palabras que han sido aleatoriamente **enmascaradas** o cubiertas en la frase. Esto lo obliga a comprender el contexto bidireccional para llenar el vacío.
    * ***Next Sentence Prediction (NSP):*** El modelo predice si la segunda frase sigue lógicamente a la primera.

* **Uso:** Los *embeddings* de salida de BERT (la representación contextualizada) son excelentes para tareas de **comprensión** como clasificación de texto, reconocimiento de entidades o respuesta a preguntas (extracción de información).

### B. GPT (Generative Pre-trained Transformer)

Los modelos GPT (OpenAI) representan la arquitectura dominante para la generación de texto.

* **Arquitectura:** Utiliza el **Decodificador (*Decoder*)** del Transformador con **Atención Enmascarada**.
* **Contexto:** Genera *embeddings* **unidireccionales** (o auto-regresivos). Al generar una palabra, solo puede considerar el contexto que la **precede** (su izquierda). Esto es una necesidad para la generación de texto, ya que la red no debe "ver" la respuesta futura.
* **Tarea de Entrenamiento Clave:**
    * ***Causal Language Modeling (CLM):*** El modelo se entrena para predecir el **siguiente *token*** en la secuencia.

* **Uso:** Los *embeddings* de GPT son excelentes para tareas de **generación** (respuesta a preguntas abstractas, escritura creativa, *chatbots*).



## 4. El Proceso de Uso (*Transfer Learning*)

El poder de los *embeddings* contextuales reside en el **Aprendizaje por Transferencia (*Transfer Learning*)**:

1.  **Pre-entrenamiento:** El modelo (BERT o GPT) se entrena exhaustivamente en una enorme y diversa cantidad de datos (cientos de miles de millones de palabras) para que sus *embeddings* capturen la estructura general del lenguaje.
2.  **Ajuste Fino (*Fine-Tuning*):** Para una tarea específica (ej. clasificación de reseñas de películas), el modelo pre-entrenado se ajusta con una pequeña cantidad de datos etiquetados. En esta fase, los *embeddings* contextuales ya codifican el significado sofisticado de las palabras, y la red solo necesita aprender cómo mapear estas ricas representaciones a la etiqueta de salida ("positivo" o "negativo").

Esto redujo drásticamente la cantidad de datos etiquetados necesarios para alcanzar el alto rendimiento en tareas de NLP.

## 5. El Impacto de los Embeddings Contextuales

La adopción de modelos basados en Transformadores y *embeddings* contextuales transformó el campo del NLP:

* **Resolución de Ambigüedad:** Los modelos pudieron distinguir con éxito entre los diferentes significados de las palabras basándose en la frase, logrando una comprensión semántica mucho más profunda.
* **Nuevo Récord (*State-of-the-Art*):** Estos modelos establecieron rápidamente nuevos récords de rendimiento en casi todas las tareas de NLP.
* **Fundación de los LLMs:** La capacidad de los *embeddings* contextuales para codificar el significado en un espacio denso y de alta dimensión es el requisito previo que permitió la escalabilidad y las capacidades de razonamiento de los Modelos de Lenguaje Grandes modernos.

En resumen, el *embedding* contextual representa la evolución de la representación del lenguaje, pasando de una mirada estática y aislada de las palabras a una representación dinámica, sensible al contexto y profundamente interconectada.



---

Continua: [[5-2-b1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/5-2-b1-atencion-bidireccional.md)] 

