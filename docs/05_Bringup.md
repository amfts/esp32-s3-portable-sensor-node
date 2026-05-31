# Bring-up de hardware

## Objetivo

Verificar el correcto funcionamiento de los distintos bloques funcionales de la PCB.

## Secuencia de validación prevista

### Alimentación USB

Verificaciones:

- Presencia de 5V en VBUS.
    
- Correcto funcionamiento del cargador.
    

### Alimentación principal

Verificaciones:

- Tensión de salida del convertidor DC/DC.
    
- Ausencia de cortocircuitos.
    

### Microcontrolador

Verificaciones:

- Alimentación correcta.
    
- Estado de EN y BOOT.
    
- Detección por USB.
    

### Sensores

Verificaciones:

- Comunicación I2C.
    
- Detección de dispositivos.
    

### MicroSD

Verificaciones:

- Inicialización.
    
- Lectura y escritura.
    

## Instrumentación utilizada

- Multímetro digital.
    
- Tester USB.
    
- Inspección visual.
    
- Revisión de datasheets.
