# Problema: Ventilador del hotend nunca se apagaba

## Descripción del problema

El ventilador del heatsink del extrusor LDO Smart Orbiter v3.0 **nunca se apagaba**, independientemente de la temperatura del hotend.

También ocurrió el problema opuesto: después de calibrar, el ventilador **nunca arrancaba** hasta que se corrigió el pin.

## Diagnóstico

### Parte 1: Pin incorrecto

El ventilador se conectó inicialmente al slot **PG12** de la Octopus Pro. Ese pin no está definido como pin de ventilador PWM en la placa — el comando no tenía efecto.

> «Huy aparece esto, recuerda que el cable está conectado al slot PG12» — conversación

Los slots de ventilador de la Octopus Pro son FAN0–FAN7, con pines específicos. **PG12 no es un pin de ventilador válido.**

### Parte 2: Siempre encendido

Al mover el ventilador al pin correcto (**PD12**, que corresponde a FAN2), el ventilador arrancaba pero **nunca se apagaba**, incluso cuando el hotend estaba frío.

El problema: la configuración inicial usaba `[fan]` (ventilador de capa) en lugar de `[heater_fan]` (ventilador de heatsink controlado por temperatura).

```ini
# INCORRECTO — ventilador de capa, no de heatsink
[fan]
pin: PD12

# CORRECTO — ventilador controlado por temperatura del hotend
[heater_fan hotend_fan]
pin: PD12
heater: extruder
heater_temp: 50.0    # se apaga cuando baja de 50°C
fan_speed: 1.0
```

## Solución

1. Mover el cable del ventilador de PG12 al **FAN2 (PD12)**
2. Cambiar la sección de `[fan]` a `[heater_fan hotend_fan]`
3. Configurar `heater_temp: 50.0` — el ventilador se activa cuando el hotend supera 50°C y se apaga cuando baja de esa temperatura

Combinado con `[idle_timeout]`:
```ini
[idle_timeout]
timeout: 600
gcode:
  TURN_OFF_HEATERS
  M84
```
Al apagar el calefactor, el hotend baja de 50°C → el ventilador se apaga automáticamente.

## Resultado

El ventilador del heatsink funciona correctamente: se enciende al calentar y se apaga al enfriarse.

## Lección

- **`[fan]`** = ventilador de capa (controlado por el G-code de impresión)
- **`[heater_fan]`** = ventilador de heatsink (controlado por temperatura del hotend)
- El ventilador del SO3 es de heatsink, no de capa — debe usar `[heater_fan]`
- Verificar siempre que el pin usado sea un pin de ventilador válido en la placa
