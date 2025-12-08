## 🔬 El Truco de la Reparametrización en Detalle

El **Truco de la Reparametrización** es la innovación técnica que hizo posible entrenar los VAEs. Su necesidad surge del uso de la **Divergencia KL**, que penaliza la diferencia entre la distribución aprendida $q(\mathbf{z}|\mathbf{x})$ y la *a priori* $p(\mathbf{z})$.

### El Problema de la Muestra

En un VAE, el codificador produce los parámetros ($\boldsymbol{\mu}$ y $\boldsymbol{\sigma}$) de la distribución latente. Luego, el vector latente $\mathbf{z}$ se obtiene mediante un **muestreo estocástico** de esa distribución:

$$\mathbf{z} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\sigma}^2 \mathbf{I})$$

Durante el entrenamiento, el error de reconstrucción depende de este $\mathbf{z}$. Para que el algoritmo de **retropropagación (*backpropagation*)** funcione, el gradiente debe fluir de la pérdida de vuelta a los pesos del codificador, es decir, a $\boldsymbol{\mu}$ y $\boldsymbol{\sigma}$. Sin embargo, el **proceso de muestreo es no diferenciable**, rompiendo la cadena de gradientes.

### La Solución: Separar la Aleatoriedad

El truco consiste en reformular el muestreo de una variable aleatoria $\mathbf{z}$ con media $\boldsymbol{\mu}$ y desviación estándar $\boldsymbol{\sigma}$ en dos partes:

1.  **Una variable estocástica fija:** Un vector de ruido $\boldsymbol{\epsilon}$ que se muestrea de una **distribución conocida y fija** (la Gaussiana estándar, $\mathcal{N}(0, 1)$).
2.  **Una transformación determinista:** Una operación que convierte $\boldsymbol{\epsilon}$ en $\mathbf{z}$ utilizando los parámetros del codificador ($\boldsymbol{\mu}$ y $\boldsymbol{\sigma}$).

La nueva ecuación para $\mathbf{z}$ es:

$$\mathbf{z} = \boldsymbol{\mu} + \boldsymbol{\sigma} \odot \boldsymbol{\epsilon}$$



**Efecto en la Retropropagación:**

* **Ruta del Gradiente:** Ahora, los gradientes pueden fluir desde $\mathbf{z}$ a través de la multiplicación y suma hacia $\boldsymbol{\mu}$ y $\boldsymbol{\sigma}$.
* **Aislamiento:** La fuente de la aleatoriedad, $\boldsymbol{\epsilon}$, no tiene parámetros que necesiten ser aprendidos, por lo que el gradiente no necesita pasar a través del proceso de muestreo. Solo los parámetros deterministas ($\boldsymbol{\mu}$ y $\boldsymbol{\sigma}$) reciben gradientes, lo que permite el ajuste de los pesos del codificador para modificar la distribución latente.

---

## ⚔️ VAE vs. GAN: Comparativa de Modelos Generativos

Los **Autoencoders Variacionales (VAE)** y las **Redes Generativas Antagónicas (GAN)** son los dos modelos generativos más influyentes de la última década, pero difieren fundamentalmente en su objetivo, entrenamiento y resultados.

### VAE (Autoencoder Variacional)

| Característica | Descripción |
| :--- | :--- |
| **Fundamento** | Probabilístico/Bayesiano. Maximiza el Límite Inferior de la Evidencia (ELBO). |
| **Arquitectura** | Un solo modelo con dos partes: **Codificador** (Inferencia) y **Decodificador** (Generación). |
| **Entrenamiento** | **Directo** (entrenan el Codificador y el Decodificador simultáneamente). |
| **Pérdida Principal** | La pérdida de reconstrucción se optimiza junto con la Divergencia KL. |
| **Generación** | **Fácil y controlable.** El espacio latente $\mathbf{z}$ es continuo y estructurado (suave), facilitando la interpolación y manipulación de atributos. |
| **Calidad de Salida** | Tiende a producir resultados más **borrosos** o suavizados, ya que optimiza una pérdida de reconstrucción media basada en verosimilitud (minimiza la distancia a la distribución real). |
| **Propósito** | Aprendizaje de representaciones, inferencia, manipulación de atributos. |

### GAN (Red Generativa Antagónica)

| Característica | Descripción |
| :--- | :--- |
| **Fundamento** | Teoría de juegos y aprendizaje adversario (*Adversarial Learning*). |
| **Arquitectura** | Dos modelos en competencia: **Generador** ($G$) y **Discriminador** ($D$). |
| **Entrenamiento** | **Adversario** (minimax). El Generador intenta engañar al Discriminador; el Discriminador intenta distinguir las muestras reales de las falsas. |
| **Pérdida Principal** | El Generador se entrena en base al error del Discriminador. No hay término explícito de reconstrucción. |
| **Generación** | **Difícil de controlar.** El espacio latente suele ser más desordenado, lo que dificulta la interpolación lineal sin producir resultados inválidos. |
| **Calidad de Salida** | Tiende a generar resultados **más nítidos y realistas**, ya que el Discriminador castiga las salidas borrosas o no naturales. |
| **Propósito** | Generación de muestras fotorealistas, síntesis de imágenes. |

### Conclusión de la Comparativa

| Criterio | GAN | VAE |
| :--- | :--- | :--- |
| **Generación de Imagen de Alta Calidad** | $\text{GAN} \gg \text{VAE}$ |
| **Control sobre el Espacio Latente** | $\text{VAE} \gg \text{GAN}$ |
| **Estabilidad de Entrenamiento** | $\text{VAE} \gg \text{GAN}$ |
| **Inferencia (*Encoding*)** | $\text{VAE}$ lo permite intrínsecamente ($\mathbf{x} \to \mathbf{z}$); $\text{GAN}$ requiere un modelo de inferencia separado. |

En esencia, el VAE sacrifica una pequeña cantidad de calidad de salida a cambio de una **estructura latente más interpretable** y un **entrenamiento más estable**. El GAN, por otro lado, busca la excelencia visual a través de la competencia, lo que a menudo lo hace inestable y sin una forma nativa de mapear datos reales a códigos latentes.


---

Continua: [[3-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/3-2-redes-generativas-adversarias.md)] 
