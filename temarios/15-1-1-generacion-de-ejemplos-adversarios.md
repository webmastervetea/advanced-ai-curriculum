# 👻 Ejemplos Adversarios: La Vulnerabilidad Oculta de los Modelos de Clasificación

Los **Ejemplos Adversarios (*Adversarial Examples*)** son *inputs* diseñados maliciosamente que se parecen casi idénticos a los datos legítimos para un observador humano, pero que provocan que un modelo de *Machine Learning* (especialmente las Redes Neuronales Profundas, DNNs) haga una **clasificación errónea y específica** con alta confianza.

El estudio de estos ataques es fundamental para la **Seguridad de la Inteligencia Artificial (AI Security)**, ya que revelan vulnerabilidades fundamentales en el aprendizaje profundo y tienen implicaciones críticas en sistemas de alto riesgo (como reconocimiento facial, detección de malware y vehículos autónomos).

## 1. El Fenómeno Adversario

El ataque funciona al explotar la **linealidad** y la **alta dimensión** de las redes neuronales.

* **Mecanismo:** Un pequeño, pero intencional, **ruido imperceptible** se añade al *input* original ($\mathbf{x}$). Este ruido ($\mathbf{\eta}$) está diseñado para explotar los límites de decisión del modelo.
* **Resultado:** El nuevo *input* perturbado ($\mathbf{x}_{\text{adv}} = \mathbf{x} + \mathbf{\eta}$) se clasifica erróneamente.
* **Ejemplo Clásico:** Una imagen de un **Panda** se altera con un ruido minúsculo, lo que hace que la red la clasifique con alta confianza como un **Gibón**. 

---

## 2. Ataques White-Box (Caja Blanca)

En los ataques **White-Box**, el atacante tiene un **conocimiento completo** sobre el modelo objetivo: su arquitectura, sus parámetros (pesos y sesgos) y los gradientes del modelo. Este conocimiento permite al atacante calcular el vector de ruido ($\mathbf{\eta}$) de forma extremadamente precisa.

### A. Método de la Señal de Gradiente Rápida (FGSM)

El **Fast Gradient Sign Method (FGSM)** es uno de los ataques *White-Box* más antiguos y eficientes.

* **Mecanismo:** El atacante calcula el **gradiente** de la función de pérdida del modelo con respecto a la entrada de la imagen original. Este gradiente indica en qué dirección el *input* debe moverse para maximizar la pérdida (es decir, maximizar la probabilidad de que el modelo se equivoque).
* **Fórmula:** La perturbación $\mathbf{\eta}$ se calcula como:
$$\mathbf{\eta} = \epsilon \cdot \text{sign}(\nabla_{\mathbf{x}} J(\mathbf{\theta}, \mathbf{x}, y))$$
Donde $J$ es la función de pérdida, $\nabla_{\mathbf{x}}$ es el gradiente con respecto a la entrada $\mathbf{x}$, $\epsilon$ es la magnitud de la perturbación (controla qué tan invisible es el ruido) y $\text{sign}$ toma el signo del gradiente.
* **Ventaja:** Es muy rápido, ya que solo requiere un paso de *backpropagation*.

### B. Ataque Iterativo (PGD)

El ataque **Projected Gradient Descent (PGD)** es una versión más fuerte de FGSM que utiliza múltiples iteraciones y una proyección para refinar el ruido. Se considera el punto de referencia de los ataques *White-Box* en la investigación.

---

## 3. Ataques Black-Box (Caja Negra)

En los ataques **Black-Box**, el atacante **no tiene acceso** a los pesos, la arquitectura o los gradientes internos del modelo. El atacante solo puede interactuar con el modelo a través de la interfaz pública, enviando *inputs* y recibiendo *outputs* (predicciones).

Estos ataques son mucho más realistas en el mundo real.

### A. Ataques Basados en Transferibilidad

Los ataques *Black-Box* explotan una propiedad clave de los modelos adversarios: la **transferibilidad**.

* **Mecanismo:** El atacante entrena un **Modelo Sustituto (*Surrogate Model*)** que intenta imitar el comportamiento del modelo objetivo *Black-Box*. Luego, el atacante genera Ejemplos Adversarios *White-Box* usando el Modelo Sustituto (ej. usando FGSM).
* **Transferencia:** El atacante observa que, sorprendentemente, los Ejemplos Adversarios creados para el Modelo Sustituto a menudo son efectivos para engañar al modelo objetivo *Black-Box*. Esto sucede porque las redes neuronales profundas tienden a aprender funciones de decisión con límites de decisión similares.

### B. Ataques Basados en la Consulta (*Query-Based*)

Estos ataques intentan estimar el gradiente o la información relevante del modelo *Black-Box* mediante la realización de múltiples consultas.

* **Mecanismo:** El atacante realiza muchas pequeñas perturbaciones aleatorias en la imagen de entrada y observa cómo cambia la salida del modelo. Esto le permite estimar el gradiente del modelo **sin acceso directo** a su estructura interna.
* **Costo:** Estos ataques son muy efectivos, pero son costosos en términos de **número de consultas** que deben hacerse.

---

## 4. Implicaciones y Defensas

### A. Implicaciones Críticas

* **Seguridad:** Un atacante puede crear una pegatina invisible para una señal de tráfico que haga que un coche autónomo la clasifique como "Límite de Velocidad 100", lo que demuestra el peligro en la toma de decisiones críticas.
* **Inyección de Código:** Los Ejemplos Adversarios se han utilizado para modificar *inputs* de texto de manera imperceptible para evitar filtros de contenido o inyectar comandos maliciosos.

### B. Defensas (Entrenamiento Adversario)

La principal defensa contra estos ataques es el **Entrenamiento Adversario (*Adversarial Training*)**.

* **Mecanismo:** El modelo se entrena iterativamente con sus propios **Ejemplos Adversarios generados en cada época**. El objetivo es forzar al modelo a clasificar correctamente no solo los datos originales, sino también sus versiones perturbadas.
* **Resultado:** Esto suaviza los límites de decisión del modelo en el vecindario del dato, haciendo que la red sea más robusta y menos sensible a las pequeñas perturbaciones.

La lucha entre los ataques adversarios y las defensas es un campo activo y esencial que impulsa la investigación hacia modelos de IA más robustos y confiables.
