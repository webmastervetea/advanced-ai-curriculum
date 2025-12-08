# 🌐 Paralelismo en el Entrenamiento de Modelos Grandes

El entrenamiento distribuido es necesario cuando el **tamaño del modelo** (billones de parámetros) o el **tamaño del lote (*batch size*)** son demasiado grandes para la memoria de un único acelerador (GPU/TPU).

## 1. Paralelismo de Datos (*Data Parallelism*)

El **Paralelismo de Datos** es la estrategia más común y más fácil de implementar. Se utiliza cuando el **modelo completo cabe en la memoria** de una sola GPU, pero el entrenamiento sería demasiado lento o el *batch size* deseado es demasiado grande para procesarse de una vez.

### A. Mecanismo

1.  **Modelo Duplicado:** Se crea una **copia idéntica** del modelo ($\theta$) en cada uno de los $N$ dispositivos (GPUs/TPUs).
2.  **Datos Divididos:** El lote de datos global se divide en $N$ sub-lotes (*mini-batches*). Cada dispositivo procesa su propio sub-lote de forma independiente.
3.  **Cálculo:** Cada dispositivo calcula el pase hacia adelante y hacia atrás, generando sus propios **gradientes locales** ($\nabla_i$).
4.  **Sincronización:** Todos los gradientes locales se **agregan y promedian** entre todos los dispositivos (típicamente utilizando la técnica **All-Reduce**).
5.  **Actualización:** El modelo maestro o cada dispositivo individual actualiza sus pesos utilizando el gradiente promedio, garantizando que **todos los modelos permanezcan idénticos** al comienzo del siguiente paso.



### B. Ventajas y Limitaciones

* **Ventaja:** **Fácil de implementar** y proporciona una **aceleración lineal** casi perfecta con el número de dispositivos. El código base del modelo no necesita ser modificado.
* **Limitación:** **No resuelve el problema de la memoria del modelo.** Si el modelo por sí mismo es demasiado grande para caber en una sola GPU (ej. 70 mil millones de parámetros), esta técnica no funciona.

---

## 2. Paralelismo de Modelos (*Model Parallelism*)

El **Paralelismo de Modelos** se utiliza cuando el **modelo en sí mismo es demasiado grande** para caber en la memoria de una sola GPU/TPU (el escenario crítico para los Modelos de Lenguaje Grandes, LLMs).

El modelo se divide en partes y cada parte se coloca en un dispositivo diferente. Existen dos enfoques principales:

### A. Paralelismo de Pipeline (*Pipeline Parallelism*)

1.  **División Vertical (Capas):** El modelo se divide verticalmente a lo largo de sus capas. Las primeras $K$ capas van a la GPU 1, las siguientes $M$ capas van a la GPU 2, y así sucesivamente.
2.  **Ejecución:** La GPU 1 calcula las primeras capas y pasa las activaciones a la GPU 2. La GPU 2 calcula su bloque y pasa las activaciones a la GPU 3. La ejecución se asemeja a una **línea de ensamblaje**.
3.  **Eficiencia:** Para evitar que las GPUs permanezcan inactivas (burbujas en el *pipeline*), se utiliza la micro-división de los lotes y la programación avanzada.

### B. Paralelismo de Tensor (*Tensor/Layer Parallelism*)

1.  **División Horizontal (Matrices):** Dentro de una sola capa del Transformador, las matrices de pesos (la matriz de Atención o la matriz *Feed-Forward*) se dividen horizontal o verticalmente entre los dispositivos.
2.  **Ejecución:** Cada GPU ejecuta solo una porción de las operaciones de matriz. Requiere comunicación de alta velocidad entre GPUs para intercambiar resultados parciales (*All-Gather*, *Reduce-Scatter*) antes de que la capa pueda pasar a la siguiente etapa.
3.  **Uso:** Crucial para modelos gigantescos (cientos de miles de millones de parámetros) donde incluso una sola capa es demasiado grande.



### C. Ventajas y Limitaciones

* **Ventaja:** Permite entrenar **modelos de tamaño arbitrario** que no cabrían en una sola GPU.
* **Limitación:** **Alta complejidad** de implementación. Introduce **latencia de comunicación** significativa, ya que la activación de una capa debe esperar a que la capa anterior termine. Esto reduce la eficiencia en comparación con el Paralelismo de Datos.

---

## 3. Estrategias Híbridas (La Solución Estándar)

En la práctica, el entrenamiento de los LLMs modernos combina ambas estrategias:

1.  **Paralelismo de Modelo:** El modelo se divide entre los dispositivos necesarios para que quepa en la memoria (ej. utilizando Paralelismo de *Pipeline* y/o *Tensor*).
2.  **Paralelismo de Datos:** Se aplica el Paralelismo de Datos a través de los grupos de dispositivos que contienen réplicas del modelo dividido.

* *Ejemplo:* Un modelo se divide en 4 GPUs (Paralelismo de Modelo). Para acelerar el entrenamiento, esta configuración de 4 GPUs se replica 8 veces, sumando 32 GPUs en total (Paralelismo de Datos). Esto permite un *batch size* 8 veces más grande.

Esta combinación es lo que permite a las organizaciones entrenar modelos con miles de millones de parámetros en grandes clústeres de aceleradores.


---

Continua: [[7-1-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/7-1-2-entrenamiento-distribuido-modelos-grandes.md)] 
