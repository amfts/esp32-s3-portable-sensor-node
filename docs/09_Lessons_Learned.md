# Lecciones aprendidas

## Introducción

El principal objetivo de este proyecto era adquirir experiencia práctica en diseño electrónico completo.

Aunque la Rev A no alcanzó inicialmente el funcionamiento esperado, el proyecto permitió recorrer todas las etapas de un desarrollo hardware real:

- Definición de requisitos.
    
- Diseño esquemático.
    
- Diseño PCB.
    
- Fabricación.
    
- Montaje SMD.
    
- Bring-up.
    
- Depuración.
    
- Análisis de fallos.
    
- Documentación técnica.
    

Además, la depuración posterior permitió recuperar parcialmente la funcionalidad de la placa y validar una parte importante del diseño original.

---

# Lección 1: La alimentación es lo primero

El fallo principal de la Rev A se produjo en el sistema de alimentación.

La arquitectura seleccionada impedía generar correctamente el rail de 3.3V requerido por el sistema.

## Aprendizaje

Antes de diseñar el resto del hardware es imprescindible validar:

- Arquitectura de alimentación.
    
- Tensiones de entrada.
    
- Tensiones de salida.
    
- Condiciones límite.
    
- Comportamiento real de los convertidores.
    

Una alimentación incorrecta puede impedir el funcionamiento de todo el sistema independientemente de que el resto del diseño sea correcto.

---

# Lección 2: No basta con configurar una tensión nominal

Configurar un convertidor para una tensión determinada no garantiza que pueda generarla en todas las condiciones de funcionamiento.

En la Rev A se asumió que el TPS61022 podría generar un rail estable de 3.3V.

La validación práctica demostró que esta hipótesis era incorrecta.

## Aprendizaje

Es necesario comprender:

- Tipo de convertidor.
    
- Rango de funcionamiento.
    
- Modos de operación.
    
- Limitaciones.
    
- Casos límite.
    

La lectura completa del datasheet es tan importante como el cálculo de la tensión de salida.

---

# Lección 3: Los componentes pequeños tienen un coste oculto

La miniaturización tiene consecuencias directas sobre:

- Montaje.
    
- Inspección.
    
- Reparación.
    
- Depuración.
    

Durante el ensamblaje se comprobó que algunos encapsulados extremadamente pequeños complicaban significativamente el trabajo.

## Componentes 0201

Casos identificados:

- C19
    
- C25
    

Problemas encontrados:

- Difíciles de manipular.
    
- Difíciles de soldar.
    
- Difíciles de inspeccionar.
    
- Difíciles de medir.
    
- Muy fáciles de perder.
    

## Componentes 0402

Aunque más manejables que los 0201, siguen siendo incómodos para montaje manual.

## Regla personal

- 0603 por defecto.
    
- 0402 únicamente cuando sea necesario.
    
- Evitar 0201 en montaje manual.
    

---

# Lección 4: Los encapsulados QFN son mucho más difíciles de lo que parecen

El TPS61022 utiliza un encapsulado QFN de 2 mm × 2 mm.

Durante la depuración se comprobó que:

- Las soldaduras quedan ocultas.
    
- No es posible inspeccionar visualmente todos los contactos.
    
- El retrabajo es complejo.
    
- Los fallos son difíciles de diagnosticar.
    

## Aprendizaje

Cuando sea posible:

- Priorizar encapsulados más accesibles.
    
- Diseñar pensando también en la depuración y reparación.
    

---

# Lección 5: Añadir test points desde el principio

Durante la depuración se echó en falta disponer de puntos de medida dedicados.

Muchas medidas tuvieron que realizarse directamente sobre componentes muy pequeños.

## Aprendizaje

Añadir test points para:

- VBUS
    
- VBAT
    
- VREG
    
- 3V3
    
- EN
    
- BOOT
    
- RESET
    
- FB
    
- SDA
    
- SCL
    

supone un coste mínimo y simplifica enormemente la validación.

---

# Lección 6: Revisar los footprints con el mismo cuidado que el esquemático

Durante la Rev A aparecieron varios problemas relacionados con la integración física de componentes.

## HDC1080

Durante la investigación se descubrió que:

- El DAP (Exposed Pad) fue conectado a GND.
    

Posteriormente se encontró documentación oficial de Texas Instruments indicando que:

- El DAP debe soldarse.
    
- El pad de la PCB debe permanecer flotante.
    
- No debe conectarse a masa.
    

## LIS3DH

Durante la validación se descubrió que:

- El pin CS había sido dejado inicialmente sin conexión.
    

Aunque posteriormente se corrigió temporalmente, el error demuestra la importancia de revisar cuidadosamente todos los modos de funcionamiento de un dispositivo.

## Aprendizaje

No basta con verificar conexiones eléctricas.

También es necesario revisar:

- Footprints.
    
- Notas del fabricante.
    
- Pines especiales.
    
- Modos de funcionamiento.
    
- Recomendaciones de diseño.
    

---

# Lección 7: Los pines especiales del microcontrolador importan

Durante la validación del sensor Hall se utilizó GPIO46.

Posteriormente se comprobó que:

- GPIO46 es un pin de strapping del ESP32-S3.
    
- Afecta al proceso de arranque.
    
- No es recomendable para periféricos generales.
    

## Aprendizaje

Antes de asignar un GPIO es necesario revisar:

- Strapping pins.
    
- Pines reservados.
    
- Pines de depuración.
    
- Pines USB.
    
- Pines de memoria.
    

Un GPIO disponible no siempre es un GPIO recomendable.

---

# Lección 8: Diseñar es fácil, depurar es la parte difícil

La mayor parte del tiempo invertido en el proyecto no se dedicó a dibujar el esquemático o el PCB.

Se dedicó a:

- Medir.
    
- Revisar datasheets.
    
- Analizar hipótesis.
    
- Buscar causas raíz.
    
- Validar resultados.
    
- Corregir errores.
    

## Aprendizaje

La depuración forma parte fundamental del desarrollo hardware.

Un diseño no termina cuando se envían los Gerbers a fabricación.

---

# Lección 9: Una PCB aparentemente fallida puede convertirse en una plataforma válida de aprendizaje

Inicialmente parecía que la Rev A estaba completamente perdida.

Sin embargo, tras eliminar el TPS61022 e instalar temporalmente un LM3940-3.3 se consiguió validar:

- ESP32-S3.
    
- USB.
    
- Flash.
    
- Programación de firmware.
    
- Comunicación serie.
    
- LEDs RGB.
    

## Aprendizaje

La validación progresiva por bloques permite aislar problemas y recuperar información muy valiosa incluso cuando el sistema completo no funciona.

Una revisión que falla sigue siendo útil si permite identificar causas raíz y validar hipótesis.

---

# Lección 10: La documentación tiene tanto valor como el hardware

Gran parte del valor obtenido durante este proyecto proviene de la documentación generada.

La documentación permite:

- Registrar medidas.
    
- Conservar hallazgos.
    
- Justificar decisiones.
    
- Evitar repetir errores.
    
- Facilitar futuras revisiones.
    

## Aprendizaje

Documentar de forma continua resulta mucho más útil que intentar reconstruir todo el proceso al final del proyecto.

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

# Conclusión

La Rev A permitió completar por primera vez un ciclo completo de desarrollo hardware real:

- Requisitos.
    
- Esquemático.
    
- PCB.
    
- Fabricación.
    
- Montaje.
    
- Bring-up.
    
- Depuración.
    
- Análisis de fallos.
    
- Propuesta de mejora.
    

Aunque la placa no alcanzó inicialmente el funcionamiento previsto, la investigación posterior permitió identificar la causa raíz principal y recuperar parcialmente el sistema.

La experiencia obtenida durante la Rev A constituye una base sólida para una futura Rev B más robusta, más fácil de fabricar y considerablemente más sencilla de depurar.