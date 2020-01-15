1. [¿Qué es ID3? ¿cómo funciona?](#definición-y-funcionamiento-del-algoritmo-id3)

2. [¿Que problemas tiene el algoritmo?](#problemas-del-id3)
3. [¿Como lo solucionamos?](#soluciones)
4. [¿Como se ha implementado la solución?](#implementación-de-la-solución-propuesta)



















# Definición y funcionamiento del algoritmo ID3

ID3 es un algoritmo inventado por Ross Quinlan y se usa para generar árboles de decisión a partir de un conjunto de datos. En cada iteracion del algoritmo itera sobre cada atributo no usado aun y calcula la entropia o la ganancia de información de ese atributo, posteriormente selecciona el atributo con la menor entropía, o en el caso de la ganancia de información el mayor.

A partir de ciertos ejemplos observados en el mundo real, y para cada uno de ellos tenemos una clasificación observada. ID3 consiste en un **árbol de decisión** que explique cada instancia de la secuencia de entrada de la manera más compacta posible a partir de una tabla de inducción y de forma que también en cada momento elige el mejor atributo dependiendo de una determinada heurística.

Un árbol de decisión está formado por nodos de decisión y nodos de respuesta:

* Un nodo de decisión esta asociado a los atributos y a la vez éste tiene varias ramas que salen de el que van a representar los distintos valores que puede tomar el atributo.
* Un nodo repuesta es una hoja que devuelve la decisión

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\ID3_C45 view.png)





# Problemas del ID3

Debido a que ID3 siempre termina haciendo el árbol lo mas completo posible, se termina produciendo un sobreajuste y como se ha visto en clase no es nada bueno ya que el algoritmo se queda ajustado a unas caracteristicas muy especificas de los datos de entrenamiento, luego si queremos respoder muestras nuevas dará peores resultados comparando con el caso de no sobreajustar.

Para intentar corregir esto, vamos a implementar 2 parámetros con los cuales podremos modificar la generacion del arbol de decision siempre intentando que este no se sobreajuste.





# Soluciones

Para intentar lidiar con el sobreajuste vamos a crear 2 parámetros que influyan en la generacion del arbol:

* El primer parámetro hará referencia a la cantidad de información minima que se necesita en cualquiera de las ramas para seguir desarrollando hacia abajo, si no se llega a este tamaño minimo que nosotros pongamos esa rama dará lugar a un nodo hoja con el resultado expresado en porcentaje. Este porcentaje se consigue viendo sobre cada entrada correspondiente a una rama en concreto y viendo el resultado que obtenemos como respuesta.

  Con **tamaño minimo** 6 si una rama dada no contiene al menos 6 entradas de datos, devolvemos los porcentajes correspondientes a la respuesta que obtenemos hasta esa parte.

  ![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\ID3_C45 view2.png)

* El segundo parámetro indicará la profundidad máxima del árbol, de forma que cuando una rama se expanda hasta llegar al límite puesto por el parámetro dará un resultado en forma de porcentaje al igual que en el parámetro anterior.

  Con **profundidad** 2 estamos obligando a las ramas a tener hasta profundidad 2 del árbol, lo que significa que cortamos lo que seguiría de la rama y colocamos directamente los porcentajes al igual que el el primer parámetro.

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\ID3_C45 view3.png)



# Implementación de la solución propuesta