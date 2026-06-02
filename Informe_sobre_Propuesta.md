# Selección de Propuesta para el Modelado de Autómatas Finitos

## Propuesta Elegida: **Delta Indexado por Estado (Propuesta 2)**

---

## 1. Introducción
En este informe voy a explicar por qué elegí la **Propuesta 2** para modelar el Autómata Finito en el proyecto. La idea es entender qué propone cada opción y argumentar cuál se adapta mejor al trabajo que venimos planteando.

Recordemos que un autómata finito se define como:

$$A = (Q, \Sigma, \delta, q_0, F)$$

De todos esos componentes, la **función de transición ($\delta$)** es la más importante a la hora de decidir cómo modelar la estructura, porque determina cómo el autómata pasa de un estado a otro al leer un símbolo. Sabemos que, según cómo representemos $\delta$ en memoria, el código puede quedar más o menos claro y más o menos eficiente.

---

## 2. Qué propone la Propuesta 2
Lo que entendí de la idea de esta propuesta es que se va a modelar el autómata como un **grafo**. En vez de tener una lista global con todas las transiciones juntas, cada estado guarda sus propias transiciones. Es decir, cada estado es un nodo que tiene:

* Su nombre.
* Si es o no estado de aceptación.
* La lista de transiciones que salen de él.

Entonces, para consultar $\delta(q, a)$ el proceso sería en dos pasos:

1.  Primero se busca el estado $q$ en la lista de estados del autómata.
2.  Una vez encontrado, se recorre *sólo* la lista de transiciones de ese estado buscando el símbolo $a$.

Esto es mucho más ordenado que recorrer una lista enorme con todas las transiciones mezcladas. Cada estado es responsable de sus propias transiciones, lo que refleja directamente la idea del grafo para dibujar un autómata.

> **Ventaja importante:** El campo de estado destino dentro de cada transición puede ser tanto un único estado como un conjunto de estados, sin cambiar la estructura. Eso permite modelar tanto AFD como AFND con la misma base.

---

## 3. Por qué la elegí

### Encaja con la forma en que programo
Durante el proyecto aprendí que me resulta más natural resolver problemas con recursión. Buscar el estado $q$ en una lista y luego buscar el símbolo $a$ en la lista de transiciones de ese estado son dos problemas que se pueden resolver cada uno con su propia función recursiva, con su caso base claro. En el proyecto del año pasado, porque soy recursante, realicé en conjunto con mi grupo funciones como `hat_delta` ($\hat{\delta}$) y `tabla_transiciones` que siguieron exactamente esa lógica, y funcionaron bien. La Propuesta 2 me permite mantener ese estilo, cambiando el enfoque de lo que hacía la propuesta 1, que es prácticamente la que desarrollé el año pasado.

### Se integra con lo que ya tengo
El proyecto ya tiene implementado el tipo `Tdata` con `STR`, `SET` y `LIST`. Los nombres de los estados son `str`, los conjuntos de estados destino son `SET`, y las listas de estados y transiciones son listas enlazadas, que es el tipo de estructura con la que ya trabajamos. No hace falta inventar nada nuevo desde cero, la Propuesta 2 se apoya en lo que ya existe.

### Es más eficiente que la Propuesta 1
En la Propuesta 1, para consultar $\delta$ hay que recorrer toda la lista de transiciones del autómata. En la Propuesta 2, la búsqueda queda limitada a las transiciones del estado que se está procesando. Esto quizá importa especialmente en la conversión de AFND a AFD, donde se consulta $\delta$ muchas veces y el autómata puede tener bastantes transiciones.

### Se adapta bien a la conversión AFND a AFD
Para convertir un AFND a AFD necesitamos aplicar $\delta$ sobre conjuntos de estados y acumular los destinos. Con esta propuesta, podemos consultar la $\delta$ de cada estado del conjunto de forma independiente y luego unir los resultados usando las operaciones de `SET` que ya están implementadas. No necesitamos cambiar la estructura para soportar esto.

---

## 4. Comparación con las otras propuestas

La siguiente tabla resume las diferencias más relevantes (a mi criterio). El símbolo (**) indica la propuesta seleccionada.

| Criterio | Propuesta 1 | Propuesta 2 (**) | Propuesta 4 | Propuesta 5 |
| :--- | :---: | :---: | :---: | :---: |
| **Búsqueda $\delta$** | Lista completa | **Solo el estado** | Lista completa | Lista completa |
| **Recursión** | Media | **Alta** | Media | Media |
| **Uso de `Tdata`** | Directo | **Directo** | Directo | Parcial |
| **Modela AFND** | Sí | **Sí** | Sí | Parcial |
| **Complejidad de impl.**| Baja | **Media** | Baja-Media | Alta |

* La **Propuesta 1** se descartó porque es la más básica y no aprovecha una organización más inteligente de la información. 
* La **Propuesta 4** resulta muy similar a la 1 cuando se implementa con listas enlazadas. 
* La **Propuesta 3** (no incluida en la tabla) requiere convertir estados y símbolos a índices enteros, lo que rompe el uso natural de `str` y `Tdata`. 
* La **Propuesta 5** es interesante pero creo que está pensada solo para la etapa de conversión y agrega cierta complejidad de diseño ya que la conversión puede resolverse con la Propuesta 2.

---

## 5. Conclusión
Elegí la **Propuesta 2** porque organiza el autómata de una manera que me resulta clara y natural, se integra sin problemas con los TADs que ya tenemos, es compatible con un estilo recursivo de programación y modela tanto AFD como AFND sin necesidad de cambiar la estructura `Tdata`. Creo que es una base sólida para terminar el proyecto y también para encarar la conversión de AFND a AFD.
