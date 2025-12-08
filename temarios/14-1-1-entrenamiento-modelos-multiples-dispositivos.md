# 🌐 Aprendizaje Federado (*Federated Learning*): Entrenamiento Descentralizado con Privacidad

El **Aprendizaje Federado (*Federated Learning*, FL)** es un paradigma de *Machine Learning* que permite entrenar un modelo global de alto rendimiento utilizando datos distribuidos en una gran cantidad de dispositivos o servidores (*clients*) sin que esos datos sensibles abandonen nunca su origen.

Desarrollado originalmente por Google para entrenar modelos predictivos en *smartphones* (ej. teclados predictivos), el FL se ha convertido en una solución esencial para problemas de **privacidad**, **seguridad** y **escasez de ancho de banda** en industrias como la sanidad, las finanzas y el IoT (*Internet of Things*).

## 1. El Paradigma de Privacidad por Diseño

El FL invierte el flujo de datos y cómputo del *Machine Learning* tradicional:

* **Modelo Tradicional (Centralizado):** Los datos se recopilan de todos los dispositivos y se envían a un servidor central para entrenar el modelo.
* **Aprendizaje Federado (Descentralizado):** El modelo (o una copia inicial del mismo) se envía a los dispositivos. Los dispositivos entrenan el modelo localmente con sus datos privados, y solo envían las **actualizaciones de los pesos del modelo** (los *gradients*) de vuelta al servidor central.

De esta manera, los **datos sensibles nunca se comparten** ni se almacenan en la nube. 

## 2. El Ciclo de Aprendizaje Federado

El entrenamiento en FL se lleva a cabo en rondas iterativas y coordinadas:

1.  **Distribución Inicial:** El **Servidor Central** inicializa el modelo global ($\mathbf{W}_t$) y lo envía a una cohorte seleccionada de **Clientes** (dispositivos).
2.  **Entrenamiento Local:** Cada cliente $k$ entrena el modelo localmente usando su conjunto de datos privado $\mathcal{D}_k$. El cliente calcula las actualizaciones de peso local $(\Delta \mathbf{W}_k)$ que optimizan el modelo para sus datos.
3.  **Comunicación:** Los clientes envían solo las actualizaciones locales $(\Delta \mathbf{W}_k)$ de vuelta al servidor. Este es el único paso de comunicación de datos.
4.  **Agregación Global:** El servidor utiliza un algoritmo de agregación (más comúnmente **FedAvg**) para combinar las actualizaciones locales y generar un nuevo modelo global actualizado ($\mathbf{W}_{t+1}$).
    $$\mathbf{W}_{t+1} \leftarrow \sum_{k=1}^K \frac{n_k}{N} \mathbf{W}_{t+1}^k$$
    Donde $n_k$ es el tamaño del conjunto de datos del cliente $k$, y $N$ es el tamaño total de la muestra.
5.  **Nueva Ronda:** El servidor distribuye el modelo $\mathbf{W}_{t+1}$ a los clientes para la siguiente ronda de entrenamiento.

---

## 3. Desafíos Centrales del Aprendizaje Federado

El diseño descentralizado introduce desafíos técnicos que no existen en el entrenamiento tradicional:

### A. Heterogeneidad de Datos (No-IID)

Los datos en diferentes dispositivos no suelen estar **distribuidos de forma independiente e idéntica (*Non-IID*)**.

* **Ejemplo:** Un usuario de *smartphone* puede escribir principalmente sobre recetas (dominio alimenticio), mientras que otro solo escribe sobre finanzas.
* **Problema:** Si el modelo se entrena en datos no-IID, el algoritmo de agregación puede tender a un modelo global sesgado hacia los clientes con mayor peso o puede tener un rendimiento pobre en dominios minoritarios.

### B. Heterogeneidad de Sistemas (*System Heterogeneity*)

Los dispositivos tienen capacidades computacionales, velocidades de conexión y niveles de batería muy diferentes.

* **Problema:** Los clientes más lentos (los *stragglers*) pueden ralentizar el proceso de entrenamiento global, o el servidor puede verse obligado a esperar por ellos.

### C. Privacidad y Seguridad Reforzadas

Aunque el FL no comparte datos sin procesar, las **actualizaciones de peso ($\Delta \mathbf{W}_k$) pueden filtrar información sensible** si se atacan con técnicas sofisticadas.

* **Ataques de Inversión de Gradiente:** Un atacante puede, en teoría, reconstruir el dato original a partir del gradiente enviado.

---

## 4. Técnicas para Reforzar la Privacidad

Para mitigar los riesgos de fuga de información a través de los gradientes, el FL se combina a menudo con otras técnicas de privacidad:

### A. Privacidad Diferencial (DP)

* **Mecanismo:** Se añade una **cantidad controlada de ruido** a los gradientes antes de que el cliente los envíe al servidor.
* **Resultado:** Esto hace que sea matemáticamente casi imposible inferir una única observación de datos a partir del gradiente agregado, proporcionando una garantía de privacidad cuantificable.

### B. Cifrado Homomórfico (HE)

* **Mecanismo:** Permite realizar **operaciones matemáticas** (como la suma y el promedio de los gradientes) directamente sobre los datos cifrados.
* **Resultado:** Los gradientes permanecen cifrados durante todo el proceso de agregación en el servidor, y solo el servidor puede descifrarlos después de la agregación.

## 5. Conclusión

El Aprendizaje Federado no es solo una optimización técnica; es una solución ética y práctica para la IA. Al descentralizar el entrenamiento, el FL permite que la IA se beneficie de la riqueza de los datos del mundo real manteniendo al mismo tiempo las protecciones de privacidad y las restricciones de ancho de banda, lo que es esencial para el futuro de las aplicaciones críticas en el *Edge* y la sanidad.


---

Continua: [[14-1-2]()] 
