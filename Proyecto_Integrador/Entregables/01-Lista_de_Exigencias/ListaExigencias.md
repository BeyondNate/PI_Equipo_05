# Entregable: Lista de Exigencias

> **Proyecto:** Desarrollo de un sistema automatizado para la clarificación de agua mediante dosificación de quitosano.

Esta **Lista de Exigencias** presenta los requerimientos definidos para el desarrollo del proyecto por los integrantes del equipo.

### Integrantes del equipo

| Iniciales | Integrante      |
| :-------: | --------------- |
| **M.A.** | Marcelo Alarcón |
| **B.C.** | Brad Cárdenas   |
| **G.M.** | Gael Milla      |
| **I.P.** | Idania Parhuay  |
| **L.U.** | Leonel Urbano   |

> **Leyenda:** Las iniciales se utilizarán para identificar a los responsables de cada exigencia dentro del documento.  
> **E:** Exigencia obligatoria. **D:** Deseo o característica deseable.

---

## 1. Lista de exigencias

| Fecha | Tipo | Deseo o exigencia | Descripción | Responsable |
| ------- | :--: | -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| 08/2026 | E | **Función Principal** | El sistema debe permitir realizar el proceso de clarificación de muestras de agua provenientes de fuentes naturales mediante la dosificación controlada de una solución de quitosano. El sistema deberá integrar las etapas de caracterización inicial, determinación de dosis, dosificación, mezcla/agitación, sedimentación y evaluación final de la turbidez. | M.A |
| 08/2026 | E | **Geometría** | El prototipo debe contar con una estructura física compacta y adecuada para su utilización como sistema experimental de laboratorio. La geometría final deberá definirse considerando la ubicación de los sensores, recipiente de tratamiento, sistema de dosificación, mecanismo de agitación, componentes electrónicos y elementos necesarios para su operación y mantenimiento. | L.U |
| 08/2026 | E | **Cinemática** | El sistema de agitación debe permitir controlar el movimiento de la muestra durante la etapa de mezcla. La velocidad de agitación deberá poder configurarse desde el sistema de control y los parámetros finales serán definidos mediante investigación y experimentación. | B.C |
| 08/2026 | E | **Fuerzas** | El mecanismo de agitación deberá proporcionar el movimiento necesario para favorecer la interacción del quitosano con las partículas presentes en la muestra de agua. La selección del motor y mecanismo de agitación deberá realizarse considerando las características del recipiente y del fluido a tratar. | M.A |
| 08/2026 | E | **Energía** | El sistema deberá contar con una fuente de alimentación adecuada para el microcontrolador, sensores, bomba o mecanismo de dosificación, motor de agitación y demás componentes electrónicos utilizados. Se deberán incorporar los elementos necesarios para realizar una alimentación eléctrica segura y estable. | B.C |
| 08/2026 | E | **Materia** | El sistema procesará muestras de agua provenientes de fuentes naturales, principalmente agua superficial o de río. El agente clarificante utilizado será una solución de quitosano. Los materiales que entren en contacto con el agua o la solución de quitosano deberán ser adecuados para el uso experimental y facilitar la limpieza del sistema. | I.P |
| 08/2026 | E | **Señales (Información)** | El sistema deberá obtener como señales de entrada las mediciones de **pH y turbidez** de la muestra antes del tratamiento. También deberá permitir obtener una medición de turbidez después del proceso para evaluar el resultado de la clarificación. Las mediciones deberán estar disponibles para el sistema de control y para su registro. | L.U |
| 08/2026 | E | **Control** | El sistema deberá controlar de forma secuencial las principales etapas del proceso: medición inicial, determinación de dosis, dosificación de quitosano, mezcla/agitación, sedimentación y medición final. La secuencia deberá poder ejecutarse de manera controlada mediante el software del sistema. | I.P |
| 08/2026 | E | **Determinación de dosis** | El sistema deberá determinar una dosis adecuada de quitosano utilizando como variables de entrada las mediciones de pH y turbidez y la información obtenida previamente mediante experimentación. La relación entre las características del agua, la cantidad de quitosano y el resultado de la clarificación deberá establecerse experimentalmente. No se asumirá una fórmula universal que determine directamente la dosis exacta para cualquier muestra. | M.A |
| 08/2026 | E | **Dosificación** | El sistema deberá incorporar una bomba o mecanismo equivalente que permita suministrar de manera controlada la solución de quitosano al recipiente de tratamiento. La cantidad suministrada deberá poder ser configurada por el sistema de control de acuerdo con la dosis determinada. | B.C |
| 08/2026 | E | **Electrónico (hardware)** | El sistema deberá integrar un microcontrolador o dispositivo de control capaz de adquirir las señales de los sensores de pH y turbidez y controlar los actuadores del sistema. El hardware deberá permitir la integración de sensores, sistema de dosificación, motor de agitación y demás componentes necesarios para ejecutar el proceso automatizado. | B.C |
| 08/2026 | E | **Software** | El sistema deberá contar con un programa capaz de leer y procesar las mediciones de los sensores, determinar la dosis correspondiente utilizando los datos experimentales disponibles, controlar la dosificación y agitación, ejecutar la secuencia del proceso y registrar los datos obtenidos durante cada ensayo. | G.M |
| 08/2026 | E | **Monitoreo** | El sistema deberá permitir visualizar las principales variables del proceso, incluyendo las mediciones de pH y turbidez y el estado de las etapas de operación. Cuando sea posible, también deberá mostrar información relacionada con la dosis aplicada, tiempo de proceso y resultado de la medición final. | G.M |
| 08/2026 | D | **Comunicaciones** | Como característica deseable, el sistema podrá incorporar comunicación inalámbrica o cableada para transmitir los datos obtenidos hacia un dispositivo externo o plataforma de almacenamiento. La implementación dependerá de la viabilidad técnica y del tiempo disponible para el desarrollo del prototipo. | I.P |
| 08/2026 | E | **Seguridad** | El sistema deberá considerar medidas de seguridad para evitar el contacto accidental del usuario con componentes eléctricos, partes móviles y sustancias utilizadas durante el proceso. Los componentes eléctricos deberán estar protegidos frente a posibles salpicaduras de agua y el sistema deberá permitir detener el funcionamiento de los actuadores de manera segura. | M.A |
| 08/2026 | E | **Ergonomía** | La disposición de los componentes deberá permitir que el operador pueda colocar la muestra, iniciar el proceso, supervisar las mediciones y retirar la muestra tratada de manera sencilla. La interfaz de software deberá presentar la información del proceso de forma clara y comprensible. | L.U |
| 08/2026 | E | **Fabricación** | Los elementos estructurales y soportes desarrollados para el prototipo deberán poder fabricarse utilizando métodos disponibles para el equipo, incluyendo fabricación convencional o impresión 3D cuando corresponda. Los sensores, actuadores y componentes electrónicos deberán seleccionarse considerando su disponibilidad y facilidad de reemplazo. | B.C |
| 08/2026 | E | **Control de calidad** | El desempeño del sistema deberá evaluarse comparando la turbidez inicial y final de las muestras tratadas. Cada ensayo deberá registrar como mínimo las mediciones iniciales y finales de turbidez, pH inicial, dosis de quitosano utilizada y parámetros relevantes del proceso. La eficiencia de clarificación será determinada a partir de los resultados experimentales obtenidos, sin establecer previamente un porcentaje de eficiencia no validado. | M.A |
| 08/2026 | E | **Registro de datos** | El sistema deberá registrar los datos relevantes de cada ensayo para permitir posteriormente su análisis y comparación. El registro deberá incluir las mediciones realizadas por los sensores, la dosis aplicada y las condiciones utilizadas durante el proceso de clarificación. | G.M |
| 08/2026 | E | **Montaje** | La arquitectura del sistema deberá ser modular, integrando como mínimo un módulo de tratamiento, un módulo de dosificación, un módulo de agitación, un módulo electrónico y un módulo de software. El diseño deberá facilitar el montaje, desmontaje y mantenimiento de los componentes. | G.M |
| 08/2026 | E | **Transporte** | El prototipo deberá poder trasladarse dentro de las instalaciones universitarias cuando se encuentre apagado, desconectado y sin muestras líquidas en su interior. La estructura deberá proteger los componentes principales durante su manipulación. | L.U |
| 08/2026 | D | **Uso** | El sistema estará destinado principalmente a pruebas experimentales en un entorno de laboratorio. Su funcionamiento deberá estar documentado mediante instrucciones que expliquen la preparación de la muestra, colocación de la solución de quitosano, inicio del proceso, supervisión y finalización del ensayo. | I.P |
| 08/2026 | E | **Mantenimiento** | El recipiente de tratamiento, sistema de dosificación, sensores y elementos de agitación deberán poder limpiarse y mantenerse de manera sencilla después de cada ensayo. Las partes que entren en contacto con la muestra deberán permitir su limpieza para evitar acumulación de residuos y contaminación cruzada entre pruebas. | G.M |
| 08/2026 | D | **Costos** | El costo total del prototipo deberá mantenerse dentro del presupuesto disponible para el proyecto. Se deberá elaborar un registro de materiales, sensores, actuadores, componentes electrónicos y demás elementos necesarios, priorizando alternativas de menor costo que no comprometan la funcionalidad del sistema. | I.P |
| 08/2026 | E | **Plazos** | El desarrollo del proyecto deberá realizarse de acuerdo con el cronograma establecido para el curso, distribuyendo las actividades entre investigación, experimentación, diseño, modelado 3D, desarrollo de hardware y software, construcción, integración y validación del prototipo. | I.P |

---

## 2. Plan de Trabajo

### Figura 1. Plan de Trabajo


---

## Bibliografía

> **Nota:** Se utilizarán referencias relacionadas con el uso de quitosano en procesos de coagulación y clarificación, métodos de medición de turbidez, calidad del agua y metodología de diseño de sistemas mecatrónicos. Las referencias podrán actualizarse conforme avance la investigación experimental del proyecto.

1. Abidin CZA, et al. Development of chitosan-based coagulant for water and wastewater treatment. Procedia Eng. 2013;53:221-226.

2. American Water Works Association, American Public Health Association, Water Environment Federation. Standard methods for the examination of water and wastewater. 23rd ed. Washington (DC): American Water Works Association; 2017.

3. International Organization for Standardization. ISO 7027-1:2016. Water quality — Determination of turbidity — Part 1: Quantitative methods. Geneva: International Organization for Standardization; 2016.

4. Ministerio del Ambiente. Estándares de Calidad Ambiental (ECA) para Agua: Decreto Supremo N.° 004-2017-MINAM. Lima: Ministerio del Ambiente; 2017.

5. Organismo Supervisor de la Inversión en Energía y Minería. Código Nacional de Electricidad – Utilización. Lima: Osinergmin; 2011.

6. United States Environmental Protection Agency. Enhanced coagulation and enhanced precipitative softening guidance manual. Washington (DC): Office of Water, United States Environmental Protection Agency; 2011.

7. Verein Deutscher Ingenieure. VDI 2206: Design methodology for mechatronic systems and cyber-physical systems. Düsseldorf: VDI-Gesellschaft; 2022.
