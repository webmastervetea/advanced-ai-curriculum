# ⚖️ Comparativa: Ajuste Fino Completo vs. LoRA (Ajuste Fino Eficiente)

El **Ajuste Fino (*Fine-Tuning*)** es esencial para especializar un **Modelo de Lenguaje Grande (LLM)**. Sin embargo, la estrategia utilizada tiene implicaciones críticas en los costos, el rendimiento y la practicidad operativa del modelo.

## 1. Ajuste Fino Completo (*Full Fine-Tuning*)

El Ajuste Fino Completo es el método tradicional y directo para adaptar un LLM.

### Características y Mecanismo

* **Actualización Total:** Se utiliza el gradiente de la nueva tarea específica para actualizar **todos y cada uno de los parámetros** del LLM, desde la capa de entrada hasta la capa de salida. Si un modelo tiene 70 mil millones de parámetros, los 70 mil millones se ajustan.
* **Requisitos de Almacenamiento:** El modelo ajustado final requiere almacenar una **copia completa** del modelo base, lo que consume una gran cantidad de espacio en disco.
* **Entrenamiento y Hardware:**
    * **Consumo de Memoria VRAM:** Extremadamente alto, ya que la memoria debe mantener tanto los pesos del modelo como los gradientes de todos los parámetros. A menudo, requiere múltiples GPUs de alta gama.
    * **Tiempo de Entrenamiento:** Lento, debido a la gran cantidad de cálculos de gradientes y la propagación a través de miles de millones de pesos.

### Ventajas y Desventajas

| Aspecto | Ventaja | Desventaja |
| :--- | :--- | :--- |
| **Rendimiento** | Máximo rendimiento potencial, ya que todas las interacciones del modelo se optimizan. | Puede sufrir de **olvido catastrófico** si el conjunto de datos es muy pequeño o el *learning rate* es alto. |
| **Flexibilidad** | Alta adaptabilidad a tareas muy diferentes al pre-entrenamiento inicial. | Baja eficiencia de almacenamiento y despliegue. |
| **Costo** | N/A | Alto costo en hardware (VRAM) y tiempo. |

---

## 2. Ajuste Fino Eficiente: Adaptación de Bajo Rango (LoRA)

**LoRA** (*Low-Rank Adaptation of Large Language Models*) es la técnica **PEFT** (*Parameter-Efficient Fine-Tuning*) más popular y efectiva. Se basa en el principio de que los cambios necesarios para adaptar un modelo a una nueva tarea tienen una **dimensión intrínseca baja** o de **rango bajo**.

### Mecanismo de LoRA

En lugar de ajustar las matrices de pesos originales ($W$) del Transformador, LoRA **congela** la matriz $W$ y aprende la actualización ($\Delta W$) mediante una **descomposición de rango bajo**.



1.  **Descomposición:** La matriz de actualización $\Delta W$ se descompone en dos matrices mucho más pequeñas, $A$ y $B$:
    $$\Delta W = B \cdot A$$
    Donde $A$ y $B$ son matrices estrechas, y su dimensión interior, $r$ (**rango**), es un hiperparámetro muy pequeño (ej. $r=8$ o $r=16$), mientras que la matriz original $W$ puede tener miles de columnas y filas.
2.  **Entrenamiento:** Durante el ajuste fino, **solo se entrenan las matrices $A$ y $B$**. La matriz original $W$ se mantiene inmutable. El número de parámetros a entrenar se reduce de manera masiva.
3.  **Inferencia:** Para la predicción, la matriz ajustada final es: $W' = W + B \cdot A$. La suma se realiza solo en la inferencia, sin aumentar la latencia.

### Ventajas y Desventajas de LoRA

| Aspecto | Ventaja | Desventaja |
| :--- | :--- | :--- |
| **Eficiencia de Parámetros** | Se entrenan $\ll 1\%$ de los parámetros (ej. 0.01% de los 70B). | Puede no alcanzar el mismo *peak* de rendimiento que el Ajuste Fino Completo en casos muy específicos y divergentes. |
| **Memoria VRAM** | Muy bajo consumo. Puede entrenarse en GPUs de consumo (ej. 16GB de VRAM). | La **Latencia de Inferencia** puede aumentar ligeramente (debido a la suma de $W + B \cdot A$). |
| **Almacenamiento y Despliegue** | **Excelente:** Un modelo ajustado solo almacena las matrices pequeñas $A$ y $B$ (los *adaptadores*), que son archivos minúsculos (megabytes) en lugar de gigabytes. | N/A |
| **Velocidad** | Entrenamientos significativamente más rápidos (menos cálculos de gradientes). | N/A |

## 3. Conclusión Estratégica

La elección entre los métodos de ajuste fino se reduce a la disponibilidad de recursos y los objetivos de rendimiento:

| Situación | Método Recomendado | Razón |
| :--- | :--- | :--- |
| **Investigación de Vanguardia / Máximo Rendimiento** | Ajuste Fino Completo | Se necesita exprimir hasta el último punto de rendimiento y los recursos no son una limitación. |
| **Casos de Uso Empresarial / Producción / Recurso Limitado** | **LoRA (PEFT)** | La eficiencia en VRAM, la velocidad de entrenamiento y la facilidad de almacenamiento y gestión de múltiples adaptadores superan la ligera pérdida de rendimiento teórico. |

En el panorama actual de los LLMs (modelos de miles de millones de parámetros), **LoRA** se ha convertido en la solución de facto para la mayoría de las aplicaciones. Permite a las empresas mantener un único modelo base (como Llama 3) y entrenar cientos de adaptadores LoRA específicos (para servicio al cliente, legal, técnico, etc.), desplegándolos según la necesidad, sin requerir el entrenamiento o el almacenamiento de múltiples copias completas del LLM.


---

Continua: [[5-2]()] 
