# Arquitectura del sistema

## Bloque de alimentación

### Entrada USB-C

Responsable de:

- Alimentación externa.
    
- Recarga de batería.
    
- Programación del microcontrolador.
    

### Cargador Li-Ion

TP4056/TP4065.

Funciones:

- Gestión de carga.
    
- Indicador de estado mediante LED.
    

### Convertidor DC/DC

TPS61022.

Funciones:

- Generación de la tensión principal del sistema.
    

## Procesamiento

### ESP32-S3-WROOM-1

Funciones:

- Control principal.
    
- Comunicaciones.
    
- Gestión de sensores.
    
- Almacenamiento de datos.
    

## Sensores

### HDC1080

- Temperatura.
    
- Humedad relativa.
    

### LIS3DH

- Aceleración.
    
- Movimiento.
    

### DRV5013

- Detección magnética.
    

## Almacenamiento

### MicroSD

- Registro de datos.
    
- Almacenamiento de configuraciones.
    

## Interfaz visual

### WS2812

- Estado del sistema.
    
- Diagnóstico.
    
- Indicadores de funcionamiento.
    

## Expansión

Conectores GPIO externos para futuras ampliaciones.