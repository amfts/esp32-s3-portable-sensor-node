# Diseño PCB - Rev A

## Descripción general

La PCB fue diseñada utilizando EasyEDA como parte de un proyecto de aprendizaje orientado al diseño electrónico, integración de sistemas embebidos y fabricación de hardware propio.

El objetivo era integrar en una única placa:

- ESP32-S3-WROOM-1
    
- Alimentación mediante batería Li-Ion
    
- Cargador USB-C
    
- Convertidor DC/DC
    
- Sensor de temperatura y humedad HDC1080
    
- Acelerómetro LIS3DH
    
- Sensor Hall DRV5013
    
- Tarjeta MicroSD
    
- LEDs RGB direccionables
    
- Conectores de expansión GPIO
    

Posteriormente la placa fue enviada a fabricación y ensamblada manualmente.

---

# Distribución general

La PCB se organizó en varios bloques funcionales:

## Bloque de alimentación

Compuesto por:

- Conector USB-C
    
- Cargador TP4056/TP4065
    
- Gestión de batería Li-Ion
    
- Convertidor TPS61022
    
- Protección ESD
    

Este bloque se situó cerca de la entrada USB para minimizar recorridos de corriente elevados.

## Bloque de procesamiento

Compuesto por:

- ESP32-S3-WROOM-1
    

Ubicado en la zona central de la placa.

Se intentó mantener la antena relativamente despejada para minimizar pérdidas de RF.

## Bloque de sensores

Compuesto por:

- HDC1080
    
- LIS3DH
    
- DRV5013
    

Agrupados alrededor del microcontrolador para facilitar el encaminamiento de señales.

## Bloque de almacenamiento

Compuesto por:

- Conector MicroSD
    

Situado en un lateral de la placa para facilitar el acceso físico.

## Bloque visual

Compuesto por:

- LEDs RGB WS2812
    

Destinados a indicar estados del sistema y facilitar tareas de depuración.

---

# Aspectos positivos del diseño

## Integración

La PCB integra múltiples funciones en un espacio relativamente reducido.

Se consiguió incluir:

- Alimentación
    
- Procesamiento
    
- Sensores
    
- Almacenamiento
    
- Expansión
    

en una única placa.

## Modularidad

El diseño está claramente dividido en bloques funcionales.

Esto facilita:

- La comprensión del circuito.
    
- La depuración.
    
- Las futuras revisiones.
    

## Antena del ESP32

La zona de la antena se mantuvo relativamente despejada.

Aunque mejorable, se evitó colocar componentes de gran tamaño directamente delante de la antena.

## Experiencia adquirida

Este proyecto permitió adquirir experiencia práctica en:

- Captura esquemática.
    
- Diseño PCB.
    
- Integración de sensores.
    
- Gestión de alimentación.
    
- USB-C.
    
- Diseño para fabricación.
    
- Montaje SMD.
    
- Depuración de hardware.
    

---

# Aspectos mejorables

## Falta de puntos de test

Durante la depuración se observó que habría sido muy útil disponer de test points dedicados para:

- VBUS
    
- VBAT
    
- VREG
    
- 3V3
    
- EN
    
- BOOT
    
- FB
    

Esto habría simplificado considerablemente las medidas.

## Accesibilidad para depuración

Algunos nodos importantes quedaron poco accesibles una vez montados los componentes.

En futuras revisiones se recomienda añadir:

- Pads de medida.
    
- Puntos de test.
    
- Etiquetado de señales críticas.
    

## Complejidad excesiva para una primera revisión

La primera versión integraba demasiados subsistemas simultáneamente:

- Alimentación
    
- USB
    
- Batería
    
- Sensores
    
- MicroSD
    
- LEDs
    
- RTC
    

Esto dificultó la localización de fallos durante la fase de bring-up.

---

# Selección de encapsulados

## Componentes 0201

Se utilizaron condensadores en encapsulado 0201.

Casos identificados:

- C19
    
- C25
    

### Experiencia práctica

Los componentes 0201 resultaron extremadamente difíciles de soldar manualmente.

Problemas encontrados:

- Tamaño extremadamente pequeño.
    
- Difíciles de manipular con pinzas.
    
- Muy fáciles de perder durante el montaje.
    
- Difíciles de inspeccionar visualmente.
    
- Difíciles de retrabajar.
    
- Difíciles de medir.
    

### Conclusión

No volvería a utilizar encapsulados 0201 en proyectos destinados a montaje manual.

El ahorro de espacio obtenido no compensa el aumento de dificultad en:

- Montaje.
    
- Inspección.
    
- Depuración.
    
- Reparación.
    

---

## Componentes 0402

También se utilizaron varios componentes en encapsulado 0402.

### Experiencia práctica

Aunque son significativamente más manejables que los 0201, siguen siendo incómodos para montaje manual.

Problemas encontrados:

- Requieren buena iluminación.
    
- Requieren pinzas de precisión.
    
- Son fáciles de desplazar durante el soldado.
    
- Complican los retrabajos.
    

### Conclusión

0402 es utilizable para prototipos personales.

Sin embargo, para proyectos destinados a montaje manual es preferible utilizar 0603 siempre que sea posible.

---

## Recomendación para futuras revisiones

### Tamaño preferido

Para proyectos personales:

- 0603 → tamaño por defecto
    

### Tamaño aceptable

- 0402 → únicamente cuando sea necesario
    

### Tamaño a evitar

- 0201 → evitar en montaje manual
    

---

# Encapsulados QFN

## TPS61022

El convertidor TPS61022 utiliza encapsulado QFN de 2 mm x 2 mm.

Características:

- Muy pequeño.
    
- Contactos parcialmente ocultos.
    
- Pad térmico inferior.
    

### Problemas encontrados

Durante el montaje se detectó que:

- Es difícil verificar la calidad de la soldadura.
    
- No es posible inspeccionar todos los contactos visualmente.
    
- El retrabajo es complejo.
    
- Puede existir una falsa sensación de montaje correcto.
    

Durante las pruebas se observó calentamiento anómalo del componente en una de las placas.

### Lección aprendida

Los encapsulados QFN requieren:

- Flux de calidad.
    
- Aire caliente.
    
- Buena técnica de montaje.
    
- Preferiblemente stencil y pasta de soldadura.
    

---

# Consideraciones para Rev B

## Alimentación

Rediseñar completamente la arquitectura de alimentación.

## Test points

Añadir puntos de medida dedicados.

## Fabricabilidad

Priorizar encapsulados fáciles de montar manualmente.

## Depuración

Facilitar el acceso a señales críticas.

## Validación

Realizar pruebas por bloques antes de montar el sistema completo.

---

# Regla personal adquirida

Antes de enviar una PCB a fabricación:

1. Revisar la arquitectura de alimentación.
2. Revisar el esquemático completo.
3. Revisar footprints críticos.
4. Revisar disponibilidad de componentes.
5. Revisar soldabilidad manual.

Una PCB ligeramente más grande pero fácil de montar suele ser preferible a una PCB muy compacta pero difícil de depurar.

En particular, para proyectos de montaje manual:

- Priorizar 0603 frente a 0402.
- Utilizar 0402 únicamente cuando exista una justificación clara.
- Evitar encapsulados 0201.
- Evitar encapsulados QFN extremadamente pequeños cuando existan alternativas más accesibles.
# Conclusión

La Rev A permitió completar por primera vez un ciclo completo de desarrollo hardware:

- Definición de requisitos.
    
- Diseño esquemático.
    
- Diseño PCB.
    
- Fabricación.
    
- Montaje.
    
- Validación.
    
- Depuración.
    

Aunque la placa no llegó a funcionar correctamente, el proyecto proporcionó experiencia práctica muy valiosa y permitió identificar varias mejoras que se incorporarán en futuras revisiones.
