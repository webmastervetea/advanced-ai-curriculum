# ⚡ Arquitecturas de Unidades de Procesamiento Tensorial (TPUs) de Google

Las **Unidades de Procesamiento Tensorial (*Tensor Processing Units*, TPUs)** son **aceleradores de *hardware* específicos de la aplicación (ASICs)** diseñados por Google y optimizados para el **cálculo matricial y tensorial**. Fueron creadas para acelerar drásticamente las cargas de trabajo de *Machine Learning* y *Deep Learning*, en particular aquellas que utilizan la librería **TensorFlow** de Google.

Las TPUs no reemplazan a las CPUs o GPUs, sino que están diseñadas para la fase de **entrenamiento** y **ejecución (inferencia)** de modelos a gran escala, donde dominan las operaciones de multiplicación matricial y convolución.

## 1. Motivación y Principio Básico

El rendimiento de un chip se mide por su capacidad de realizar operaciones por segundo. Para el *Deep Learning*, la métrica clave son las **operaciones de punto fijo por segundo (*Integer Operations Per Second*)** y las **operaciones de coma flotante por segundo (*Floating-point Operations Per Second*, FLOPS)**.

* **Cuellos de Botella de los CPUs/GPUs:** Aunque las GPUs son eficientes, su arquitectura es compleja y debe soportar gráficos y cómputo de propósito general. Las TPUs eliminan toda la complejidad innecesaria para el *Deep Learning*, enfocándose únicamente en la eficiencia energética y el rendimiento del álgebra lineal.
* **El Enfoque de la TPU:** Maximizar las operaciones por ciclo de reloj utilizando **precisión reducida** (a menudo $bfloat16$ en lugar de $float32$ estándar) y una arquitectura masivamente paralela.

## 2. Componentes Clave de la Arquitectura

La arquitectura de una TPU se centra en un componente fundamental que es la clave de su rendimiento: el **Motor Matricial (*Matrix Multiplier Unit*, MXU)**.

### A. El Motor Matricial (MXU)

El MXU es un gran conjunto de multiplicadores y sumadores interconectados que implementan una **Systolic Array** (Matriz Sistólica).

* **Matriz Sistólica:** Esta arquitectura permite la ejecución eficiente de multiplicaciones matriciales y convoluciones. Los datos fluyen a través de la matriz de multiplicadores de manera rítmica (como un sistema circulatorio, de ahí el nombre "sistólico") sin necesidad de acceder a la memoria externa después de la carga inicial.
* **Ventaja:** Esta organización de flujo de datos minimiza el movimiento de datos dentro del chip, lo cual es el mayor cuello de botella en términos de energía y velocidad. Las TPUs de última generación pueden tener decenas de miles de multiplicadores/sumadores en su MXU. 

### B. Memoria y Acceso

* **Memoria de Interconexión (*On-chip Memory*):** Las TPUs tienen grandes cantidades de memoria rápida y local para almacenar los resultados intermedios y las activaciones.
* **Memoria de Alto Ancho de Banda (HBM):** Utilizan memoria HBM apilada para proporcionar un ancho de banda extremadamente alto al chip, aunque es mucho más pequeña que la VRAM de una GPU.

---

## 3. Generaciones de TPUs

Google ha desarrollado varias generaciones de TPUs, escalando tanto el rendimiento como la capacidad de interconexión.

### A. TPUv1 (Inferencia)

La TPU de primera generación (2016) se lanzó inicialmente solo para la **inferencia** (ejecución) en los centros de datos de Google (Search, Translate). Utilizaba **precisión de 8 bits** para maximizar la velocidad y la eficiencia energética.

### B. TPUv2 (Entrenamiento y Escalabilidad)

Lanzada en 2017, marcó la transición al **entrenamiento** de modelos.

* **Float16 Básico (bfloat16):** Introdujo el formato **bfloat16**, que tiene el mismo rango dinámico que el $float32$ pero solo 16 bits de precisión. Esto permite un entrenamiento estable al tiempo que se duplica el número de operaciones por ciclo.
* **Interconexión:** Se diseñó para ser escalable, permitiendo la conexión de hasta 256 chips TPUv2 en un solo **Pod TPU**.

### C. TPUv3 (Refrigeración Líquida)

Lanzada en 2018, TPUv3 duplicó el rendimiento de TPUv2.

* **Densidad:** Mayor densidad de núcleos y velocidad de reloj, lo que requirió la introducción de la **refrigeración líquida** para gestionar la disipación de calor.

### D. TPUv4 y Más Allá

TPUv4 (2021) se centró en la **eficiencia y la escalabilidad masiva**.

* **Interconexión:** Utiliza una topología de interconexión bidimensional de alta velocidad, lo que permite crear **Pods TPU** masivos con miles de chips y un rendimiento agregado en el rango de los exaflops (mil millones de miles de millones de operaciones por segundo).
* **Software:** La arquitectura se ha integrado profundamente con **JAX** y **PyTorch/XLA**, aunque TensorFlow sigue siendo el principal *framework*.

---

## 4. El Paradigma de Uso: Pods TPU

La verdadera potencia de la TPU reside en la arquitectura de **Pods TPU**.

* **Diseño para Paralelismo:** Los Pods TPU son *clusters* de chips TPUs conectados mediante una red de alta velocidad y ancho de banda, diseñados para el **paralelismo de datos** y el **paralelismo de modelos**.
* **Ejecución de Carga de Trabajo:** Un solo modelo de *Deep Learning* (como un gran modelo de lenguaje, LLM) puede entrenarse distribuyendo el cálculo y los datos a través de todos los chips del Pod, lo que permite entrenar modelos que serían imposibles en *hardware* convencional.

## 5. Ventajas y Desventajas

| Aspecto | Ventajas de la TPU | Desventajas de la TPU |
| :--- | :--- | :--- |
| **Rendimiento/Eficiencia** | Alto rendimiento en FLOPS con excelente **eficiencia energética** debido al MXU y $bfloat16$. | Arquitectura **rígida**. Solo óptima para operaciones tensoriales. |
| **Latencia** | Baja latencia de inferencia gracias a la precisión reducida y el diseño ASIC. | Pobre rendimiento en operaciones que no son álgebra lineal (ej. bifurcaciones complejas, *sparse computation*). |
| **Escalabilidad** | Escalabilidad masiva y optimizada a través de la arquitectura de Pods TPU. | Requiere el uso del *framework* **XLA** (Accelerated Linear Algebra) para compilar y optimizar el código. |

Las TPUs representan un cambio significativo en la computación de IA, demostrando el valor de crear *hardware* especializado para cargas de trabajo específicas, asegurando que los avances en *Deep Learning* sigan siendo computacionalmente viables en la era de los modelos gigantes.


---

Continua: [[13-2-2]()] 
