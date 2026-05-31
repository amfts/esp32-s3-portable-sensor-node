# Depuración de hardware - Rev A

## Síntoma inicial

La placa no funcionaba correctamente tras el montaje.

## Medidas realizadas

Con alimentación por USB:

- VBAT ≈ 4.2V
- VREG ≈ 4.7V
- Rail 3V3 ≈ 4.3V - 4.7V
- EN ≈ 4.2V
- BOOT ≈ 4.3V

## Interpretación

El rail etiquetado como 3V3 estaba claramente por encima de la tensión máxima esperada para el ESP32-S3 y los sensores.

Esto indicaba que el problema estaba en la etapa de alimentación.

## Revisión del divisor de feedback

Valores esperados:

- R2 ≈ 464kΩ
- R5 ≈ 100kΩ

Estos valores deberían configurar una salida cercana a 3.3V si el convertidor pudiera regular correctamente en esa condición.

## Hallazgo principal

El TPS61022 es un boost converter.

No puede convertir:

- 5V USB → 3.3V
- 4.2V batería Li-Ion → 3.3V

Por tanto, el fallo no era únicamente de soldadura: el regulador seleccionado no era adecuado como regulador principal de 3.3V para esta arquitectura.

## Posible problema adicional

En una de las placas, U1 se calentaba.

Esto podría indicar:

- Mala soldadura.
- Puente oculto bajo el encapsulado QFN.
- Pad inferior mal soldado.
- Componente dañado.

## Conclusión de depuración

El fallo principal es de diseño de alimentación.

Además, puede existir un problema secundario de montaje en U1 debido al encapsulado QFN.