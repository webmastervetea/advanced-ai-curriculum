# ♟️ Modelado de la Cooperación y la Competencia en Sistemas Multi-Agente Complejos

Los **Sistemas Multi-Agente (SMA)** representan colecciones de agentes autónomos que interactúan en un entorno compartido. El modelado de la interacción entre estos agentes es crucial, ya que el rendimiento global del sistema depende de si sus objetivos individuales están **alineados (cooperación)** o **en conflicto (competencia)**.

El análisis de estos comportamientos se aborda primariamente a través de la **Teoría de Juegos** y las extensiones del **Aprendizaje por Refuerzo (RL)**.

## 1. Clasificación de Entornos Multi-Agente

El primer paso para el modelado es clasificar la naturaleza de la interacción. Esto se define por cómo la recompensa de un agente se relaciona con la recompensa de los demás.

### A. Entornos Cooperativos (*Cooperative Games*)

En un sistema puramente cooperativo, todos los agentes trabajan para un **objetivo común**, y la **recompensa es compartida y global**.

* **Definición:** La suma de las recompensas es constante y positiva, y todos los agentes obtienen el mismo valor por el éxito.
* **Modelado:** El desafío se centra en la **coordinación** y la **comunicación** eficiente para evitar la duplicación de esfuerzos o la interferencia mutua. Los agentes deben aprender una **política de equipo** que maximice la recompensa conjunta ($R_{\text{total}} = \sum R_i$).
* **Ejemplo:** Un equipo de robots de almacén que colabora para mover una carga pesada.

### B. Entornos Competitivos (*Zero-Sum Games*)

En un sistema puramente competitivo, la ganancia de un agente es directamente la pérdida de otro.

* **Definición:** La suma de las recompensas es cero (o una constante), lo que significa que el éxito de $A$ implica el fracaso de $B$.
* **Modelado:** El foco está en la **estrategia de equilibrio**. Se busca el **Equilibrio de Nash**, donde ningún agente puede mejorar su recompensa cambiando su estrategia unilateralmente, asumiendo que el otro agente mantiene su estrategia óptima.
* **Ejemplo:** Juegos de mesa de dos jugadores como el ajedrez o el Go (dominados por algoritmos como **AlphaZero**, que utiliza **Aprendizaje por Refuerzo Multi-Agente, MARL**, para el auto-juego).

### C. Entornos Mixtos (*Mixed-Motive Games*)

Los entornos complejos del mundo real (ej. tráfico, mercados financieros) rara vez son puramente cooperativos o competitivos; tienen **motivaciones mixtas**.

* **Definición:** Los agentes pueden tener objetivos parcialmente alineados y parcialmente conflictivos. La suma de recompensas puede variar.
* **Modelado:** Requiere métodos sofisticados que permitan la **negociación**, el **compromiso** y la **predicción** del comportamiento del oponente (modelado mental o *Theory of Mind*).
* **Ejemplo:** Negociación en el mercado o regulación del tráfico (los conductores cooperan para mantener el flujo, pero compiten por la prioridad en un carril).

---

## 2. Modelado de la Interacción mediante Aprendizaje por Refuerzo Multi-Agente (MARL)

El **Aprendizaje por Refuerzo Multi-Agente (MARL)** extiende el RL tradicional para entrenar a múltiples agentes simultáneamente.

### A. Centralized Training, Decentralized Execution (CTDE)

Esta arquitectura es la más común y exitosa para modelar la cooperación en entornos complejos, ya que resuelve el problema de la **observabilidad parcial**.

* **Entrenamiento (Centralizado):** Un controlador central (o un **modelo crítico**) tiene acceso a los estados y acciones **de todos los agentes** y al estado global del entorno. Esto simplifica el aprendizaje de una política de equipo óptima.
* **Ejecución (Descentralizada):** Una vez entrenados, los agentes individuales solo requieren su **propia observación local** para tomar decisiones, lo que permite el despliegue en sistemas distribuidos.
* **Ventaja:** Permite una coordinación eficiente sin saturar los canales de comunicación durante la ejecución real.

### B. Modelado del Oponente (Para Competencia)

En entornos competitivos, el agente debe modelar la política de su oponente.

* **RL Basado en la Teoría de Juegos:** Los agentes utilizan técnicas como el **Fictitious Play** o el **Meta-Aprendizaje** para crear modelos internos que predicen cómo responderán los otros agentes a sus propias acciones.
* **Redes Adversarias:** Los agentes pueden ser entrenados en un marco adversarial, donde el entrenamiento de uno mejora la estrategia de ataque, y el entrenamiento del otro mejora la estrategia de defensa.

---

## 3. Desafíos en Sistemas Complejos

El modelado en SMA complejos introduce desafíos únicos que deben mitigarse.

### A. La No Estacionariedad

En MARL, el entorno del agente $A$ no solo cambia debido a sus propias acciones, sino también debido a las acciones de los otros agentes ($B, C, \dots$). Si los otros agentes están aprendiendo (cambiando sus políticas), el entorno percibido por $A$ es **no estacionario**.

* **Mitigación:** Usar CTDE, o algoritmos que asuman una **estructura jerárquica** donde un agente principal coordina los sub-agentes.

### B. Credit Assignment (*Asignación de Crédito*)

En sistemas cooperativos, es difícil determinar la contribución individual de un agente a la recompensa total.

* **Problema:** Si el equipo gana, ¿fue por la acción brillante del Agente $A$ o por la lenta pero esencial acción del Agente $B$?
* **Solución:** Uso de **funciones de valor factorizadas** que descomponen la recompensa global en contribuciones individuales, permitiendo a cada agente aprender de su impacto local (ej. **QMIX**).

### C. Comunicación y Observabilidad

En entornos con comunicación limitada o ruidosa, el agente debe aprender a inferir el estado y la intención de los demás agentes basándose en la **observación parcial** de su comportamiento (la **Teoría de la Mente**).

El modelado exitoso de la cooperación y la competencia en sistemas multi-agente es esencial para aplicaciones como la gestión de flotas de vehículos autónomos, la robótica de rescate y la simulación económica, donde la interacción es la clave para la eficiencia del sistema.
