1. [¿Qué es ID3? ¿cómo funciona?](#definición-y-funcionamiento-del-algoritmo-id3)
2. [¿Que problemas tiene el algoritmo?](#problemas-del-id3)
3. [¿Como lo solucionamos?](#soluciones-propuestas)
4. [¿Como se ha implementado la solución?](#implementación-de-la-solución-propuesta)
   - [Establecer tamaño mínimo de datos](#condicionar-la-formación-del-árbol-de-decisión-según-la-cantidad-de-datos)
   - [Controlar la profundidad](#condicionar-la-formación-del-árbol-de-decisión-según-la-profundidad del-árbol)
5. [Matriz de confusión](#implementación-de-la-matriz-de-confusión)
6. [Ejemplos gráficos](#más-ejemplos)
7. [Bibliografía](#Bibliografía)



# Definición y funcionamiento del algoritmo ID3

ID3 es un algoritmo inventado por Ross Quinlan y se usa para generar árboles de decisión a partir de un conjunto de datos. En cada iteración del algoritmo itera sobre cada atributo no usado aun y calcula la entropía o la ganancia de información de ese atributo, posteriormente selecciona el atributo con la menor entropía, o en el caso de la ganancia de información el mayor.

A partir de ciertos **ejemplos observados en el mundo real**, y para cada uno de ellos tenemos una clasificación observada. ID3 consiste en un **árbol de decisión** que explique cada instancia de la secuencia de entrada de la manera más compacta posible a partir de una tabla de inducción y de forma que también en cada momento elige el mejor atributo dependiendo de una determinada heurística.

Un **árbol de decisión** está formado por **nodos de decisión** y **nodos de respuesta**:

* Un **nodo de decisión** esta asociado a los **atributos** y a la vez éste tiene varias **ramas** que salen de el que van a representar los **distintos valores que puede tomar el atributo**.
* Un **nodo** **repuesta** es una **hoja** que devuelve la decisión

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\ID3_C45 view.png)





# Problemas del ID3

Debido a que ID3 **siempre** termina haciendo el **árbol lo mas completo posible**, se termina produciendo un **sobreajuste** y como se ha visto en clase **no es nada bueno** ya que el algoritmo se queda ajustado a unas características muy especificas de los datos de entrenamiento, luego si queremos responder muestras nuevas dará peores resultados comparando con el caso de no sobre-ajustar.





# Soluciones propuestas

Para **intentar lidiar** con el sobreajuste vamos a crear 2 parámetros que influyan en la generación del árbol  intentando siempre que éste no se sobreajuste, para aplicar algunos ejemplos y entenderlos mejor, vamos a utilizar los siguientes datos:

### Ejemplo 1

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\datos.jpg)

* El **primer parámetro** hará referencia a la cantidad de información mínima que se necesita en cualquiera de las ramas para seguir desarrollando hacia abajo, si no se llega a este tamaño mínimo que nosotros pongamos esa rama dará lugar a un nodo hoja con el resultado expresado en porcentaje. Este porcentaje se consigue viendo sobre cada entrada correspondiente a una rama en concreto y viendo el resultado que obtenemos como respuesta.

  Con **tamaño mínimo** 6 si una rama dada no contiene al menos 6 entradas de datos, devolvemos los porcentajes correspondientes a la respuesta que obtenemos hasta esa parte.

  ![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\ID3_C45 view2.png)

* El **segundo parámetro** indicará la **profundidad máxima** del árbol, de forma que cuando una rama se expanda hasta llegar al límite puesto por el parámetro dará un resultado en forma de porcentaje al igual que en el parámetro anterior.

  Con **profundidad** 2 estamos obligando a las ramas a tener hasta profundidad 2 del árbol, lo que significa que cortamos lo que seguiría de la rama y colocamos directamente los porcentajes al igual que el primer parámetro.

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\ID3_C45 view3.png)



# Implementación de la solución propuesta

A continuación vamos a ver en NetLogo una explicación de la implementación de las soluciones nombradas anteriormente.

Para entender como se han implementado, tenemos que saber que tenemos una raza llamada `ID3:nodes` que hace referencia a los nodos del árbol de decisión, cada uno de estos nodos tiene unas propiedades como el tipo `ID3:node-type` (raíz, decisión o respuesta),  valor ` ID3:value ` (va a depender de que tipo de nodo es), profundidad  `depth` (profundidad alcanzada) y por último un atributo que va a guardar unos porcentajes como veremos a continuación, `ID3:porcentaje`.



## Condicionar la formación del árbol de decisión según la cantidad de datos

El algoritmo general funciona de forma recursiva, por lo que vamos a aplicar estos cambios a medida que se crea el árbol. Para detectar cuando una rama no tiene suficientes datos, vamos a comparar nuestro parámetro definido `minimum-size` con la cantidad de entradas que contengan el valor de esa rama, luego si la cantidad de entradas no llegan a nuestra restricción vamos a devolver una hoja con la probabilidad de lo que podía de devolver dicha rama.

Dentro del código hemos aplicado una serie de técnicas para conocer los diferentes valores que nos harán falta para calcular dicho porcentaje. En cuanto al atributo `ID3:porcentaje` vamos a asignarle un valor en caso de necesitar un porcentaje, en caso de devolver directamente una respuesta no es necesario utilizar dicho atributo. Voy a adelantar que este atributo nos servirá mas adelante para la matriz de confusión.

Lo único que tenemos que hacer para el cálculo de este porcentaje es obtener el número de elementos que contengan dicho valor de la rama, y contar las distintas respuestas que dan ya que de esta forma se puede calcular el porcentaje fácilmente. Además de esto hemos tenido que obtener cuantos tipos de respuestas distintos nos puede dar para hacerlo genérico. Dentro del código vamos a poder ver dicha implementación ya que está definida entre dos comentarios que lo indican.





## Condicionar la formación del árbol de decisión según la profundidad del árbol

Para implementar este parámetro vamos a necesitar aplicar las mismas técnicas que en la implementación del parámetro anterior, ya que hay que devolver igualmente un porcentaje en caso de que no se cumpla la profundidad que exijamos con el parámetro `profundidad`.

Lo único que vamos a tener que cambiar es la condición con la que comprobamos si la profundidad llega a los requisitos que nosotros le impongamos.





## Implementación de la matriz de confusión

La matriz de confusión es una tabla que nos muestra el rendimiento de nuestro algoritmo, ya que comparamos los resultados que nos da con los valores reales de los datos, es decir, con la respuesta correcta. 

La matriz consta de las filas que se nombran según las clases reales y las columnas según las clases previstas por el modelo, de esta forma en el cuerpo de la matriz vamos a contar cuantos casos hemos acertado y cuantos hemos confundido por otros, al tener la matriz n x n, siendo n el numero de posibles respuestas vamos a poder ver perfectamente que respuestas hemos confundido.

La diagonal principal siempre contendrá la suma de todas las predicciones correctas mientras que las otras celdas indicarán predicciones fallidas, al representar tanta información dentro de la matriz, ya que vemos todos los casos en los que se ha confundido, vamos a poder sacar diferentes tipos de error. Para ello además de tener una representación muy completa de como se ha desenvuelto nuestro algoritmo vamos a  calcular una serie de métricas que serán bastante útiles.

* **Precisión **se refiere a la dispersión del conjunto de valores obtenidos a partir de mediciones repetidas de una magnitud. Cuanto menor es la dispersión mayor la precisión. Se representa por la proporción entre el número de predicciones correctas (tanto positivas como negativas) y el total de predicciones.
* La **sensibilidad y la especificidad** son dos valores que nos indican la capacidad de nuestro estimador para discriminar los casos positivos, de los negativos. La sensibilidad es la fracción de verdaderos positivos, mientras que la especificidad, es la fracción de verdaderos negativos.
* **Exactitud** se refiere a lo cerca que está el resultado de una medición del valor verdadero. Se representa por la proporción entre los positivos reales predichos por el algoritmo y todos los casos positivos.
* La **Tasa de Verdaderos Negativos** se trata de los casos negativos que el algoritmo ha clasificado correctamente.







## Más ejemplos..

### Ejemplo 2

Para mostrar otro ejemplo vamos a considerar los siguientes datos:

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\datos1.jpg)

**Profundidad  2 y tamaño mínimo 5.**

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\ID3_C45 view4.png)

### Ejemplo 3

Para mostrar otro ejemplo vamos a considerar los siguientes datos:

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\datos2.jpg)





**Tamaño mínimo 4 y profundidad ilimitada.**

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\ID3_C45 view5.png)





**Tamaño indefinido y profundidad 3.**

![](C:\Users\Marcel\OneDrive - UNIVERSIDAD DE SEVILLA\GitHub\approximate-decision-trees\images\esquemaOtro.jpg)







# Bibliografía

http://www.cs.us.es/~fsancho/?e=104

https://www.nebrija.es/~cmalagon/inco/apuntes_mios/ejemplo_ID3_clase.pdf

[http://www.itnuevolaredo.edu.mx/takeyas/Apuntes/Inteligencia%20Artificial/Apuntes/IA/ID3.pdf](http://www.itnuevolaredo.edu.mx/takeyas/Apuntes/Inteligencia Artificial/Apuntes/IA/ID3.pdf)

https://es.slideshare.net/FernandoCaparrini/arboles-decision-id3

http://banzai-deim.urv.net/~riano/teaching/id3-m5.pdf

https://en.wikipedia.org/wiki/ID3_algorithm

https://es.wikipedia.org/wiki/Algoritmo_ID3

https://es.wikipedia.org/wiki/Sobreajuste

https://ccl.northwestern.edu/netlogo/docs/programming.html

https://empresas.blogthinkbig.com/ml-a-tu-alcance-matriz-confusion/