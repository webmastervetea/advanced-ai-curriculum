## 📝 Profundizando en la Teoría de la Información

---

## 🌪️ Entropía Condicional ($H(X|Y)$): La Incertidumbre Residual

Si la **Entropía** ($H(X)$) mide la incertidumbre de una variable $X$ sin conocimiento previo, la **Entropía Condicional** ($H(X|Y)$) mide la incertidumbre **restante** de $X$ después de que se ha observado otra variable $Y$.

En esencia, $H(X|Y)$ responde a la pregunta: "Si ya conozco el valor de $Y$, ¿cuánta incertidumbre me queda sobre $X$?"

### Definición Matemática

La entropía condicional se define como el promedio de la entropía de $X$ para cada valor posible de $Y$, ponderado por la probabilidad de ese valor de $Y$. Utiliza la probabilidad conjunta $P(x, y)$ y la probabilidad condicional $P(x|y)$:

$$H(X|Y) = \sum_{y \in \mathcal{Y}} P(y) H(X|Y=y) = - \sum_{x \in \mathcal{X}} \sum_{y \in \mathcal{Y}} P(x, y) \log_2 (P(x|y))$$

### Relación con la Comunicación

En un canal de comunicación donde $X$ es la señal transmitida e $Y$ es la señal recibida:

* $H(X)$ es la incertidumbre del mensaje original.
* $H(Y|X)$ es la **entropía del error o ruido**. Mide la incertidumbre en la señal recibida ($Y$) dada la señal transmitida ($X$). Si el canal es **perfecto** (sin ruido), $H(Y|X) = 0$.
* $H(X|Y)$ es la **incertidumbre residual** en el mensaje original ($X$) después de haber visto la salida recibida ($Y$). Esto representa la probabilidad de que el mensaje se haya interpretado mal.

El vínculo con la Información Mutua es directo:
$$I(X; Y) = H(X) - H(X|Y)$$
Es decir, la información mutua es la **reducción** de la incertidumbre original ($H(X)$) lograda al conocer la otra variable ($Y$).

---

## 📡 El Teorema del Canal Ruidoso de Shannon: El Límite de la Transmisión

El **Teorema del Canal Ruidoso de Shannon** (o Segundo Teorema de Shannon) es el resultado más fundamental y trascendental de la Teoría de la Información. Responde a la pregunta: **¿Cuál es la tasa de datos máxima a la que la información puede transmitirse de forma fiable a través de un canal con ruido?**

### 1. La Capacidad del Canal ($C$)

Shannon definió la **Capacidad del Canal ($C$)** como la tasa máxima de Información Mutua posible entre la entrada ($X$) y la salida ($Y$) sobre todas las posibles distribuciones de entrada $P(X)$:

$$C = \max_{P(X)} I(X; Y)$$

La capacidad $C$ se mide en **bits por segundo (bps)** o **bits por uso del canal**. Es una propiedad intrínseca del canal de comunicación (como un cable o una conexión inalámbrica).

### 2. El Teorema

El teorema establece dos afirmaciones cruciales:

* **Si la Tasa de Información ($R$) es menor que la Capacidad ($C$):**
    $$R < C$$
    Es teóricamente posible diseñar un sistema de codificación que permita transmitir la información con una **tasa de error arbitrariamente pequeña**. Esto significa que podemos enviar datos de manera fiable siempre que no excedamos el límite del canal.
* **Si la Tasa de Información ($R$) es mayor que la Capacidad ($C$):**
    $$R > C$$
    La tasa de error en la recepción **no puede** hacerse arbitrariamente pequeña. No importa qué tan inteligente sea el sistema de codificación que utilicemos, siempre habrá una tasa de error mínima en la recepción, ya que estamos intentando forzar demasiada información a través de un canal limitado.

### 3. La Fórmula de la Capacidad (Fórmula de Shannon-Hartley)

Para el caso específico de un **canal Gaussiano de Ruido Blanco Aditivo (AWGN)**, la capacidad del canal $C$ puede calcularse con una fórmula famosa:

$$C = B \log_2 \left( 1 + \frac{S}{N} \right)$$

Donde:
* $C$: Capacidad del canal (en bits por segundo).
* $B$: Ancho de banda del canal (en Hertz).
* $S/N$: La **Relación Señal a Ruido (SNR)**, que es la relación de la potencia promedio de la señal recibida ($S$) sobre la potencia promedio del ruido o interferencia ($N$).



Esta fórmula tiene implicaciones profundas:

1.  **La Banda Ancha Importa ($B$):** Duplicar el ancho de banda duplica directamente la capacidad.
2.  **La Calidad de la Señal Importa ($S/N$):** Mejorar la relación señal a ruido aumenta la capacidad logarítmicamente.
3.  **El Límite Absoluto:** La fórmula establece un **límite superior teórico** para la tasa de datos, sin importar la complejidad de la tecnología utilizada. Las tecnologías modernas (como el 5G y el Wi-Fi 6) se esfuerzan por acercarse a este límite de Shannon.


---

Continua: [[2-1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/2-1-redes-neuronales-recurrentes-memoria-maquinas.md)] 
