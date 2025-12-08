# 🌐 Entrenamiento Distribuido: Escalando Modelos Grandes con Paralelismo

El **Entrenamiento Distribuido** consiste en dividir el proceso computacional y de memoria de un modelo de *Deep Learning* entre múltiples dispositivos o nodos (GPUs, TPUs, CPUs) conectados en red. Esto es necesario cuando el **tamaño del modelo** (cientos de miles de millones de parámetros) o el **tamaño del lote (*batch size*)** requerido para una convergencia estable exceden la capacidad de memoria o procesamiento de un único acelerador.

## 1. Paralelismo de Datos (*Data Parallelism*)

El **Paralelismo de Datos** es la forma más común y simple de entrenamiento distribuido. Se utiliza cuando el **modelo completo cabe en la memoria** de una sola unidad de procesamiento, pero se necesita un **lote de datos más grande** o una mayor velocidad de entrenamiento.

### A. Mecanismo Básico

1.  **Réplicas del Modelo:** Se crea una **copia idéntica** y completa del modelo ($\theta$) en cada uno de los $N$ dispositivos (llamados *workers* o nodos).
2.  **División del Lote:** El lote de datos global se divide equitativamente en $N$ sub-lotes (*mini-batches*). Cada dispositivo procesa su propio sub-lote de forma **independiente**.
3.  **Cálculo de Gradientes:** Cada dispositivo calcula el pase hacia adelante y hacia atrás, obteniendo sus propios **gradientes locales** ($\nabla_i$) basados en su sub-lote.
4.  **Sincronización (All-Reduce):** Los gradientes locales se **agregan y promedian** entre todos los dispositivos para obtener un gradiente global que representa el paso de actualización. Esta comunicación de gradientes es el punto de contención principal de la latencia.
5.  **Actualización:** El optimizador utiliza el gradiente promedio para actualizar los pesos en cada dispositivo, asegurando que todos los modelos permanezcan **sincronizados** al comienzo del siguiente paso.



### B. Tipos de Sincronización

* **Sincrónico:** Todos los dispositivos deben esperar a que el dispositivo más lento termine su cálculo y sincronice los gradientes antes de proceder al siguiente paso. Garantiza una convergencia idéntica al entrenamiento con un solo dispositivo.
* **Asincrónico:** Los dispositivos actualizan los pesos del modelo maestro de forma independiente tan pronto como terminan. Esto puede ser más rápido, pero puede llevar a una **convergencia inestable** debido a que algunos gradientes pueden basarse en pesos ya desactualizados.

---

## 2. Paralelismo de Modelos (*Model Parallelism*)

El **Paralelismo de Modelos** es esencial cuando el **modelo en sí mismo es demasiado grande** para que sus parámetros, gradientes y estados del optimizador quepan en la memoria de un solo dispositivo (el caso típico de los LLMs con cientos de miles de millones de parámetros).

El modelo se divide en partes, y cada parte se coloca en un dispositivo diferente.

### A. Paralelismo de Pipeline (*Pipeline Parallelism*)

Esta técnica divide el modelo **verticalmente** a lo largo de sus capas, creando una línea de ensamblaje (*pipeline*).

1.  **División por Capas:** Las capas del modelo se dividen en $K$ etapas (particiones), y cada etapa se asigna a una GPU/TPU diferente.
2.  **Flujo de Datos:** La GPU 1 calcula las primeras capas y pasa las **activaciones** a la GPU 2. La GPU 2 calcula sus capas y pasa las activaciones a la GPU 3, y así sucesivamente.
3.  **Desafío:** La ejecución secuencial pura causa que las GPUs permanezcan inactivas esperando la entrada de la GPU anterior. Esto se resuelve dividiendo el lote en **micro-lotes** y ejecutando el *forward* y *backward pass* de manera intercalada para mantener el *pipeline* lleno.



### B. Paralelismo de Tensor (*Tensor Parallelism* o *Intra-Layer Parallelism*)

Esta técnica divide las grandes operaciones tensoriales **horizontalmente** dentro de una misma capa. Se utiliza cuando incluso las **matrices de pesos de una sola capa son demasiado grandes** para un solo dispositivo.

1.  **División de Matrices:** Las matrices clave de la capa (típicas de la Atención o *Feed-Forward* en un Transformador) se dividen en fragmentos y cada fragmento se asigna a una GPU.
2.  **Comunicación Intensa:** Requiere una comunicación de red de muy alta velocidad, ya que los dispositivos deben intercambiar resultados intermedios (*All-Gather*, *Reduce-Scatter*) antes de que se pueda completar la operación de la capa.

---

## 3. Estrategias Híbridas y Otros Métodos de Eficiencia

El entrenamiento de modelos de vanguardia (ej. GPT-4, Llama 3) utiliza una combinación sofisticada de todas estas técnicas.

### A. Estrategia Híbrida (Modelo y Datos)

El enfoque más común para los LLMs es la **Hibridación del Paralelismo**:

1.  **Paralelismo de Modelo (Tensor/Pipeline):** Se divide el modelo original entre $M$ dispositivos para que quepa en la memoria.
2.  **Paralelismo de Datos:** Se replica esta configuración de $M$ dispositivos $D$ veces, creando un clúster de $M \times D$ dispositivos. El *batch* de datos se divide entre las $D$ réplicas.

### B. Paralelismo del Optimizador (ZeRO)

A medida que los modelos crecen, los pesos del modelo (parámetros), los gradientes y el **estado del optimizador** (ej. los momentos de Adam) consumen una cantidad masiva de memoria. Por ejemplo, el estado del optimizador por sí solo puede ocupar 12 veces la memoria de los pesos originales.

**ZeRO** (*Zero Redundancy Optimizer*) es una técnica clave que aborda esto dividiendo y distribuyendo:

1.  **ZeRO-1:** Distribuye el estado del optimizador entre todos los *workers*.
2.  **ZeRO-2:** Distribuye el estado del optimizador **y** los gradientes.
3.  **ZeRO-3:** Distribuye el estado del optimizador, los gradientes **y los propios parámetros del modelo**.

Esto permite entrenar modelos con miles de millones de parámetros con mucho menos hardware por nodo, ya que el modelo en sí nunca existe por completo en una sola GPU.

### C. Activaciones de Punto de Verificación (*Activation Checkpointing*)

Durante el *backward pass*, el cálculo de gradientes requiere las **activaciones** del *forward pass*. Para modelos profundos, almacenar todas estas activaciones consume gran parte de la memoria.

El *Activation Checkpointing* resuelve esto al **almacenar solo las activaciones de unas pocas capas seleccionadas** (los *checkpoints*). Las activaciones de las capas intermedias se **recalculan** durante el *backward pass* solo cuando es necesario. Esto ahorra una gran cantidad de memoria a costa de un ligero aumento en el tiempo de cálculo.

Al combinar el Paralelismo de Datos, las estrategias de Paralelismo de Modelos y las técnicas de eficiencia de memoria como ZeRO y el *Checkpointing*, los investigadores e ingenieros pueden superar las limitaciones físicas del hardware y entrenar los sistemas de IA más avanzados del mundo.


---

Continua: [[7-2-1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/7-2-1-optimizacion-latencia-rendimiento-produccion.md)] 
