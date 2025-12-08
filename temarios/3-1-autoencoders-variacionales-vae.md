# 🌌 Autoencoders Variacionales (VAE): Aprendizaje de Representaciones y Generación Probabilística

Los **Autoencoders Variacionales (VAE)** son una clase de modelos de aprendizaje profundo que combinan la estructura del **Autoencoder** tradicional con principios de la **Estadística Bayesiana** y el **Aprendizaje Variacional**. Su objetivo principal no es solo la compresión de datos y el aprendizaje de representaciones (como un autoencoder estándar), sino también la **generación de datos nuevos** mediante el aprendizaje de una distribución de probabilidad subyacente de los datos.

## 1. Contexto: De Autoencoder a VAE

### El Autoencoder Clásico

Un **Autoencoder** (*AE*) se compone de dos partes:

1.  **Codificador (*Encoder*):** Mapea un dato de entrada $\mathbf{x}$ (ej. una imagen) a un vector de baja dimensión $\mathbf{z}$ (la **representación latente** o **código**): $q(\mathbf{z}|\mathbf{x})$.
2.  **Decodificador (*Decoder*):** Reconstruye el dato de entrada $\mathbf{x}$ a partir del código latente $\mathbf{z}$: $p(\mathbf{x}|\mathbf{z})$.

El AE se entrena minimizando la **pérdida de reconstrucción** (ej. Error Cuadrático Medio) entre la entrada $\mathbf{x}$ y la salida reconstruida $\mathbf{x}'$. El problema es que el espacio latente $\mathbf{z}$ aprendido por un AE clásico puede ser **irregular** y tener "huecos". Al muestrear un punto $\mathbf{z}$ al azar, el decodificador podría generar una salida sin sentido.

### La Solución del VAE

El VAE resuelve este problema imponiendo una **estructura probabilística** al espacio latente. Obliga al codificador a mapear los datos de entrada a distribuciones que se asemejen a una **distribución a priori simple** (típicamente una distribución Gaussiana estándar o *normal* $\mathcal{N}(0, I)$). Esto asegura que el espacio latente sea **continuo** y **suave**, haciendo que el muestreo aleatorio sea coherente para la generación.



---

## 2. Arquitectura y Funcionamiento

A diferencia del AE, el **Codificador del VAE** no produce directamente el vector $\mathbf{z}$, sino los parámetros de la distribución de probabilidad que gobierna la representación latente.

### A. El Codificador (Inferencia)

El codificador, $q(\mathbf{z}|\mathbf{x})$, toma la entrada $\mathbf{x}$ y genera dos vectores:

1.  **Vector de Media ($\mu$):** Controla la posición central de la distribución latente.
2.  **Vector de Desviación Estándar ($\sigma$, o log-varianza $\log\sigma^2$):** Controla la dispersión o la "cantidad de ruido" permitida alrededor de la media.

El vector latente $\mathbf{z}$ se extrae (muestra) de esta distribución codificada:

$$\mathbf{z} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$$

### B. El Truco de la Reparametrización

Para entrenar la red usando *backpropagation*, necesitamos que el gradiente fluya desde la pérdida de reconstrucción de vuelta al codificador. Sin embargo, el **muestreo** es un proceso aleatorio no diferenciable.

El **Truco de la Reparametrización** (*Reparameterization Trick*) resuelve esto: en lugar de muestrear $\mathbf{z}$ directamente, reescribimos el proceso de muestreo como una función determinista de dos componentes:

$$\mathbf{z} = \boldsymbol{\mu} + \boldsymbol{\sigma} * \boldsymbol{\epsilon}$$

Donde:
* $\boldsymbol{\epsilon}$ es un vector de ruido muestreado de una Gaussiana estándar ($\mathcal{N}(0, I)$).
* $\boldsymbol{\sigma}$ es la desviación estándar (calculada a partir de la log-varianza del codificador).
* $*$ es la multiplicación elemento a elemento.

Al separar la aleatoriedad ($\boldsymbol{\epsilon}$) del proceso de aprendizaje ($\boldsymbol{\mu}, \boldsymbol{\sigma}$), el gradiente ahora puede fluir a través de los parámetros $\boldsymbol{\mu}$ y $\boldsymbol{\sigma}$ del codificador.

### C. El Decodificador (Generación)

El decodificador, $p(\mathbf{x}|\mathbf{z})$, es una red neuronal estándar que toma el vector latente muestreado $\mathbf{z}$ y lo mapea de vuelta al espacio de datos, produciendo la reconstrucción $\mathbf{x}'$.

---

## 3. La Función de Pérdida del VAE (ELBO)

La función de pérdida de un VAE se conoce como el **Límite Inferior de la Evidencia (Evidence Lower Bound, ELBO)**. Minimizar la pérdida del VAE es equivalente a maximizar el ELBO. La pérdida se compone de dos términos principales:

$$\mathcal{L}(\theta, \phi) = \text{Pérdida de Reconstrucción} + \text{Pérdida KL}$$

### A. Pérdida de Reconstrucción (Fidelidad)

Mide qué tan bien el decodificador reconstruye la entrada original. Es la forma estándar de pérdida del autoencoder, pero vista a través de una lente probabilística (como la esperanza negativa del logaritmo de la verosimilitud).

$$\text{Pérdida de Reconstrucción} = - \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z}|\mathbf{x})} [\log p(\mathbf{x}|\mathbf{z})]$$

* **Función:** Asegura que las salidas del decodificador sean similares a las entradas $\mathbf{x}$.

### B. Pérdida de Divergencia KL (Regularización)

Este término es exclusivo del VAE. Utiliza la **Divergencia de Kullback-Leibler ($\text{KL}$)** para medir la diferencia entre la distribución latente aprendida por el codificador $q(\mathbf{z}|\mathbf{x})$ y la distribución *a priori* deseada $p(\mathbf{z})$ (la Gaussiana estándar).

$$\text{Pérdida KL} = D_{KL}(q(\mathbf{z}|\mathbf{x}) || p(\mathbf{z}))$$

* **Función:** Actúa como un **regularizador**, obligando a las distribuciones latentes de las entradas a ser similares a la Gaussiana estándar $\mathcal{N}(0, I)$. Esto asegura que el espacio latente sea **suave, continuo y denso** alrededor del origen, garantizando que el muestreo aleatorio en esta área genere datos coherentes.

**El VAE es un balance:** Intenta minimizar la Pérdida de Reconstrucción (ser preciso) y al mismo tiempo minimizar la Pérdida KL (mantener el espacio latente ordenado y suave para la generación).

---

## 4. Generación de Nuevos Datos

Una vez que el VAE ha sido entrenado, se descarta el codificador. El decodificador se convierte en un **modelo generativo**.

El proceso de generación es simple:

1.  **Muestreo Latente:** Se muestrea un vector $\mathbf{z}$ al azar directamente de la distribución *a priori* (ej. $\mathbf{z} \sim \mathcal{N}(0, I)$).
2.  **Decodificación:** El vector $\mathbf{z}$ se alimenta al decodificador para producir un nuevo dato $\mathbf{x}_{\text{gen}}$.

Como el término KL ha obligado a que todas las representaciones latentes caigan en una región suave y conectada, cualquier punto $\mathbf{z}$ muestreado cerca de esta región debería ser decodificado en un dato válido (aunque posiblemente novedoso) que se asemeje a los datos de entrenamiento.

## 5. Aplicaciones Clave

* **Generación de Imágenes:** Los VAE fueron pioneros en la generación de imágenes de rostros, objetos y ropa, aunque posteriormente fueron superados en calidad visual por los GANs y los Modelos de Difusión.
* **Aprendizaje de Representaciones y Descomposición:** Pueden aprender representaciones latentes que capturan factores explicativos de los datos (ej. un eje del espacio latente puede corresponder al color del cabello, otro al ángulo de la cara).
* **Generación de Variantes y Manipulación de Atributos:** Permiten la manipulación semántica de los datos: moviéndose a lo largo de un "eje" latente específico, se puede cambiar un atributo (ej. hacer que una cara parezca más sonriente) sin alterar significativamente otros atributos.
* **Detección de Anomalías:** Los VAE son excelentes para detectar valores atípicos, ya que una entrada anómala tendrá una alta pérdida de reconstrucción y una distribución latente muy diferente a la Gaussiana $\mathcal{N}(0, I)$.

Los VAE establecieron un puente crucial entre los métodos probabilísticos y las arquitecturas de redes neuronales, sentando las bases para gran parte del trabajo moderno en modelos generativos.




---

Continua: [[3-1-b1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/3-1-b1-truco-reparametrizacion.md)] 
