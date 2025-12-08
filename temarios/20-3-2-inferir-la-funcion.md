# 🔎 Aprendizaje por Refuerzo Inverso (IRL): Infiriendo la Intención del Experto

El **Aprendizaje por Refuerzo Inverso (*Inverse Reinforcement Learning*, IRL)** es un paradigma del Aprendizaje Automático que aborda el problema opuesto al Aprendizaje por Refuerzo (RL). Mientras que el RL toma una función de recompensa y encuentra la política óptima, el **IRL toma la política (las demostraciones de las acciones de un experto) y deduce la función de recompensa subyacente ($R$)** que mejor explica dicho comportamiento.

Inferir la función de recompensa es crucial porque la función de recompensa codifica la verdadera **intención u objetivo** del experto, lo cual es más transferible y robusto que la mera imitación de acciones.

## 1. El Problema Fundamental del IRL

El Aprendizaje por Imitación (*Imitation Learning*, IL), como el *Behavior Cloning*, solo copia las acciones $\mathbf{a}_t$ para un estado $\mathbf{s}_t$. Si el agente se desvía, no sabe cómo corregir su rumbo porque no conoce el objetivo a largo plazo.

IRL resuelve esto respondiendo a la pregunta: "Si el experto actúa de manera óptima según alguna recompensa, ¿cuál es esa recompensa?".

### A. La Ambigüedad de la Recompensa

El desafío central del IRL es que el problema es **mal planteado (*ill-posed*)**: existe una cantidad infinita de funciones de recompensa que podrían explicar una política óptima observada.

* *Ejemplo:* ¿Un conductor está siendo óptimo por la función $R_1$ (llegar al destino rápido) o por la función $R_2$ (llegar al destino minimizando el consumo de combustible)? Ambas recompensas pueden resultar en trayectorias muy similares.

Los algoritmos de IRL deben incorporar supuestos o restricciones para desambiguar esta elección.

---

## 2. Algoritmos Clave en IRL

Para resolver la ambigüedad, los métodos de IRL buscan la función de recompensa que haga que la política observada sea **significativamente mejor** que cualquier otra política.

### A. Matching de *Features* Esperados (*Feature Matching*)

Esta es la base de los primeros métodos de IRL y de muchos métodos modernos.

* **Función de Recompensa Lineal:** Se asume que la función de recompensa es una combinación lineal ponderada de un conjunto de **características (*features*)** del estado $\phi(\mathbf{s})$:
$$R(\mathbf{s}) = \mathbf{w}^T \phi(\mathbf{s})$$
* **El Principio del Matching:** El IRL busca el vector de pesos $\mathbf{w}$ tal que la **suma de los *features* esperados** de la política $\pi$ que se deriva de $R$ sea igual a la **suma de los *features* esperados** de la política del experto $\pi_E$.
$$\mathbb{E}_{\pi_E}\left[\sum_{t=0}^{\infty} \gamma^t \phi(\mathbf{s}_t)\right] \approx \mathbb{E}_{\pi}\left[\sum_{t=0}^{\infty} \gamma^t \phi(\mathbf{s}_t)\right]$$
* **Significado:** Si el agente y el experto terminan visitando las mismas partes del espacio de estado y *features* a lo largo del tiempo, la función de recompensa inferida es probablemente la correcta.

### B. Aprendizaje por Refuerzo por Máxima Entropía (*Maximum Entropy IRL*, MaxEnt IRL)

MaxEnt IRL es la técnica más utilizada para resolver la ambigüedad de la recompensa.

* **Principio:** De todas las funciones de recompensa que explican las demostraciones, MaxEnt elige aquella que es la más **"suave" o "aleatoria"** (la que tiene la **máxima entropía**).
* **Mecanismo:** La política óptima derivada de esta recompensa no solo tiene altos valores de acción en las trayectorias del experto, sino que también permite una **mayor variabilidad** en las trayectorias que no son exactamente las del experto, siempre y cuando sigan la misma lógica de alto nivel (maximizar la recompensa).
* **Ventaja:** Esta penalización de la "rigidez" ayuda a evitar el sobreajuste a las demostraciones exactas y produce una función de recompensa más generalizable.

---

## 3. IRL y el Aprendizaje por Refuerzo de la Demostración (Apprenticeship Learning)

Una vez que se infiere la función de recompensa $R_{\text{inferida}}$, esta se utiliza para el siguiente paso, conocido como **Aprendizaje por Refuerzo de la Demostración (*Apprenticeship Learning*)**:

1.  **IRL:** Infiere $R_{\text{inferida}}$ a partir de $\pi_E$.
2.  **RL Estándar:** Utiliza un algoritmo de RL tradicional (como Q-Learning o PPO) para entrenar una nueva política $\pi_{\text{nueva}}$ que optimice $R_{\text{inferida}}$.

Esta nueva política suele ser **superior** a la política del experto porque, al optimizar la función de recompensa general, puede encontrar trayectorias que el experto nunca demostró o que son ligeramente más óptimas.

## 4. Aplicaciones Críticas

IRL es vital en dominios donde el costo o el riesgo de la experimentación aleatoria es prohibitivo:

* **Vehículos Autónomos:** Inferir la recompensa que codifica la conducción segura y social (mantener distancias, ceder el paso). Esto es más robusto que solo imitar la dirección del volante.
* **Control Robótico:** Enseñar a un robot a ensamblar un producto. En lugar de codificar una secuencia de movimientos rígida, IRL infiere la importancia relativa de cada sub-objetivo (ej. "sostener firmemente" vs. "mover rápido").
* **Modelado de Agentes:** Entender por qué otros agentes (humanos o IA) en un Sistema Multi-Agente toman ciertas decisiones, infiriendo sus preferencias y objetivos.

El Aprendizaje por Refuerzo Inverso transforma la imitación superficial en la comprensión de la verdadera intención, permitiendo a los agentes no solo copiar, sino también comprender y superar a sus maestros.


---

## Enorabuena has finalizado con exito los modulos 1 a 20 de Inteligencia Artificial
Modulos: [[21-40]()] 
