# Sustentación

## Solución del sistema lógico de apertura de la bóveda

**Función de entrada:** y = Σ(0, 1, 4, 5, 6, 7, 14, 15)

**Convención de notación:** el sistema trabaja con cuatro variables de entrada A, B, C y D, donde A es el bit más significativo y D el menos significativo, de manera que el peso de cada variable es A = 8, B = 4, C = 2, D = 1.

---

## 1. Extracción de la información secreta de acceso

Para descifrar la combinación se convierte cada número decimal a su representación binaria de cuatro bits, y a partir de ese binario se construye el término producto correspondiente. Cuando una variable vale 1 en el minitérmino aparece sin negar, y cuando vale 0 aparece negada.

| Minitérmino | A | B | C | D | Término producto |
|:---:|:---:|:---:|:---:|:---:|:---|
| m0 | 0 | 0 | 0 | 0 | A'B'C'D' |
| m1 | 0 | 0 | 0 | 1 | A'B'C'D |
| m4 | 0 | 1 | 0 | 0 | A'BC'D' |
| m5 | 0 | 1 | 0 | 1 | A'BC'D |
| m6 | 0 | 1 | 1 | 0 | A'BCD' |
| m7 | 0 | 1 | 1 | 1 | A'BCD |
| m14 | 1 | 1 | 1 | 0 | ABCD' |
| m15 | 1 | 1 | 1 | 1 | ABCD |

Uniendo los ocho términos mediante la operación OR se obtiene la función canónica en su forma de suma de productos:

**y = A'B'C'D' + A'B'C'D + A'BC'D' + A'BC'D + A'BCD' + A'BCD + ABCD' + ABCD**

Esta expresión canónica contiene ocho términos de cuatro literales cada uno, para un total de treinta y dos literales. Como referencia complementaria, la misma función puede escribirse en notación de maxitérminos como y = Π(2, 3, 8, 9, 10, 11, 12, 13), donde esos ocho números corresponden a las combinaciones que producen salida en estado bajo, es decir, aquellas que dejan la bóveda cerrada.

---

## 2. Tabla de verdad de la función original

La tabla siguiente relaciona las dieciséis combinaciones posibles de entrada con el estado de apertura de la bóveda. Se recorre en orden binario ascendente desde 0000 hasta 1111 para garantizar que ninguna combinación quede sin evaluar.

| Decimal | A | B | C | D | y |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | 0 | 0 | **1** |
| 1 | 0 | 0 | 0 | 1 | **1** |
| 2 | 0 | 0 | 1 | 0 | 0 |
| 3 | 0 | 0 | 1 | 1 | 0 |
| 4 | 0 | 1 | 0 | 0 | **1** |
| 5 | 0 | 1 | 0 | 1 | **1** |
| 6 | 0 | 1 | 1 | 0 | **1** |
| 7 | 0 | 1 | 1 | 1 | **1** |
| 8 | 1 | 0 | 0 | 0 | 0 |
| 9 | 1 | 0 | 0 | 1 | 0 |
| 10 | 1 | 0 | 1 | 0 | 0 |
| 11 | 1 | 0 | 1 | 1 | 0 |
| 12 | 1 | 1 | 0 | 0 | 0 |
| 13 | 1 | 1 | 0 | 1 | 0 |
| 14 | 1 | 1 | 1 | 0 | **1** |
| 15 | 1 | 1 | 1 | 1 | **1** |

De las dieciséis combinaciones posibles, ocho producen la apertura de la bóveda y ocho la mantienen cerrada, lo cual concuerda con la cantidad de minitérminos especificados en el mensaje codificado.

---

## 3. Simplificación mediante mapa de Karnaugh



### Mapa

| AB \ CD | 00 | 01 | 11 | 10 |
|:---:|:---:|:---:|:---:|:---:|
| **00** | 1 (m0) | 1 (m1) | 0 (m3) | 0 (m2) |
| **01** | 1 (m4) | 1 (m5) | 1 (m7) | 1 (m6) |
| **11** | 0 (m12) | 0 (m13) | 1 (m15) | 1 (m14) |
| **10** | 0 (m8) | 0 (m9) | 0 (m11) | 0 (m10) |

### Identificación de los grupos

La agrupación se realiza formando rectángulos de casillas con valor 1 cuyo tamaño sea una potencia de dos, buscando siempre los grupos más grandes posibles, ya que cada duplicación del tamaño del grupo elimina una variable adicional del término resultante.

**Primer cuarteto: m0, m1, m4, m5.** Este grupo ocupa las dos primeras columnas de las filas AB igual a 00 y AB igual a 01. Al revisar las cuatro casillas se observa que la variable A permanece constante en 0 y la variable C permanece constante en 0 en todas ellas, mientras que B y D toman ambos valores dentro del grupo. Las variables que cambian no influyen en el resultado y por lo tanto se eliminan del término, que queda reducido a **A'C'**.

**Segundo cuarteto: m6, m7, m14, m15.** Este grupo ocupa las columnas CD igual a 11 y CD igual a 10 de las filas AB igual a 01 y AB igual a 11. En las cuatro casillas la variable B permanece constante en 1 y la variable C permanece constante en 1, mientras que A y D varían libremente. El término se reduce entonces a **BC**.

### Justificación

Los dos cuartetos identificados cubren en conjunto las ocho casillas con valor 1 del mapa, de modo que no se requiere ningún grupo adicional. Tampoco es posible ampliar ninguno de los dos: si se intentara extender el primer cuarteto hacia las columnas de la derecha, la variable C dejaría de permanecer constante y el grupo perdería validez.

### Expresión simplificada

**y = A'C' + BC**

El resultado más notable de la simplificación es la desaparición completa de la variable D. Esto ocurre porque los minitérminos especificados vienen siempre en parejas consecutivas, concretamente 0 con 1, 4 con 5, 6 con 7 y 14 con 15, lo cual significa que para cada combinación de A, B y C aparecen tanto el caso D igual a 0 como el caso D igual a 1. En consecuencia, la variable D no aporta información alguna al criterio de apertura de la bóveda y el sistema abre independientemente de su valor.

La reducción lograda es considerable, ya que se pasa de ocho términos con cuatro literales cada uno, a solo dos términos de dos literales cada uno, es decir cuatro literales.

### Verificación algebraica de la simplificación

El mismo resultado puede obtenerse por manipulación algebraica sin recurrir al mapa. Agrupando primero los pares de términos canónicos que solo difieren en la variable D y aplicando la propiedad del complemento, según la cual D más D negada es igual a uno, se obtiene la expresión reducida A'B'C' + A'BC' + A'BC + ABC. Factorizando ahora A'C' en los dos primeros términos y BC en los dos últimos resulta A'C'(B' + B) + BC(A' + A), y aplicando nuevamente la propiedad del complemento sobre los paréntesis se llega a A'C' + BC, que coincide exactamente con el resultado obtenido por el mapa de Karnaugh.

---

## 4. Verificación de integridad del sistema

Para demostrar que la función simplificada es equivalente a la función original se realiza una comparación evaluando las dieciséis combinaciones posibles. Se calculan por separado los dos términos de la expresión simplificada y luego su unión mediante OR, para finalmente contrastar el resultado con la salida de la función canónica registrada en el punto 2.

| Decimal | A B C D | A'C' | BC | y simplificada | y original | Coincide |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 0 | 0 0 0 0 | 1 | 0 | 1 | 1 | Sí |
| 1 | 0 0 0 1 | 1 | 0 | 1 | 1 | Sí |
| 2 | 0 0 1 0 | 0 | 0 | 0 | 0 | Sí |
| 3 | 0 0 1 1 | 0 | 0 | 0 | 0 | Sí |
| 4 | 0 1 0 0 | 1 | 0 | 1 | 1 | Sí |
| 5 | 0 1 0 1 | 1 | 0 | 1 | 1 | Sí |
| 6 | 0 1 1 0 | 0 | 1 | 1 | 1 | Sí |
| 7 | 0 1 1 1 | 0 | 1 | 1 | 1 | Sí |
| 8 | 1 0 0 0 | 0 | 0 | 0 | 0 | Sí |
| 9 | 1 0 0 1 | 0 | 0 | 0 | 0 | Sí |
| 10 | 1 0 1 0 | 0 | 0 | 0 | 0 | Sí |
| 11 | 1 0 1 1 | 0 | 0 | 0 | 0 | Sí |
| 12 | 1 1 0 0 | 0 | 0 | 0 | 0 | Sí |
| 13 | 1 1 0 1 | 0 | 0 | 0 | 0 | Sí |
| 14 | 1 1 1 0 | 0 | 1 | 1 | 1 | Sí |
| 15 | 1 1 1 1 | 0 | 1 | 1 | 1 | Sí |

Las dieciséis filas de la comparación coinciden, lo cual constituye una demostración exhaustiva de que ambas funciones son lógicamente equivalentes. Dado que el espacio de entradas de un sistema de cuatro variables contiene exactamente dieciséis combinaciones y todas fueron evaluadas.

Esta equivalencia fue además confirmada experimentalmente mediante simulación del circuito, recorriendo las dieciséis combinaciones de los interruptores de entrada y verificando en cada caso el estado del indicador de salida, con resultados coincidentes en la totalidad de las pruebas.

---

## Tabla de verdad de la función simplificada

Se presenta la tabla de verdad construida directamente a partir de la expresión simplificada y = A'C' + BC, evaluando el circuito etapa por etapa. Las columnas intermedias corresponden a las señales físicas reales que existen en el montaje, lo cual permite usar esta tabla como referencia de diagnóstico durante las pruebas del circuito.

| Decimal | A | B | C | D | A' | C' | A'·C' | B·C | y = A'C' + BC |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 | 0 | **1** |
| 1 | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 0 | **1** |
| 2 | 0 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 |
| 3 | 0 | 0 | 1 | 1 | 1 | 0 | 0 | 0 | 0 |
| 4 | 0 | 1 | 0 | 0 | 1 | 1 | 1 | 0 | **1** |
| 5 | 0 | 1 | 0 | 1 | 1 | 1 | 1 | 0 | **1** |
| 6 | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 1 | **1** |
| 7 | 0 | 1 | 1 | 1 | 1 | 0 | 0 | 1 | **1** |
| 8 | 1 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| 9 | 1 | 0 | 0 | 1 | 0 | 1 | 0 | 0 | 0 |
| 10 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| 11 | 1 | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| 12 | 1 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| 13 | 1 | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 0 |
| 14 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 1 | **1** |
| 15 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 | **1** |

La columna de salida de esta tabla reproduce exactamente la columna de salida de la tabla de verdad original presentada en el punto 2, confirmando una vez más la equivalencia entre ambas expresiones. La comparación por parejas de filas consecutivas evidencia también el comportamiento de la variable D: las filas 0 y 1 producen el mismo resultado, al igual que las filas 4 y 5, las filas 6 y 7 y las filas 14 y 15, lo cual verifica que el valor de D no altera en ningún caso el estado de apertura de la bóveda.


## Enlaces videos

- Enlace video simulación de Tinkercad
    https://youtu.be/cACsvG3riek

- Enlace video montaje físico
    https://youtube.com/shorts/Jc7byJVHym8