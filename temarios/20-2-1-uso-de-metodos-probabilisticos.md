# 📊 Estimación de Estado: Filtrado Probabilístico en Sistemas Dinámicos

La **Estimación de Estado** es el proceso de inferir los valores internos no observables de un sistema dinámico (como su posición, velocidad o temperatura interna) a partir de una secuencia de **mediciones ruidosas y parciales**. Este es un desafío fundamental en ingeniería, robótica, finanzas y control, ya que los sensores del mundo real siempre introducen ruido e incertidumbre.

Los métodos probabilísticos abordan este problema manteniendo una **distribución de probabilidad** sobre el estado del sistema en lugar de un único valor puntual, lo que permite la fusión óptima de la información predictiva del modelo y los datos ruidosos de la medición.

## 1. El Problema: Modelos y Ruido

Un sistema dinámico se define por su **modelo de transición** y su **modelo de medición**:

* **Modelo de Transición (Predicción):** Describe cómo evoluciona el estado del sistema ($\mathbf{x}_k$) a lo largo del tiempo, con un **ruido de proceso** ($\mathbf{w}_{k-1}$) que representa fuerzas externas desconocidas o errores de modelado.
    $$\mathbf{x}_k = f(\mathbf{x}_{k-1}) + \mathbf{w}_{k-1}$$
* **Modelo de Medición (Observación):** Describe cómo el estado real del sistema se relaciona con lo que observamos en los sensores ($\mathbf{z}_k$), con un **ruido de medición** ($\mathbf{v}_k$).
    $$\mathbf{z}_k = h(\mathbf{x}_k) + \mathbf{v}_k$$

El objetivo del filtrado es calcular la mejor estimación del estado actual ($\mathbf{\hat{x}}_k$) dada toda la información de medición histórica disponible ($\mathbf{Z}^k$).

---

## 2. El Algoritmo Fundamental: El Filtro de Kalman (KF)

El **Filtro de Kalman (KF)** es el algoritmo más famoso y eficiente para la estimación de estado, pero requiere que el sistema sea **lineal** y que el ruido sea **Gaussiano** (es decir, el ruido sigue una distribución normal).

El KF opera en un ciclo predictivo-correctivo continuo:

### A. Fase 1: Predicción (Propagación del Estado)

En esta fase, el filtro utiliza el modelo de transición para predecir el estado del sistema en el siguiente instante de tiempo ($k$), antes de que llegue la medición:

1.  **Estimación de Estado *A Priori*:** Propaga la estimación del estado anterior ($\mathbf{\hat{x}}_{k-1}$) a través del modelo lineal de transición.
2.  **Covarianza *A Priori*:** Estima la **incertidumbre** de la predicción, $\mathbf{P}_k$. Esta incertidumbre siempre aumenta en la predicción debido al ruido de proceso ($\mathbf{Q}$).

### B. Fase 2: Actualización (Corrección con Medición)

Cuando llega la medición ruidosa ($\mathbf{z}_k$), el filtro corrige su predicción:

1.  **Ganancia de Kalman ($\mathbf{K}_k$):** Este es el corazón del algoritmo. La **Ganancia de Kalman** calcula cuánto debe confiar el filtro en la nueva medición ($\mathbf{z}_k$) en comparación con su propia predicción. La confianza es inversamente proporcional a la incertidumbre de la medición.
2.  **Corrección de Estado *A Posteriori*:** La estimación final del estado ($\mathbf{\hat{x}}_k$) se calcula como un promedio ponderado de la predicción y la medición, utilizando la $\mathbf{K}_k$ como factor de ponderación.
3.  **Actualización de Covarianza:** Reduce la incertidumbre $\mathbf{P}_k$, ya que la medición reduce el desconocimiento.



---

## 3. Extensiones para Sistemas No Lineales

La mayoría de los sistemas reales son no lineales. Los Filtros de Kalman se han extendido para manejar esta complejidad:

### A. Filtro de Kalman Extendido (EKF)

El **Filtro de Kalman Extendido (EKF)** se utiliza cuando los modelos de transición ($f$) o de medición ($h$) son no lineales.

* **Mecanismo:** El EKF aborda la no linealidad **linealizando** el sistema alrededor del punto de operación actual (el estado estimado) utilizando el cálculo de **matrices Jacobianas**.
* **Desafío:** La linealización solo es una aproximación, y si el sistema es altamente no lineal o el punto de operación es inestable, el EKF puede divergir.

### B. Filtro de Kalman sin Perfilar (*Unscented Kalman Filter*, UKF)

El **UKF** es a menudo superior al EKF porque aborda el problema de la linealización de una manera más robusta.

* **Mecanismo:** En lugar de linealizar la función (Jacobiano), el UKF utiliza un conjunto de **puntos sigma** cuidadosamente seleccionados que representan la distribución de probabilidad del estado estimado. Estos puntos se propagan a través de la función no lineal original.
* **Ventaja:** El UKF captura mejor las verdaderas estadísticas de la distribución (media y covarianza) después de una transformación no lineal.

## 4. Aplicaciones Críticas

El filtrado probabilístico es esencial para:

* **Robótica y Navegación:** Fusión de datos de GPS (ruidosos y lentos) e IMU (inerciales, rápidos pero con deriva) para la estimación precisa de la posición de vehículos autónomos.
* **Finanzas Cuantitativas:** Estimación del estado oculto (volatilidad) de los mercados a partir de precios observables y ruidosos.
* **Sistemas de Control:** En lazo cerrado, el filtro proporciona la estimación de estado más limpia posible, lo que permite al controlador tomar acciones más precisas.

El uso de estos métodos permite a los sistemas dinámicos operar de manera más confiable y eficiente a pesar de la inherente imperfección de los sensores y la complejidad del mundo real.


---

Continua: [[20-2-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/20-2-2-integracion-de-filtros-de-kalman.md)] 
