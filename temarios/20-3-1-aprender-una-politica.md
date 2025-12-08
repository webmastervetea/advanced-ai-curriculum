# 🎓 Aprendizaje por Imitación (*Imitation Learning*): Aprendiendo de los Expertos

El **Aprendizaje por Imitación (*Imitation Learning*, IL)** es un paradigma del **Aprendizaje Automático (ML)** y la **Robótica** cuyo objetivo es entrenar un agente para que imite el comportamiento de un experto (humano o algorítmico) a partir de un conjunto de **demostraciones**. A diferencia del **Aprendizaje por Refuerzo (RL)**, que requiere que el agente aprenda mediante prueba y error a través de la maximización de una función de recompensa, el IL simplifica el problema al tratarlo como una tarea de **Aprendizaje Supervisado**.

Este enfoque es fundamental para tareas donde diseñar una función de recompensa adecuada es difícil o donde la seguridad es primordial (ej. vehículos autónomos).

## 1. El Paradigma del Aprendizaje Supervisado

La forma más básica y común de IL se reduce a un problema clásico de **Aprendizaje Supervisado**.

### A. Datos de Entrenamiento

Los datos son un conjunto de trayectorias recogidas del experto, que consisten en pares de **estado y acción óptima**:

$$\mathcal{D} = \{(\mathbf{s}_1, \mathbf{a}_1), (\mathbf{s}_2, \mathbf{a}_2), \dots, (\mathbf{s}_n, \mathbf{a}_n)\}$$

* **Estado ($\mathbf{s}$):** La observación actual del experto (ej. la imagen de la carretera, las posiciones articulares de un robot).
* **Acción ($\mathbf{a}$):** La acción que el experto tomó en ese estado (ej. ángulo del volante, torque del motor).

### B. Mapeo Directo de Política (*Policy Mapping*)

La tarea es entrenar un modelo de política ($\pi$) (a menudo una **Red Neuronal Profunda, DNN**) para que aprenda el mapeo de $\mathbf{s} \to \mathbf{a}$:

$$\pi(\mathbf{s}) \approx \mathbf{a}_{\text{experto}}$$

* Para un espacio de **acciones discretas** (ej. "girar izquierda", "girar derecha"), esto es un problema de **clasificación**.
* Para un espacio de **acciones continuas** (ej. valor de aceleración o frenado), esto es un problema de **regresión**.

El entrenamiento se realiza minimizando la **pérdida de imitación** (ej. error cuadrático medio para regresión o entropía cruzada para clasificación) entre la acción de la política y la acción del experto.

## 2. Detección de Comportamiento (*Behavior Cloning*, BC)

La implementación más sencilla de esta idea se conoce como **Detección de Comportamiento** o *Behavior Cloning* (BC).

### A. Mecanismo y Limitaciones

El BC toma todas las demostraciones del experto y entrena la política ($\pi$) de forma *offline*. Es sencillo y funciona bien si el conjunto de datos de demostraciones es amplio y cubre todos los posibles estados.

* **El Problema del Desajuste de Distribución (*Covariate Shift*):** Esta es la limitación más grave del BC. Si el agente entrenado se desvía ligeramente de la trayectoria del experto (algo inevitable debido a pequeños errores), se encontrará en un **estado nunca antes visto** en los datos de entrenamiento.
* **Consecuencia:** Al estar en un estado desconocido, la política puede tomar una acción catastrófica, alejándose cada vez más de la distribución de estados del experto. Esto conduce a errores acumulativos y fallos rápidos en entornos dinámicos.



---

## 3. Algoritmos Avanzados para Superar el *Covariate Shift*

Para abordar la acumulación de errores de BC, los algoritmos de IL modernos reintroducen un **bucle de interacción** entre el agente y el entorno.

### A. Muestreo de Desviación del Experto (*Dataset Aggregation*, DAgger)

**DAgger** resuelve el *Covariate Shift* al introducir un ciclo de recolección de datos interactivo.

1.  **Entrenamiento Inicial:** Entrenar la política ($\pi_0$) con los datos iniciales del experto.
2.  **Ejecución de la Política:** El agente ejecuta la política ($\pi_t$) en el entorno real, encontrando nuevos estados.
3.  **Etiquetado del Experto:** El experto **etiqueta las acciones correctas** para los nuevos estados encontrados por el agente (incluso los estados de "desviación").
4.  **Agregación:** Los nuevos pares $(\mathbf{s}_{\text{nuevo}}, \mathbf{a}_{\text{experto}})$ se agregan al conjunto de datos de entrenamiento.
5.  **Reentrenamiento:** La política ($\pi_{t+1}$) se reentrena en el conjunto de datos acumulado.

Al forzar al experto a proporcionar acciones correctas en los estados de error del agente, DAgger expande la distribución de estados de entrenamiento, haciendo que la política sea robusta a sus propios errores.

### B. Aprendizaje por Refuerzo a partir de Preferencias Humanas (RLHF)

Aunque no es estrictamente IL puro, **RLHF** (utilizado en modelos de lenguaje como ChatGPT) toma demostraciones (IL) y las combina con recompensas aprendidas para refinar la política.

* **Mecanismo:** El agente aprende una **función de recompensa** a partir de las preferencias y clasificaciones del experto sobre las trayectorias generadas. Luego, utiliza esta función de recompensa aprendida en un marco de RL (como **PPO**) para optimizar la política, lo que resulta en un comportamiento que no solo imita, sino que también satisface las preferencias sutiles del experto.

## 4. Aplicaciones y Relevancia

El Aprendizaje por Imitación es vital cuando:

* **La Tarea es Demasiado Compleja para el RL Puro:** Conducir un coche en el tráfico es complejo. Es más fácil que un agente aprenda imitando millones de kilómetros de conducción humana que diseñando una función de recompensa por "seguridad y comodidad".
* **La Recompensa es Escasa o Tarda en Llegar:** En RL, si el objetivo es lejano, el agente no recibe *feedback* para aprender. En IL, cada par $(\mathbf{s}, \mathbf{a})$ proporciona *feedback* inmediato.
* **Se Requiere Comportamiento Humano:** Para la creación de avatares o NPCs que se muevan y actúen de manera realista.

El Aprendizaje por Imitación proporciona un puente fundamental entre la experiencia humana y la automatización inteligente, permitiendo a los agentes adquirir habilidades complejas rápidamente y con un *feedback* de calidad garantizada desde el principio.


---

Continua: [[20-3-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/20-3-2-inferir-la-funcion.md)] 
