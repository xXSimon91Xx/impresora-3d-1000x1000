# Problema: CR Touch — z_offset obligatorio al arrancar

## Descripción del problema

Al añadir la sección `[bltouch]` a `printer.cfg`, **Klipper no arrancaba**:

```
Option 'z_offset' in section 'bltouch' must be specified
```

## Causa

Klipper exige que el campo `z_offset` esté **siempre presente** en la sección `[bltouch]`, incluso si todavía no lo has calibrado. Si el campo no existe, el servidor Klipper falla al cargar la configuración.

## Solución

Añadir `z_offset: 0` temporalmente para que Klipper arranque:

```ini
[bltouch]
sensor_pin: ^PB7
control_pin: PB6
pin_up_touch_mode_reports_triggered: False
x_offset: 0
y_offset: 0
z_offset: 0        # TEMPORAL — calibrar después con PROBE_CALIBRATE
speed: 5.0
lift_speed: 10.0
samples: 2
sample_retract_dist: 5.0
```

Luego calibrar el offset real con:

```
PROBE_CALIBRATE
```

Klipper baja el CR Touch hasta tocar la cama y te permite ajustar el offset con el comando `TESTZ Z=-0.1` (bajar) o `TESTZ Z=+0.1` (subir). Al terminar, `SAVE_CONFIG` guarda el valor real en el bloque `#*# SAVE_CONFIG` al final del archivo.

## Valor calibrado en nuestra máquina

El `SAVE_CONFIG` guarda automáticamente el offset real. El valor varía según el montaje físico del CR Touch respecto a la boquilla.

## Lección

- Klipper valida toda la configuración al arrancar — un campo faltante impide el inicio
- El `z_offset` en `[bltouch]` es un **placeholder** hasta calibrar — siempre pon `0`
- El valor real queda guardado en `#*# SAVE_CONFIG` — **no editar ese bloque manualmente**
