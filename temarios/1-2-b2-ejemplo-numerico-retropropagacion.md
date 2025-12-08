## 🔢 Ejemplo Numérico de Retropropagación (Una Sola Neurona)

Supongamos una red extremadamente simple con **una sola neurona** y dos entradas. Nuestro objetivo es calcular $\frac{\partial L}{\partial w_1}$ para ajustar el peso $w_1$.

### ⚙️ 1. Definición del Modelo y Parámetros Iniciales

| Elemento | Fórmula / Valor | Descripción |
| :--- | :--- | :--- |
| **Entradas ($x$)** | $x_1 = 0.5$, $x_2 = 1.0$ | Los datos de entrada. |
| **Pesos ($w$)** | $w_1 = 0.3$, $w_2 = 0.7$ | Pesos iniciales a optimizar. |
| **Sesgo ($b$)** | $b = 0.1$ | Valor del sesgo. |
| **Etiqueta real ($y$)** | $y = 0.8$ | El valor que el modelo debería haber predicho. |
| **Función de Activación** | **Sigmoide** $f(z) = \frac{1}{1 + e^{-z}}$ | Se usa para introducir no-linealidad. |
| **Función de Pérdida ($L$)** | **Error Cuadrático** $L = \frac{1}{2} (y - a)^2$ | Una forma común de medir el error. |



### ➡️ 2. Propagación Hacia Adelante (Forward Pass)

Primero calculamos la salida de la neurona:

#### a) Entrada Neta ($z$)
$$z = (w_1 x_1) + (w_2 x_2) + b$$
$$z = (0.3 \cdot 0.5) + (0.7 \cdot 1.0) + 0.1$$
$$z = 0.15 + 0.7 + 0.1 = \mathbf{0.95}$$

#### b) Salida Activada ($a$)
Aplicamos la función Sigmoide a $z$:
$$a = f(z) = \frac{1}{1 + e^{-0.95}} \approx \mathbf{0.721}$$

#### c) Pérdida ($L$)
Calculamos el error del modelo:
$$L = \frac{1}{2} (y - a)^2 = \frac{1}{2} (0.8 - 0.721)^2$$
$$L = \frac{1}{2} (0.079)^2 \approx \mathbf{0.00312}$$

### ⬅️ 3. Propagación Hacia Atrás (Backward Pass)

Ahora, aplicamos la Regla de la Cadena para calcular el gradiente $\frac{\partial L}{\partial w_1}$.

$$\frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial a} \cdot \frac{\partial a}{\partial z} \cdot \frac{\partial z}{\partial w_1}$$

#### A. Tasa de Cambio de la Pérdida con respecto a la Salida ($\frac{\partial L}{\partial a}$)
Esta es la primera parte de la cadena, midiendo la sensibilidad del error a la predicción:
$$\frac{\partial L}{\partial a} = \frac{\partial}{\partial a} \left[ \frac{1}{2} (y - a)^2 \right] = -(y - a)$$
$$\frac{\partial L}{\partial a} = -(0.8 - 0.721) = \mathbf{-0.079}$$

#### B. Tasa de Cambio de la Salida con respecto a la Entrada Neta ($\frac{\partial a}{\partial z}$)
Esta es la derivada de la función de activación (Sigmoide), que resulta ser: $\frac{\partial a}{\partial z} = a (1-a)$.
$$\frac{\partial a}{\partial z} = 0.721 \cdot (1 - 0.721) = 0.721 \cdot 0.279 \approx \mathbf{0.201}$$

#### C. Tasa de Cambio de la Entrada Neta con respecto al Peso ($w_1$) ($\frac{\partial z}{\partial w_1}$)
Volviendo a $z = w_1 x_1 + w_2 x_2 + b$, la derivada parcial con respecto a $w_1$ es simplemente $x_1$:
$$\frac{\partial z}{\partial w_1} = \frac{\partial}{\partial w_1} (w_1 x_1 + w_2 x_2 + b) = x_1$$
$$\frac{\partial z}{\partial w_1} = \mathbf{0.5}$$

### 📉 4. Cálculo del Gradiente y Actualización

#### a) Gradiente Final ($\frac{\partial L}{\partial w_1}$)
Multiplicamos los tres términos de la cadena:
$$\frac{\partial L}{\partial w_1} = \underbrace{(-0.079)}_{\text{Paso A}} \cdot \underbrace{(0.201)}_{\text{Paso B}} \cdot \underbrace{(0.5)}_{\text{Paso C}}$$
$$\frac{\partial L}{\partial w_1} \approx \mathbf{-0.00793}$$

**Interpretación:** El signo negativo indica que si **aumentamos** el peso $w_1$, la pérdida $L$ **disminuirá** (lo cual es el objetivo).

#### b) Actualización del Peso (Descenso de Gradiente)

Usando una tasa de aprendizaje $\eta = 0.1$:
$$w_{nuevo} = w_{viejo} - \eta \cdot \frac{\partial L}{\partial w_1}$$
$$w_{nuevo} = 0.3 - (0.1) \cdot (-0.00793)$$
$$w_{nuevo} = 0.3 + 0.000793 \approx \mathbf{0.300793}$$

El peso $w_1$ ha sido ligeramente **aumentado**. Si volviéramos a ejecutar la Propagación Hacia Adelante con este nuevo valor, la pérdida $L$ sería ligeramente menor, demostrando que el modelo ha aprendido y mejorado.

---

Este proceso, que hemos simplificado a una sola neurona, se repite para **todos los millones de pesos** en cada capa de una red profunda, utilizando los gradientes calculados en las capas posteriores para informar los gradientes en las capas anteriores.


---

Continua: [[1-2-b3](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/1-2-b3-integrando-momento.md)] 
