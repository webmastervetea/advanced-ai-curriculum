# 🧬 Algoritmos y Programación Genética: Búsqueda Inspirada en la Evolución

Los **Algoritmos Genéticos (*Genetic Algorithms*, GA)** y la **Programación Genética (*Genetic Programming*, GP)** son dos poderosas técnicas de **computación evolutiva**. Ambas se inspiran en los principios de la **selección natural** y la **genética biológica** para resolver problemas de optimización y búsqueda en espacios de soluciones vastos y complejos donde los métodos tradicionales (como la fuerza bruta o el descenso de gradiente) fallan o son inviables.

## 1. Algoritmos Genéticos (GA): Optimización de Parámetros

El Algoritmo Genético es una metaheurística de búsqueda utilizada primariamente para **optimizar parámetros numéricos o binarios** dentro de un marco de solución predefinido.

### A. La Estructura de la Solución (Cromosoma)

* **Representación:** Una solución candidata (llamada **cromosoma**) se codifica típicamente como una cadena de bits o un vector de números. Esta cadena representa los parámetros que se están optimizando.
* *Ejemplo:* En un problema de optimización de *hardware*, el cromosoma podría ser un vector de tres parámetros: `[Frecuencia (bits), Voltaje (bits), Latencia (bits)]`.

### B. El Ciclo de Evolución (Las 5 Fases)

Un GA opera sobre una **población** de soluciones y evoluciona a lo largo de varias **generaciones**:

1.  **Inicialización:** Se genera aleatoriamente una población inicial de cromosomas.
2.  **Evaluación (*Fitness*):** Cada cromosoma es evaluado utilizando una **función de *fitness***. Esta función cuantifica qué tan buena es la solución para el problema (ej. minimizar el costo o maximizar la ganancia).
3.  **Selección:** Se seleccionan los individuos (cromosomas) con el *fitness* más alto (los "más aptos") para que sean los "padres" de la siguiente generación. Los métodos comunes incluyen la Selección por Ruleta o la Selección por Torneo.
4.  **Cruce (*Crossover*):** Los cromosomas seleccionados se aparean e intercambian partes de su material genético. Esta operación de **recombinación** es crucial para explorar nuevas combinaciones de parámetros de manera eficiente.
5.  **Mutación:** Se introducen pequeños cambios aleatorios en el cromosoma (ej. voltear un bit o cambiar un valor numérico). La mutación asegura la **diversidad** y evita que el algoritmo quede atrapado en un óptimo local.

El proceso se repite hasta que se cumple un criterio de terminación (ej. alcanzar un *fitness* objetivo o un número máximo de generaciones). 

---

## 2. Programación Genética (GP): Evolución de Programas

La **Programación Genética (GP)** es una extensión más ambiciosa de los GA. En lugar de optimizar solo los parámetros de una solución fija, la GP evoluciona la **estructura y el código del programa o la fórmula misma**.

### A. La Estructura de la Solución (Árbol de Sintaxis)

* **Representación:** Una solución (el "programa") se representa como un **Árbol de Análisis Sintáctico (*Parse Tree*)**.
* **Nodos:** Los nodos internos del árbol son **funciones** (ej. $+$, $-$, $\sin$, $\cos$, IF, THEN, ELSE).
* **Hojas:** Las hojas son **terminales** (ej. variables de entrada, constantes).
* *Ejemplo:* La fórmula $(X + 5) \times Y$ se representa como un árbol donde '$\times$' es la raíz, sus hijos son '$+$' y '$Y$', y los hijos de '$+$' son '$X$' y '$5$'.

### B. Operaciones Genéticas en GP

Las operaciones de cruce y mutación se adaptan para manipular las estructuras de árbol:

* **Cruce (*Crossover*):** Se intercambian subárboles enteros entre dos programas padres. Si el programa padre A tiene la rama $\sin(Z)$ y el padre B tiene la rama $X \times 2$, después del cruce, el hijo podría tener el programa que resulta de reemplazar $\sin(Z)$ por $X \times 2$.
* **Mutación:** Un subárbol es reemplazado aleatoriamente por un nuevo subárbol generado al azar.

## 3. Ventajas en Espacios de Búsqueda Complejos

Ambas técnicas son valiosas donde la búsqueda de soluciones es difícil:

* **Espacios No Derivables:** A diferencia del Descenso de Gradiente (usado en *Deep Learning*), que requiere que la función objetivo sea diferenciable, la GA/GP no utiliza gradientes y puede optimizar funciones **discontinuas, ruidosas o de caja negra**.
* **Optimización Multimodal:** Pueden explorar múltiples picos en el espacio de *fitness* simultáneamente, aumentando la probabilidad de encontrar el óptimo global y evitando óptimos locales (gracias a la población y la mutación).
* **Descubrimiento de Conocimiento (GP):** La GP es especialmente poderosa para la **regresión simbólica** o el descubrimiento de modelos. Cuando la GP encuentra un programa que resuelve un problema, este programa es una fórmula o un algoritmo legible por humanos, lo que facilita la interpretación y el descubrimiento científico.

---

## 4. Aplicaciones Clave

| Técnica | Aplicación Típica | Lo que se Evoluciona |
| :--- | :--- | :--- |
| **Algoritmos Genéticos (GA)** | **Optimización Numérica:** Optimización de horarios, diseño de antenas, *tuning* de hiperparámetros. | Los **valores** de un conjunto fijo de parámetros. |
| **Programación Genética (GP)** | **Descubrimiento de Fórmulas:** Regresión simbólica, creación de clasificadores, diseño de circuitos. | La **estructura** y la **lógica** del programa/fórmula. |

En resumen, los Algoritmos Genéticos y la Programación Genética ofrecen un enfoque robusto y flexible para la optimización y el aprendizaje. Al imitar el poder de la evolución natural, permiten a las máquinas diseñar sus propias soluciones y algoritmos, incluso en los entornos computacionales más desafiantes.
