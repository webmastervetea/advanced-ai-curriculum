# 🤝 Estrategias de Agregación de Modelos: El Núcleo de Federated Averaging (FedAvg)

El **Aprendizaje Federado (*Federated Learning*, FL)** permite entrenar un modelo de *Machine Learning* utilizando datos descentralizados en múltiples clientes (dispositivos móviles, hospitales, bancos) sin que los datos abandonen su ubicación original. Sin embargo, el éxito de este paradigma recae enteramente en la estrategia utilizada para **agregar** las actualizaciones de los modelos locales en un único modelo global.

El algoritmo más influyente y ampliamente adoptado para esta tarea es el **Promedio Federado (*Federated Averaging*, FedAvg)**.

## 1. El Rol de la Agregación

La agregación es el proceso mediante el cual un **Servidor Central** combina las actualizaciones de peso (gradientes o modelos locales) recibidas de una cohorte de clientes durante una ronda de entrenamiento, con el objetivo de producir un nuevo modelo global mejorado.

La agregación debe ser:
* **Eficiente:** Debe ser rápido para no ralentizar el proceso iterativo.
* **Justa:** Debe reflejar la contribución de los diferentes clientes.
* **Robusta:** Debe funcionar a pesar de la heterogeneidad de los datos y del sistema.

---

## 2. Federated Averaging (FedAvg): El Estándar de Oro

**FedAvg** es la solución propuesta por Google que combina la paralelización local del entrenamiento con una agregación ponderada en el servidor. Su objetivo es minimizar la pérdida de la función global $\mathcal{L}(\mathbf{W})$ promediando la pérdida de todos los clientes.

### A. El Ciclo Operacional de FedAvg

El entrenamiento de FedAvg se lleva a cabo en rondas comunicacionales ($t$):

1.  **Distribución ($t$):** El servidor selecciona un subconjunto de clientes $K_t$ (por razones de eficiencia y disponibilidad) y envía el modelo global actual $\mathbf{W}_t$.
2.  **Cómputo Local:** Cada cliente $k \in K_t$ recibe $\mathbf{W}_t$ y realiza múltiples pasos de **Descenso de Gradiente Estocástico (SGD)** localmente usando sus datos privados $\mathcal{D}_k$.
    * **Hiperparámetros Locales Clave:**
        * $E$: Número de épocas locales que el cliente entrena.
        * $B$: Tamaño del lote (*batch size*).
    * El entrenamiento local produce un modelo actualizado $\mathbf{W}_{t+1}^k$.
3.  **Agregación Ponderada:** El servidor recibe los modelos actualizados $\mathbf{W}_{t+1}^k$ de los clientes. El nuevo modelo global $\mathbf{W}_{t+1}$ se calcula como el promedio ponderado de los modelos locales. La clave de FedAvg es que el peso de cada cliente es proporcional al **tamaño de su conjunto de datos local ($n_k$)**.

$$\mathbf{W}_{t+1} \leftarrow \sum_{k \in K_t} \frac{n_k}{N_t} \mathbf{W}_{t+1}^k$$

Donde $n_k$ es el número de muestras de datos del cliente $k$, y $N_t = \sum_{k \in K_t} n_k$ es el número total de muestras utilizadas en la ronda $t$. 

### B. Ventajas de FedAvg

* **Eficiencia Comunicacional:** Al realizar múltiples épocas locales ($E > 1$) en cada cliente, FedAvg **reduce la frecuencia de comunicación** con el servidor, lo cual es vital para dispositivos con conexiones lentas.
* **Simpleza:** El algoritmo de agregación es simple y fácil de implementar.
* **Aprovechamiento de Recursos:** Permite que los clientes con mayor poder de cómputo y más datos contribuyan con un entrenamiento más extenso.

---

## 3. Desafíos y Extensiones de FedAvg

FedAvg, si bien es robusto, se enfrenta a desafíos importantes relacionados con la naturaleza descentralizada de los datos.

### A. Desafío de Datos No-IID (Heterogeneidad)

El problema más grande de FedAvg ocurre cuando los datos de los clientes están **distribuidos de forma no independiente e idéntica (Non-IID)**. Si un cliente entrena únicamente con datos de un dominio específico (ej. fotos de perros), su actualización local puede "alejar" el modelo global de un buen desempeño en otros dominios (ej. gatos).

* **Divergencia de Pesos:** El entrenamiento local prolongado ($E$ alto) en datos No-IID puede causar que los modelos locales diverjan significativamente antes de la agregación, lo que resulta en un modelo global subóptimo o en una convergencia lenta.

### B. Extensiones para Mitigar la Divergencia

Para abordar el problema Non-IID, han surgido extensiones de FedAvg:

* **FedProx:** Modifica la función de pérdida local de cada cliente añadiendo un **término de regularización de proximidad** $\mu$. Este término penaliza las actualizaciones de peso que se alejan demasiado del modelo global anterior ($\mathbf{W}_t$). Esto evita que los clientes Non-IID se desvíen demasiado.
* **SCAFFOLD:** Introduce un mecanismo para **corregir el sesgo** de los gradientes locales causados por la heterogeneidad de los datos. Cada cliente y el servidor mantienen variables de control para estimar y corregir la diferencia entre el gradiente local y el gradiente global ideal.

---

## 4. Agregación Robusta (Seguridad)

Otro desafío crucial es la **robustez** ante clientes maliciosos o fallos de datos.

### A. Ataques de Envenenamiento de Datos (*Data Poisoning*)

Un cliente malicioso podría enviar gradientes diseñados específicamente para degradar el rendimiento del modelo global o para inyectar una "puerta trasera" (*backdoor*) en el modelo.

### B. Técnicas de Agregación Segura

* **Filtrado (*Trimming*):** Se eliminan las actualizaciones de clientes que son valores atípicos (*outliers*) extremos, basándose en la distancia de los gradientes al gradiente medio.
* **Mediana Ponderada (*Weighted Median*):** En lugar de promediar los pesos, se utiliza la mediana ponderada. La mediana es intrínsecamente más robusta a los valores atípicos que el promedio.

En resumen, **FedAvg** proporcionó el *framework* inicial para la descentralización del entrenamiento. Las estrategias de agregación actuales se centran en mejorar la robustez de FedAvg ante la heterogeneidad y garantizar la seguridad del modelo resultante.
