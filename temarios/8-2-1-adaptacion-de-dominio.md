# 🔄 Adaptación de Dominio (*Domain Adaptation*): Superando el Salto de Distribución

La **Adaptación de Dominio (*Domain Adaptation*)** es una rama del **Aprendizaje por Transferencia (*Transfer Learning*)** que se enfoca en resolver un problema fundamental en el *Machine Learning* aplicado: la **brecha de dominio**.

Esta brecha ocurre cuando un modelo entrenado en un conjunto de datos (el **Dominio Fuente**) tiene un rendimiento significativamente pobre cuando se aplica a un conjunto de datos diferente, pero relacionado (el **Dominio Objetivo**), debido a una disparidad en las distribuciones estadísticas de los datos.

## 1. El Problema de la Brecha de Dominio

En el *Machine Learning* tradicional, se asume que los datos de entrenamiento y los datos de prueba provienen de la **misma distribución** de probabilidad. Sin embargo, en la práctica, esta suposición a menudo se rompe:

* **Dominio Fuente ($D_S$):** El conjunto de datos grande y **etiquetado** donde el modelo es entrenado (ej. imágenes limpias de un catálogo profesional).
* **Dominio Objetivo ($D_T$):** El conjunto de datos de producción **no etiquetado** o con pocas etiquetas (ej. imágenes borrosas o con poca luz de una cámara de vigilancia real).

Aunque la **tarea** a realizar es la misma (ej. clasificación de objetos), la **distribución de los datos** ($P_S(\mathbf{X}) \neq P_T(\mathbf{X})$) ha cambiado. El objetivo de la Adaptación de Dominio es utilizar la información del Dominio Fuente para mejorar el rendimiento en el Dominio Objetivo con una mínima o nula intervención humana. 

## 2. Clasificaciones Clave de la Adaptación de Dominio

La adaptación se clasifica según la disponibilidad de etiquetas en el Dominio Objetivo:

| Clasificación | Etiquetas en $D_T$ | Descripción | Aplicación Típica |
| :--- | :--- | :--- | :--- |
| **Adaptación Supervisada** | Muchas | El Dominio Objetivo tiene suficientes datos etiquetados para un ajuste fino (*fine-tuning*) estándar. | Requiere el mayor costo de etiquetado. |
| **Adaptación Semi-Supervisada** | Pocas | El Dominio Objetivo tiene una pequeña cantidad de datos etiquetados que se usan junto con muchos datos sin etiquetar. | Situación intermedia, más realista. |
| **Adaptación No Supervisada (UDA)** | Ninguna | El Dominio Objetivo **no tiene etiquetas**. Toda la adaptación debe basarse en la alineación de características. | Más desafiante y de mayor interés en la investigación actual. |

---

## 3. Técnicas de Adaptación de Dominio No Supervisada (UDA)

La UDA es el subcampo más activo, ya que aborda el problema de la falta de etiquetas. Las técnicas se centran en alinear el espacio de características de los dos dominios.

### A. Métodos Basados en Discrepancia

Estos métodos intentan minimizar una métrica que cuantifica la distancia entre las distribuciones de características de los dos dominios en el espacio de *embedding*.

* **MMD (Maximum Mean Discrepancy):** Utiliza funciones *kernel* para medir la distancia entre las medias de las características de $D_S$ y $D_T$ en un espacio de Hilbert de características. El modelo se entrena para minimizar la pérdida de clasificación en $D_S$ y simultáneamente minimizar la distancia MMD entre $D_S$ y $D_T$.

### B. Métodos Basados en Aprendizaje Adversario (DAAN)

Esta es la técnica más popular y se inspira en las **Redes Generativas Adversarias (GANs)**.

1.  **Extractor de Características ($G_f$):** Una red neuronal (el generador) que mapea la entrada al espacio de *embedding*.
2.  **Clasificador de Dominio ($D_d$):** Una red discriminadora que intenta predecir de qué **dominio** (Fuente o Objetivo) proviene una característica dada.
3.  **Entrenamiento:**
    * El **Clasificador de Dominio** ($D_d$) se entrena para ser lo más preciso posible.
    * El **Extractor de Características** ($G_f$) se entrena para **engañar** al clasificador de dominio, generando representaciones que son indistinguibles entre los dos dominios.

Al maximizar la pérdida de $D_d$ (su error), el extractor $G_f$ produce **representaciones invariantes al dominio**. El clasificador final se entrena únicamente en las etiquetas de $D_S$ utilizando estas representaciones invariantes.

* **Ejemplos:** **DANN** (*Domain-Adversarial Neural Network*) es el ejemplo prototípico.

### C. Métodos Basados en Pseudo-Etiquetas

Estos métodos utilizan el modelo entrenado en $D_S$ para generar etiquetas para el Dominio Objetivo, que luego se utilizan para auto-entrenar el modelo.

1.  **Predicción:** El modelo entrenado en $D_S$ genera predicciones para $D_T$.
2.  **Filtrado:** Solo las predicciones con **alta confianza** (pseudo-etiquetas) se mantienen.
3.  **Ajuste Fino:** El modelo se ajusta utilizando la pérdida de clasificación en el Dominio Objetivo con estas pseudo-etiquetas de alta confianza.
4.  **Iteración:** El proceso se repite, mejorando la calidad de las pseudo-etiquetas en cada iteración.

---

## 4. Aplicaciones Críticas de la Adaptación de Dominio

La Adaptación de Dominio es indispensable en escenarios donde la recolección de datos etiquetados en el entorno operativo es costosa, inviable o peligrosa:

* **Visión por Computadora:**
    * **Simulación a Realidad (*Sim-to-Real*):** Adaptar modelos entrenados en entornos de simulación (coches autónomos, robótica) a datos del mundo real.
    * **Cambios de Estilo:** Adaptar un clasificador entrenado en fotos de día a fotos de noche, o de fotos a dibujos/bocetos.
* **Procesamiento de Lenguaje Natural (NLP):**
    * **Traducción entre Variantes:** Adaptar un modelo entrenado en inglés británico a inglés americano, o de español de España a variantes latinoamericanas.
    * **Cambios de Género:** Adaptar un modelo entrenado en noticias formales a texto de redes sociales o *blogs*.
* **Diagnóstico Médico:** Adaptar un modelo entrenado en un conjunto de datos de imágenes de un hospital a las imágenes de otro hospital, donde el equipo (y por lo tanto la distribución de píxeles) es diferente.

## 5. El Futuro de la Adaptación

Las técnicas modernas se están moviendo hacia la **Adaptación de Dominio Generalizado (*Generalized Domain Adaptation*, GDA)**, donde no solo se asume que las distribuciones son diferentes, sino que el modelo debe funcionar bien tanto en el Dominio Fuente como en el Dominio Objetivo después de la adaptación. El objetivo final es la creación de modelos que sean robustos ante cualquier cambio de distribución no anticipado.


---

Continua: [[8-2-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/8-2-2-few-shot-y-zero-shot.md)] 
