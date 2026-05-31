# Propuesta de mejora - Rev B

## Objetivo

Desarrollar una segunda revisión de la placa que corrija los problemas detectados durante la validación de la Rev A y mejore la fabricabilidad del diseño.

---

# Objetivos principales

## Corregir la arquitectura de alimentación

La principal modificación será el rediseño completo del sistema de alimentación.

Problema detectado en Rev A:

- Uso de TPS61022 como regulador principal del rail 3V3.
    
- Imposibilidad de garantizar una tensión estable de 3.3V cuando la entrada proviene de USB o batería Li-Ion.
    

Objetivo Rev B:

- Generar una tensión estable de 3.3V en cualquier condición de funcionamiento.
    

Alternativas a estudiar:

### Opción 1: Buck-Boost

Ventajas:

- 3.3V estables.
    
- Compatible con batería Li-Ion en todo su rango de descarga.
    
- Solución técnicamente más robusta.
    

### Opción 2: LDO

Ventajas:

- Menor complejidad.
    
- Menor ruido.
    
- Menor coste.
    
- Más sencillo de depurar.
    

Inconvenientes:

- Menor eficiencia.
    
- Limitaciones cuando la batería descienda por debajo del margen de regulación.
    

---

# Mejoras de fabricabilidad

## Eliminación de encapsulados 0201

Componentes afectados:

- C19
    
- C25
    

Nueva política:

- 0603 por defecto.
    
- 0402 únicamente cuando exista una justificación clara.
    
- Evitar 0201 en diseños de montaje manual.
    

## Revisión de encapsulados QFN

Evaluar alternativas más sencillas de ensamblar cuando existan opciones equivalentes.

---

# Mejoras para depuración

## Test Points

Añadir puntos de medida dedicados para:

- VBUS
    
- VBAT
    
- VREG
    
- 3V3
    
- EN
    
- BOOT
    
- FB
    
- SDA
    
- SCL
    

## Validación por bloques

La Rev B deberá validarse progresivamente:

1. Alimentación.
    
2. ESP32.
    
3. Sensores.
    
4. MicroSD.
    
5. LEDs.
    
6. Integración completa.
    

---

# Mejoras mecánicas

## Accesibilidad

Facilitar el acceso a:

- Conectores.
    
- Señales de depuración.
    
- Puntos de medida.
    

## Serigrafía

Añadir más referencias visibles para:

- Alimentación.
    
- GPIO.
    
- Test points.
    
- Señales críticas.
    

---

# Objetivo final

Conseguir una plataforma ESP32-S3 completamente funcional, alimentada mediante batería Li-Ion y USB-C, apta para aplicaciones IoT, adquisición de datos y desarrollo de firmware.

---

# Estado

Pendiente de rediseño y validación.
