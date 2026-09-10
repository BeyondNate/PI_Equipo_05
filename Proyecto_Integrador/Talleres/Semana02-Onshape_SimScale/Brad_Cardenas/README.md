# Actividad de Brad Cárdenas

## Modelado 3D en Onshape

Se realizó el modelado 3D de la **base de una caja correspondiente a un agitador magnético**, incluyendo la estructura y la tapa del modelo. El diseño fue preparado para posteriormente evaluar su resistencia frente a una fuerza aplicada sobre la estructura.

* [⚙️ Ver modelado Onshape](https://cad.onshape.com/documents/06947ddb91d11baa99cb7771/w/ae9234bc94d45c112f779e45/e/fcb191ef33bc070e9450e8b6?renderMode=0&uiState=6a90af5d569b9402d022c221)

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana02-Onshape_SimScale/Fotos/Brad_Onshape.jpeg" width="1000"/>

## Simulación en SimScale

### 🧪 Análisis y Simulación Estructural (FEA)

Para validar la carcasa del agitador magnético, se realizó el modelado 3D en **Onshape** y la simulación de elementos finitos en **SimScale** evaluando las condiciones reales de trabajo.

#### 1. Condiciones de Cargas y Entorno
* **Material:** PLA (*Polylactic Acid*).
* **Gravedad:** 9.81 m/s² (aplicada a la masa completa de la carcasa).
* **Carga en la Base ($F_{\text{base}}$):** Peso y soporte mecánico del circuito de control, motor e imanes de agitación.
* **Carga en la Tapa ($F_{\text{tapa}}$):** 5 N verticales en el centro de apoyo (peso del vaso de precipitados, fluido y pastilla magnética ≈ 500 g).
* **Fijación:** Base restrictiva fija (*Fixed Support*) sobre la superficie de trabajo.

#### 2. Resultados Obtenidos
* **Esfuerzo Von Mises Máximo Registrado:** 0.157 MPa (156,964 Pa).
* **Evaluación de Resistencia:** Considerando que el PLA presenta un límite elástico estándar de 40–60 MPa, la carcasa opera con un amplio factor de seguridad. La tapa no sufrirá pandeo ni deformación permanente bajo el peso de la muestra.

#### 3. Recomendaciones para Impresión 3D
* **Refuerzo Geométrico:** Agregar redondeos (*fillets*) en las uniones internas pared-tapa para mitigar la concentración de esfuerzos en las esquinas.
* **Parámetros de Laminado:** Utilizar relleno del 25%–30% (tipo giroide) y 3–4 paredes perimetrales.
* **Estabilidad Mecánica:** Añadir apoyos antideslizantes de goma en la base para absorber las vibraciones del motor interno.

* [🌍 Ver simulación en SimScale](https://www.simscale.com/workbench/?pid=5045070654597885458&rru=d1db9098-401d-452d-b447-91a27147070c&ci=9d7662a5-85af-4731-833f-1f5b0aa1d95a&mt=SIMULATION_RESULT&ct=SOLUTION_FIELD)

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana02-Onshape_SimScale/Fotos/Brad_SimScale.jpeg" width="1000">
