# 🤖 Diseño de Agentes Autónomos y su Interacción

Un **Agente Autónomo** es un sistema capaz de **percibir** su entorno, **tomar decisiones** de forma independiente y **actuar** sobre ese entorno para alcanzar objetivos predefinidos, a menudo sin intervención o supervisión humana continua. El diseño de estos agentes es fundamental para sistemas que van desde vehículos autónomos y robots industriales hasta *bots* de *software* de gestión de red.

El verdadero desafío y el futuro de la autonomía reside en la **interacción** de múltiples agentes en un entorno compartido, lo que da lugar a los **Sistemas Multi-Agente (SMA)**.

## 1. El Ciclo de Diseño de un Agente Individual

El diseño de un agente autónomo sigue un ciclo perceptual y de acción fundamental: el ciclo **Percepción-Decisión-Acción**.

### A. Percepción y Estado

El agente recibe información del entorno a través de **sensores** (cámaras, LiDAR, datos de red).

* **Modelo del Entorno:** El agente utiliza la información percibida para construir un **modelo interno** o una representación de su estado actual y la dinámica del mundo (esto se relaciona con la **Planificación Basada en Modelos**).
* **Incertidumbre:** El diseño debe incorporar mecanismos para manejar la **incertidumbre** y el ruido en las percepciones (ej. filtros de Kalman o modelos probabilísticos).

### B. Decisión y Racionalidad

La **racionalidad** es el principio central: el agente debe elegir la acción que maximice su **utilidad** (recompensa esperada) dada su percepción actual.

* **Planificación:** El agente utiliza algoritmos de planificación (como **MCTS** o algoritmos de **Aprendizaje por Refuerzo**) para simular trayectorias futuras y seleccionar la mejor acción inicial.
* **Tipos de Agentes:** Los agentes pueden ser reactivos (solo responden a estímulos inmediatos) o basados en objetivos (planifican secuencias de acciones para alcanzar un estado futuro deseado).

### C. Acción y Ejecución

El agente ejecuta la acción elegida a través de **actuadores** (motores, comandos de *software*). Después de la acción, el entorno cambia, lo que inicia un nuevo ciclo de percepción.



---

## 2. Sistemas Multi-Agente (SMA): El Desafío de la Interacción

Cuando múltiples agentes autónomos (o agentes que interactúan con humanos) operan en el mismo espacio, el diseño debe evolucionar para abordar la **coordinación**, la **comunicación** y el **conflicto**.

### A. Comunicación y Coordinación

La eficiencia del sistema depende de cómo los agentes intercambian información:

* **Lenguaje de Comunicación de Agentes (ACL):** Se necesitan protocolos estandarizados para que los agentes puedan negociar, hacer peticiones e informar sus estados de forma inteligible.
* **Coordinación Distribuida:** Los agentes deben coordinar sus acciones para evitar la duplicación de esfuerzos o la colisión. Esto puede lograrse mediante:
    * **Asignación de Tareas:** Algoritmos que distribuyen tareas de manera eficiente entre los agentes disponibles.
    * **Políticas de Formación:** En el **Aprendizaje por Refuerzo Multi-Agente (MARL)**, los agentes aprenden políticas que consideran implícita o explícitamente las políticas de los otros agentes.

### B. Cooperación vs. Competencia

La interacción del agente se rige por la naturaleza de la tarea:

* **Entornos Cooperativos:** Todos los agentes comparten el mismo objetivo global. Su diseño se centra en la **Maximización de la Recompensa Conjunta**. La comunicación es abierta y veraz.
* **Entornos Competitivos:** Los agentes tienen objetivos en conflicto (juegos de suma cero). El diseño implica la **Teoría de Juegos** y la búsqueda de equilibrios (ej. Equilibrio de Nash), donde ningún agente puede mejorar su recompensa cambiando su estrategia unilateralmente.
* **Entornos Mixtos:** Tienen elementos de cooperación y competencia (ej. negociación).

### C. Conflicto y Resolución

El diseño debe incluir mecanismos robustos para la resolución de conflictos, especialmente en el control de acceso a recursos compartidos.

* **Mediación:** Un agente supervisor (el mediador) toma decisiones de asignación de recursos o resuelve disputas.
* **Negociación:** Los agentes negocian explícitamente los planes de acción, llegando a compromisos que son aceptables para todas las partes.

---

## 3. Consideraciones Éticas y Sociales

A medida que los agentes autónomos interactúan más con los humanos y entre sí, surgen imperativos de diseño ético.

### A. Confiabilidad y Explicabilidad

Los agentes deben ser **confiables** y sus decisiones deben ser **explicables** para los usuarios.

* **Transparencia:** El diseño debe permitir que el agente justifique su elección (ej. "Elegí esta ruta porque la probabilidad de colisión era inferior al 1%").

### B. Seguridad y Equidad

El diseño debe considerar cómo las interacciones pueden ser explotadas por actores maliciosos o cómo las políticas de interacción podrían llevar a resultados sesgados.

* **Defensa Adversaria:** Diseñar agentes robustos a las interacciones maliciosas de otros agentes (análogo a la defensa contra **Ejemplos Adversarios**).
* **Equidad de la Política:** Asegurar que las políticas de interacción no resulten en un trato desigual o discriminatorio hacia ciertos agentes o grupos de usuarios.

El diseño de agentes autónomos y su interacción está evolucionando rápidamente, pasando de la optimización de un solo sistema a la creación de ecosistemas complejos y cooperativos capaces de realizar tareas sofisticadas en el mundo real.


---

Continua: [[18-3-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/18-3-2-modelado-de-la-cooperacion.md)] 
