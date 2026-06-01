# Análisis de fallos - Rev A

## Resumen ejecutivo

La Rev A permitió identificar varios problemas de diseño y validación que impidieron el funcionamiento completo de la placa durante las primeras pruebas.

El hallazgo más importante fue un error de arquitectura en el sistema de alimentación, acompañado de varios problemas secundarios relacionados con la integración de sensores y decisiones de diseño que dificultaron la depuración.

A pesar de ello, la sustitución temporal del TPS61022 por un regulador LM3940 permitió recuperar gran parte de la funcionalidad de la placa y validar múltiples bloques hardware.

---

# Fallo principal: arquitectura de alimentación incorrecta

## Descripción

La Rev A utiliza un TPS61022 como regulador principal para generar el rail de alimentación denominado 3V3.

El TPS61022 es un convertidor boost.

Su función principal es elevar tensión.

---

## Problema detectado

Las fuentes de alimentación del sistema pueden presentar tensiones superiores a 3.3V:

### Alimentación USB

- USB-C ≈ 5V
    

### Alimentación mediante batería

- Batería Li-Ion cargada ≈ 4.2V
    

En estas condiciones el TPS61022 no puede generar correctamente un rail regulado de 3.3V.

---

## Evidencias observadas

Durante las pruebas se registraron valores como:

|Señal|Valor|
|---|---|
|VBAT|≈ 4.2 V|
|VREG|≈ 4.7 V|
|3V3|≈ 4.3 V – 4.7 V|

Estos valores son incompatibles con una alimentación nominal de 3.3V.

---

## Consecuencias

Posible exposición a sobretensión de:

- ESP32-S3
    
- HDC1080
    
- LIS3DH
    
- DRV5033
    
- Tarjeta MicroSD
    
- LEDs WS2812
    

No puede descartarse daño permanente en algunos componentes.

---

## Validación de la hipótesis

Para confirmar la causa raíz se eliminó el TPS61022 y se instaló temporalmente un regulador LM3940-3.3.

Tras la modificación se obtuvieron:

|Señal|Valor|
|---|---|
|3V3|3.23 V|
|EN|3.23 V|
|BOOT|3.25 V|

Y se verificó:

- Enumeración USB correcta.
    
- Programación de firmware correcta.
    
- Comunicación serie funcional.
    
- Ejecución de firmware correcta.
    
- Flash funcional.
    
- LEDs RGB funcionales.
    

---

## Conclusión

La arquitectura de alimentación constituye la causa raíz principal identificada durante la Rev A.

---

# Fallo secundario: posible problema de montaje en U1

## Descripción

El TPS61022 utiliza encapsulado QFN de 2 mm × 2 mm.

Se trata de un encapsulado especialmente complejo para soldadura manual.

---

## Observaciones

Durante las pruebas se detectó:

- Calentamiento anómalo de U1 en una de las placas.
    
- Comportamiento inconsistente.
    
- Medidas difíciles de interpretar en el nodo FB.
    

---

## Posibles causas

- Mala soldadura.
    
- Contacto deficiente del pad inferior.
    
- Puentes ocultos.
    
- Daño térmico durante el montaje.
    
- Componente defectuoso.
    

---

## Impacto

Aunque el error de alimentación explica el comportamiento observado, no puede descartarse la existencia de problemas adicionales de ensamblaje en U1.

---

# Problemas detectados en el HDC1080

## Síntomas

Durante la validación:

- Alimentación correcta.
    
- SDA = 3.24 V.
    
- SCL = 3.24 V.
    
- No detectado por escáner I2C.
    

---

## Hallazgo de diseño

Durante la revisión documental se identificó un error en el footprint utilizado.

El pad inferior DAP/EP fue conectado a GND.

Sin embargo, Texas Instruments indica explícitamente:

> El DAP debe soldarse a un pad flotante y no conectarse eléctricamente a GND.

---

## Estado actual

No se ha podido determinar si la ausencia de comunicación I2C está causada por:

- El DAP conectado a GND.
    
- Problemas de soldadura.
    
- Orientación incorrecta.
    
- Daño por sobretensión.
    

---

## Impacto

El sensor no pudo ser validado durante la Rev A.

---

# Problemas detectados en el LIS3DH

## Síntomas

Durante las pruebas:

- Alimentación correcta.
    
- SDA y SCL correctos.
    
- No detectado por escáner I2C.
    

---

## Hallazgo de diseño

Durante la revisión se detectó que:

- El pin CS fue dejado inicialmente sin conexión.
    

Según el datasheet:

- CS = 1 → Modo I2C.
    
- CS = 0 → Modo SPI.
    

Posteriormente se realizó una modificación temporal conectando CS a 3.3 V.

---

## Resultado

El sensor continuó sin responder.

---

## Posibles causas

- Problemas de soldadura.
    
- Orientación incorrecta.
    
- Footprint incorrecto.
    
- Daño por sobretensión.
    

---

## Estado actual

No validado.

---

# Problemas detectados en el sensor Hall

## Síntomas

El DRV5033 mostró comportamiento parcialmente funcional.

Observaciones:

- Alimentación correcta.
    
- Continuidad correcta entre salida y ESP32.
    
- Variación de tensión observada en la salida durante pruebas con imán.
    

Sin embargo:

- El firmware no detectó correctamente el cambio de estado.
    

---

## Hallazgo de diseño

El sensor fue conectado a GPIO46.

Posteriormente se identificó que GPIO46 es un pin de strapping del ESP32-S3.

Este pin afecta al proceso de arranque y no es recomendable para periféricos generales.

---

## Estado actual

Parcialmente validado.

---

# Fallo de fabricabilidad

## Descripción

Aunque no fue la causa principal del fallo de funcionamiento, la selección de encapsulados excesivamente pequeños complicó significativamente el ensamblaje y la depuración de la placa.

---

## Componentes 0201

Casos identificados:

- C19
    
- C25
    

Estos condensadores resultaron extremadamente difíciles de soldar manualmente.

---

## Componentes 0402

También se emplearon diversos componentes en encapsulado 0402.

Aunque son más manejables que los 0201, siguen aumentando significativamente la dificultad de montaje respecto a encapsulados 0603.

---

## Impacto

- Incremento del tiempo de montaje.
    
- Mayor riesgo de soldaduras defectuosas.
    
- Mayor dificultad para inspección visual.
    
- Mayor dificultad para retrabajos.
    
- Mayor dificultad para realizar mediciones.
    
- Mayor probabilidad de errores de ensamblaje.
    

---

## Lección aprendida

El tamaño mínimo técnicamente posible no siempre es la mejor elección.

En proyectos destinados a montaje manual resulta preferible priorizar:

- Facilidad de montaje.
    
- Facilidad de inspección.
    
- Facilidad de reparación.
    
- Facilidad de depuración.
    

aunque ello implique una PCB ligeramente más grande.

---

## Acción para Rev B

Migrar todos los componentes pasivos posibles a:

- 0603 (preferido)
    
- 0402 únicamente cuando sea necesario
    

Evitar encapsulados 0201 en diseños destinados a montaje manual.

---

# Conclusión

Aunque la Rev A no alcanzó el funcionamiento completo previsto inicialmente, la fase de depuración permitió identificar la causa raíz principal y validar una parte importante del hardware.

Bloques validados:

- ESP32-S3
    
- USB
    
- Flash
    
- Programación de firmware
    
- Comunicación serie
    
- LEDs RGB
    

Bloques parcialmente validados:

- DRV5033
    

Bloques pendientes:

- HDC1080
    
- LIS3DH
    
- MicroSD
    

La información obtenida durante la Rev A constituye una base sólida para una futura Rev B más robusta, más fácil de fabricar y considerablemente más sencilla de depurar.