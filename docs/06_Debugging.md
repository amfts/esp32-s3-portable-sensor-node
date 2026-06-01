# Depuración de hardware - Rev A

## Síntoma inicial

La placa no funcionaba correctamente tras el montaje.

Durante las primeras pruebas no era posible validar el funcionamiento normal del ESP32-S3 ni del resto de periféricos.

---

# Medidas iniciales

Con alimentación mediante USB se obtuvieron las siguientes medidas:

|Señal|Valor|
|---|---|
|VBAT|≈ 4.2 V|
|VREG|≈ 4.7 V|
|3V3|≈ 4.3 V – 4.7 V|
|EN|≈ 4.2 V|
|BOOT|≈ 4.3 V|

Estas medidas indicaban claramente que el rail etiquetado como 3V3 estaba fuera de especificación.

---

# Primera hipótesis

Inicialmente se consideraron varias posibilidades:

- Error de soldadura.
    
- Componente defectuoso.
    
- Problema en el divisor de feedback.
    
- Cortocircuito parcial.
    
- Error de diseño en la etapa de alimentación.
    

---

# Revisión del divisor de feedback

Se verificaron los componentes asociados al pin FB del TPS61022.

Valores medidos:

- R2 ≈ 462 kΩ
    
- R5 ≈ 86 kΩ
    

Los valores eran compatibles con una configuración cercana a 3.3 V.

Por tanto, el problema no parecía estar relacionado con el divisor resistivo.

---

# Hallazgo principal

Tras revisar el datasheet del TPS61022 se identificó el problema principal.

El TPS61022 es un convertidor boost.

Su función es elevar tensión.

No puede realizar correctamente conversiones como:

- 5 V USB → 3.3 V
    
- 4.2 V batería Li-Ion → 3.3 V
    

La arquitectura de alimentación de la Rev A asumía implícitamente que el convertidor podría generar un rail estable de 3.3 V independientemente de la tensión de entrada.

Esta hipótesis era incorrecta.

---

# Posible problema secundario en U1

Durante las pruebas se observó que una de las placas presentaba calentamiento anómalo en U1.

Posibles causas:

- Mala soldadura.
    
- Contacto deficiente del pad inferior.
    
- Puentes ocultos.
    
- Daño térmico durante el montaje.
    
- Componente defectuoso.
    

Debido al tamaño extremadamente reducido del encapsulado QFN no fue posible verificar visualmente todas las uniones.

---

# Modificación temporal de alimentación

Con el objetivo de validar el resto del hardware se decidió eliminar temporalmente el TPS61022.

Posteriormente se instaló un regulador lineal LM3940-3.3 mediante cableado manual.

## Conexiones

### Entrada

LM3940 IN → VREG

### Salida

LM3940 OUT → Rail +3V

### Masa

LM3940 GND → GND

---

# Resultado de la modificación

Tras instalar el LM3940 se obtuvieron las siguientes medidas:

|Señal|Valor|
|---|---|
|3V3|3.23 V|
|EN|3.23 V|
|BOOT|3.25 V|

Estas medidas eran compatibles con un funcionamiento correcto del ESP32-S3.

---

# Validación del ESP32-S3

Tras estabilizar la alimentación se realizaron nuevas pruebas.

Resultados obtenidos:

- Detección USB correcta.
    
- Enumeración USB correcta.
    
- Dispositivo Espressif detectado por el sistema operativo.
    
- Programación de firmware correcta.
    
- Verificación de Flash correcta.
    
- Comunicación serie funcional.
    
- Ejecución de firmware validada.
    

Durante la primera carga fue necesario pulsar manualmente RESET para iniciar correctamente el firmware.

Posteriormente el sistema funcionó con normalidad.

---

# Validación de Flash

Se verificó:

- Escritura de firmware.
    
- Lectura de firmware.
    
- Verificación de integridad.
    
- Arranque correcto.
    

Esto permitió concluir que la memoria Flash sobrevivió a la sobretensión observada durante las primeras pruebas.

---

# Validación de LEDs RGB

Se validó la cadena de LEDs RGB WS2812 integrada en la placa.

Configuración utilizada:

- GPIO3 (ESPLED)
    

Resultados:

- Encendido correcto.
    
- Control individual de LEDs.
    
- Comunicación validada.
    
- Firmware funcional.
    

Esto confirmó el correcto funcionamiento de:

- GPIO3.
    
- Cadena WS2812.
    
- Alimentación de LEDs.
    
- Ejecución de firmware.
    

---

# Investigación del sensor Hall

Se realizaron pruebas sobre el DRV5033.

Observaciones:

- Alimentación correcta.
    
- Continuidad correcta entre salida y GPIO46.
    
- Variación de tensión observada en la salida del sensor durante algunas pruebas con imán.
    

Sin embargo:

- El ESP32 no detectó correctamente los cambios de estado.
    
- El comportamiento observado fue inconsistente.
    

Posteriormente se identificó que GPIO46 es un pin de strapping del ESP32-S3.

Estado actual:

- Sensor parcialmente validado.
    
- Pendiente migración a un GPIO de propósito general para validación definitiva.
    

---

# Investigación del HDC1080

Se realizaron medidas sobre el sensor.

Valores observados:

|Pin|Función|Valor|
|---|---|---|
|1|SDA|3.24 V|
|2|GND|0 V|
|5|VDD|3.24 V|
|6|SCL|3.24 V|

Observaciones:

- Alimentación correcta.
    
- Bus I2C en reposo correcto.
    
- No detectado por escáner I2C.
    

Durante la revisión documental se identificó además un error de diseño:

- El pad inferior DAP/EP fue conectado a GND.
    
- Texas Instruments indica que debe permanecer flotante.
    

Estado actual:

- Pendiente determinar si el fallo está relacionado con:
    
    - DAP conectado a GND.
        
    - Soldadura.
        
    - Orientación.
        
    - Daño por sobretensión.
        

---

# Investigación del LIS3DH

Observaciones:

- Alimentación correcta.
    
- Bus I2C correcto.
    
- No detectado por escáner I2C.
    

Durante la revisión del esquemático se detectó que:

- El pin CS fue dejado sin conexión.
    

Según el datasheet:

- CS = 1 → I2C habilitado.
    
- CS = 0 → SPI habilitado.
    

Se realizó una modificación temporal conectando CS a 3.3 V.

Resultado:

- El sensor continuó sin responder.
    

Estado actual:

Posibles causas:

- Soldadura.
    
- Orientación incorrecta.
    
- Footprint incorrecto.
    
- Daño por sobretensión.
    

---

# Conclusiones

La investigación permitió identificar con alto grado de confianza que el fallo principal de la Rev A estaba localizado en la arquitectura de alimentación.

La sustitución temporal del TPS61022 por un LM3940 permitió recuperar gran parte de la funcionalidad de la placa.

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
    
- Hall sensor.
    
- MicroSD.
    

La Rev A pasó de considerarse una placa completamente fallida a una plataforma parcialmente funcional que permitió validar una parte importante del diseño original.