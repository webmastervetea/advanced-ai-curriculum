# 🧠 Modelos Gráficos Causales (DAGs) y el Cálculo Do: Formalizando la Causa y el Efecto

La **Inferencia Causal** es la rama de la estadística que busca determinar las relaciones de causa y efecto, respondiendo a la pregunta fundamental: "¿Qué pasaría si...?" A diferencia de la inferencia predictiva (que solo busca correlaciones), la causalidad requiere herramientas que permitan simular una **intervención** o una **manipulación**.

El marco formal para esto se basa en los **Gráficos Acíclicos Dirigidos Causales (DAGs)** y el **Cálculo Do ($\text{do}(\cdot)$)**.

## 1. Modelos Gráficos Causales (DAGs)

Un **Gráfico Acíclico Dirigido (DAG)** es una representación visual y matemática de las relaciones causales en un sistema.

### A. Elementos del DAG

* **Nodos (Variables):** Representan variables aleatorias en el sistema (ej. un tratamiento, una enfermedad, un ingreso).
* **Arcos Dirigidos (Flechas):** Representan una **relación causal directa**. Si existe una flecha de $X$ a $Y$ ($X \to Y$), esto significa que **$X$ es una causa directa de $Y$**. La aciclicidad significa que no puede haber bucles causales (una variable no puede causarse a sí misma).
* **Estructura Causal:** El DAG codifica supuestos cruciales sobre la independencia condicional en el sistema, específicamente, la ausencia de una flecha entre $X$ y $Y$ implica que $Y$ no es una causa directa de $X$ (o viceversa).

### B. Caminos y Bloqueo de Información (d-Separación)

La información fluye a lo largo de los caminos en el DAG, y ciertas variables pueden bloquear o permitir este flujo, un concepto llamado **d-Separación** (*d-Separation*). Los tres patrones causales básicos que definen el flujo de información son:

| Patrón | Representación | Nombre | Flujo de Información |
| :--- | :--- | :--- | :--- |
| **Cadena** | $X \to M \to Y$ | Mediador | La información fluye de $X$ a $Y$ a menos que $M$ sea observado. |
| **Bifurcación** | $X \leftarrow C \to Y$ | Confusor (*Confounder*) | El efecto de $C$ es espurio. La correlación $X-Y$ se bloquea al **condicionar** en $C$. |
| **Colisionador** | $X \to K \leftarrow Y$ | Colisionador (*Collider*) | La información **no fluye** entre $X$ y $Y$. La correlación se **crea** al **condicionar** en $K$. |

El **Confusor** ($C$) es el principal desafío de la inferencia observacional. La correlación observada entre $X$ y $Y$ no es causal, sino que es causada por $C$. Para obtener la relación causal real, debemos **ajustar** o **controlar** por $C$.

## 2. El Cálculo Do ($\text{do}(\cdot)$) y la Intervención

La diferencia crucial entre la **observación** y la **intervención** se formaliza mediante la introducción del **operador do**.

### A. Observación vs. Intervención

1.  **Observación ($P(Y | X=x)$):** La probabilidad de que $Y$ sea $y$ **dado que hemos observado** que $X$ tomó el valor $x$. Esto mide la **correlación**.
2.  **Intervención ($P(Y | \text{do}(X=x))$):** La probabilidad de que $Y$ sea $y$ **dado que hemos forzado** o **intervenido** en el sistema para fijar $X$ al valor $x$. Esto mide la **causalidad**.



### B. El Mecanismo de Intervención (Gráfico Mutilado)

El operador $\text{do}(X=x)$ se modela en el DAG mediante una **"cirugía" o mutilación** del gráfico:

* **Paso 1:** Se elimina cualquier flecha dirigida que **entre** en el nodo $X$. Esto refleja que el valor de $X$ ya no está determinado por sus causas originales; ha sido fijado externamente por el experimentador.
* **Paso 2:** El nodo $X$ se fija al valor $x$.
* **Paso 3:** Se calcula la probabilidad de $Y$ en este **gráfico mutilado**.

## 3. El Ajuste del Respaldo (*Backdoor Adjustment*)

El objetivo principal es encontrar la relación causal *a partir de datos observacionales*, es decir, encontrar una fórmula que permita calcular $P(Y | \text{do}(X=x))$ utilizando solo probabilidades observacionales $P(\cdot | \cdot)$.

La herramienta más famosa para esto es el **Criterio del Camino de Respaldo (*Backdoor Criterion*)**.

### A. El Criterio del Camino de Respaldo

Para calcular el efecto causal de $X$ en $Y$, necesitamos encontrar un conjunto de variables de control $\mathbf{Z}$ (el **conjunto de ajuste**) que cumpla dos condiciones:

1.  **Bloqueo de Respaldo:** El conjunto $\mathbf{Z}$ debe bloquear todos los **caminos de respaldo** entre $X$ y $Y$ (caminos que van de $X$ a $Y$ con una flecha saliendo de $X$).
2.  **No Creación de Sesgo:** $\mathbf{Z}$ no debe ser un descendiente de $X$.

Si se encuentra dicho conjunto $\mathbf{Z}$, el efecto causal se puede calcular mediante la **Fórmula de Ajuste del Respaldo**:

$$P(Y| \text{do}(X=x)) = \sum_{\mathbf{z}} P(Y|X=x, \mathbf{Z}=\mathbf{z}) P(\mathbf{Z}=\mathbf{z})$$

* Esta fórmula descompone el efecto causal en una suma ponderada de las probabilidades condicionales, ajustando por el efecto del confusor $\mathbf{Z}$.

## 4. Legado e Impacto

La formalización de la causalidad a través de DAGs y el Cálculo Do, principalmente por Judea Pearl, ha tenido un impacto profundo:

* **IA Explicable:** Permite a los modelos no solo predecir, sino también razonar sobre los mecanismos de decisión.
* **Ciencias Sociales y Economía:** Permite a los investigadores identificar qué variables deben ser controladas en un análisis de regresión para eliminar el sesgo de las variables confusoras.
* **ML Causal:** Las bibliotecas modernas (como DoWhy o CausalPy) implementan estos principios, permitiendo a los ingenieros de datos y científicos de ML realizar análisis causales rigurosos a partir de datos observacionales complejos.
