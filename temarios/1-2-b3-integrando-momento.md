## 🚀 Integrando el Momento: El Descenso de Gradiente con Memoria

El problema del Descenso de Gradiente Estocástico (SGD) simple es que se comporta como una persona que camina por una colina con niebla: solo ve el gradiente inmediato. Esto conduce a dos ineficiencias clave:

1.  **Oscilaciones en Valles Estrechos:** En funciones de pérdida con "valles" estrechos (donde la pendiente es muy empinada en una dirección y muy plana en la otra), el SGD tiende a zigzaguear, dando grandes pasos inútiles a través del valle en lugar de avanzar de manera constante hacia el mínimo.
2.  **Lenta Convergencia:** Cuando el gradiente es pequeño (cerca del mínimo), el progreso es muy lento.

El optimizador con Momento resuelve esto añadiendo un término de **velocidad** que acumula una fracción de los gradientes pasados.

### 💨 El Concepto de Velocidad Acumulada

El Momento simula la inercia de una bola rodando por una colina. Si la bola ha estado rodando consistentemente en una dirección, no se detendrá ni cambiará bruscamente de rumbo solo por un pequeño cambio en la pendiente; seguirá moviéndose gracias a su inercia.

La actualización con Momento se divide en dos pasos:

#### 1. Cálculo del Vector de Velocidad ($v_t$)

En lugar de usar el gradiente actual $\nabla L$ directamente, calculamos una **velocidad** $v_t$ que es un promedio móvil exponencial de los gradientes anteriores.

$$v_t = \beta \cdot v_{t-1} + (1 - \beta) \cdot \nabla L$$

Donde:
* $v_t$: La nueva velocidad (o momento acumulado).
* $v_{t-1}$: La velocidad del paso de tiempo anterior (la "memoria" del sistema).
* $\nabla L$: El gradiente actual (el $\frac{\partial L}{\partial w}$ que calculamos en la Retropropagación).
* $\mathbf{\beta}$ (Beta): El **Coeficiente de Momento**, un hiperparámetro (típicamente entre 0.9 y 0.99) que controla cuánta "memoria" se retiene.

#### 2. Actualización del Peso ($w_{t}$)

El peso se actualiza utilizando esta nueva velocidad acumulada $v_t$ en lugar del gradiente puro.

$$w_{nuevo} = w_{viejo} - \eta \cdot v_t$$



### 💡 ¿Qué logra el Momento?

1.  **Aceleración Consistente:** En las dimensiones donde el gradiente apunta constantemente en la misma dirección (es decir, hacia el mínimo), el término $v_{t-1}$ se acumula rápidamente, aumentando la magnitud de $v_t$. Esto permite que el optimizador tome pasos más grandes y acelere hacia la solución.
2.  **Amortiguación de Oscilaciones:** En las dimensiones donde el gradiente zigzaguea (como en los valles estrechos), los gradientes positivos y negativos se **cancelan** mutuamente en el promedio de $v_t$. Esto reduce las oscilaciones inútiles, permitiendo que el avance neto sea mucho más suave y dirigido.

### 🔄 Aplicación a Nuestro Ejemplo Numérico

Retomemos el ejemplo de la neurona anterior.

| Valor | Descripción |
| :--- | :--- |
| $\frac{\partial L}{\partial w_1}$ | Gradiente actual (Paso $t=1$) | $\approx \mathbf{-0.00793}$ |
| $w_{viejo}$ | Peso inicial | $\mathbf{0.3}$ |
| $\eta$ | Tasa de Aprendizaje | $\mathbf{0.1}$ |

**Nuevos Parámetros de Momento:**
* **Coeficiente de Momento ($\beta$)**: Supongamos $\mathbf{0.9}$.
* **Velocidad Inicial ($v_0$)**: $\mathbf{0}$ (asumimos que la red comienza sin inercia).

#### Paso 1: Cálculo de la Nueva Velocidad ($v_1$)

$$v_1 = \beta \cdot v_0 + (1 - \beta) \cdot \nabla L$$
$$v_1 = 0.9 \cdot (0) + (1 - 0.9) \cdot (-0.00793)$$
$$v_1 = 0 + 0.1 \cdot (-0.00793)$$
$$v_1 = \mathbf{-0.000793}$$

#### Paso 2: Actualización del Peso con la Nueva Velocidad

$$w_{nuevo} = w_{viejo} - \eta \cdot v_1$$
$$w_{nuevo} = 0.3 - (0.1) \cdot (-0.000793)$$
$$w_{nuevo} = 0.3 + 0.0000793 \approx \mathbf{0.3000793}$$

**Observación:** En el primer paso, la velocidad no hace mucha diferencia (de hecho, el cambio fue menor que con el SGD puro, ya que el factor $(1-\beta)$ diluyó el gradiente). Sin embargo, en el **siguiente paso de entrenamiento ($t=2$)**, si el gradiente es similar, el término de Momento entrará en acción:

$$v_2 = \underbrace{0.9 \cdot (-0.000793)}_{\text{Inercia del paso anterior}} + \underbrace{0.1 \cdot (\nabla L_{t=2})}_{\text{Gradiente actual}}$$

A lo largo de muchas iteraciones, la inercia del $0.9 \cdot v_{t-1}$ se convierte en la fuerza dominante, empujando al peso en la dirección consistente que ha demostrado reducir la pérdida de manera más efectiva.

---

Continua: [[1-3](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/1-3-teoria-informacion%3A-midiendo-incertidumbre.md)] 
