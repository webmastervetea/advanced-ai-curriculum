# 📈 Procesos Gaussianos: Modelado de Funciones Caras y Ruidosas

Los **Procesos Gaussianos (*Gaussian Processes*, GPs)** son una herramienta estadística no paramétrica poderosa utilizada para modelar distribuciones sobre funciones. A diferencia de los modelos paramétricos que asumen una forma funcional fija (ej. lineal o polinomial) con un número finito de parámetros, un GP define una **distribución de probabilidad sobre el espacio de todas las posibles funciones**.

Esta capacidad única los hace indispensables en escenarios donde la función objetivo que se intenta optimizar o modelar es costosa de evaluar (por ejemplo, requiere experimentos físicos, simulaciones complejas, o el entrenamiento de un modelo de *Deep Learning*), o cuando las observaciones están contaminadas por ruido.

## 1. Fundamentos: Una Distribución sobre Funciones

Un Proceso Gaussiano se puede entender como una **extensión multivariada de la distribución Normal (Gaussiana)**, pero cuyas variables son los valores de la función en diferentes puntos.

### A. La Definición del GP

Un GP está completamente especificado por su **función de media ($\mu$)** y su **función de covarianza ($\mathbf{K}$)**.

1.  **Función de Media ($\mu(\mathbf{x})$):** Define el valor esperado de la función en cualquier punto $\mathbf{x}$. A menudo se asume que es cero ($\mu(\mathbf{x}) = 0$) por simplicidad.
2.  **Función de Covarianza (*Kernel*, $\mathbf{K}(\mathbf{x}_i, \mathbf{x}_j)$):** Es el componente crítico. Define la **similitud** entre dos puntos $\mathbf{x}_i$ y $\mathbf{x}_j$ en el espacio de entrada, y por lo tanto, la correlación entre los valores de la función en esos puntos.

> **Principio Clave:** Los puntos de entrada que son similares deberían tener valores de salida de la función similares (suaves) y, por lo tanto, altamente correlacionados.

El *kernel* más común es el **RBF (*Radial Basis Function*)** o *Squared Exponential*, que asume que la correlación disminuye exponencialmente con la distancia euclidiana entre los puntos.

### B. Inferencia: Creación del Modelo Predictivo

Dado un conjunto de datos observados ($\mathcal{D} = \{(\mathbf{x}_i, y_i)\}$), el GP calcula la distribución posterior de las funciones.

* Para cualquier nuevo punto $\mathbf{x}_{\star}$, el GP produce una **distribución Gaussiana** completa de posibles resultados.
* Esta distribución se caracteriza por una **predicción de media** (la predicción más probable, $\mu(\mathbf{x}_{\star})$) y una **varianza** (la incertidumbre sobre esa predicción, $\sigma^2(\mathbf{x}_{\star})$).



---

## 2. Aplicación Central: Optimización Bayesiana (BO)

El uso principal de los Procesos Gaussianos es como **modelo sustituto (*surrogate model*)** en la **Optimización Bayesiana (BO)**. La BO es una estrategia secuencial para optimizar funciones objetivo caras ($f(\mathbf{x})$).

El ciclo de la BO opera en dos pasos iterativos:

### A. 1. Modelo Sustituto (GP)

El GP se ajusta a los datos de las evaluaciones anteriores. Esto proporciona una **predicción de la media** y, crucialmente, una **estimación de la incertidumbre (varianza)** para todos los puntos del espacio de búsqueda.

### B. 2. Función de Adquisición (*Acquisition Function*)

La función de adquisición utiliza la media y la varianza del GP para decidir **dónde evaluar la función objetivo a continuación** (es decir, qué $\mathbf{x}$ probar).

La función de adquisición equilibra la **Exploración** (ir a regiones con alta incertidumbre, $\sigma^2$ alta) y la **Explotación** (ir a regiones con la mejor media prevista, $\mu$ alta).

* **Ejemplo Común:** La **Mejora Esperada (*Expected Improvement*, EI)**. Esta función mide la mejora esperada sobre el mejor valor conocido hasta ahora ($y_{\text{best}}$), sopesada por la incertidumbre.
* **Decisión:** Se elige el punto $\mathbf{x}_{\text{next}}$ que maximiza el valor de la función de adquisición.

Finalmente, la función objetivo real se evalúa en $\mathbf{x}_{\text{next}}$, y el nuevo par $(\mathbf{x}_{\text{next}}, y_{\text{next}})$ se añade al conjunto de datos $\mathcal{D}$, cerrando el ciclo.

## 3. Manejo de Funciones Ruidosas

Los Procesos Gaussianos manejan el ruido inherente a los datos de forma natural a través de su modelo probabilístico.

### A. Ruido como Parámetro

En un GP, el ruido se modela añadiendo un término de varianza ($\sigma_n^2$) al *kernel* en la diagonal de la matriz de covarianza.

$$K_{\text{ruido}}(\mathbf{x}_i, \mathbf{x}_j) = K(\mathbf{x}_i, \mathbf{x}_j) + \delta_{ij}\sigma_n^2$$

Donde $\delta_{ij}$ es la delta de Kronecker (igual a 1 si $i=j$, y 0 si $i \ne j$).

* **Efecto:** El GP no tiene que pasar exactamente por los puntos de datos ruidosos, lo que permite que la función de media se mantenga suave y represente la verdadera función subyacente, mientras que la varianza explica el ruido de la medición.

## 4. Conclusión

Los Procesos Gaussianos son una herramienta esencial para la optimización y el modelado de *Machine Learning* en entornos del mundo real. Al proporcionar no solo una predicción puntual, sino también una cuantificación de la incertidumbre, el GP permite a los algoritmos de Optimización Bayesiana tomar decisiones racionalmente informadas sobre el equilibrio entre exploración y explotación, lo que resulta en una convergencia eficiente y una reducción drástica del costo de las evaluaciones.


---

Continua: [[19-1-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/19-1-2-aplicacion-de-bo.md)] 
