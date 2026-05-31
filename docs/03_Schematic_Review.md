# Revisión del esquemático - Rev A

## Microcontrolador

El diseño utiliza un ESP32-S3-WROOM-1 como unidad principal de procesamiento.

Funciones previstas:

- Control del sistema.
- Comunicación USB.
- Lectura de sensores.
- Gestión de MicroSD.
- Control de LEDs RGB.
- Expansión mediante GPIO.

## Alimentación

El sistema se diseñó con:

- Entrada USB-C.
- Cargador Li-Ion.
- Batería de una celda.
- Convertidor TPS61022.
- Rail principal etiquetado como 3V3.

## Problema detectado

El TPS61022 es un convertidor boost, por lo que no puede reducir tensión.

Esto supone un problema porque las fuentes de entrada pueden estar por encima de 3.3V:

- USB: aproximadamente 5V.
- Batería Li-Ion cargada: aproximadamente 4.2V.

Por tanto, el rail etiquetado como 3V3 puede quedar por encima de 3.3V.

## Sensores

Sensores integrados:

- HDC1080: temperatura y humedad.
- LIS3DH: acelerómetro.
- DRV5013: sensor Hall.

## Almacenamiento

Se incluye tarjeta MicroSD para registro local de datos.

## Conclusión

El esquemático integra correctamente varios bloques funcionales, pero la arquitectura de alimentación de la Rev A no es adecuada para generar un rail estable de 3.3V desde USB o batería Li-Ion.
