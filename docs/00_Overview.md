# ESP32-S3 Portable Sensor Node

**Estado:** Rev A validada parcialmente | Rev B pendiente

---

# Descripción general

ESP32-S3 Portable Sensor Node es un proyecto personal de diseño electrónico desarrollado con el objetivo de adquirir experiencia práctica en todas las fases de un desarrollo hardware completo.

El proyecto consiste en una placa electrónica basada en ESP32-S3 que integra alimentación mediante batería Li-Ion, carga por USB-C, almacenamiento en tarjeta MicroSD y diversos sensores ambientales y de movimiento en una única PCB compacta.

Más allá del resultado funcional, el principal objetivo era recorrer el ciclo completo de desarrollo de producto:

- Definición de requisitos.
    
- Diseño esquemático.
    
- Diseño PCB.
    
- Fabricación.
    
- Montaje SMD.
    
- Bring-up.
    
- Depuración.
    
- Análisis de fallos.
    
- Documentación técnica.
    

---

# Galería

## PCB Rev A

![PCB Rev A - Vista superior](Docs/RevA_PCB_Top.jpg)

Vista superior de la PCB fabricada.

![PCB Rev A - Vista inferior](Docs/RevA_PCB_Bottom.jpg)

Vista inferior de la PCB fabricada.

---

## Montaje

![Montaje parcial](Docs/RevA_PCB_Top_Half_Populated.jpg)

Primera fase de ensamblaje de componentes SMD.

![Montaje avanzado](Docs/RevA_PCB_Top_Populated.jpg)

Placa ensamblada con ESP32-S3 y circuitería principal.

---

## Modificación de depuración

![LM3940 Debug Fix|148]()

Modificación temporal realizada durante la fase de depuración para sustituir el TPS61022 por un regulador LM3940-3.3.

Esta modificación permitió validar el resto del sistema.

---

# Objetivos del proyecto

## Objetivos técnicos

Diseñar una plataforma autónoma capaz de:

- Adquirir datos de sensores.
    
- Almacenar información localmente.
    
- Funcionar mediante batería recargable.
    
- Ofrecer conectividad inalámbrica mediante ESP32-S3.
    
- Servir como base para futuros desarrollos IoT.
    

## Objetivos de aprendizaje

Adquirir experiencia práctica en:

- Diseño electrónico.
    
- Integración de sensores.
    
- Gestión de alimentación.
    
- Diseño PCB multicapa.
    
- Montaje de componentes SMD.
    
- Interpretación de datasheets.
    
- Depuración de hardware.
    
- Análisis de problemas reales de diseño.
    

---

# Características principales

## Procesamiento

### ESP32-S3-WROOM-1-N16R8

Funciones previstas:

- Control principal del sistema.
    
- Gestión de sensores.
    
- Registro de datos.
    
- Comunicaciones inalámbricas.
    
- Actualizaciones de firmware.
    

---

## Alimentación

- USB-C para alimentación y carga.
    
- Batería Li-Ion de una celda.
    
- Cargador TP4056/TP4065.
    
- Convertidor TPS61022 (Rev A).
    

---

## Sensores

### HDC1080

Medición de:

- Temperatura.
    
- Humedad relativa.
    

Estado actual:

- Alimentación validada.
    
- Comunicación I2C pendiente de validación.
    

### LIS3DH

Medición de:

- Aceleración.
    
- Movimiento.
    
- Orientación.
    

Estado actual:

- Alimentación validada.
    
- Comunicación I2C pendiente de validación.
    

### DRV5033

Detección de:

- Campos magnéticos.
    
- Presencia de imanes.
    

Estado actual:

- Salida analizada eléctricamente.
    
- Validación firmware pendiente.
    

---

## Almacenamiento

### MicroSD

Funciones previstas:

- Registro de medidas.
    
- Almacenamiento de configuraciones.
    
- Almacenamiento de eventos.
    

Estado actual:

- Pendiente de validación.
    

---

## Interfaz visual

### LEDs RGB WS2812

Funciones:

- Indicación de estado.
    
- Diagnóstico visual.
    
- Feedback de funcionamiento.
    

Estado actual:

- Funcionamiento validado.
    

---

# Herramientas utilizadas

## Diseño

- EasyEDA
    

## Fabricación

- JLCPCB
    

## Montaje

- Soldadura SMD manual.
    
- Flux.
    
- Aire caliente.
    

## Firmware

- Arduino IDE
    
- ESP-IDF Framework (ESP32)
    

## Validación

- Multímetro digital.
    
- Tester USB.
    
- Datasheets de fabricantes.
    

---

# Resultados obtenidos

## Fabricación

La PCB fue fabricada correctamente y recibida sin incidencias.

## Montaje

La placa fue ensamblada manualmente utilizando componentes SMD.

Durante el montaje se identificaron dificultades importantes asociadas al uso de encapsulados:

- 0201
    
- 0402
    
- QFN 2x2 mm
    

---

## Bring-up inicial

Durante las primeras pruebas se observaron tensiones anómalas:

|Señal|Valor|
|---|---|
|VBAT|4.2 V|
|VREG|4.7 V|
|3V3|4.3 – 4.7 V|

Estas medidas indicaban un problema grave en la arquitectura de alimentación.

---

## Investigación

La depuración permitió identificar que el TPS61022 era un convertidor boost y no podía generar correctamente un rail de 3.3 V a partir de:

- USB (5 V)
    
- Batería Li-Ion cargada (4.2 V)
    

Como resultado, el rail etiquetado como 3V3 alcanzaba valores peligrosamente elevados.

---

## Modificación temporal

Para verificar el estado real de la placa se retiró el TPS61022 y se instaló temporalmente un regulador LM3940-3.3.

Resultado:

|Señal|Valor|
|---|---|
|3V3|3.23 V|
|EN|3.23 V|
|BOOT|3.25 V|

---

## Validación obtenida

Tras la modificación temporal se verificó:

### ESP32-S3

- Enumeración USB correcta.
    
- Programación de firmware correcta.
    
- Comunicación serie funcional.
    
- Ejecución de firmware validada.
    

### Flash

- Escritura correcta.
    
- Verificación correcta.
    
- Arranque correcto.
    

### LEDs RGB

- Encendido correcto.
    
- Comunicación validada.
    
- Control desde firmware validado.
    

### Hall Sensor

- Se observó respuesta eléctrica en la salida del sensor.
    
- Validación completa pendiente.
    

### Sensores I2C

HDC1080 y LIS3DH:

- Alimentación correcta.
    
- No detectados por escáner I2C.
    
- Pendientes de investigación adicional.
    

---

# Hallazgos principales

## Error de arquitectura de alimentación

El principal hallazgo del proyecto fue la selección incorrecta del TPS61022 como generador principal del rail de 3.3 V.

Este error provocó:

- Sobretensión en la alimentación.
    
- Imposibilidad de validar correctamente el sistema.
    
- Riesgo potencial para los componentes conectados.
    

---

## Problemas secundarios detectados

### HDC1080

Se detectó que el pad inferior (DAP/EP) fue conectado a GND.

Texas Instruments indica que este pad debe permanecer flotante.

### LIS3DH

El pin CS quedó inicialmente sin conexión.

Para funcionamiento I2C debería conectarse permanentemente a 3.3 V.

### Hall Sensor

Se utilizó GPIO46 como entrada del sensor Hall.

Posteriormente se identificó que GPIO46 es un pin de strapping del ESP32-S3 y no es una elección recomendable para periféricos.

---

# Estado actual

## Rev A

Estado:

- Diseñada.
    
- Fabricada.
    
- Montada.
    
- Depurada.
    
- Parcialmente validada.
    
- Documentada.
    

Resultado:

La placa es funcional tras corregir temporalmente la alimentación.

Bloques validados:

- ESP32-S3.
    
- USB.
    
- Flash.
    
- Programación de firmware.
    
- Comunicación serie.
    
- LEDs RGB.
    

Bloques pendientes:

- HDC1080.
    
- LIS3DH.
    
- MicroSD.
    
- Hall sensor.
    

---

## Rev B

Estado:

- Pendiente de rediseño.
    

Mejoras previstas:

- Nueva arquitectura de alimentación.
    
- Corrección de errores de esquemático.
    
- Mejora de fabricabilidad.
    
- Eliminación de encapsulados 0201.
    
- Añadir test points.
    
- Simplificación de la depuración.
    

---

# Tecnologías utilizadas

- ESP32-S3
    
- USB-C
    
- Li-Ion
    
- TP4056 / TP4065
    
- TPS61022
    
- LM3940-3.3
    
- HDC1080
    
- LIS3DH
    
- DRV5033
    
- WS2812
    
- MicroSD
    
- EasyEDA
    
- JLCPCB
    

---

# Lección principal

El aprendizaje más importante obtenido durante este proyecto fue comprender que la alimentación es el bloque más crítico de cualquier diseño electrónico.

Una arquitectura de alimentación incorrecta puede impedir el funcionamiento de todo el sistema independientemente de que el resto del diseño sea correcto.

Sin embargo, una depuración sistemática permitió recuperar parcialmente la placa y validar gran parte del diseño original, convirtiendo la Rev A en una experiencia de aprendizaje extremadamente valiosa para futuras revisiones.