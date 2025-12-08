## 📐 Desarrollo Matemático de las Compuertas Recurrentes

Para cualquier unidad recurrente (RNN, LSTM, GRU), las entradas consisten en el vector de entrada actual ($x_t$) y el estado oculto anterior ($h_{t-1}$). Para simplificar la notación y el cómputo, a menudo se concatenan estas dos entradas en un solo vector: $[h_{t-1}, x_t]$.

### Notación de Pesos

* $W$: Representa la matriz de pesos que multiplica el estado oculto anterior ($h_{t-1}$).
* $U$: Representa la matriz de pesos que multiplica la entrada actual ($x_t$).
* $W_g$ o $W_{gh}$ / $U_{gx}$ (donde $g$ es la compuerta: $i, f, o, z, r$) son las matrices de pesos combinadas en la notación anterior.
* $b$: Es el vector de sesgo (bias).
* $\sigma$: Es la función sigmoide ($1 / (1 + e^{-x})$), utilizada en las compuertas para producir valores entre 0 y 1.
* $\tanh$: Es la tangente hiperbólica, utilizada para escalar los estados y candidatos entre -1 y 1.
* $*$ (asterisco): Representa la multiplicación por elementos (producto de Hadamard).

---

## 1. Long Short-Term Memory (LSTM)

La LSTM utiliza tres compuertas y el estado de la celda ($C_t$). La complejidad reside en que cada compuerta tiene su propio conjunto de pesos $W$ y $U$ (o el combinado $W$).

### A. Compuerta de Olvido (Forget Gate, $f_t$)
Decide qué información del estado anterior de la celda ($C_{t-1}$) se debe mantener.
$$f_t = \sigma(W_f h_{t-1} + U_f x_t + b_f)$$

### B. Compuerta de Entrada (Input Gate, $i_t$) y Candidato ($\tilde{C}_t$)
La compuerta de entrada ($i_t$) decide qué elementos actualizar, y el candidato ($\tilde{C}_t$) genera los nuevos valores que podrían añadirse al estado de la celda.

$$i_t = \sigma(W_i h_{t-1} + U_i x_t + b_i)$$

$$\tilde{C}_t = \tanh(W_C h_{t-1} + U_C x_t + b_C)$$

### C. Actualización del Estado de Celda ($C_t$)
Combina la información retenida (olvidada) del pasado con la nueva información de la entrada.
$$C_t = f_t * C_{t-1} + i_t * \tilde{C}_t$$

### D. Compuerta de Salida (Output Gate, $o_t$)
Decide qué parte del estado de celda ($C_t$) se revelará como el estado oculto actual ($h_t$).
$$o_t = \sigma(W_o h_{t-1} + U_o x_t + b_o)$$

$$h_t = o_t * \tanh(C_t)$$

La clave del éxito de la LSTM es que el flujo de gradientes a través del estado de celda ($C_t$) es una simple multiplicación (*elemento a elemento* $f_t * C_{t-1}$) antes de la suma, lo que permite que el gradiente fluya sin explotar o desvanecerse durante muchos pasos de tiempo.

---

## 2. Gated Recurrent Units (GRU)

La GRU simplifica el modelo al combinar la compuerta de olvido y la compuerta de entrada en una sola **Compuerta de Actualización** ($z_t$) y usa una **Compuerta de Reinicio** ($r_t$). Además, no tiene un estado de celda separado.

### A. Compuerta de Reinicio (Reset Gate, $r_t$)
Se utiliza en la fase de cálculo del candidato ($\tilde{h}_t$) para decidir cuánta información del pasado ($h_{t-1}$) debe ignorarse.

$$r_t = \sigma(W_r h_{t-1} + U_r x_t + b_r)$$

### B. Compuerta de Actualización (Update Gate, $z_t$)
Controla la mezcla entre el nuevo candidato y el estado oculto anterior. Un $z_t$ grande significa mantener más información del estado anterior.

$$z_t = \sigma(W_z h_{t-1} + U_z x_t + b_z)$$

### C. Estado Oculto Candidato ($\tilde{h}_t$)
El candidato utiliza la compuerta de reinicio ($r_t$) para modular el estado oculto anterior ($h_{t-1}$) antes de la transformación $\tanh$. Si $r_t$ es $\approx 0$, $h_{t-1}$ se ignora por completo.
$$\tilde{h}_t = \tanh(W_{\tilde{h}} (r_t * h_{t-1}) + U_{\tilde{h}} x_t + b_{\tilde{h}})$$

### D. Actualización del Estado Oculto ($h_t$)
El nuevo estado es un promedio ponderado (controlado por $z_t$) del estado anterior ($h_{t-1}$) y el nuevo candidato ($\tilde{h}_t$).
$$h_t = (1 - z_t) * h_{t-1} + z_t * \tilde{h}_t$$

La principal ventaja de esta notación es ver cómo las GRU utilizan el mismo conjunto de operaciones (multiplicación matricial, sumas y activaciones) pero con un **menor número de parámetros** (solo dos compuertas principales), lo que las hace más eficientes.



---

Continua: [[2-1-b2]()] 
