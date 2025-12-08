# 📐 Técnicas de Optimización con Restricciones (Optimización Restringida)

Los problemas de **Optimización Restringida** son aquellos en los que buscamos maximizar o minimizar una **función objetivo** ($f(\mathbf{x})$) sujeto a un conjunto de **restricciones** que limitan el espacio de soluciones posibles. En el mundo real, casi todos los problemas de optimización (logística, ingeniería, finanzas) caen en esta categoría, ya que las soluciones deben cumplir con limitaciones de recursos, tiempo, presupuesto o reglas físicas.

El desafío es encontrar el punto óptimo ($\mathbf{x}^*$) que se encuentra dentro de la **región factible** definida por estas restricciones.

## 1. Clasificación de las Restricciones

Las restricciones se clasifican en función de su naturaleza matemática:

* **Restricciones de Igualdad:** Limitan la solución a una superficie o curva definida por $g_i(\mathbf{x}) = 0$.
* **Restricciones de Desigualdad:** Limitan la solución a una región (o hipersuperficie) definida por $h_j(\mathbf{x}) \le 0$.



---

## 2. Técnicas Clásicas de Optimización Restringida

Para problemas matemáticos bien definidos, las técnicas clásicas buscan transformar el problema restringido en uno no restringido.

### A. Multiplicadores de Lagrange (Restricciones de Igualdad)

Esta es la técnica fundamental para resolver problemas de optimización sujetos solo a **restricciones de igualdad**.

* **Mecanismo:** El método introduce una nueva variable, el **multiplicador de Lagrange** ($\lambda$), por cada restricción. Se construye una nueva función (*Lagrangiano*) que combina la función objetivo y las restricciones.
* **Fórmula:** Se busca optimizar el Lagrangiano:
    $$L(\mathbf{x}, \mathbf{\lambda}) = f(\mathbf{x}) - \sum_{i} \lambda_i g_i(\mathbf{x})$$
    El punto óptimo $\mathbf{x}^*$ se encuentra cuando el gradiente de $L$ con respecto a $\mathbf{x}$ y $\mathbf{\lambda}$ es cero, que corresponde a las **condiciones de Karush-Kuhn-Tucker (KKT)**.

### B. Condiciones de Karush-Kuhn-Tucker (KKT)

Las condiciones KKT son la generalización de los multiplicadores de Lagrange para incluir tanto **restricciones de igualdad como de desigualdad**. Son condiciones necesarias (bajo ciertas suposiciones de regularidad) para que una solución sea localmente óptima en un problema no lineal.

### C. Métodos de Penalización

En lugar de imponer las restricciones de forma estricta, estos métodos las incorporan directamente en la función objetivo como un **término de penalización**.

* **Mecanismo:** Si una solución candidata viola una restricción, se le aplica una penalización alta a su *fitness* (valor de la función objetivo).
    $$f_{\text{penalizado}}(\mathbf{x}) = f(\mathbf{x}) + P(\mathbf{x})$$
    Donde $P(\mathbf{x})$ es una función que es cero si las restricciones se cumplen, y aumenta cuadráticamente si se violan.

---

## 3. Técnicas Metaheurísticas para Problemas Complejos

Para problemas de optimización combinatoria o con funciones objetivo de "caja negra" (no derivables), los métodos metaheurísticos son más efectivos.

### A. Algoritmos Genéticos (GA) y Manejo de Restricciones

Los GA deben adaptarse para operar dentro de un espacio restringido:

1.  **Término de Penalización:** El enfoque más común es usar la **función de penalización** (descrita arriba). Las soluciones infactibles tienen un *fitness* muy bajo y son eliminadas por la selección natural.
2.  **Representación de la Solución (Codificación):** La estructura del cromosoma se diseña para que sea **imposible generar soluciones infactibles**.
    * *Ejemplo:* En un problema de ruteo con capacidad de camiones, la codificación del GA se asegura de que la suma de las demandas nunca exceda la capacidad.
3.  **Reparación:** Después de la operación de **cruce** o **mutación**, si la nueva solución viola una restricción, se aplica un operador de **reparación** que la ajusta a la frontera de la región factible más cercana.

### B. Optimización por Colonia de Hormigas (ACO)

ACO es naturalmente adecuado para algunos problemas restringidos, como el **Problema del Viajante (TSP)**, donde la restricción es que cada ciudad debe visitarse una sola vez.

* **Mecanismo:** La estructura del algoritmo impone la restricción. El algoritmo de construcción de rutas de la hormiga solo permite elegir nodos que aún no han sido visitados, garantizando la factibilidad de la ruta generada.

---

## 4. Programación Lineal y Entera (La Base Estricta)

Para problemas donde la función objetivo y las restricciones son lineales, se utilizan métodos de optimización estricta:

* **Programación Lineal (LP):** Si $f(\mathbf{x})$ y todas las restricciones son lineales, el método del **Símplex** garantiza encontrar el óptimo global en la frontera de la región factible.
* **Programación Entera (IP):** Cuando las variables $\mathbf{x}$ deben ser números enteros (comprar un número entero de máquinas, asignar una tarea a un individuo), se utilizan algoritmos de **Ramificación y Acotamiento (*Branch and Bound*)** para resolver la complejidad adicional.

El enfoque más adecuado para la optimización restringida siempre depende de la naturaleza del problema: **métodos analíticos** para problemas bien comportados, y **metaheurísticas** para problemas combinatorios o de alta complejidad.


---

Continua: [[20-1-1]()] 
