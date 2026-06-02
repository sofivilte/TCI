markdown_content = """# 📋 Selección de Propuesta para el Modelado de Autómatas Finitos

## 🔹 Propuesta Elegida: **Delta Indexado por Estado (Propuesta 2)**

---

## 1. Introducción
En este informe se presenta la justificación técnica de la elección de la **Propuesta 2** para el modelado del Autómata Finito dentro del proyecto. El objetivo es analizar las ventajas de esta opción frente a las demás alternativas y fundamentar por qué es la que mejor se adapta al diseño arquitectónico y de programación que se viene desarrollando.

Recordemos que un autómata finito se define formalmente como un quintuplo:

$$\mathcal{A} = (Q, \Sigma, \delta, q_0, F)$$

De todos estos componentes, la **función de transición ($\delta$)** es el elemento más crítico a la hora de decidir el modelo de datos en memoria. Su representación determina directamente la legibilidad del código, la facilidad de mantenimiento y la eficiencia algorítmica al calcular las transiciones de estado al leer un símbolo.

---

## 2. ¿Qué propone la Propuesta 2?

Esta propuesta plantea modelar el autómata bajo el paradigma de **grafos**, donde el acceso a la función de transición se encuentra indexado directamente en cada estado. En lugar de mantener una lista global y centralizada con todas las transiciones del sistema, **cada estado es responsable de almacenar y gestionar sus propias transiciones salientes**.

Estructuralmente, cada estado se define como un nodo que contiene:
* **Identificador/Nombre** (ej. `str`)
* **Flag de Aceptación** (booleano que indica si es un estado final o no)
* **Lista de Transiciones** locales (únicamente las que parten de ese estado)



