# 🔭 Atención Bidireccional vs. Atención Enmascarada (Unidireccional)

La distinción clave entre las arquitecturas de Codificador (*Encoder-only*, como BERT) y las de Decodificador (*Decoder-only*, como GPT) reside en la forma en que el mecanismo de **Auto-Atención (*Self-Attention*)** accede al contexto de la secuencia.

## 1. Atención Bidireccional (BERT)

La Atención Bidireccional se utiliza en los modelos de tipo **Codificador** que están diseñados para la **comprensión** profunda del lenguaje.

### Mecanismo

Cuando BERT calcula el *embedding* contextual para un *token* en la posición $i$ de una frase, el mecanismo de Auto-Atención tiene acceso completo a **todos los demás *tokens*** en la secuencia, tanto los que están a su **izquierda** (los que preceden) como los que están a su **derecha** (los que siguen).

* **Acceso al Contexto:** El *token* $i$ puede "atender" y extraer información de los *tokens* $1, 2, \dots, i-1, i+1, \dots, N$ (donde $N$ es la longitud total de la secuencia).
* **Finalidad:** Esto permite que el modelo genere una representación vectorial para cada palabra que está informada por el contexto más rico y completo posible. Esta representación es ideal para tareas donde se necesita un entendimiento integral de la frase (ej. clasificar el sentimiento de una reseña completa).

### La Importancia del MLM

El entrenamiento de BERT con el objetivo **MLM (*Masked Language Model*)** refuerza esta bidireccionalidad. Al enmascarar una palabra y pedirle al modelo que la prediga, el modelo se ve obligado a utilizar el contexto de ambos lados para adivinar con precisión.

* **Ejemplo:** En la frase: "El **[MASK]** estaba caliente", BERT puede usar tanto "El" (izquierda) como "estaba caliente" (derecha) para inferir que la palabra enmascarada es probablemente un sustantivo singular que se puede calentar (ej. *sol*, *café* o *horno*).

## 2. Atención Enmascarada / Causal (GPT)

La Atención Enmascarada, también conocida como **Atención Causal**, se utiliza en los modelos de tipo **Decodificador** que están diseñados para la **generación secuencial** (auto-regresiva).

### Mecanismo

La atención causal impone una restricción artificial al mecanismo de Auto-Atención:

* **Enmascaramiento:** Cuando GPT genera el *token* en la posición $i$, la matriz de atención aplica una **máscara triangular superior** a la matriz de puntuaciones de atención. Esta máscara establece efectivamente las puntuaciones de atención en las posiciones $i+1, i+2, \dots, N$ (el contexto futuro) a cero o un valor negativo muy grande.
* **Acceso al Contexto:** El *token* $i$ solo puede atender a los *tokens* $1, 2, \dots, i-1$ (el contexto que lo precede). El modelo no tiene acceso a lo que va a generar a continuación.

* **Finalidad:** Esta restricción **imita el flujo natural del lenguaje humano** (hablamos o escribimos secuencialmente, sin saber la siguiente palabra). Es fundamental para garantizar que el modelo aprenda a generar texto de forma coherente. Si el modelo pudiera ver la respuesta completa, no aprendería a predecir el siguiente *token* basándose únicamente en el historial.



## 3. Resumen de la Diferencia

| Característica | Atención Bidireccional (BERT) | Atención Enmascarada (GPT) |
| :--- | :--- | :--- |
| **Arquitectura Principal** | Codificador (*Encoder*) | Decodificador (*Decoder*) |
| **Direccionalidad** | Bidireccional (Izquierda y Derecha) | Unidireccional / Causal (Solo Izquierda) |
| **Restricción** | No hay. Acceso libre a toda la secuencia. | Sí. Máscara triangular superior para bloquear el contexto futuro. |
| **Función Primaria** | **Comprensión** y extracción de características. | **Generación** secuencial y auto-regresiva. |
| **Ejemplo de Tarea** | Clasificación de texto. | Respuesta a *prompts*, generación de historias. |


---

Continua: [[5-3]()] 
