# 🌌 Redes Neuronales Cuánticas (QNNs): Integrando IA y Cuántica

Las **Redes Neuronales Cuánticas (*Quantum Neural Networks*, QNNs)** son un campo emergente dentro del **Aprendizaje Automático Cuántico (*Quantum Machine Learning*, QML)** que busca fusionar el poder de procesamiento del **Aprendizaje Profundo (*Deep Learning*)** con los principios de la **Mecánica Cuántica** (superposición, entrelazamiento e interferencia).

El objetivo principal de las QNNs es aprovechar las propiedades cuánticas para realizar cálculos sobre los datos de una manera fundamentalmente diferente a las redes neuronales clásicas (ANNs), ofreciendo el potencial de una **ventaja cuántica** en términos de capacidad de *features*, eficiencia o velocidad de convergencia.

## 1. Arquitectura de las QNNs: Modelos Híbridos

Actualmente, la mayoría de las QNNs que se ejecutan en *hardware* cuántico real (*NISQ - Noisy Intermediate-Scale Quantum*) son **modelos híbridos cuántico-clásicos**.

### A. Componentes Clave

1.  **Codificación de Datos Cuánticos (*Quantum Data Encoding*):** El *input* clásico ($\mathbf{x}$) debe ser mapeado o codificado en un estado cuántico inicial de los qubits ($|\psi_0\rangle$). Esto se logra mediante una secuencia de puertas de rotación y/o entrelazamiento.
2.  **Circuito Variacional (*Ansatz*):** El corazón de la QNN es un circuito cuántico parametrizado, $\mathbf{U}(\mathbf{\theta})$. Este circuito consiste en capas de **puertas cuánticas** (análogas a las capas de una ANN) que aplican transformaciones a los qubits utilizando parámetros ajustables ($\mathbf{\theta}$).
3.  **Medición y Función de Costo:** La salida cuántica se obtiene al medir los qubits. La medición es una distribución de probabilidad que se utiliza para calcular una **Función de Costo (o pérdida)** clásica.
4.  **Optimizador Clásico:** Un ordenador clásico ajusta los parámetros $\mathbf{\theta}$ del Ansatz para minimizar la función de costo, cerrando el ciclo de retroalimentación (análogo al **Descenso de Gradiente**).



## 2. El Poder del Procesamiento Cuántico

El potencial de las QNNs radica en la forma en que los principios cuánticos operan dentro del Ansatz.

### A. Superposición y Paralelismo de Cómputo

Cuando un qubit está en **superposición**, un único circuito cuántico puede operar sobre **todos los posibles estados** de los datos simultáneamente.

* **Principio:** Si un Ansatz de $n$ qubits está en superposición, la QNN procesa $2^n$ *inputs* a la vez en un solo paso de cómputo.
* **Resultado:** Esto podría acelerar el entrenamiento o la búsqueda de *features*, aunque el resultado final aún debe extraerse mediante medición, lo que colapsa el estado.

### B. Entrelazamiento y Espacio de *Features*

El **Entrelazamiento** es quizás el recurso cuántico más valioso. Las puertas de entrelazamiento (como la puerta CNOT) crean correlaciones no locales entre los qubits, transformando los datos de entrada a un **espacio de *features* cuántico** de muy alta dimensión.

* **Ventaja:** Esta transformación no lineal, facilitada por el entrelazamiento, puede revelar patrones en los datos que son invisibles o intratables de codificar por redes neuronales clásicas, lo que potencialmente resuelve problemas que no son linealmente separables en el espacio clásico.

### C. Interferencias Cuánticas (Función de Activación)

En una ANN, una función de activación (ej. ReLU) introduce la no linealidad. En las QNNs, la **Interferencia Cuántica** actúa como el mecanismo que permite que las probabilidades de los estados cuánticos sean manipuladas de forma no lineal.

* **Mecanismo:** La suma de las amplitudes de probabilidad en un circuito genera la interferencia. Esta interferencia es lo que modula la probabilidad de medir un estado específico, lo que permite a la QNN aprender.

---

## 3. Tipos Específicos de QNNs

Los **Clasificadores Cuánticos Variacionales (VQC)** son la forma más común de QNNs en la práctica actual.

* **VQC como Clasificador:** El Ansatz se diseña para mapear los *inputs* a estados que, al medirse, indican la clase de pertenencia. Los VQC se utilizan para tareas de clasificación binaria y multiclase.
* **QNNs como Modelos Generativos:** Se están investigando modelos como las **Redes Neuronales Cuánticas Generativas Adversariales (QGANs)**, donde el generador y el discriminador son ambos circuitos cuánticos o híbridos.

## 4. Desafíos Clave

A pesar de su potencial teórico, las QNNs enfrentan obstáculos significativos:

* **El Problema del *Barren Plateau***: A medida que la QNN se escala (más qubits o más profundidad), el gradiente de la función de costo se vuelve extremadamente pequeño, lo que hace que el optimizador clásico no pueda entrenar el modelo de manera efectiva.
* **Ruido y Decoherencia:** Los procesadores cuánticos NISQ son ruidosos. El ruido causa la **decoherencia**, haciendo que el estado cuántico se corrompa antes de que se complete el cálculo.
* **Codificación de Datos:** Codificar grandes conjuntos de datos clásicos en el número limitado de qubits disponibles es un cuello de botella importante.

Las Redes Neuronales Cuánticas representan un esfuerzo ambicioso para infundir la robustez del aprendizaje profundo con la capacidad de procesamiento exponencial de la cuántica, con el potencial de transformar la forma en que abordamos problemas de análisis de datos a gran escala en el futuro.


---

Continua: [[18-1-1](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/18-1-1-algoritmos-de-busqueda-avanzados.md)] 
