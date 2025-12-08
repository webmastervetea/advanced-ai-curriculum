# 👁️ Aprendizaje Basado en Contrastes: La Clave para Representaciones de Datos sin Etiquetas

El **Aprendizaje Auto-Supervisado (*Self-Supervised Learning* o SSL)** se ha convertido en una alternativa viable al Aprendizaje Supervisado tradicional. En lugar de depender de costosas etiquetas humanas, SSL genera sus propias **señales de supervisión** a partir de la estructura inherente de los datos.

Dentro de SSL, las **Técnicas Basadas en Contrastes (*Contrastive Learning*)** son la metodología dominante. Su objetivo fundamental es simple:

> **Las representaciones de las muestras similares (positivas) deben estar cerca en el espacio de *embedding*, mientras que las representaciones de las muestras diferentes (negativas) deben estar lejos.**

Este principio permite a los modelos aprender características ricas que son **invariantes** a ciertas transformaciones.

## 1. El Fundamento Teórico: La Pérdida Contrastiva

El corazón de los métodos contrastivos es la **Función de Pérdida de Información Contrastiva Normalizada y Escalonada (NT-Xent)**, introducida por SimCLR.

Esta función maximiza la similitud entre la representación de una **pareja positiva** y minimiza la similitud entre la pareja positiva y todas las demás muestras del lote (**muestras negativas**). La similitud se mide típicamente utilizando la **similitud coseno** y se escala mediante una **temperatura** ($\tau$).

La pérdida para un par de muestras positivas $(i, j)$ dentro de un lote $B$ de tamaño $N$ es:

$$\mathcal{L}_{i, j} = -\log \frac{\exp(\text{sim}(z_i, z_j) / \tau)}{\sum_{k=1}^{2N} \mathbb{1}_{k \neq i} \exp(\text{sim}(z_i, z_k) / \tau)}$$

Donde:
* $z_i$ y $z_j$ son las representaciones (*embeddings*) de las dos vistas de la misma imagen.
* La **Función de Similitud** $\text{sim}(u, v)$ es el producto punto (similitud coseno).
* El **denominador** suma las similitudes con todas las $2N-2$ muestras negativas del lote.

## 2. SimCLR: Simplicidad y Lotes Grandes

**SimCLR** (*A Simple Framework for Contrastive Learning of Visual Representations*, 2020) demostró que el SSL podía igualar o incluso superar el rendimiento del aprendizaje supervisado, manteniendo una arquitectura sorprendentemente simple.

### A. El Pipeline de SimCLR

1.  **Aumento de Datos (*Data Augmentation*):** Una imagen $\mathbf{x}$ se somete a **dos transformaciones aleatorias diferentes** (ej. recorte, rotación, color *jitter*, desenfoque gaussiano) para crear dos "vistas" correlacionadas, $x_i$ y $x_j$. Estas dos vistas forman la **pareja positiva**. 
2.  **Codificador (*Encoder*):** Ambas vistas ($x_i, x_j$) pasan a través de una red neuronal base (ej. ResNet) para obtener sus representaciones $\mathbf{h}_i$ y $\mathbf{h}_j$.
3.  **Cabeza de Proyección (*Projection Head*):** Las representaciones $\mathbf{h}$ pasan a través de una pequeña MLP (Multi-Layer Perceptron) para obtener los *embeddings* $\mathbf{z}_i$ y $\mathbf{z}_j$ en el espacio donde se aplica la pérdida contrastiva. (La MLP es crucial para mejorar la calidad de las representaciones $\mathbf{h}$).
4.  **Pérdida y Optimización:** La pérdida NT-Xent se aplica para atraer $\mathbf{z}_i$ y $\mathbf{z}_j$, mientras se repelen todos los demás *embeddings* del lote.

### B. Dependencia del Lote Grande

El éxito de SimCLR depende en gran medida de tener un **lote de entrenamiento muy grande**. Un lote grande proporciona un gran número de **muestras negativas** de forma natural (todas las demás imágenes en el lote, $2N-2$). Cuantas más negativas haya, mejor aprenderá el modelo a distinguir las características clave de la imagen.

* **Desventaja:** Requiere hardware extremadamente potente (ej. un clúster de TPU) para alcanzar el máximo rendimiento.

## 3. MoCo: Diccionarios de Colas y Modelos Clave

**MoCo** (*Momentum Contrast*, 2020) abordó la limitación de *batch size* de SimCLR introduciendo una solución más escalable: el **diccionario de colas dinámico** y el **codificador de impulso**.

### A. El Mecanismo de Diccionario

MoCo evita la necesidad de un lote gigante en la memoria de la GPU para obtener una gran cantidad de negativas. En su lugar, utiliza un **Diccionario de Colas (*Queue Dictionary*)** para almacenar representaciones negativas de lotes anteriores.

1.  **Consultas y Claves:** El *pipeline* tiene dos ramas:
    * **Codificador de Consulta (Query Encoder):** Entrenado directamente por gradiente. Produce la representación de la vista actual (consulta, $\mathbf{q}$).
    * **Codificador Clave (Key Encoder):** Sus pesos ($\theta_k$) no se actualizan por gradiente. Produce la representación de la pareja positiva y de las muestras negativas antiguas (claves, $\mathbf{k}$).
2.  **Diccionario de Colas:** Las representaciones $\mathbf{k}$ del codificador clave se empujan a una cola, que actúa como el diccionario de **muestras negativas**. Cuando se añade una nueva, la muestra más antigua se elimina (FIFO). Esto garantiza un gran y consistente número de negativas.

### B. Actualización por Impulso (*Momentum Update*)

Dado que el diccionario se compone de representaciones de modelos con pesos antiguos, el **Codificador Clave** se actualiza lentamente utilizando un promedio móvil exponencial (impulso) de los pesos del Codificador de Consulta ($\theta_q$).

$$\theta_k \leftarrow m \theta_k + (1 - m) \theta_q$$

Donde $m$ (impulso) es un valor cercano a 1 (ej. 0.999). Esta actualización suave asegura que las claves en el diccionario sigan siendo lo suficientemente coherentes como para ser negativas efectivas, sin introducir ruido masivo.

### C. Ventajas de MoCo

* **Escalabilidad:** Permite un *batch size* pequeño para la consulta, mientras se utilizan **miles de negativos** almacenados en el diccionario.
* **Eficiencia:** Requiere mucha menos memoria y puede entrenarse eficazmente en hardware de consumo.

## 4. Comparación y Legado

| Característica | SimCLR | MoCo |
| :--- | :--- | :--- |
| **Fuente de Negativas** | Otras muestras dentro del mismo **lote grande**. | **Diccionario de Colas** (muestras de lotes anteriores). |
| **Requisito de Memoria** | Muy Alto (necesita *batch size* $\geq 4096$). | Bajo (funciona con *batch sizes* pequeños). |
| **Actualización de Claves** | Por gradiente directo. | Por **impulso (momentum)**, que garantiza consistencia. |
| **Mejora Clave** | Importancia del aumento de datos y de la cabeza de proyección. | Escalabilidad a diccionarios grandes sin sacrificar hardware. |

Los métodos contrastivos han establecido el SSL como el nuevo estándar de facto para el pre-entrenamiento en visión por computadora (y NLP, con modelos como SimCSE), permitiendo que la mayoría de los datos no etiquetados se utilicen para generar representaciones de alta calidad.
