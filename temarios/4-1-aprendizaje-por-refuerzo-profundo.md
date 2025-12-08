# 🤖 Aprendizaje por Refuerzo Profundo (Deep RL): Uniendo Percepción y Decisión

El **Aprendizaje por Refuerzo Profundo (Deep RL)** es un marco de la Inteligencia Artificial que permite a los agentes (el modelo de IA) aprender a tomar decisiones en entornos complejos. Combina la capacidad de toma de decisiones del **Aprendizaje por Refuerzo (RL)** con las potentes capacidades de extracción de características de las **Redes Neuronales Profundas** (Deep Learning).

El objetivo del agente es maximizar una **recompensa acumulada** (retorno) a largo plazo mediante la interacción con el entorno (probando acciones) y recibiendo retroalimentación (recompensas).

## 1. Deep Q-Networks (DQN): El Origen del Deep RL

Las **Deep Q-Networks (DQN)**, introducidas por DeepMind en 2015 para dominar juegos de Atari, marcan el inicio de la era moderna del Deep RL. El DQN es un algoritmo de **aprendizaje basado en valores** (*Value-Based Learning*) que utiliza una red neuronal para aproximar la **función Q** (función de valor-acción).

### A. La Función Q y su Aproximación

En RL, la **Función Q** ($Q(s, a)$) estima el retorno esperado (recompensa acumulada descontada) que se obtendrá al tomar una acción $a$ en un estado $s$ y seguir una política óptima a partir de entonces.

El DQN utiliza una Red Neuronal Profunda (a menudo una CNN para entradas de imagen) para aproximar esta función:

$$Q(s, a; \theta) \approx Q^*(s, a)$$

Donde $\theta$ son los pesos de la red. En lugar de almacenar una tabla Q para cada par estado-acción (inviable en entornos grandes o continuos), la red neuronal aprende a generalizar los valores Q a partir de un número infinito de estados.

### B. Técnicas Clave para la Estabilidad

El entrenamiento de una red neuronal para predecir un valor Q objetivo que también está cambiando (el "problema de la bota" o *bootstrapping*) es inherentemente inestable. El DQN utiliza dos técnicas cruciales para estabilizar el proceso:

1.  **Memoria de Experiencia (*Experience Replay*):** El agente almacena las transiciones de experiencia ($\langle s_t, a_t, r_t, s_{t+1} \rangle$) en un *buffer* de memoria. Durante el entrenamiento, se muestrean mini-lotes aleatorios de este *buffer*. Esto tiene dos ventajas:
    * **Rompe la Correlación Temporal:** Los datos secuenciales consecutivos son altamente correlacionados, lo que dificulta la convergencia. El muestreo aleatorio convierte el problema en algo más parecido al aprendizaje supervisado.
    * **Reutiliza la Experiencia:** Cada experiencia se utiliza para múltiples actualizaciones de los pesos.

2.  **Red Objetivo Fija (*Fixed Target Network*):** Se utiliza una segunda red, la **Red Objetivo** ($Q_{\text{target}}$), con pesos $\theta^{-}$ que se copian de la red principal ($\theta$) solo después de un número fijo de pasos.
    * **Función de Pérdida:** La red principal ($\theta$) se entrena para minimizar la diferencia entre su predicción de valor Q y el **Valor Objetivo de Bellman** (calculado usando la Red Objetivo $\theta^{-}$):

$$\mathcal{L}(\theta) = \mathbb{E}_{\langle s, a, r, s' \rangle \sim U(\mathcal{D})} \left[ \left( r + \gamma \max_{a'} Q_{\text{target}}(s', a'; \theta^-) - Q(s, a; \theta) \right)^2 \right]$$

Esta separación temporal proporciona un objetivo estable durante la actualización, permitiendo que la red principal converja.

---

## 2. Policy Gradients: Aprendizaje Basado en la Política

A diferencia de los métodos basados en valores (DQN), que aprenden la bondad de los estados, los métodos de **Gradiente de Política (*Policy Gradients*)** aprenden directamente una **Política** ($\pi(a|s)$). La política es una función que mapea estados a acciones (una distribución de probabilidad sobre acciones).

El objetivo es ajustar los parámetros de la política ($\theta$) para **aumentar la probabilidad de tomar acciones que conduzcan a una alta recompensa** a largo plazo.

$$\nabla J(\theta) = \mathbb{E}_{\tau \sim \pi_\theta} \left[ \sum_{t=0}^T \nabla \log \pi_\theta(a_t | s_t) A_t \right]$$

Donde $\nabla J(\theta)$ es el gradiente de la función de recompensa $J(\theta)$, y $A_t$ es la **Ventaja** (o alguna aproximación del valor esperado de la recompensa).

### Ventajas de los Policy Gradients

* **Entornos Estocásticos:** Funcionan bien en entornos donde las acciones son probabilísticas.
* **Espacios de Acción Continuos:** Pueden manejar espacios de acción continuos (ej. mover un motor en un ángulo de $x$ grados), algo difícil para DQN.

### Actor-Critic (AC): Combinando Valor y Política

Los métodos **Actor-Critic (AC)** combinan los beneficios de los métodos basados en valores y los basados en políticas. Utilizan dos redes neuronales:

1.  **Actor ($\pi$):** La red de política. Decide qué acción tomar. Se actualiza utilizando el gradiente de política.
2.  **Crítico ($V$):** La red de valor. Estima la función de valor ($V(s)$), prediciendo la recompensa esperada de un estado. Esta predicción se utiliza para guiar la actualización del Actor.

El Crítico se utiliza para calcular la **Ventaja** ($A_t$), que mide qué tan buena fue la acción tomada en el estado actual en comparación con lo que se esperaba:
$$A_t = r_t + \gamma V(s_{t+1}) - V(s_t)$$

---

## 3. A3C y A2C: Paralelismo y Sincronización

Los algoritmos **Asynchronous Advantage Actor-Critic (A3C)** y **Advantage Actor-Critic (A2C)** son implementaciones populares de la arquitectura Actor-Critic.

### A. A3C (Asynchronous Advantage Actor-Critic)

A3C, desarrollado también por DeepMind, fue pionero en el uso de la **computación paralela asíncrona** en Deep RL.

* **Paralelismo Asíncrono:** Múltiples agentes (o *workers*) ejecutan copias del entorno simultáneamente en diferentes hilos de la CPU. Cada agente interactúa con su propia copia del entorno y calcula sus propios gradientes (usando el método Actor-Critic).
* **Actualización Asíncrona:** Estos agentes envían sus gradientes de forma **asíncrona** a un modelo maestro central, que actualiza los pesos de forma global.

**Ventaja:** Rompe la correlación temporal de manera natural (ya que cada agente experimenta diferentes trayectorias simultáneamente) y permite una exploración de estado mucho más amplia y rápida.

### B. A2C (Advantage Actor-Critic)

A2C es una versión **sincrónica** de A3C que se considera generalmente superior en entornos con GPUs modernas.

* **Paralelismo Sincrónico:** En lugar de que los agentes actualicen el modelo central de manera asíncrona (lo que puede llevar a gradientes desactualizados), los *workers* esperan al final de cada episodio o después de un número fijo de pasos.
* **Actualización Sincrónica:** Todos los agentes recopilan sus experiencias y gradientes. Luego, el modelo central calcula la actualización **una sola vez** utilizando todos los gradientes agregados, y luego distribuye los nuevos pesos a todos los *workers*.

**Ventaja:** Utiliza mejor el hardware moderno (especialmente las GPUs), garantiza que todas las actualizaciones se basen en el último modelo, y ofrece un entrenamiento más eficiente y a menudo más estable que A3C.

---

## 4. Conclusión

El Deep RL ha permitido que los agentes aprendan políticas óptimas en entornos de alta dimensión y complejidad.

* **DQN:** Sentó las bases al permitir que el RL basado en valores escalara a grandes entradas (imágenes) utilizando *Experience Replay* y la *Red Objetivo Fija*. Es ideal para problemas con espacios de acción discretos.
* **Policy Gradients (A2C/A3C):** Se centran en aprender directamente la política, haciéndolos ideales para entornos con espacios de acción continuos y beneficiándose enormemente de las arquitecturas Actor-Critic y el paralelismo.

La combinación de estas técnicas ha llevado a avances en robótica, juegos complejos y sistemas de control autónomo.


---

Continua: [[4-1-b1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/4-1-b1-ejemplo-practico.md)] 
