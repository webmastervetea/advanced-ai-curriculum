## 🐦 5. Ejemplo Práctico: DQN y Flappy Bird

**Flappy Bird** es un entorno ideal para ilustrar el funcionamiento de DQN porque tiene un **espacio de estados de alta dimensión** (debido a las imágenes de la pantalla), pero un **espacio de acciones discreto** muy simple (solo dos acciones posibles: **saltar** o **no saltar**).

### A. Definición del Entorno para DQN

1.  **Estado ($s$):** En lugar de la imagen completa de la pantalla (lo que ralentizaría el entrenamiento), el estado se define generalmente como un vector de características cruciales o una pila de los últimos 4 *frames* procesados. Las características clave pueden incluir:
    * Posición vertical del pájaro.
    * Velocidad vertical del pájaro.
    * Distancia horizontal al siguiente tubo.
    * Altura del hueco del siguiente tubo.
2.  **Acción ($a$):**
    * $a=0$: No saltar (caer).
    * $a=1$: Saltar (*Flap*).
3.  **Recompensa ($r$):**
    * $r=+1$: El pájaro pasa con éxito un tubo.
    * $r=+0.1$: El pájaro sobrevive un paso de tiempo.
    * $r=-1$ o $-10$: El pájaro choca contra un tubo o toca el suelo.

### B. La Red Neuronal (DQN)

La red DQN es una red neuronal que toma el **estado $s$** como entrada y produce **dos valores Q** como salida (uno para cada acción: $Q(s, \text{saltar})$ y $Q(s, \text{no saltar})$).

* **Arquitectura:** Para las características extraídas, se utilizarían capas *fully connected*. Si se utiliza una pila de *frames* brutos (imágenes), la arquitectura sería una **CNN** (Red Neuronal Convolucional) seguida de capas densas.
* **Decisión de Acción:** El agente sigue una política $\epsilon$-greedy. Con probabilidad $\epsilon$, elige una acción al azar (exploración). Con probabilidad $1-\epsilon$, elige la acción que maximiza el valor Q: $a = \arg\max_a Q(s, a; \theta)$.

### C. Aprendizaje

El agente juega millones de partidas, almacenando cada transición ($\langle s, a, r, s' \rangle$) en el **Buffer de Memoria de Experiencia**.

1.  **Muestreo:** Se muestrean mini-lotes aleatorios del *buffer*.
2.  **Cálculo del Objetivo:** Se calcula el valor objetivo $y$:
    $$y = r + \gamma \max_{a'} Q_{\text{target}}(s', a'; \theta^-)$$
3.  **Actualización:** La red principal $Q(s, a; \theta)$ se actualiza para que su valor predicho se acerque a $y$.

De esta manera, el DQN aprende que, si la distancia al tubo es corta y la posición es baja, la acción de "saltar" tiene un valor $Q$ mucho más alto que la acción de "no saltar".

---

## ⚙️ 6. Proximal Policy Optimization (PPO): El Estándar Moderno

Si bien A2C/A3C mejoraron el método de gradiente de política, el algoritmo que se ha convertido en el estándar de facto en Deep RL (utilizado por OpenAI y DeepMind en la mayoría de sus trabajos recientes, incluyendo entrenamiento de robótica y juegos complejos) es el **Proximal Policy Optimization (PPO)**.

PPO es un algoritmo **Actor-Critic** que resuelve un problema clave de los métodos Actor-Critic simples: su **sensibilidad al tamaño del paso de aprendizaje**.

### A. El Problema de las Políticas de Gradiente

Los algoritmos de Gradiente de Política simples (como A2C) actualizan la política tomando un paso del gradiente de recompensa. Si este paso es **demasiado grande**, la nueva política puede ser significativamente peor que la anterior, lo que provoca que el entrenamiento se vuelva inestable o que el modelo "caiga" fuera de un buen mínimo.

### B. El Mecanismo de PPO: La Pérdida Clipada (Clipped Loss)

PPO introduce una **función de pérdida modificada** que restringe la magnitud del cambio de política en cada paso de optimización. Esto garantiza que la nueva política ($\pi_{\text{new}}$) se mantenga **próxima** a la política anterior ($\pi_{\text{old}}$), lo que resulta en un entrenamiento mucho más estable y robusto.

La función de pérdida de PPO, $\mathcal{L}^{\text{CLIP}}(\theta)$, es una función compleja que incluye el término de la **Ventaja** ($A_t$) y un factor de ratio:

$$\mathcal{L}^{\text{CLIP}}(\theta) = \mathbb{E}_t \left[ \min(r_t(\theta) A_t, \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon) A_t) \right]$$

Donde:
* $r_t(\theta) = \frac{\pi_{\text{new}}(a_t|s_t)}{\pi_{\text{old}}(a_t|s_t)}$ es el **ratio de probabilidad** (qué tan probable es la acción en la política nueva en comparación con la vieja).
* $\epsilon$ es el **hiperparámetro de *clipping*** (típicamente 0.1 o 0.2), que define la región permitida de cambio.



### C. Funcionamiento del Clipping

La función $\min(\dots)$ asegura que la actualización de la política sea limitada:

1.  Si la nueva política es **mucho mejor** que la anterior ($r_t(\theta)$ es muy grande y $A_t$ es positiva), la actualización se **limita** a $(1+\epsilon)A_t$. Esto evita pasos excesivamente grandes.
2.  Si la nueva política es **mucho peor** que la anterior ($r_t(\theta)$ es muy pequeña y $A_t$ es negativa), la actualización se **limita** de manera similar, evitando que el modelo se castigue demasiado.

Al mantener el cambio de política **dentro de un vecindario seguro** (el rango $[1-\epsilon, 1+\epsilon]$), PPO logra la estabilidad de los métodos de optimización por lotes mientras mantiene la potencia de los métodos de gradiente de política. Esto lo convierte en uno de los algoritmos más utilizados para el entrenamiento de IA en entornos complejos.
