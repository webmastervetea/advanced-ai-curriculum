# 🤖 DDPG y TD3: Aprendizaje por Refuerzo en Espacios Continuos

El **Aprendizaje por Refuerzo Profundo (*Deep Reinforcement Learning*, DRL)** ha logrado un éxito notable en dominios complejos. Sin embargo, los primeros algoritmos de DRL se diseñaron principalmente para espacios de acción discretos (ej. mover ficha, presionar un botón). Para problemas del mundo real, como el control robótico o la conducción autónoma, las acciones (ej. ángulo del volante, torque del motor) son **continuas e infinitas**.

Los algoritmos **DDPG (*Deep Deterministic Policy Gradient*)** y **TD3 (*Twin Delayed Deep Deterministic Policy Gradient*)** son extensiones del DRL que resuelven este desafío al combinar el **Aprendizaje por Refuerzo Basado en el Valor** (Q-Learning) con el **Aprendizaje Basado en la Política** (Policy Gradients).

## 1. DDPG: La Fusión Determinista para Espacios Continuos

**DDPG** fue uno de los primeros y más exitosos algoritmos para manejar espacios de acción continuos. Se basa en el algoritmo **AC (*Actor-Critic*)**, pero utiliza el concepto de política **determinista** y redes neuronales profundas.

### A. Arquitectura Actor-Crítico

DDPG utiliza dos redes neuronales principales que trabajan conjuntamente (arquitectura *Actor-Critic*):

1.  **Actor ($\mu$):** Es la red de **política**. Toma el **estado continuo** ($s$) como entrada y produce la **acción continua** ($a$) directamente. Es **determinista** porque no produce una distribución de probabilidades, sino un valor de acción específico: $a = \mu(s)$.
2.  **Crítico ($Q$):** Es la red de **valor**. Toma el **estado ($s$) y la acción ($a$)** como entrada y estima la **función de valor-acción $Q(s, a)$**. El Crítico enseña al Actor indicándole qué tan buena es la acción que eligió.

### B. Redes *Target* y *Experience Replay*

Para lograr estabilidad en el entrenamiento de las redes profundas, DDPG introduce técnicas del algoritmo DQN:

* **Redes *Target*:** Se mantienen copias congeladas (ligeramente retrasadas) de las redes Actor y Crítico. Estas redes *target* se utilizan para calcular la recompensa futura (el objetivo *target* de Q-Learning), lo que estabiliza el proceso de ajuste.
* ***Experience Replay Buffer*:** Las transiciones de experiencia ($s, a, r, s'$) se almacenan y se muestrean aleatoriamente para el entrenamiento, rompiendo la correlación secuencial de los datos.

### C. Desafío de DDPG: La Sobreestimación

El problema principal de DDPG es que la red Crítico ($Q$) tiende a **sobreestimar sistemáticamente** los valores $Q$. Esta sobreestimación, al retroalimentarse al Actor, puede conducir a políticas subóptimas y a una convergencia frágil.

---

## 2. TD3: Estabilización a Través del Retraso y la Duplicidad

**TD3 (*Twin Delayed Deep Deterministic Policy Gradient*)** es una mejora directa y significativa sobre DDPG, diseñada específicamente para contrarrestar la sobreestimación del valor y estabilizar la política.

### A. El Triple Retraso (TD3)

TD3 introduce tres modificaciones clave para mejorar la estabilidad y el rendimiento:

1.  **Redes Críticas Gemelas (*Twin Critics*):** En lugar de usar una sola red Crítico, TD3 utiliza **dos redes Críticas independientes** ($Q_1$ y $Q_2$). Para calcular el valor *target* de la recompensa futura, utiliza el **mínimo** de las dos estimaciones de $Q$:
    $$Q_{\text{target}} = r + \gamma \cdot \min(Q_1'(s', a'), Q_2'(s', a'))$$
    Esto reduce la sobreestimación ya que es menos probable que ambas redes Críticas sobreestimen la misma acción.

2.  **Actualización Retrasada del Actor (*Delayed Policy Updates*):** La red Actor se actualiza con menos frecuencia que las redes Críticas (ej. el Crítico se actualiza 2 veces por cada 1 vez del Actor).
    * **Razón:** Esto da tiempo a las redes Críticas para alcanzar una estimación de valor más precisa antes de que el Actor use esa información para mejorar su política.

3.  **Suavizado de la Política (*Target Policy Smoothing*):** Se añade un **ruido aleatorio truncado** a la acción *target* utilizada para calcular el *target* $Q$.
    * **Razón:** Esto suaviza la función de valor sobre acciones similares, haciendo que el Crítico sea menos susceptible a los picos de valor causados por las acciones del Actor.



## 3. Comparación y Aplicaciones

| Característica | DDPG | TD3 |
| :--- | :--- | :--- |
| **Arquitectura** | Actor y Crítico únicos | Actor y **dos Críticos** gemelos |
| **Estabilidad** | Moderada; propenso a la sobreestimación | Alta; utiliza $\min(Q_1, Q_2)$ para prevenir la sobreestimación |
| **Actualización del Actor**| Cada paso de entrenamiento | **Retrasada** (menos frecuente que el Crítico) |
| **Suavizado** | No | Sí, añade ruido truncado a las acciones *target* |
| **Complejidad** | Más simple | Más complejo computacionalmente |

**Aplicaciones:** Ambos algoritmos son fundamentales para:

* **Robótica de Alto Grado de Libertad:** Control de brazos robóticos y manipuladores donde las articulaciones requieren ángulos y torques continuos.
* **Vehículos Autónomos:** Control de aceleración, frenado y dirección.
* **Control de Sistemas:** Regulación de variables continuas en plantas industriales o simulaciones físicas.

En resumen, TD3 representa una mejora sólida sobre DDPG, ofreciendo una solución más estable y robusta al desafío de entrenar políticas de acción continua en el Aprendizaje por Refuerzo Profundo.
