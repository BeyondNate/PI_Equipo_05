# CLARIFICACIÓN DE AGUA CON QUITOSANO

### Formulación conceptual de la propuesta tecnológica · TRL 2

Proyecto Integrador 2026-II | Universidad Peruana Cayetano Heredia | Equipo 05

27 de octubre de 2026

---

## Ficha del proyecto

| Campo | Información |
|:---|:---|
| **Curso** | Proyecto Integrador 2026-II |
| **Institución** | Universidad Peruana Cayetano Heredia |
| **Tema** | TRL 2 · Sistema automatizado de clarificación de agua mediante dosificación de solución de quitosano en ácido acético al 1 % |
| **Agua de referencia** | Río Lurín (Pachacámac) |
| **Fecha** | 27/10/2026 |

**Integrantes:**

- Marcelo Alarcón Camones
- Brad Cárdenas Parián
- Leonel Urbano Castillo
- Idania Parhuay Meza
- Gael Milla Fasabi

**Ingenieros:** Umbert Lewis De La Cruz, Maria Rejas, Harry Rivera y Renzo Chan

---

## Presentación

En el TRL 1 reunimos los principios científicos que sustentan el sistema: la coagulación-floculación con quitosano, la medición de pH, turbidez y nivel, y la dosificación peristáltica. En este TRL 2 damos el paso siguiente: formulamos el concepto tecnológico, es decir, cómo se combinarían esos principios en una aplicación práctica concreta.

Según la metodología de CONCYTEC, el TRL 2 corresponde a la formulación conceptual de la propuesta tecnológica. Es un nivel de investigación aplicada en el que se exploran aplicaciones prácticas que aún son especulativas, sustentadas en un estado del arte acotado al concepto de aplicación tecnológica; la propiedad intelectual empieza a ganar importancia y el trabajo se desarrolla en laboratorio [1]. Sus indicadores de resultados son (1) artículos científicos que respalden o tengan relación directa con la propuesta, (2) un análisis de patentabilidad y protección, y (3) un perfil o propuesta de proyecto. Este documento se organiza siguiendo esos tres indicadores, precedidos por la formulación del concepto.

El alcance se mantiene acotado: se estudia una etapa de clarificación, no la producción de agua potable. El equipo aún no presenta mediciones ni resultados propios del prototipo; las cifras de la literatura se usan como antecedentes y no como desempeño esperado.

**Pregunta guía:** ¿cómo usar tecnología y experimentación para adaptar la clarificación del agua del río Lurín (Pachacámac) a las características de cada muestra? [2]

---

## 0 Formulación del concepto tecnológico

### Concepto

El sistema recibe una muestra de agua turbia en un reactor de 1 L. Un ESP32 lee pH, turbidez y nivel; consulta una relación experimental pH–turbidez–dosis; activa una bomba peristáltica que entrega la solución de quitosano; agita; deja sedimentar; mide de nuevo la turbidez y registra el resultado en pantalla LCD, con aviso sonoro y datos por Wi-Fi. La solución de trabajo es quitosano disuelto en ácido acético al 1 %, una preparación habitual en la literatura de coagulación [3], [4], [5]. El concepto corresponde a la **Solución Azul (Concepto B)**, que obtuvo el mayor puntaje ponderado (4,35 de 5) en la matriz de decisión del equipo [2].

### Secuencia de operación

1. **Llenado:** la bomba de alimentación transporta el agua al reactor y el sensor XKC-Y25 confirma el nivel.
2. **Medición inicial:** se registran pH y turbidez.
3. **Cálculo de dosis:** el ESP32 consulta la tabla experimental (no se asume una fórmula universal).
4. **Dosificación y agitación:** la bomba peristáltica entrega el volumen definido; el agitador, posicionado por el conjunto NEMA 17 con husillo T8, mezcla.
5. **Sedimentación y medición final:** se mide la turbidez final y se calcula la remoción, (Tᵢ − T_f) / Tᵢ × 100.
6. **Reporte:** LCD, buzzer y Wi-Fi.

### Arquitectura de hardware (según el esquemático v1.2, actualizado el 29/10/2026)

| Bloque | Componentes en el esquemático | Función en el concepto |
|:---|:---|:---|
| Control | ESP32 DevKit V1 | Lectura de sensores, cálculo de dosis, secuencia y comunicación |
| Energía | Entrada de 12 V por bornera; convertidor LM2596; 5 V y 3,3 V | Alimentar bombas, motor, sensores y control |
| Sensado | Módulo pH PH4502C y módulo de turbidez (el sensor de nivel XKC-Y25 ya no aparece en la v1.2) | pH y turbidez |
| Acondicionamiento | Divisores 15 kΩ / 20 kΩ en las señales de 5 V | Reducir 5 V a unos 2,86 V, dentro del rango de 3,3 V del ADC |
| Bombas | Dos canales con MOSFET IRLZ44N, diodo de protección y resistencias de 100 Ω y 10 kΩ | Transporte de agua y dosificación de quitosano |
| Posicionamiento | Motor NEMA 17 con driver TMC2209 (STEP, DIR, EN) | Subir y bajar el conjunto de agitación |
| Interfaz | LCD 16×2 I2C y buzzer | Estado, valores y avisos |

**Asignación de pines en la v1.2:** pH en GPIO34 y turbidez en GPIO35, ambos con divisor 15 kΩ / 20 kΩ; LCD I2C en GPIO21 y GPIO22; buzzer en GPIO23; MOSFET de la bomba 1 en GPIO18 y de la bomba 2 en GPIO19; driver TMC2209 con STEP, DIR y EN en GPIO25, GPIO26 y GPIO27. Ningún pin se repite entre funciones.

### Modelo 3D

El ensamblaje en Onshape (archivo asemblyfinal.stl, unos 149 000 triángulos) tiene un volumen envolvente de aproximadamente 62,0 × 40,0 × 47,9 unidades del archivo, más grande que la versión anterior (51,8 × 40,0 × 34,0); conviene confirmar la unidad (probablemente centímetros) frente a la base de 50 cm de los planos. Incluye base modular, reactor de 1 L con tapa, soporte del agitador, depósitos y cajas, y es la referencia para fabricar con impresión 3D (PLA) y componentes comerciales.

### Hipótesis de trabajo (por comprobar en TRL 3)

- **H1.** Con pH y turbidez inicial medidos se puede asignar una dosis a partir de una tabla experimental propia, con una remoción de turbidez comparable a la de una prueba de jarras manual.
- **H2.** La medición de turbidez final permite verificar cada ensayo y alimentar la tabla de dosis con datos de cada corrida.
- **H3.** El volumen entregado por la bomba peristáltica puede verificarse y calibrarse con un método de referencia (por ejemplo, pesada), sin asumirlo a partir de la velocidad del motor.

### Dos desafíos de diseño detectados al formular el concepto

**Resolución de la dosis.** Una solución al 1 % (p/v) contiene 10 g/L, es decir, 10 000 mg/L de quitosano. En un reactor de 1 L, 1 mg/L equivale a 0,1 mL de solución, y las dosis reportadas en la literatura van desde 0,5 mg/L hasta 190 mg/L [3], [5], [6], [7]. Eso exige entregar entre 0,05 mL y 19 mL con exactitud. Probablemente convenga trabajar con una dilución intermedia y calibrar la bomba en ese rango.

**Sobredosis.** Hesami y colaboradores observaron reestabilización de la turbidez cuando se superó la concentración óptima de quitosano [5]. El control debe limitar la dosis máxima y la tabla experimental debe incluir ese comportamiento.

---

## 1 Artículos científicos que respaldan la propuesta

A diferencia del TRL 1, que reunió principios, aquí se buscan trabajos con relación directa con la aplicación: quitosano en ácido acético sobre agua de río, dosificación automática de coagulantes con sensores y microcontrolador.

### Quitosano en ácido acético aplicado a aguas superficiales

Hesami, Bina y Ebrahimi estudiaron el quitosano como coagulante auxiliar en agua del río Zayandeh Rood, entre pH 4 y 9 y con turbidez inicial entre 20 y 200 mg/L. Prepararon el quitosano en ácido acético al 1 %, hallaron una concentración óptima de 0,5 mg/L junto con cloruro férrico y observaron reestabilización a concentraciones mayores [5]. Aporta a nuestro concepto la preparación de la solución y el riesgo de sobredosis.

Irawati, Hikmah y Junaidi compararon quitosano disuelto en ácido acético y en ácido clorhídrico para reducir la turbidez de agua eutrófica. Ambos tuvieron su óptimo a 10 mg/L; el disuelto en ácido acético alcanzó 64,78 % de reducción a pH 7, frente a 85,33 % del disuelto en clorhídrico a pH 8, y la concentración del ácido influyó en el resultado [6]. Aporta una advertencia: el solvente cambia el desempeño, de modo que nuestras pruebas deben fijar y registrar la preparación.

Iqbal y colaboradores disolvieron 1 g de quitosano de concha en 100 mL de ácido acético al 1 % y lo probaron con agua de río de 49,5 NTU; reportaron un óptimo de 1 ppm, 99 % de remoción y pH óptimo de 6,32 [7]. Un trabajo de la Universidad Nacional de Costa Rica con agua del río Pirro (15,5 NTU) halló 76,6 % de remoción con 1,0 mg/L a pH 7,09, y señaló que el pH y la turbidez inicial modifican la remoción en agua sintética [8]. Otro estudio, con agua del Río Grande de Añasco (Puerto Rico) a 403, 1220 y 5038 NTU, evaluó quitosano en soluciones ácidas y reportó remociones de carbono orgánico total de 39,8 % a 74,3 % [9]. En el contexto de Sinardi [3] y De-Paz-Arroyo [4], ya considerados en el TRL 1, estos resultados confirman que el desempeño depende del pH, de la turbidez, del solvente y del tipo de agua. Es la razón de ser de medir antes de dosificar. Ninguna de estas cifras se adopta como desempeño esperado del prototipo.

### Dosificación automática con sensores y microcontrolador

Amri y colaboradores (2026) construyeron un sistema de dosificación de policloruro de aluminio con sensor de turbidez, bomba y NodeMCU-ESP32, con envío de datos a la nube, y lo probaron con agua de río, de arrozal y subterránea de 29, 35 y 64 NTU [11]. Es el antecedente más cercano en arquitectura (ESP32, turbidez, bomba, IoT), aunque usa otro coagulante. Viana y colaboradores desarrollaron un turbidímetro de bajo costo basado en procesamiento de imágenes para que un controlador lógico programable dosifique coagulante y lo evaluaron en una planta piloto [10]. Bachri y colaboradores, ya citados en el TRL 1, revisaron 25 estudios y encontraron que la dosis, la turbidez y el pH están entre las variables más usadas, casi siempre a escala de laboratorio o piloto [13]. En el Perú, Guerrero Escobedo usó un sensor económico con Arduino calibrado contra un turbidímetro Hach para estudiar mezclas de coagulantes con agua del canal madre de Chavimochic [12].

Sobre la mecánica de dosificación, Klespitz y Kovács mantienen vigente la advertencia sobre calibración y desgaste del tubo [14], y Zhu y colaboradores insisten en la calibración individual de los sensores de bajo costo [15].

### Relación entre el concepto y la evidencia

| Elemento del concepto | Respaldo | Lo que aún no está respaldado |
|:---|:---|:---|
| Quitosano en ácido acético al 1 % | [5], [6], [7], [9] | Desempeño con agua del río Lurín |
| Dosis dependiente de pH y turbidez | [3], [4], [8] | La tabla propia del equipo |
| Límite de dosis por reestabilización | [5] | El valor límite para nuestro quitosano |
| Dosificación automática con ESP32 y turbidez | [10], [11], [13] | Su uso específico con quitosano |
| Calibración de sensores y bomba | [14], [15] | Curvas propias del TS-300B y de la bomba |

---

## 2 Análisis de patentabilidad y protección

Este es un análisis preliminar de estudiantes. No reemplaza una búsqueda profesional de anterioridades ni la opinión de un abogado o agente de propiedad industrial.

### Marco legal en el Perú

En el Perú las invenciones se protegen ante INDECOPI bajo dos figuras (Decisión 486 de la Comunidad Andina). La patente de invención protege productos o procedimientos durante 20 años y exige novedad, nivel inventivo y aplicación industrial. El modelo de utilidad protege productos, es decir, mejoras de forma o disposición con ventaja técnica sobre algo existente, durante 10 años, y exige un nivel inventivo menor. Ambas vigencias se cuentan desde la presentación [16]. La novedad se pierde si la invención fue divulgada antes de presentar la solicitud, con un período de gracia de un año desde la divulgación [16]. Los diseños industriales se registran hasta por 10 años [17].

### Antecedentes tecnológicos encontrados

| Documento | Qué describe | Relación con el proyecto |
|:---|:---|:---|
| US 4,855,061 [18] | Control de la dosis de coagulante con un sensor de carga y un turbidímetro, mediante un controlador programable | Control automático de dosis con turbidez |
| US 5,160,439 [19] | Control de coagulante con varios parámetros (carga, turbidez, pH, caudal), con relación desarrollada para cada sitio | Dosis a partir de pH y turbidez con relación empírica propia del sitio |
| US 6,241,896 [20] | Determinación automática de la dosis de coagulante con color y turbidez, con microprocesador y bomba | Cálculo automático de dosis con microcontrolador |
| US 11,505,478 B2 [21] | Dosificación automática de coagulante y ajuste de pH según potencial zeta, con tabla de consulta y aprendizaje automático | Tabla de consulta pH–dosis; patente aparentemente vigente |
| US 5,543,056 [22] | Tratamiento de agua potable con polímeros catiónicos naturales, entre ellos quitosano en ácido acético | Uso del quitosano preparado en medio ácido |
| Controlador CoagSense (Process Instruments) [23] | Producto comercial que combina pH, turbidez y otros sensores para controlar una bomba de coagulante | Producto equivalente en agua potable a gran escala |
| Amri et al., 2026 [11] | Dosificación con ESP32 y turbidez para PAC | Arquitectura similar en la literatura |

Las patentes de los años ochenta y noventa probablemente ya expiraron, pero su estado legal debe verificarse. Además, una patente estadounidense solo produce derechos en Estados Unidos; para el Perú importa si existe una solicitud o patente equivalente.

### Evaluación preliminar

**Elementos conocidos.** Cada pieza es conocida: sensores de pH, turbidez y nivel, bomba peristáltica, ESP32, quitosano y dosificación automática según pH y turbidez. La idea general de dosificar un coagulante según parámetros medidos ya está en el estado de la técnica [18]–[21], [11].

**Posibles diferenciadores, aún por demostrar.**

- Combinación de quitosano en ácido acético, tabla experimental pH–turbidez–dosis y medición de turbidez final en un ciclo por lotes de 1 L.
- Disposición mecánica del reactor: tapa con sensores y agitador cuyo conjunto sube y baja con NEMA 17 y husillo T8, en una estructura modular de impresión 3D.
- La base de datos de dosificación para el agua del río Lurín, que será un resultado experimental propio.

**Conclusión provisional.** Una patente de invención parece difícil de sostener en esta etapa, porque la combinación de elementos podría considerarse evidente frente a los antecedentes. El modelo de utilidad es más plausible para la disposición mecánica del reactor, siempre que una búsqueda posterior confirme su novedad y se demuestre una ventaja técnica. Con los datos actuales no podemos afirmar que el concepto sea patentable.

### Otras formas de protección

| Figura | Qué protegería | Comentario |
|:---|:---|:---|
| Derecho de autor | Código del ESP32, esquemático y planos | Surge con la creación; el registro en INDECOPI facilita probar la autoría |
| Secreto empresarial | Tabla de dosificación | Solo si se mantiene confidencial; publicar los datos elimina esta vía |
| Diseño industrial | Apariencia de la carcasa | Aplicable solo si el diseño se vuelve distintivo |
| Licencia abierta | El repositorio del equipo | Alternativa si se prefiere difundir; debe elegirse con conciencia de que impide patentar lo ya divulgado |

### Alertas y acciones recomendadas

1. **Divulgación previa.** La presentación de avance cita el repositorio de GitHub del equipo y el enlace público de Onshape. Si son públicos, ya hubo divulgación y el período de gracia de un año (si aplica) habría empezado. Conviene verificar su visibilidad y decidir antes de publicar más.
2. **Política institucional.** Consultar la política de propiedad intelectual de la UPCH y los derechos de los docentes y estudiantes sobre los resultados del proyecto.
3. **Búsqueda formal.** Repetir la búsqueda en Google Patents, Espacenet y la base de INDECOPI con clasificaciones relacionadas con tratamiento de agua por coagulación y floculación, control de parámetros del tratamiento y medición de turbidez; incluir las palabras «chitosan», «dosing», «turbidity» y «jar».
4. **Registro de laboratorio.** Llevar un cuaderno fechado con diseños, versiones y ensayos, que sirva de prueba de creación.
5. **Licencias de terceros.** Registrar las licencias de las bibliotecas del ESP32 y de los módulos usados.

---

## 3 Perfil o propuesta de proyecto

### Título

Sistema automatizado para la clarificación de agua del río Lurín (Pachacámac) mediante dosificación de quitosano.

### Problema y justificación

La calidad del río Lurín varía según el tramo y la época del año, y su tramo bajo supera los estándares de calidad ambiental microbiológicos [2]. Una dosis fija de coagulante podría no ser adecuada para todas las muestras, y los valores de turbidez disponibles corresponden a puntos y momentos concretos. Por eso el proyecto mide cada muestra y adapta la dosis. Se alinea con el ODS 6 (meta 6.3) como eje principal, el ODS 9 (investigación e innovación), el ODS 12 (uso eficiente de un biopolímero) y, de forma indirecta, el ODS 13 [2].

### Objetivos

**General.** Formular y demostrar a escala de laboratorio un prototipo que clarifique muestras de agua del río Lurín dosificando quitosano según el pH y la turbidez medidos.

**Específicos.**

1. Calibrar los sensores de pH, turbidez y nivel y verificar la dosificación peristáltica.
2. Construir, con jarras y con el prototipo, una tabla experimental pH–turbidez–dosis para muestras del río Lurín.
3. Integrar y programar la secuencia automática de llenado, medición, dosificación, agitación, sedimentación y evaluación.
4. Registrar y comparar turbidez inicial y final y pH inicial en cada ensayo.

### Alcance y exclusiones

Incluye: ensayos de laboratorio con muestras del río Lurín; un reactor de 1 L; mediciones de pH y turbidez; registro de datos. No incluye: producir agua potable (faltarían filtración y desinfección), cumplir normas de agua de consumo, ni operación continua en campo.

### Requerimientos principales

La lista de exigencias del equipo reúne 25 requisitos (22 obligatorios y 3 deseables). Los más críticos para este nivel son: medir pH y turbidez antes del tratamiento y la turbidez al final; calcular la dosis con datos experimentales; dosificar la solución con una bomba; ejecutar la secuencia en orden; mostrar y registrar los datos; y garantizar seguridad, limpieza y transporte con el equipo apagado y vacío [2].

### Plan para pasar a TRL 3 (prueba de concepto experimental)

| Fase | Actividad | Entregable |
|:---|:---|:---|
| 1 | Preparar la solución de quitosano en ácido acético al 1 % y fijar el procedimiento | Protocolo de preparación |
| 2 | Calibrar pH (soluciones patrón), turbidez (patrones y turbidímetro de referencia) y bomba (pesada) | Curvas de calibración |
| 3 | Pruebas de jarras con muestras del río Lurín de distintos puntos | Tabla preliminar pH–turbidez–dosis |
| 4 | Integrar hardware y firmware; ensayo de la secuencia sin agua y con agua | Prototipo funcional |
| 5 | Ensayos de clarificación con el prototipo y comparación con las jarras | Informe con remoción de turbidez |

### Indicadores de éxito propuestos

- Remoción de turbidez (Tᵢ − T_f) / Tᵢ × 100, comparada con la prueba de jarras.
- Error de volumen dosificado frente a la referencia por pesada.
- Repetibilidad de las lecturas de pH y turbidez en réplicas.
- Secuencia completa sin intervención y con registro de datos.

Los umbrales numéricos de aceptación deben fijarse con el equipo y los ingenieros antes de iniciar los ensayos.

### Riesgos

| Riesgo | Efecto | Mitigación |
|:---|:---|:---|
| Lecturas de turbidez inestables por luz, burbujas o flóculos | Dosis incorrecta | Calibración propia, promedios y cubierta del sensor |
| Dosis muy pequeñas con la bomba peristáltica | Error de dosificación | Dilución intermedia y calibración en ese rango |
| Sobredosis y reestabilización | Menor remoción | Límite máximo de dosis |
| Ácido acético sobre sondas, tubos y piezas de PLA | Desgaste o contaminación | Verificar compatibilidad de materiales y enjuagar con agua destilada |
| Variabilidad de las muestras del río | Tabla poco representativa | Muestrear en varios puntos y épocas |
| Plazos del curso | Retrasos | Priorizar jarras y calibración antes de la integración |

### Observaciones de coherencia detectadas en la documentación

- El esquemático v1.2 muestra dos canales de bomba y el driver del NEMA 17, pero no muestra el motor de corriente continua con PWM que la presentación asigna a la agitación. Conviene añadirlo o aclarar cuál es el actuador de mezcla.
- El XKC-Y25 sigue en la presentación y en el TRL 1, pero ya no aparece en el esquemático v1.2 (la versión anterior lo conectaba con un divisor 15 kΩ / 20 kΩ). Hay que reincorporarlo o ajustar cómo se controla el nivel de llenado.
- Usar un solo nombre para la fuente de agua: «río Lurín (Pachacámac)», pues Pachacámac es el distrito. El TRL 1 la nombra como río Pachacamac.
- Completar el presupuesto con los precios reales de los componentes: el documento no los estima para no inventarlos.

### Equipo y responsabilidades

Líder y control de calidad: M. Alarcón. Diseño, electrónica y fabricación: B. Cárdenas. Investigación y geometría: L. Urbano. Documentación, comunicaciones y gestión: I. Parhuay. Programación y software: G. Milla [2].

---

## Conclusión

El TRL 2 del proyecto queda formulado como un concepto de laboratorio: un reactor de 1 L, controlado por un ESP32, que mide pH y turbidez, dosifica una solución de quitosano en ácido acético al 1 % con una bomba peristáltica, agita y evalúa la turbidez final. La literatura respalda cada pieza y muestra sistemas de dosificación automática comparables, pero no una combinación idéntica con quitosano. El análisis preliminar indica que la protección más realista por ahora es el derecho de autor y, tal vez, un modelo de utilidad para la disposición mecánica, y alerta sobre la divulgación previa. Lo que falta, y es el objetivo del TRL 3, es demostrar experimentalmente que el prototipo funciona con agua del río Lurín.

---

## Referencias bibliográficas

[1] Consejo Nacional de Ciencia, Tecnología e Innovación Tecnológica (CONCYTEC), “Directiva N.° 001-2022-CONCYTEC-P: Uso de la metodología Nivel de Madurez Tecnológica (TRL),” Resolución de Presidencia N.° 020-2022-CONCYTEC-P, Lima, Perú, 2022. [En línea]. Disponible: https://vinculate.concytec.gob.pe/download/RP-020-2022-CONCYTEC-P.pdf

[2] Equipo 05, “Presentación parcial: Desarrollo de un sistema automatizado para la clarificación de agua mediante dosificación de quitosano,” Proyecto Integrador 2026-II, Universidad Peruana Cayetano Heredia, Lima, Perú, 2026.

[3] S. Sinardi et al., “Linking degree of deacetylation to coagulation efficiency of crustacean waste chitosan in water treatment,” Jurnal Presipitasi, vol. 23, no. 2, pp. 533–542, jul. 2026, doi: 10.14710/presipitasi.v23i2.533-542.

[4] G. De-Paz-Arroyo et al., “Synergy between low-cost chitosan and polyaluminum chloride (PAC) improves the flocculation process for river water treatment,” Polymers, vol. 17, no. 13, Art. no. 1822, 2025, doi: 10.3390/polym17131822.

[5] F. Hesami, B. Bina y A. Ebrahimi, “The effectiveness of chitosan as coagulant aid in turbidity removal from water,” International Journal of Environmental Health Engineering, vol. 3, pp. 46–51, abr. 2014, doi: 10.4103/2277-9183.131814.

[6] U. Irawati, F. A. Hikmah y A. B. Junaidi, “The effect of acetic acid and hydrochloric acid as a solvent for chitosan to reduce turbidity in eutrophic water,” International Journal of Environmental Science and Development, vol. 14, no. 5, pp. 287–291, 2023, doi: 10.18178/ijesd.2023.14.5.1446.

[7] Iqbal et al., artículo sobre quitosano de concha de molusco como coagulante para agua cruda, Konversi, vol. 13, no. 2, pp. 82–86, 2024, doi: 10.20527/k.v13i2.19882. [En línea]. Disponible: https://ppjp.ulm.ac.id/journal/index.php/konversi/article/download/19882/10689

[8] Universidad Nacional de Costa Rica, trabajo sobre coagulación-floculación con quitosanos de distinto peso molecular y agua del río Pirro, repositorio institucional. [En línea]. Disponible: https://repositorio.una.ac.cr/items/00976ed7-ba0f-4554-953d-1c159f436107

[9] “Effectiveness of acidic chitosan solutions for total organic carbon removal in drinking water treatment,” PubMed Central, PMC12251864. [En línea]. Disponible: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12251864/

[10] D. C. Viana, R. A. Braga, B. H. G. Barbosa, A. R. C. R. Campos y D. D. Ferreira, “Turbidímetro baseado em processamento de imagens: elaboração e avaliação de desempenho por meio de uma planta piloto,” Congresso Brasileiro de Automática, Sociedade Brasileira de Automática. [En línea]. Disponible: https://sba.org.br/open_journal_systems/index.php/cba/article/view/3394

[11] F. Amri, B. Dharmawan Hadi, T. Haryanti, R. Lutfiar Rizky, S. Rahayu y A. Wanda Alifah, “Automatic polyaluminium chloride (PAC) dosing control system for water purification using NodeMCU-ESP32,” LOGIC: Jurnal Rancang Bangun dan Teknologi, vol. 26, no. 1, pp. 10–16, mar. 2026, doi: 10.31940/logic.v26i1.10-16.

[12] A. E. Guerrero Escobedo, “Remoción de turbidez de aguas del canal madre de Chavimochic empleando diseño de mezclas de coagulantes,” Dialnet. [En línea]. Disponible: https://dialnet.unirioja.es/descarga/articulo/8092598.pdf

[13] A. Bachri, A. K. Mahamad, W. M. Utomo, S. Saon y A. B. Laksono, “Optimization of coagulation process in water treatment plant using artificial intelligence: A systematic literature review and framework,” Journal of Robotics and Control, vol. 6, no. 5, pp. 2471–2487, 2025, doi: 10.18196/jrc.v6i5.27621.

[14] J. Klespitz y L. Kovács, “Peristaltic pumps – a review on working and control possibilities,” en Proc. IEEE 12th SAMI, Herl’any, Eslovaquia, ene. 2014, pp. 191–194.

[15] Q. Zhu, F. Cherqui y J.-L. Bertrand-Krajewski, “End-user perspective of low-cost sensors for urban stormwater monitoring: a review,” Water Science and Technology, vol. 87, no. 11, pp. 2648–2684, 2023, doi: 10.2166/wst.2023.142.

[16] INDECOPI, “Guía de patentes para investigadores,” Lima, Perú. [En línea]. Disponible: https://investigacion.pucp.edu.pe/wp-content/uploads/2024/06/GUIA_DE_PATENTES_PARA_INVESTIGADORES.pdf

[17] Institut National de la Propriété Industrielle (INPI), “Fiche PI Pérou,” 2025. [En línea]. Disponible: https://www.inpi.fr/sites/default/files/2025-09/Fiche%20PI%20Pe%CC%81rou%202025.pdf

[18] “Method and apparatus for controlling the coagulant dosage for water treatment,” Patente de Estados Unidos US 4,855,061. [En línea]. Disponible: https://patents.google.com/patent/US4855061

[19] “System for controlling coagulant treatment based on monitoring of plural parameters,” Patente de Estados Unidos US 5,160,439.

[20] “Auto dosage determination method and apparatus for coagulant control in the treatment of water,” Patente de Estados Unidos US 6,241,896.

[21] “Automated methods and systems for optimized zeta potential chemical dosing in water treatment systems,” Marmac Water LLC, Patente de Estados Unidos US 11,505,478 B2. [En línea]. Disponible: https://patents.google.com/patent/US11505478

[22] “Method of drinking water treatment with natural cationic polymers,” Patente de Estados Unidos US 5,543,056.

[23] Process Instruments, “CoagSense automatic coagulation controller,” hoja técnica ISB163. [En línea]. Disponible: https://www.processinstruments.net/wp-content/uploads/pdf/ISB163-Automatic-Coagulation-Controller-CoagSense.pdf

---

*Clarificación de agua con quitosano · TRL 2 · Equipo 05 · Proyecto Integrador 2026-II*
