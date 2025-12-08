## 🎲 Ejemplo de Implementación: Búsqueda Aleatoria en una Red Neuronal

Imaginemos que estamos construyendo una red neuronal de clasificación simple y queremos optimizar tres hiperparámetros clave.

### 1. Definición del Espacio de Búsqueda

Primero, definimos el rango de valores (el espacio de búsqueda) para los hiperparámetros que consideramos más influyentes:

| Hiperparámetro | Tipo | Espacio de Búsqueda (Rango de Valores) |
| :--- | :--- | :--- |
| **Tasa de Aprendizaje** (*Learning Rate*) | Real (logarítmico) | De $10^{-4}$ a $10^{-1}$ (ej. 0.0001 a 0.1) |
| **Número de Capas Ocultas** | Entero discreto | $\{1, 2, 3\}$ |
| **Tamaño del Lote** (*Batch Size*) | Entero discreto | $\{16, 32, 64, 128\}$ |

### 2. Metodología de Búsqueda Aleatoria

En lugar de probar todas las $3 \times 4 = 12$ combinaciones posibles (si la Tasa de Aprendizaje fuera también un conjunto pequeño de valores discretos), la Búsqueda Aleatoria nos permite muestrear combinaciones de manera más eficiente.

**Procedimiento:**

1.  **Iteraciones ($N$):** Decidimos que haremos $N=10$ pruebas (iteraciones) en total.
2.  **Muestreo Aleatorio:** En cada iteración, se selecciona un valor al azar para cada hiperparámetro dentro de su rango:
    * **Tasa de Aprendizaje:** Se suele muestrear en una escala logarítmica (por ejemplo, $\log_{10}(\text{LR})$ se muestrea uniformemente), ya que las diferencias entre $10^{-4}$ y $10^{-3}$ son tan críticas como entre $10^{-2}$ y $10^{-1}$.
    * **Número de Capas y Tamaño del Lote:** Se muestrean uniformemente de su conjunto discreto.
3.  **Entrenamiento y Evaluación:** Por cada combinación muestreada:
    * Se configura y entrena el modelo.
    * Se evalúa su rendimiento (ej. precisión) en el **conjunto de validación**.

### 3. Ejemplo de Ejecución (Tabla de Resultados)

La tabla muestra un posible resultado de 5 de las 10 iteraciones, junto con la precisión obtenida en el conjunto de validación:

| Iteración | Tasa de Aprendizaje | Capas Ocultas | Tamaño del Lote | Precisión en Validación |
| :---: | :---: | :---: | :---: | :---: |
| 1 | 0.005 | 2 | 64 | 85.1% |
| 2 | 0.08 | 1 | 128 | 78.9% |
| 3 | **0.0009** | **3** | **32** | **88.5%** |
| 4 | 0.02 | 2 | 16 | 84.0% |
| 5 | 0.001 | 3 | 128 | 87.2% |

### 4. Selección del Mejor Modelo

Al finalizar las 10 iteraciones, revisamos la tabla y seleccionamos la combinación de hiperparámetros que arrojó la mejor métrica:

* En este ejemplo, la **Iteración 3** ofrece el mejor rendimiento con una Precisión de Validación del **88.5%**.
* Los hiperparámetros óptimos (dentro de este espacio de búsqueda y estas 10 pruebas) son: $\text{Tasa de Aprendizaje}=0.0009$, $\text{Capas}=3$, $\text{Lote}=32$.

### 5. Prueba Final

Solo después de seleccionar esta mejor combinación, se entrena el modelo **una última vez** con el conjunto de entrenamiento completo (o la combinación de entrenamiento + validación) y se evalúa su rendimiento en el **conjunto de prueba** (el que nunca se ha visto). Este resultado final es la métrica de rendimiento que se reporta.

---

### ¿Por qué la Búsqueda Aleatoria es Mejor que la Búsqueda en Rejilla?

La ventaja crucial de la Búsqueda Aleatoria se muestra cuando solo un subconjunto de hiperparámetros es realmente importante.

* **En la Búsqueda en Rejilla:** Si se prueba la Tasa de Aprendizaje con 5 valores y el Tamaño del Lote con 5 valores, y el Tamaño del Lote es irrelevante, se dedican 5 pruebas a variaciones inútiles en el Lote por cada valor de la Tasa de Aprendizaje.
* **En la Búsqueda Aleatoria:** Se tiene una mayor probabilidad de muestrear valores significativamente diferentes para el hiperparámetro verdaderamente importante (ej. la Tasa de Aprendizaje) en las $N$ iteraciones, explorando de manera más efectiva la dimensión crítica.



La Búsqueda Aleatoria maximiza la posibilidad de probar valores únicos y potencialmente óptimos a lo largo de cada dimensión de hiperparámetro, siendo más eficiente para descubrir combinaciones ganadoras.



---

Continua: [[3-1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/3-1-autoencoders-variacionales-vae.md)] 
