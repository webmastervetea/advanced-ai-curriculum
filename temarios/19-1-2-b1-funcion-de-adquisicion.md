# 🎯 La Función de Adquisición: Mejora Esperada (EI)

La **Mejora Esperada (*Expected Improvement*, EI)** es la función de adquisición más popular y eficaz utilizada en la **Optimización Bayesiana (BO)**. Su propósito es guiar el proceso de búsqueda, determinando **cuál es el siguiente punto del espacio de hiperparámetros ($\mathbf{x}_{\text{next}}$) que se debe evaluar** en la función objetivo (el error de validación del modelo).

La fuerza de la EI radica en que cuantifica directamente, en términos de valor de la función objetivo, la **mejora potencial** que se puede obtener al probar un punto, incorporando la incertidumbre del **Proceso Gaussiano (GP)**.

## 1. El Principio de la Mejora

La **Mejora (*Improvement*, $I(\mathbf{x})$)** es una métrica simple que mide cuánto mejor es un posible resultado ($f(\mathbf{x})$) en un punto $\mathbf{x}$ que el mejor valor de función objetivo encontrado hasta ahora ($f_{\text{best}}$). Dado que la BO busca la minimización del error, la mejora se define como:

$$I(\mathbf{x}) = \max(0, f_{\text{best}} - f(\mathbf{x}))$$

Donde $f(\mathbf{x})$ es el valor de la función objetivo en el punto $\mathbf{x}$. Si el valor predicho es peor (más grande) que $f_{\text{best}}$, la mejora es cero. Si es mejor (más pequeño), la mejora es la diferencia.

## 2. El Paso a la Mejora Esperada (EI)

Dado que el GP nos proporciona una **distribución probabilística** sobre $f(\mathbf{x})$ (y no un valor único), no podemos simplemente usar la *Mejora* ($I(\mathbf{x})$). En su lugar, utilizamos la **Mejora Esperada (EI)**, que es el valor esperado de $I(\mathbf{x})$ sobre la distribución de predicción del GP.

La distribución de predicción del GP en el punto $\mathbf{x}$ es una normal: $f(\mathbf{x}) \sim \mathcal{N}(\mu(\mathbf{x}), \sigma^2(\mathbf{x}))$.

La fórmula de la Mejora Esperada es:

$$EI(\mathbf{x}) = \sigma(\mathbf{x}) \cdot (\phi(Z) + Z \cdot \Phi(Z))$$

Donde:
* $Z = \frac{f_{\text{best}} - \mu(\mathbf{x})}{\sigma(\mathbf{x})}$
* $\mu(\mathbf{x})$ y $\sigma(\mathbf{x})$ son la media y la desviación estándar de la predicción del GP en $\mathbf{x}$.
* $\phi(Z)$ es la función de densidad de probabilidad estándar normal.
* $\Phi(Z)$ es la función de distribución acumulativa estándar normal.

## 3. El Equilibrio Estratégico: Exploración vs. Explotación

La belleza de la fórmula de EI es que incorpora la incertidumbre ($\sigma(\mathbf{x})$) y la cercanía al óptimo conocido ($\mu(\mathbf{x})$) de manera equilibrada, lo que se traduce en una toma de decisiones inteligente:

### A. Explotación (Cuando $\sigma(\mathbf{x})$ es Baja)

Si la incertidumbre ($\sigma(\mathbf{x})$) es baja, el valor de $EI(\mathbf{x})$ estará dominado por el término $\mu(\mathbf{x})$.

* El EI será alto cerca de los puntos donde la **predicción de la media ($\mu$) es mucho mejor que $f_{\text{best}}$**.
* El algoritmo se enfoca en **explotar** regiones donde el GP está seguro de que la función objetivo tiene un valor excelente.

### B. Exploración (Cuando $\sigma(\mathbf{x})$ es Alta)

Si la incertidumbre ($\sigma(\mathbf{x})$) es alta, pero la predicción de la media ($\mu$) no es necesariamente la mejor, el gran valor de $\sigma(\mathbf{x})$ puede mantener el $EI(\mathbf{x})$ alto.

* Esto alienta al algoritmo a **explorar** regiones que aún no se han muestreado. Incluso si la media predicha no es fantástica, la alta incertidumbre implica que **podría haber un pico de mejora** oculto allí.



## 4. Un Ligerio Sesgo: La Mejora Esperada con Ruido

En la práctica, la fórmula se modifica ligeramente introduciendo un término $\epsilon$ (pequeño y positivo) para dar un sesgo sutil hacia la exploración:

$$I_{\epsilon}(\mathbf{x}) = \max(0, f_{\text{best}} - f(\mathbf{x}) - \epsilon)$$

Este término $\epsilon$ asegura que el algoritmo siempre tenga un incentivo para **explorar nuevos puntos**, en lugar de simplemente volver a probar un punto cercano al óptimo actual, incluso si el GP predice que no habrá una mejora masiva.

## 5. Conclusión

La **Mejora Esperada (EI)** proporciona un criterio robusto para la toma de decisiones secuencial en la Optimización Bayesiana. Al calcular la ganancia esperada de la evaluación de un punto, el EI permite que la BO supere la ineficiencia de la búsqueda ciega, convergiendo rápidamente al óptimo global de la función de error de validación con el menor número posible de costosos entrenamientos de modelos.


---

Continua: [[19-1-2-b2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/19-1-2-b2-funcion-adquisicion-mejora-esperada.md)] 
