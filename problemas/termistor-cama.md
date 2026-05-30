# Problema: Termistor de la cama calefactada no instalado

## Descripción

La cama calefactada de 1000×1000mm todavía **no tiene termistor instalado**. Klipper necesita un termistor para controlar la temperatura de la cama, pero sin sensor el sistema no puede leer la temperatura real.

## Solución temporal en printer.cfg

Para que Klipper no bloquee el inicio por un sensor de temperatura inválido, usamos una configuración de parche:

```ini
[heater_bed]
heater_pin: PA1
sensor_type: EPCOS 100K B57560G104F  # termistor genérico como placeholder
sensor_pin: PF3
control: watermark
min_temp: -110   # valor muy bajo para que no falle aunque no haya sensor
max_temp: 130
```

`min_temp: -110` evita que Klipper dé error de "temperatura por debajo del mínimo" cuando el pin de temperatura lee valores fuera de rango (sin termistor conectado).

> **Advertencia:** Esta configuración no es segura para uso real. Sin termistor, Klipper no puede detectar si la cama se sobrecalienta.

## Solución definitiva

Instalar un termistor **NTC 100K** (tipo EPCOS B57560G104F o equivalente) en la cama:

1. Pegar el termistor en la parte inferior de la cama calefactada con cinta de kapton
2. Pasar el cable hasta la electrónica
3. Conectar al pin **PF3** (T0 de la cama en la Octopus Pro)
4. Cambiar `min_temp: -110` a `min_temp: 0` (o el mínimo real)

Después recalibrar el PID de la cama:
```
PID_CALIBRATE HEATER=heater_bed TARGET=60
SAVE_CONFIG
```
