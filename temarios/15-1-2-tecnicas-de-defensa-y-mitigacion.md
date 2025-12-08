# 🛡️ Defensas y Mitigación: Blindando Modelos contra Ataques Adversarios

Los **Ejemplos Adversarios** representan una amenaza crítica para la seguridad e integridad de los sistemas de *Deep Learning* en aplicaciones sensibles. Las **Técnicas de Defensa y Mitigación** son esenciales para aumentar la robustez de los modelos y garantizar que se comporten de manera predecible y fiable ante *inputs* maliciosos.

Las estrategias de defensa se dividen generalmente en tres categorías: **Modificación del Entrenamiento**, **Modificación del Modelo** y **Detección de Entrada**.

## 1. Modificación del Entrenamiento: Entrenamiento Adversario

El **Entrenamiento Adversario (*Adversarial Training*)** es la técnica de defensa más efectiva y considerada el estándar de oro en la lucha contra los ataques adversarios. En lugar de cambiar la arquitectura del modelo, cambia el proceso por el cual el modelo aprende.

### A. Mecanismo de Funcionamiento

El entrenamiento adversario opera mediante un ciclo iterativo de dos pasos, similar a un juego min-max:

1.  **Generación de Ejemplos Adversarios:** En cada paso de entrenamiento (o cada pocas épocas), se genera un nuevo lote de **Ejemplos Adversarios** ($\mathbf{x}_{\text{adv}}$) para los datos del lote actual ($\mathbf{x}$). Esto generalmente se realiza utilizando un ataque **White-Box** fuerte y eficiente, como **PGD (*Projected Gradient Descent*)**.
2.  **Entrenamiento y Refuerzo:** El modelo se entrena para **clasificar correctamente** no solo los datos originales ($\mathbf{x}$), sino también sus correspondientes versiones adversarias ($\mathbf{x}_{\text{adv}}$). La función de pérdida se calcula sobre una mezcla de datos normales y adversarios.

### B. Beneficios y Desafíos

| Aspecto | Descripción |
| :--- | :--- |
| **Robustez Local** | El entrenamiento adversario **suaviza los límites de decisión** del modelo en las vecindades cercanas a los datos de entrenamiento, haciendo que el modelo sea intrínsecamente más resistente a pequeñas perturbaciones. |
| **Costo Computacional** | Es **muy costoso** computacionalmente. Generar ejemplos adversarios por gradiente en cada paso de entrenamiento aumenta el tiempo de entrenamiento en un factor significativo (a menudo 5x a 10x). |
| **Generalización** | Tiende a mejorar la robustez contra el tipo específico de ataque utilizado en el entrenamiento (ej. si se entrena con FGSM, es robusto a FGSM). Es deseable usar ataques iterativos fuertes como PGD para la generalización. |

---

## 2. Detección de Entrada Atípica (*Outlier Detection*)

Los métodos de detección no intentan hacer que el modelo clasifique el *input* adversario correctamente, sino que intentan **identificar la entrada perturbada** antes de que llegue al clasificador. Si el sistema detecta un *input* adversario con alta probabilidad, puede rechazarlo o dirigirlo a un clasificador más robusto o a un experto humano.

### A. Detección por Diferencias en las Activaciones

Los ejemplos adversarios a menudo residen en regiones de baja probabilidad de la distribución de datos original.

* **Mecanismo:** Se utiliza la capa de **Activaciones Internas** del modelo. Se entrena un clasificador binario simple (ej. una Máquina de Soporte Vectorial, SVM) para distinguir entre las activaciones de entradas legítimas y las activaciones de entradas adversarias.
* **Principio:** Aunque la salida final (la predicción) del modelo se ve afectada por el ataque, el patrón de activación en las capas intermedias puede ser notablemente diferente entre un dato normal y un dato ruidoso.

### B. Detección por Espacio de *Embedding*

Se evalúa la distancia del *input* respecto a la distribución de los datos de entrenamiento.

* **Mecanismo:** El *input* se proyecta a un espacio de *embedding* (una capa latente). Si la distancia del *input* a su vecino más cercano en el conjunto de entrenamiento es anormalmente grande (utilizando métricas como el **Score de Mahalanobis**), se etiqueta como atípico.
* **Ventaja:** Detecta *inputs* que están fuera de la distribución del entrenamiento (OOD) y que son un subproducto común de los ataques adversarios.

---

## 3. Otras Estrategias de Mitigación

### A. Gradiente Oculto (*Gradient Masking/Obfuscation*)

Esta es una defensa que busca hacer que el ataque **White-Box** falle.

* **Mecanismo:** El modelo se modifica para tener un gradiente inutilizable o nulo en los puntos de decisión críticos (ej. usando funciones no diferenciables o cuantificación).
* **Desafío:** Aunque inicialmente parece exitoso, los investigadores adversarios han demostrado que el **enmascaramiento de gradiente es una defensa débil**. Los atacantes pueden eludirlo usando ataques basados en la transferencia (*Black-Box*) o gradientes aproximados.

### B. Procesamiento de Entrada (*Input Preprocessing*)

Los *inputs* se "limpian" antes de ser alimentados al modelo.

* **Mecanismo:** Se aplican técnicas de reducción de ruido (ej. **Filtrado Gaussiano**, **Reducción Total de la Variación**) o se utilizan métodos basados en *Autoencoders* para eliminar la perturbación imperceptible.
* **Desafío:** Puede ser vulnerable si el atacante conoce el preprocesamiento y diseña el ruido para evadirlo.

## 4. Conclusión

La **Robustez Adversaria** no es un estado binario, sino un continuo. El **Entrenamiento Adversario** es la base más sólida para construir modelos resistentes, pero a menudo debe combinarse con **Técnicas de Detección de Entradas Atípicas** para manejar *inputs* que están completamente fuera de la distribución de entrenamiento y para mitigar ataques *Black-Box* más complejos. La investigación en este campo es una carrera armamentística constante entre atacantes y defensores.


---

Continua: [[15-2-1]()] 
