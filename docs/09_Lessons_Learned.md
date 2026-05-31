# Lecciones aprendidas

## Introducción

El principal objetivo de este proyecto era adquirir experiencia práctica en diseño electrónico completo.

Aunque la Rev A no alcanzó el funcionamiento esperado, el proyecto permitió recorrer todas las etapas de un desarrollo hardware real.

---

# Lección 1: La alimentación es lo primero

El fallo principal de la Rev A se produjo en el sistema de alimentación.

Aprendizaje:

Antes de diseñar el resto del sistema es imprescindible validar:

- Arquitectura de alimentación.
    
- Tensión de entrada.
    
- Tensión de salida.
    
- Condiciones límite.
    
- Comportamiento real de los convertidores.
    

Una alimentación incorrecta puede invalidar completamente el resto del diseño.

---

# Lección 2: No basta con leer una tensión nominal

Configurar un convertidor para 3.3V no garantiza que siempre entregue 3.3V.

Es necesario comprender:

- Modo de funcionamiento.
    
- Limitaciones.
    
- Condiciones de operación.
    
- Casos límite.
    

---

# Lección 3: Los componentes pequeños tienen un coste oculto

Los encapsulados extremadamente pequeños pueden ahorrar espacio, pero aumentan considerablemente:

- Tiempo de montaje.
    
- Dificultad de inspección.
    
- Dificultad de depuración.
    
- Riesgo de errores.
    

## Regla personal

- 0603 por defecto.
    
- 0402 únicamente cuando sea necesario.
    
- Evitar 0201 en montaje manual.
    

---

# Lección 4: Los QFN son mucho más difíciles de lo que parecen

El TPS61022 utiliza encapsulado QFN de 2 mm x 2 mm.

Problemas observados:

- Soldaduras invisibles.
    
- Difícil inspección.
    
- Difícil retrabajo.
    
- Diagnóstico complicado.
    

Para prototipos manuales es recomendable evaluar encapsulados más accesibles cuando sea posible.

---

# Lección 5: Añadir test points desde el principio

Durante la depuración se echó en falta disponer de puntos de medida dedicados.

En futuras revisiones se añadirán test points para todas las señales críticas.

---

# Lección 6: Diseñar es fácil, depurar es la parte difícil

La mayor parte del tiempo del proyecto no se invirtió en dibujar el esquemático o el PCB.

Se invirtió en:

- Medir.
    
- Revisar datasheets.
    
- Analizar hipótesis.
    
- Identificar fallos.
    
- Buscar causas raíz.
    

La depuración forma parte fundamental del desarrollo hardware.

---

# Lección 7: Una PCB fallida sigue siendo un éxito

La Rev A no alcanzó el funcionamiento previsto.

Sin embargo permitió adquirir experiencia real en:

- Diseño esquemático.
    
- Diseño PCB.
    
- Fabricación.
    
- Montaje SMD.
    
- Validación.
    
- Diagnóstico.
    
- Documentación técnica.
    

El conocimiento adquirido es mucho más valioso que una placa que hubiese funcionado a la primera sin necesidad de análisis.

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

La Rev A permitió completar por primera vez un ciclo completo de desarrollo hardware profesional:

- Requisitos.
    
- Esquemático.
    
- PCB.
    
- Fabricación.
    
- Montaje.
    
- Bring-up.
    
- Depuración.
    
- Análisis de fallos.
    
- Propuesta de mejora.
    

La experiencia obtenida servirá como base para futuras revisiones y proyectos de mayor complejidad.
