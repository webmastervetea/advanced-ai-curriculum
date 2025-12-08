# 🧠 Redes Neuronales Recurrentes y la Memoria de las Máquinas

Las **Redes Neuronales Recurrentes (RNN)** son una clase especializada de redes neuronales diseñada para manejar datos secuenciales, donde la entrada en un momento dado depende de las entradas anteriores. A diferencia de las Redes Neuronales Convolucionales (CNN) o las Redes *Feedforward* (DNN), que tratan cada entrada como independiente, las RNN poseen un **estado interno** o **memoria** que les permite capturar la información de la secuencia pasada.



## 1. Redes Neuronales Recurrentes (RNN) Clásicas

La característica distintiva de una RNN es el **bucle de retroalimentación** en la capa oculta. En cada paso de tiempo $t$, la capa oculta recibe dos entradas:

1.  La entrada actual, $x_t$.
2.  El estado oculto anterior, $h_{t-1}$.

El nuevo estado oculto, $h_t$, se calcula utilizando una función de activación (típicamente $\tanh$ o $\text{ReLU}$) aplicada a la suma ponderada de la entrada actual y el estado oculto anterior:

$$h_t = f(W_{hh} h_{t-1} + W_{xh} x_t + b_h)$$

Donde $W_{hh}$ y $W_{xh}$ son las matrices de pesos para el estado oculto anterior y la entrada actual, respectivamente.

### Desafíos de las RNN Clásicas

Aunque son conceptualmente simples, las RNN clásicas sufren de problemas graves al manejar secuencias muy largas:

* **El Problema del Desvanecimiento del Gradiente (Vanishing Gradient):** Durante el entrenamiento (usando *Backpropagation Through Time* o BPTT), los gradientes (derivadas) que fluyen hacia atrás a través de muchos pasos de tiempo se vuelven exponencialmente pequeños. Esto impide que la red aprenda dependencias a **largo plazo**; la red olvida rápidamente la información que ocurrió hace muchos pasos de tiempo.
* **El Problema de la Explosión del Gradiente (Exploding Gradient):** En el caso opuesto, los gradientes pueden volverse demasiado grandes, causando inestabilidad y que los pesos de la red se actualicen drásticamente. (Esto se suele mitigar con técnicas de *clipping*).

Para resolver el problema del desvanecimiento del gradiente y permitir que la red retenga información durante periodos largos, se desarrollaron arquitecturas de celda más sofisticadas: **LSTM** y **GRU**.

---

## 2. Long Short-Term Memory (LSTM)

Las redes **Long Short-Term Memory (LSTM)**, introducidas por Hochreiter y Schmidhuber en 1997, son el avance más importante para solucionar los problemas de memoria a largo plazo de las RNN. Las LSTM reemplazan la simple unidad oculta de la RNN con una unidad de memoria compleja llamada **celda de memoria** (o bloque LSTM), la cual utiliza **compuertas (gates)** para controlar el flujo de información.



Cada celda LSTM opera con tres compuertas principales y un estado de celda:

### A. El Estado de la Celda ($C_t$)
Este es el **"cinturón transportador"** de la LSTM. La información fluye a través de él con solo algunas interacciones lineales menores. Permite que la información fluya sin cambios a lo largo de muchos pasos de tiempo, resolviendo así el problema del desvanecimiento del gradiente.

### B. Las Compuertas (Gates)
Son mecanismos basados en la capa sigmoide (que produce valores entre 0 y 1) y una operación de multiplicación por elementos, actuando como un "filtro" o "interruptor":

1.  **Compuerta de Olvido ($f_t$):** Decide qué información debe **descartarse** del estado de celda anterior ($C_{t-1}$). Un valor de 1 significa "mantener completamente" y 0 significa "olvidar completamente".
    $$f_t = \sigma(W_f \cdot [h_{t-1}, x_t] + b_f)$$
2.  **Compuerta de Entrada ($i_t$):** Decide qué valores **actualizar** en el estado de celda. Esto se hace en dos partes:
    * Una compuerta sigmoide ($i_t$) que decide qué valores actualizar.
    * Una capa $\tanh$ ($\tilde{C}_t$) que crea un vector de nuevos candidatos a añadir al estado.
    $$\tilde{C}_t = \tanh(W_C \cdot [h_{t-1}, x_t] + b_C)$$
3.  **Actualización del Estado de la Celda:** El estado anterior se olvida parcialmente y se le añade la nueva información candidata.
    $$C_t = f_t * C_{t-1} + i_t * \tilde{C}_t$$
4.  **Compuerta de Salida ($o_t$):** Decide qué partes del estado de la celda ($C_t$) se van a **emitir** como el nuevo estado oculto ($h_t$).
    $$o_t = \sigma(W_o \cdot [h_{t-1}, x_t] + b_o)$$
    $$h_t = o_t * \tanh(C_t)$$

Gracias a estas compuertas, las LSTM pueden retener información relevante durante cientos o miles de pasos de tiempo.

---

## 3. Gated Recurrent Units (GRU)

Las **Gated Recurrent Units (GRU)**, introducidas por Cho et al. en 2014, son una simplificación de las LSTM. Logran un rendimiento similar en muchas tareas, pero con una arquitectura más simple y, por lo tanto, son **más rápidas de calcular** y requieren **menos datos** para entrenar.

La principal diferencia con la LSTM es que la GRU:

1.  **Combina el estado oculto y el estado de celda:** La GRU solo tiene un estado, $h_t$, en lugar de los dos separados ($C_t$ y $h_t$) de la LSTM.
2.  **Utiliza solo dos compuertas:** Combina las compuertas de olvido y de entrada en una sola **Compuerta de Actualización** y mantiene una **Compuerta de Reinicio**.



### A. Compuerta de Reinicio ($r_t$)
Decide cuánta información del estado oculto anterior ($h_{t-1}$) es relevante para calcular el nuevo estado candidato. Si $r_t$ es 0, el estado oculto anterior es ignorado por completo.

$$\text{Candidato} (\tilde{h}_t) = \tanh(W_{\tilde{h}} \cdot [r_t * h_{t-1}, x_t] + b_{\tilde{h}})$$

### B. Compuerta de Actualización ($z_t$)
Actúa como un controlador principal entre el pasado y el presente. Decide qué parte del estado oculto anterior ($h_{t-1}$) se debe **mantener** y qué parte del nuevo contenido candidato ($\tilde{h}_t$) se debe **añadir**.

$$z_t = \sigma(W_z \cdot [h_{t-1}, x_t] + b_z)$$

### C. Actualización del Estado Oculto
El nuevo estado oculto es una combinación lineal del estado anterior y el nuevo candidato.
$$h_t = (1 - z_t) * h_{t-1} + z_t * \tilde{h}_t$$

Si $z_t$ es cercano a 1, la GRU se enfoca en la nueva información (similar a la compuerta de entrada de la LSTM). Si $z_t$ es cercano a 0, la GRU tiende a retener el estado anterior (similar a la compuerta de olvido de la LSTM).

---

## Resumen y Elección de Arquitectura

| Característica | RNN Clásica | LSTM | GRU |
| :--- | :--- | :--- | :--- |
| **Manejo de Secuencia** | Bueno para secuencias cortas. | Excelente para dependencias largas. | Excelente para dependencias largas. |
| **Problema de Gradiente** | Susceptible a desvanecimiento/explosión. | Solucionado por las compuertas. | Solucionado por las compuertas. |
| **Complejidad** | Baja. | Alta (Cuatro compuertas/parámetros). | Media (Dos compuertas/parámetros). |
| **Velocidad de Cómputo** | Rápida. | Lenta. | Más rápida que LSTM. |
| **Estado de Memoria** | Un solo estado oculto ($h_t$). | Dos estados ($C_t$ y $h_t$). | Un solo estado oculto ($h_t$). |

En la práctica, la elección entre LSTM y GRU a menudo se reduce a la tarea específica y los recursos:

* **LSTM:** Suele ser la opción preferida para problemas con **secuencias extremadamente largas** o donde la **precisión** es crítica y se tienen suficientes datos de entrenamiento.
* **GRU:** Es una excelente alternativa cuando se requiere **rapidez**, se tienen **menos datos** de entrenamiento, o como un buen punto de partida, ya que su simplicidad a menudo evita el sobreajuste.

Estos modelos son la base de aplicaciones de vanguardia como la traducción automática, la generación de texto (parte de la base de los modelos de lenguaje modernos), el reconocimiento de voz y la predicción de series temporales.



---

Continua: [[2-1-b1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/2-1-b1-desarrollo-matem%C3%A1tico-de-las-compuertas-recurrentes.md)] 
