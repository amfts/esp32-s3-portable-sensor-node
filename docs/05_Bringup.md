# Bring-up de hardware - Rev A

## Objetivo

Verificar el correcto funcionamiento de los distintos bloques funcionales de la PCB tras el ensamblaje.

El objetivo principal era validar progresivamente:

1. Alimentación.
    
2. ESP32-S3.
    
3. Sensores.
    
4. MicroSD.
    
5. LEDs RGB.
    
6. Sistema completo.
    

---

# Instrumentación utilizada

Durante las pruebas se emplearon las siguientes herramientas:

- Multímetro digital.
    
- Tester USB.
    
- Arduino IDE.
    
- Cable USB-C.
    
- Datasheets de fabricantes.
    
- Inspección visual.
    
- Firmware de prueba.
    

---

# Secuencia de validación prevista

## Alimentación USB

Verificaciones previstas:

- Presencia de 5V en VBUS.
    
- Funcionamiento correcto del cargador Li-Ion.
    
- Presencia de tensión en batería.
    

---

## Alimentación principal

Verificaciones previstas:

- Funcionamiento del TPS61022.
    
- Presencia de un rail estable de 3.3V.
    
- Ausencia de cortocircuitos.
    

---

## ESP32-S3

Verificaciones previstas:

- Alimentación correcta.
    
- Estado correcto de EN.
    
- Estado correcto de BOOT.
    
- Detección USB.
    

---

## Sensores

Verificaciones previstas:

### HDC1080

- Detección I2C.
    
- Lectura de temperatura.
    
- Lectura de humedad.
    

### LIS3DH

- Detección I2C.
    
- Lectura de aceleración.
    

### DRV5033

- Detección de campo magnético.
    
- Cambio de estado mediante imán.
    

---

## MicroSD

Verificaciones previstas:

- Inicialización.
    
- Lectura.
    
- Escritura.
    

---

## LEDs RGB

Verificaciones previstas:

- Encendido.
    
- Control individual.
    
- Comunicación con ESP32.
    

---

# Resultados iniciales

Durante las primeras pruebas se observaron tensiones anómalas.

## Medidas registradas

|Señal|Valor|
|---|---|
|VBAT|4.2 V|
|VREG|4.7 V|
|3V3|4.3 V – 4.7 V|

Estas medidas indicaban que el rail principal de alimentación estaba fuera de especificación.

---

# Investigación

La revisión posterior permitió identificar que el TPS61022 utilizado en Rev A era un convertidor boost.

El componente no era adecuado para generar un rail estable de 3.3 V cuando la tensión de entrada provenía de:

- USB (5 V)
    
- Batería Li-Ion cargada (4.2 V)
    

Se decidió detener temporalmente el proceso de validación y comenzar una fase específica de depuración.

---

# Modificación temporal

Para continuar la validación del hardware se retiró el TPS61022.

Posteriormente se instaló un regulador LM3940-3.3 mediante cableado manual.

Conexiones realizadas:

- IN → VREG
    
- OUT → Rail +3V
    
- GND → GND
    

---

# Medidas tras la modificación

|Señal|Valor|
|---|---|
|3V3|3.23 V|
|EN|3.23 V|
|BOOT|3.25 V|

La alimentación pasó a ser compatible con el funcionamiento normal del ESP32-S3.

---

# Validación del ESP32-S3

Resultados obtenidos:

- Detección USB correcta.
    
- Enumeración USB correcta.
    
- Dispositivo Espressif reconocido por el sistema operativo.
    
- Programación de firmware correcta.
    
- Comunicación serie funcional.
    
- Ejecución de firmware validada.
    

Durante la primera carga fue necesario pulsar manualmente RESET para iniciar correctamente la aplicación.

---

# Validación de Flash

Se verificó:

- Escritura de firmware.
    
- Verificación de firmware.
    
- Arranque correcto.
    

La memoria Flash resultó completamente funcional.

---

# Validación de LEDs RGB

Se validó el funcionamiento de la cadena WS2812 integrada.

Resultados:

- Encendido correcto.
    
- Control individual correcto.
    
- Comunicación correcta con el ESP32-S3.
    

Estado:

✅ Validado

---

# Validación del sensor Hall

Observaciones:

- Alimentación correcta.
    
- Continuidad correcta entre sensor y ESP32.
    
- Variación de tensión observada en la salida del sensor al aproximar un imán.
    

Sin embargo:

- El comportamiento observado mediante firmware fue inconsistente.
    

Estado:

🟡 Parcialmente validado

---

# Validación del HDC1080

Observaciones:

- Alimentación correcta.
    
- SDA = 3.24 V.
    
- SCL = 3.24 V.
    
- No detectado por escáner I2C.
    

Estado:

❌ Pendiente de validación

---

# Validación del LIS3DH

Observaciones:

- Alimentación correcta.
    
- Bus I2C correcto.
    
- No detectado por escáner I2C.
    

Durante la depuración se detectó que el pin CS había sido dejado inicialmente flotante.

Tras forzarlo a 3.3 V el dispositivo continuó sin responder.

Estado:

❌ Pendiente de validación

---

# Validación de MicroSD

No se realizaron pruebas funcionales durante la Rev A.

Estado:

⏳ Pendiente

---

# Resultado final del Bring-up

La Rev A no pudo validarse completamente debido a un error de diseño en la arquitectura de alimentación.

Sin embargo, la modificación temporal mediante LM3940 permitió recuperar gran parte de la funcionalidad de la placa.

## Bloques validados

- ESP32-S3.
    
- USB.
    
- Flash.
    
- Programación de firmware.
    
- Comunicación serie.
    
- LEDs RGB.
    

## Bloques parcialmente validados

- DRV5033.
    

## Bloques pendientes

- HDC1080.
    
- LIS3DH.
    
- MicroSD.
    

El bring-up permitió localizar el problema principal de la revisión y proporcionó información suficiente para definir una futura Rev B más robusta y fácil de depurar.