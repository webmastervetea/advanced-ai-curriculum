# 🖼️ Modelos de Difusión: De Ruido al Arte Fotorrealista

Los **Modelos de Difusión** son una clase de modelos generativos que han revolucionado la síntesis de imágenes en la última década (siendo los modelos **DALL-E 2**, **Midjourney** y **Stable Diffusion** ejemplos notables). Su marco teórico se basa en procesos estocásticos de la termodinámica fuera del equilibrio, específicamente en la idea de que podemos **modelar el proceso de generación como el inverso del proceso de difusión (o ruido)**.

## 1. El Fundamento: Dos Procesos Clave

Un Modelo de Difusión se entrena para aprender la estructura subyacente de los datos a través de la simulación de dos procesos opuestos:

### A. El Proceso de Difusión (Forward Process)

Este proceso es **fijo** y **conocido** (no se aprende). Se aplica repetidamente a una imagen real $\mathbf{x}_0$.

1.  **Adición de Ruido:** En cada paso de tiempo discreto $t$ (desde $t=1$ hasta $T$, donde $T$ es grande, típicamente 1000), se añade una pequeña cantidad controlada de **ruido gaussiano** a la imagen.
2.  **Destrucción Gradual:** Con el tiempo, la imagen original $\mathbf{x}_0$ se transforma progresivamente en una imagen completamente aleatoria $\mathbf{x}_T$, que es esencialmente **ruido blanco puro**.

Este proceso se puede describir como una **cadena de Markov**:

$$q(\mathbf{x}_t | \mathbf{x}_{t-1}) = \mathcal{N}(\mathbf{x}_t; \sqrt{1 - \beta_t} \mathbf{x}_{t-1}, \beta_t \mathbf{I})$$

Donde $\beta_t$ controla la cantidad de ruido añadido en el paso $t$. La clave es que el ruido se añade de forma que la distribución de la imagen en cualquier paso $t$ se puede calcular directamente a partir de la imagen original $\mathbf{x}_0$.



### B. El Proceso de Reversa (Reverse Process)

Este proceso es **aprendido** por la red neuronal y es el corazón de la generación. El objetivo es entrenar la red para revertir el proceso de difusión:

1.  **Eliminación del Ruido:** La red neuronal (que suele ser una arquitectura **U-Net**) debe aprender a predecir y eliminar el ruido $\boldsymbol{\epsilon}$ que se añadió en cada paso $t$.
2.  **Reconstrucción:** Partiendo del ruido puro $\mathbf{x}_T$, el modelo utiliza esta predicción para dar un pequeño paso hacia atrás y obtener $\mathbf{x}_{T-1}$, luego $\mathbf{x}_{T-2}$, y así sucesivamente, hasta llegar a la imagen limpia $\mathbf{x}_0$.

$$p_\theta(\mathbf{x}_{t-1} | \mathbf{x}_t) = \mathcal{N}(\mathbf{x}_{t-1}; \boldsymbol{\mu}_\theta(\mathbf{x}_t, t), \boldsymbol{\Sigma}_\theta(\mathbf{x}_t, t))$$

Aquí, $\boldsymbol{\mu}_\theta$ y $\boldsymbol{\Sigma}_\theta$ son la media y la covarianza que son aprendidas por la red (parametrizadas por $\theta$).

---

## 2. La Arquitectura del Modelo de Difusión

### La Red Predictora de Ruido (U-Net)

El modelo de Difusión no predice la imagen limpia directamente, sino que predice el **ruido puro** que se necesita restar en cada paso.

La red elegida para esta tarea es, casi universalmente, una **U-Net**.

* **U-Net:** Es una red convolucional con una estructura simétrica en forma de 'U'. Tiene un camino de **Codificación** (que comprime la información) y un camino de **Decodificación** (que reconstruye la imagen). Crucialmente, utiliza **conexiones de salto (*skip connections*)** que conectan las capas correspondientes del codificador y el decodificador, permitiendo que la información de baja resolución (ubicación y forma) se combine con la información de alta resolución (detalle fino).
* **Entrada y Salida:** En el paso $t$, la U-Net toma la imagen ruidosa $\mathbf{x}_t$ como entrada y el **paso de tiempo $t$** (como una codificación posicional) y produce una predicción del ruido $\boldsymbol{\epsilon}_\theta(\mathbf{x}_t, t)$ que se añadió.

### La Función de Pérdida

El entrenamiento es sorprendentemente simple y eficiente. Se minimiza la diferencia (típicamente el error cuadrático medio, L2) entre el ruido real $\boldsymbol{\epsilon}$ y el ruido predicho $\boldsymbol{\epsilon}_\theta$:

$$\mathcal{L}(\theta) = \mathbb{E}_{t, \mathbf{x}_0, \boldsymbol{\epsilon}} [ \| \boldsymbol{\epsilon} - \boldsymbol{\epsilon}_\theta(\sqrt{\bar{\alpha}_t}\mathbf{x}_0 + \sqrt{1 - \bar{\alpha}_t}\boldsymbol{\epsilon}, t) \|^2 ]$$

Donde $\mathbf{x}_t$ se calcula directamente a partir de $\mathbf{x}_0$ y el ruido $\boldsymbol{\epsilon}$. El modelo aprende a ser un **reductor de ruido** perfecto en cada paso del proceso.

---

## 3. Difusión Condicional (*Conditional Diffusion*)

El verdadero avance que llevó a la generación de imágenes moderna es la capacidad de **condicionar** el proceso de generación a una entrada específica, como un texto.

### El Desafío del Texto a Imagen

Para que la U-Net genere una imagen que coincida con el *prompt* ("Un perro con gafas de sol sobre una patineta"), la información textual debe guiar el proceso de eliminación de ruido en cada uno de los miles de pasos de tiempo.

### Integración de Transformadores y Condicionamiento

Esto se logra utilizando **Modelos de Transformadores** (como se vio en el artículo anterior):

1.  **Codificación del *Prompt*:** El texto de entrada (el *prompt*) se alimenta a un **Codificador de Transformador** (típicamente una versión de CLIP o BERT). Este Transformador convierte el texto en una representación densa y contextualizada (un conjunto de *embeddings*).
2.  **Inyección en la U-Net:** Estos *embeddings* de texto se inyectan en las capas de la U-Net predictora de ruido, generalmente a través de capas de **Atención Cruzada (*Cross-Attention*)**.



El mecanismo de Atención Cruzada permite que la U-Net evalúe la importancia de cada *token* del *prompt* codificado al predecir el ruido en una región específica de la imagen. La red aprende, en el paso $t$, a eliminar el ruido de manera que la imagen resultante sea coherente tanto con la estructura de la imagen anterior como con el significado del texto.

---

## 4. Modelos de Difusión Latente (Latent Diffusion Models - LDM)

El principal problema de los primeros Modelos de Difusión era la **velocidad y el costo computacional**. Trabajar directamente en el espacio de píxeles de imágenes de alta resolución (ej. $512 \times 512$ o $1024 \times 1024$) era prohibitivamente caro.

El **Modelo de Difusión Latente (LDM)**, la base de **Stable Diffusion**, resuelve esto:

1.  **Compresión de Espacio:** En lugar de realizar la difusión en el espacio de píxeles de la imagen, se utiliza un **Autoencoder Variacional (VAE)** pre-entrenado para comprimir la imagen de alta resolución en un **espacio latente** mucho más pequeño.
2.  **Difusión en el Latente:** La adición y eliminación de ruido ocurre completamente en este **espacio latente de baja dimensión**. Esto reduce drásticamente el costo computacional.
3.  **Decodificación Final:** Una vez que el proceso de reversa ha generado una representación latente limpia, se utiliza el **Decodificador VAE** para mapearla de vuelta al espacio de píxeles de alta resolución.

Esto hace que la generación sea mucho más rápida y eficiente en términos de memoria, permitiendo que la tecnología sea accesible a una base de usuarios más amplia.

## 5. Ventajas sobre GANs y VAEs

| Criterio | GANs | VAEs | Modelos de Difusión (DMs) |
| :--- | :--- | :--- | :--- |
| **Estabilidad de Entrenamiento** | Inestable (Juego Adversario). | Muy Estable (Pérdida de Reconstrucción + KL). | **Muy Estable** (Pérdida L2 simple y determinista). |
| **Calidad de Salida** | Excelente, pero puede tener artefactos (Modos Colapsados). | Borrosa, baja fidelidad. | **Fotorrealismo y coherencia superiores.** |
| **Diversidad** | Puede sufrir de **Colapso de Modos**. | Alta. | **Alta** (Explora bien la distribución de datos). |
| **Inferencia/Generación** | Muy rápida (un solo paso). | Muy rápida (un solo paso). | **Lenta** (requiere cientos o miles de pasos secuenciales). |

Los Modelos de Difusión combinan la **estabilidad** del entrenamiento de los VAEs con la capacidad de generar **imágenes de alta fidelidad y nitidez** de las GANs. Su éxito se basa en descomponer una tarea compleja (generar una imagen) en una secuencia de tareas simples (eliminar pequeñas cantidades de ruido).
