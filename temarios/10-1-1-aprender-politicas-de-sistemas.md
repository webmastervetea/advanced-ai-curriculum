# 🧊 Aprendizaje por Refuerzo *Offline*: Políticas a Partir de Datos Estáticos

El **Aprendizaje por Refuerzo (*Reinforcement Learning*, RL)** tradicional se basa en la interacción continua del agente con su entorno, generando nuevos datos en tiempo real para explorar y aprender la política óptima. Sin embargo, en muchos dominios críticos —como la **robótica**, la **medicina**, los **vehículos autónomos** y la **economía**— esta interacción en tiempo real es inviable, peligrosa o prohibitivamente costosa.

El **Aprendizaje por Refuerzo *Offline*** aborda este desafío: **aprender la mejor política de acción únicamente a partir de un conjunto de datos estático, pre-recopilado y fijo, sin ninguna interacción adicional con el entorno.**

## 1. El Desafío Fundamental: Desconexión de Distribución

El principal obstáculo en el RL *Offline* es la **Desconexión de Distribución (*Distribution Shift*)** o **Error de Extrapolación de la Q-Función**.

### A. La Brecha de la Política

En el RL *Offline*, el conjunto de datos estático $\mathcal{D}$ fue generado por una **política de comportamiento** histórica ($\pi_b$), que a menudo es subóptima. El algoritmo intenta aprender una **política objetivo** u óptima ($\pi_{\theta}$) que maximice el retorno.

El problema surge cuando la política objetivo $\pi_{\theta}$ intenta tomar acciones ($a$) que **no están bien representadas** en el conjunto de datos de comportamiento $\mathcal{D}$ (es decir, $a \notin \pi_b(s)$).

* **Error de Valoración:** Si $\pi_{\theta}$ evalúa una acción desconocida ($a^*$) y la función de valor ($Q$) le asigna un valor erróneamente alto (porque no hay datos para corregir el error), el agente podría seleccionar $a^*$ en la vida real, llevando a un resultado desastroso (ej. el robot choca o se administra una dosis incorrecta).

## 2. Aplicaciones Críticas

La necesidad de aprender *offline* se debe a las severas consecuencias de la exploración en el mundo real.

### A. Robótica y Vehículos Autónomos 🚗

* **Riesgo de Seguridad:** La exploración física es peligrosa (daños al robot, accidentes de tráfico).
* **Recolección Costosa:** Se requiere un gran volumen de datos para cubrir escenarios de fallo (ej. condiciones climáticas extremas).
* **Solución:** Los algoritmos *offline* permiten entrenar y validar políticas en vastos *datasets* de datos operativos recogidos de flotas de vehículos, antes de desplegar el sistema.

### B. Medicina y Atención Sanitaria 🩺

* **Riesgo Ético:** Es imposible entrenar un agente RL en un paciente real (ej. probando diferentes dosis de fármacos al azar).
* **Datos Históricos:** Los algoritmos deben aprender políticas de tratamiento óptimas a partir de registros médicos electrónicos pasados, donde cada decisión médica pasada es una "acción" y la respuesta del paciente es la "recompensa".
* **Solución:** RL *Offline* permite la **personalización de tratamientos** al inferir políticas causales óptimas a partir de datos observacionales históricos.

## 3. Técnicas para Abordar la Desconexión de Distribución

La investigación se centra en cómo restringir la política objetivo para que solo confíe en acciones bien representadas en el conjunto de datos estático.

### A. Regularización y Restricción de Políticas (Pessimistic Approaches)

Estos métodos introducen una penalización o una restricción en la función de pérdida para evitar que la política objetivo se aleje demasiado de la política de comportamiento ($\pi_b$).

1.  **IQL (*Implicit Q-Learning*):** En lugar de aprender directamente la Q-función a través de la minimización del error de Bellman (que es sensible a los errores de extrapolación), IQL se enfoca en la **desigualdad de Bellman**. Intenta aprender un límite inferior pesimista de la Q-función, haciendo que la optimización sea más robusta.
2.  **CQL (*Conservative Q-Learning*):** Este es uno de los métodos más sólidos. CQL modifica la función de pérdida de la Q-función para que **penalice** activamente los valores Q estimados erróneamente altos para las acciones que no están en $\mathcal{D}$, y **fomente** valores Q altos para las acciones que sí están en $\mathcal{D}$. Esto obliga a que la estimación de valor sea **pesimista** sobre las acciones fuera de la distribución.

### B. Mapeo Explícito de la Distribución

Estos métodos intentan cuantificar qué tan bien una acción está cubierta por el *dataset* $\mathcal{D}$.

1.  **BCQ (*Batch-Constrained Q-Learning*):** Restringe las acciones a un subconjunto de acciones que son probablemente seleccionadas por la política de comportamiento $\pi_b$. Utiliza una **Red Generativa** (ej. un VAE) entrenada en $\mathcal{D}$ para proponer solo acciones que son similares a las acciones vistas.

## 4. El Papel de la Modelización del Comportamiento

Una componente común en muchos algoritmos *offline* es entrenar una **política de comportamiento ($\pi_b$)** para estimar la probabilidad de que la acción $a$ haya sido tomada en el estado $s$ en el conjunto de datos $\mathcal{D}$, $P(a|s, \mathcal{D})$.

Esta política $\pi_b$ se usa luego como un término de regularización o como un filtro para saber cuándo el algoritmo está intentando extrapolar. 

## 5. Perspectivas Futuras: De *Offline* a *Online*

El RL *Offline* no es solo una solución de nicho, sino un puente hacia un RL más generalizable:

* **Inicio de Políticas (*Policy Warm-Start*):** Los grandes *datasets* *offline* se pueden usar para pre-entrenar una política inicial muy sólida. Esta política se usa luego como un punto de partida seguro para una exploración *online* mínima y muy dirigida, reduciendo drásticamente el tiempo de entrenamiento y el riesgo en el entorno real.
* **Modelos del Mundo (*World Models*):** La combinación del RL *Offline* con modelos que intentan aprender una simulación precisa del entorno directamente a partir de los datos históricos.

En resumen, el Aprendizaje por Refuerzo *Offline* representa un cambio fundamental, al transformar el desafío de la escasez de interacción en un problema de **gestión de la incertidumbre** y **control de la extrapolación**. Sus avances son esenciales para la aplicación segura y ética del RL en los dominios de alto riesgo.


---

Continua: [[10-2-1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/10-2-1-procesamiento-de-nubes-de-puntos.md)] 
