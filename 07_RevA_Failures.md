# Análisis de fallos - Rev A

## Resumen ejecutivo

La Rev A permitió identificar varios problemas de diseño y montaje que impidieron el correcto funcionamiento de la placa.

El hallazgo más importante fue un error de arquitectura en el sistema de alimentación, acompañado de posibles problemas de ensamblaje asociados al uso de encapsulados muy pequeños y componentes QFN.

A pesar de no alcanzar el funcionamiento esperado, la revisión proporcionó información valiosa para una futura Rev B.

---

# Fallo principal: arquitectura de alimentación incorrecta

## Descripción

La placa utiliza un TPS61022 como regulador principal para generar el rail de alimentación denominado "3V3".

El TPS61022 es un convertidor boost.

Su función principal es elevar tensión.

## Problema detectado

Las fuentes de alimentación del sistema pueden presentar tensiones superiores a 3.3V:

### Alimentación mediante USB

- USB-C ≈ 5V
    

### Alimentación mediante batería

- Batería Li-Ion cargada ≈ 4.2V
    

En estas condiciones el convertidor seleccionado no resulta adecuado para generar un rail estable de 3.3V.

## Evidencias observadas

Durante las pruebas se registraron valores como:

- VBAT ≈ 4.2V
    
- VREG ≈ 4.7V
    
- Rail 3V3 ≈ 4.3V - 4.7V
    

Estos valores son incompatibles con una alimentación nominal de 3.3V.

## Consecuencias

Posible exposición a sobretensión de:

- ESP32-S3
    
- HDC1080
    
- LIS3DH
    
- DRV5013
    
- Tarjeta MicroSD
    
- LEDs WS2812
    

No puede descartarse daño permanente en algunos componentes.

## Lección aprendida

No basta con verificar que un regulador puede configurarse a una determinada tensión de salida.

También es necesario analizar:

- Comportamiento con distintas tensiones de entrada.
    
- Modos de operación especiales.
    
- Condiciones límite.
    
- Compatibilidad con la arquitectura completa del sistema.
    

---

# Fallo secundario: posible problema de montaje en U1

## Descripción

El TPS61022 utiliza encapsulado QFN de 2 mm x 2 mm.

Se trata de un encapsulado especialmente complejo para soldadura manual.

## Observaciones

Durante las pruebas se detectó:

- Calentamiento anómalo de U1.
    
- Comportamiento inconsistente del regulador.
    
- Medidas difíciles de interpretar en el nodo FB.
    

## Posibles causas

- Mala soldadura.
    
- Contacto deficiente del pad inferior.
    
- Puentes ocultos.
    
- Pin FB mal soldado.
    
- Daño térmico durante el montaje.
    

## Impacto

No puede descartarse que parte de los problemas observados estén relacionados con defectos de ensamblaje además del error de diseño principal.

---

# Fallo de fabricabilidad

## Descripción

Aunque no fue la causa principal del fallo de funcionamiento, la selección de encapsulados excesivamente pequeños complicó significativamente el ensamblaje y la depuración de la placa.

## Casos destacados

### Componentes 0201

- C19
    
- C25
    

Estos condensadores resultaron extremadamente difíciles de soldar manualmente.

### Componentes 0402

También se emplearon diversos componentes en encapsulado 0402.

Aunque son más manejables que los 0201, siguen aumentando considerablemente la dificultad de montaje respecto a encapsulados 0603.

## Impacto

- Incremento del tiempo de montaje.
    
- Mayor riesgo de soldaduras defectuosas.
    
- Mayor dificultad para inspección visual.
    
- Mayor dificultad para retrabajos.
    
- Mayor dificultad para mediciones durante la depuración.
    
- Mayor probabilidad de errores de ensamblaje.
    

## Lección aprendida

El tamaño mínimo técnicamente posible no siempre es la mejor elección.

En proyectos de montaje manual resulta preferible priorizar:

- Facilidad de ensamblaje.
    
- Facilidad de inspección.
    
- Facilidad de reparación.
    

aunque ello implique una PCB ligeramente más grande.

## Acción para Rev B

Migrar todos los componentes pasivos posibles a:

- 0603 (preferido)
    
- 0402 únicamente cuando sea estrictamente necesario
    

Evitar encapsulados 0201 en diseños destinados a montaje manual.

---

# Propuestas para Rev B

## Alimentación

Rediseñar completamente la arquitectura de alimentación.

Opciones a estudiar:

- Buck-boost 3.3V.
    
- Regulador LDO.
    
- Conversión buck dedicada.
    

## Fabricabilidad

- Eliminar encapsulados 0201.
    
- Reducir el número de componentes 0402.
    
- Facilitar retrabajos y depuración.
    

## Depuración

Añadir puntos de test para:

- VBUS
    
- VBAT
    
- VREG
    
- 3V3
    
- EN
    
- BOOT
    
- FB
    

## Validación

Realizar pruebas por bloques antes del montaje completo del sistema.

---

# Conclusión

La Rev A no alcanzó el funcionamiento esperado debido principalmente a una arquitectura de alimentación inadecuada para generar un rail estable de 3.3V.

Adicionalmente, la utilización de encapsulados extremadamente pequeños y componentes QFN complicó el proceso de ensamblaje y depuración.

A pesar del resultado final, el proyecto permitió completar con éxito todas las fases de un desarrollo hardware real:

- Definición de requisitos.
    
- Diseño esquemático.
    
- Diseño PCB.
    
- Fabricación.
    
- Montaje.
    
- Medición.
    
- Diagnóstico.
    
- Identificación de causas raíz.
    
- Propuesta de mejoras para futuras revisiones.
    

La información obtenida durante la Rev A servirá como base para una futura Rev B más robusta y fácil de fabricar.