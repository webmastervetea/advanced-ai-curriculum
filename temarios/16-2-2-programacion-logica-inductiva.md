# 🔎 Programación Lógica Inductiva (ILP): Aprendizaje de Reglas Simbólicas

La **Programación Lógica Inductiva (*Inductive Logic Programming*, ILP)** es un subcampo de la Inteligencia Artificial (IA) y el Aprendizaje Automático que se distingue por su uso de la **Lógica de Primer Orden** como marco fundamental para la representación de datos, conocimiento previo y resultados aprendidos. El objetivo principal de ILP es **inducir (aprender)** reglas lógicas complejas a partir de ejemplos de entrenamiento y conocimiento de fondo existente.

ILP se sitúa en la intersección del **Aprendizaje Automático Simbólico** y la **Programación Lógica** (principalmente Prolog), ofreciendo una solución para tareas de razonamiento que los modelos conexionistas (*Deep Learning*) puros encuentran difíciles.

## 1. El Paradigma de ILP: Inducción de Reglas

A diferencia de los modelos estadísticos que aprenden funciones de mapeo numéricas, ILP aprende **reglas declarativas** y **relaciones estructurales**.

### A. Componentes de Entrada

El proceso de ILP utiliza tres elementos clave:

1.  **Ejemplos Positivos ($E^{+}$):** Instancias de hechos que se sabe que son ciertos.
    * *Ejemplo:* `padre(juan, maria)`
2.  **Ejemplos Negativos ($E^{-}$):** Instancias de hechos que se sabe que son falsos.
    * *Ejemplo:* `padre(maria, juan)`
3.  **Conocimiento de Fondo ($B$):** Reglas y hechos previamente conocidos (axiomas) que se supone son ciertos.
    * *Ejemplo:* `masculino(juan)`, `progenitor(X, Y) :- padre(X, Y)`. (El conocimiento de que la paternidad implica la procreación).

### B. El Objetivo de la Inducción

El algoritmo de ILP busca una **Hipótesis ($H$)** (un conjunto de nuevas reglas lógicas) tal que:

1.  **Completitud:** El conocimiento de fondo ($B$) y la Hipótesis ($H$) juntos implican todos los ejemplos positivos ($E^{+}$).
    $$B \land H \models E^{+}$$
2.  **Consistencia:** El conocimiento de fondo ($B$) y la Hipótesis ($H$) juntos no implican ningún ejemplo negativo ($E^{-}$).
    $$B \land H \not\models E^{-}$$



---

## 2. Ventajas Clave de ILP

La representación lógica confiere a ILP ventajas únicas sobre los modelos numéricos:

* **Explicabilidad (XAI):** Las hipótesis aprendidas son reglas lógicas legibles por humanos. Esto facilita la auditoría, la validación y la comprensión del modelo. Por ejemplo, `abuelo(X, Z) :- padre(X, Y), progenitor(Y, Z)`.
* **Aprendizaje Relacional:** ILP sobresale en problemas donde las **relaciones estructurales** son más importantes que los atributos. Puede aprender reglas que involucran cualquier número de variables, no solo aquellas limitadas por la longitud de un vector de *features* (como en los algoritmos de clasificación estándar).
* **Eficiencia de Datos:** ILP puede aprender reglas robustas a partir de un **número relativamente pequeño de ejemplos**, ya que aprovecha fuertemente el conocimiento de fondo existente.

## 3. Algoritmos de Búsqueda de Reglas

La inducción de reglas implica una búsqueda en el vasto espacio de posibles cláusulas lógicas. ILP utiliza principios de inversión de la deducción:

### A. Generalización (Ascendente)

Este enfoque comienza con los ejemplos más específicos y utiliza operaciones de **generalización** para subir en el espacio de la hipótesis, buscando la regla más general que todavía cubra los ejemplos positivos y no cubra los negativos.

* **Mecanismo:** El algoritmo más conocido, **GOLEM**, utiliza la noción de **Generalización Menos General (*Least General Generalization*, LGG)**. Dada una colección de ejemplos positivos, LGG encuentra la regla más específica que es aún una generalización válida de todos ellos.

### B. Especialización (Descendente)

Este enfoque comienza con las reglas más generales posibles (que probablemente cubran ejemplos negativos) y utiliza operaciones de **especialización** (añadiendo literales al cuerpo de la regla) para reducir su alcance y eliminar la cobertura de los ejemplos negativos.

* **Mecanismo:** El algoritmo **FOIL** utiliza un enfoque de "cubrir y dividir" (similar a los árboles de decisión). En cada paso, selecciona el literal (condición) que maximiza la ganancia de información para la nueva cláusula.

---

## 4. Aplicaciones y Relevancia en la IA Híbrida

ILP es crucial para tareas donde se requiere una comprensión profunda y simbólica de las relaciones:

* **Bioinformática:** Descubrimiento de reglas sobre la estructura de proteínas o la actividad de compuestos químicos. Por ejemplo, aprender reglas sobre la estructura molecular que confieren toxicidad.
* **Ingeniería de Conocimiento:** Extracción automática de reglas y conocimiento de bases de datos para construir o enriquecer **Grafos de Conocimiento (*Knowledge Graphs*)**.
* **IA Híbrida:** ILP es un componente clave en la **IA Híbrida**. Los modelos de *Deep Learning* pueden encargarse de la percepción (ej. identificar entidades en una imagen), y las reglas aprendidas por ILP se utilizan para el **razonamiento simbólico** sobre esas entidades.

La Programación Lógica Inductiva proporciona una vía para que la IA no solo reconozca patrones, sino que también **razone, explique y descubra nuevas leyes y relaciones** de forma estructurada y declarativa.


---

Continua: [[17-1-1]()] 
