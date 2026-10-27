# CLARIFICACIÓN DE AGUA CON QUITOSANO

### Principios científicos del sistema automatizado · TRL 1

Proyecto Integrador 2026-II | Universidad Peruana Cayetano Heredia | Equipo 05

27 de octubre de 2026

---

## Ficha del proyecto

| Campo | Información |
|:---|:---|
| **Curso** | Proyecto Integrador 2026-II |
| **Institución** | Universidad Peruana Cayetano Heredia |
| **Tema** | TRL 1 · Sistema automatizado de clarificación de agua mediante dosificación de quitosano |
| **Fecha** | 27/10/2026 |

**Integrantes:**

- Marcelo Alarcón Camones
- Brad Cárdenas Parián
- Leonel Urbano Castillo
- Idania Parhuay Meza
- Gael Milla Fasabi

**Ingenieros:** Umbert Lewis De La Cruz, Maria Rejas, Harry Rivera y Renzo Chan

---

## Presentación del proyecto

La tecnología que proponemos estudiar es un sistema automatizado para clarificar agua turbia mediante la dosificación de quitosano. El agua cruda de referencia es agua superficial del río Pachacamac. El sistema mide el pH y la turbidez de la muestra, determina una dosis de quitosano a partir de datos obtenidos por experimentación, la suministra con una bomba peristáltica, mezcla, deja sedimentar y vuelve a medir la turbidez para evaluar el resultado. Un ESP32 coordina la adquisición de las señales y el control de los actuadores, y un sensor de nivel confirma que el reactor alcance el volumen previsto.

De acuerdo con la metodología de CONCYTEC, el TRL 1 corresponde a los principios básicos reportados de la nueva tecnología: investigación básica, revisión y publicación de artículos científicos y generación del conocimiento necesario para iniciar una tecnología, en un entorno de laboratorio [1]. Sus indicadores de resultados son los artículos científicos sobre los principios de la tecnología, el estado del arte, las bases de datos, los libros y las tesis, y este documento se organiza siguiendo esos indicadores. El equipo no presenta aquí mediciones ni resultados propios; el prototipo aún no ha sido caracterizado.

El alcance también es acotado en lo técnico. El objetivo es estudiar un proceso de clarificación, que es una etapa dentro de un tratamiento más amplio; no se busca producir agua potable, lo que exigiría etapas adicionales como filtración y desinfección. La relación entre las características del agua, la dosis de quitosano y la turbidez final se determinará experimentalmente en etapas posteriores.

**Pregunta de investigación:**
Nuestro planteamiento inicial es el siguiente: ¿qué principios científicos permiten que el quitosano actúe como agente clarificante y que los sensores de pH, turbidez y nivel, junto con la dosificación peristáltica, midan y controlen las variables del proceso, y cómo sustentan su posible utilización en un sistema automatizado de clarificación de agua?

---

## 1 Artículos científicos sobre los principios de la tecnología propuesta

### Quitosano como agente clarificante

El principio que identificamos para la clarificación es la coagulación-floculación. Las partículas coloidales suspendidas en el agua suelen tener carga superficial negativa, lo que las mantiene dispersas y contribuye a la turbidez. El quitosano es un polisacárido derivado de la quitina de crustáceos; sus grupos amino libres adquieren carga positiva en medio ácido y neutralizan la carga de las partículas, mientras que su cadena larga permite unirlas entre sí por puenteo. Los agregados resultantes, llamados flóculos, son más grandes y sedimentan [2].

Sinardi y colaboradores relacionaron el grado de desacetilación de quitosanos obtenidos de distintos crustáceos con el potencial zeta y la remoción de turbidez a pH 5, 7 y 9 en agua sintética. A pH 7, el quitosano de camarón alcanzó 98,38 % de remoción con 190 mg/L, mientras que los otros dos requirieron dosis mayores, y el cambio del potencial zeta indicó que predomina la neutralización de carga [3]. De-Paz-Arroyo y colaboradores evaluaron un quitosano de bajo costo, combinado con policloruro de aluminio, en agua de los ríos Tamazula y Humaya, y señalaron que muchos estudios previos usaron aguas con poca variedad de contaminantes [4]. De estos antecedentes retomamos una idea central: la dosis adecuada no es universal, depende de las propiedades del quitosano, del pH y de la turbidez inicial. Por eso no trasladamos sus cifras como desempeño esperado, y por eso el sistema debe medir pH y turbidez antes de dosificar.

### Medición de pH

El pH expresa la actividad de los iones hidrógeno en una solución:

> pH = −log₁₀(a_H⁺)

La IUPAC establece las definiciones y procedimientos de medición, con soluciones patrón como base de la calibración [5]. El método habitual es potenciométrico: una membrana de vidrio sensible a los iones hidrógeno genera una diferencia de potencial respecto de un electrodo de referencia, y esa diferencia se relaciona con el pH mediante la ecuación de Nernst [5]:

> E = E⁰ − (2,303·R·T / F) · pH

donde E es el potencial medido, E⁰ una constante del sistema de electrodos, R la constante de los gases, T la temperatura absoluta y F la constante de Faraday. A 25 °C la pendiente teórica es de unos 59,16 mV por unidad de pH, aunque el valor real depende del electrodo, de su estado y de la temperatura. Como la fuente del potencial es una membrana de alta impedancia, la lectura requiere una etapa electrónica que no la altere.

Vivaldi y colaboradores confirman que el electrodo de vidrio sigue siendo la referencia de la medición de pH, pero señalan su fragilidad y la necesidad de calibrarlo con frecuencia [6]. Kamel y colaboradores desarrollaron un electrodo potenciométrico compacto de óxido de estaño dopado con cerio sobre vidrio, con respuesta cercana a la nernstiana entre pH 2 y 12 [7], y Doi y colaboradores estudiaron microelectrodos de vidrio, subrayando el papel de las propiedades eléctricas de la interfaz electrodo-solución [8]. Ambos sustentan el principio sin validar la sonda del proyecto. Aquí el pH cumple una doble función: caracteriza el agua antes de dosificar, porque el desempeño del quitosano varía con él [3], y verifica el estado final del agua clarificada.

### Medición de turbidez con el sensor TS-300B

La turbidez es la reducción de la transparencia del agua causada por la luz que dispersan y absorben las partículas suspendidas. Un nefelómetro mide la luz dispersada a 90° respecto del haz incidente, y la norma ISO 7027-1:2016 especifica una fuente infrarroja de 860 nm. La intensidad de luz detectada solo se convierte en unidades de turbidez, como NTU o FNU, mediante una calibración con patrones de turbidez conocida [9].

El TS-300B es un sensor óptico con un emisor de luz y un receptor: cuando el agua es clara llega más luz al detector, y cuando aumentan las partículas la señal cambia. Una aproximación conceptual de la atenuación es la ley de Beer-Lambert:

> I = I₀ · e^(−α·L)

con I la intensidad transmitida, I₀ la incidente, α un coeficiente efectivo de atenuación y L el recorrido óptico. En muestras con partículas hay dispersión múltiple, de modo que la relación real debe establecerse con calibración experimental; una lectura analógica del TS-300B no equivale a un valor en NTU sin una curva propia.

Sperandio y colaboradores construyeron un turbidímetro portátil de bajo costo con un LED infrarrojo, un fototransistor y una plataforma Arduino, calibrado frente a un equipo comercial entre 100 y 1000 NTU [10]. Metzger y colaboradores señalan que las rayaduras y la suciedad en las superficies ópticas generan errores [9]. Zhu, Cherqui y Bertrand-Krajewski revisaron sensores de bajo costo, incluyendo el TS-300B, y destacaron la necesidad de calibración individual y la influencia de la luz ambiental, la suciedad y la reproducibilidad [11]. Sanchez y colaboradores proponen un método ratiométrico que reduce el efecto de las variaciones de la fuente luminosa [12]. En el proyecto, la turbidez es la variable principal: es entrada para decidir la dosis y salida para evaluar el resultado, expresada como porcentaje de remoción, (Tᵢ − T_f) / Tᵢ × 100, donde Tᵢ y T_f son la turbidez inicial y final.

### Detección de nivel sin contacto con el XKC-Y25

Para controlar el llenado del reactor se considera el sensor capacitivo sin contacto XKC-Y25. Presenta una capacitancia estática cuando no hay líquido cerca; al acercarse el líquido se suma una capacitancia parásita y la capacitancia total aumenta. Un circuito integrado interpreta esa variación y, al superar un umbral, indica que el líquido llegó al punto de detección. Se coloca sobre recipientes no metálicos, sin tocar el agua [13]. Este principio implica que detecta presencia o ausencia de líquido en un punto, no el volumen de forma continua, y su función es confirmar que el reactor alcanzó el nivel previsto sin introducir una sonda adicional.

### Dosificación peristáltica

La bomba peristáltica es de desplazamiento positivo: rodillos giratorios comprimen un tubo flexible y empujan el líquido atrapado, de modo que el fluido solo toca el tubo y no el mecanismo. Esto es útil para dosificar una solución de quitosano preparada en medio ácido, siempre que el material del tubo sea compatible. La revisión de Klespitz y Kovács advierte que la exactitud depende del tubo, que la bomba requiere calibración, que el flujo presenta cierta pulsación y que el desgaste obliga a recalibrar [14]. Por ello el volumen dosificado debe verificarse y no asumirse a partir de la velocidad del motor.

El resto de los actuadores (bomba de alimentación, agitador con motor de corriente continua controlado por PWM y posicionamiento con motor paso a paso) cumple funciones de transporte y mezcla, y no constituye un principio de medición. Sus especificaciones se documentarán con sus fichas técnicas en etapas posteriores.

### Síntesis de los principios

| Elemento | Principio | Variable | Limitación principal |
|:---|:---|:---|:---|
| Quitosano | Neutralización de carga y puenteo | Remoción de turbidez | La dosis depende del quitosano, del pH y de la turbidez inicial |
| Sensor de pH | Potenciométrico (Nernst) | Actividad de iones hidrógeno | Calibración, temperatura y fragilidad del electrodo |
| TS-300B | Óptico (dispersión y atenuación) | Turbidez | Calibración, suciedad y luz externa |
| XKC-Y25 | Capacitivo sin contacto | Presencia de líquido en un punto | No mide volumen continuo |
| Bomba peristáltica | Desplazamiento positivo | Volumen dosificado | Calibración, pulsación y desgaste del tubo |

---

## 2 Estado del arte

Los antecedentes indican que el quitosano es un candidato sólido como coagulante-floculante natural, pero que su desempeño depende de varias condiciones. Yang y colaboradores describen la neutralización de carga y el puenteo como mecanismos principales, y señalan que el costo es un obstáculo para su uso práctico [2]. Los estudios de Sinardi y de De-Paz-Arroyo muestran la influencia del grado de desacetilación, del pH y del tipo de agua en la remoción de turbidez [3], [4]. En conjunto orientan una necesidad del proyecto: conocer pH y turbidez antes de decidir cuánto quitosano dosificar.

En cuanto a la medición, es posible construir turbidímetros de bajo costo con emisores y receptores infrarrojos y calibrarlos frente a patrones o equipos comerciales [9], [10], aunque las revisiones advierten que cada sensor necesita calibración individual [11]. Para el pH, el electrodo de vidrio sigue siendo la referencia y su calibración frecuente forma parte de su uso [6]. Para la dosificación, la literatura sobre bombas peristálticas aporta los criterios de calibración necesarios para que el volumen entregado sea confiable [14].

La automatización de la dosificación de coagulantes es un campo activo. Bachri y colaboradores revisaron 25 estudios sobre inteligencia artificial aplicada a la optimización de la coagulación y encontraron que la dosis de coagulante, la turbidez y el pH están entre las variables más usadas; también señalan que la mayoría de los trabajos se limita a escala de laboratorio o piloto [15]. Nuestro sistema comparte esa lógica de dosificar según las condiciones medidas, a escala de laboratorio y con quitosano.

Las patentes aportan antecedentes tecnológicos de los principios de medición. Speck y Wilhelm describen un electrodo de vidrio con una membrana sensible al analito y una capa conductora que lleva la señal al circuito de medición [16]; Tokhtuev y colaboradores patentaron un sensor óptico de turbidez con diodo emisor de luz, fotodiodo y tarjeta electrónica que procesa la luz dispersada [17]. No describen necesariamente los módulos comerciales del proyecto; se incluyen como referencia de mecanismos comparables.

En las fuentes revisadas no identificamos un antecedente que reúna estos elementos para quitosano, lo que no demuestra que no exista y obliga a ampliar la búsqueda. La necesidad pendiente es saber cómo se comportará el conjunto en el reactor: la relación entre la dosis y la turbidez final, la influencia del tiempo y la intensidad de la mezcla, y la estabilidad de las lecturas con flóculos y lodos presentes. En el TRL 1 establecemos la base científica; la caracterización de los sensores y las pruebas de desempeño corresponden a etapas posteriores.

---

## 3 Bases de datos

Para localizar y ampliar el sustento bibliográfico, las plataformas de consulta previstas son Google Académico, ScienceDirect e IEEE Xplore. Google Académico funciona como buscador de literatura académica; ScienceDirect reúne revistas de ingeniería ambiental y tratamiento de agua, como Water Research; e IEEE Xplore concentra literatura técnica sobre instrumentación, control y sistemas embebidos. Para patentes se consultan Google Patents y la base de la USPTO, y para antecedentes de titulación, repositorios universitarios. Las búsquedas pertinentes incluyen “chitosan coagulation turbidity removal”, “chitosan degree of deacetylation zeta potential”, “low-cost turbidity sensor calibration”, “automatic coagulant dosing turbidity pH” y “peristaltic pump dosing calibration”, además de “quitosano coagulante turbidez tesis” para antecedentes en español.

Para esta revisión se identificaron artículos en Water Research, Polymers, Sensors, Chemosensors, Analytical Methods, Scientific Reports, Water Science and Technology, Jurnal Presipitasi y Journal of Robotics and Control, un documento de la IUPAC, un trabajo de acceso abierto en arXiv, patentes y antecedentes de titulación. También se utilizó el manual del fabricante del sensor de nivel, que complementa los artículos pero no se presenta como publicación científica arbitrada. En este apartado, “bases de datos” se refiere a fuentes bibliográficas, no a datos experimentales ya obtenidos. El sistema prevé una base de datos de dosificación para decidir la cantidad de quitosano según pH y turbidez; esa base aún no ha sido generada y no forma parte de esta entrega.

---

## 4 Libros

Como fundamento sobre el quitosano consideramos *Sustainable Agriculture Reviews 36: Chitin and Chitosan*, editado por Crini y Lichtfouse (Springer, 2019). Su capítulo “Chitosan for direct bioflocculation processes” describe el quitosano como polisacárido parcialmente desacetilado y discute sus ventajas y limitaciones como floculante directo en el tratamiento de agua [18]. También consideramos *Wastewater Engineering: Treatment and Resource Recovery*, de Metcalf & Eddy (5.ª ed., McGraw-Hill, 2014), donde se consultan los fundamentos de mezcla, floculación y sedimentación [19]. Su aporte es conceptual y no reemplaza los estudios específicos sobre quitosano.

Como referencia metodológica identificamos *Standard Methods for the Examination of Water and Wastewater*, 24.ª edición, de APHA, AWWA y WEF (2023), que reúne métodos normalizados para analizar parámetros físicos y químicos, entre ellos turbidez y pH [20]. Su relevancia es ofrecer un criterio de comparación para las mediciones del proyecto cuando se llegue a la verificación; no se le atribuye validación alguna de los sensores seleccionados.

Para la instrumentación consideramos *Sensores y acondicionadores de señal*, de Ramón Pallás Areny (Marcombo), que organiza los sensores según la magnitud eléctrica que cambia y aborda los circuitos de acondicionamiento necesarios para leerlas [21], y *Handbook of Modern Sensors*, de Jacob Fraden (5.ª ed., Springer, 2016), organizado por variable medida y con capítulos sobre principios físicos de detección, circuitos de interfaz, sensores de presencia y nivel y sensores químicos [22]. Ambos ayudan a comprender por qué un cambio de potencial, de intensidad luminosa o de capacitancia necesita un circuito de lectura y calibración.

---

## 5 Tesis

Como antecedente de titulación peruano reciente, Coaquira Taco, Huanca Barrantes y Luque Caceres desarrollaron en la Universidad Continental (Arequipa, 2025) el trabajo *Evaluación del quitosano como coagulante natural para tratar la turbidez de las aguas superficiales de la laguna Lagunillas, Lampa – Puno, 2024*. Muestra el interés local por el quitosano en agua superficial; su objeto es evaluar el coagulante, no desarrollar un sistema de dosificación automática [23].

Otro antecedente es *Utilización del quitosano como agente coagulante en el manejo de agua residual de calamar*, tesis de maestría de la Universidad Nacional de Ingeniería (Lima, 2017). Con agua residual sintética y pruebas de jarras, comparó un quitosano de peso molecular medio con uno de peso molecular bajo, optimizando pH, dosis, velocidad y tiempo de agitación; reportó pH óptimos de 5 y 6, dosis óptimas de 25 y 100 mg/L y remociones de turbidez de 96 % y 93 % [24]. Su valor para nosotros está en identificar las variables que el sistema deberá controlar o registrar (pH, dosis, agitación y tiempo de mezcla); sus valores no se asumen como propios.

En el ámbito de la medición, García Gonzales desarrolló en la Universidad Nacional de San Martín (Tarapoto, 2026) un sistema de bajo costo con Arduino Mega 2560 y sensores de pH, turbidez, temperatura, sólidos disueltos y conductividad, que calibra el sensor de pH con soluciones amortiguadoras de 4,00 y 7,00 y contrasta sus resultados con análisis de laboratorio [25]. Aporta un ejemplo del uso de sensores de bajo costo en agua real y de la importancia de la calibración; usa otros módulos y, para la turbidez, ecuaciones preconfiguradas del fabricante, mientras que nuestro proyecto considera necesaria una calibración propia.

Estos trabajos se incorporan como antecedentes de titulación, respetando su denominación institucional. Sus resultados no prueban la eficacia del sistema propuesto ni sustituyen los ensayos con nuestra agua y nuestro quitosano.

Con esta revisión, el TRL 1 del proyecto queda centrado en los principios fisicoquímicos de la coagulación-floculación con quitosano, los principios potenciométrico, óptico y capacitivo de los sensores, y el principio de desplazamiento positivo de la dosificación peristáltica. A futuro, ese conocimiento podrá apoyar el diseño y la caracterización de un sistema automatizado de clarificación, cuyo funcionamiento y alcance deberán comprobarse experimentalmente.

---

## Referencias bibliográficas

[1] Consejo Nacional de Ciencia, Tecnología e Innovación Tecnológica (CONCYTEC), “Directiva N.° 001-2022-CONCYTEC-P: Uso de la metodología Nivel de Madurez Tecnológica (TRL),” Resolución de Presidencia N.° 020-2022-CONCYTEC-P, Lima, Perú, 2022. [En línea]. Disponible: https://vinculate.concytec.gob.pe/download/RP-020-2022-CONCYTEC-P.pdf

[2] R. Yang, H. Li, M. Huang, H. Yang y A. Li, “A review on chitosan-based flocculants and their applications in water treatment,” Water Research, vol. 95, pp. 59–89, 2016, doi: 10.1016/j.watres.2016.02.068.

[3] S. Sinardi et al., “Linking degree of deacetylation to coagulation efficiency of crustacean waste chitosan in water treatment,” Jurnal Presipitasi: Media Komunikasi dan Pengembangan Teknik Lingkungan, vol. 23, no. 2, pp. 533–542, jul. 2026, doi: 10.14710/presipitasi.v23i2.533-542.

[4] G. De-Paz-Arroyo et al., “Synergy between low-cost chitosan and polyaluminum chloride (PAC) improves the flocculation process for river water treatment,” Polymers, vol. 17, no. 13, Art. no. 1822, 2025, doi: 10.3390/polym17131822.

[5] R. P. Buck et al., “Measurement of pH. Definition, standards, and procedures (IUPAC Recommendations 2002),” Pure and Applied Chemistry, vol. 74, no. 11, pp. 2169–2200, 2002, doi: 10.1351/pac200274112169.

[6] F. Vivaldi et al., “Recent advances in optical, electrochemical and field effect pH sensors,” Chemosensors, vol. 9, no. 2, Art. no. 33, 2021, doi: 10.3390/chemosensors9020033.

[7] A. H. Kamel, N. H. A. Al-Sabbagh, I. Moussa, M. Obaida, H. S. M. Abd-Rabboh y W. E. Boraie, “A new compact potentiometric electrode for pH monitoring built upon a glass substrate with a Ce-doped SnO₂ layer,” Analytical Methods, vol. 17, no. 1, pp. 92–103, 2025, doi: 10.1039/D4AY01161A.

[8] K. Doi, N. Asano y S. Kawano, “Development of glass micro-electrodes for local electric field, electrical conductivity, and pH measurements,” Scientific Reports, vol. 10, Art. no. 4110, 2020, doi: 10.1038/s41598-020-60713-z.

[9] M. Metzger et al., “Low-cost GRIN-lens-based nephelometric turbidity sensing in the range of 0.1–1000 NTU,” Sensors, vol. 18, no. 4, Art. no. 1115, 2018, doi: 10.3390/s18041115.

[10] L. C. Sperandio, M. S. Colombo, C. M. G. Andrade y C. B. B. Costa, “Development of a low-cost portable turbidimeter for processes,” arXiv:2106.09491, 2021. [En línea]. Disponible: https://arxiv.org/abs/2106.09491

[11] Q. Zhu, F. Cherqui y J.-L. Bertrand-Krajewski, “End-user perspective of low-cost sensors for urban stormwater monitoring: a review,” Water Science and Technology, vol. 87, no. 11, pp. 2648–2684, 2023, doi: 10.2166/wst.2023.142.

[12] R. Sanchez, M. Groc, R. Vuillemin, M. Pujo-Pay y V. Raimbault, “Development of a frugal, in situ sensor implementing a ratiometric method for continuous monitoring of turbidity in natural waters,” Sensors, vol. 23, no. 4, Art. no. 1897, 2023, doi: 10.3390/s23041897.

[13] XKC, “Non-contact liquid level sensor instruction manual, XKC-Y25-V,” ver. 1.6, hoja de datos. Accedido: 26-oct-2026. [En línea]. Disponible: https://www.electrokit.com/upload/quick/dc/01/adff_41022296-tds.pdf

[14] J. Klespitz y L. Kovács, “Peristaltic pumps – a review on working and control possibilities,” en Proc. IEEE 12th Int. Symp. Applied Machine Intelligence and Informatics (SAMI), Herl’any, Eslovaquia, ene. 2014, pp. 191–194.

[15] A. Bachri, A. K. Mahamad, W. M. Utomo, S. Saon y A. B. Laksono, “Optimization of coagulation process in water treatment plant using artificial intelligence: A systematic literature review and framework,” Journal of Robotics and Control, vol. 6, no. 5, pp. 2471–2487, 2025, doi: 10.18196/jrc.v6i5.27621.

[16] M. Speck y T. Wilhelm, “Glass electrode,” Patente de Estados Unidos US20190187091A1, 20 de junio de 2019.

[17] E. Tokhtuev, C. Owen, A. Skirda, V. Slobodyan y J. Goin, “Turbidity sensor,” Patente de Estados Unidos US20030117623A1, 26 de junio de 2003.

[18] E. Lichtfouse et al., “Chitosan for direct bioflocculation processes,” en Sustainable Agriculture Reviews 36: Chitin and Chitosan: Applications in Food, Agriculture, Pharmacy, Medicine and Wastewater Treatment, G. Crini y E. Lichtfouse, Eds. Cham, Suiza: Springer, 2019, doi: 10.1007/978-3-030-16581-9.

[19] G. Tchobanoglous, H. D. Stensel, R. Tsuchihashi y F. L. Burton, Metcalf & Eddy | AECOM, Wastewater Engineering: Treatment and Resource Recovery, 5.ª ed. Nueva York, NY, EE. UU.: McGraw-Hill Education, 2014.

[20] W. C. Lipps, E. B. Braun-Howland y T. E. Baxter, Eds., Standard Methods for the Examination of Water and Wastewater, 24.ª ed. Washington, DC, EE. UU.: APHA Press, 2023.

[21] R. Pallás Areny, Sensores y acondicionadores de señal. Barcelona, España: Marcombo, 2004.

[22] J. Fraden, Handbook of Modern Sensors: Physics, Designs, and Applications, 5.ª ed. Cham, Suiza: Springer, 2016, doi: 10.1007/978-3-319-19303-8.

[23] G. Coaquira Taco, M. I. Huanca Barrantes y M. M. Luque Caceres, “Evaluación del quitosano como coagulante natural para tratar la turbidez de las aguas superficiales de la laguna Lagunillas, Lampa – Puno, 2024,” Tesis para optar el título profesional, Facultad de Ingeniería, Universidad Continental, Arequipa, Perú, 2025. [En línea]. Disponible: https://repositorio.continental.edu.pe/bitstream/20.500.12394/17662/3/IV_FIN_107_TE_Coaquira_Huanca_Luque_2025.pdf

[24] M. H. Jahuira Arias, “Utilización del quitosano como agente coagulante en el manejo de agua residual de calamar,” Tesis de maestría, Universidad Nacional de Ingeniería, Lima, Perú, 2017. [En línea]. Disponible: http://hdl.handle.net/20.500.14076/13285

[25] A. A. García Gonzales, “Monitoreo de la calidad del agua para zonas rurales mediante un sistema de sensores de bajo costo basados en IoT,” Tesis para optar el título profesional de Ingeniero de Sistemas e Informática, Universidad Nacional de San Martín, Tarapoto, Perú, 2026. [En línea]. Disponible: https://repositorio.unsm.edu.pe/backend/api/core/bitstreams/7b703913-6a81-469a-a5b5-e6e399c3ab88/content

---

*Clarificación de agua con quitosano · TRL 1 · Equipo 05 · Proyecto Integrador 2026-II*
