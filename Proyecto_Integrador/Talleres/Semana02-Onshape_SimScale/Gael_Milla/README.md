# Actividad de Gael Milla

## Modelado 3D en Onshape

Se realizó el modelado 3D de un **soporte triangular**, basado inicialmente en un soporte para vinos y posteriormente adaptado a las necesidades previstas del proyecto. El diseño fue modificado considerando su posible utilización como parte de la estructura del equipo.

* [⚙️ Ver modelado en Onshape](https://cad.onshape.com/documents/00e50ba611e377f51dc1281b/w/1d9c39c76b98d2c9560b802b/e/58ab4260c3cf800704ecc767?renderMode=0&uiState=6a90aea2ef930a37c4b2c2f1)

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana02-Onshape_SimScale/Fotos/Gael_Onshape.jpeg" width="1000"/>

## Simulación estructural en SimScale

Se realizó una **simulación estructural en SimScale** con el propósito de analizar el comportamiento del soporte triangular ante las cargas que podría experimentar durante su funcionamiento.

Para la simulación se utilizó **PLA como material del soporte** y se consideraron diferentes condiciones de carga:

- **Fuerza de gravedad de 9.81 m/s²**, considerando el peso propio de los elementos del soporte.
- **Carga adicional asociada al peso del elemento que será sostenido por el soporte: 13 N**, con el objetivo de representar una condición de funcionamiento más cercana a la que podría presentarse durante su utilización.

El análisis se realizó mediante el criterio de **esfuerzo de Von Mises**, permitiendo identificar las zonas donde se concentra una mayor cantidad de esfuerzo como consecuencia de las cargas aplicadas.

De acuerdo con la escala de resultados mostrada en la simulación, el **esfuerzo máximo de Von Mises alcanzó aproximadamente 84.81 kPa (84 810 Pa)**. Las zonas con mayor concentración de esfuerzo se encuentran principalmente cerca de las **uniones entre los elementos verticales y la estructura horizontal**.

Como se observa en la simulación, estas zonas presentan tonalidades **celestes y turquesas**, mientras que la mayor parte del soporte permanece en tonalidades azules, correspondientes a valores de esfuerzo inferiores.

Este resultado permite identificar los puntos críticos del diseño y sirve como referencia para futuras iteraciones del soporte. En particular, las zonas de unión podrían ser reforzadas o modificadas geométricamente para mejorar la distribución de las cargas y aumentar la resistencia de la estructura.

La simulación constituye una primera aproximación al comportamiento mecánico del soporte, por lo que los resultados pueden utilizarse como referencia para posteriores pruebas y mejoras del prototipo físico.

* [🌍 Ver simulación en SimScale](https://www.simscale.com/workbench/?pid=7395617598599356015&rru=4569c5a3-b2dd-4d76-8be7-84ad880149c1&ci=41b13397-48c0-45e3-bb2c-9c92ffa576ca&mt=SIMULATION_RESULT&ct=SOLUTION_FIELD)

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana02-Onshape_SimScale/Fotos/Gael_SimScaleVectorv2.jpg" width="1000"/>

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana02-Onshape_SimScale/Fotos/Gael_SimScalev2.jpg" width="1000"/>

---
