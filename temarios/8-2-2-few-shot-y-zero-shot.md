# 🎯 Few-Shot y Zero-Shot Learning: Aprender con Datos Escasos

El **Aprendizaje con pocos o ningún ejemplo** (Few-Shot y Zero-Shot Learning) desafía la dependencia tradicional del *Deep Learning* en grandes volúmenes de datos etiquetados. Estos enfoques permiten a los modelos **generalizar** a nuevas categorías o tareas que nunca vieron durante el entrenamiento, o que solo vieron representadas por un puñado de ejemplos.

## 1. Zero-Shot Learning (ZSL): Aprender sin Ejemplos

**Zero-Shot Learning (ZSL)** es el escenario más extremo: el modelo debe clasificar o generar una respuesta para una clase **que no estaba presente** en el conjunto de entrenamiento, sin haber visto ni un solo ejemplo de esa clase.

### A. El Mecanismo de Transferencia Semántica

Para que ZSL funcione, el modelo debe utilizar un **espacio semántico compartido** que conecte las clases que conoce (vistas) con las clases que no conoce (no vistas).

1.  **Representaciones Semánticas:** Cada clase se asocia con una representación semántica (ej. un vector de atributos, una descripción de texto, o un *embedding* de palabra).
2.  **Entrenamiento:** El modelo aprende a mapear las **características de la imagen** (o texto) al **espacio semántico** utilizando solo las clases vistas.
3.  **Inferencia:** Cuando se presenta un ejemplo de una clase no vista, el modelo proyecta sus características al espacio semántico y encuentra la clase no vista cuyo vector semántico es el **más cercano** al vector proyectado. 

* **Ejemplo en Visión:** Un modelo entrenado para clasificar perros y gatos, utilizando descripciones semánticas (ej. "tiene alas", "emite graznidos"). Al presentarle una imagen de un cuervo (clase no vista), el modelo proyecta la imagen cerca del vector semántico "pájaro que emite graznidos" y lo clasifica como "Cuervo".

### B. Aplicaciones Clave

* **Clasificación de Imágenes a Gran Escala:** Permite a los sistemas de visión clasificar conceptos raros o de cola larga sin requerir etiquetado constante.
* **Modelos de Lenguaje Grandes (LLMs):** El **Zero-Shot Prompting** es el uso más común de ZSL, donde un LLM responde a una instrucción sin ejemplos, basándose únicamente en el conocimiento adquirido durante el pre-entrenamiento.

---

## 2. Few-Shot Learning (FSL): Aprender con Muestras Mínimas

**Few-Shot Learning (FSL)** aborda el problema de la escasez de datos proporcionando un **pequeño conjunto de apoyo (*support set*)** que contiene $K$ ejemplos etiquetados para cada nueva clase que debe aprender. Típicamente, $K$ es un número muy pequeño, como $K=1$ (*One-Shot Learning*) o $K=5$ (*Five-Shot Learning*).

### A. Paradigma Meta-Learning

FSL se resuelve generalmente mediante **Meta-Learning** (*Learning to Learn*). En lugar de aprender a resolver directamente una tarea, el modelo aprende a **adquirir nuevas habilidades rápidamente**.

1.  **Episodios:** Durante el entrenamiento, el modelo es expuesto a muchas tareas de *K-shot* sintéticas, llamadas **Episodios**.
2.  **Meta-Aprendizaje:** El objetivo del meta-aprendizaje es optimizar los parámetros del modelo para que pueda lograr una alta precisión en una nueva tarea de *K-shot* después de solo una o unas pocas actualizaciones de gradiente (el proceso de adaptación).

### B. Técnicas Fundamentales de FSL

* **Redes Siamesas y Redes de Prototipos (*Prototypical Networks*):**
    * **Mecanismo:** El modelo aprende una función de **similitud** o **distancia**. En lugar de clasificar, calcula la distancia entre el nuevo ejemplo de consulta y un **prototipo** (el vector medio) de cada nueva clase en el conjunto de apoyo.
    * **Inferencia:** Clasifica el nuevo ejemplo a la clase cuyo prototipo está más cerca en el espacio de *embedding*. Esto permite la clasificación inmediata de nuevas clases sin reentrenamiento.
* **MAML (*Model-Agnostic Meta-Learning*):**
    * **Mecanismo:** MAML busca un conjunto inicial de parámetros ($\theta_0$) que sea **sensible** a pequeñas adaptaciones. Los parámetros se optimizan de manera que, si se realiza un pequeño número de pasos de gradiente en una nueva tarea, el modelo alcanza un alto rendimiento en esa tarea. MAML es "agnóstico" porque se puede aplicar a cualquier modelo que se entrene con gradiente.
* **Few-Shot Prompting (LLMs):**
    * **Mecanismo:** En el contexto de los LLMs, se incluyen unos pocos pares de **ejemplos de entrada-salida** dentro del *prompt* para enseñarle al modelo el formato, el estilo o la lógica de la tarea deseada. 

---

## 3. Convergencia y Retos

### A. Generalización vs. Sobreajuste

El mayor desafío en FSL es evitar el **sobreajuste (*overfitting*)** al pequeño conjunto de apoyo. Las técnicas de meta-aprendizaje y las redes prototípicas abordan esto asegurando que el modelo aprenda la **función de similitud** correcta, en lugar de memorizar ejemplos.

### B. La Era de la Transferencia de LLMs

En la práctica moderna de los **LLMs**, la distinción entre FSL y ZSL a menudo se difumina y se resuelve con el *prompting*:

* **ZSL (Zero-Shot) en LLMs:** El modelo ya tiene todo el conocimiento necesario gracias al pre-entrenamiento masivo. El *prompt* solo necesita instruir la tarea.
* **FSL (Few-Shot) en LLMs:** Se utiliza para **acondicionar** la respuesta del modelo, especialmente para tareas específicas de formato o razonamiento (ej. forzar una respuesta en formato JSON o seguir un estilo de escritura muy particular).

Ambos paradigmas son vitales para llevar la IA a entornos con datos escasos y para crear sistemas que puedan adaptarse rápidamente a nuevas realidades del mundo sin requerir un ciclo completo de recolección y etiquetado de datos.


---

Continua: [[9-1-1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/9-1-1-modelos-graficos-causales.md)] 
