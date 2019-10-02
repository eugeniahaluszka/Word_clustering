# Word_clustering
Repositorio de proyectos realizados en la materia de "Text-mining".

Se elaboró una notebook para realizar word clustering, seleccionando el texto de "cien años de soledad" de Gabriel García Marquez.

Se inicia con el *preprocesamiento del corpus*:
    -Como se trabajó con un texto literario que no contiene signos/caracteres especiales tales como '%', '#', '&', etc, no fue necesaria su eliminación.
    -En cuanto a los signos de puntuación ',', '.', '¿?', etc, se decidió dejarlos en este paso para la correcta distinción de oraciones, pero será necesaria su eliminación para la matriz de coocurrencia.
    -Se trabajó con la librería SpaCy que nos provee tokenización y detección de stopwords automáticamente, no implementandolo por separado.

**Parsing del corpus***

Para el parseo del corpus se utilizó la librería SpaCy, donde primero se cargó el modelo de español y luego se le proveyó el corpus preprocesado. Esto nos provee un objeto tipo Doc, el cual posee todo el corpus tokenizado, junto a su información sintáctica y semántica que el mismo puede proveer. A partir de esta información, se procedió a armar el diccionario de features de las palabras.

En este punto se realiza:
  -*Tokenización*: significa dividir el texto en unidades mínimas significativas. Es un paso obligatorio antes de cualquier tipo de procesamiento.
  -*Análisis morfosintáctico y funcionalidad de cada palabra.*
  -*Se eliminan las oraciones con menos de 12 palabras*.
  -*Stopwords*: Identificamos las stopwords. Importamos la lista de palabras anterior, por lo que solo se trata de iterar a través de los tokens almacenados en el "Doc" objeto y realizar una comparación.
  -*Lematización de palabras*: Se usa un diccionario de lemas, ya que el lematizador que utiliza Scapy en español sólo transforma las palabras en lowercase. 
  -*Creación del diccionario*: Agregado de palabras al diccionario, aquellas que no sean números, puntuaciones o desconocidas. Cada palabra contiene un diccionario. Se toma como palabra de contexto una palabra a la izquierda y una a la derecha. En el diccionario se tiene en cuenta el tagging y su morfología, funcionalidad y triplas de dependencia.
  -*Eliminación de palabras poco frecuentes en el cuerpo del texto y en el contexto*: Esto se logra eliminando aquellas entre las que existe poca varianza.
  -Se procede a realizar la *vectorización* de los diccionarios de palabras.
  -Normalización de la matriz (número de ocurrencias totales de la columna sobre ocurrencias por cada fila).
  
**Clustering**
-Se selecciona el número de clusters = 15, 30 y 45 clusters.
-Se utiliza el algoritmo de k-means usando la distancia coseno para crear los clusters.
-Se observa la iteración a tres valores distintos de k.

**Resultados**
Los resultados obtenidos para el diccionario que contiene palabras de contexto (ventana de palabras 1, der e izq) y triplas se observó que en el gráfico los valores se encontraban muy dispersos sin identificar a simple vista ningún cluster.
Dentro de los 45 clústers se observó:
Clúster: *Stopwords*
['de', 'al', 'en', 'que', 'a', 'y', 'por', 'parir', 'con', 'del', 'sin', 'sobrar', 'hasta', 'En']

Clúster: *Familia Buendía*
['Aureliano', 'Buendía', 'mismo', 'José', 'Arcadio', 'Úrsula', 'último', 'cuartar', 'morir', 'Remedios', 'Segundo', 'Fernanda']

Clúster: *Verbos*
['llevar', 'dar', 'hacer', 'ver', 'tener', 'ir', 'encontrar', 'poner', 'pasar', 'regresar', 'decir', 'quedar']

Clúster: *Lugares*
['Macondo', 'mundo', 'cuyo', 'poblar']

Como podemos observar los clústers contienen palabras que corresponden al conjunto nombrado, como otras que no tienen nada que ver.
Se procede a cambiar el diccionario de palabras eliminando las triplas y utilizando como palabra de contexto una ventana de 2 palabras.

Al realizar este nuevo diccionario de palabras el gráfico mostró una dispersión diferente que parecía presentar clústers, pero mucha cantidad. Se probó con la misma cantidad de clústers que en el ejemplo anterior lo cuál no dió un buen resultado. Se empezó a trabajar en la ventana de palabras que era 150 y el resultado mejoró a medida que disminuyó ese valor.


