# 🌌 Conceptos Fundamentales de la Computación Cuántica

La **Computación Cuántica** es un paradigma radicalmente diferente del cómputo clásico. En lugar de utilizar bits que solo pueden representar 0 o 1, aprovecha las leyes de la mecánica cuántica para realizar cálculos en paralelo, ofreciendo el potencial para resolver problemas actualmente intratables para las supercomputadoras. Los tres pilares de este poder son el **Qubit**, la **Superposición** y el **Entrelazamiento**.

## 1. El Qubit (*Quantum Bit*)

El **Qubit** (Bit Cuántico) es la unidad básica de información en la computación cuántica, el análogo cuántico del bit clásico (que almacena un valor de 0 o 1).

### A. Representación

Un qubit representa el estado cuántico de una partícula (como un electrón o un fotón). Matemáticamente, un qubit es un vector de dos dimensiones que se representa como:

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$$

* $|0\rangle$ y $|1\rangle$ son los estados base clásicos.
* $\alpha$ y $\beta$ son números complejos llamados **amplitudes de probabilidad**.
* La suma de los cuadrados de las magnitudes de estas amplitudes debe ser igual a uno: $|\alpha|^2 + |\beta|^2 = 1$.



### B. Medición

La naturaleza cuántica del qubit solo se manifiesta mientras no se le observe. En el momento en que se **mide** el qubit, su estado colapsa instantáneamente a un estado clásico (0 o 1) con las siguientes probabilidades:

* Probabilidad de medir $|0\rangle$: $|\alpha|^2$
* Probabilidad de medir $|1\rangle$: $|\beta|^2$

---

## 2. Superposición

La **Superposición** es la capacidad de un qubit de existir en una **combinación lineal** de sus dos estados base, $|0\rangle$ y $|1\rangle$, simultáneamente.

### A. La Analogía del Interruptor

Mientras que un interruptor clásico solo puede estar en la posición "apagado" (0) o "encendido" (1), un qubit en superposición puede estar, metafóricamente, **parcialmente encendido y parcialmente apagado** al mismo tiempo.

### B. Paralelismo de Cómputo

La superposición es crucial porque permite que un registro de $n$ qubits almacene $2^n$ estados distintos **a la vez**.

* Un registro de **3 bits** clásicos puede almacenar un solo número de 0 a 7.
* Un registro de **3 qubits** en superposición puede almacenar **todos los números de 0 a 7 simultáneamente**.

Esto permite que un ordenador cuántico realice cálculos sobre una inmensa cantidad de *inputs* en una sola operación, un fenómeno conocido como **paralelismo cuántico**.

---

## 3. Entrelazamiento (*Entanglement*)

El **Entrelazamiento** es quizás el concepto más misterioso y poderoso de la mecánica cuántica, y fue llamado por Einstein "acción fantasmal a distancia". Ocurre cuando dos o más qubits quedan **conectados de tal manera que el estado cuántico de uno no puede describirse independientemente del estado de los otros**, incluso si están separados por grandes distancias.

### A. El Vínculo Cuántico

* Si dos qubits están entrelazados, y se mide el estado de uno de ellos (haciendo que colapse a 0 o 1), el estado del otro qubit colapsa **instantáneamente** al estado correlacionado correspondiente, sin importar la distancia entre ellos.

* **Ejemplo:** Si un par entrelazado está en el estado $(|00\rangle + |11\rangle)/\sqrt{2}$, y medimos el primer qubit como $|1\rangle$, sabemos **inmediatamente** que el segundo qubit también es $|1\rangle$.

### B. El Poder del Entrelazamiento

El entrelazamiento es lo que permite que una computadora cuántica coordine el cómputo masivamente paralelo realizado por los qubits en superposición. Sin él, la capacidad de cómputo del sistema no crecería exponencialmente.

El entrelazamiento es la base de algoritmos poderosos como el **Algoritmo de Shor** (para factorización) y el **Algoritmo de Grover** (para búsqueda en bases de datos).

---

## 4. Resumen de la Distinción

| Concepto | Análogo Clásico | Rol en Cómputo Cuántico |
| :--- | :--- | :--- |
| **Qubit** | Bit | Unidad de almacenamiento de información. |
| **Superposición** | Interruptor encendido/apagado | Permite **almacenar** $2^n$ valores simultáneamente. |
| **Entrelazamiento** | Conexión lógica entre bits | Permite **procesar** $2^n$ valores simultáneamente, coordinando el cómputo. |

Juntos, estos tres conceptos permiten a la computación cuántica manipular la información de formas que el cómputo clásico simplemente no puede replicar, abriendo la puerta a nuevas soluciones en química, farmacéutica y optimización.


---

Continua: [[17-1-2](https://github.com/webmastervetea/advanced-ai-curriculum/blob/main/temarios/17-1-2-algoritmos-cuanticos.md)] 
