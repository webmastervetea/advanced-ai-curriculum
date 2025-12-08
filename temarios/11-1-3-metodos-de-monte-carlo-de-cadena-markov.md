# 📊 MCMC y Variational Inference: La Aproximación de la Distribución Posterior

El objetivo central del **Aprendizaje Bayesiano** es calcular la **distribución posterior** $P(\mathbf{\theta}|\mathcal{D})$, que representa la probabilidad de los parámetros del modelo ($\mathbf{\theta}$) dadas las observaciones ($\mathcal{D}$). Esta distribución es clave para la cuantificación de la incertidumbre y la toma de decisiones.

Sin embargo, para modelos complejos (como las redes neuronales profundas o modelos con muchas variables), calcular o muestrear directamente la distribución posterior es **analíticamente intratable** e **intratable computacionalmente**. Aquí es donde entran en juego los métodos de aproximación: **MCMC** y **Inferencia Variacional (VI)**.

## 1. Métodos de Monte Carlo de Cadena de Markov (MCMC)

**MCMC** es una clase de algoritmos que generan una secuencia de muestras dependientes (una **cadena de Markov**) de una distribución objetivo. A medida que la cadena se ejecuta por un tiempo suficiente, la distribución empírica de las muestras converge a la distribución posterior verdadera $P(\mathbf{\theta}|\mathcal{D})$.

### A. Mecanismo Fundamental: El Paseo Aleatorio

1.  **Cadena de Markov:** Es un proceso estocástico donde la probabilidad de moverse a un nuevo estado ($\mathbf{\theta}_{t+1}$) depende únicamente del estado actual ($\mathbf{\theta}_t$).
2.  **Muestreo:** El algoritmo está diseñado para que la distribución de equilibrio (el estado estable) de la cadena de Markov sea exactamente la distribución posterior objetivo.
3.  **Algoritmo de Metropolis-Hastings (MH):** Es el MCMC más famoso. En cada paso:
    * Se propone un nuevo estado candidato $\mathbf{\theta}'$.
    * Se acepta o rechaza $\mathbf{\theta}'$ basándose en la razón de la densidad de probabilidad del estado candidato y el estado actual. Esto garantiza que la cadena se mueva hacia regiones de alta probabilidad.
4.  **Gibbs Sampling:** Un caso especial de MH utilizado cuando los parámetros pueden agruparse y muestrearse individualmente de sus distribuciones condicionales (a menudo más simples).

### B. Ventajas y Desventajas de MCMC

| Aspecto | Ventajas | Desventajas |
| :--- | :--- | :--- |
| **Precisión** | Proporciona muestras que, en el límite, son **exactas** (no sesgadas) de la distribución posterior verdadera. | **Lentitud y Escalabilidad:** Es muy lento para grandes conjuntos de datos y modelos con millones de parámetros (ej. *Deep Learning*). |
| **Diagnóstico** | Requiere un largo periodo de **quemado (*burn-in*)** para que la cadena converja a la distribución. | La convergencia es difícil de diagnosticar formalmente. |
| **Robustez** | Es robusto a la forma compleja de la distribución posterior (ej. distribuciones multimodales). | **Correlación:** Las muestras consecutivas están correlacionadas, lo que requiere más muestras para lograr una precisión equivalente a muestras independientes. |

---

## 2. Inferencia Variacional (VI)

La **Inferencia Variacional (VI)** reformula el problema de la inferencia Bayesiana como un **problema de optimización**. En lugar de muestrear, VI busca encontrar la distribución más cercana a la posterior verdadera dentro de una clase de distribuciones sencillas y manejables.

### A. Mecanismo Fundamental: Minimización de la Distancia

1.  **Distribución Aproximada ($q$):** Se elige una distribución de aproximación simple, $q(\mathbf{\theta}|\mathbf{\lambda})$, gobernada por un conjunto de **parámetros variacionales** ($\mathbf{\lambda}$). Típicamente, $q$ es una distribución factorizada (ej. Gaussianas independientes) para simplificar los cálculos.
2.  **Distancia (Divergencia KL):** El objetivo es encontrar los parámetros $\mathbf{\lambda}$ que minimicen la **Divergencia de Kullback-Leibler (KL)** entre $q$ y la posterior verdadera $P(\mathbf{\theta}|\mathcal{D})$:

$$\mathbf{\lambda}^* = \arg \min_{\mathbf{\lambda}} \text{KL}[q(\mathbf{\theta}|\mathbf{\lambda}) || P(\mathbf{\theta}|\mathcal{D})]$$

3.  **ELBO (Evidence Lower Bound):** Como la posterior $P(\mathbf{\theta}|\mathcal{D})$ es intratable, la minimización de $\text{KL}[q || P]$ es matemáticamente equivalente a **maximizar el Límite Inferior de la Evidencia (ELBO)**. La función ELBO es diferenciable y, por lo tanto, puede optimizarse mediante **Descenso de Gradiente**.



### B. Ventajas y Desventajas de VI

| Aspecto | Ventajas | Desventajas |
| :--- | :--- | :--- |
| **Velocidad y Escalabilidad** | Convierte la inferencia en un problema de optimización, lo que permite el uso de SGD. Es **mucho más rápido** que MCMC y puede escalar a modelos de *Deep Learning* con millones de parámetros. | **Sesgo:** La aproximación $q$ introduce un sesgo. VI no converge a la distribución verdadera, sino a la mejor aproximación dentro de la clase de $q$. |
| **Facilidad de Uso** | Compatible con *frameworks* de *Deep Learning* y diferenciación automática (Programación Diferenciable). | **Inflexibilidad:** La elección de la familia de distribuciones $q$ (ej. independencia entre parámetros) puede ser una simplificación demasiado fuerte. |
| **Convergencia** | La convergencia se diagnostica fácilmente mediante el monitoreo del ELBO. | Tiende a subestimar la varianza de la posterior verdadera (por la naturaleza de la KL). |

---

## 3. Elección y Tendencias Modernas

La elección entre MCMC y VI depende del contexto:

* **MCMC es preferido** cuando la precisión absoluta en la aproximación de la distribución es primordial (ej. investigación teórica, problemas de baja dimensión).
* **VI es el estándar de facto** en el **Aprendizaje Profundo Bayesiano (BDL)** debido a sus requisitos de escalabilidad y velocidad.

Las tendencias modernas, como el **Hamiltonian Monte Carlo (HMC)** (una versión de MCMC que utiliza gradientes) y el **MCMC No Convergente** (MCMC ejecutado por un número limitado de pasos para mejorar la calidad de las muestras de VI), buscan fusionar las ventajas de ambos, logrando un equilibrio entre precisión y velocidad.


---

Continua: [[11-2-1]()] 
