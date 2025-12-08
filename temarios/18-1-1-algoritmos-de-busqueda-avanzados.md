# 🌳 Algoritmos de Búsqueda Avanzados: Monte Carlo Tree Search (MCTS)

El **Monte Carlo Tree Search (MCTS)** es un algoritmo de búsqueda heurística probabilística que se utiliza para la toma de decisiones en dominios complejos, especialmente en juegos de información perfecta (como el ajedrez o el Go). Se hizo mundialmente famoso como el algoritmo principal utilizado por **AlphaGo** de DeepMind, que derrotó a los campeones mundiales de Go.

MCTS combina la exploración aleatoria de los métodos de Monte Carlo con la búsqueda sistemática de un árbol de juego, equilibrando la exploración de nuevos movimientos con la explotación de los movimientos prometedores ya encontrados.

## 1. El Problema que Resuelve MCTS

Los algoritmos de búsqueda tradicionales (como el *Minimax* o *Alpha-Beta Pruning*) funcionan bien en juegos con espacios de estado limitados (como el ajedrez), pero fallan en juegos con un **factor de ramificación** extremadamente alto, como el Go (donde el número de movimientos posibles en cada turno es enorme).

MCTS resuelve esto al centrar sus recursos computacionales en la **parte más prometedora** del árbol de búsqueda, sin requerir una función de evaluación estática para cada estado.

## 2. Las Cuatro Fases del MCTS

El algoritmo MCTS realiza iteraciones continuas. En cada iteración, un árbol de búsqueda se construye o se expande a través de cuatro pasos secuenciales:

### A. 1. Selección (*Selection*)

El proceso comienza en el nodo raíz (el estado actual del juego). El algoritmo recorre el árbol de búsqueda existente, seleccionando el nodo hijo más prometedor en cada nivel hasta llegar a un nodo **hoja** (un nodo que aún no ha sido explorado completamente).

* **Exploración vs. Explotación:** La selección se guía por la métrica **Upper Confidence Bound 1 applied to Trees (UCT)**. UCT equilibra el **Exploitation** (elegir el movimiento que históricamente ha dado el mejor resultado) con la **Exploration** (elegir movimientos que han sido explorados muy pocas veces).
$$UCT = \frac{v_i}{n_i} + c \sqrt{\frac{\ln N}{n_i}}$$
Donde:
* $\frac{v_i}{n_i}$ es el término de **explotación** (ratio de victorias del nodo $i$).
* $c \sqrt{\frac{\ln N}{n_i}}$ es el término de **exploración** (desalienta la sobre-exploración de nodos visitados).


### B. 2. Expansión (*Expansion*)

Una vez que la selección llega a un nodo hoja no completamente explorado, se crea **un (o más)** nodo hijo nuevo para el nodo hoja. Este nodo recién creado representa un movimiento legal nunca antes simulado.

### C. 3. Simulación (*Simulation/Rollout*)

Desde el nodo recién expandido, se realiza una **simulación completa del juego** hasta que se alcanza un estado terminal (el juego termina).

* **Estrategia:** Esta simulación (*rollout*) se realiza utilizando una **política por defecto** (generalmente una política puramente aleatoria o una heurística simple y rápida, ya que el objetivo es solo obtener un resultado rápido).

### D. 4. Retropropagación (*Backpropagation*)

El resultado de la simulación (victoria, derrota o empate) se utiliza para **actualizar las estadísticas** de todos los nodos en el camino desde el nodo expandido hasta la raíz.

* **Actualización:** El contador de **visitas** ($n_i$) de cada nodo y su contador de **victorias** ($v_i$) se actualizan.

Este proceso se repite miles o millones de veces dentro de un tiempo de pensamiento limitado, construyendo un árbol de juego asimétrico y altamente enfocado.

## 3. La Decisión Final

Una vez que se agota el tiempo asignado para la búsqueda, el algoritmo no elige el movimiento con la tasa de victorias más alta, sino el nodo hijo de la raíz que ha sido **visitado más veces**.

* **Razón:** El número de visitas refleja el movimiento que el algoritmo ha considerado más prometedor y al que ha dedicado la mayor parte de sus recursos de simulación.

## 4. MCTS y Deep Learning (AlphaGo)

Si bien el MCTS tradicional es poderoso, su rendimiento depende de la calidad de la política por defecto utilizada en la simulación. **AlphaGo** combinó MCTS con **Redes Neuronales Profundas (*Deep Learning*)** para mejorar drásticamente las fases 1 y 3.

### A. Política (*Policy Network*) y Selección

AlphaGo utilizó una **Red de Política (*Policy Network*)** (una DNN) entrenada para imitar a jugadores expertos.

* **Impacto en MCTS:** En lugar de usar la fórmula UCT para todos los movimientos, la Red de Política sugería los movimientos más probables y **sesgaba la exploración** hacia ellos, podando el 99% de las ramas inútiles en el Go.

### B. Evaluación (*Value Network*) y Rollout

AlphaGo reemplazó la simulación *rollout* simple con una **Red de Valor (*Value Network*)** (otra DNN).

* **Impacto en MCTS:** Cuando MCTS llega a un nodo hoja, en lugar de simular aleatoriamente hasta el final, la Red de Valor **estima la probabilidad de victoria** a partir de ese estado. Esto ahorró mucho tiempo de cómputo que de otro modo se gastaría en simulaciones, permitiendo que el árbol creciera más profundamente y de forma más precisa.

La combinación de la eficiencia de búsqueda de MCTS con la percepción profunda de las redes neuronales creó un sistema de IA que superó la capacidad humana en la estrategia de juego.
