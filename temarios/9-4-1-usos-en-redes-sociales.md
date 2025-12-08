# 🌐 Aplicaciones de Redes Neuronales Gráficas (GNNs): De Moléculas a Redes Sociales

Las **Redes Neuronales Gráficas (GNNs)** han emergido como la herramienta de *Deep Learning* más efectiva para modelar datos relacionales y no euclidianos. Su capacidad para capturar las dependencias entre nodos a través de las aristas (relaciones) ha transformado la forma en que abordamos problemas en sistemas altamente interconectados.

A continuación, exploraremos su impacto en tres dominios fundamentales.

---

## 1. 🧑‍🤝‍🧑 Usos en Redes Sociales: Análisis de Estructura y Comportamiento

En las redes sociales, los datos son inherentemente relacionales: los **usuarios** son nodos, y las **conexiones** (amistad, seguimiento, grupos) son aristas. Las GNNs son ideales para tareas que requieren comprender tanto las características individuales del usuario como su contexto social.

### A. Clasificación de Nodos y Usuarios

El objetivo es clasificar el tipo de usuario o nodo basado en sus características (perfil, publicaciones) y las características de sus vecinos.

* **Identificación de Bots y Cuentas Falsas:** Los bots a menudo exhiben patrones de conexión muy diferentes a los usuarios orgánicos (ej. se conectan rápidamente a muchos nodos influyentes o tienen patrones de interacción no recíprocos). Una GNN puede entrenarse para detectar estas anomalías estructurales en el *embedding* del nodo.
* **Segmentación y Orientación (*Targeting*):** Clasificar usuarios en segmentos (ej. "interesado en tecnología", "activista político") basándose no solo en lo que publican, sino en **quién los sigue o a qué grupos pertenecen**.

### B. Predicción de Enlaces (*Link Prediction*)

La predicción de enlaces es la tarea de predecir si debería existir una arista entre dos nodos actualmente desconectados.

* **Sugerencias de Amistad y Colaboración:** Al calcular la similitud entre los *embeddings* de dos nodos ($h_i$ y $h_j$), una GNN puede predecir la probabilidad de una conexión futura. Es crucial en plataformas como LinkedIn o Facebook.

### C. Detección de Comunidades

Las GNNs pueden ser utilizadas para detectar **comunidades** o **grupos de interés** dentro de la red. Al agregar la información de los nodos, se pueden identificar subgrafos densamente conectados que representan grupos naturales, lo cual es esencial para el análisis de la propagación de información o la polarización.

---

## 🧪 2. Química de Moléculas: Predicción de Propiedades Químicas

En la química computacional y el descubrimiento de fármacos, la estructura de una molécula es intrínsecamente un grafo, lo que convierte a las GNNs en herramientas de vanguardia.

### A. Moléculas como Grafos

* **Nodos:** Representan los **átomos** (con características como tipo de átomo, número atómico, carga formal).
* **Aristas:** Representan los **enlaces químicos** (con características como tipo de enlace —simple, doble, triple— o distancia).

### B. Predicción de Propiedades (Clasificación a Nivel de Grafo)

El objetivo es predecir una propiedad de la molécula **completa** (el grafo), no de un átomo individual.

* **Descubrimiento de Fármacos:** Las GNNs se utilizan para predecir si una molécula será **eficaz** (ej. se unirá a un objetivo proteico específico), **biodisponible** (ej. será absorbida por el cuerpo) y **no tóxica**. Al entrenar sobre miles de estructuras químicas conocidas, la GNN aprende las subestructuras que confieren propiedades deseables.
* **Materiales Ciencia:** Predicción de propiedades físicas (ej. dureza, estabilidad energética, banda prohibida) de nuevos compuestos o cristales, acelerando el descubrimiento de materiales. 

### C. Síntesis y Diseño Molecular

Modelos generativos basados en GNNs (como VAEs de grafos o GANs de grafos) pueden generar **nuevas estructuras moleculares** con propiedades deseadas específicas. Esto permite a los químicos explorar millones de posibles moléculas candidatas sin tener que sintetizarlas y probarlas en el laboratorio.

---

## 🛍️ 3. Sistemas de Recomendación: Recomendaciones Contextuales y de Ítems

Los sistemas de recomendación modernos se basan en grafos para modelar las complejas interacciones entre usuarios, ítems y sus atributos.

### A. Grafo de Interacción Bipartito

Un sistema de recomendación se puede modelar como un **grafo bipartito** donde los nodos se dividen en dos conjuntos:

* **Nodos de Usuario ($U$):** Representan a los clientes.
* **Nodos de Ítem ($I$):** Representan productos, películas o artículos.
* **Aristas:** Representan interacciones (ej. "el usuario A compró el ítem B", "el usuario C vio la película D").

### B. Inferencia de Preferencias y *Cold Start*

Las GNNs resuelven dos problemas clave en la recomendación:

1.  **Recomendación de Ítems Basada en Colaboración:** Las GNNs propagan información a través del grafo para inferir preferencias latentes. Si el **Usuario A** y el **Usuario B** interactuaron con ítems similares, sus *embeddings* se acercarán. Si el **Ítem C** y el **Ítem D** fueron comprados por usuarios similares, sus *embeddings* también se acercarán. La GNN aprende estas dependencias complejas de manera más efectiva que los métodos factoriales matriciales tradicionales.
2.  **Problema del *Cold Start*:** Cuando un nuevo ítem (sin interacciones previas) o un nuevo usuario (sin historial) aparece, las GNNs pueden utilizar las **características de los nodos** (ej. la descripción del nuevo ítem o la demografía del nuevo usuario) y propagar la información de sus pocos vecinos para generar un *embedding* útil. Esto resuelve la parálisis inicial que sufren los sistemas basados únicamente en el historial.

Las GNNs, por lo tanto, proporcionan un marco unificado para dar sentido a la estructura intrínseca de los datos, demostrando que la **relación** es tan importante como la **entidad** en sí misma, lo que conduce a sistemas más inteligentes y predictivos en una amplia gama de campos científicos y comerciales.


---

Continua: [[10-1-1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/10-1-1-aprender-politicas-de-sistemas.md)] 
