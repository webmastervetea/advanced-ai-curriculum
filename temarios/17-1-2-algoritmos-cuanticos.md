# ⚛️ Algoritmos Cuánticos Relevantes: La Superioridad de la Computación Cuántica

Los **Algoritmos Cuánticos** son procedimientos computacionales diseñados para ejecutarse en un ordenador cuántico, aprovechando fenómenos como la **superposición** y el **entrelazamiento** para resolver problemas de manera fundamentalmente más rápida que cualquier algoritmo clásico conocido. Dos algoritmos en particular —el de Grover y el de Shor— son la razón principal del inmenso interés en este campo.

## 1. Algoritmo de Shor: La Amenaza a la Criptografía

El **Algoritmo de Shor**, propuesto por Peter Shor en 1994, es el algoritmo cuántico más famoso debido a sus profundas implicaciones para la **seguridad cibernética global**. Su función es la **factorización de números enteros grandes** de manera exponencialmente más rápida que los mejores algoritmos clásicos.

### A. El Problema que Resuelve

El algoritmo de Shor resuelve el problema de **Factorización de Enteros**, que es la base de la criptografía de clave pública más utilizada en la actualidad, el sistema **RSA**.

* **Fundamento de RSA:** La seguridad de RSA se basa en la dificultad de que un ordenador clásico pueda encontrar los dos números primos que, al multiplicarse, dan como resultado un número muy grande ($N$). Para los ordenadores clásicos, el tiempo de ejecución crece exponencialmente con el tamaño de $N$.
* **Velocidad de Shor:** El algoritmo de Shor puede factorizar un número de $L$ bits en un tiempo que escala polinomialmente ($O(L^3)$), mientras que los métodos clásicos más rápidos (como la Criba de Campos de Números, NFTS) escalan subexponencialmente. Esto significa que un ordenador cuántico suficientemente grande podría **romper el cifrado RSA** en horas o minutos, una tarea que a las supercomputadoras clásicas les tomaría miles de millones de años.

### B. El Mecanismo Clave: Búsqueda de Período

El algoritmo de Shor reduce el problema de la factorización a un problema de **búsqueda de período** (*period-finding*). Para encontrar el período de una función periódica, utiliza la **Transformada de Fourier Cuántica (QFT)**, que es la contraparte cuántica de la Transformada de Fourier Discreta (DFT). La QFT se puede calcular de forma exponencialmente más rápida en un ordenador cuántico, lo que permite identificar el patrón periódico oculto en la función que lleva directamente a los factores primos.

---

## 2. Algoritmo de Grover: Aceleración de Búsquedas

El **Algoritmo de Grover**, propuesto por Lov Grover en 1996, es un algoritmo cuántico diseñado para acelerar el proceso de **búsqueda de un elemento en una base de datos no estructurada o no ordenada**.

### A. El Problema que Resuelve

Imagine una lista de $N$ elementos donde solo un elemento coincide con la condición de búsqueda (el "elemento objetivo"). En un ordenador clásico, el tiempo promedio para encontrar este elemento es $O(N)$ (lineal), ya que, en el peor de los casos, hay que revisar todos los elementos.

* **Aceleración Cuántica:** El algoritmo de Grover encuentra el elemento objetivo en un tiempo que escala como $O(\sqrt{N})$.
* **Ventaja:** Si la base de datos tiene $N = 1$ billón de entradas, un algoritmo clásico requeriría $10^{12}$ pasos, mientras que Grover solo requeriría $\sqrt{10^{12}} = 10^6$ pasos. Esta es una aceleración cuadrática significativa.

### B. El Mecanismo Clave: Amplificación de Amplitud

El algoritmo de Grover funciona mediante la **Amplificación de Amplitud**, un proceso que iterativamente aumenta la probabilidad de medir el estado objetivo, mientras disminuye la probabilidad de medir los estados no objetivos.

1.  **Superposición Inicial:** Todos los posibles estados (elementos de la base de datos) se colocan en una superposición uniforme.
2.  **Operador de Oráculo:** Una función (el oráculo) marca el elemento objetivo, invirtiendo la amplitud de probabilidad de su estado.
3.  **Reflexión (Amplificación):** Un operador de inversión amplifica las amplitudes negativas (la del objetivo) y reduce las amplitudes de los demás.
4.  **Iteración:** El proceso de oráculo y reflexión se repite $\approx \sqrt{N}$ veces. Después de estas iteraciones, la probabilidad de medir el elemento objetivo es muy cercana al $100\%$. 

---

## 3. Otros Algoritmos Relevantes

### A. Algoritmo de Simulación Cuántica

* **Problema:** La simulación de sistemas cuánticos (moléculas, materiales) es imposible para los ordenadores clásicos, ya que el espacio de estados crece exponencialmente.
* **Solución:** Los ordenadores cuánticos pueden simular estos sistemas de forma nativa. Esto es crucial para el diseño de nuevos materiales, el descubrimiento de fármacos y la química. Algoritmos como la **Ecuación de Fase Cuántica (*Quantum Phase Estimation*)** permiten estimar las propiedades de los sistemas moleculares.

### B. Algoritmos Híbridos (QAOA y VQE)

* **Problema:** Los ordenadores cuánticos actuales (NISQ - *Noisy Intermediate-Scale Quantum*) tienen qubits limitados y ruidosos.
* **Solución:** Algoritmos **híbridos** como el **Algoritmo de Optimización Aproximada Cuántica (*QAOA*)** y el **Eigensolver Variacional Cuántico (*VQE*)** combinan lo mejor de ambos mundos:
    * Un circuito cuántico maneja la parte más difícil del cálculo.
    * Un ordenador clásico optimiza los parámetros del circuito.
* **Aplicación:** Estos algoritmos se utilizan para problemas de optimización, finanzas y ML.

Los algoritmos de Shor y Grover establecieron el potencial teórico de la computación cuántica, mientras que los algoritmos de simulación y los híbridos impulsan la aplicación práctica a corto y medio plazo.


---

Continua: [[17-2-1]()] 
