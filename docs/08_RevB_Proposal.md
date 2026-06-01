# Propuesta de mejora - Rev B

## Objetivo

Desarrollar una segunda revisión de la placa que corrija los problemas identificados durante la validación de la Rev A y mejore significativamente la facilidad de montaje, depuración y mantenimiento.

La Rev B no pretende únicamente corregir errores, sino aplicar todas las lecciones aprendidas durante el desarrollo de la primera versión.

---

# Objetivos principales

## Corregir la arquitectura de alimentación

La principal modificación será el rediseño completo del sistema de alimentación.

### Problema detectado en Rev A

La Rev A utilizaba un TPS61022 como regulador principal del rail 3V3.

Durante la validación se comprobó que:

- USB proporciona aproximadamente 5V.
    
- Una batería Li-Ion completamente cargada proporciona aproximadamente 4.2V.
    

El TPS61022 es un convertidor boost y no puede generar correctamente una tensión regulada de 3.3V cuando la tensión de entrada es superior a la de salida.

Como consecuencia se observaron:

- VREG ≈ 4.7V
    
- Rail 3V3 ≈ 4.3V – 4.7V
    

---

## Objetivo Rev B

Garantizar una tensión estable de 3.3V en todas las condiciones de funcionamiento.

---

## Alternativas a estudiar

### Opción 1: Buck-Boost

Ventajas:

- 3.3V estables en todo el rango de descarga de la batería.
    
- Solución técnicamente más robusta.
    
- Compatible con USB y batería sin restricciones.
    

Desventajas:

- Mayor complejidad.
    
- Mayor coste.
    
- Mayor dificultad de depuración.
    

---

### Opción 2: Regulador LDO

Ventajas:

- Diseño extremadamente simple.
    
- Menor ruido.
    
- Menor coste.
    
- Fácil depuración.
    
- Menor número de componentes.
    

Desventajas:

- Menor eficiencia.
    
- Pérdida de regulación cuando la batería descienda por debajo de aproximadamente 3.5V.
    

---

### Opción 3: Buck dedicado

Ventajas:

- Buena eficiencia.
    
- Solución ampliamente utilizada.
    
- Adecuado para alimentación desde USB.
    

Desventajas:

- Requiere estudiar el comportamiento cuando la alimentación provenga exclusivamente de batería.
    

---

# Mejoras en sensores

## HDC1080

### Problema detectado

Durante la revisión se identificó que el DAP (Exposed Pad) fue conectado a GND.

Según Texas Instruments:

- El DAP debe soldarse.
    
- El pad de la PCB debe permanecer flotante.
    
- No debe conectarse a masa.
    

---

### Acción Rev B

Modificar el footprint para:

- Mantener el DAP soldado.
    
- Eliminar cualquier conexión eléctrica a GND.
    
- Seguir estrictamente las recomendaciones del fabricante.
    

---

## LIS3DH

### Problema detectado

El pin CS fue dejado inicialmente sin conexión.

Según el datasheet:

- CS = 1 → Modo I2C.
    
- CS = 0 → Modo SPI.
    

Aunque posteriormente se realizó una modificación temporal conectándolo a 3.3V, esta condición debe quedar resuelta desde el diseño.

---

### Acción Rev B

Conectar permanentemente:

```text
CS → 3.3V
```

para garantizar funcionamiento I2C.

---

### Dirección I2C

Definir explícitamente:

```text
SDO/SA0 → GND
```

o

```text
SDO/SA0 → 3.3V
```

para evitar estados indeterminados.

---

## Sensor Hall

### Problema detectado

El DRV5033 fue conectado a GPIO46.

Posteriormente se comprobó que GPIO46 es un pin de strapping del ESP32-S3.

---

### Acción Rev B

Migrar el sensor Hall a un GPIO de propósito general.

Posibles candidatos:

- GPIO13
    
- GPIO14
    
- GPIO15
    
- GPIO17
    
- GPIO18
    
- GPIO21
    

Evitar:

- GPIO0
    
- GPIO3
    
- GPIO45
    
- GPIO46
    

---

# Mejoras de fabricabilidad

## Eliminación de encapsulados 0201

Componentes afectados:

- C19
    
- C25
    

---

### Nueva política

Para todos los futuros proyectos:

#### Tamaño preferido

- 0603
    

#### Tamaño aceptable

- 0402
    

#### Tamaño a evitar

- 0201
    

---

## Reducción de componentes 0402

Siempre que el espacio lo permita:

- Migrar 0402 → 0603
    

Objetivos:

- Facilitar montaje.
    
- Facilitar inspección.
    
- Facilitar retrabajos.
    

---

# Encapsulados QFN

## TPS61022

Durante la Rev A se observó que el encapsulado QFN de 2 mm × 2 mm complica considerablemente:

- Inspección visual.
    
- Retrabajo.
    
- Diagnóstico.
    

---

### Acción Rev B

Siempre que exista una alternativa equivalente:

- Evaluar encapsulados más accesibles.
    
- Priorizar facilidad de montaje frente a miniaturización extrema.
    

---

# Mejoras para depuración

## Test Points

La Rev A puso de manifiesto la necesidad de disponer de puntos de medida accesibles.

Añadir test points para:

### Alimentación

- VBUS
    
- VBAT
    
- VREG
    
- 3V3
    

### ESP32

- EN
    
- BOOT
    
- RESET
    

### Regulador

- FB
    

### I2C

- SDA_HDC
    
- SCL_HDC
    
- SDA_LIS
    
- SCL_LIS
    

### Hall

- HALL_OUT
    

---

## Serigrafía

Añadir identificación visible para:

- Alimentaciones.
    
- GPIO.
    
- Test points.
    
- Señales críticas.
    

---

# Estrategia de validación

La Rev B deberá validarse por bloques.

## Fase 1

Alimentación:

- USB.
    
- Batería.
    
- Carga.
    
- Regulación.
    

---

## Fase 2

ESP32:

- USB.
    
- Flash.
    
- Firmware.
    
- Comunicación serie.
    

---

## Fase 3

Sensores:

- HDC1080.
    
- LIS3DH.
    
- DRV5033.
    

---

## Fase 4

MicroSD.

---

## Fase 5

WS2812.

---

## Fase 6

Integración completa.

---

# Regla personal adquirida

Antes de enviar una PCB a fabricación:

1. Revisar la arquitectura de alimentación.
    
2. Revisar el esquemático completo.
    
3. Revisar footprints críticos.
    
4. Revisar disponibilidad de componentes.
    
5. Revisar soldabilidad manual.
    

Una PCB ligeramente más grande pero fácil de montar suele ser preferible a una PCB muy compacta pero difícil de depurar.

---

# Objetivo final

Desarrollar una plataforma ESP32-S3 completamente funcional capaz de:

- Adquirir datos de sensores.
    
- Registrar información en MicroSD.
    
- Funcionar mediante batería Li-Ion.
    
- Programarse mediante USB-C.
    
- Servir como base para futuros desarrollos IoT.
    

La Rev B deberá mantener las funcionalidades de la Rev A eliminando los errores detectados durante la validación y facilitando significativamente el proceso de fabricación y depuración.