# Approximate Decision Trees
## Enunciado
Un problema que tiene el algoritmo ID3/C4.5 tal y como lo hemos visto es que puede producirse un sobreajuste ya que termina realiza el árbol más completo posible con los datos disponibles. Modifica el algoritmo visto en clase para que construya árboles de decisión aproximados. Para ello, haz que la construcción del árbol dependa de los parámetros siguientes (busca en la literatura si hay más opciones): un parámetro que indique el tamaño mínimo de los conjuntos de datos para que se consideren directamente hojas, otro parámetro que establezca una profundidad máxima en el árbol construido (una vez alcanzada esa profundidad, el algoritmo considera que los nodos alcanzados son hojas y da una respuesta por mayoría). En este caso, una opción interesante sería que la salida del árbol ya no sea una clasificación concreta, sino la probabilidad de que sea de una clase u otra (un vector de probabilidades). Añadir, además, las funciones necesarias para que calcule la matriz de confusión del modelo obtenido sobre un conjunto de test que se le pase.
